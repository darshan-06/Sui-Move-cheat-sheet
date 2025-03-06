# Sui Move Complete Cheat Sheet

## Basics

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

## Functions

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

## Events

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

## Constants
```move
const MAX_VALUE: u64 = 1000;
```

## Errors
```move
type Error = u64;
const ERROR_CODE: Error = 1;

public fun check_value(value: u64) {
    if (value > MAX_VALUE) {
        abort ERROR_CODE;
    }
}
```

## Generics
```move
struct Wrapper<T> has store {
    value: T,
}
```

## Capabilities
```move
struct AdminCap has key, store {}
```

## Scripting
```move
script {
    use sui::tx_context;

    fun main(ctx: &mut TxContext) {
        let sender = tx_context::sender(ctx);
    }
}
```

## Utility Functions

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

## Use Cases

### Example 1: Creating and Storing an Object
```move
public entry fun create_and_store(ctx: &mut TxContext) {
    let obj = create_object(ctx);
    store_object(obj, ctx);
}
```

### Example 2: Checking Value Before Update
```move
public entry fun safe_update(obj: &mut MyStruct, value: u64) {
    check_value(value);
    update_field(obj, value);
}
```

### Example 3: Emitting an Event After Transfer
```move
public entry fun transfer_with_event(obj: MyStruct, recipient: address, ctx: &mut TxContext) {
    transfer_object(obj, recipient, ctx);
    let event = MyEvent { message: b"Object Transferred".to_vec() };
    event::emit(event);
}
```

### Example 4: Conditional Object Deletion
```move
public entry fun conditional_delete(obj: MyStruct, condition: bool) {
    if condition {
        destroy_object(obj);
    }
}
```

### Example 5: Validating Ownership
```move
public fun validate_ownership(obj: &MyStruct, ctx: &TxContext): bool {
    obj.field2 == tx_context::sender(ctx)
}
```
