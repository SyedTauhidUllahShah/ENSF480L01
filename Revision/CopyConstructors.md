
# Copy Constructor in C++

The copy constructor is a special constructor that initializes a new object as a copy of an existing object. It is used when:
- A new object is created from an existing object, e.g., `MyClass obj2 = obj1;`.
- An object is passed by value to a function.
- An object is returned by value from a function.

The default behavior of a copy constructor performs a shallow copy (i.e., copies the member variables as-is). However, in some cases (e.g., when the class contains pointers to dynamically allocated memory), a deep copy is needed to avoid memory issues such as double deletion or dangling pointers.

## Copy Constructor Syntax
The copy constructor takes a constant reference to an object of the same class to avoid modifying the original object:

```cpp
class MyClass {
public:
    MyClass(const MyClass &obj);  // Copy constructor
};
```

## Example: Copy Constructor with Deep Copy
Let’s look at an example where a deep copy is required.

```cpp
#include <iostream>
using namespace std;

class MyClass {
private:
    int *data;  // Pointer to hold dynamically allocated memory
public:
    // Parameterized constructor
    MyClass(int value) {
        data = new int(value);  // Dynamically allocate memory and store value
        cout << "Parameterized constructor called. Value: " << *data << endl;
    }

    // Copy constructor for deep copy
    MyClass(const MyClass &obj) {
        // Allocate new memory and copy the value from obj's data
        data = new int(*(obj.data));
        cout << "Copy constructor called. Copied value: " << *data << endl;
    }

    // Destructor to free allocated memory
    ~MyClass() {
        delete data;  // Release the dynamically allocated memory
        cout << "Destructor called." << endl;
    }

    // Display function to show the value of data
    void display() {
        cout << "Value: " << *data << endl;
    }
};

int main() {
    MyClass obj1(10);   // Calls the parameterized constructor
    MyClass obj2 = obj1;  // Calls the copy constructor (deep copy)
    
    // Display values for both objects
    obj1.display();  // Displays 10
    obj2.display();  // Displays 10
    
    return 0;
}
```

### Line-by-Line Explanation:
- `int *data;`:
  - A pointer to hold dynamically allocated memory. This will store the value passed during object construction.

- `MyClass(int value):`
  - Parameterized constructor that dynamically allocates memory for the data pointer and assigns the value to it.

- `data = new int(value);`: 
  - Allocates memory dynamically and stores value in the allocated space.

- `MyClass(const MyClass &obj):`
  - The copy constructor is defined to create a new object as a deep copy of obj. This means a new memory block is allocated, and the value from `obj.data` is copied into the new memory.

- `data = new int(*(obj.data));`: 
  - This creates new memory and copies the value pointed to by `obj.data` into it.

- Destructor (`~MyClass()`):
  - The destructor is responsible for freeing dynamically allocated memory when an object is destroyed (e.g., when it goes out of scope).

- `delete data;`: 
  - Releases the memory that was allocated during the constructor or copy constructor.

- `MyClass obj1(10);`:
  - Calls the parameterized constructor, allocating memory for `data` and setting its value to 10.

- `MyClass obj2 = obj1;`:
  - Calls the copy constructor to create `obj2` as a copy of `obj1`. In this case, a deep copy is made, so `obj2.data` points to a separate memory location with the same value (10).

- `obj1.display();` and `obj2.display();`:
  - Display the values stored in `obj1` and `obj2`. Both show 10, but the values are stored in different memory locations due to the deep copy.

## Why Use a Copy Constructor?
In cases where a class contains pointers to dynamically allocated memory, a copy constructor ensures that when an object is copied, the copied object gets its own separate memory allocation. This prevents multiple objects from pointing to the same memory, which could lead to double deletion (when both objects try to free the same memory) or dangling pointers.

## Shallow Copy vs. Deep Copy
- **Shallow Copy**: The default behavior where the copy constructor simply copies the pointer values. This results in two objects pointing to the same memory location, which can lead to issues such as memory corruption or double deletion.

- **Deep Copy**: The copy constructor allocates separate memory for the new object and copies the value from the original object into this new memory. This ensures that the copied object has its own independent memory, preventing interference between objects.

## Example of the Problem with Shallow Copy (No Deep Copy):
```cpp
#include <iostream>
using namespace std;

class MyClass {
private:
    int *data;
public:
    // Parameterized constructor
    MyClass(int value) {
        data = new int(value);
        cout << "Parameterized constructor called. Value: " << *data << endl;
    }

    // Default copy constructor (shallow copy)
    // Destructor to free allocated memory
    ~MyClass() {
        delete data;  // Releases the dynamically allocated memory
        cout << "Destructor called." << endl;
    }
};

int main() {
    MyClass obj1(10);   // Calls the parameterized constructor
    MyClass obj2 = obj1;  // Default shallow copy constructor (no deep copy)
    
    return 0;
}
```

## What Happens with Shallow Copy:
- The default copy constructor performs a shallow copy, meaning `obj2.data` points to the same memory as `obj1.data`.
- When the destructor is called for both objects at the end of `main()`, both `obj1` and `obj2` will try to delete the same memory location. This results in undefined behavior or a double deletion error because the same memory is freed twice.
