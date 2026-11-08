---
layout: default
title: How copying works in C++
permalink: /notes/programming/cpp-copying/
---

# How copying works in C++

In this section, we will go through the concepts of assignment operator, shallow copy, deep copy and copy constructors in C++. Though they all may seem to be simply making copies of some object, how they do it varies and carries some nuances.

### Assignment Operator

What happens when we are assigning some variable to another variable, will depend on the variable type. When dealing with primitive data types like int, float, and char, the assignment operator just copies values from one variable to another. Each variable remains independent and will have a different memory address. Changing one will not change the other.

```cpp
int a = 10;
int b = a; //only value  (10) gets copied.
cout<< &a ; //prints memory of a
cout<< &b; //prints memory of b (which is different than a)
```

When dealing with a user-defined object, we can copy in two different ways. Let’s say we have two Distance objects d1 and d2.

```cpp
Distance d1 (10,5);
Distance d2;  // default constructor called
d2 = d1;
```

What the assignment does here is that it copies the value of member variables of d1 to d2. d1 and d2 remain independent objects.

```cpp
Distance d1(10,5);
Distance d2 = d1;
```

Here, the object d2 is being created and initiliazed as d1 at the same time. Just as previous case, they remain as independent objects except for this time, the assignment operator calls for the copy constructor of the Distance class.

### Copy Constructor

Let’s say you have a user-defined object dist1 and you wish to create another object that has its member variables the same as dist1 i.e. we want each member variable of dist1 copied to dist2. This is where the copy constructor comes into use. This is where the copy constructor comes into use.

```cpp
//copy constructor
Distance(Distance& dist)
    {
        feet = dist.feet;
        inches= dist.inches;
    }
```

Copy constructors are always associated with a user-defined object regardless we declare them explicitly or not. In case when we don’t declare them explicitly, the compiler automatically generates a default copy constructor which performs a shallow copy of the object’s members. We will talk about shallow and deep copy later, just keep in mind that <mark>unless a copy constructor is explicitly declared and defined to perform deep copy, it will always perform a shallow copy.</mark>

```cpp
Distance d1(10,5);
Distance d2 = d1;  //invokes copy constructor
Distance d3(d1); //also invokes copy constructor
```

A bit more detailed read in copy constructors can be found here.

### Shallow Copy vs Deep Copy

As long as we are copying some object that doesn’t have pointers as members and doesn’t allocate memory dynamically, both shallow copy and deep copy are functionally the same. We will see why when the object has pointers or dynamic memory, they are different.

```cpp
class Distance {
public:
    int feet;
    int* inches;  // Pointer as member variable

    // Constructor
    Distance(int f, int i) {
        feet = f;
        inches = &i;  // Point to the local parameter 'i'
    }

    // Copy Constructor (shallow copy)
    Distance(const Distance& obj) {
        feet = obj.feet;
        inches = obj.inches;  // Both objects point to the same data
    }

    // Display function
    void show() const {
        cout << "Distance: " << feet << "' " << *inches << "\"" << endl;
    }
};
```

Here, in the copy constructor **inches = obj.inches;** basically copies the address of the variable ‘i’ and hence both the original and the copied object will point to the same memory location where i is located. If we change the value of i via one object, the change will be reflected in the copied object as well.

```cpp
int inchesValue = 8;
Distance d1(5, inchesValue);  // Create object with existing data
Distance d2 = d1;             // Invoke shallow copy constructor
// Modify 'inchesValue' directly through the 'd1' object
*d1.inches = 10;  //modify inchesValue throough d1
```

Here, changing d1.inches will change the d2.inches as well because they are pointing to the same memory location. This is shallow copy. If we wish to avoid this issue we will have to dynamically create a new memory location, copy the value of inches there and assign the pointer d2.inches to that new location. This way, only the value gets copied and both object remain independent of each other. This is exactly what the deep copy constructor does.

```cpp
// Deep Copy Constructor
Distance(const Distance& obj) {
    feet = obj.feet;
    inches = new int(*(obj.inches)); //Allocate new memory and copy the value
    }
```

So, as long as we are dealing with pointers as member variables in our class be it because we are dynamically allocating memory (**new** operator returns pointer) or we are simply storing pointer to an existing variable, we should be careful to define a deep copy constructor. Wrapping up,

* **Shallow copy**: Copies all the member values as is, including pointers. It doesn't allocate new memory for dynamically allocated data; instead, it copies the address, so both objects point to the same memory.

* **Deep copy**: Allocates new memory for dynamically allocated data and copies the values. This ensures that each object has its own separate memory.

References : [https://www.geeksforgeeks.org/copy-constructor-in-cpp/](https://www.geeksforgeeks.org/copy-constructor-in-cpp/)
