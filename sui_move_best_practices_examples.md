# Sui Move Best Practices Examples

This file provides a series of examples that illustrate best practices in Sui Move development. Each example focuses on a key security or design aspect—from resource safety and invariant checking to access control and error handling.

---

## Example 1: Resource Safety and Invariant Checks

This example demonstrates how to create a resource safely by enforcing invariants using assertions. The invariant in this case is that the resource value must always be greater than zero.

```move
module 0x1::ResourceExample {
    // A simple resource that must always hold a positive value.
    struct MyResource has key {
        value: u64,
    }

    /// Creates a new MyResource ensuring that the provided value is greater than 0.
    public fun create(account: &signer, value: u64): MyResource {
        // Invariant check: Ensure value is positive.
        assert!(value > 0, 100); // 100: Error code for invalid value.
        MyResource { value }
    }

    /// Updates the resource's value with a new value, ensuring the invariant remains intact.
    public fun update(account: &signer, resource: &mut MyResource, new_value: u64) {
        // Invariant check: New value must also be positive.
        assert!(new_value > 0, 101); // 101: Error code for invalid update.
        resource.value = new_value;
    }
}
```

*Key Points:*
- **Invariant Enforcement:** Use `assert!` to check conditions.
- **Resource Safety:** Resources can only be moved or modified in controlled ways.

---

## Example 2: Access Control Using Capabilities

Access control is critical. The following example illustrates a capability-based approach where only a user with a valid admin capability can perform secure operations.

```move
module 0x1::CapabilityExample {
    // The capability granting admin privileges.
    struct AdminCapability has key {
        admin: address,
    }

    /// Grants admin capability to the caller.
    public fun grant_admin(account: &signer): AdminCapability {
        AdminCapability { admin: signer::address_of(account) }
    }

    /// Performs a secure operation that requires the caller to possess the admin capability.
    public fun secure_operation(account: &signer, cap: AdminCapability) {
        // Verify that the admin capability belongs to the caller.
        assert!(cap.admin == signer::address_of(account), 200); // 200: Unauthorized access.
        // Execute sensitive operations here.
    }
}
```

*Key Points:*
- **Capability Pattern:** Use a dedicated struct to encapsulate privileges.
- **Explicit Authorization:** Validate the capability against the caller’s address.

---

## Example 3: Error Handling and State Management

Proper error handling ensures that the module’s state is not corrupted when unexpected inputs are encountered.

```move
module 0x1::ErrorHandlingExample {
    // A resource containing a vector of bytes.
    struct DataResource has key {
        data: vector<u8>,
    }

    /// Updates the data in the resource with a non-empty vector.
    public fun set_data(account: &signer, resource: &mut DataResource, new_data: vector<u8>) {
        // Check that the provided data vector is not empty.
        assert!(vector::length(&new_data) > 0, 300); // 300: Error code for empty data.
        resource.data = new_data;
    }
}
```

*Key Points:*
- **Error Codes:** Use distinct error codes for easier debugging.
- **Graceful Failure:** Ensure that operations abort before altering the state when conditions aren’t met.

---

## Example 4: Guidelines for Unit Testing

While Move tests are typically written using the Move CLI or integrated testing frameworks, the following guidelines can help structure your tests:

- **Test Each Function:** Write tests to cover both successful execution and expected failures (i.e., assertion failures).
- **Edge Cases:** Ensure that tests include boundary conditions for invariants.
- **Simulate Real Scenarios:** Mimic concurrent operations where possible to ensure that the module behaves correctly under various conditions.

*Testing Example (Pseudo-code):*

```move
// Example pseudo-code for a test scenario using the Move testing framework.
script {
    use 0x1::ResourceExample;

    fun main(account: &signer) {
        // Test creating a resource with a valid value.
        let res = ResourceExample::create(account, 10);
        // Update the resource with a valid new value.
        ResourceExample::update(account, &mut res, 20);

        // Expect failure when creating a resource with an invalid value.
        // The test framework should capture the abort code 100.
        // ResourceExample::create(account, 0);
    }
}
```

*Key Points:*
- **Comprehensive Testing:** Always cover both valid and invalid execution paths.
- **Automated Testing:** Integrate tests into your CI/CD pipeline to catch regressions early.

---

## Conclusion

These examples highlight core best practices in Sui Move development. By enforcing invariants, applying proper access controls, and handling errors gracefully, you can develop secure and reliable smart contracts on the Sui blockchain. Regular testing and reviews further ensure that your modules remain robust against potential vulnerabilities.

*Note: Always keep abreast of the latest Sui and Move documentation, as best practices evolve with the ecosystem.*
