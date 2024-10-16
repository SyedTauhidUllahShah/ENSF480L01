
# Inheritance in C++ (Inheritance)

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

## Syntax of Inheritance

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

## Example: Inheritance

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



# Constructors in Inheritance in C++

When dealing with inheritance in C++, one of the key concepts is how constructors work between the base class and the derived class. When an object of a derived class is created, both the base class and the derived class constructors are called. This ensures that the base part of the derived object is properly initialized before the derived class adds its own initialization.

---

## Key Points About Constructors in Inheritance

- **Base Class Constructor Call**: When a derived class object is instantiated, the constructor of the base class is automatically called first, followed by the constructor of the derived class.
- **Constructor Order**: The base class constructor is called before the derived class constructor, and the order of execution is always from the base class to the derived class.
- **Default Constructor**: If no constructor is explicitly defined in the base class, the default constructor is called automatically. However, if the base class has parameterized constructors, the derived class needs to specify which base class constructor should be called.
- **Constructor Initialization List**: The derived class can use a constructor initialization list to specify which constructor of the base class to call and to initialize its own members.

---

## Constructor Call in Inheritance

Let’s explore the details of how constructors are called in inheritance through an example.

---

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
    Animal(string name) {
        cout << "Animal constructor with name: " << name << " called." << endl;
    }
};

// Derived class
class Dog : public Animal {
public:
    // Derived class constructor
    Dog() {
        cout << "Dog constructor called." << endl;
    }

    // Parameterized derived class constructor
    Dog(string dogName, string animalName) : Animal(animalName) {  // Calling base class constructor
        cout << "Dog constructor with name: " << dogName << " called." << endl;
    }
};

int main() {
    cout << "Creating Dog with default constructor:" << endl;
    Dog dog1;  // Calls Animal(), then Dog()

    cout << "
Creating Dog with parameterized constructor:" << endl;
    Dog dog2("Buddy", "AnimalName");  // Calls Animal(animalName), then Dog(dogName)

    return 0;
}
```

### Output:

```
Creating Dog with default constructor:
Animal constructor called.
Dog constructor called.

Creating Dog with parameterized constructor:
Animal constructor with name: AnimalName called.
Dog constructor with name: Buddy called.
```

---

### Detailed Explanation:

- **Base Class Constructor (Animal)**:
  - **Default Constructor (Animal())**: This constructor is called when no parameters are passed to the derived class (`Dog`). It outputs "Animal constructor called."
  - **Parameterized Constructor (Animal(string name))**: This constructor takes a string as an argument. In the derived class, when we want to pass data to the base class constructor, we can do so using the constructor initialization list.

- **Derived Class Constructor (Dog)**:
  - **Default Constructor (Dog())**: This constructor is called when the object `dog1` is created without any arguments. Notice that before the `Dog` constructor is executed, the base class constructor `Animal()` is called first.
  - **Parameterized Constructor (Dog(string dogName, string animalName))**: This constructor calls the base class constructor `Animal(animalName)` using the constructor initialization list `: Animal(animalName)`. This way, the derived class `Dog` is explicitly telling the compiler to call the parameterized constructor of `Animal` and pass the value of `animalName`.

---

## Constructor Initialization List

The constructor initialization list is a way to explicitly initialize base class constructors and member variables before entering the body of the derived class constructor.

### Syntax of Constructor Initialization List:

```cpp
DerivedClass(parameters) : BaseClass(parameters) {
    // Derived class constructor body
}
```

In the above example:
`Dog(string dogName, string animalName) : Animal(animalName)` initializes the base class `Animal` with the parameter `animalName` before the body of the `Dog` constructor is executed.

---

## Key Points About Constructor Initialization:

- **Order of Constructor Calls**:
  - The base class constructor is always called before the derived class constructor, regardless of the constructor initialization list.
  - If no constructor initialization list is provided, the default constructor of the base class is called.

- **Why Use Constructor Initialization List?**:
  - **Efficiency**: The constructor initialization list is more efficient than assigning values inside the body of the constructor because it directly initializes members, avoiding default initialization followed by reassignment.
  - **Required for Constant Members**: If the derived class contains constant members (e.g., `const int x`), those members must be initialized using the initialization list.
  - **Base Class Constructor Parameters**: When the base class does not have a default constructor, you must use a constructor initialization list to specify which base class constructor to call and pass the necessary parameters.

---

## Default Constructor Behavior

If the base class does not have a default constructor (i.e., a constructor without arguments), the derived class must explicitly call one of the base class’s constructors using the initialization list. If the base class only has parameterized constructors and no default constructor, failing to provide a constructor initialization list will result in a compilation error.

---

### Example: Base Class Without Default Constructor

```cpp
#include <iostream>
using namespace std;

