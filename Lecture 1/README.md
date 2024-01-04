# Topic: Introduction to Automata and Logic

## Logic
Logic refers to the calculus of computation. What does this calculus mean? A computation method related to a straightforward machine that helps solve computational problems efficiently.

## Automata
Automata refers to how machines are built and work fundamentally (remember our seemingly elementary MIPS computer from architecture? The ISA forms how that machine works). It is a theory that governs the working of an abstract, deterministic machine according to a fixed set of instructions and predetermined functions. It can describe and analyze the dynamic behavior of discrete systems. 

## Not all problems are solvable, no matter what computer is used.
* Choosing a truly random number (NOT a pseudo-random number. mt19937 'random' also is a function of the seed).
* Halting problem (generate an algorithm/program P0 which takes an input i0: a program p1 + ANY input i1 for p1. It returns TRUE if p1[i1] terminates, and FALSE if p1[i1] runs in an infinite loop). The algorithm should work for ANY i0 (virtually any and every p1 and i1).
* Problems involving irrational numbers like finding the exact (not approximate) value of math::pi, math::exp.
All the above are examples of problems that are simply not solvable, no matter what machine is used.

## An Example: Is the number of 1s in the binary number even?
One algorithm: maintain a counter (at 1). Keep XOR-ing with the bits, first to last. Each XOR with '1' inverts the counter, and each XOR with 0 does nothing. Then we return that counter as it is (1 if even no. of 1s encountered, 0 if odd no. of 1s encountered). Time Complexity: O(n), where n = length of binary representation. This is a computational algorithm and an example of automata. Note that this should work for any binary number and process that, not just one single number such as 01011011011.

## An Extention: Is the number of 1s a multiple of three?
A slightly more exciting version of the above automata problem. One way is to maintain two bit-counters, starting at 11. After an encounter of 1, we can use an apt circuit to make it 11 --> 00 --> 01 --> 11. Here, the arrow symbol means that whenever we encounter a 1 in the number string, we move the bit-counters to the following configuration according to the arrow. Flashback to DLDCA Lab 1 for how it is made at the circuit level. We return the first bit (1 if the number has no. of 1s a multiple of 3; 0 otherwise). What we saw here is an excellent start to automata.

## A Slightly Different Problem: Is the binary number, as such, a multiple of three?
A good approach would be to have a ternary counter (0, 1, or 2) and scroll left to right. The counter starts at 0. We traverse the bits from left to right. At each traversal, we first multiply the counter by 2 (meaning: change the value as 0 --> 0; 1 --> 2; 2 --> 1). If that bit is 1, we add 1 to the counter (meaning: change the value as 0 --> 1; 1 --> 2; 2 --> 0). Repeat till the last bit. We return TRUE if the counter is 0. The exact design of this is left as an exercise.

## Another Example: Product of numbers and equating.
Let us look at this string n0: 1111.1.11 0 1111...11 0 11111...11111. Ignore the spaces. Let there be a 1s followed by a '0', then by b 1s, a '0', and c 1s. We are to return if a * b == c. Here, a, b, and c are themselves natural numbers. It is easy to find a, b, c. Just maintain integer counters. Comparing a * b (if found) and c is also straightforward (bitwise comparison if binary representation; evaluate if the length of strings is the same if it is the comparison of strings of 1s). But the main problem: how is the multiplication of a*b done? How to go behind it?

One naive way is to start with a null binary string s0 and iterate through the second string of b 1s. During each iteration, we add a 1s to s0 (a == length of the first string of 1s in n0. Again, we can iterate through that every time to add a 1s to s0). We then consider the string of c 1s and iterate through that separately but simultaneously s0. We compare the lengths. Of course, instead of unary strings, we can maintain an integer counter, but that requires an addition to work, another automata problem but solvable using adder circuits. Either way, we have a working solution.

## An Extention: Binary numbers separated by a 2.
Let us look at this string s0: 0101 2 1100 2 101110. Ignore the spaces here, too. Here, we have a0 bits (0 or 1) corresponding to the first number a1 in the sense that a1 is the value of the binary number represented by the string a2 (of a0 bits, before the first 2) (leading 0s eliminated ofc). Same for b1 and c1. We are to determine if a1 * b1 == c1.

Notice that, unlike the previous problem, the naive way is not linear in the input string length but is exponential. In fact, it is O((a1 * b1) + c1) which is in general, O(((2**a0) * (2**b0)) + (2**c0)). Can we say this is exponential in at least half the length of the string? Exponential ==> will take a very long time (maybe seconds, maybe years, maybe > lifetime of the universe), and this time changes drastically with a change in the input length.

The big question is: is there an algorithm that can do this in a better amount of time for any input, say linear or at max quadratic in the input string length? This is a fascinating automata problem. Of course, if we can define multiplication by 2 for a string representation of a number (left shift the bits) and addition both in O(length of the string) size, then the problem can be solved in quadratic time complexity, using the multiplication algorithm learned in elementary school. With this comes the problem of making the circuit for a multiplier with as little overhead as possible, which is a challenge in itself and an exercise for the reader.
