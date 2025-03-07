In the Sui Move programming language, the `String` type is used to represent a string of characters, similar to strings in other programming languages. The Move standard library provides several useful methods for manipulating `String` objects.

Below are the common methods available for the `String` type in Sui Move, along with explanations and examples.

### 1. **`String::empty()`**
   - **Description:** Returns an empty string (`""`).
   - **Example:**
     ```move
     public fun create_empty_string(): string {
         String::empty()
     }
     ```
     This function creates an empty string.

### 2. **`String::length(s: &string)`**
   - **Description:** Returns the length of the string `s` in terms of the number of characters.
   - **Example:**
     ```move
     public fun get_string_length(s: &string): u64 {
         String::length(s)
     }
     ```
     This function returns the length of the string `s` in terms of the number of characters.

### 3. **`String::push_back(s: &mut string, c: char)`**
   - **Description:** Appends the character `c` to the end of the string `s`.
   - **Example:**
     ```move
     public fun append_char_to_string(s: &mut string, c: char) {
         String::push_back(s, c)
     }
     ```
     This function appends the character `c` to the string `s`.

### 4. **`String::pop_back(s: &mut string)`**
   - **Description:** Removes the last character from the string `s`.
   - **Example:**
     ```move
     public fun remove_last_char_from_string(s: &mut string) {
         String::pop_back(s)
     }
     ```
     This function removes the last character from the string `s`.

### 5. **`String::get(s: &string, index: u64)`**
   - **Description:** Returns the character at the specified index in the string `s`.
   - **Example:**
     ```move
     public fun get_char_at_index(s: &string, index: u64): char {
         String::get(s, index)
     }
     ```
     This function returns the character at the specified `index` in the string `s`. If the index is out of bounds, it will panic.

### 6. **`String::substr(s: &string, start: u64, end: u64)`**
   - **Description:** Extracts a substring from the string `s` starting from `start` index to `end` index.
   - **Example:**
     ```move
     public fun get_substring(s: &string, start: u64, end: u64): string {
         String::substr(s, start, end)
     }
     ```
     This function extracts the substring from the `start` index to the `end` index in the string `s`.

### 7. **`String::contains(s: &string, substring: &string)`**
   - **Description:** Checks if the string `s` contains the specified `substring`.
   - **Example:**
     ```move
     public fun contains_substring(s: &string, substring: &string): bool {
         String::contains(s, substring)
     }
     ```
     This function checks if the string `s` contains the `substring`. It returns `true` if the substring is found, otherwise `false`.

### 8. **`String::starts_with(s: &string, prefix: &string)`**
   - **Description:** Checks if the string `s` starts with the specified `prefix`.
   - **Example:**
     ```move
     public fun starts_with_prefix(s: &string, prefix: &string): bool {
         String::starts_with(s, prefix)
     }
     ```
     This function checks if the string `s` starts with the given `prefix`. It returns `true` if it does, otherwise `false`.

### 9. **`String::ends_with(s: &string, suffix: &string)`**
   - **Description:** Checks if the string `s` ends with the specified `suffix`.
   - **Example:**
     ```move
     public fun ends_with_suffix(s: &string, suffix: &string): bool {
         String::ends_with(s, suffix)
     }
     ```
     This function checks if the string `s` ends with the given `suffix`. It returns `true` if it does, otherwise `false`.

### 10. **`String::replace(s: &mut string, old_sub: &string, new_sub: &string)`**
   - **Description:** Replaces all occurrences of `old_sub` with `new_sub` in the string `s`.
   - **Example:**
     ```move
     public fun replace_substring(s: &mut string, old_sub: &string, new_sub: &string) {
         String::replace(s, old_sub, new_sub)
     }
     ```
     This function replaces all occurrences of `old_sub` with `new_sub` in the string `s`.

### 11. **`String::split(s: &string, delimiter: char): vector<string>`**
   - **Description:** Splits the string `s` into substrings using the specified `delimiter`.
   - **Example:**
     ```move
     public fun split_string(s: &string, delimiter: char): vector<string> {
         String::split(s, delimiter)
     }
     ```
     This function splits the string `s` into a vector of substrings based on the given `delimiter`.

### 12. **`String::to_upper_case(s: &string)`**
   - **Description:** Converts all characters in the string `s` to uppercase.
   - **Example:**
     ```move
     public fun to_upper_case(s: &string): string {
         String::to_upper_case(s)
     }
     ```
     This function converts all characters in the string `s` to uppercase.

### 13. **`String::to_lower_case(s: &string)`**
   - **Description:** Converts all characters in the string `s` to lowercase.
   - **Example:**
     ```move
     public fun to_lower_case(s: &string): string {
         String::to_lower_case(s)
     }
     ```
     This function converts all characters in the string `s` to lowercase.

### 14. **`String::trim(s: &string)`**
   - **Description:** Removes leading and trailing whitespace characters from the string `s`.
   - **Example:**
     ```move
     public fun trim_string(s: &string): string {
         String::trim(s)
     }
     ```
     This function removes leading and trailing whitespace characters from the string `s`.

### 15. **`String::compare(s1: &string, s2: &string): bool`**
   - **Description:** Compares two strings `s1` and `s2` for equality.
   - **Example:**
     ```move
     public fun compare_strings(s1: &string, s2: &string): bool {
         String::compare(s1, s2)
     }
     ```
     This function checks whether the two strings `s1` and `s2` are equal, returning `true` if they are and `false` if they are not.

### Important Notes:
- **String Mutability:** Many string functions in Move are designed to modify the string in place, requiring mutable references (`&mut string`) for operations like `push_back`, `pop_back`, and `replace`.
- **Memory Considerations:** Strings are typically immutable in Move, so be mindful when performing operations that involve copying strings, especially in smart contract execution, to avoid unnecessary gas costs.
- **Efficiency:** String operations can be costly, particularly for large strings or many operations. You should optimize them for performance and gas usage in smart contracts.
---
