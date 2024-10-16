
# Multiple Inheritance in C++

Multiple inheritance is a feature in C++ that allows a class to inherit from more than one base class. In other words, a derived class can have multiple parent classes. This can be useful in scenarios where a derived class needs to inherit functionality or properties from multiple distinct base classes.

While multiple inheritance is a powerful feature, it introduces some complexities, especially with regard to ambiguities and diamond problem, which we'll cover in detail.

## Syntax of Multiple Inheritance

In multiple inheritance, a class can inherit from multiple base classes by separating the base class names with commas in the class declaration.

### Syntax:
```cpp
class Derived : public Base1, public Base2 {
    // Derived class body
};
```
Here, the `Derived` class inherits from both `Base1` and `Base2`.

## Example: Multiple Inheritance
Let’s consider an example where a `Person` class, a `Worker` class, and a `Student` class exist, and we want to create a derived class `PartTimeStudentWorker` that inherits from both `Worker` and `Student`.

```cpp
#include <iostream>
using namespace std;

// Base class 1
class Worker {
public:
    void work() {
        cout << "Worker is working." << endl;
    }
};

// Base class 2
class Student {
public:
    void study() {
        cout << "Student is studying." << endl;
    }
};

// Derived class with multiple inheritance
class PartTimeStudentWorker : public Worker, public Student {
public:
    void balance() {
        cout << "Balancing work and study." << endl;
    }
};

int main() {
    PartTimeStudentWorker person;

    person.work();    // Inherited from Worker
    person.study();   // Inherited from Student
    person.balance(); // PartTimeStudentWorker's own method

    return 0;
}
```

### Output:
```
Worker is working.
Student is studying.
Balancing work and study.
```

### Explanation:
- **Base Classes (`Worker`, `Student`)**: The `Worker` class has a `work()` method, and the `Student` class has a `study()` method.
- **Derived Class (`PartTimeStudentWorker`)**: The `PartTimeStudentWorker` class inherits from both `Worker` and `Student`. As a result, it has access to the `work()` method from `Worker` and the `study()` method from `Student`.
- **Method Access**: The `person` object of type `PartTimeStudentWorker` can call methods from both base classes (`work()` and `study()`) as well as its own method (`balance()`).

## Ambiguities in Multiple Inheritance
One common issue with multiple inheritance is ambiguity. If two base classes have a member with the same name (e.g., a function or a data member), the derived class may not know which one to inherit. In such cases, the compiler raises an ambiguity error, and you need to specify explicitly which base class's member you are referring to.

### Example: Ambiguity in Multiple Inheritance
```cpp
#include <iostream>
using namespace std;

// Base class 1
class Worker {
public:
    void display() {
        cout << "Worker display" << endl;
    }
};

// Base class 2
class Student {
public:
    void display() {
        cout << "Student display" << endl;
    }
};

// Derived class with multiple inheritance
class PartTimeStudentWorker : public Worker, public Student {
public:
    void show() {
        // Attempt to call display() will cause ambiguity
        // display();  // Error: ambiguous
        Worker::display();  // Resolving ambiguity by specifying base class
        Student::display(); // Resolving ambiguity by specifying base class
    }
};

int main() {
    PartTimeStudentWorker person;
    person.show();  // Explicitly calls the display method of both base classes
    return 0;
}
```

### Output:
```
Worker display
Student display
```

### Explanation:
- **Ambiguity**: The `display()` function exists in both `Worker` and `Student`. If we try to call `display()` directly inside `PartTimeStudentWorker`, the compiler cannot determine which version of the function to call, resulting in an ambiguity error.
- **Resolving Ambiguity**: To resolve the ambiguity, we specify the base class name (e.g., `Worker::display()` or `Student::display()`) to tell the compiler explicitly which version of the method to call.

## The Diamond Problem in Multiple Inheritance
The diamond problem occurs when a class inherits from two base classes that both inherit from a common base class. This creates a "diamond-shaped" inheritance structure. In such cases, the derived class inherits two copies of the common base class's members, which can lead to ambiguity and redundancy.

### Diamond Problem Example
```cpp
#include <iostream>
using namespace std;

// Common base class
class Animal {
public:
    void speak() {
        cout << "Animal speaking" << endl;
    }
};

// Derived class 1
class Mammal : public Animal {};

// Derived class 2
class Bird : public Animal {};

// Derived class with multiple inheritance (diamond problem)
class Bat : public Mammal, public Bird {
public:
    void sound() {
        // Attempting to call speak() will cause ambiguity
        // speak();  // Error: ambiguous
        Mammal::speak();  // Resolving ambiguity by specifying Mammal
        Bird::speak();    // Resolving ambiguity by specifying Bird
    }
};

int main() {
    Bat bat;
    bat.sound();  // Calls speak() from both Mammal and Bird
    return 0;
}
```