// Base class without default constructor
class Animal {
public:
    Animal(string name) {
        cout << "Animal constructor with name: " << name << " called." << endl;
    }
};

// Derived class
class Dog : public Animal {
public:
    // Derived class must use constructor initialization list to call base class constructor
    Dog(string dogName) : Animal("Generic Animal") {
        cout << "Dog constructor with name: " << dogName << " called." << endl;
    }
};

int main() {
    Dog dog("Buddy");  // Calls Animal("Generic Animal"), then Dog("Buddy")

    return 0;
}
```

### Output:

```
Animal constructor with name: Generic Animal called.
Dog constructor with name: Buddy called.
```

---

### Explanation:

- The base class `Animal` does not have a default constructor. It only has a constructor that takes a string argument.
- The derived class `Dog` is required to call the base class constructor using the initialization list. In this case, it passes the value "Generic Animal" to the `Animal` constructor.
- If we did not include the constructor initialization list in `Dog`, the program would fail to compile because the base class `Animal` needs to be initialized with a string, and there is no default constructor available.





# Virtual in Inheritance in C++

The `virtual` keyword in C++ is a key concept in inheritance that allows for run-time polymorphism. It enables the dynamic binding of methods, meaning that the function to be executed is determined at runtime based on the actual object type, rather than the type of the pointer or reference. This is essential in object-oriented programming when you want derived classes to provide their own implementations of base class methods.

When you declare a method as `virtual` in a base class, it allows that method to be overridden in derived classes, and when you invoke that method on a base class pointer or reference, the most derived version of the function is called.

---

## Key Concepts of `virtual` in Inheritance:

- **Virtual Function**:
  - A function in the base class that is declared with the `virtual` keyword, allowing it to be overridden by derived classes.

- **Dynamic Binding (Late Binding)**:
  - The process of determining which function to call (base or derived class version) occurs at runtime. C++ uses a mechanism called the vtable (virtual table) to achieve this.

- **Overriding**:
  - Derived classes provide their own version of the virtual function, which "overrides" the base class version.

- **Base Class Pointer/Reference**:
  - The `virtual` mechanism is primarily used when calling methods via a base class pointer or reference. The object being pointed to determines the version of the function that gets called.

- **Virtual Destructors**:
  - If you intend to delete a derived class object via a base class pointer, the base class destructor should be declared as `virtual` to ensure that the derived class destructor is called first.

---

## Basic Example of Virtual Functions in Inheritance

Let’s explore a basic example where `virtual` enables a function to be overridden in a derived class.

### Example: Virtual Function

```cpp
#include <iostream>
using namespace std;

// Base class
class Animal {
public:
    // Virtual function in the base class
    virtual void makeSound() {
        cout << "Animal makes a sound" << endl;
    }
};

// Derived class
class Dog : public Animal {
public:
    // Override the base class method
    void makeSound() override {
        cout << "Dog barks" << endl;
    }
};

// Derived class
class Cat : public Animal {
public:
    // Override the base class method
    void makeSound() override {
        cout << "Cat meows" << endl;
    }
};

