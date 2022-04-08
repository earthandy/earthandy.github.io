---
title: Control Logic How Expressions And Statements Coordinate Program Execution
date: 2022-03-18 20:52:30
keywords: C
categories: C
tags:
  - C
---


#### Expression

An expression is a grammatical structure consisting of a series of operators and operands. Generally speaking, the evaluation process of an expression is actually the process of recursively evaluating the expression and the sub-expressions it contains according to the precedence and associativity of the operators. From the point of view of compilation, the actual evaluation order of the operands involved in this process will be determined in the syntax analysis stage and reflected in the abstract syntax tree (AST, Abstract Syntax Tree) corresponding to the source code.

Expressions provide the ability to allow data to participate in different computations of multiple operators at the same time. By providing rules for operator precedence and associativity, expressions can control the orderly execution of the entire calculation process.

#### Statement

Statements are the basic building blocks used to describe programs. Unlike expressions, a statement is the largest granularity unit that makes up a C program. Inside it, it can contain simple or complex expression structures, but it can also contain nothing. In addition to that, the statement returns no results when executed, but may have side effects.

In the C language, statements can be divided into five types: compound statements, expression statements, selection statements, iteration statements, and jump statements. But regardless of the type, the statements must end with a semicolon and be executed sequentially from top to bottom.

Among them, a compound statement refers to an area marked by curly braces "{}". In this area, we can place declarations and statements, and the most common type of compound statement is the function body. Inside the function body, we can define variables and combine various other statements to implement the function's functionality. An expression statement is a statement consisting of an expression followed by a semicolon, such as a function call statement or a variable assignment statement. Of course, the expression in the expression statement can also be empty, which makes it an empty statement consisting of only one ";".

```c
int foo(int x, int y) {  
  int sum = x + y; 
  if (sum < 0) {  
    sum = -sum;  
  }
  return sum;
}
```

##### Select Statement

Similar to most other languages, in C, the select statement mainly refers to the statement composed of the two syntax structures `if…else` and `switch…case`. First look at the `if…else` statement:

![C language ifelse](C-ifelse.png)

As can be seen from the assembly code in the red box, the value of the variable v is stored in the stack memory at the address of the value of the rbp register minus 4. The program uses multiple labels (such as .L2, .L3, etc.) to divide the processing logic corresponding to different branches, and the branch judgment process is completed by the instruction cmp and the conditional jump instructions je and jne. The overall logic of assembly code and C code is basically a one-to-one correspondence. Therefore, in this case, in order to keep the execution performance of the program as much as possible, you can put the conditional statement with a higher chance of hitting in the front position.

Next, let's look at the `switch…case` statement.

![C language switchcase](C-language-switchcase.png)

First, the assembly code will pass the cmp instruction to determine the value in the register eax, that is, whether the value of the variable v is greater than 60. If the judgment is established, the program will directly jump to the label .L2, and the number 4 will be used as the return value; if the condition is not established, the program will continue to execute. The code then generates a "token" value for participating in subsequent operators based on the value of the variable v. The steps to generate this value are as follows:

1. Set the value of register `edx` to 1;
2. Set the value of register `ecx` to the value of variable v;
3. Shift the value in register `rdx` left by v bits (the value is extended to 64 bits);
4. Move the value in register `rdx` to rax for later use.

Next, the program completes the first screening process for the value of the variable v. This process is easy to understand. If you expand the immediate operand 1154047404513689600 of the first line of instruction movabs in 64-bit binary, you will find that only the 50th and 60th bits are set.

The `and` instruction in the second line will "AND" the super-long immediate value with the token value obtained by shifting the value of the variable v before. If the result of the operation is not 0, it means that the 50th or 60th bit of the token value is definitely not 0, that is, the value of the variable v is 50 or 60. Otherwise, the value of the variable v does not meet the filter criteria of the case statement. At this point, I believe you have already understood the basic logic of screening.