### Output:
```
Animal speaking
Animal speaking
```

### Explanation:
- **Diamond Problem**: The `Bat` class inherits from both `Mammal` and `Bird`, which both inherit from `Animal`. As a result, `Bat` ends up with two copies of the `Animal` class and its `speak()` method.
- **Ambiguity**: If we try to call `speak()` directly inside `Bat`, the compiler will not know whether to call the version inherited from `Mammal` or the one inherited from `Bird`.
- **Resolving Ambiguity**: To resolve this ambiguity, we explicitly specify which path of inheritance we want to use by calling `Mammal::speak()` or `Bird::speak()`.

## Solving the Diamond Problem with Virtual Inheritance
C++ provides a mechanism called virtual inheritance to solve the diamond problem. With virtual inheritance, the derived class will only inherit one copy of the common base class, avoiding ambiguity and redundancy.

### Virtual Inheritance Example
```cpp
#include <iostream>
using namespace std;

// Common base class with virtual inheritance
class Animal {
public:
    void speak() {
        cout << "Animal speaking" << endl;
    }
};

// Derived class 1 with virtual inheritance
class Mammal : virtual public Animal {};

// Derived class 2 with virtual inheritance
class Bird : virtual public Animal {};

// Derived class with multiple inheritance and virtual inheritance
class Bat : public Mammal, public Bird {
public:
    void sound() {
        speak();  // No ambiguity, only one copy of Animal
    }
};

int main() {
    Bat bat;
    bat.sound();  // Calls speak() from the single instance of Animal
    return 0;
}
```

### Output:
```
Animal speaking
```

### Explanation:
- **Virtual Inheritance**: By declaring the inheritance from `Animal` as virtual, we ensure that only one instance of `Animal` is shared among all classes that inherit from it.
- **No Ambiguity**: The `Bat` class no longer has two copies of `Animal`. Instead, it has a single, shared instance, and there is no ambiguity when calling `speak()`.



# Constructors and Destructors in Multiple Inheritance (C++)

In multiple inheritance, where a derived class inherits from more than one base class, the constructors and destructors of all the base classes and the derived class are called in a specific order. Understanding the order in which constructors and destructors are invoked is crucial for managing the initialization and cleanup of objects.

## Constructor Call in Multiple Inheritance
When a derived class inherits from multiple base classes:

- Base Class Constructors are called first, in the order in which the base classes are inherited.
- Derived Class Constructor is called after the base class constructors have completed.

### Constructor Call Order:
- The constructors of the base classes are called in the order they are listed in the class definition.
- If base class constructors take arguments, you can pass those arguments using a constructor initialization list in the derived class.

### Example: Constructor Call in Multiple Inheritance
Let’s consider an example where a derived class `Bat` inherits from two base classes, `Mammal` and `Bird`, and each class has a constructor.

```cpp
#include <iostream>
using namespace std;

// Base class 1
class Mammal {
public:
    Mammal() {
        cout << "Mammal constructor called." << endl;
    }
};

// Base class 2
class Bird {
public:
    Bird() {
        cout << "Bird constructor called." << endl;
    }
};

// Derived class with multiple inheritance
class Bat : public Mammal, public Bird {
public:
    Bat() {
        cout << "Bat constructor called." << endl;
    }
};

int main() {
    Bat bat;  // Creating an object of the derived class
    return 0;
}
```

### Output:
```
Mammal constructor called.
Bird constructor called.
Bat constructor called.
```

### Explanation:
#### Order of Base Class Constructor Calls:
- The `Mammal` constructor is called first because `Mammal` is listed first in the inheritance list (`Bat : public Mammal, public Bird`).
- The `Bird` constructor is called second.

#### Derived Class Constructor:
- After the base class constructors are executed, the constructor of the derived class `Bat` is called.

#### Order:
- Base Class Constructors are called before the Derived Class Constructor.

## Passing Arguments to Base Class Constructors
If the base classes have parameterized constructors, the derived class needs to pass arguments to them using the constructor initialization list.

### Example: Passing Arguments to Base Class Constructors

