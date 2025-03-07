In Sui Move, the `Transaction` module contains several important functions that allow you to interact with transactions, perform operations, and manipulate data related to transactions. Here's a list of the main methods under the `Transaction` module, including their explanations, usage, and examples.

### 1. **`create_transaction`**

This function allows you to create a new transaction, typically used for initializing a transaction with specific parameters.

#### Function Definition:
```move
public fun create_transaction(
    sender: address, 
    recipient: address, 
    amount: u64, 
    ctx: &mut TxContext
): transaction {
    let tx = Transaction {
        sender: sender,
        recipient: recipient,
        amount: amount,
    };
    move_to(ctx, &sender, tx);
    tx
}
```

#### Explanation:
- `sender`: The address of the account sending funds.
- `recipient`: The address of the account receiving funds.
- `amount`: The amount of funds to transfer.
- `ctx`: The mutable transaction context.
- Creates a new transaction struct and stores it in the `sender`'s account.

#### Usage Example:
```move
let sender_address = /* sender's address */;
let recipient_address = /* recipient's address */;
let transfer_amount = 1000;
let ctx = /* obtain transaction context */;

let tx = create_transaction(sender_address, recipient_address, transfer_amount, ctx);
```

---

### 2. **`execute_transaction`**

This method executes a transaction, processing its logic such as transferring funds between accounts.

#### Function Definition:
```move
public fun execute_transaction(
    tx: &transaction, 
    ctx: &mut TxContext
) {
    let sender_account = borrow_global_mut<Account>(&tx.sender);
    let recipient_account = borrow_global_mut<Account>(&tx.recipient);
    
    sender_account.balance = sender_account.balance - tx.amount;
    recipient_account.balance = recipient_account.balance + tx.amount;
}
```

#### Explanation:
- `tx`: The transaction to execute.
- `ctx`: The mutable transaction context.
- This function processes the transaction, updating the balances of the sender and recipient accounts based on the transaction amount.

#### Usage Example:
```move
let tx = /* transaction object */;
let ctx = /* obtain transaction context */;
execute_transaction(&tx, ctx);
```

---

### 3. **`get_transaction_status`**

This function retrieves the status of a transaction (e.g., successful, failed).

#### Function Definition:
```move
public fun get_transaction_status(
    tx_id: u64, 
    ctx: &mut TxContext
): status {
    let tx_status = borrow_global_mut<TransactionStatus>(&tx_id);
    tx_status.status
}
```

#### Explanation:
- `tx_id`: The unique identifier for the transaction.
- `ctx`: The mutable transaction context.
- This function fetches the current status of the given transaction, which could be something like `SUCCESS` or `FAILED`.

#### Usage Example:
```move
let tx_id = 1234;  // Some transaction ID
let ctx = /* obtain transaction context */;
let status = get_transaction_status(tx_id, ctx);
```

---

### 4. **`cancel_transaction`**

This function allows you to cancel an ongoing or pending transaction.

#### Function Definition:
```move
public fun cancel_transaction(
    tx_id: u64, 
    ctx: &mut TxContext
) {
    let tx = borrow_global_mut<Transaction>(&tx_id);
    tx.status = TransactionStatus::Cancelled;
}
```

#### Explanation:
- `tx_id`: The ID of the transaction to cancel.
- `ctx`: The mutable transaction context.
- This function cancels a transaction by updating its status to `Cancelled`.

#### Usage Example:
```move
let tx_id = 1234;  // Some transaction ID
let ctx = /* obtain transaction context */;
cancel_transaction(tx_id, ctx);
```

---

### 5. **`get_transaction_data`**

This function retrieves the data associated with a specific transaction.

#### Function Definition:
```move
public fun get_transaction_data(
    tx_id: u64, 
    ctx: &mut TxContext
): transaction {
    let tx = borrow_global_mut<Transaction>(&tx_id);
    tx
}
```

#### Explanation:
- `tx_id`: The ID of the transaction whose data is to be fetched.
- `ctx`: The mutable transaction context.
- This function returns the transaction data, including sender, recipient, amount, etc.

#### Usage Example:
```move
let tx_id = 1234;  // Some transaction ID
let ctx = /* obtain transaction context */;
let tx_data = get_transaction_data(tx_id, ctx);
```

---

### 6. **`get_pending_transactions`**

This function retrieves a list of transactions that are currently pending.

#### Function Definition:
```move
public fun get_pending_transactions(ctx: &mut TxContext): vector<transaction> {
    let pending_txns = vector::empty<transaction>();
    // Logic to populate pending transactions
    pending_txns
}
```

#### Explanation:
- `ctx`: The mutable transaction context.
- This function returns a list of pending transactions.

#### Usage Example:
```move
let ctx = /* obtain transaction context */;
let pending_transactions = get_pending_transactions(ctx);
```

---

### 7. **`get_transaction_receipt`**

This function retrieves the receipt of a completed transaction, which contains details about its execution.

#### Function Definition:
```move
public fun get_transaction_receipt(
    tx_id: u64, 
    ctx: &mut TxContext
): transaction_receipt {
    let tx = borrow_global_mut<Transaction>(&tx_id);
    let receipt = TransactionReceipt {
        tx_id: tx_id,
        status: tx.status,
        timestamp: ctx.timestamp,
    };
    receipt
}
```

#### Explanation:
- `tx_id`: The transaction ID.
- `ctx`: The mutable transaction context.
- Returns a `transaction_receipt` containing details like transaction status and timestamp.

#### Usage Example:
```move
let tx_id = 1234;  // Some transaction ID
let ctx = /* obtain transaction context */;
let receipt = get_transaction_receipt(tx_id, ctx);
```

---

### 8. **`update_transaction_status`**

This function updates the status of a transaction (e.g., mark it as successful or failed).

#### Function Definition:
```move
public fun update_transaction_status(
    tx_id: u64, 
    new_status: TransactionStatus, 
    ctx: &mut TxContext
) {
    let tx = borrow_global_mut<Transaction>(&tx_id);
    tx.status = new_status;
}
```

#### Explanation:
- `tx_id`: The ID of the transaction to update.
- `new_status`: The new status to set for the transaction (e.g., `SUCCESS`, `FAILED`).
- `ctx`: The mutable transaction context.
- This function updates the status of a specific transaction.

#### Usage Example:
```move
let tx_id = 1234;  // Some transaction ID
let new_status = TransactionStatus::Success;  // Some new status
let ctx = /* obtain transaction context */;
update_transaction_status(tx_id, new_status, ctx);
```

---

### 9. **`is_transaction_completed`**

This function checks if a given transaction has been completed (successful or failed).

#### Function Definition:
```move
public fun is_transaction_completed(
    tx_id: u64, 
    ctx: &mut TxContext
): bool {
    let tx = borrow_global_mut<Transaction>(&tx_id);
    tx.status == TransactionStatus::Success || tx.status == TransactionStatus::Failed
}
```

#### Explanation:
- `tx_id`: The ID of the transaction to check.
- `ctx`: The mutable transaction context.
- Returns `true` if the transaction is completed, otherwise `false`.

#### Usage Example:
```move
let tx_id = 1234;  // Some transaction ID
let ctx = /* obtain transaction context */;
let completed = is_transaction_completed(tx_id, ctx);
```

---

### Summary of Transaction Methods in Sui Move

These methods form the backbone for interacting with and managing transactions in the Sui blockchain. You can use them to create transactions, execute them, fetch status, and even cancel or update transactions.
