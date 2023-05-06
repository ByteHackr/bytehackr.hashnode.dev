---
title: "5 Way to Prevent Out of Bounds Write"
datePublished: Sat May 06 2023 05:13:28 GMT+0000 (Coordinated Universal Time)
cuid: clhbj4rfb000209mq3gtx25qt
slug: 5-way-to-prevent-out-of-bounds-write
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1683349908216/1b8ce0b8-8f5d-4b57-9fe0-a02871ded9d6.png
tags: cpp, programming-blogs, security, learning, coding

---

## Understanding Out-of-Bounds Write in C++

Out of Bounds Write(CWE-787) is a type of programming error that occurs when a program writes data beyond the bounds of an allocated memory buffer. This can happen when a program tries to write data to an index that is outside the range of indices allocated for that buffer. Let's take a look at an example in C++:

```cpp
int arr[5] = {1, 2, 3, 4, 5}; // allocate an array of size 5
arr[6] = 6; // out of bounds write!
```

In this example, the program attempts to write to the 7th element of the `arr` array, which is beyond the bounds of the allocated memory. This can cause serious issues for the program, including crashes, data corruption, and even security vulnerabilities.

Lets Deep Drive into another Example:

```cpp
void foo(char* buffer, int size) {
    buffer[size] = '\0';
}

int main() {
    char* buffer = new char[10];
    foo(buffer, 15);
    delete[] buffer;
    return 0;
}
```

In this code, we allocate a buffer of size 10 using the `new` operator and pass it to the `foo` function along with a size of 15. Inside the `foo` function, we attempt to write a null terminator character at the `size` index of the `buffer`. However, the allocated buffer only has a size of 10, so the write is Out of Bounds.

The consequences of this Out of Bounds Write error can be severe. In this case, the buffer overflow can corrupt adjacent memory, leading to unpredictable behavior or even program crashes. This can be particularly dangerous if the adjacent memory belongs to other variables or data structures that contain sensitive information.

Furthermore, an attacker can potentially exploit this vulnerability by crafting input that triggers the Out of Bounds Write error. For example, an attacker can send a specially crafted input to a web application that causes it to write data beyond the bounds of a buffer. This can enable the attacker to execute arbitrary code, steal sensitive data, or launch further attacks.

To prevent Out of Bounds Write errors, developers can take several steps, such as using bound checking functions, avoiding unsafe functions, and validating input values, we will discuss these in later sections. In this case, one way to prevent the Out of Bounds Write error would be to modify the `foo` function to check if the `size` parameter is within the bounds of the allocated buffer before writing to it:

```cpp
void foo(char* buffer, int size) {
    if (size >= 0 && size < 10) {
        buffer[size] = '\0';
    }
}
```

Alternatively, we could use C++'s standard library to ensure that the access is within bounds:

```cpp
#include <string>

void foo(char* buffer, int size) {
    std::string str(buffer, size);
}
```

In this case, we create a string object from the buffer and size, which automatically handles bounds checking and null-termination.

## **Detect Out-of-Bounds** Write **in C++**

Detecting Out of Bounds Write errors is an important step in ensuring the stability and security of a program. Here are some ways to detect these errors:

1. Test the program with different inputs: Developers can test their programs with different input values to check for Out of Bounds Write errors. For example, if a program expects an array of size 5, the developer can test it with arrays of different sizes to see if any Out of Bounds Write errors occur.
    
2. Use debuggers: Debuggers can be used to track the execution of a program and identify any Out of Bounds Write errors that occur. Developers can use breakpoints to pause the program at specific points and examine the contents of memory buffers.
    
3. Use memory analysis tools: Memory analysis tools can be used to detect Out of Bounds Write errors by examining the memory usage of a program. These tools can detect when a program writes data beyond the bounds of an allocated buffer.
    
4. Use static analysis tools: Static analysis tools can analyze the source code of a program and detect potential Out of Bounds Write errors. These tools can detect errors that might not be apparent during testing or debugging.
    
5. Enable runtime checks: C++ provides runtime checks that can detect Out of Bounds Write errors. For example, the `-fsanitize=address` flag can be used with GCC and Clang to enable runtime checks for memory errors, including Out of Bounds Write errors.
    

