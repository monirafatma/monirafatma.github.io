---
layout: default
title: C++ Const Qualifier
permalink: /notes/programming/cpp-const-qualifier/
---

<p class="section-label"><a href="{{ '/notebooks/programming/' | relative_url }}">Programming</a> / Note</p>

# C++ Const Qualifier

When we declare a variable in C++ with **const**, it implies that its value will not change throughout the program. If we try to assign a new value to the variable, it will generate an error.

```cpp
const float  PI = 3.1415; //type const float
```

### const Function Arguments

When we pass some argument to a function and we want a guarantee that the function can not modify the variable, we can apply the **const** modifier to the variable in the function declaration to achieve the same.

```cpp
float scaleD( int a, const float multiplier); //declaration
```

Here, the function scaleD can only use the variable multiplier and can’t modify it. It will throw an error if we attempt to modify it within the function definition.

When we pass some variable in the function as a reference argument, the function operates on the **original variable**, and not a copy. In such cases, we generally do not wish the function to end up modifying the original variable. Here, passing the variable with a **const** modifier comes in handy.

```cpp
float scaleD( int a, const float& multiplier); //declaration
```

If a variable has been declared with a const qualifier and let’s say we need to pass that variable as a reference argument to some function, then that variable must be declared const in the function argument.

```cpp
const float multiplier =10;
float scaleD( int a, const float& multiplier); //correct declaration
float scaleD( int a, float& multiplier); //wrong declaration
```

On the other hand, When we pass a variable **by value**, the function gets a **copy** of the variable. Any changes to the parameter inside the function do not affect the original variable. Therefore, **const** is not necessary in this case.

### const Return Value

we can declare a function with a const modifier before it to ensure that the value it returns can not be modified.

```cpp
const int& getSquare(int& x) {
      static int result;
      result= x*x;
      return result;
}
```

Here, in the above example, the function returns a const reference which essentially means that the returned reference can not be used to modify the the value it refers to.

### const and Classes

So far we saw **const** being used on variables and function arguments. We will see that in the context of classes, it can be used on member functions, member variables, member function arguments and class objects. Let’s create a class **Distance** to understand better.

```cpp
class Distance {
private:
    const double miles;  // Constant member
    double kilometers;   // Non-constant member

public:
    // Constructor to initialize both miles and kilometers
    Distance(double m, double km) : miles(m), kilometers(km) {}

    // Const member function (can only access const members)
    double getMiles() const {
        return miles;  // Accessing const member variable 'miles'
    }
    // A function to get kilometers (non-const)
    double getKilometers() {
        return kilometers;  // Accessing non-const member variable 'kilometers'
    }
    // Non-const member function (can modify non-const members)
    void setKilometers(double km) {
        kilometers = km;  // Modifying non-const member variable 'kilometers'
    }    
    
};
```

Here, the member variable miles is const while kilometres is non-const.

Notice that the getter function for kilometre is without a **const** qualifier as the kilometre member is declared as non-const. But one can declare getKilometers() even with a const qualifier at the end like below and it will still compile successfully.

```cpp
double getKilometers() const {
        return kilometers;  // Accessing non-const member variable 'kilometers'
    }                       // but the function is const
```

Because,

* A **const** member function can be called by both **const** and non-const objects because it does not modify the object's state.
* A **non-const** member function, however, cannot be called on a **const** object because a **const** object can only call functions that preserve its immutability.

Let’s understand these concepts better by creating two Distance objects. One as a const and another as a non-const.

```cpp
int main() {
    Distance d(5.0, 8.0);  // Initializing with 5 miles and 8 kilometers
    // Now create a const object of Distance
    const Distance constD(10.0, 20.0);  // Const object with 10 miles and 20 kilometer

    // Calling non-const member function to modify kilometers
    d.setKilometers(12.0);

    return 0;
}
```

The above code will compile just fine because we have called a non-const function **setKilometers()** on a non-const object i.e. on **d**. While if we try to do the same with **constD**, we will get an error.

```cpp
constD.setKilometers(12.0);  //This will throw error.
```

### const and Pointers

* **Pointer to a constant value:** When the value being pointed to can not be changed.

```cpp
const int value = 10;
const int*ptr = &value;
*ptr = 20; //ERROR
```

* **Constant Pointer:** The pointer itself can not point to some other address but the value it points to, can be changed.

```cpp
int value1 = 10;
int value2= 20;
int* const ptr = &value;
*ptr = 20; //This is allowed
ptr = &value2; //ERROR
```

* **Constant Pointer to a constant value:** Neither the value nor the pointer can be changed.

```cpp
const int value = 10;
const int* const ptr = &value;
ptr = &newValue; //ERROR
*ptr = 20; //ERROR
```

Reference: Object Oriented Programming in C++ by Robert Lafore.
