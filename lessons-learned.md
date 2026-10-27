## Lessons Learned: Implementing Gimli in Jasmin

This document records what I learned while working through the Gimli tutorial in the Formosa Crypto Jasmin tutorial repository.

My goal was to understand how Jasmin can be used to implement cryptographic operations and how to debug low-level code by comparing its behaviour against a C reference implementation.

## 1. Bitwise Shifts and Rotations
Initially, I treated a left shift in C as if it were a rotation in Jasmin when implementing the sbox functions. 
Although they both move bits to the left, their behaviour differs:
- Left shift: Bits move to the left, and bits that leave the word are discarded.
- Left rotation: Bits move to the left, but bits that leave the word wrap around to the right.

What I learnt is that, when implementing cryptographic algorithms, understanding the precise semantics of bitwise operations is essential. Operations that look similar in different languages may behave differently.

## 2. Understanding Pointers and State Updates
Within the gimli function, I assigned introduce a variable state1, assigned state1 to it and then passed state1 to sbox inside the loop. 

However, I learnt that in C and Jasmin, assigning one pointer to another copies the pointer value, not the contents of the memory it references.

## 3. Understanding Jasmin's inline Concepts
While implementing the column loop in gimli, I encountered two different concepts in Jasmin:
- inline int, which represents a compile-time integer.
- reg u64, which represents a 64-bit register value.

The sbox function accepts a column parameter declared as reg u64, while the loop variable in gimli is declared as inline int.

I do not yet understand the relationship between compile-time values, register values.

## 4. Functional Testing and Formal Verification

After the functional tests passed, when running the tutorial's `make check` command, the constant-time proof checks succeeded, but another EasyCrypt proof failed because the Array12 theory could not be imported (which I have not investigated or resolved).

I learnt that functional testing and formal verification are different activities. A successful test suite does not automatically establish that an implementation is formally verified or secure against every attack.

Functional testing checks whether an implementation produces expected results for selected inputs. 
Formal verification uses a formal specification and mathematical reasoning to establish particular properties under stated assumptions. 
Constant-time verification concerns whether relevant program behaviour avoids depending on secret data in ways that could leak information through timing or other observable behaviour.