```cpp
#include <iostream>
using namespace std;

// Base class 1
class Mammal {
public:
    Mammal(string name) {
        cout << "Mammal constructor called for " << name << endl;
    }
};

// Base class 2
class Bird {
public:
    Bird(string name) {
        cout << "Bird constructor called for " << name << endl;
    }
};

// Derived class with multiple inheritance
class Bat : public Mammal, public Bird {
public:
    // Constructor initialization list to pass arguments to base class constructors
    Bat(string mammalName, string birdName) : Mammal(mammalName), Bird(birdName) {
        cout << "Bat constructor called." << endl;
    }
};

int main() {
    Bat bat("BatMammal", "BatBird");
    return 0;
}
```

### Output:
```
Mammal constructor called for BatMammal
Bird constructor called for BatBird
Bat constructor called.
```

### Explanation:
#### Parameterized Constructors in Base Classes:
- The `Mammal` and `Bird` classes both have constructors that take a string argument.

#### Constructor Initialization List:
- In the `Bat` constructor, the base class constructors are invoked using the constructor initialization list (`: Mammal(mammalName), Bird(birdName)`), passing the arguments `"BatMammal"` and `"BatBird"`.

#### Order of Execution:
- The `Mammal` constructor is called first, followed by the `Bird` constructor, and finally the `Bat` constructor.

## Destructor Call in Multiple Inheritance
The order of destructor calls in multiple inheritance is the reverse of the constructor call order. This means:

- Derived Class Destructor is called first.
- Base Class Destructors are called in the reverse order of their construction.

### Destructor Call Order:
- Destructors are invoked in the reverse order of the constructors, ensuring that the derived class is cleaned up before the base classes.

### Example: Destructor Call in Multiple Inheritance
Let’s modify our previous example to include destructors and observe their order of execution.

```cpp
#include <iostream>
using namespace std;

// Base class 1
class Mammal {
public:
    Mammal() {
        cout << "Mammal constructor called." << endl;
    }
    ~Mammal() {
        cout << "Mammal destructor called." << endl;
    }
};

// Base class 2
class Bird {
public:
    Bird() {
        cout << "Bird constructor called." << endl;
    }
    ~Bird() {
        cout << "Bird destructor called." << endl;
    }
};

// Derived class with multiple inheritance
class Bat : public Mammal, public Bird {
public:
    Bat() {
        cout << "Bat constructor called." << endl;
    }
    ~Bat() {
        cout << "Bat destructor called." << endl;
    }
};

int main() {
    Bat bat;  // Creating an object of the derived class
    // Destructors will be called automatically when the object goes out of scope
    return 0;
}
```

### Output:
```
Mammal constructor called.
Bird constructor called.
Bat constructor called.
Bat destructor called.
Bird destructor called.
Mammal destructor called.
```

### Explanation:
#### Constructor Call Order:
- The constructors are called in the following order:
    - `Mammal` constructor (base class 1)
    - `Bird` constructor (base class 2)
    - `Bat` constructor (derived class)

#### Destructor Call Order:
- The destructors are called in the reverse order of the constructors:
    - `Bat` destructor (derived class)
    - `Bird` destructor (base class 2)
    - `Mammal` destructor (base class 1)

#### Why Reverse Order?:
- Destructors are called in reverse order to ensure that the derived class resources are released before the base class resources. This avoids potential access to invalidated base class resources in the derived class during destruction.

## Virtual Inheritance and Constructors/Destructors
In the case of virtual inheritance, the constructor of the virtual base class is called only once, regardless of how many times it appears in the inheritance hierarchy. This ensures that only one instance of the virtual base class is created, preventing ambiguity and duplication.

### Example: Virtual Inheritance Constructor Call

```cpp
#include <iostream>
using namespace std;

// Virtual base class
class Animal {
public:
    Animal() {
        cout << "Animal constructor called." << endl;
    }
    ~Animal() {
        cout << "Animal destructor called." << endl;
    }
};

// Derived class 1 with virtual inheritance
class Mammal : virtual public Animal {
public:
    Mammal() {
        cout << "Mammal constructor called." << endl;
    }
    ~Mammal() {
        cout << "Mammal destructor called." << endl;
    }
};

// Derived class 2 with virtual inheritance
class Bird : virtual public Animal {
public:
    Bird() {
        cout << "Bird constructor called." << endl;
    }
    ~Bird() {
        cout << "Bird destructor called." << endl;
    }
};

// Derived class with multiple and virtual inheritance
class Bat : public Mammal, public Bird {
public:
    Bat() {
        cout << "Bat constructor called." << endl;
    }
    ~Bat() {
        cout << "Bat destructor called." << endl;
    }
};

int main() {
    Bat bat;  // Creating an object of the derived class
    return 0;
}
```

