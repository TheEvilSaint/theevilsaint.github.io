---
layout: post
title: "C Tutorial"
date: "2021-09-17"
description: ""
coverimage: C_tutorial.jpg
tags: c
published: true
posttype: article
categories: tutorial
---
# C
C is a general-purpose, procedural computer programming language, developed by
R&D company Bell Labs back in 1972. It's a widely used programming language,
most notably in systems programming for operating systems, compilers,
interpreters, libraries of other programming languages and in implementations
of embedded system applications. For example, Linux and Windows kernel are
written in C, most of GNU components and also Python, PHP, Ruby, Perl interpreters.

Before a C program can do anything, it first needs to be compiled,
i.e. translated from C source into a binary code which can instruct the CPU.

The C language provides no built-in facilities for performing common
operations such as input/output, memory management, string manipulation, and the others. Instead, these facilities are defined in a standard libraries, which you compile and link with your programs.

Information about standard and other libraries should be included inside the first lines of the code. The following line includes the standard input/output
library, where 'stdio.h' is the name of the library's header file.


```c
#include <stdio.h>
```

Now, some brief notes about the C program: it consists of functions and variables. A function contains statements that specify the computing
operations, and variables store values used during the computation. Communication between the functions is by arguments and values returned by the functions, and through external variables.
The return statement defines a value to be returned from the called function to its caller.

You can name your functions however you want, with the exception of the name
'main'. Function called 'main' is special - your program begins executing
at the beginning of the main. This means that every program must have a main
defined somewhere in code.


```c
#include <stdio.h>

int main() {
  printf("Goodbye, World!");
  return 0;
}
```
When we write `int main()` we are stating that our program will return and integer. The number returned by the function indicates whether the
program we wrote worked correctly. A return of 0 mean our programmed returned
successfully, a number greater than 0 means it failed. Every line in C must end
with a semicolon, so that the compiler knows that a new line has started.



## Variables


### Types

There are only a few basic data types in C:

 - ``char`` - single byte, capable of holding one character in the local character set.

 - ``int`` - integer, typically reflecting the natural size of integers on the host machine.

 - ``float`` - single-precision floating point.

 - ``double`` - double-precision floating point

In addition, there are a number of qualifiers that can be applied to these
basic types. ```short``` and ```long``` apply to integers:

```c
short int var_one;
long int var_two;
```

Note: 'int' can be omitted in these declarations.


### Defining variables

All variables must be declared before use, although certain declarations can
be made implicitly by content. A declaration specifies a type, and contains a
list of one or more variables of that type, as in

```c
int lower, upper, step;
char c, line[1000];
```

For whole numbers, int type is used, which an integer in the size of a "word"
the default number size of the machine which your program is compiled on. On
most computers today, it is a 32-bit number, which means the number can range
from -2,147,483,648 to 2,147,483,647.

```c
int a = 0,b = 1,c = 2,d = 3, e = 4;
a = b - c + d * e;
printf("%d", a); /* will print 1-2+3*4 = 11 */
```


Declaring Strings
```c
char name[] = "John Smith";
/* is the same as */
char name[11] = "John Smith";
```

The reason that we need to add one, although the string John Smith is exactly 10 characters long, is for the string termination, a special character (equal to 0) which indicates the end of the string. The end of the string is marked because the program does not know the length of the string - only the compiler knows it according to the code.

Using a pointer to define a simple string. This method creates a string which can only be used for reading
```c
char * name = "John Smith";
```

String formatting with `printf`
```c
char * name = "John Smith";
int age = 27;

/* prints out 'John Smith is 27 years old.' */
printf("%s is %d years old.\n", name, age)
```

String Length - The function `strlen` returns the length of the string which has to be passed as an argument:
```c
char * name = "Nikhil";
printf("%d\n",strlen(name));
```


## Operators

### Unary operators

Unary operators are placed as prefix relative to operands and operate on
single a operand. E.g. incrementing a 'var' variable with '++' operator.:
```c
++var;
```

#### Increment and decrement operators

** ++ ** - Increment operatror-
** -- ** - Decremet operator.

#### Address and indirection operators

