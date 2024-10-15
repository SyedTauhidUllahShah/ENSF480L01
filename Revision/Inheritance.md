
# Inheritance in C++ (Single Inheritance)

Inheritance is one of the core principles of object-oriented programming (OOP). In C++, inheritance allows a class (called a derived class) to inherit properties and behaviors (members and methods) from another class (called a base class). The derived class can use the functionality of the base class, and it can also extend or override that functionality.

---

## Key Concepts:

- **Base Class**: The class whose members (attributes and methods) are inherited by another class.
- **Derived Class**: The class that inherits from the base class.
- **Access Specifiers**: These control the visibility of base class members in the derived class:
  - **public**: The derived class inherits public members of the base class as public.
  - **protected**: The derived class inherits protected members, but they are only accessible within the class and its derived classes.
  - **private**: The derived class inherits private members, but they remain private and are not accessible outside the base class.

---

## Syntax of Single Inheritance

The syntax for inheritance in C++ is:

```cpp
class BaseClass {
    // Base class members
};

class DerivedClass : public BaseClass {
    // Derived class members
};
```

The colon (`:`) indicates inheritance, and `public` specifies that the derived class inherits the base class members with public access.

---

## Example: Single Inheritance

Let’s look at a simple example where a `BaseClass` defines common attributes and methods, and a `DerivedClass` inherits those properties and methods.

```cpp
#include <iostream>
using namespace std;

// Base class
class Animal {
public:
    void eat() {
        cout << "This animal is eating." << endl;
    }
};

// Derived class
class Dog : public Animal {
public:
    void bark() {
        cout << "The dog is barking." << endl;
    }
};

int main() {
    Dog dog;       // Create a Dog object
    dog.eat();     // Inherited method from Animal class
    dog.bark();    // Dog's own method

    return 0;
}
```

---

### Explanation:

- **Base Class Animal**: The class `Animal` defines a method `eat()`. This method is general to all animals, and other animal classes (like `Dog`) can reuse this behavior.
- **Derived Class Dog**: The `Dog` class inherits from the `Animal` class using the `public` keyword. This means that `Dog` will inherit all public methods from `Animal` and can access them directly.
- **Inheritance in Action**: In `main()`, we create an object of the `Dog` class. Since `Dog` inherits from `Animal`, the `Dog` object has access to the `eat()` method, which belongs to the `Animal` class. The `Dog` class also defines its own method, `bark()`. This demonstrates that the `Dog` class can add new functionality in addition to the functionality it inherits from `Animal`.

---

## Access Specifiers in Inheritance

The access specifiers (`public`, `protected`, `private`) determine how the base class members are inherited into the derived class.

### Example: Access Specifiers in Inheritance

Let’s modify the example to demonstrate how different access specifiers work.

```cpp
#include <iostream>
using namespace std;

// Base class
class Animal {
protected:
    void sleep() {
        cout << "This animal is sleeping." << endl;
    }
public:
    void eat() {
        cout << "This animal is eating." << endl;
    }
};

// Derived class
class Dog : public Animal {
public:
    void bark() {
        cout << "The dog is barking." << endl;
    }

    void sleepNow() {
        sleep();  // Accessing protected method from base class
    }
};

int main() {
    Dog dog;

    dog.eat();      // Accessing public method from base class
    dog.bark();     // Accessing Dog's own method
    dog.sleepNow(); // Accessing protected method from base class indirectly

    return 0;
}
```

---

### Explanation:

- **Protected Member**: The method `sleep()` in the `Animal` class is declared as `protected`. This means that it can only be accessed by the `Animal` class itself or by classes derived from `Animal`. The method is not accessible directly from outside the class, including from objects of `Dog`.
- **Accessing Protected Members**: The `Dog` class can access the `sleep()` method because it is derived from the `Animal` class. In the `sleepNow()` method, `Dog` calls the `sleep()` method of `Animal`.
- **Public Member**: The method `eat()` is public in `Animal`, so it is inherited as a public method in `Dog`. The `dog` object can directly call `dog.eat()`.

---

## Constructors and Inheritance

When a derived class is instantiated, the constructor of the base class is called first, followed by the constructor of the derived class. This is because the derived class relies on the base class for its initialization.

### Example: Constructors in Inheritance

```cpp
#include <iostream>
using namespace std;

// Base class
class Animal {
public:
    Animal() {
        cout << "Animal constructor called." << endl;
    }
};

// Derived class
class Dog : public Animal {
public:
    Dog() {
        cout << "Dog constructor called." << endl;
    }
};

int main() {
    Dog dog;  // Animal constructor is called first, then Dog constructor
    return 0;
}
```

---

### Explanation:

- **Base Class Constructor**: When the `Dog` object is created, the constructor of `Animal` is called first. This ensures that the base part of the object (the `Animal` part) is initialized before the derived part (`Dog`).
- **Derived Class Constructor**: After the `Animal` constructor is executed, the `Dog` constructor is called to initialize the `Dog`-specific members.

---

## Key Points About Single Inheritance:

- **Reuse**: Inheritance allows reusing the code of a base class in a derived class. The derived class can use all public and protected members of the base class.
- **Extension**: The derived class can add new functionality or override existing methods of the base class.
- **Access Specifiers**: The base class members' access level can affect how they are inherited (public, protected, private).
- **Constructors**: The base class constructor is called before the derived class constructor when an object of the derived class is created.