However, branch filtering by means of "bitmap" does not cover all cases perfectly. For example, when the filter value of the case statement is too large to use the register to map, the compiler will "fall back" the implementation of `switch…case` to a similar way to `if…else` under the default optimization condition. That is to say, use cmp instructions and conditional jump instructions to filter and branch branches. Of course, which implementation strategy is adopted varies by compiler.

In addition to the implementation of the `if…else` and `switch…case` statements described above, under high optimization levels, the compiler may also use a method called "skip table" to implement these two conditional selection statements.

![Switch case statement with high optimization in C language](Switchcase-optimization-in-C-language.png)

**The skip table is a conditional matching strategy that trades space for time.**

First, the assembly code marked in red subtracts the value of the variable v from the value of the smallest matching condition in the select statement, which is 10 in this case. Then, the program uses the cmp and ja instructions to determine whether the value of the variable v exceeds the difference between the maximum matching condition and the minimum matching condition in the select statement, which is 30 here. If so, the program jumps directly to label .LBB0_3 and returns the value 3.

Otherwise, the program uses the skip table to find the correct branch for the value of variable v. The so-called jump table is the address information that is stored in a continuous memory and can be used to assist in finding the correct target address. In the above example, the jump table starts at label .LJTI0_0. In this memory, the correct branch processing address corresponding to each integer value in the range of 10 to 40 of the filter value is continuously stored. The next blue code holds the value that the function needs to return when the skip table item 0 "hit".

Suppose that when calling function foo, the value of the variable v passed in is 20. When the jmp instruction in the dotted box is executed, it will find its corresponding correct branch address in the jump table according to the value of v. Since the value in the rdi register here is 10 (20 - 10), the correct branch handle address is the value corresponding to the tenth entry in the jump table. It can be seen here that at the position of the .LJTI0_0 label + 80 bytes (.quad represents 8 bytes of data), it corresponds to the address of the label .LBB0_4. The location of this label is the correct branch processing address when the variable v has the value 20.

In addition to the common ways that these compilers implement branch statements mentioned above, depending on the specifics of the branch statement, the compiler may also employ some specialized optimizations for specific shapes of code. Even for the most "original" combination of cmp and conditional jump statements, the compiler will appropriately use optimization strategies such as "dichotomy" according to the conditions of the C source code to speed up the conditional screening process.

In summary, select statement compilers typically implement them in the form of bitmaps, skip tables, dichotomy-based tests, and conditional jump statements.

##### Iteration Statement

In the C language, the iteration statement mainly includes three basic grammatical forms: `do…while`, for, while. Except for some differences in the execution details of these statements, the corresponding assembly implementation ideas are similar. Here I take the `do…while` statement as an example to explain, the specific code is as follows:

![Do while statement in C language](Dowhile-statement-in-C-language.png)

It can be seen that before the actual conditional judgment on the variable v, the program has executed the printf function once, and this is the characteristic of the `do…while` statement compared with other iterative statements. The iterative process uses the .L2 label as the starting point for each iteration, and each iteration follows the rule of "execute the loop body first, and then judge the conditions". The judgment of the condition and the execution transfer process are carried out by the instructions test and jne respectively.

Even at high optimization levels, the machine-level assembly implementations of these three basic iteration statements in C don't differ significantly, but that doesn't mean you can use them at will. At least for `do…while` and while , they differ in execution details, and if used indiscriminately, are likely to introduce unnecessary and hard-to-debug bugs into your program.

##### Jump Statement

Jump statements in C language mainly refer to those grammatical structures that can change the program execution flow. They mainly include the following four types:

* break statement;
* continue statement;
* return statement;
* goto statement.

In C code, most of the statements used to control the execution logic of the program are implemented through conditional jump statements. Through code analysis, the compiler can find the possible "jump-in points" and "jump-out points" in the program, and at the machine instruction level, use conditional jump instructions such as je to control the execution flow of the program to transfer between these points.