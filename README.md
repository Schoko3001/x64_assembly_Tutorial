# x64assemblyTutorial
### short introduction
I will be writing this Tutorial as I am learning, and it is written exactly how I would have wished a Tutorial to be when learning.
This repository is not really made for anyone to read, and more like a document for me to re-read and reflect on, however it is written in a way that should be understandable to anyone with a little knowledge about adresses, heap and stack.
Also, feel free to correct me if I made any mistakes. 

My first step of learning assembly was to figure out which kind I wanted to learn.

After a day of playing around I decided to learn `x64` with `AT&T syntax` on `Linux (on a virtual machine)`.

The reasons?
  1. my cpu is x64 based
  2. I found more information on AT&T
  3. gnu uses AT&T as a standard
  4. Linux because searching for Systemcalls on windows is really painfull

### useful Links
  1. sycall https://blog.rchapman.org/posts/Linux_System_Call_Table_for_x86_64/
  2. assembler directives https://ftp.gnu/old-gnu/Manuals/gas/html_chapter/as_7.html

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
**Instruction pointer**

The `Instruction pointer` determines `which line of code is being executed`. It can be manipulated by many different instructions that i will write about in chapter 2.

**.global**

To tell the computer where to start, `.global something` is written at the top of the program. This will tell the computer to start executing the code from wherever you write `something:`.
```assembly
.global _start
.text

_start:
  "do something"
  "exit the program"
```

**.text**
The `.text` tells the computer where code is. This part of a program marks the beginning of a text segment.

**Label**

The expression `something:` is call a Label.
A `label` always has a `:` at the end. The label serves as a "gateway" that can be jumped to by the `instruction pointer` or accessed.

## 0. 2 - movq instruction
**mov**

The mov instruction sets one thing equal to another. In my opinion it is the most essential instructions
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
**instruction suffix**

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
**register types**

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
The `rbp, rsp` registers manage the stack, and should not be touched unless you know what you are doing.

The `rax, rsi, rdi` registers are used for syscalls, although other registers might also come in use.

Apart from these names, one might also come arcross something like `eax` or `r15d`.
These expressions refer to a smaller amount of bytes of the same register.
```txt
64 bits  | rax | rbx | ... | r8  | r9  | ...
32 bits  | eax | ebx | ... | r8d | r9d | ...
16 bits  |  ax |  bx | ... | r8w | r9q | ...
 8 bits  | idk look it up yourself you lazy
```
**usage**

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
movq $0, %rdi
systemcall
```
The `rax is set to 60`, which makes the syscall a `sys_exit`.

The `rdi` determines for sys_exit the `error_code`. As you might know, an error code of 0 means that no error happened.

## 0. 7 - First program
Combining everything that has been said up until now would look like this:
```assembly
.global _start
.text

_start:
  movq $60, %rax
  movq $0, %rdi
  systemcall

```
oh and don't forget to add a new line at the end, because it will not assemble if you don't

Now you just need to assemble and execute the program. For me that would be:
```txt
gcc useless.s -o -g
ld useless.o -o useless
./useless
```
Now to see the error_code just type:
```txt
echo $?
```
Obviously this will also work for the rdi/error_code being any other value.

# 1 - Hello World  
In this chapter I will show you how to write a easily readable program that can write "Hello World" in the Terminal.

## 1. 0 - What needs to be done?
We have previously written a Program that starts and ends.

For it to write "Hello World" we will need to `create a string` and `tell the OS to print it` before exiting the program. 

## 1. 1 - Creating a string
The `.ascii` tells the computer that the "Hello World" is a string. Apart from `.ascii` you may also find:
```txt
.ascii  "Hello World\n"  |  
.asciz  "Hello World\n"  |  adds a \0 (termination char) at the end
.string "Hello World\n"  |  adds a \0 (termination char) at the end
```
To access the string we will use a `label` and the `assembler directive: .ascii`.
```assembly
.Hello_World:
  .ascii "Hello World\n"
```
When the label is referenced a certain way, it is replaced by the adress of the string. 

Here .Hello_World is not an assembler directive. The . is a stylistic choice and only there for me to keep track of the labels referencing to assembler directives.

I use the website https://ftp.gnu/old-gnu/Manuals/gas/html_chapter/as_7.html whenever I need to look up assembler directives.

## 1. 2 - Writing into the Terminal
**sys_write**

To write into the terminal we need the `Write` syscall: By looking it up we see the following:
```txt
rax                      |  1  
rdi (destination index)  |  1 (for the Terminal)
rsi (source index)       |  adress of the string
rdx                      |  lenght of the string
```

This means that the code should look like this:
```assembly
movq $1, %rax
movq $1, %rdi
leaq .Hello_World, %rsi
movq $12, %rdx
syscall
```
Here the label reference `.Hello_World` represents the adress of the in 1. 1 defined string: `"Hello World\n"`.

**lea instruction**

`lea` stands for `load effective adress`

The `lea instruction` copies the `adress` unlike the `mov instruction` which copies the value. 
```assembly
leaq .Hello_World, %rsi
; is the same as writing
movq $.Hello_World, %rsi
```
Note that I have written `$.Hello_World`. This works because the `$` takes the `direct value` of whatever we write after it, in this case the `adress`.

However the latter is not the normal way to do this. Usually the `lea` instruction is used in cases like these.

I `highly recommended to use the lea instruction` for readability. Especially since the intel syntax does not use a $.

## 1. 3 - Hello World program
By combining everything that has been said, we get something like this:
```assembly
.global _start
.Hello_World:
  .ascii "Hello World\n"
