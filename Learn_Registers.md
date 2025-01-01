# Registers
### Links:
1. Flag wiki article: https://en.wikipedia.org/wiki/FLAGS_register
2. 
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
________________________________________________________________________________________________________
| 64-bit                                           _____________________________________________________
|                                                  | 32-bit                  ___________________________
|                                                  |                         | 16-bit     ______________
|                                                  |                         | high 8-bit | low 8-bit  |
|  00000000  |  00000000 |  00000000  |  00000000  |  00000000  |  00000000  |  00000000  |  00000000  |
```
```
64-bit | 32-bit | 16-bit | low 8-bit | high 8-bit  
-------|--------|--------|-----------|--------------
rax    | eax    | ax     | al        | ah
rbx    | ebx    | bx     | bl        | bh
rcx    | ecx    | cx     | cl        | ch
rdx    | edx    | dx     | dl        | dh
rsi    | esi    | si     | sil       | ?
rdi    | edi    | di     | dil       | ?
rbp    | ebp    | bp     | bpl       | ?
rsp    | esp    | sp     | spl       | ?
-------|--------|--------|-----------|------------
r8     | e8d    | r8w    | r8b       | ?
r9     | e9d    | r9w    | r9b       | ?
r10    | e10d   | r10w   | r10b      | ?
r11    | e11d   | r11w   | r11b      | ?
r12    | e12d   | r12w   | r12b      | ?
r13    | e13d   | r13w   | r13b      | ?
r14    | e14d   | r14w   | r14b      | ?
r15    | e15d   | r15w   | r15b      | ?
```
## 1. 3 - Special use of some general purpose registers:
**ax | accumulation register**
1. The rax register determines what syscall is performed. (linux)
2. In functions, ax is often used to store the return value.
3. For `multiplication (mul)` ax is used as an operand.
4. For `multiplication (mul) by 16^n bit`, the lower 16^n bits are stored in ax.
5. When `dividing (div) by 8 bits`, ax will be used as dividend.
6. When `dividing (div) by 32^n bits`, the lower 32^n bit of ax will be used as a dividend.
7. When `dividing (div) by 8 bits`, the quotient goes into al, the remainder into ah.
8. When `dividing (div) by 16^n bits`, the quotient goes into ax.

(I am actually uncertain what happens when dividing by 16 bits, and haven't found anything)

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

# 2 - FLAGS register
The FLAGS register is a 64 bit register. It cannot be accesed like a normal register.

It serves as a storage for single bits values with predefined uses.

Link for more information: https://en.wikipedia.org/wiki/FLAGS_register

## 2. 1 - Exeptions

## 2. 2 - Carry flag
Unsigned operation resulted in an integer Overflow -> flag = 1

conditional jump instructions:
```
flag = 1 | jc  | jump if carry
flag = 0 | jnc | jump if not carry
````
## 2. 3 - Overflow flag
Signed operation resulted in an integer Overflow -> flag = 1


conditional jump instructions:
```
flag = 1 | jo  | jump if overflow
flag = 0 | jno | jump if not overflow
```
## 2. 6 - Zero flag (also equality)
value is zero -> flag = 1

conditional jump instructions:
```
flag = 1 | jz  | jump if zero
flag = 1 | je  | jump if equal
flag = 0 | jnz | jump if not zero
flag = 0 | jne | jump if not equal
```
## 2. 5 - Parity flag
Lowest bit is 0, value is even -> flag = 1

conditional jump instructions:
```
flag = 1 | jp  | jump if parity
flag = 1 | jpe | jump if parity is even
flag = 0 | jnp | jump if not parity
flag = 0 | jpo | jump if parity is odd
```
## 2. 6 - Sign flag
Highest bit not zero -> flag = 1 

conditional jump instructions:
```
flag = 1 | js  | jump if signed
flag = 0 | jns | jump if not signed
```



# 3 - Instruction pointer
The instruction pointer determines which isntruction is executed.

It is incremented after each instruction.

The instruction pointer reaching the end of a program results in an error

**jmp**

It can be manipulated with the jmp instruction and all of its conditional variants.

**call and ret**

call pushes the instruction pointer onto the stack, and then 

ret pops the instruction pointer and jumps to the popped location + 1

(I have not completely confirmed if ret increments the popped location or if call does before pushing it.







usefull website: https://gpfault.net/posts/asm-tut-3.txt.html










