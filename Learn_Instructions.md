# Learn_Instructions

# Syntax

# Data Movement

# Arithmetik

## placeholder

## Bitwise

## Shift

# Conditional Instructions

## j, cmov and set explained

j is the conditional version of the jump instruction.

cmov is the conditional version the move instruction.

set sets an `8 bit register` to 1 if true, and to 0 if not.

## lookup tables
```
n | not      | ! 

e | equal    | =
a | above    | unsigned version of >
g | greater  |   signed version of >
b | below    | unsigned version of <
l | less     |   signed version of <

z | zero     | value is zero
c | carry    |
o | overflow |
s | signed   | highest bit is 1
p | parity   |  lowest bit is 0


```
```
         |   =   |   !=   |   >    |   >=   |   <    |   <=
---------|-------|--------|--------|--------|--------|--------
unsigned | e, z  | ne, nz | a, nbe | ae, nb | b, nae | be, na
signed   | e, z  | ne, nz | g, nle | ge, nl | l, nge | le, ng
```
```
         | ZF     | CF | OF | SF | PF
---------|--------|----|----|----|--------
flag = 1 | z, e   | c  | o  | s  | p, pe
flag = 0 | nz, ne | nc | no | ns | np, po
```

## =
```
j    | je, jz
cmov | cmove, cmovz
set  | sete, setz
```

## !=
```
j    | jne, jz
cmov | cmovne, cmovnz
set  | setne, setnz
```

## >

**unsigned**
```
j    | ja, jnbe
cmov | cmova, cmovnbe
set  | seta, setnbe
```

**signed**
```
j    | jg, jnle
cmov | cmovg, cmovnle
set  | setg, setnle
```

## >=

**unsigned**
```
j    | jae, jnb
cmov | cmovae, cmovnb
set  | setae, setnb
```

**signed**
```
j    | jge, jnl
cmov | cmovge, cmovnl
set  | setge, setnl
```

## <

**unsigned**
```
j    | jb, jnae
cmov | cmovb, cmovnae
set  | setb, setnae
```

**signed**
```
j    | jl, jnge
cmov | cmovl, cmovnge
set  | setl, setnge
```

## <=

**unsigned**
```
j    | jbe, jna
cmov | cmovbe, cmovna
set  | setbe, setna
```

**signed**
```
j    | jle, jng
cmov | cmovle, cmovng
set  | setle, setng
```

## ZF | Zero Flag
**zero or equal**
```
j    | jz, je
cmov | cmovz, cmove
set  | setz, sete
```
**not zero or not equal**
```
j    | jnz, jne
cmov | cmovnz, cmovne
set  | setnz, setne
```

## CF | Carry Flag
**carry**
```
j    | jc
cmov | cmovc
set  | setc
```
**no carry**
```
j    | jnc
cmov | cmovnc
set  | setnc
```

## OF | Overflow Flag
**overflow**
```
j    | jo
cmov | cmovo
set  | seto
```
**no overflow**
```
j    | jno
cmov | cmovno
set  | setno
```

## SF | Sign Flag
(highest bit is 1)

**sign**
```
j    | js
cmov | cmovs
set  | sets
```
**no sign**
```
j    | jns
cmov | cmovns
set  | setns
```

## PF | Parity Flag
(lowest bit is 1)

**parity**
```
j    | jp
cmov | cmovp
set  | setp
```
**no parity**
```
j    | jnp
cmov | cmovnp
set  | setnp
```


