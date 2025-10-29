# Approach 
1. Read the input file (the JSON)

Think of this like opening an envelope containing a list of labelled cards.

The file tells us:

n: how many cards (points) are in the envelope,

k: the minimum number of cards we need to reconstruct the recipe (polynomial),

and then each card has two things: a base and a value (the big number written in the given base).

Example card: "2": { "base": "2", "value": "111" } — the label 2 is the small number, base=2 says the big number is written in binary, and value="111" is the big number in that base.

2. Convert every big number into a standard number we can compute with

Numbers in JSON may be written in different numbering systems (binary, octal, hexadecimal, etc.). We convert them all into a single standard internal representation (a big integer).

This is like taking “111” in binary and converting to the familiar decimal 7.

We do this conversion carefully because the numbers can be very large — so we use a tool that supports very big numbers (in code this is BigInteger or equivalent).

3. Collect the pairs (small number, big number)

From each card we now have a pair: (x, y) where x is the label (small number like 2, 3, 6 …) and y is the converted big number.

From all cards, we choose the first k pairs. k is chosen because a polynomial of degree m needs m+1 points to be uniquely identified — the problem gives k = m + 1.

4. Reconstruct the hidden polynomial (in a robust but understandable way)

Instead of trying to guess the entire recipe step by step, we use a known safe method (Lagrange interpolation).

Very loosely: for each chosen point, we compute a small “contribution” that describes how that point alone affects the value of the polynomial at 0, then add up these contributions.

You can picture it as: each card says “if you want the recipe’s value at zero, take my output and scale it by a weight based on the other cards, then add these scaled outputs together”.

The math behind the weights guarantees that combining the contributions from all k points reproduces exactly the recipe’s value at zero.

5. Compute the polynomial value at 0 (the constant term)

The previous step produces the value of the polynomial when the input is zero — i.e., the constant term of the polynomial.

6. Output the result

Print that single number (in normal decimal form) as the result. 
