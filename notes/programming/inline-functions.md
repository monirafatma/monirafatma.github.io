---
layout: default
title: Inline Functions
permalink: /notes/programming/inline-functions/
---

<p class="section-label"><a href="{{ '/notebooks/programming/' | relative_url }}">Programming</a> / Note</p>

# Inline Functions

For normal functions, the definition and declaration are written in some .hpp and .cpp files respectively and whenever in code we need to call the function, we import the header file and call the function in our source code. When we call the function, the control transfers to the function (.cpp file), executes and then returns the result.

All this gives us a compact, modular code but at the expense of increased execution time. A normal function call initializes and maintains a call stack, increasing the computational overhead.

For small functions where the function body does not require too many lines of code, we can declare the function within the source body itself using the keyword **inline** so that when called, the execution time is saved. When we call an **Inline** function, the compiler replaces the function call in the code with the actual function body so the execution time is saved. The **inline** keyword is a request to the compiler that the function be not treated as a normal function but rather as a special case. Let’s see an example.

```cpp
inline float km_to_m(int km) {
    return km * 1000;
}

int main() {
    std::cout << km_to_m(10);
}
```

At the time of compilation, in the main(), the function call km_to_m(10) is replaced by 10*100. Now let’s see what happens when we don’t use the word inline.

```cpp
float km_to_m(int km) {
    return km * 1000;
}

int main() {
    std::cout << km_to_m(10);
}
```

Here, in the main() at the time of compilation, km_to_m(10) jumps to the function definition, executes it and returns the result.

It is a good practice to declare and define the inline function in the header file is to avoid “unresolved external” errors from the linker. That error will occur if we put the inline function’s definition in a cpp file and if that function is called from some other cpp file. [[1]](https://isocpp.org/wiki/faq/inline-functions)

The compiler can sometimes ignore the **inline** request when the function is long. Other times, the compiler can automatically make a function inline if the function is small.

Further read: Can virtual functions be inlined? I hope to cover this some day but for the interested people, there are a bunch of discussions available already. [[2]](https://www.geeksforgeeks.org/inline-virtual-function/) [[3]](https://stackoverflow.com/questions/733737/are-inline-virtual-functions-really-a-non-sense)

Reference: Object Oriented Programming in C++ by Robert Lafore.