int main() {
    Animal* animalPtr;  // Base class pointer

    Dog dog;
    Cat cat;

    // Point to Dog object and call makeSound
    animalPtr = &dog;
    animalPtr->makeSound();  // Output: Dog barks (dynamic binding)

    // Point to Cat object and call makeSound
    animalPtr = &cat;
    animalPtr->makeSound();  // Output: Cat meows (dynamic binding)

    return 0;
}
```

---

### Explanation:

- **Virtual Function in Base Class**:
  - The function `makeSound()` in the base class `Animal` is declared with the `virtual` keyword. This allows derived classes (`Dog` and `Cat`) to override it with their own versions.

- **Overriding in Derived Classes**:
  - Both `Dog` and `Cat` provide their own implementation of `makeSound()`, overriding the base class version.

- **Dynamic Binding**:
  - The pointer `animalPtr` of type `Animal*` points to objects of both `Dog` and `Cat`. When `makeSound()` is called, the version of the function specific to the object being pointed to (either `Dog` or `Cat`) is executed. This behavior is determined at runtime.

- **Runtime Polymorphism**:
  - The use of `virtual` enables runtime polymorphism. Even though the pointer `animalPtr` is of type `Animal*`, the correct method (either Dog's or Cat's version of `makeSound()`) is called based on the actual type of the object it points to.

---

## Virtual Functions and the vtable (Virtual Table)

When you declare a function as `virtual`, C++ creates a `vtable` (virtual table) for the class. The vtable is essentially a lookup table that holds pointers to the virtual functions for that class. When an object is created, a hidden `vptr` (virtual pointer) is initialized, which points to the vtable. This enables the runtime determination of which function to call (dynamic binding).

- **Base Class vtable**:
  - The base class (`Animal`) has a vtable with a pointer to the `Animal::makeSound()` function.

- **Derived Class vtable**:
  - The derived classes (`Dog` and `Cat`) each have their own vtables. The vtable for `Dog` contains a pointer to `Dog::makeSound()`, and the vtable for `Cat` contains a pointer to `Cat::makeSound()`.

- **Dynamic Binding**:
  - When you call a virtual function through a base class pointer (`animalPtr->makeSound()`), the `vptr` of the object being pointed to is used to look up the correct function in the vtable. This ensures that the most derived version of the function is called.



---

## Why Use Virtual Functions?

- **Polymorphism**:
  - Virtual functions allow you to write flexible and reusable code. The base class can define an interface (set of methods), and derived classes can provide their own implementation. This allows you to work with base class pointers or references while still invoking the most specific behavior of derived class objects.

- **Extensibility**:
  - With virtual functions, you can extend your base class with new derived classes, and the existing code that uses base class pointers will automatically work with the new derived classes, calling their specific implementations.

- **Code Maintainability**:
  - Using virtual functions and polymorphism can reduce redundancy. You can define common behavior in the base class and customize it in derived classes, allowing code sharing and specialization where necessary.

---

## Overriding Virtual Functions

When you override a virtual function in a derived class, you can explicitly indicate that you are overriding the base class method by using the `override` specifier. This is not mandatory, but it helps catch errors during compilation if the function signature does not match the base class version.

---

### Example: Using `override` Specifier

```cpp
#include <iostream>
using namespace std;

// Base class
class Animal {
public:
    virtual void makeSound() {
        cout << "Animal makes a sound" << endl;
    }
};

// Derived class
class Dog : public Animal {
public:
    // Use override to explicitly indicate that this function overrides the base class function
    void makeSound() override {
        cout << "Dog barks" << endl;
    }
};

int main() {
    Animal* animalPtr = new Dog();  // Base class pointer to derived class object
    animalPtr->makeSound();  // Output: Dog barks (dynamic binding)

    delete animalPtr;
    return 0;
}
```

---

### Explanation:

The `override` keyword is optional but recommended. It tells the compiler that the method is meant to override a base class method. If the function signatures don’t match (for example, if you misspell the function name), the compiler will catch the error.

---

## Virtual Destructors

In C++, when you are using inheritance and polymorphism (i.e., when you have base class pointers pointing to derived class objects), you should declare the base class destructor as `virtual`. This ensures that when an object is deleted via a base class pointer, the derived class destructor is called first, followed by the base class destructor.

---

### Example: Virtual Destructor

```cpp
#include <iostream>
using namespace std;

