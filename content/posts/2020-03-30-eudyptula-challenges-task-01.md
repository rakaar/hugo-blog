---
date: "2020-03-30T00:00:00Z"
subtitle: Getting Started with Linux kernel learning
tags:
- kernel
- linux
- make
- module
title: Eudyptual challenges task 01
---


### Task 
The task briefly can be described as:-
> Write a linux kernel module that when loaded should print 'Hello' in the debug level of the linux. 
> You also need to write the makefile which builds the kernel module against the source file.


### Kernel Modules:-
Modules are pieces of code that can be loaded or unloaded as per requirement. 
As this task talks about inserting module, it is very dangerous to experiment on the kernel you are using. Hence it would be better to try the task in a Virtual machine.
Coming back to modules, to get the list of modules you can use the command `lsmod` . To get information about a specific module you can use `modinfo MODULE_NAME`

### Make
In linux, make is used to build executable programmes and libraries.
Makefile  is a file that contains a list of instructions to build programmes and libraries in a particular syntax (in the background it utilizes the make hence its called a makefile)

What is a build process:
- Compiler taking source code and converting to object files
- Linker taking the object files and converting them to executable

### Syntax of makefile
```
target: dependencies
<tab> system_command
```
For example 
```
all:
    g++ main.cpp hello.cpp factorial.cpp -o hello
```
all is the default target for a make file. if no other target is specified, it will execute this target

Example of target with no system commands(look at `all`),
```
all: hello

hello: a.o b.o c.o
    g++ a.0 b.o c.o
a.o: a.cpp
    g++ -c a.cpp
b.o: b.cpp
    g++ -c b.cpp
c.o: c.cpp
    g++ -c c.cpp
clean:
    rm -rf *.o
```
Here in case to execute all of them 
you can simple use make
to execute one specific target(useful in case when one file is changed)
```
make target name
```
another example to make things clear
```
all: hello

hello: a b c
	@echo "all"
a:
	@echo "a"
b: 
	@echo "b"
c: 
	@echo "c"
clean:
	rm -rf *.o
```
In the above example command make will print only "all" on the terminal
command `make a` will print only "a" on the terminal

using variable names in makefile
its as simple as assigning a variable and accessing using dollar notation
for example
```
CC = gcc
```
$(CC) to use gcc

### Kernel Debug level
There are totally 8 levels of log in linux, from level0 to level7. The level7 is the debug level.


### Task

The C code which accomplishes the task: hello_world_module.c 
```
#include <linux/module.h> // included for all kernel modules
#include <linux/kernel.h> // included for KERN_INFO
#include <linux/init.h>   // for the __init  and __exit macors

MODULE_LICENSE("GPL");
MODULE_AUTHOR("RKA");
MODULE_DESCRIPTION("Accomplish Task 1");

static int __init hello_init(void) {
    printk(KERN_DEBUG "Hello \n"); 
    // since the challenge asks to print in the debug level
    return 0;
    // Non zero return implies module couldn't load
}
static void __exit hello_cleanup(void) {
    printk(KERN_DEBUG "Clean module\n");
}

module_init(hello_init); // when module loads
module_exit(hello_cleanup); // when module unloadss
```
   
At the beginning the headers that required are included. Description in comments.
The next three lines, give an info about the module. The info about the module can be obtained from the command line using modinfo MODULE_NAME

There are 2 static functions on that runs when module is loaded, and the other when module is unloaded.

```
Static function is a function that has scope limited to its object file. The advantage of this is that we can use the same name at different files, without any clashes.
```

The last two lines indicate what functions to be called on loading and unloading of the module. We can also see that here printk is used instead of printf, because we want it to print in the kernel logs.  Also in  printk function defines the log level to which it wants to print. Here since in the task, it is asked to print at debug level, KERN_DEBUG is the first argument.

Now to build this module we need to write a makefile
here is its code:Makefile
```
obj-m += hello_world_module.o
all:
    make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules
clean:
     make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```

The first line is called the goal definition. It indicates what files need to be built
Run the makefile by typing make  in the terminal. We get a list of different files. We need the `.ko`  file. By loading a kernel module means we are inserting this `.ko` file in the kernel 

Now to insert the hello_world_module.c (To make it ready to convert into a .ko file)
```
sudo insmod hello_world_module.ko
```
Verify that you have inserted the module

You can do this by using the dmesg ,which print the kernel log
Also you can check the log file whose path is /var/log/kern.log

To remove the module, the comand `sudo rmmod  MODULE_NAME` can be used.
To verify that you have removed a module, you can check using the dmesg command or verify the kern.log file

**NOTE**: Suppose you are getting an error like 'Operation not permitted'.  It is due to the fact we are trying a module which is not signed. In generally modules are signed, which are like proof the module is a valid one. You can either manually sign the module by yourself or you can simply turn off the secure boot to add unsigned modules in your kernel. You can also see the kernel logs that there is a entry which says 'module verification failed'

### References:
- http://mrbook.org/blog/tutorials/make/
- https://opensource.com/article/18/8/what-how-makefile
- https://www.kernel.org/doc/Documentation/kbuild/makefiles.txtm