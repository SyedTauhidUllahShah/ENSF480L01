
# Arrays in C++

An array is a collection of elements of the same data type stored in contiguous memory locations. Arrays allow easy access to elements using indices, and their size is fixed once defined.

## 1. Declaring and Initializing Arrays

Arrays can be declared and initialized at the same time:

```cpp
int arr[5] = {10, 20, 30, 40, 50};  // Declares and initializes an array of 5 integers
```
Here, `arr` is an array of size 5, holding integers.

## 2. Accessing Array Elements

Array elements are accessed using an index (starting from 0):

```cpp
int firstElement = arr[0];  // Accesses the first element, which is 10
int secondElement = arr[1];  // Accesses the second element, which is 20
```

## 3. Memory Representation of Arrays

Arrays are stored in contiguous memory locations. This means the elements of the array are placed one after another in memory. For example, if `arr[0]` is stored at address `0x1000`, then `arr[1]` will be stored at `0x1004` (assuming an `int` takes 4 bytes).

## Pointers and Arrays

In C++, arrays and pointers are closely related. The name of an array (e.g., `arr`) can be thought of as a constant pointer to the first element of the array.

### 1. Array Name as a Pointer

The name of an array (e.g., `arr`) is a pointer to its first element. Therefore, the following statements are equivalent:

```cpp
int *ptr = arr;      // arr acts like a pointer to the first element
int firstElement = *ptr;  // Dereferencing ptr gives the first element (same as arr[0])
```
This means `arr` can be used like a pointer, but with one key difference: you cannot modify the value of `arr` (because it’s a constant pointer).

### 2. Accessing Array Elements with Pointers

Using pointer arithmetic, you can access array elements by moving the pointer. The following two lines of code are equivalent:

```cpp
int secondElement = arr[1];  // Access using array index
int secondElementPtr = *(arr + 1);  // Access using pointer arithmetic
```
`arr + 1` moves the pointer to the next element in the array, and `*(arr + 1)` dereferences that pointer to get the value of the second element.

#### Example of Pointer and Array Access:

```cpp
#include <iostream>
using namespace std;

int main() {
    int arr[3] = {100, 200, 300};

    // Access elements using array indices
    cout << "Array access using indices: " << arr[0] << ", " << arr[1] << ", " << arr[2] << endl;

    // Access elements using pointers
    int *ptr = arr;  // ptr points to the first element of arr
    cout << "Array access using pointers: " << *ptr << ", " << *(ptr + 1) << ", " << *(ptr + 2) << endl;

    return 0;
}
```

## Pointer Arithmetic

Pointer arithmetic is a technique to move pointers across an array or memory. This allows the pointer to "traverse" through elements in memory.

### 1. Pointer Arithmetic Operations

- **Incrementing a Pointer (`ptr++`)**:
  Moves the pointer to the next element in memory. This doesn’t increase the pointer value by 1, but by the size of the data type the pointer is pointing to.

  ```cpp
  ptr++;  // Moves the pointer to the next memory address of the type it points to
  ```

- **Decrementing a Pointer (`ptr--`)**:
  Moves the pointer to the previous element in memory.

- **Addition of an Integer to a Pointer (`ptr + n`)**:
  Moves the pointer forward by `n` elements.

  ```cpp
  ptr + 3;  // Moves the pointer 3 elements forward
  ```

- **Subtraction of an Integer from a Pointer (`ptr - n`)**:
  Moves the pointer backward by `n` elements.

### 2. Example of Pointer Arithmetic

Let’s take an example of traversing an array using pointer arithmetic:

```cpp
#include <iostream>
using namespace std;

int main() {
    int arr[5] = {10, 20, 30, 40, 50};
    int *ptr = arr;  // Points to the first element of the array

    // Traverse the array using pointer arithmetic
    cout << "Array elements using pointer arithmetic: ";
    for (int i = 0; i < 5; i++) {
        cout << *ptr << " ";  // Dereference the pointer to get the value
        ptr++;  // Move to the next element in the array
    }
    cout << endl;

    return 0;
}
```


## 3. Understanding How Pointer Arithmetic Works

Suppose `arr[0]` is stored at memory location `0x1000` and an `int` is 4 bytes:
- When you perform `ptr++`, the pointer will move from `0x1000` (address of `arr[0]`) to `0x1004` (address of `arr[1]`), because each integer is 4 bytes.
- Similarly, `ptr + 2` would move to `0x1008`, and dereferencing it (`*(ptr + 2)`) will give you the value of `arr[2]`.

## Combining Pointers, Arrays, and Pointer Arithmetic

In C++, arrays and pointers are often used together for low-level memory manipulation, especially in tasks like iterating over arrays, dynamic memory allocation, and passing arrays to functions.

### Example: Sum of Array Elements Using Pointers

```cpp
#include <iostream>
using namespace std;

int main() {
    int arr[4] = {1, 2, 3, 4};
    int *ptr = arr;  // Pointer to the first element
    int sum = 0;

    // Use pointer arithmetic to calculate the sum of array elements
    for (int i = 0; i < 4; i++) {
        sum += *ptr;  // Dereference the pointer to get the value
        ptr++;  // Move to the next element
    }

    cout << "Sum of array elements: " << sum << endl;  // Output: 10

    return 0;
}
```

## Key Points to Remember

- Arrays and pointers are closely related, but there are some differences:
  - An array name (like `arr`) is a constant pointer to the first element of the array, while a pointer (like `int *ptr`) can be reassigned.
  - Pointer arithmetic is useful for traversing arrays. You can move the pointer forward and backward using arithmetic operations like `++`, `--`, `+`, and `-`.

- When performing pointer arithmetic, the pointer moves by the size of the data type it points to. For example, if `ptr` is an `int*`, `ptr + 1` moves the pointer by 4 bytes (size of `int`).