// Base class with virtual destructor
class Animal {
public:
    Animal() {
        cout << "Animal constructor called." << endl;
    }

    virtual ~Animal() {
        cout << "Animal destructor called." << endl;
    }
};

// Derived class
class Dog : public Animal {
public:
    Dog() {
        cout << "Dog constructor called." << endl;
    }

    ~Dog() {
        cout << "Dog destructor called." << endl;
    }
};

int main() {
    Animal* animalPtr = new Dog();  // Base class pointer to derived class object
    delete animalPtr;  // Properly calls Dog destructor first, then Animal destructor

    return 0;
}
```

---

## Pure Virtual Functions and Abstract Classes

A pure virtual function is a virtual function that is declared but not defined in the base class. It forces the derived classes to provide an implementation. A class that contains at least one pure virtual function is called an abstract class and cannot be instantiated directly.

---

### Example: Pure Virtual Function

```cpp
#include <iostream>
using namespace std;

// Base class (Abstract class)
class Animal {
public:
    // Pure virtual function (no implementation)
    virtual void makeSound() = 0;
};

// Derived class
class Dog : public Animal {
public:
    // Override the pure virtual function
    void makeSound() override {
        cout << "Dog barks" << endl;
    }
};

int main() {
    Animal* animalPtr = new Dog();  // Base class pointer to derived class object
    animalPtr->makeSound();  // Output: Dog barks

    delete animalPtr;
    return 0;
}
```

---

### Explanation:

- **Pure Virtual Function**: The `Animal` class declares `makeSound()` as a pure virtual function (`virtual void makeSound() = 0;`). This makes `Animal` an abstract class, which means it cannot be instantiated directly.

- **Forcing Derived Classes to Implement**: Since `Animal` declares a pure virtual function, any derived class (such as `Dog`) must provide an implementation for `makeSound()`. If the derived class does not implement the function, it too will become abstract and cannot be instantiated.





# Destructors in Inheritance in C++

A destructor is a special member function in C++ that is called automatically when an object goes out of scope or is explicitly deleted. Its primary role is to release any resources (such as dynamically allocated memory) held by an object and perform clean-up tasks. In the context of inheritance, destructors play a key role in ensuring that both the base class and the derived class are properly cleaned up.

---

## Key Points About Destructors in Inheritance

- **Order of Destructor Calls**:
  - The destructor of the derived class is called first, followed by the destructor of the base class. This is the reverse order of constructor calls.
  - This ensures that the derived part of the object is destroyed before the base part, maintaining the integrity of the object during destruction.

- **Virtual Destructors**:
  - If you expect to delete an object through a pointer to the base class (i.e., if polymorphism is involved), the base class destructor should be declared as `virtual` to ensure that the derived class destructor is called first, followed by the base class destructor.

- **Automatic Destructor Calls**:
  - If no destructor is explicitly defined, the compiler generates a default destructor that automatically cleans up resources that do not require explicit destruction (such as built-in types).

---

## Example: Destructors in Inheritance

Let’s look at a simple example of destructors in inheritance, demonstrating the order of destructor calls.

```cpp
#include <iostream>
using namespace std;

// Base class
class Animal {
public:
    Animal() {
        cout << "Animal constructor called." << endl;
    }

    ~Animal() {
        cout << "Animal destructor called." << endl;
    }
};

// Derived class
class Dog : public Animal {
public:
    Dog() {
        cout << "Dog constructor called." << endl;
    }

    ~Dog() {
        cout << "Dog destructor called." << endl;
    }
};