### Output:
```
Animal constructor called.
Mammal constructor called.
Bird constructor called.
Bat constructor called.
Bat destructor called.
Bird destructor called.
Mammal destructor called.
Animal destructor called.
```

### Explanation:
#### Virtual Inheritance Constructor Call:
- The `Animal` constructor is called only once, even though `Mammal` and `Bird` both inherit from `Animal` virtually.

#### Order of Constructor Calls:
- The constructor call order is as follows:
    - `Animal` (virtual base class)
    - `Mammal`
    - `Bird`
    - `Bat`

#### Order of Destructor Calls:
- Destructors are called in the reverse order:
    - `Bat`
    - `Bird`
    - `Mammal`
    - `Animal`




# Virtual Inheritance in C++

Virtual inheritance is a feature in C++ that helps resolve issues arising from multiple inheritance, specifically addressing the diamond problem. It ensures that a single instance of a common base class is shared across all derived classes, rather than duplicating base class instances, thus avoiding ambiguity and redundancy.

## The Diamond Problem

The diamond problem occurs when a class inherits from two classes that both inherit from the same base class. This creates a "diamond-shaped" inheritance structure, where the derived class ends up with two copies of the common base class, leading to ambiguity and redundancy.

### Diamond Problem Example:

```cpp
#include <iostream>
using namespace std;

// Base class
class Animal {
public:
    void speak() {
        cout << "Animal speaking" << endl;
    }
};

// Derived class 1
class Mammal : public Animal {};

// Derived class 2
class Bird : public Animal {};

// Derived class that inherits from both Mammal and Bird
class Bat : public Mammal, public Bird {};

int main() {
    Bat bat;
    // bat.speak();  // Error: Ambiguous, two copies of Animal in Bat
    return 0;
}
```

### Explanation:

- **Diamond Structure**: The `Bat` class inherits from both `Mammal` and `Bird`, which in turn both inherit from `Animal`. As a result, `Bat` contains two separate instances of `Animal`, one from `Mammal` and one from `Bird`.
- **Ambiguity**: If we try to call `bat.speak()`, the compiler doesn’t know which `Animal::speak()` function to call (the one from `Mammal` or the one from `Bird`).

## Solving the Diamond Problem with Virtual Inheritance

To solve this issue, we use virtual inheritance. With virtual inheritance, only one instance of the common base class (`Animal` in this case) is shared among all the derived classes, eliminating ambiguity.

### Syntax of Virtual Inheritance:

In virtual inheritance, you declare the base class as virtual during inheritance:

```cpp
class Derived : virtual public BaseClass {};
```

This ensures that only one instance of `BaseClass` is inherited, even if it appears multiple times in the inheritance hierarchy.

### Example: Virtual Inheritance to Solve the Diamond Problem

```cpp
#include <iostream>
using namespace std;

// Base class with virtual inheritance
class Animal {
public:
    Animal() {
        cout << "Animal constructor called." << endl;
    }
    void speak() {
        cout << "Animal speaking" << endl;
    }
};

// Derived class 1 with virtual inheritance
class Mammal : virtual public Animal {
public:
    Mammal() {
        cout << "Mammal constructor called." << endl;
    }
};

// Derived class 2 with virtual inheritance
class Bird : virtual public Animal {
public:
    Bird() {
        cout << "Bird constructor called." << endl;
    }
};

// Derived class that inherits from both Mammal and Bird
class Bat : public Mammal, public Bird {
public:
    Bat() {
        cout << "Bat constructor called." << endl;
    }
};

int main() {
    Bat bat;
    bat.speak();  // No ambiguity, only one instance of Animal
    return 0;
}
```

### Output:

```bash
Animal constructor called.
Mammal constructor called.
Bird constructor called.
Bat constructor called.
Animal speaking
```

### Explanation:

- **Virtual Inheritance**: Both `Mammal` and `Bird` inherit from `Animal` virtually (`virtual public Animal`). This ensures that only one instance of `Animal` is shared between `Mammal` and `Bird`.
- **Single Animal Instance**: When we create a `Bat` object, only one instance of the `Animal` class is created, even though `Bat` inherits from both `Mammal` and `Bird`.
- **No Ambiguity**: Since there is only one instance of `Animal`, the call to `bat.speak()` is unambiguous, and it calls the single `Animal::speak()` function.

