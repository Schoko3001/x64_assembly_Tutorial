# Learn_Instructions

# Syntax

# Data Movement
```
mov
lea
```

# Arithmetic

## placeholder

## Bitwise
```
not | and | or | xor
```
`xor %a, %a` is often used in the place of `mov $0, %a` because it is faster.

## Shift
```
                 | left | right
-----------------|------|-------             
   logical shift | slf  | slr
arithmetic shift | saf  | sar
```
- leftshift `"<<"` increments the value
- rightshift `">>"` decrements the value

- a logical shift also shifts the sign
- an arithmetic shift doesn't shift the sign and if there is one, a leftshift fills the new space with "1". This makes an arithmetic shift always a multiplication of two or division by two.

# Conditional Instructions

## j, cmov and set explained

j is the conditional version of the jump instruction.

cmov is the conditional version the move instruction.

set sets an `8 bit register` to 1 if true, and to 0 if not.

## lookup tables
**condition suffixes written out:**
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
**condition suffix table for arithmetics:**
```
         |   =   |   !=   |   >    |   >=   |   <    |   <=
---------|-------|--------|--------|--------|--------|--------
unsigned | e, z  | ne, nz | a, nbe | ae, nb | b, nae | be, na
signed   | e, z  | ne, nz | g, nle | ge, nl | l, nge | le, ng
```
**condition suffix table for flags:**
```
         | ZF     | CF | OF | SF | PF
---------|--------|----|----|----|--------------------
flag = 1 | z, e   | c  | o  | s  | p, pe (parityEven)
flag = 0 | nz, ne | nc | no | ns | np, po (parityOdd)
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