int main() {
    Dog dog;  // Creating an object of derived class Dog

    // As the Dog object goes out of scope, destructors will be called automatically
    return 0;
}
```

### Output:

```
Animal constructor called.
Dog constructor called.
Dog destructor called.
Animal destructor called.
```

---

### Explanation:

- **Constructor Call Order**:
  - When the `Dog` object is created, the constructor of the base class `Animal` is called first, followed by the constructor of the derived class `Dog`. This ensures that the base class part of the `Dog` object is initialized before the derived class part.
  
- **Destructor Call Order**:
  - When the `Dog` object goes out of scope at the end of `main()`, the destructor of the derived class `Dog` is called first, followed by the destructor of the base class `Animal`.
  - The destructor order is the reverse of the constructor order: derived class destructor is called first, and base class destructor is called afterward.

---

## Virtual Destructors and Polymorphism

When working with inheritance and polymorphism (i.e., using base class pointers to point to derived class objects), you should declare the base class destructor as `virtual`. This ensures that when an object is deleted via a base class pointer, the derived class destructor is called first, followed by the base class destructor. Without a virtual destructor, only the base class destructor will be called, leading to potential memory leaks or incomplete cleanup.

---

### Example: Virtual Destructors

```cpp
#include <iostream>
using namespace std;

// Base class with virtual destructor
class Animal {
public:
    Animal() {
        cout << "Animal constructor called." << endl;
    }

    virtual ~Animal() {
        cout << "Animal destructor called." << endl;
    }
};

// Derived class
class Dog : public Animal {
public:
    Dog() {
        cout << "Dog constructor called." << endl;
    }

    ~Dog() {
        cout << "Dog destructor called." << endl;
    }
};

int main() {
    Animal* animalPtr = new Dog();  // Base class pointer to a derived class object
    delete animalPtr;  // Calls Dog destructor first, then Animal destructor

    return 0;
}
```

### Output:

```
Animal constructor called.
Dog constructor called.
Dog destructor called.
Animal destructor called.
```

---

### Explanation:

- **Constructor Call**:
  - When `new Dog()` is called, the `Animal` constructor is executed first, followed by the `Dog` constructor.
  
- **Destructor Call**:
  - Since the destructor in the base class `Animal` is declared as `virtual`, when `delete animalPtr` is executed, the destructor of the derived class `Dog` is called first, followed by the destructor of the base class `Animal`.
  - This ensures proper cleanup of both the derived class and the base class, even when using polymorphism (base class pointer pointing to a derived class object).

---

## Key Concepts in Destructor and Inheritance

- **Destructor Call Order**:
  - Inheritance follows a reverse destructor order. The derived class destructor is called first, followed by the base class destructor.
  - This ensures that any derived class-specific resources are cleaned up before the base class resources are destroyed.

- **Virtual Destructors**:
  - **Why Use Virtual Destructors**: In cases where you are using polymorphism (i.e., pointers or references to base class types that point to derived class objects), you need to declare the base class destructor as `virtual`. This guarantees that the derived class destructor is called, preventing memory leaks or incomplete cleanup.
  - Without a virtual destructor, if you delete an object through a base class pointer, only the base class destructor is invoked, and the derived class destructor is skipped.

---

## Destructors in Absence of Explicit Definition

If no destructor is provided, the compiler generates a default destructor, which automatically handles cleanup of built-in types or objects without dynamic memory. However, if dynamic memory allocation is involved (e.g., using `new` to allocate memory), you must provide a custom destructor to release that memory.

---

### Example: Custom Destructor for Dynamic Memory

If a class has dynamically allocated resources (such as memory allocated with `new`), it’s crucial to define a destructor to free those resources.

```cpp
#include <iostream>
using namespace std;

// Base class
class Animal {
public:
    Animal() {
        cout << "Animal constructor called." << endl;
    }

    virtual ~Animal() {
        cout << "Animal destructor called." << endl;
    }
};

// Derived class with dynamic memory
class Dog : public Animal {
private:
    int* age;
public:
    Dog(int a) {
        age = new int(a);  // Dynamically allocate memory
        cout << "Dog constructor called. Age: " << *age << endl;
    }

    ~Dog() {
        delete age;  // Free dynamically allocated memory
        cout << "Dog destructor called." << endl;
    }
};

