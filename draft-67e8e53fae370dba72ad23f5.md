---
title: "A Practical Guide to GDB: Debugging Like a Pro"
slug: a-practical-guide-to-gdb-debugging-like-a-pro

---

Debugging is an essential skill for any programmer, and the GNU Debugger (GDB) is one of the most powerful tools available for finding and fixing bugs in your code. In this guide, we'll explore GDB's capabilities through practical, hands-on examples that will help you become proficient with this indispensable tool.

## What is GDB?

GDB (GNU Debugger) is a portable debugger that runs on many Unix-like systems and works for various programming languages including C, C++, Fortran, and more. It allows you to:

* Start a program and control its execution
    
* Stop the program at specific points (breakpoints)
    
* Examine what's happening when the program is running
    
* Change things in the program to experiment with correcting bugs
    

## Getting Started with GDB

Let's begin with a simple example to demonstrate GDB basics. Consider this C program that has a bug:

```c
// buggy.c
#include <stdio.h>

int main() {
    int numbers[5] = {1, 2, 3, 4, 5};
    int i = 0;
    
    for (i = 0; i <= 5; i++) {  // Bug: array index out of bounds
        printf("numbers[%d] = %d\n", i, numbers[i]);
    }
    
    return 0;
}
```

Compile the program with debugging information:

```bash
gcc -g -o buggy buggy.c
```

The `-g` flag includes debugging information in the executable.

### Starting GDB

To start debugging:

```bash
gdb ./buggy
```

You'll see the GDB prompt:

```plaintext
GNU gdb (Ubuntu 9.2-0ubuntu1~20.04) 9.2
...
(gdb) 
```

## Essential GDB Commands

### Running the Program

To run the program:

```plaintext
(gdb) run
```

Our buggy program will likely crash with an error like:

```plaintext
numbers[0] = 1
numbers[1] = 2
numbers[2] = 3
numbers[3] = 4
numbers[4] = 5
numbers[5] = 32767  # Garbage value - accessing out of bounds

Program received signal SIGSEGV, Segmentation fault.
0x0000555555555149 in main () at buggy.c:8
8        printf("numbers[%d] = %d\n", i, numbers[i]);
```

### Setting Breakpoints

To investigate, let's set a breakpoint at line 8:

```plaintext
(gdb) break 8
Breakpoint 1 at 0x1145: file buggy.c, line 8.
```

Or by function name:

```plaintext
(gdb) break main
Breakpoint 2 at 0x1131: file buggy.c, line 6.
```

### Controlling Execution

Now restart and execute step by step:

```plaintext
(gdb) run
Starting program: /home/user/buggy

Breakpoint 2, main () at buggy.c:6
6       for (i = 0; i <= 5; i++) {
```

To continue to the next breakpoint:

```plaintext
(gdb) continue
Continuing.

Breakpoint 1, main () at buggy.c:8
8        printf("numbers[%d] = %d\n", i, numbers[i]);
```

To execute a single instruction:

```plaintext
(gdb) step
numbers[0] = 1
6       for (i = 0; i <= 5; i++) {
```

### Examining Variables

To see the value of a variable:

```plaintext
(gdb) print i
$1 = 0
```

To look at arrays:

```plaintext
(gdb) print numbers
$2 = {1, 2, 3, 4, 5}
```

To watch how a variable changes:

```plaintext
(gdb) watch i
Hardware watchpoint 3: i
```

### Finding the Bug

Let's find where our program accesses out-of-bounds:

```plaintext
(gdb) break 6 if i == 5
Breakpoint 4 at 0x1138: file buggy.c, line 6.

(gdb) continue
Continuing.

Breakpoint 4, main () at buggy.c:6
6       for (i = 0; i <= 5; i++) {

(gdb) print i
$3 = 5
```

Now we can see the problem - the loop condition is `i <= 5`, which allows `i` to reach 5, but our array `numbers` has indices 0-4. We need to change the condition to `i < 5`.

## Advanced GDB Features

### Backtrace

When your program crashes, use backtrace to see the call stack:

```plaintext
(gdb) backtrace
#0  main () at buggy.c:8
```

For more complex programs, this shows the function call sequence that led to the crash.

### Conditional Breakpoints

You can set breakpoints that only trigger under specific conditions:

```plaintext
(gdb) break 8 if i == 3
Breakpoint 5 at 0x1145: file buggy.c, line 8.
```