## Constructor Call Order in Virtual Inheritance

In the case of virtual inheritance, the constructor of the virtual base class (`Animal` in this example) is called by the most derived class (`Bat`), not by the intermediate classes (`Mammal` and `Bird`). This ensures that the virtual base class is constructed only once.

### Example: Constructor Call Order with Virtual Inheritance

```cpp
#include <iostream>
using namespace std;

// Virtual base class
class Animal {
public:
    Animal() {
        cout << "Animal constructor called." << endl;
    }
    ~Animal() {
        cout << "Animal destructor called." << endl;
    }
};

// Derived class 1 with virtual inheritance
class Mammal : virtual public Animal {
public:
    Mammal() {
        cout << "Mammal constructor called." << endl;
    }
    ~Mammal() {
        cout << "Mammal destructor called." << endl;
    }
};

// Derived class 2 with virtual inheritance
class Bird : virtual public Animal {
public:
    Bird() {
        cout << "Bird constructor called." << endl;
    }
    ~Bird() {
        cout << "Bird destructor called." << endl;
    }
};

// Derived class that inherits from both Mammal and Bird
class Bat : public Mammal, public Bird {
public:
    Bat() {
        cout << "Bat constructor called." << endl;
    }
    ~Bat() {
        cout << "Bat destructor called." << endl;
    }
};

int main() {
    Bat bat;  // Creating an object of the derived class
    return 0;
}
```

### Output:

```bash
Animal constructor called.
Mammal constructor called.
Bird constructor called.
Bat constructor called.
Bat destructor called.
Bird destructor called.
Mammal destructor called.
Animal destructor called.
```

### Explanation:

- **Animal Constructor**: The `Animal` constructor is called only once, even though `Bat` inherits from both `Mammal` and `Bird`, which both virtually inherit from `Animal`. This is because `Animal` is a virtual base class, so only one instance is created.

### Order of Constructor Calls:

1. The `Animal` constructor is called first.
2. Then, the constructors of the intermediate classes (`Mammal` and `Bird`) are called.
3. Finally, the constructor of the most derived class (`Bat`) is called.

### Order of Destructor Calls:

Destructors are called in the reverse order of constructors: `Bat` destructor, `Bird` destructor, `Mammal` destructor, and finally the `Animal` destructor.



# Assignment Operator Overloading and Copy Constructor in Multiple Inheritance

In C++ multiple inheritance, when a derived class inherits from multiple base classes, you may need to define a custom copy constructor and assignment operator to handle the copying of objects correctly. This ensures that each part of the derived object (including the base class parts) is copied or assigned properly.

## Key Concepts:

### Copy Constructor:
A copy constructor is used to create a new object as a copy of an existing object. In multiple inheritance, the copy constructor needs to copy both the base class members and the derived class members.

### Assignment Operator:
The assignment operator (=) is used to copy the contents of one object to another existing object. In multiple inheritance, you may need to define an overloaded assignment operator to correctly assign base class and derived class members.

## Example: Copy Constructor and Assignment Operator in Multiple Inheritance
Let’s write a simple example where a class `Bat` inherits from two base classes, `Mammal` and `Bird`. We'll define the copy constructor and the assignment operator for `Bat` to handle the copying and assignment of objects correctly.