int main() {
    Animal* animalPtr = new Dog(5);  // Base class pointer to derived class object
    delete animalPtr;  // Properly calls both Dog and Animal destructors

    return 0;
}
```

### Output:

```
Animal constructor called.
Dog constructor called. Age: 5
Dog destructor called.
Animal destructor called.
```

---

### Explanation:

- **Dynamic Memory Allocation**:
  - The `Dog` class dynamically allocates memory for the `age` member using `new`.
  
- **Destructor Cleanup**:
  - The `Dog` destructor releases the dynamically allocated memory using `delete`. If the destructor were missing or incomplete, this would lead to a memory leak.
  
- **Virtual Destructor**:
  - Since the `Animal` destructor is `virtual`, deleting `animalPtr` ensures that both the `Dog` destructor and the `Animal` destructor are called in the correct order, properly cleaning up the object and its resources.

---

## Summary of Destructors in Inheritance

- **Destructor Call Order**:
  - Destruction follows the reverse order of construction: the destructor of the derived class is called first, followed by the base class destructor.
  
- **Virtual Destructors**:
  - Use virtual destructors when you are using inheritance and polymorphism to ensure that derived class destructors are called when a base class pointer is used to delete an object.
  
- **Custom Destructor for Dynamic Memory**:
  - If a class allocates dynamic memory (e.g., using `new`), you must explicitly provide a destructor to free that memory. This ensures that no memory leaks occur when the object is destroyed.

- **Automatic Destructor Generation**:
  - If no destructor is explicitly defined, the compiler generates a default destructor. However, this default destructor does not handle dynamic memory cleanup, so if dynamic memory allocation is involved, a custom destructor is necessary.



# Public, Private, and Protected Inheritance in C++

In C++, inheritance can be categorized based on the access specifier used when a derived class inherits from a base class. These access specifiers control how the members of the base class are inherited in the derived class and how they can be accessed by the derived class and other parts of the program.

The three types of inheritance are:
- Public Inheritance
- Private Inheritance
- Protected Inheritance

Each of these inheritance types alters the visibility of the base class members when they are inherited by the derived class.

---

## Key Concepts of Access Specifiers
- **Public Members**: Members declared as public in the base class are accessible anywhere the object is visible.
- **Protected Members**: Members declared as protected are accessible only within the base class, its derived classes, and friends of the base class.
- **Private Members**: Members declared as private are accessible only within the base class and friends of the base class. They are not accessible by derived classes.

---

## 1. Public Inheritance

In public inheritance, the public and protected members of the base class maintain their access levels in the derived class. This is the most commonly used form of inheritance because it models the "is-a" relationship between the base and derived classes.

### Access Levels in Public Inheritance:
- Public members of the base class become public in the derived class.
- Protected members of the base class become protected in the derived class.
- Private members of the base class are not accessible directly in the derived class, but they can still be accessed via public or protected methods of the base class.

### Syntax:
```cpp
class Derived : public Base {
    // Body of derived class
};
```

### Example: Public Inheritance
```cpp
#include <iostream>
using namespace std;

// Base class
class Animal {
public:
    void eat() {
        cout << "This animal is eating." << endl;
    }
protected:
    void sleep() {
        cout << "This animal is sleeping." << endl;
    }
private:
    void breathe() {
        cout << "This animal is breathing." << endl;
    }
};

// Derived class with public inheritance
class Dog : public Animal {
public:
    void bark() {
        cout << "The dog is barking." << endl;
        sleep();  // Accessing protected member
    }
};

int main() {
    Dog dog;
    dog.eat();    // Public in Animal, public in Dog
    dog.bark();   // Method of Dog
    // dog.sleep();  // Error: sleep is protected in Dog
    return 0;
}
```

---

### Explanation:
- **Public Members (eat())**: The `eat()` method is public in Animal and remains public in Dog. It can be accessed directly through a Dog object.
- **Protected Members (sleep())**: The `sleep()` method is protected in Animal and remains protected in Dog. It can be accessed inside the Dog class but not directly through a Dog object.
- **Private Members (breathe())**: The `breathe()` method is private in Animal and is not accessible in Dog or from a Dog object.

---

## 2. Private Inheritance

In private inheritance, all the public and protected members of the base class become private members in the derived class. This means that no outside class can access the base class members via the derived class, even if those members were public in the base class.

### Access Levels in Private Inheritance:
- Public members of the base class become private in the derived class.
- Protected members of the base class become private in the derived class.
- Private members of the base class remain private and inaccessible in the derived class.

Private inheritance is usually used when you want to reuse the functionality of the base class without exposing its interface to the users of the derived class.

### Syntax:
```cpp
class Derived : private Base {
    // Body of derived class
};
```

### Example: Private Inheritance
```cpp
#include <iostream>
using namespace std;