.text

_start:
  movq $1, %rax
  movq $1, %rdi
  leaq .Hello_World, %rsi
  movq $12, %rdx
  syscall

  movq $60, %rax
  movq $0, %rdi
  syscall
```
## 1. 4 - .equ
The .equ assembler directive can be used to make the program more readable by giving us the opportunity to replace numbers with words.
```assembly
.equ Hello_World_len,12
```
Now the code for printing "Hello World\n" would look like this:
```assembly
movq $1, %rax
movq $1, %rdi
leaq .Hello_World, %rsi
movq Hello_World_len, %rdx
syscall
```
Although a similar readability can be achieved with comments, .equ might still be usefull in some cases.

# 2 - If and Loops | Cmp and Jmp

# 3 - Stack and Functions 
The Stack is the place most programming languages use to store memory and many instructions like `call`, `ret`, `pop` and `push` make use of it.
In this chapter i will explain how we can use the stack in assembly.

## 3. 0 - Translating a function from c to assembly
If we write a program with a function in c, it might look like this:
```c
int add(int a, int b)
{
  return a + b;
}

int main()
{
 add(1,2);
}
```
Now if we convert it into assembly, change it's structure and the way it exits, we will get something like this:
```assembly
.global _start
.text

_start:
  pushq %rbp
  movq %rsp, %rbp
  movl $2, %esi
  movl $1, %edi
  call add
  popq %rbp

  ;this is the exit syscall from 0. 7
  movq $60, %rax
  movq $0, %rdi
  systemcall
  
add:
  pushq %rbp
  movq %rsp, %rbp
  movl %edi, -4(%rbp)
  movl %esi, -8(%rbp)
  movl -4(%rbp), %edx
  movl -8(%rbp), %eax
  addl %edx, %eax
  popq %rbp
  ret
```
In this chapter I will explain exactly what is happening here.

## 3. 1 - Stackframe
A stackframe the a place where a function can store its memory.

Everytime a function is called, a new stack frame is created right above the previous one. 

The `rsp | stack pointer` is pointing at the `top variable`. Everything `above is garbage`

The `rbp | base  pointer` points at the `rbp of the previous stackframe`. Everything `below is part of a previous stackframe`

Inbetween is the space where a function stores its variables.
```
address |  value  | 
--------|---------|----------
...     |   ...   |
 96     | top var |  <-- rsp
...     |   ...   |
120     |   var   |   
128     | old rbp |  <-- rbp
...     |   ...   |
```
Everything outside the stackframe is usually considered garbage.
This is the reason it is not possible to directly access variables from other functions. 
Of course in Assembly we can access memory outside of the stackframe without any problems. 

When a program starts, it is assigned a place on the stack to store memory on. We can access the exact location with the `stackpointer`, short `rsp/esp/...`.

In modern operating systems, there is no "below" the esp, and the value the esp is pointing to should be ignored unless you know what you are doing
```
address | value |
  0     |  ...  |
           ...
 40     |  ...  |
 48     |  ...  |
 56     |  ...  |
 96     |  ???  |  <-- rsp
           ...    
 n      |  ???  |  <-- rbp
```
## 3. 2 - push and pop
**push**

Lets look at what happens at the start of the Programm
```assembly
_start:
  pushq %rbp
```
Here we can see the pushq instruction, but what does it do?

`pushq %rbp` essentially executes two instructions:
1. `decq %rsp`
3. `movq %rbp, (%rsp)`

after executing `push %rbp`, the memory storage would look like this:
```
address | value |
--------|-------|---------
  0     |  ...  |
           ...
 40     |  ...  |
 48     |  ...  |
 56     |   n   |  <-- rsp
 64     |  ???  |
           ...    
 n      |  ???  |  <-- rbp 