By using these techniques, developers can detect Out of Bounds Write errors and fix them before they cause issues for their programs.

## **Preventing Out-of-Bounds** Write **in C++**

Out of Bounds Write vulnerabilities are dangerous as they can lead to memory corruption, exploitation by attackers, denial of service attacks, and data leakage. These vulnerabilities can be prevented by following secure coding guidelines and best practices in programming.

1. ### Use Bound Checking Functions
    

One of the easiest ways to prevent Out of Bounds Write errors is to use bound checking functions. These functions check whether a program is attempting to write outside of the bounds of an allocated memory buffer. Examples of such functions include `memcpy_s`, `strncpy_s`, and `sprintf_s`. These functions take additional parameters that specify the size of the destination buffer, preventing the program from writing beyond its bounds.

```cpp
char src[] = "Hello, world!";
char dst[10];

// Using strncpy_s to prevent Out of Bounds Write
strncpy_s(dst, sizeof(dst), src, sizeof(dst) - 1);
```

In this example, we have a source string `src` with the value "Hello, world!" and a destination character array `dst` with a size of 10. To prevent Out of Bounds Write, we use `strncpy_s` to copy the source string to the destination array, specifying the size of the destination buffer as the second parameter.

1. ### Use Language Features
    

Some programming languages provide built-in features to prevent Out of Bounds Write errors. For example, C++ provides the `std::vector` and `std::array` containers, which automatically manage the memory allocation and resizing of arrays. These containers provide bounds checking and exception handling, making it more difficult to write outside of their bounds.

```cpp
#include <vector>

// Using std::vector to prevent Out of Bounds Write
std::vector<int> v(10);

// Accessing elements within the bounds of the vector
for (int i = 0; i < v.size(); i++) {
  v[i] = i;
}
```

In this example, we use the `std::vector` container to store an array of integers with a size of 10. Since `std::vector` manages the memory allocation and resizing of the array, we don't have to worry about Out of Bounds Write errors. We can safely access and modify the elements within the bounds of the vector using the subscript operator `[]`.

1. ### Avoid Unsafe Functions
    

Certain functions, such as `strcpy` and `sprintf`, do not perform bounds checking and can lead to Out of Bounds Write errors. Developers should avoid using these functions whenever possible and opt for safer alternatives, such as `strncpy_s` and `snprintf`.

```cpp
char src[] = "Hello, world!";
char dst[10];

// Using snprintf to prevent Out of Bounds Write
snprintf(dst, sizeof(dst), "%s", src);
```

In this example, we have a source string `src` with the value "Hello, world!" and a destination character array `dst` with a size of 10. Instead of using `sprintf`, which can lead to Out of Bounds Write errors, we use `snprintf` to copy the source string to the destination array, specifying the size of the destination buffer as the second parameter.

1. ### Allocate Sufficient Memory
    

To prevent Out of Bounds Write errors, developers should allocate sufficient memory to hold the data that their program will write. For example, consider the following code:

```cpp
char* str = new char[5]; // allocate a buffer of size 5
strcpy(str, "Hello, World!"); // out of bounds write!
```

In this example, the buffer allocated for `str` is too small to hold the entire string "Hello, World!". To prevent this error, the developer should allocate a buffer of sufficient size:

```cpp
char* str = new char[14]; // allocate a buffer of size 14
strcpy(str, "Hello, World!"); // safer write
```

1. ### Use Static Analysis Tools
    

Static analysis tools can detect potential Out of Bounds Write errors before a program is executed. For example, consider the following code:

```cpp
char dest[5];
strcpy(dest, "Hello, World!"); // out of bounds write!
```

A static analysis tool, such as Clang or GCC, can detect the potential Out of Bounds Write error and provide feedback to the developer.

In conclusion, Out of Bounds Write errors can cause serious issues for C++ programs, including security vulnerabilities and crashes. Developers can prevent these errors by using bound checking functions, language features, avoiding unsafe functions, allocating sufficient memory, and using static analysis tools. By incorporating these techniques into their development process, developers can write more secure and stable C++ programs.