
# Operator Overloading in C++

Operator Overloading is a feature in C++ that allows you to redefine the way operators (such as +, -, *, etc.) work with user-defined types like classes or structs. This can make your classes more intuitive to use, allowing objects to interact with operators just like built-in types (like int or double).

Operators can be overloaded as member functions or non-member (friend) functions, and the choice depends on the type and number of operands involved. To fully understand this, we need to explore both single-argument (unary) and double-argument (binary) operator overloading.

---

## Single-Argument Operators (Unary Operators)
Unary operators work on a single operand. Common unary operators include +, -, ++, --, !, etc.

### Example: Overloading the Unary `-` Operator
Let’s create a class `Number` and overload the unary `-` operator to negate its value.

```cpp
#include <iostream>
using namespace std;

class Number {
private:
    int value;
public:
    // Constructor to initialize value
    Number(int val = 0) : value(val) {}

    // Overloading unary - operator
    Number operator - () const {
        return Number(-value);
    }

    // Display function
    void display() const {
        cout << "Value: " << value << endl;
    }
};

int main() {
    Number n1(10);      // Creating object with value 10
    Number n2 = -n1;    // Overloading unary - operator

    n1.display();  // Output: 10
    n2.display();  // Output: -10

    return 0;
}
```

### Code Explanation:
- **Constructor**: Initializes the value of the `Number` object.
- **Unary Operator Overloading**: The function `Number operator-() const` overloads the `-` operator. It negates the value and returns a new `Number` object.
- **Single Argument**: The single operand is implicitly the object that invokes the operator (n1 in this case).
- `Number n2 = -n1;`: Invokes the overloaded `-` operator, negating the value of `n1` and storing the result in `n2`.

---

## Double-Argument Operators (Binary Operators)
Binary operators work with two operands. Common binary operators include +, -, *, /, ==, !=, etc.

### Example: Overloading the Binary `+` Operator
Let’s extend the `Number` class and overload the binary `+` operator to add two `Number` objects.

```cpp
#include <iostream>
using namespace std;

class Number {
private:
    int value;
public:
    // Constructor to initialize value
    Number(int val = 0) : value(val) {}

    // Overloading binary + operator
    Number operator + (const Number &obj) const {
        return Number(this->value + obj.value);
    }

    // Display function
    void display() const {
        cout << "Value: " << value << endl;
    }
};

int main() {
    Number n1(10), n2(20);   // Creating two objects
    Number n3 = n1 + n2;     // Overloading binary + operator

    n1.display();  // Output: 10
    n2.display();  // Output: 20
    n3.display();  // Output: 30

    return 0;
}
```

### Code Explanation:
- **Binary Operator Overloading**: The function `Number operator+(const Number &obj) const` overloads the `+` operator.
- The left-hand operand is the object that invokes the operator (`n1`), and the right-hand operand is passed as an argument (`n2`).
- The function adds the value of both objects and returns a new `Number` object with the result.

---

## Operator Overloading Using Friend Functions
Some operators (such as `<<`, `>>`, and some binary operators like `+` when you want non-member access) require a friend function. A friend function is declared inside the class but defined outside of it. Friend functions can access private members of the class.

### Example: Overloading the `<<` Operator (Stream Insertion)
```cpp
#include <iostream>
using namespace std;

class Number {
private:
    int value;
public:
    // Constructor to initialize value
    Number(int val = 0) : value(val) {}

    // Friend function to overload << operator
    friend ostream& operator << (ostream &out, const Number &obj);

    // Friend function to overload >> operator
    friend istream& operator >> (istream &in, Number &obj);
};

// Overloading << operator (stream insertion)
ostream& operator<<(ostream &out, const Number &obj) {
    out << "Value: " << obj.value;
    return out;
}

// Overloading >> operator (stream extraction)
istream& operator>>(istream &in, Number &obj) {
    cout << "Enter value: ";
    in >> obj.value;
    return in;
}

int main() {
    Number n;
    cin >> n;           // Overloaded >> operator to input value
    cout << n << endl;  // Overloaded << operator to print value

    return 0;
}
```

---

## Overloading Increment (++) and Decrement (--) Operators
The increment (`++`) and decrement (`--`) operators are examples of unary operators. They can be overloaded in two forms:

- **Prefix Form**: When the operator is placed before the operand (e.g., `++n`).
- **Postfix Form**: When the operator is placed after the operand (e.g., `n++`).
The postfix version takes an `int` parameter to differentiate it from the prefix version.

### Example: Overloading `++` (Prefix and Postfix)
```cpp
#include <iostream>
using namespace std;

class Number {
private:
    int value;
public:
    // Constructor to initialize value
    Number(int val = 0) : value(val) {}

    // Prefix increment
    Number& operator++() {
        ++value;
        return *this;
    }

    // Postfix increment
    Number operator++(int) {
        Number temp = *this;  // Save the original value
        value++;
        return temp;          // Return the old value
    }

    // Display function
    void display() const {
        cout << "Value: " << value << endl;
    }
};

int main() {
    Number n(5);

    ++n;        // Calls prefix ++
    n.display();  // Output: 6

    n++;        // Calls postfix ++
    n.display();  // Output: 7

    return 0;
}
```

---

## Overloading Comparison Operators (==, !=, <, >, etc.)
Comparison operators are binary operators that return a boolean value (true or false). These operators can also be overloaded to compare objects of user-defined types.

### Example: Overloading `==` (Equality)
```cpp
#include <iostream>
using namespace std;

class Number {
private:
    int value;
public:
    // Constructor to initialize value
    Number(int val = 0) : value(val) {}

    // Overloading == operator
    bool operator == (const Number &obj) const {
        return value == obj.value;
    }
};

int main() {
    Number n1(10), n2(10), n3(20);

    if (n1 == n2) {  // Calls the overloaded == operator
        cout << "n1 and n2 are equal." << endl;  // Output: n1 and n2 are equal.
    }

    if (n1 != n3) {  // Overload != similarly
        cout << "n1 and n3 are not equal." << endl;  // Output: n1 and n3 are not equal.
    }

    return 0;
}
```

---

## Overloading Assignment Operator (=)
The assignment operator (`=`) must also be overloaded when a class involves dynamic memory allocation to handle deep copying correctly.

### Example: Overloading `=`
```cpp
#include <iostream>
using namespace std;

class Number {
private:
    int *value;
public:
    // Constructor
    Number(int val = 0) {
        value = new int(val);
    }

    // Overloading the assignment operator
    Number& operator=(const Number &obj) {
        if (this != &obj) {  // Self-assignment check
            delete value;  // Free old memory
            value = new int(*(obj.value));  // Deep copy
        }
        return *this;
    }

    // Display function
    void display() const {
        cout << "Value: " << *value << endl;
    }

    // Destructor
    ~Number() {
        delete value;
    }
};

int main() {
    Number n1(10), n2(20);
    n2 = n1;  // Calls the overloaded = operator

    n1.display();  // Output: 10
    n2.display();  // Output: 10

    return 0;
}
```

---

## Conclusion
Operator overloading in C++ allows you to customize the behavior of operators for your user-defined types, making them behave more like built-in types. This enhances the readability and usability of your classes. Key concepts include:

- **Unary Operators**: Work with a single operand and can be overloaded as member functions.
- **Binary Operators**: Work with two operands and can be overloaded either as member functions or friend functions.
