---
layout: default
title: References
permalink: /notes/programming/references/
---

<p class="section-label"><a href="{{ '/notebooks/programming/' | relative_url }}">Programming</a> / Note</p>

# References

**References** much like **pointers** allow us to pass objects to functions without making a copy of the object. This helps save memory and improves performance, especially when working with large objects (like containers or complex data structures). But unlike pointers, references allow for easier manipulation of variables.

A reference can be thought of as a constant pointer that is automatically dereferenced.

Once initialised, a reference cannot be made to refer to a different variable. A reference can not be null and hence must be initialized to a variable when declared. These two reasons combined make references safer than pointers.

**Syntax:**

`data_type& ref_name= variable;`

`data_type &ref_name = variable;`

Both above are valid syntaxes.

```cpp
int value=10;
int& ref = value; //valid
int &ref = value; //valid
```

Here, **int&** implies that the **ref_to_value** is a reference to an **int** value.

Reference **&** should not be confused with the address operator **&**.

```cpp
int value = 10;
int& ref_to_value = value; //reference syntax
int* address = &value; //address syntax
```

Here, **ref_to_value** is a reference to the value while the variable **address** stores the address of the value.

References are aliases to the variable they are bound to, i.e., they are not new variables but just another name for the variable.

When it comes to arithmetic operations, passing the variables as function arguments and accessing or modifying the value, we can use the reference just as the original variable (in terms of syntax); however, they differ in other regards.

* Variables can be initialized without value, ref can’t.

```cpp
int x; //VALID
int& ref; //ERROR
```

* Variable can be reassigned to other value but ref can’t.

```cpp
int x=10;
int y=20;
x= 20; //VALID
int& ref = x;
ref = y; //This is same as x = y and is valid 
ref = &y; //INVALID, CAN'T REBIND THE REF
```

* References do not have their own memory address separate from the variable they refer to.

```cpp
int x = 5;
int& ref = x;
std::cout << &x << " " << &ref;  // Prints the same address
```

### L-Values, R- Values and L-Value references

**l-values**

* values that have names and are addressable
* modifiable if they aren’t declared **const**

```cpp
int x ;
x = 100; // x is a l-value
100 = x; //ERROR as 100 is a literal and not an l-value
string name;
name = "Monira"; //name is a l-value
"Monira" = name; //ERROR, "Monira" is not a l-value
```

**r- values**

* non-addressable, non-assignable
* present on the right side of an assignment expression.
* a literal
* a temporary value created by the compiler, for example,

```cpp
int x{100};
int y{200};
y = x+y; //here x+y is a r-value
```

r- values can be assigned to l-values explicitly.

```cpp
int x ;
x = 100; // x is a l-value and 100 is r- value that has been assigned to 
         //x explicitly
```

**l-value references**

So far all the references we saw were l-value references.

```cpp
int x{100};
int& ref = x; //l-value referece i.e ref is a reference to l-value(i.e x)
int& new_ref = 100; //ERROR, 100 is a r-value 
```

Let’s see an example when we pass by refernce,

```cpp
int square(int& num){
return n*n;
}
int num {10};
square(num); //Okay because num is a l-value so it can substitute 
             //an l-value reference
square(10);// ERROR beacause can't reference an r-value 
```

At this point, one obvious question arises: Does C++ provide an r-value reference, and what purpose might they serve? This will be left for further reading and some online discussions around this topic can be found [here](https://www.geeksforgeeks.org/lvalues-references-and-rvalues-references-in-c-with-examples/).

Reference: Object-Oriented Programming in C++ by Robert Lafore, [Beginning C++ Programming -From Beginner to Beyond](https://www.udemy.com/course/beginning-c-plus-plus-programming).
