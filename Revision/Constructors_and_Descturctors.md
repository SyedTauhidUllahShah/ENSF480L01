
# Constructors in C++

A constructor is a special member function of a class that is automatically called when an object of that class is created. The main purpose of a constructor is to initialize the object’s data members.

## 1. Key Characteristics of Constructors:
- The constructor name must be the same as the class name.
- No return type (not even void).
- Called automatically when an object is created.
- Can be overloaded to accept different parameters.

## 2. Types of Constructors:

### Default Constructor:
A constructor that takes no arguments and is called automatically when an object is created without arguments.

```cpp
class MyClass {
public:
    MyClass() {
        // Constructor body
        cout << "Default constructor called!" << endl;
    }
};

int main() {
    MyClass obj;  // Calls the default constructor
    return 0;
}
```

### Parameterized Constructor:
A constructor that accepts arguments to initialize the object with specific values.

```cpp
class MyClass {
private:
    int value;
public:
    // Parameterized constructor
    MyClass(int v) {
        value = v;
        cout << "Parameterized constructor called with value: " << value << endl;
    }
};

int main() {
    MyClass obj(100);  // Calls the parameterized constructor
    return 0;
}
```

### Copy Constructor:
A constructor that creates a new object as a copy of an existing object.

```cpp
class MyClass {
private:
    int value;
public:
    MyClass(int v) : value(v) {}

    // Copy constructor
    MyClass(const MyClass &obj) {
        value = obj.value;
        cout << "Copy constructor called!" << endl;
    }
};

int main() {
    MyClass obj1(50);       // Calls parameterized constructor
    MyClass obj2 = obj1;    // Calls copy constructor
    return 0;
}
```

## 3. Constructor Overloading:
C++ allows multiple constructors with different parameter lists. This is known as constructor overloading.

```cpp
class MyClass {
public:
    // Default constructor
    MyClass() {
        cout << "Default constructor called!" << endl;
    }

    // Parameterized constructor
    MyClass(int v) {
        cout << "Parameterized constructor called with value: " << v << endl;
    }
};

int main() {
    MyClass obj1;        // Calls default constructor
    MyClass obj2(10);    // Calls parameterized constructor
    return 0;
}
```

# Destructors in C++

A destructor is a special member function that is automatically called when an object goes out of scope or is explicitly destroyed. Its primary function is to perform cleanup operations, such as releasing memory or closing file handles.

## 1. Key Characteristics of Destructors:
- The destructor name is the same as the class name, but preceded by a tilde (`~`).
- It takes no arguments and does not return anything.
- Called automatically when an object goes out of scope or is deleted.
- There can be only one destructor in a class (it cannot be overloaded).

## 2. Example of a Destructor:

```cpp
class MyClass {
public:
    // Constructor
    MyClass() {
        cout << "Constructor called!" << endl;
    }

    // Destructor
    ~MyClass() {
        cout << "Destructor called!" << endl;
    }
};

int main() {
    MyClass obj;  // Constructor is called here
    // Destructor is called automatically when obj goes out of scope at the end of main()
    return 0;
}
```

## 3. Destructor in Dynamic Memory Allocation:
Destructors are especially useful when you allocate memory dynamically. Without proper cleanup in the destructor, you may face memory leaks.

```cpp
class MyClass {
private:
    int *ptr;
public:
    // Constructor
    MyClass(int size) {
        ptr = new int[size];  // Dynamically allocating memory
        cout << "Memory allocated in constructor!" << endl;
    }

    // Destructor
    ~MyClass() {
        delete[] ptr;  // Releasing the allocated memory
        cout << "Memory released in destructor!" << endl;
    }
};

int main() {
    MyClass obj(5);  // Constructor is called
    // Destructor is automatically called at the end of main()
    return 0;
}
```

# Key Differences Between Constructors and Destructors

| **Constructor**                        | **Destructor**                           |
|----------------------------------------|------------------------------------------|
| Used to initialize objects.            | Used to clean up before an object is destroyed. |
| Can be overloaded.                     | Cannot be overloaded (only one destructor per class). |
| Automatically called when an object is created. | Automatically called when an object goes out of scope or is deleted. |
| Can take arguments.                    | Takes no arguments.                      |
| Can have multiple constructors in a class (constructor overloading). | Only one destructor per class.           |
| Responsible for allocating resources (e.g., memory). | Responsible for deallocating resources (e.g., releasing memory). |