** & ** - Address Operator. Yields the address of the operand.
** * ** - Indirection operator. Yields the object or function to which its operand points, also known as pointer.
** + ** - Unary Plus Operator. Yields the value of the operand, which must be of arithmetic type.
** - ** - Unary Minus Operator. Yields the negative value of the operand, which must be of arithmetic type.
** ~ ** - One's Complement Operator. Yields the complement value of the operand, which must be of integral type.
** ! ** - Logical Negation Operator. Yields 1 if the value of its operand compares equal to 0, and 0 otherwise.  
** sizeof ** -Sizeof Operator. Yields the number of bytes required to store an object of the type of its operand.

### Multiplicative operators
** * ** - Multiplication Operator. Yields the multiple of two operands, both of whcih must be of arithmetic type.
** / ** - Division Operator. Yields the divison of first by the second operand, both of which must be of arithmetic type.
** % ** Remainder Operator. Yields the remainder of divison of first operand by the second, both of which must be of integral type.

### Additive Operators
If the operands have arithmetic type, the usual arithmetic conversions are performed.

** + ** - Yields the sum of operands.
** - ** - Yields the value of the first operand subtracted by the value of second.

### Shift Operators

### Equality Operators

### Relational operators

### Bitwise AND Operator

### Bitwise Exclusive OR Operator

### Logical AND Operator

### Logical OR Operator

### Conditional Operator

### Assignment Operators

### Comma Operator


## Arrays
An 'array' type variable is a contiguously allocated nonempty set of objects,
alos called elements, with a particular member object type. Each object can
be accessed via subscript, also called index, number. In C, index number starts from 0 since it
references a memory location offset by ```n``` objects away from the first element.

Arrays are defined in a very straightforward syntax:

```c
int numbers[10];
```

The example above defines an array containing 11 objects, indexed from 0 to
10, of type 'int'.


We can assign and retrive as follows:

```c
//int numbers[10];
/* populate the array */
numbers[0] = 10;
numbers[1] = 20;
numbers[2] = 30;
numbers[3] = 40;
numbers[4] = 50;
numbers[5] = 60;
numbers[6] = 70;

/* print the 7th number from the array, which has an index of 6 */
printf("The 7th number in the array is %d", numbers[6]);
```

Here is the general form of a multidimensional array declaration:
```c
type arrayName [x][y];
```

Initialise a two dimensional array
```c
int a[3][4] = {
   {0, 1, 2, 3} ,   /*  initializers for row indexed by 0 */
   {4, 5, 6, 7} ,   /*  initializers for row indexed by 1 */
   {8, 9, 10, 11}   /*  initializers for row indexed by 2 */
};
```

The inside braces, which indicates the wanted row, are optional. The following initialization is the same to the previous example.
```c
int a[3][4] = {0,1,2,3,4,5,6,7,8,9,10,11};
```

Let's access a two-dimensional element
```c
int val = a[2][3];
```

Example program using multidimensional arrays
```c
     #include <stdio.h>

     int main() {
          int grades[2][5];
          float average;
          int i;
          int j;

          grades[0][0] = 80;
          grades[0][1] = 70;
          grades[0][2] = 65;
          grades[0][3] = 89;
          grades[0][4] = 90;

          grades[1][0] = 85;
          grades[1][1] = 80;
          grades[1][2] = 80;
          grades[1][3] = 82;
          grades[1][4] = 87;

          for (i = 0; i < 2; i++) {
               average = 0;

               for (j = 0; j < 5; j++) {
                    average += grades[i][j];
               }

               average /= 5.0;
               printf("The average marks obtained in subject %d is: %.2f\n", i, average);
          }

          return 0;
     }
```

## Selection (Conditional) statements
A selection statement is a flow of control mechanism which selects among a set of substatements depending on the value of a controlling expression. The first substatement whose conditional expression, which must be arithmetic or pointer type, is evaluated to be unequal to 0 is executed.

Selection statement can be in three forms:
```
//pseudocode
if (expression) statement
if (expression) statement else statement
switch (expression) statement
```

In the follwoing example:
```c
if ( var < 4 ) {
   printf( "var is less than 4." );
} else {
   printf( "var is larger than 4." );
}
```


## Loops

### For Loops


Using a for loop to iterate over a block 10 times.
```c
int i;
for (i = 0; i < 10; i++) {
    printf("%d\n", i);
}
```

For loops can iterate over arrays
```c
int array[10] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
int sum = 0;
int i;

for (i = 0; i < 10; i++) {
    sum += array[i];
}

/* sum now contains a[0] + a[1] + ... + a[9] */
printf("Sum of the array is %d\n", sum);
```

### While Loops

