---
layout: default
title: Pointers
permalink: /notes/programming/pointers/
---

<p class="section-label"><a href="{{ '/notebooks/programming/' | relative_url }}">Programming</a> / Note</p>

# Pointers

**Part - 1**

We have seen variable types so far which can hold integers (int), floating point numbers (float) and so on. So, what if we wanted a special variable or just some variable that could hold the address of a variable? Such a variable is called a pointer variable or simply a pointer.

Before we proceed, we should talk about the address operator (&). It basically returns the hexadecimal address of the variable it is used with.

```cpp
int length = 10;
float breadth = 20.5;
cout<< &length; //this will print address (memory location) of 
                //the variable length
```

Now, let’s declare pointers to store the addresses of the variables’ length and breadth.

```cpp
int* ptr_len; 
float *ptr_breadth;
```

Both the above syntax are valid i.e we can place asterisk(*) next to either the data type or the pointer name. Here, both the pointers currently hold some garbage value because we have not initialized them. int* implies that the pointer is pointing to an integer value.

```cpp
ptr_len = &length;
ptr_breadth = &bredth;
```

Now how do we use these pointers to access the values stored in length and breadth variables?

```cpp
cout<< *ptr_len; //prints 10
cout<< *ptr_breadth; //print 20.5
```

Here, the `*` before the pointer is dereferencing operator and is used to fetch the value of the variable it is pointing to.

```cpp
//both will print the same result (address of them)
cout<< &length;  
cout<< &(*ptr_len);
```

Basically, *ptr_len and length are the same variables. For now, it will be helpful to consider these two as the same as far as methods of value access and modification are concerned except when using pointers, one should use parenthesis to avoid ambiguity.

| Expression | Meaning |
| --- | --- |
| `length++;` | `(*ptr_len)++;` // Increments the value |
| | *Note: ptr_len++ Increments the pointer, not the value* |
| `length = 45;` | `*ptr_len = 45;` |

**Pointer to the Void**

We saw in previous cases that at the time of declaring the pointer, we had to specify the type of the variable it is pointing to. In situations where we don’t know the type of the variable apriori, void pointers come in handy because they can hold an address of any type and can be typecasted to any type.

```cpp
int a = 10;
char b = 'x';
void* p = &a; // void pointer holds address of int 'a'
p = &b; // void pointer holds address of char 'b'
```

But at the time of dereferencing the void pointer, we will have to do explicit casting.

```cpp
int x = 10;
void* ptr = &x;
std::cout << *ptr;  // Error!
std::cout << *static_cast<int*>(ptr);  // Correct
```

Void pointers are not type-safe, meaning the compiler cannot check if the correct data type is being used. This can lead to errors if not handled carefully. Modern C++ practices prefer templates for type safety in generic code.

References: Object-Oriented Programming in C++ by Robert Lafore.
