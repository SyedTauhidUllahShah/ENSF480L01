
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

### Para