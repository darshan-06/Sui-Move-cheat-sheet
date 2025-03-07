In the context of the Sui Move programming language, `Vector` is a type that represents an ordered, homogeneous collection of elements, similar to an array or a list in other programming languages. Sui Move's standard library provides various methods that you can use to manipulate `Vector` objects.

Here are some of the commonly used methods for `Vector` in Sui Move:

### 1. **`Vector::empty<T>()`**
   - **Description:** Creates an empty vector of type `T`.
   - **Example:**
     ```move
     public fun create_empty_vector<T>(): vector<T> {
         Vector::empty<T>()
     }
     ```
     This function creates an empty vector that can later be populated with elements of type `T`.

### 2. **`Vector::length<T>(v: &vector<T>)`**
   - **Description:** Returns the length of a vector.
   - **Example:**
     ```move
     public fun get_vector_length<T>(v: &vector<T>): u64 {
         Vector::length(v)
     }
     ```
     This function returns the number of elements in the vector `v`.

### 3. **`Vector::push_back<T>(v: &mut vector<T>, elem: T)`**
   - **Description:** Adds an element to the end of the vector.
   - **Example:**
     ```move
     public fun add_element_to_vector<T>(v: &mut vector<T>, elem: T) {
         Vector::push_back(v, elem)
     }
     ```
     This function appends `elem` to the end of the vector `v`.

### 4. **`Vector::pop_back<T>(v: &mut vector<T>)`**
   - **Description:** Removes the last element from the vector and returns it.
   - **Example:**
     ```move
     public fun remove_last_element<T>(v: &mut vector<T>): Option<T> {
         Vector::pop_back(v)
     }
     ```
     This function removes the last element from the vector `v` and returns it as an `Option`. If the vector is empty, it returns `None`.

### 5. **`Vector::get<T>(v: &vector<T>, index: u64)`**
   - **Description:** Returns the element at the specified index in the vector.
   - **Example:**
     ```move
     public fun get_element_at_index<T>(v: &vector<T>, index: u64): Option<T> {
         Vector::get(v, index)
     }
     ```
     This function returns the element at the given index in the vector `v`, wrapped in an `Option`. If the index is out of bounds, it returns `None`.

### 6. **`Vector::swap_remove<T>(v: &mut vector<T>, index: u64)`**
   - **Description:** Removes and returns the element at the specified index, replacing it with the last element.
   - **Example:**
     ```move
     public fun swap_remove_element<T>(v: &mut vector<T>, index: u64): Option<T> {
         Vector::swap_remove(v, index)
     }
     ```
     This function removes the element at `index` in the vector `v` and replaces it with the last element. It returns the removed element wrapped in an `Option`.

### 7. **`Vector::clear<T>(v: &mut vector<T>)`**
   - **Description:** Removes all elements from the vector.
   - **Example:**
     ```move
     public fun clear_vector<T>(v: &mut vector<T>) {
         Vector::clear(v)
     }
     ```
     This function removes all elements from the vector `v`, leaving it empty.

### 8. **`Vector::reverse<T>(v: &mut vector<T>)`**
   - **Description:** Reverses the order of the elements in the vector.
   - **Example:**
     ```move
     public fun reverse_vector<T>(v: &mut vector<T>) {
         Vector::reverse(v)
     }
     ```
     This function reverses the order of elements in the vector `v`.

### 9. **`Vector::extend<T>(v: &mut vector<T>, other: vector<T>)`**
   - **Description:** Adds all elements of another vector to the end of the current vector.
   - **Example:**
     ```move
     public fun extend_vector<T>(v: &mut vector<T>, other: vector<T>) {
         Vector::extend(v, other)
     }
     ```
     This function appends all elements from `other` to the vector `v`.

### 10. **`Vector::contains<T>(v: &vector<T>, elem: T): bool`**
   - **Description:** Checks if the vector contains the given element.
   - **Example:**
     ```move
     public fun contains_element<T>(v: &vector<T>, elem: T): bool {
         Vector::contains(v, elem)
     }
     ```
     This function checks if the element `elem` is present in the vector `v`. It returns `true` if found, otherwise `false`.

### 11. **`Vector::copy<T>(v: &vector<T>)`**
   - **Description:** Creates a copy of the given vector.
   - **Example:**
     ```move
     public fun copy_vector<T>(v: &vector<T>): vector<T> {
         Vector::copy(v)
     }
     ```
     This function creates a new vector that is a copy of `v`.

### 12. **`Vector::remove<T>(v: &mut vector<T>, index: u64)`**
   - **Description:** Removes the element at the specified index.
   - **Example:**
     ```move
     public fun remove_element_at_index<T>(v: &mut vector<T>, index: u64) {
         Vector::remove(v, index)
     }
     ```
     This function removes the element at `index` from the vector `v`.

### 13. **`Vector::sort<T>(v: &mut vector<T>, comparator: &impl Fn(&T, &T) -> bool)`**
   - **Description:** Sorts the elements of the vector using the provided comparator function.
   - **Example:**
     ```move
     public fun sort_vector<T>(v: &mut vector<T>, comparator: &impl Fn(&T, &T) -> bool) {
         Vector::sort(v, comparator)
     }
     ```
     This function sorts the elements of the vector `v` using the provided comparator.

---

### Important Considerations:
- **Type `T`:** The type parameter `T` can be any Move type that is stored in the vector, including custom types.
- **Memory and Performance:** Like other collections, vectors are dynamically resized. Operations like `push_back`, `pop_back`, and `swap_remove` can be more costly as the vector grows, especially when copying or shifting elements.
  
For full documentation and more advanced features, you can refer to the official [Sui Move documentation](https://docs.sui.io/) or explore Sui's standard library for more vector methods.
