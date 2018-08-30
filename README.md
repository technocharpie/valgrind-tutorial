# Valgrind Basics

Simple tutorial for Valgrind: a memory mismanagement detector.


## Installation

Linux: `sudo apt install valgrind`

Brew: `brew install valgrind`

## Valgrind?

Valgrind is a command-line program that by passing an executable file (as well as flags, and/or any parameters the executable might need), it spits out a "receipt" of sorts, with the status of the Heap after execution, detailing possible memory leaks.

## How-To-Run

After compilation, and with the executable file of your program at hand, one must only pass the executable to valgrind as a parameter as such:

`valgrind ./my_exec`

This will produce the most basic output from valgrind. Of course, things can get a little more verbose...

`valgrind --leak-check=full --verbose ./my_exec param1 param2`

What this now produces is the full leak-check summary, as per the `leak-check` flag, as well as more amount of detail about the program's memory usage through the `verbose` flag. What follows the executable file are possible parameters the program might need, though these are respective to the program, and are not a necessity.

### Compilation sidenote

Before running the executable file through valgrind, it might be wise to pass a debugging flag when compiling. For example, passing the `ggdb3` 
