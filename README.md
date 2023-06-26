
# malloc

## Description

In this project, I created my own implementation of malloc and free. Which means that code needs to implement a library that interacts with the operating system to perform heap management on behalf of a user process. 

## Building and Running the Code

The code compiles into four shared libraries and six test programs. To build the code, change to your top level assignment directory and type:
```
make
```
Once you have the library, you can use it to override the existing malloc by using
LD_PRELOAD. The following example shows running the ffnf test using the First Fit shared library:
```
$ env LD_PRELOAD=lib/libmalloc-ff.so tests/ffnf
```

To run the other heap management schemes replace libmalloc-ff.so with the appropriate library:
```
Best-Fit: libmalloc-bf.so
First-Fit: libmalloc-ff.so
Next-Fit: libmalloc-nf.so
Worst-Fit: libmalloc-wf.so
```
## Program Features

1. Implemented splitting and coalescing of free blocks. If two free blocks are adjacent then
they will combined. If a free block is larger than the requested size then it splits the block into two.
2. Implemented three additional heap management strategies: Next Fit, Worst Fit, Best Fit, and First Fit.
3. Counters exist in the code for tracking of the following events:

* Number of times the user calls malloc successfully
* Number of times the user calls free successfully
* Number of times we reuse an existing block
* Number of times we request a new block
* Number of times we split a block
* Number of times we coalesce blocks
* Number blocks in free list
* Total amount of memory requested
* Maximum size of the heap

The code will print the statistics ( THESE STATS ARE FAKE) upon exit and should look like similar to:
```
mallocs: 8
frees: 8
reuses: 1
grows: 5
splits: 1
coalesces: 1
blocks: 5
requested: 7298
max heap: 4096
```


4. Eight test programs are provided to help debug your code. They are located in the tests
directory.
5. Implemented versions of realloc and calloc as well.


## Debugging
While running the tests, may encounter some segfaults and other memory errors. Because we are side-loading our own malloc implementation, we will need to do the following to debug a test application using gdb if memory error encountered.
```
$ gdb ./tests/ffnf
...
(gdb) set exec-wrapper env LD_PRELOAD=./lib/libmalloc-ff.so
(gdb) run
...
(gdb) where
```
Basically, you want to first load gdb with the test program that is crashing. Next, you need to tell gdb to utilize the appropriate malloc library by creating an exec-wrapper that loads it into memory. Next, simply run the program. Once it segfaults, you can print a stack trace by using the where command. From there, you can explore the state of your program and see if you can determine what went wrong.

