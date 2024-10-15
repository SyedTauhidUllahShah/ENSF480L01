
# Friend in C++

In C++, the `friend` keyword allows a function or another class to access the private and protected members of a class, even though it is not a member of that class. By default, private and protected members of a class are only accessible to member functions of that class. However, in certain cases, you may need to allow non-member functions or other classes to access these members, and that’s where the `friend` keyword is used.

---

## Key Points about Friends:
- A friend function or friend class can access private and protected members of a class.
- Friendship is not reciprocal: If class A is a friend of class B, class B is not automatically a friend of class A.
- Friendship is not inherited: Derived classes do not inherit the friends of their base class.
- Declaring a function or class as a friend does not make it a member of the class.

---

## 1. Friend Functions

A friend function is a function that is not a member of the class, but it can access the private and protected members of the class. A friend function is declared inside the class with the keyword `friend`.

### Example: Friend Function
```cpp
#include <iostream>
using namespace std;

class MyClass {
private:
    int data;
public:
    MyClass(int value) : data(value) {}  // Constructor to initialize data

    // Declare the global function as a friend of the class
    friend void displayData(const MyClass &obj);
};

// Definition of the friend function
void displayData(const MyClass &obj) {
    // Since displayData is a friend, it can access the private members of MyClass
    cout << "Data: " << obj.data << endl;
}

int main() {
    MyClass obj(10);  // Create an object of MyClass with data = 10
    displayData(obj); // Call the friend function, which can access obj's private data
    return 0;
}
```

### Line-by-Line Explanation:
- `friend void displayData(const MyClass &obj);`: Declares `displayData` as a friend function inside the class. Although `displayData` is not a member of the class, it can access the private member `data` because it is a friend.
- `void displayData(const MyClass &obj)`: Defines the friend function. It takes a reference to an object of `MyClass` and can access its private members (`obj.data`).
- `displayData(obj);`: Calls the friend function `displayData`, which can access the private member data of `obj`.

---

## 2. Friend Classes

A friend class is a class whose members have access to the private and protected members of another class. If class A is a friend of class B, then class A's member functions can access the private and protected members of class B.

### Example: Friend Class
```cpp
#include <iostream>
using namespace std;

class MyClass {
private:
    int data;
public:
    MyClass(int value) : data(value) {}  // Constructor to initialize data

    // Declare the class FriendClass as a friend of MyClass
    friend class FriendClass;
};

class FriendClass {
public:
    void display(const MyClass &obj) {
        // Since FriendClass is a friend, it can access private members of MyClass
        cout << "Data from FriendClass: " << obj.data << endl;
    }
};

int main() {
    MyClass obj(20);       // Create an object of MyClass with data = 20
    FriendClass friendObj;  // Create an object of FriendClass
    friendObj.display(obj); // FriendClass can access MyClass's private member
    return 0;
}
```

### Line-by-Line Explanation:
- `friend class FriendClass;`: Declares the entire `FriendClass` as a friend of `MyClass`. This allows all the member functions of `FriendClass` to access the private members of `MyClass`.
- `void display(const MyClass &obj)`: Defines a member function of `FriendClass` that takes an object of `MyClass`. It can access the private member `data` of `MyClass` because `FriendClass` is declared as a friend.
- `friendObj.display(obj);`: Calls `display`, a member function of `FriendClass`, which accesses the private member data of `obj`.

---

## 3. Friend Function of Two Classes

A function can be a friend of multiple classes, allowing it to access the private and protected members of more than one class. This can be useful when you want a function to operate on two different classes and access their private data.

### Example: Friend Function for Two Classes
```cpp
#include <iostream>
using namespace std;

class ClassA;  // Forward declaration of ClassA

class ClassB {
private:
    int dataB;
public:
    ClassB(int value) : dataB(value) {}

    // Friend function declaration
    friend void add(ClassA &a, ClassB &b);
};

class ClassA {
private:
    int dataA;
public:
    ClassA(int value) : dataA(value) {}

    // Friend function declaration
    friend void add(ClassA &a, ClassB &b);
};

// Friend function definition
void add(ClassA &a, ClassB &b) {
    cout << "Sum: " << (a.dataA + b.dataB) << endl;  // Accesses private members of both ClassA and ClassB
}

int main() {
    ClassA objA(10);  // Create object of ClassA
    ClassB objB(20);  // Create object of ClassB

    add(objA, objB);  // Call the friend function to add private members
    return 0;
}
```

### Line-by-Line Explanation:
- `friend void add(ClassA &a, ClassB &b);`: Declares the `add` function as a friend of both `ClassA` and `ClassB`. This allows `add` to access the private members of both classes.
- `void add(ClassA &a, ClassB &b)`: Defines the friend function `add`. It takes references to objects of `ClassA` and `ClassB` and can access their private members.
- `add(objA, objB);`: Calls the `add` function to add the private members of `objA` and `objB`.

---

## 4. Friendship and Inheritance

Friendship is not inherited. If class B is a derived class of class A, and class C is a friend of class A, class C will not automatically have access to the private members of class B.

---

## Summary of `friend` in C++:
- **Friend Function**: A non-member function that has access to the private and protected members of a class.
  - Syntax: Declared inside the class with the keyword `friend`.
  - Usage: Useful when you need external functions to access class internals.
  
- **Friend Class**: A class whose member functions have access to the private and protected members of another class.
  - Syntax: Declared with `friend class ClassName;`.
  - Usage: Useful when you want two classes to work closely together, accessing each other’s private data.
  
- **Friend Function for Multiple Classes**: A single function can be a friend of multiple classes, allowing it to access private data from more than one class.

---

### Key Points to Remember:
- **Friendship is not reciprocal**: If ClassA is a friend of ClassB, ClassB is not automatically a friend of ClassA.
- **Friendship is not inherited**: A derived class does not inherit the friends of its base class.
- Friends can break encapsulation by allowing access to private members, so use them carefully.
- The `friend` keyword provides flexibility when strict encapsulation is not desired, but it should be used sparingly as it can lead to tighter coupling between classes.
