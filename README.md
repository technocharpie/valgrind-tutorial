# Valgrind Basics

Simple tutorial for Valgrind: a memory mismanagement detector.


## Installation

Linux: `sudo apt install valgrind`

Brew: `brew install valgrind`

## Valgrind?

Valgrind is a command-line program that by passing an executable file (as well as flags, and/or any parameters the executable might need), it spits out a "receipt" of sorts, with the status of the Heap after execution, detailing memory leaks.

## How-To-Run

After compilation, and with the executable file of your program at hand, one must only pass the executable to valgrind as a parameter as such:

`valgrind ./my_exec`

This will produce the most basic output from valgrind. Of course, things can get a little more verbose...

`valgrind --leak-check=full --verbose ./my_exec param1 param2`

What this now produces is the full leak-check summary, as per the `leak-check` flag, as well as more amount of detail about the program's memory usage through the `verbose` flag. What follows the executable file are possible parameters the program might need, though these are respective to the program, and are not a necessity.

### Compilation sidenote

Before running the executable file through valgrind, it might be wise to pass a debugging flag when compiling. For example, passing the `ggdb3` flag to __g++__ compiler will yield line numbers at which the leak(s) can possibly be originating from. 

`g++ -ggdb3 -o my_exec file1.cpp`

## Interpreting Results (with example)

Below is a quick snippet of __c++__ code that creates a singly-linked list, adds the values 0-9, then prints them out. It does not, however, appropriately free the memory allocated for the list object. 

```c++
#include <iostream>
#include "sll.h"

int main()
{
	sll* MyList = new sll();

	for (int i = 0; i < 10; ++i)
		MyList->addNewNode(i);

	MyList->printList();

	//delete MyList;

	return 0;
}
```

Here is the valgrind "receipt" that was obtained by compiling with the `ggdb3` flag, and then passing the `leak-check=full` flag to valgrind:

```
==22147== Memcheck, a memory error detector
==22147== Copyright (C) 2002-2017, and GNU GPL'd, by Julian Seward et al.
==22147== Using Valgrind-3.13.0 and LibVEX; rerun with -h for copyright info
==22147== Command: ./my_exec
==22147== 
0
1
2
3
4
5
6
7
8
9
==22147== 
==22147== HEAP SUMMARY:
==22147==     in use at exit: 168 bytes in 11 blocks
==22147==   total heap usage: 13 allocs, 2 frees, 73,896 bytes allocated
==22147== 
==22147== 168 (8 direct, 160 indirect) bytes in 1 blocks are definitely lost in loss record 3 of 3
==22147==    at 0x4C3017F: operator new(unsigned long) (in /usr/lib/valgrind/vgpreload_memcheck-amd64-linux.so)
==22147==    by 0x108A3E: main (test3.cpp:6)
==22147== 
==22147== LEAK SUMMARY:
==22147==    definitely lost: 8 bytes in 1 blocks
==22147==    indirectly lost: 160 bytes in 10 blocks
==22147==      possibly lost: 0 bytes in 0 blocks
==22147==    still reachable: 0 bytes in 0 blocks
==22147==         suppressed: 0 bytes in 0 blocks
==22147== 
==22147== For counts of detected and suppressed errors, rerun with: -v
==22147== ERROR SUMMARY: 1 errors from 1 contexts (suppressed: 0 from 0)
```

As we can see, some leaks occurred as we never freed the memory we allocated through the use of __new__.

```
==22147== Memcheck, a memory error detector
==22147== Copyright (C) 2002-2017, and GNU GPL'd, by Julian Seward et al.
==22147== Using Valgrind-3.13.0 and LibVEX; rerun with -h for copyright info
==22147== Command: ./my_exec
==22147== 
0
1
2
3
4
5
6
7
8
9
==22147==
```
This portion of the "receipt" has basic information about valgrind, followed by any print outs the program might have had. In this case, we called the the __printList()__ function that printed out the contents of the list to the screen.

```
==22147== HEAP SUMMARY:
==22147==     in use at exit: 168 bytes in 11 blocks
==22147==   total heap usage: 13 allocs, 2 frees, 73,896 bytes allocated
```
This __printList()__ function that printed out the contents of the list to the screen.