// Base class
class Animal {
public:
    void eat() {
        cout << "This animal is eating." << endl;
    }
protected:
    void sleep() {
        cout << "This animal is sleeping." << endl;
    }
};

// Derived class with private inheritance
class Dog : private Animal {
public:
    void bark() {
        cout << "The dog is barking." << endl;
        eat();   // Public in Animal, but private in Dog
        sleep(); // Protected in Animal, but private in Dog
    }
};

int main() {
    Dog dog;
    dog.bark();  // Dog's method
    // dog.eat();   // Error: eat is private in Dog
    // dog.sleep(); // Error: sleep is private in Dog
    return 0;
}
```

---

### Explanation:
- **Public Members (eat())**: The `eat()` method, though public in Animal, becomes private in Dog. It can only be accessed within the Dog class and not through a Dog object.
- **Protected Members (sleep())**: The `sleep()` method is protected in Animal but becomes private in Dog. It can be used within the Dog class but not accessed from outside.
- **Access in Dog**: Inside the `bark()` method, Dog can call both `eat()` and `sleep()` because they are now private members of Dog.

---

## 3. Protected Inheritance

In protected inheritance, the public and protected members of the base class become protected members in the derived class. This means that even though public members of the base class remain accessible to the derived class and its subclasses, they cannot be accessed directly from objects of the derived class.

Protected inheritance is less common but is useful when you want to allow derived classes to use the base class's members while restricting their access from outside the class hierarchy.

### Access Levels in Protected Inheritance:
- Public members of the base class become protected in the derived class.
- Protected members of the base class remain protected in the derived class.
- Private members of the base class remain private and inaccessible in the derived class.

### Syntax:
```cpp
class Derived : protected Base {
    // Body of derived class
};
```

### Example: Protected Inheritance
```cpp
#include <iostream>
using namespace std;

// Base class
class Animal {
public:
    void eat() {
        cout << "This animal is eating." << endl;
    }
protected:
    void sleep() {
        cout << "This animal is sleeping." << endl;
    }
};

// Derived class with protected inheritance
class Dog : protected Animal {
public:
    void bark() {
        cout << "The dog is barking." << endl;
        eat();   // Public in Animal, but protected in Dog
        sleep(); // Protected in Animal, protected in Dog
    }
};

int main() {
    Dog dog;
    dog.bark();  // Dog's method
    // dog.eat();   // Error: eat is protected in Dog
    // dog.sleep(); // Error: sleep is protected in Dog
    return 0;
}
```

---

### Explanation:
- **Public Members (eat())**: The `eat()` method is public in Animal but becomes protected in Dog. It can only be accessed within Dog or its derived classes, but not from outside through a Dog object.
- **Protected Members (sleep())**: The `sleep()` method remains protected in Dog. It can be accessed within the Dog class but not from outside.
- **Access in Dog**: The `bark()` method of Dog can access both `eat()` and `sleep()` because they are protected members.

---

## Summary of Public, Private, and Protected Inheritance

| Inheritance Type    | Public Members in Base   | Protected Members in Base   | Private Members in Base  |
|---------------------|--------------------------|-----------------------------|--------------------------|
| Public Inheritance   | Public in Derived        | Protected in Derived         | Not accessible            |
| Private Inheritance  | Private in Derived       | Private in Derived           | Not accessible            |
| Protected Inheritance| Protected in Derived     | Protected in Derived         | Not accessible            |