```
Essentially, the `push %rbp` pushes the `the value of rbp` on top of the stack.

**pop**

`Pop` does exactly the opposite: it takes the top value off the stack and moves it where you want.

`popq %rbp` essentially executes two instructions:
1. `movq (%rsp), %rbp`
2. `incq %rsp`

## 3. 3 - call and ret
**call**

The `call` instruction is generally used to call a function.

`call labelName` essentially executes two instructions:
1. `push instructionPointer`
2. `jump labelName`
However, the call instruction is necessary here, because only it can push the instruction pointer

**ret**

Just like the pop instruction reverses the push instruction, the `ret instruction reverses the call instruction`.

`ret` essentially executes two instructions:
1. `pop instructionPointer`
2. `jump "popped instructionPointer"`
However, the ret instruction is necessary here, because only it can pop the instruction pointer.

## 3. 4 - Creating a new frame
Now that we know the pop, push and call instructions, lets take a look at the assembly code from 3. 0 and go through it step by step
```assembly
.global _start
.text

_start:
  pushq %rbp          ; <-- value of rbp is saved on stack
  movq %rsp, %rbp     ; <-- rbp is moved to rsp
```
After these instructions, a new stackframe for the _start: function was created.

The stack now looks like this:
```
address  |  value  |
---------|---------|----------
  0      |   ...   |
             ...
 40      |   ...   |
 48      |   ...   |
 56      | old rbp |  <-- rsp, rbp
 64      |   ???   |
             ...    
 old rbp |   ???   | 
```
In our case, there is nothing in the _start function, but since that would be a bad example, lets just assume that we stored some variables.
```
address  |  value  |
---------|---------|----------
  0      |   ...   |
             ...
 32      |   ...   |
 40      |  var b  |  <-- rsp
 48      |  var a  |
 56      | old rbp |  <-- rbp
 64      |   ???   |
             ...    
 old rbp |   ???   | 
```
The next step is to prepare for the new function "add".

We move the function parameters into the registers:
```assembly
  movl $2, %esi
  movl $1, %edi
```
In our case 2 and 1, because we want our function to add 1 and 2.

Now with the preparations done, the program can call the function:
```assembly
  call add
```
We have now `pushed the old instruction pointer` onto the top of the stack, and the new instruction pointer `jumped to the "add" label`

The stack now looks like this:
```
address  |  value  |
  0      |   ...   |
             ...
 24      |   ...   |
 32      |   i p   |  <-- rsp
 40      |  var b  |
 48      |  var a  |
 56      | old rbp |  <-- rbp
 64      |   ???   |
             ...    
 old rbp |   ???   | 
```
Next, we will repeat what we did for the _start function: `set up the "add" function stackframe`
```assembly
add:
  pushq %rbp
  movq %rsp, %rbp
```
The stack now looks like this:
```
address  |  value  |
  0      |   ...   |
             ...
 16      |   ...   |
 24      | old rbp |  <-- rsp, rbp
 32      |   i p   |
 40      |  var b  |
 48      |  var a  |
 56      | old rbp |
 64      |   ???   |
             ...    
 old rbp |   ???   | 
```
Now the move function enters the parameters into the stack with:
```assembly
  movl %edi, -4(%rbp)
  movl %esi, -8(%rbp)
```
The stack now looks like this:
```
address  |  value  |
  0      |   ...   |
             ...
  8      |   ...   |
 16      |    1    |
 20      |    2    |
 24      | old rbp |  <-- rsp, rbp
 32      |   i p   |
 40      |  var b  |
 48      |  var a  |
 56      | old rbp |
 64      |   ???   |
             ...    
 old rbp |   ???   | 
```
Note that we have not moved the rsp, and we don't need to since we don't intend on calling a new function from this one.

## 3. 5 - Return to a frame

Now we add the parameters we just stored into `eax (rax)`.
```assembly
  movl -4(%rbp), %edx
  movl -8(%rbp), %eax
  addl %edx, %eax
```
We do this, because the `return value of a function should always be stored in rax`.

Now we just need to `close the "add" function` and `return to the _start funtion`.
```assembly
  popq %rbp
  ret
```
After `popq %rbp`, the rbp returns to its location in the stackframe of the _start function.

The `ret` instructions `pops the saved location` of the instruction pointer and `jumps to it`.

It arrives at the instruction `right below the "call add" instruction`

The stack now looks like this:
```
address  |  value  |
  0      |   ...   |
             ...
  8      |   ...   |
 16      |    1    |
 20      |    2    |
 24      | old rbp |
 32      |   i p   |
 40      |  var b  | <-- rsp
 48      |  var a  |
 56      | old rbp | <-- rbp
 64      |   ???   |
             ...    
 old rbp |   ???   | 
```
Now we have completely exited the function and are back in the _start function. I could now also go through exiting the _start function, but that would be a bit too repetitive.







### End.
It seems that you have reached the end of this Tutorial. If you want to read more, you will just need to wait a bit since I am currently updating daily.