### Custom Display Formats

Display variables in different formats:

```plaintext
(gdb) print/x numbers[0]  # Hexadecimal
$4 = 0x1

(gdb) print/t numbers[0]  # Binary
$5 = 1
```

## Debugging a More Complex Example

Let's look at a slightly more complex program with a linked list:

```c
// list_bug.c
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int data;
    struct Node* next;
} Node;

Node* create_node(int data) {
    Node* new_node = (Node*)malloc(sizeof(Node));
    new_node->data = data;
    new_node->next = NULL;
    return new_node;
}

void print_list(Node* head) {
    Node* current = head;
    while (current != NULL) {
        printf("%d -> ", current->data);
        current = current->next;
    }
    printf("NULL\n");
}

// Bug: doesn't handle empty list case
void delete_node(Node** head, int value) {
    Node* current = *head;
    Node* previous = NULL;
    
    // The bug is here - doesn't check if current is NULL
    if (current->data == value) {
        *head = current->next;
        free(current);
        return;
    }
    
    while (current != NULL && current->data != value) {
        previous = current;
        current = current->next;
    }
    
    if (current == NULL) return;
    
    previous->next = current->next;
    free(current);
}

int main() {
    Node* list = NULL;  // Empty list
    
    // Try to delete from empty list - will crash
    delete_node(&list, 5);
    
    return 0;
}
```

Let's debug this program:

```bash
gcc -g -o list_bug list_bug.c
gdb ./list_bug
```

### Using GDB to Find Memory Errors

Set a breakpoint at the bug:

```plaintext
(gdb) break delete_node
Breakpoint 1 at 0x1201: file list_bug.c, line 26.
(gdb) run
```

Examine the pointer before dereferencing:

```plaintext
(gdb) print current
$1 = (Node *) 0x0
```

Now we can see that `current` is NULL, which will cause a segmentation fault when dereferenced.

### Fixing Problems in GDB

GDB allows you to modify variables during execution:

```plaintext
(gdb) set variable current = create_node(10)
```

While this won't fix the actual code, it demonstrates how you can experiment within GDB.

## Core File Analysis

When programs crash outside the debugger, they often generate core files. To analyze:

```bash
gdb ./your_program core
```

## GDB with Multi-threaded Programs

For multi-threaded programs, GDB provides special commands:

```plaintext
(gdb) info threads
(gdb) thread 2
(gdb) thread apply all backtrace
```

## Using GDB with C++

GDB works well with C++ and can handle:

* C++ function name demangling
    
* C++ templates
    
* Class inspection
    

Example for a C++ program:

```cpp
// cpp_example.cpp
#include <iostream>
#include <vector>

class Example {
public:
    void doSomething(int param) {
        std::vector<int> vec(param);
        // Bug: accessing out of bounds
        for (int i = 0; i <= param; i++) {
            vec[i] = i * 2;
        }
    }
};

int main() {
    Example ex;
    ex.doSomething(5);
    return 0;
}
```

To debug C++ classes:

```plaintext
(gdb) break Example::doSomething
(gdb) print vec
(gdb) print *this
```

## GDB TUI Mode (Text User Interface)

GDB offers a text-based UI that shows your code and current execution point:

```plaintext
(gdb) tui enable
```

Navigate with:

* `layout next` - Cycle through layouts
    
* `focus cmd` - Focus on command window
    
* `focus src` - Focus on source window
    

## Advanced Tips and Tricks

### Create a .gdbinit File

Create a `.gdbinit` file in your home directory to customize GDB:

```plaintext
set print pretty on
set print array on
set print array-indexes on
set history save on
set history filename ~/.gdb_history
```

### Debug Running Processes

Attach to an already running process:

```bash
gdb -p <PID>
```

### Create Custom Commands

Define your own GDB commands:

```plaintext
(gdb) define print_list_items
> set $node = head
> while $node
>   print $node->data
>   set $node = $node->next
> end
> end
```

## Conclusion

GDB is an incredibly powerful debugging tool that can save you countless hours of debugging frustration. By mastering these commands and techniques, you'll be able to solve even the most complex bugs efficiently.

Remember that effective debugging is not just about knowing the tools, but also about developing a methodical approach to problem-solving. Start with a hypothesis about what might be wrong, use GDB to gather evidence, and then refine your understanding until you find the root cause.

Happy debugging!