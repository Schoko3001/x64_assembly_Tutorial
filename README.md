# x64assemblyTutorial
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

# 0 - absolute Basics
## 0.0 writing and assembling a program
My Code is written on `vsCode` with the `GNU Assembler Lanuguage Support`.

I use the `.s` file extension.

To assemble and execute a file I type the following things into the terminal:
  1. `gcc file.s -c -g`    file.s -> file.o
  2. `ld file.o -o file`   file.o -> file
  3. `./file`

## 0.1 fundamental keywords
To tell the computer where to start, `.global something` is written at the top of the program. This will tell the computer to start executing the code wherever you write `something:`.

The expression `something:` is called a Label.
```assembly
.global _start
.text

_start:
  "do something"
  "exit the program"
```
A `label` always has a `:` at the end. The label serves as a "gateway" that can be jumped to or accessed.
The `.text` tells the computer where code is. This part of a program marks the beginning of a text segment.

## 0.2 movq
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

## 0.3 $
The `$` is used to read the value of something. For example: assigning "a" the value 5 would look like this:
```assembly
movq $1, "a"
```  
The `$` symbol tells the computer to treat whatever comes after it as `the direct value` and `not an adress to a value`.

I personally like to compare it to the `& in c` that tells you the `adress of a variable` and `not the value of a variable`.

## 0.4 Registers
A register is the fastest memory storage that can be accessed. But there are only `16 general purpose registers` in the `x64 architecture`. They are the following:
```txt

|  rax  |  register a extended
|  rbx  |  register b extended
|  rcx  |  register c extended
|  rdx  |  register d extended

|  rbp  |  register base pointer
|  rsp  |  register stack pointer
|  rsi  |  register source index
|  rdi  |  register destination index

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
 8 bits  idk look it up yourself you lazy
```

## 0.6 syscall
  (incomplete)

## 0.7 exit
  (incomplete)


