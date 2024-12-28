# x64assemblyTutorial
### short introduction
I am currently learning x64assembly, and i will be writing this as I am learning. This Tutorial is written exactly how I would have wished a Tutorial to be when learning.
This repository is not really made for anyone to read, and more like a document for me to re-read and reflect on, however it is written in a way that should be understandable to anyone.
Also, feel free to correct me if i made any mistakes. 

My first step of learning assembly was to figure out which kind i wanted to learn.

After a day of playing around I decided to learn `x64` with `AT&T syntax` on `Linux (on a virtual machine)`.

The reasons?
  1. my cpu is x64 based
  2. I found more information on AT&T
  3. gnu uses AT&T as a standard
  4. Linux because searching for Systemcalls on windows is really painfull

### useful Links
  1. sycall https://blog.rchapman.org/posts/Linux_System_Call_Table_for_x86_64/
  2. 

# 0 - absolute Basics
At the end of this chapter you will be able to write `a program that can ... do nothing?` wait what?

Well, even writing a program that does nothing except for not crashing is not that simple in assembly.

## 0. 0 writing and assembling a program
My Code is written on `vsCode` with the `GNU Assembler Lanuguage Support`.

I use the `.s` file extension.

To assemble and execute a file I type the following things into the terminal:
  1. `gcc file.s -c -g`    file.s -> file.o
  2. `ld file.o -o file`   file.o -> file
  3. `./file`

## 0. 1 - Most fundamental keywords
### .global
To tell the computer where to start, `.global something` is written at the top of the program. This will tell the computer to start executing the code from wherever you write `something:`.
```assembly
.global _start
.text

_start:
  "do something"
  "exit the program"
```
### .text
The `.text` tells the computer where code is. This part of a program marks the beginning of a text segment.
### Label
The expression `something:` is call a Label.
A `label` always has a `:` at the end. The label serves as a "gateway" that can be jumped to or accessed.

## 0. 2 - movq
### mov
The mov keyword sets one thing equal to another. In my opinion it is the most essential operation
```assembly
movq "b", "a"     ;"a" and "b" mean nothing, not real code
```
Here "a" is set equal to the value of "b". In a normal programminglanguage it would look like this:
```c
"a" = "b" //AT&T
```
It is important to remember that this repository is about the AT&T syntax, because in the `intel syntax`, the opposite would be the case:
```c
"b" = "a" //Intel
```
### operation suffix
The `q` at the end of `movq` is a called an `operation suffix` and stands for `quad`, an operation parameter of `64bits`. 

It is important to know that there are actually multiple of these operation suffixes:
```txt
int
b |  8 bits | 1 byte
w | 16 bits | 2 bytes
l | 32 bits | 4 bytes
q | 64 bits | 8 bytes

float
f | 32 bits |  4 bytes
t | 80 bits | 10 bytes
```

## 0. 3 - $
The `$` is used to read the value of something. For example: assigning "a" the value 5 would look like this:
```assembly
movq $1, "a"
```  
The `$` symbol tells the computer to treat whatever comes after it as `the direct value` and `not an adress to a value`.

I personally like to compare it to the `& in c` that tells you the `adress of a variable` and `not the value of a variable`.

## 0. 4 - Registers
### register types
A register is the fastest memory storage that can be accessed. But there are only `16 general purpose registers` in the `x64 architecture`. They are the following:
```txt
rax  |  register a extended
rbx  |  register b extended
rcx  |  register c extended
rdx  |  register d extended
-----|---------------------
rbp  |  register base pointer
rsp  |  register stack pointer
rsi  |  register source index
rdi  |  register destination index

r8, r9, r10, r11, r12, r13, r14, r15
```
The `rbp, rsp` registers manage the stack.

The `rax, rsi, rdi` registers are used for syscalls, although other registers might also come in use.

Apart from these names, one might also come arcross something like `eax` or `r15d`.
These expressions refer to a smaller amount of bytes of the same register.
```txt
64 bits  | rax | rbx | ... | r8  | r9  | ...
32 bits  | eax | ebx | ... | r8d | r9d | ...
16 bits  |  ax |  bx | ... | r8w | r9q | ...
 8 bits  | idk look it up yourself you lazy
```
### usage
In the AT&T assembly syntax, you need to put a `%` in front of the register name
```assembly
movq $5, %rax
movq %rax, %rbx 
```
In other programming languages, this code would look might look something like this:
```py
a = 5
b = a
```

## 0. 5 - Syscall
In order to do anything outside the program, be it writing, reading, or even exiting the program, you need perform a `syscall`, short for `systemcall`. Think of it as asking the operating system to do something for you.

When writing:
```assembly
movq $60, %rax
movq $0, %rdx
systemcall
```

The `rax` register determines the type of syscall performed.

The `rdi` register value is the `destination index` of the syscall.

The `rsi` register value is the `source index` of the syscall.

For some systemcalls, the registers `rdx`, `r10`, `r8` and `r9` are also needed.

I personally use https://blog.rchapman.org/posts/Linux_System_Call_Table_for_x86_64/ to look up the different syscalls 

The `systemcall` stores its `return value in the rax register`. This means, that if you want to perform muliple systemcalls of the same type with the same rax value, you still need to `readjust the rax value every syscall`.

## 0. 6 - Exit
Even to exit a program, you need to perform a syscall.
```assembly
movq $60, %rax
movq $0, %rdx
systemcall
```
The `rax is set to 60`, which makes the syscall a `sys_exit`.

The `rdx` determines for sys_exit the `error_code`. As you might know, an error code of 0 means that no error happened.

## 0. 7 - First program
Combining everything that has been said up until now would look like this:
```assembly
.global _start
.text

_start:
  movq $60, %rax
  movq $0, %rdx
  systemcall

```
oh and btw dont forget to add a new line at the end, because it will not assemble if you dont

Now you just need to assemble and execute the program. For me that would be:
```txt
gcc useless.s -o -g
ld useless.o -o useless
./useless
```
Now to see the error_code just type in
```txt
echo $?
```
Obviously this will also work if you use another error_code.

# 1 - Hello World  
