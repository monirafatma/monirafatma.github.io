---
layout: default
title: Memory Management: new and delete
permalink: /notes/programming/memory-management-new-delete/
---

# Memory Management: new and delete

Let’s say we wish to take the user’s name as input and store it somewhere in C++. Now, the name can be of any size and we don’t exactly know the length of input string apriori. How do you then declare a data structure with variable size?

```cpp
int size;
std::cin>> size;  //take size from user
int arr[size]; //ERROR; array size must be a constant
```

This is where dynamic memory allocation comes into the picture where memory size is determined **at runtime**.

### new Operator

```cpp
int n;
std::cin >> n;  // Get array size from user input
// Dynamically allocate memory for 'n' integers using 'new'
int* arr = new int[n];  //NO ERROR!
```

Here, the **new** operator is used to obtain a block of memory of size n of type int and returns the pointer to the starting point of the array of size n.

The new operator is pretty versatile and can be used to allocate objects dynamically and not only raw memory.

```cpp
class Distance {
private:
    int feet;
    int inches;

public:
     // Default constructor
    Distance() : feet(0), inches(0) {
    }
    // Constructor
    Distance(int f, int i) : feet(f), inches(i) {
    }
    // Display function
    void display() const {
        std::cout << feet << " feet, " << inches << " inches" << std::endl;
    }
    // Destructor
    ~Distance() {
    }
 };

int main() {
    // Using 'new' to dynamically allocate a Distance object
    Distance* dist1 = new Distance(5, 8);  // Constructor is called here
    dist1->display();

    return 0;
}
```

Here, it dynamically allocates memory on the **heap** for a Distance object and initializes it by calling the constructor with arguments (5,8). But what if we simply create an object the usual way-

```cpp
Distance obj(5, 8);  // Create a stack object
Distance* dist1 = &obj;  // Point to the existing object
```

Here, the memory is allocated on the stack and will get deallocated when variables go out of scope.

**Further read:** Difference in memory allocation on heap and stack.

### delete Operator

So far we saw that dynamic memory allocation happens on the heap and unlike stack, we need to explicitly delete the allocated memory. Not doing so will cause **memory leaks**, where allocated memory remains inaccessible and unused, consuming system resources. Let’s consider a case where we use **new** inside a function and that function uses a local variable pointer to this memory. Once the function terminates, this local variable pointer will get deleted but the memory will be left as an orphan. Using delete explicitly frees up this memory to be used later.

```cpp
// Function to dynamically allocate a Distance object
void createDistanceObject(int feet, int inches ) {
    // Dynamically allocate memory for a Distance object
    Distance* distPtr = new Distance(feet, inches);  // Allocates memory on the heap
    distPtr->display();  // Use the object

    // Free the allocated memory
    delete distPtr;  // Calls the destructor and releases the memory
    distPtr = nullptr;  // Avoid dangling pointer
}
```

Once we come out of the scope of the function createDistanceObject(), the pointer distPtr will get destroyed but while we are inside the function, once we have deleted the memory it points to, it will become a dangling pointer that points to something entirely different and if one ends up using it later within the function, it might lead to undefined behaviour so it is best to assign it to nullptr.

While deleting array of objects, one needs to be careful to call the delete[] operator and not delete.

```cpp
int n;
std::cin >> n;
int* arr = new int[n];  
delete[] arr;
```

The brackets ensure that all the members of the array are deleted and that the destructor is called for each of them.

| **Creating single Object** | **Creating an Array of Objects** |
| --- | --- |
| Distance* distPtr = new Distance(); | Distance* distPtrArr = new Distance[10]; |
| delete distPtr; | delete[] distPtrArr; |

Here, Distance* distPtrArr = new Distance[10] dynamically allocates an array of **10** Distance objects on the **heap** using the new[] operator. It calls for the Distance() constructor 10 times. Since we are not providing any arguments to the objects in the array, the **default constructor** is invoked for each object. delete [] operator calls for the destructor (~Distance()) 10 times to delete each object individually.

As a rule of thumb, new and delete operators should be called in pairs, for each new operator, there must be a call to delete operator.

References: Object Oriented Programming in C++ by Robert Lafore
