# Registers
# 1 - General-Purpose Registers
All general purpose registers can be fully used and manipulated.
## 1. 1 - list of general-purpose registers:
```
rax  |  register a extended  |  accumulation register
rbx  |  register b extended  |  base register
rcx  |  register c extended  |  count register
rdx  |  register d extended  |  data register
-----|-------------------------------
rsi  |  register source index
rdi  |  register destination index
rbp  |  register base pointer
rsp  |  register stack pointer

r8, r9, r10, r11, r12, r13, r14, r15
```
## 1. 2 - calling different bits of the general-purpose registers:
```
64-bit | 32-bit | 16-bit | lower 8-bit | higher 8-bit  
-------|--------|--------|-------------|--------------
rax    | eax    | ax     | al          | ah
rbx    | ebx    | bx     | bl          | bh
rcx    | ecx    | cx     | cl          | ch
rdx    | edx    | dx     | dl          | dh
rsi    | esi    | si     | sil         |--------------
rdi    | edi    | di     | dil
rbp    | ebp    | bp     | bpl
rsp    | esp    | sp     | sp
-------|--------|--------|-----
r8     | e8d    | r8w    | r8b
r9     | e9d    | r9w    | r9b
r10    | e10d   | r10w   | r10b
r11    | e11d   | r11w   | r11b
r12    | e12d   | r12w   | r12b
r13    | e13d   | r13w   | r13b
r14    | e14d   | r14w   | r14b
r15    | e15d   | r15w   | r15b
```
## 1. 3 - Special use of some general purpose registers:
**ax | accumulation register**
1. The rax register determines what syscall is performed. (linux)
2. In functions, ax is often used to store the return value.
3. For `multiplication (mul)` ax is used as an operand.
4. For `multiplication (mul) by 16^n bit`, the lower 16^n bits are stored in ax.
5. When `dividing by 8 bits (div)`, ax will be used as dividend.
6. When `dividing by 32^n bits (div)`, the lower 32^n bit of ax will be used as a dividend.
7. When `dividing by 8 bits`, the quotient goes into al, the remainder into ah.
8. When `dividing by 16^n bits`, the quotient goes into ax.

(I am actually uncertain what happens when dividing by 16 bits, and havent found anything)

**dx | data register**
1. For `multiplication (mul) by 16^n bit`, the higher 16^n bits are stored in dx.
2. When `dividing by 16^n bits`, the remainder goes into ax.

**bp | base pointer**
1. Defines the base of the stackframe.
2. The basepointer should always point at its value in the previous stackframe.

**sp | stack pointer**
1. Defines the top value of the stackframe.
2. Is decremented by `push` and `call`.
3. Is incremented by `pop` and `ret`.

**syscall order of register usage**

(just an observation from me)
1. rax
2. rdi
3. rsi
4. rdx
5. r10
6. r8
7. r9


usefull website: https://gpfault.net/posts/asm-tut-3.txt.html










