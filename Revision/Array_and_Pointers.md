
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
    int arr[3] = {100, 2