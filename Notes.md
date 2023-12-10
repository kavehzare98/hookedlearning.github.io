# Exceptions & STL (Chapter 16)

Tags: CS 1B, Programming
UID: 202312100612
Created: December 9, 2023 10:12 PM
Last Edited: December 10, 2023 2:41 PM

## 16.1 - Exceptions

```cpp
#include <regex>

double divide(int numerator, int denominator) {
    if (denominator == 0)
        throw "ERROR: Cannot divide by zero.'\n"; // throw point, control 
	        // is passed to another part called exception handler
    else
        return static_cast<double>(numerator) / denominator;
}

// Handling an exception - try/catch (general structure)

try    // try block
{
    // code here calls functions or object member functions that might
		// throw an exception.
}
catch (ExceptionParameter)  // catch block
{
    // code here handles the exception
}
// Repeat as many catch blocks as needed.
```

### Program 16-1

```cpp
// This program demonstrates an exception being thrown and caught.
#include <iostream>
#include <string>

using namespace std;

// Function prototype
double divide(int, int);

int main() {
    int num1, num2;
    double quotient;

    cout << "Enter two numbers: ";
    cin >> num1 >> num2;

    // Divide num1 by num2 and catch any
    // potential exceptions.
    **try {
        quotient = divide(num1, num2);
        cout << "The quotient is " << quotient << endl;
    }
    catch (string exceptionString) {
        cout << exceptionString;
    }**

    cout << "End of the program.\n";
    return 0;
}.  // end of main()

// divide() function definition
double divide(int num, int denom) {
    if (denom == 0) {
        string exceptionString = "ERROR: Cannot divide by zero.\n";
        **throw exceptionString;**
    }

    return static_cast<double>(num) / denom;
}   // end of divide()
```

### What if an exception is not caught?

1. try/catch construct contains no catch blocks with an exception parameter of the right data type.
2. The exception is thrown from outside a try block.

### Program 16-5

       Rectangle class Specification file                                Rectangle class Implementation file

```cpp
#ifndef RECTANGLE_H
#define RECTANGLE_H

class Rectangle {
private:
    double width;
    double length;
public:
    **// Exception class for width**
    class NegativeWidth {
    private:
        double value;
    public:
        NegativeWidth(double val) {
            value = val;
        }
        double getValue() const {
            return value;
        }
    };
		**// Exception class for length**
    class NegativeLength {
    private:
        double value;
    public:
        NegativeLength(double value) {
            this->value = value;
        }
        double getValue() const {
            return value;
        }
    };

    // Default constructor
    Rectangle();
    // Setters
    void setWidth(double);
    void setLength(double);
    // Getters
    double getWidth() const;
    double getLength() const;
    double getArea() const;
};

#endif //RECTANGLE_H
```

```cpp
#include "Rectangle.h"
Rectangle::Rectangle() {
    width = 0.0;
    length = 0.0;
}

void Rectangle::setWidth(double w) {
    if (w >= 0) {
        width = w;
    }
    else
        throw NegativeWidth(w);
}

void Rectangle:: setLength(double l) {
    if (l >= 0) {
        length = l;
    }
    else {
        throw NegativeLength(l);
    }
}

double Rectangle::getWidth() const {
    return width;
}

double Rectangle::getLength() const {
    return length;
}

double Rectangle::getArea() const {
    return width * length;
}
```

Client file

```cpp
// This program handles the Rectangle class exceptions.
#include <iostream>
#include "Rectangle.h"

using namespace std;

int main() {
    double width;
    double length;
    **bool tryAgain = true;**

    Rectangle myRectangle;

    cout << "Enter the rectangle's width:\t";
    cin >> width;
    cout << "Enter the rectangle's length:\t";
    cin >> length;

    // Store these values in the Rectangle object.
    **while (tryAgain)** {
        try {
            myRectangle.setWidth(width);
            tryAgain = false;
        }
        catch (**Rectangle::NegativeWidth e**) {
            cout << "ERROR: " << **e.getValue()** << " is an invalid value for WIDTH! Please enter a nonnegative value:\t";
            cin >> width;
        }
    }

    tryAgain = true;
    while (tryAgain) {
        try {
            myRectangle.setLength(length);
            tryAgain = false;
        }
        catch (**Rectangle::NegativeLength e**) {
            cout << "ERROR: " << **e.getValue()** << " is an invalid value for LENGTH! Please enter a nonnegative value:\t";
            cin >> length;
        }
    }

    cout << "The area of the rectangle is " << myRectangle.getArea() << endl;

    cout << "End of the program." << endl;

    return 0;
}   // end of main()
```