While loops are like for loops but have less functionality. A while loop continues executing the while block as long as the condition in the while holds true.
```c
int n = 0;
while (n < 10) {
    n++;
}
```

While loops can also execute infinitely if a condition is given which always evaluates as true (non-zero):
```c
while (1) {
   /* do something */
}
```

NOTE: the `break` and `continue` directives.

The break directive halts a loop after ten loops, even though the while loop never finishes:
```c
int n = 0;
while (1) {
    n++;
    if (n == 10) {
        break;
    }
}
```

In the following code, the continue directive causes the printf command to be skipped, so that only even numbers are printed out:
```c
int n = 0;
while (n < 10) {
    n++;

    /* check that n is odd */
    if (n % 2 == 1) {
        /* go back to the start of the while block */
        continue;
    }

    /* we reach this code only if n is even */
    printf("The number %d is even.\n", n);
}
```

## Functions

* Functions receive either a fixed or variable amount of arguments.
* Functions can only return one value, or return no value.

Example function definition
```c
int foo(int bar) {
    /* do something */
    return bar * 2;
}

int main() {
    foo(1);
}
```

In C, functions must be first defined before they are used in the code. They can be either declared first and then implemented later on using a header file or in the beginning of the C file, or they can be implemented in the order they are used (less preferable).

The correct way to use functions is as follows:
```c
/* function declaration */
int foo(int bar);

int main() {
    /* calling foo from main */
    printf("The value of foo is %d", foo(1));
}

int foo(int bar) {
    return bar + 1;
}
```

We can also create functions that do not return a value by using the keyword void:
```c
void moo() {
    /* do something and don't return a value */
}

int main() {
    moo();
}
```

## Static Variables, Refercences and Pointers

**What is a static variable?**

By default, variables are local to the scope in which they are defined. Variables can be declared as static to increase their scope up to file containing them. As a result, these variables can be accessed anywhere inside a file.

**References And Pointers**

A pointer is essentially a simple integer variable which holds a memory address that points to a value, instead of holding the actual value itself.

Dereferencing is the act of referring to where the pointer points at.
```c
#include <stdio.h>

int main() {
    /* define a local variable a */
    int a = 1;

    /* define a pointer variable, and point it to a using the & operator */
    int * pointer_to_a = &a;

    printf("The value a is %d\n", a);                          /* 1 */
    printf("The value of a is also %d\n", *pointer_to_a);      /* 1 */
    printf("The address of a is %d\n", pointer_to_a);          /*  713939084*/
    printf("The address of a is %d\n", &a);                    /*  713939084*/

    return 0;
}
```

## Structures

C structures are special, large variables which contain several named variables inside. Structures are the basic foundation for objects and classes in C. Structures are used for:
* Serialization of data
* Passing multiple arguments in and out of functions through a single argument
* Data structures such as linked lists, binary trees, and more

Defining a structure
```c
struct point {
    int x;
    int y;
}
```

Declaring a struct upfront and then building it on the fly
```c
struct point p;
p.x = 10;
p.y = 5;
draw(p);
```

Example of struct and typedef
```c
typedef struct {
    char * brand;
    int model;
} vehicle;

vehicle mycar;
mycar.brand = "Ford";
mycar.model = 2007;
```

## Function arguments by reference

Function arguments are passed by value, which means they are copied in and out of functions. But what if we copied pointers to values instead of the values themselves? This will enable us to give functions control over variables and structures of the parent functions, and not just a copy of them.

Because we pass in the memory location and reference it inside via a pointer, this example allows us to increment `n` outside of the function
```c
void addone(int * n) {
    (*n)++;
}

int n;
printf("Before: %d\n", n);
addone(&n);
printf("After: %d\n", n);
```

## Compiling

Compile C code.
```
gcc -o output.c input.c
```

Cross compile C code, compile 32 bit binary on 64 bit Linux.
```
gcc -m32 -o output.c input.c
```

Compile a binary for Windows on Linux
```
i686-w64-mingw32-gcc -o program.exe mycode.c
```

## Useful Snippets

An add windows user program

cat useradd.c
```c
/* system, NULL, EXIT_FAILURE */

#include <stdlib.h>

int main ()
{
int i;
i=system ("net localgroup administrators myuser /add");
return 0;
}
```

Now we compile this while on Linux
```
i686-w64-mingw32-gcc -o adduser.exe useradd.c
```