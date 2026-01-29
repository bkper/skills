# Bkper Script Development

Knowledge for writing local scripts and automation using `bkper-js` and the Bkper CLI.

## CLI Setup

### Installation

```bash
# Install globally
bun add -g bkper

# Or use npx
npx bkper
```

### Authentication

```bash
# Login (opens browser for OAuth)
bkper login

# Check current user
bkper whoami

# Logout
bkper logout
```

### Useful Commands

```bash
# List your books
bkper books

# Get book details
bkper books get <bookId>

# List transactions
bkper transactions list <bookId> --query "after:2024-01-01"

# App commands
bkper apps create    # Create app from bkper.yaml
bkper apps update    # Update app from bkper.yaml
bkper apps deploy    # Deploy to Workers for Platforms
bkper apps status    # Check deployment status
```

## Script Setup

### Project Structure

```
my-script/
├── package.json
├── tsconfig.json
├── .env              # BKPER_API_KEY (optional)
└── src/
    └── index.ts
```

### package.json

```json
{
  "name": "my-bkper-script",
  "type": "module",
  "scripts": {
    "start": "bun run src/index.ts"
  },
  "dependencies": {
    "bkper-js": "latest"
  },
  "devDependencies": {
    "@types/bun": "latest",
    "typescript": "^5"
  }
}
```

### tsconfig.json

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true
  },
  "include": ["src/**/*"]
}
```

## Script Authentication

### Option 1: OAuth Token (for user context)

```typescript
import { Bkper } from 'bkper-js';

// Get token from CLI session
const token = process.env.BKPER_OAUTH_TOKEN;

const bkper = new Bkper({
  oauthTokenProvider: async () => token,
});
```

### Option 2: API Key (for dedicated quota)

```typescript
import { Bkper } from 'bkper-js';

const bkper = new Bkper({
  apiKeyProvider: async () => process.env.BKPER_API_KEY,
  oauthTokenProvider: async () => process.env.BKPER_OAUTH_TOKEN,
});
```

## Common Script Patterns

### List and Process Transactions

```typescript
import { Bkper } from 'bkper-js';

const bkper = new Bkper({
  oauthTokenProvider: async () => process.env.BKPER_OAUTH_TOKEN!,
});

async function processTransactions(bookId: string, query: string) {
  const book = await bkper.getBook(bookId, true);
  
  let txList = await book.listTransactions(query);
  let count = 0;

  while (true) {
    for (const tx of txList.getItems()) {
      console.log(`${tx.getDateFormatted()} | ${tx.getAmount()} | ${tx.getDescription()}`);
      count++;
    }

    if (!txList.hasNext()) break;
    txList = await txList.next();
  }

  console.log(`Total: ${count} transactions`);
}

await processTransactions('your-book-id', 'after:2024-01-01');
```

### Bulk Create Transactions

```typescript
import { Bkper, Transaction } from 'bkper-js';

async function bulkImport(bookId: string, data: ImportRow[]) {
  const book = await bkper.getBook(bookId, true);
  
  const transactions: Transaction[] = [];

  for (const row of data) {
    const tx = new Transaction(book)
      .setDate(row.date)
      .setAmount(row.amount)
      .from(await book.getAccount(row.from))
      .to(await book.getAccount(row.to))
      .setDescription(row.description);
    
    transactions.push(tx);
  }

  // Batch create (more efficient than individual creates)
  await book.batchCreateTransactions(transactions);
  
  // Batch post
  await book.batchPostTransactions(transactions);

  console.log(`Created ${transactions.length} transactions`);
}
```

### Update Account Properties

```typescript
async function tagAccounts(bookId: string, groupName: string, property: string, value: string) {
  const book = await bkper.getBook(bookId, true);
  const group = await book.getGroup(groupName);
  
  if (!group) {
    console.error(`Group not found: ${groupName}`);
    return;
  }

  const accounts = await group.getAccounts();
  
  for (const account of accounts) {
    account.setProperty(property, value);
    await account.update();
    console.log(`Updated: ${account.getName()}`);
  }
}
```

### Export to CSV

```typescript
import { Bkper } from 'bkper-js';

async function exportToCsv(bookId: string, query: string, outputFile: string) {
  const book = await bkper.getBook(bookId, true);
  
  const rows: string[] = ['Date,Amount,Description,From,To'];
  
  let txList = await book.listTransactions(query);
  
  while (true) {
    for (const tx of txList.getItems()) {
      const creditAccount = await tx.getCreditAccount();
      const debitAccount = await tx.getDebitAccount();
      
      rows.push([
        tx.getDateFormatted(),
        tx.getAmount().toString(),
        `"${tx.getDescription()?.replace(/"/g, '""') ?? ''}"`,
        creditAccount?.getName() ?? '',
        debitAccount?.getName() ?? '',
      ].join(','));
    }

    if (!txList.hasNext()) break;
    txList = await txList.next();
  }

  await Bun.write(outputFile, rows.join('\n'));
  console.log(`Exported ${rows.length - 1} transactions to ${outputFile}`);
}
```

### Reconciliation Script

```typescript
async function reconcileTransactions(bookId: string, query: string) {
  const book = await bkper.getBook(bookId);
  
  let txList = await book.listTransactions(query);
  const toCheck: Transaction[] = [];

  while (true) {
    for (const tx of txList.getItems()) {
      if (!tx.isChecked() && tx.isPosted()) {
        toCheck.push(tx);
      }
    }

    if (!txList.hasNext()) break;
    txList = await txList.next();
  }

  if (toCheck.length === 0) {
    console.log('No transactions to reconcile');
    return;
  }

  console.log(`Checking ${toCheck.length} transactions...`);
  await book.batchCheckTransactions(toCheck);
  console.log('Done!');
}
```

## Error Handling

```typescript
try {
  const book = await bkper.getBook('invalid-id');
} catch (error: any) {
  if (error.status === 404) {
    console.error('Book not found');
  } else if (error.status === 403) {
    console.error('Access denied - check authentication');
  } else {
    console.error('Error:', error.message);
  }
}
```

## Running Scripts

```bash
# With bun
bun run src/index.ts

# With environment variables
BKPER_OAUTH_TOKEN="your-token" bun run src/index.ts

# Using .env file
bun run src/index.ts  # Bun loads .env automatically
```

## Tips

1. **Use batch operations** - `batchCreateTransactions`, `batchPostTransactions`, etc. are much faster than individual operations

2. **Pre-load accounts** - Use `getBook(id, true)` to load accounts upfront if you'll reference them

3. **Paginate large queries** - Use `hasNext()` and `next()` for queries that may return many results

4. **Handle rate limits** - Add delays between large batch operations if needed

5. **Test with a copy** - Create a book copy for testing destructive operations