```cpp
#include <iostream>
using namespace std;

// Base class 1
class Mammal {
protected:
    int age;
public:
    Mammal(int a = 0) : age(a) {
        cout << "Mammal constructor called." << endl;
    }
    Mammal(const Mammal& other) : age(other.age) {
        cout << "Mammal copy constructor called." << endl;
    }
    Mammal& operator=(const Mammal& other) {
        if (this != &other) {
            age = other.age;
            cout << "Mammal assignment operator called." << endl;
        }
        return *this;
    }
};

// Base class 2
class Bird {
protected:
    int wingspan;
public:
    Bird(int w = 0) : wingspan(w) {
        cout << "Bird constructor called." << endl;
    }
    Bird(const Bird& other) : wingspan(other.wingspan) {
        cout << "Bird copy constructor called." << endl;
    }
    Bird& operator=(const Bird& other) {
        if (this != &other) {
            wingspan = other.wingspan;
            cout << "Bird assignment operator called." << endl;
        }
        return *this;
    }
};

// Derived class that inherits from both Mammal and Bird
class Bat : public Mammal, public Bird {
private:
    int flightSpeed;
public:
    Bat(int a, int w, int speed) : Mammal(a), Bird(w), flightSpeed(speed) {
        cout << "Bat constructor called." << endl;
    }
    
    // Copy constructor for Bat
    Bat(const Bat& other) : Mammal(other), Bird(other), flightSpeed(other.flightSpeed) {
        cout << "Bat copy constructor called." << endl;
    }
    
    // Assignment operator for Bat
    Bat& operator=(const Bat& other) {
        if (this != &other) {
            Mammal::operator=(other);  // Assign Mammal part
            Bird::operator=(other);    // Assign Bird part
            flightSpeed = other.flightSpeed;
            cout << "Bat assignment operator called." << endl;
        }
        return *this;
    }

    void display() const {
        cout << "Age: " << age << ", Wingspan: " << wingspan << ", Flight Speed: " << flightSpeed << endl;
    }
};

int main() {
    Bat bat1(5, 10, 20);  // Create bat1
    Bat bat2 = bat1;      // Copy constructor called
    bat2.display();

    Bat bat3(2, 4, 6);    // Create bat3
    bat3 = bat1;          // Assignment operator called
    bat3.display();

    return 0;
}
```

## Output:
```
Mammal constructor called.
Bird constructor called.
Bat constructor called.
Mammal copy constructor called.
Bird copy constructor called.
Bat copy constructor called.
Age: 5, Wingspan: 10, Flight Speed: 20
Mammal constructor called.
Bird constructor called.
Bat constructor called.
Mammal assignment operator called.
Bird assignment operator called.
Bat assignment operator called.
Age: 5, Wingspan: 10, Flight Speed: 20
```

## Explanation of the Code:

### Base Classes (Mammal and Bird):
- Each base class has a parameterized constructor, a copy constructor, and an overloaded assignment operator.
- `Mammal` has a data member `age`, and `Bird` has a data member `wingspan`.
- The copy constructors in both base classes copy the respective members from another object, while the assignment operators handle the assignment of members.

### Derived Class (Bat):
- The `Bat` class inherits from both `Mammal` and `Bird`. It has an additional member `flightSpeed`.
- The constructor initializes the base class parts (`Mammal(a)` and `Bird(w)`) and the derived class-specific member `flightSpeed`.
- The copy constructor of `Bat` calls the copy constructors of `Mammal` and `Bird` to copy their respective members. It also copies `flightSpeed` from the other `Bat` object.
- The assignment operator for `Bat` explicitly calls the assignment operators of `Mammal` and `Bird` to assign their members, and then assigns `flightSpeed`.

## Dry Run of the Code:

### Step 1: Creating `bat1`
```cpp
Bat bat1(5, 10, 20);
```
- `Mammal` constructor is called with `a = 5`, initializing `age` to 5.
- `Bird` constructor is called with `w = 10`, initializing `wingspan` to 10.
- `Bat` constructor is called, initializing `flightSpeed` to 20.

### Step 2: Copying `bat1` to `bat2` (Copy Constructor)
```cpp
Bat bat2 = bat1;
```
- `Mammal` copy constructor is called to copy `age` from `bat1` to `bat2`.
- `Bird` copy constructor is called to copy `wingspan` from `bat1` to `bat2`.
- `Bat` copy constructor is called to copy `flightSpeed` from `bat1` to `bat2`.

### Step 3: Displaying `bat2`
```cpp
bat2.display();
```
- The `display()` function outputs: `Age: 5, Wingspan: 10, Flight Speed: 20`.

### Step 4: Creating `bat3`
```cpp
Bat bat3(2, 4, 6);
```
- `Mammal` constructor is called with `a = 2`, initializing `age` to 2.
- `Bird` constructor is called with `w = 4`, initializing `wingspan` to 4.
- `Bat` constructor is called, initializing `flightSpeed` to 6.

### Step 5: Assigning `bat1` to `bat3` (Assignment Operator)
```cpp
bat3 = bat1;
```
- `Mammal` assignment operator is called to assign `age` from `bat1` to `bat3`.
- `Bird` assignment operator is called to assign `wingspan` from `bat1` to `bat3`.
- `Bat` assignment operator is called to assign `flightSpeed` from `bat1` to `bat3`.

### Step 6: Displaying `bat3`
```cpp
bat3.display();
```
- The `display()` function outputs: `Age: 5, Wingspan: 10, Flight Speed: 20`.
