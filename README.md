# x64assenblyExperience
I am currently learning x64assembly, and in this repository i will be writing about my personal experience.
This repository is not really made for anyone to read, and more like a document for me to re-read and reflect on, however it is written in a way that should be understandable to anyone.
Also, feel free to correct me if i made any mistakes. 

My first step of learning assembly was to figure out which kind i wanted to learn.

After a day of playing around I decided to learn `x64` with `AT&T syntax` on `Linux (on a virtual machine)`.

The reasons?
  1. my cpu is x64 based
  2. I found more information on AT&T
  3. gnu uses AT&T as a standard
  4. Linux because searching for Systemcalls on windows is really painfull

## 0 - absolute Basics
### 0.0 writing and compiling a program
My Code is written on `vsCode` with the `GNU Assembler Lanuguage Support`.

I use the `.s` file extension.

To compile and execute a file I type the following things into the terminal:
  1. `gcc file.s -c -g`    file.s -> file.o
  2. `ld file.o -o file`   file.o -> file
  3. `./file`

### 0.1 Debugging
  (incomplete)

### 0.2 Registers
  (incomplete)

### 0.3 Exit a program
  (incomplete)


