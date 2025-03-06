# Sui Move Complete Cheat Sheet

This document provides a complete cheat sheet for Sui Move, including modules, functions, events, and use case examples. Each section starts with a brief description of its content.

---

## Basics
*Short Description:* Introduces the foundational concepts of Sui Move, including module declarations and struct definitions.

### Declaring a Module
```move
module example::MyModule {
    use sui::object;
    use sui::tx_context;
    use sui::event;
}
```

### Struct Definition
```move
struct MyStruct has key, store {
    field1: u64,
    field2: address,
}
```

---

## Functions
*Short Description:* Covers common functions in Sui Move, from entry functions to object management methods such as creating, transferring, and storing objects.

### Entry Functions
```move
public entry fun my_entry_function(value: u64, ctx: &mut TxContext) {
    // Function logic
}
```

### Creating an Object
```move
public entry fun create_object(ctx: &mut TxContext): MyStruct {
    let obj = MyStruct { field1: 100, field2: tx_context::sender(ctx) };
    object::new(obj)
}
```

### Transferring an Object
```move
public entry fun transfer_object(obj: MyStruct, recipient: address, ctx: &mut TxContext) {
    object::transfer(obj, recipient);
}
```

### Storing an Object
```move
public entry fun store_object(obj: MyStruct, ctx: &mut TxContext) {
    object::store(obj);
}
```

### Borrowing an Object
```move
public fun borrow_object(obj: &MyStruct): u64 {
    obj.field1
}
```

---

## Events
*Short Description:* Explains how to define and emit events in Sui Move for tracking state changes and actions.

### Defining an Event
```move
struct MyEvent has drop {
    message: vector<u8>,
}
```

### Emitting an Event
```move
public entry fun emit_event(ctx: &mut TxContext) {
    let event = MyEvent { message: b"Hello, Sui!".to_vec() };
    event::emit(event);
}
```

---

## Constants
*Short Description:* Shows how to define constant values that can be used throughout your Sui Move modules.

```move
const MAX_VALUE: u64 = 1000;
```

---

## Errors
*Short Description:* Provides an example of error handling in Sui Move, including custom error codes and value checking.

```move
type Error = u64;
const ERROR_CODE: Error = 1;

public fun check_value(value: u64) {
    if (value > MAX_VALUE) {
        abort ERROR_CODE;
    }
}
```

---

## Generics
*Short Description:* Introduces the use of generics in Sui Move, enabling the creation of flexible and reusable data structures.

```move
struct Wrapper<T> has store {
    value: T,
}
```

---

## Capabilities
*Short Description:* Demonstrates how to define capabilities, which are used to control access and manage privileges.

```move
struct AdminCap has key, store {}
```

---

## Scripting
*Short Description:* Covers how to write simple scripts in Sui Move for operations that do not require a full module.

```move
script {
    use sui::tx_context;

    fun main(ctx: &mut TxContext) {
        let sender = tx_context::sender(ctx);
    }
}
```

---

## Utility Functions
*Short Description:* Provides additional utility functions for common tasks like validating sender addresses, updating struct fields, and deleting objects.

### Checking Sender Address
```move
public fun is_sender(ctx: &TxContext, addr: address): bool {
    tx_context::sender(ctx) == addr
}
```

### Updating a Struct Field
```move
public entry fun update_field(obj: &mut MyStruct, new_value: u64) {
    obj.field1 = new_value;
}
```

### Destroying an Object
```move
public entry fun destroy_object(obj: MyStruct) {
    object::delete(obj);
}
```

---

## Use Cases
*Short Description:* Illustrates practical examples that combine multiple functions to perform common operations in Sui Move.

### Example 1: Creating and Storing an Object
*Short Description:* Demonstrates how to create a new object instance using `create_object` and then store it on-chain with `store_object`.
```move
public entry fun create_and_store(ctx: &mut TxContext) {
    let obj = create_object(ctx);
    store_object(obj, ctx);
}
```

### Example 2: Checking Value Before Update
*Short Description:* Validates that a new value is within an acceptable range (using `check_value`) before safely updating an object's field with `update_field`.
```move
public entry fun safe_update(obj: &mut MyStruct, value: u64) {
    check_value(value);
    update_field(obj, value);
}
```

### Example 3: Emitting an Event After Transfer
*Short Description:* Shows how to transfer an object using `transfer_object` and then emit an event to log the transfer activity.
```move
public entry fun transfer_with_event(obj: MyStruct, recipient: address, ctx: &mut TxContext) {
    transfer_object(obj, recipient, ctx);
    let event = MyEvent { message: b"Object Transferred".to_vec() };
    event::emit(event);
}
```

### Example 4: Conditional Object Deletion
*Short Description:* Illustrates how to conditionally delete an object by invoking `destroy_object` when a specific condition is met.
```move
public entry fun conditional_delete(obj: MyStruct, condition: bool) {
    if condition {
        destroy_object(obj);
    }
}
```

### Example 5: Validating Ownership
*Short Description:* Checks if the transaction sender is the owner of an object by comparing the sender's address with the object's owner field, ensuring proper authorization.
```move
public fun validate_ownership(obj: &MyStruct, ctx: &TxContext): bool {
    obj.field2 == tx_context::sender(ctx)
}
```
