
# Polymorphism in C++

Polymorphism is one of the core principles of object-oriented programming (OOP) in C++. It allows objects of different classes to be treated as objects of a common base class. The word "polymorphism" means "many shapes" or "many forms," and in programming, it refers to the ability of different objects to respond to the same function call in different ways.

---

## Types of Polymorphism in C++:

- **Compile-Time (Static) Polymorphism**: This includes function overloading and operator overloading.
- **Run-Time (Dynamic) Polymorphism**: This is achieved through inheritance and virtual functions.

Let’s explore these concepts in detail with simple examples.

---

## 1. Compile-Time (Static) Polymorphism

In compile-time polymorphism, the function that gets called is resolved at compile time. This is achieved using function overloading and operator overloading.

---

### Function Overloading

Function overloading allows multiple functions with the same name to be defined, but with different types or numbers of parameters. The compiler determines which function to call based on the function signature.

### Example:
```cpp
#include <iostream>
using namespace std;

class MathOperations {
public:
    // Function to add two integers
    int add(int a, int b) {
        return a + b;
    }

    // Function to add two floats
    float add(float a, float b) {
        return a + b;
    }
};

int main() {
    MathOperations math;
    cout << "Addition of integers: " << math.add(5, 3) << endl;    // Calls add(int, int)
    cout << "Addition of floats: " << math.add(2.5f, 3.5f) << endl; // Calls add(float, float)
    return 0;
}
```

### Explanation:

- **Function Overloading**: The `add` function is overloaded, meaning there are two versions of the function with the same name but different parameter types (one for int and one for float).
- **Compile-Time Polymorphism**: The compiler decides which version of the `add` function to call based on the types of the arguments passed during the function call.

---

## 2. Run-Time (Dynamic) Polymorphism

In run-time polymorphism, the function that gets called is determined at run-time based on the object type. This is achieved using inheritance and virtual functions. When a base class declares a function as virtual, the function can be overridden by derived classes, and the most specific function will be called at runtime depending on the object’s type.

---

### Key Concepts:

- **Inheritance**: The derived class inherits from the base class.
- **Virtual Function**: A function in the base class that can be overridden by a derived class to provide specific functionality.
- **Pointer or Reference to Base Class**: To enable dynamic polymorphism, we use pointers or references to the base class, which can point to derived class objects.

---

### Example: Virtual Functions and Dynamic Polymorphism

```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    // Virtual function
    virtual void makeSound() const {
        cout << "Animal makes a sound" << endl;
    }
};

class Dog : public Animal {
public:
    // Override base class method
    void makeSound() const override {
        cout << "Dog barks" << endl;
    }
};

class Cat : public Animal {
public:
    // Override base class method
    void makeSound() const override {
        cout << "Cat meows" << endl;
    }
};

int main() {
    Animal* animalPtr;  // Pointer to base class

    Dog dog;
    Cat cat;

    // Point to Dog object
    animalPtr = &dog;
    animalPtr->makeSound();  // Output: Dog barks (run-time polymorphism)

    // Point to Cat object
    animalPtr = &cat;
    animalPtr->makeSound();  // Output: Cat meows (run-time polymorphism)

    return 0;
}
```

---

### Explanation:

- **Base Class Animal**: The `Animal` class has a virtual function `makeSound()`. Declaring this function as virtual enables dynamic polymorphism.
- **Derived Classes Dog and Cat**: The `Dog` and `Cat` classes inherit from `Animal` and override the `makeSound()` function with their own specific implementation.
- **Dynamic Polymorphism**: The pointer `animalPtr` of type `Animal*` points to objects of both `Dog` and `Cat`. Despite being of type `Animal*`, when the `makeSound()` function is called, the most specific version of the function is called (i.e., the version in `Dog` or `Cat`), depending on which object the pointer points to. This is resolved at run-time, not at compile-time.

---

## How Dynamic Polymorphism Works:

- **Virtual Table (vtable)**: Behind the scenes, C++ uses a mechanism called the vtable (virtual table) to support dynamic polymorphism. When a class has virtual functions, the compiler creates a vtable for that class. The vtable stores pointers to the actual functions that should be called when a virtual function is invoked.
- **Virtual Pointer (vptr)**: Each object of a class that contains virtual functions has a hidden pointer (vptr) to its class’s vtable. At run-time, this vptr is used to resolve the correct function to call.

---

## Key Points to Remember About Polymorphism:

- **Compile-Time Polymorphism (Static Binding)**:
  - Achieved through function overloading or operator overloading.
  - The decision on which function to call is made at compile time.
  
- **Run-Time Polymorphism (Dynamic Binding)**:
  - Achieved through inheritance and virtual functions.
  - The decision on which function to call is made at run-time based on the actual object type, not the reference or pointer type.
  - You must use pointers or references to achieve run-time polymorphism.

---

## Virtual Functions:

- A function declared with the `virtual` keyword in the base class allows derived classes to override it.
- If you want to ensure that a derived class must override a base class function, you can make the function a pure virtual function by using `= 0`, making the base class abstract.

---

### Simple Example of Pure Virtual Function (Abstract Class)

A pure virtual function makes a class abstract, meaning you cannot instantiate it directly.

```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    // Pure virtual function
    virtual void makeSound() const = 0;  // This makes Animal an abstract class
};

class Dog : public Animal {
public:
    // Override base class method
    void makeSound() const override {
        cout << "Dog barks" << endl;
    }
};

class Cat : public Animal {
public:
    // Override base class method
    void makeSound() const override {
        cout << "Cat meows" << endl;
    }
};

int main() {
    Animal* animalPtr;  // Pointer to base class (abstract class)

    Dog dog;
    Cat cat;

    animalPtr = &dog;
    animalPtr->makeSound();  // Output: Dog barks

    animalPtr = &cat;
    animalPtr->makeSound();  // Output: Cat meows

    return 0;
}
```

---

### Explanation:

- **Pure Virtual Function**: The function `virtual void makeSound() const = 0;` is a pure virtual function. This makes `Animal` an abstract class, meaning it cannot be instantiated directly.
- **Derived Classes**: The `Dog` and `Cat` classes must override the pure virtual function in the `Animal` class to provide specific implementations.
