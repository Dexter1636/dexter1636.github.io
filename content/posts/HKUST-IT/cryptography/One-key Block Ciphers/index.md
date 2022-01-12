+++
title = "One-key Block Ciphers"
description = ""
math = true
type = ["posts","post"]
tags = [
    "cryptography",
    "web security",
    "computer science",
]
date = "2021-12-27"
categories = [
    "Computer Science",
    "HKUST-IT",
    "cryptography",
]
series = ["CSIT5710"]
[ author ]
  name = "Dexter"
+++

A 5-tuple $(M,C,K,E_k,D_k)$, where

- $M$: plaintext space
- $C$: ciphertext space
- $K$: key space
- $E_k$: Encryption transformation
- $D_k$: Decryption transformation

<img src="assets/EA0B1AC4E800DDE67EDF86081AA72D1A.png" style="zoom:33%;" />



#### Attacks

- Ciphertext-only attack: only know ciphertext $c$.
- Known-plaintext attack: know ciphertext-plaintext pair $(c,m)$.



#### Security Requirements

- $E_k$ and $D_k$ are known to all.
- It should be computationally infeasible to determine $m$, given $c$.
- It should be computationally infeasible to determine $D_k$ and $k$, given $c$ and $m$.



#### Transposition Ciphers

Let $f$ be a permutation of $Z_d$.

Message divided into blocks of length $d$. For each block $m=m_0 \cdots m_{d-1}$,
$$
E_k(m)=m_{f(0)} \cdots m_{f(d-1)}
$$
<img src="assets/3A59AC64ED6FFBA169C6D4F3BE795DE6.png" alt="img" style="zoom:33%;" />



#### Simple Substitution Ciphers

Let $f$ be a 1-to-1 mapping from alphabet $A$ to alphabet $B$.

For message $m=m_0m_1m_2 \cdots$,
$$
E_k(m)=f(m_0)f(m_1)f(m_2) \cdots
$$
Example:

<img src="assets/E4BA6FCAD98F82926CA8D876E93219C8.png" alt="img" style="zoom:33%;" />

Security: 

- **Insecure** with both known-plaintext attacks and ciphertext-only attacks.
- Reason: The frequency distribution of single English letters is known.



#### Unbreakable Ciphers

One-time pad (theoretically):

- $m$ is a binary string.

- $k$ is a random binary string with the same length as $m$.
- $c = m\ XOR\ k$
- $k$ is used for only one message and then discarded.



#### Linear and Nonlinear Functions

**Abelian group**

A set $A$ associated with a binary operation $+$ that for any $x$, $y$, $z$ in $A$,

- $x+y \in A$
- $(x+y)+c=x+(y+c)$
- $x+y=y+x$
- A special element $0 \in A$ such that $0+x=x$.
- $\forall x \in A$, $\exists y \in A$ such that $x+y=0$.

If $A$ is a finite set, $(A,+)$ is called a finite Abelian group.

<img src="assets/B8BFEEA4A26BF8F42A0B9BA0CB6F3859-16394109251461.png" alt="img" style="zoom:33%;" />

**The Abelian Group $(Z_m^n,+)$**

**Definition:** Let $m \geq 2$ and $n \geq 1$ be integers. Let
$$
Z_m^n=Z_m \times Z_m \times \cdots \times Z_m (n\ copies\ of\ Z_m)
$$
For any two elements
$$
x=(x_1,x_2, \cdots ,x_n) \in Z_m^n,\ y=(y_1,y_2, \cdots ,y_n) \in Z_m^n,
$$
define
$$
x+y=(x_1 \oplus_m y_1,x_2 \oplus_m y_2, \cdots ,x_n \oplus_m y_n) \in Z_m^n
$$
**Proposition:** $(Z_m^n,+)$ is an Abelian group with $m^n$ elements.

**Linear and Affine Functions**

$f:A \rightarrow B$ is **linear** if and only if $f(x+y)=f(x)+f(y)$.

$g:A \rightarrow B$ is **affine** if and only if $g=f+b$, where constant $b \in B$.

**Nonlinear Functions**

Any function that is not affine.

**Linear and Nonlinear Functions**

Both of linear and nonlinear functions are needed in many cryptographic systems as basic building blocks.



#### Diffusion and Confusion

**Diffusion**

The minimum number of bits affected in $c$ by changing one bit in $m$ over the total number of bits in $c$.

<img src="assets/9F222E5EDCA1319F910A442ACB87F718.png" alt="img" style="zoom:33%;" />

Linear functions usually provides diffusion.

**Confusion**

The complexity of the relations between the ciphertext-bit and the plaintext-bit and the key bits.

Nonlinear functions usually provides confusion.

With bad confusion (linear function for example), it is easy to solve $k$ given $(c,m)$.



#### The Iterative Design Paradigm

In order to let $E_k$ and $D_k$

- good in diffusion and confusion
- computationally fast

we could design a simple function $f_k$ and define
$$
E_k(m)=f_{k_{16}}(f_{k_{15}}( \cdots f_{k_2}(f_{k_1}(m)) \cdots ))
$$
where $k_1$, $k_2$, ... and $k_{16}$ are binary string computed from $k$ according to an algorithm.

Most ciphers are designed with this approach.



#### The Finite Field $GF(2^8)$

**Polynomials over $GF(2)$**

**Notation:** $GF(2)=Z_2$, only 0 and 1, operations $\oplus_2$ and $\otimes_2$.

**Polynomials over $GF(2)$:** $a(x)=a_0+a_1x+a_2x^2+ \cdots +a_nx^n$, where $a_i \in GF(2)$.

**Irreducible polynomial:** $p(x)=x^8+x^4+x^3+x+1 \in GF(2)[x]$, which means $p(x)$ cannot be expressed as the product of two polynomials over $GF(2)$ with smaller degrees, just like a prime.

**An reducible polynomial over $GF(2)$:** $x^4+x^3+x+1=(x+1)^2(x^2+x+1)$.

**The Elements in $GF(2^8)$**

All the polynomials that
$$
a(x)=a_0+a_1x+a_2x^2+a_3x^3+a_4x^4+a_5x^5+a_6x^6+a_7x^7 \in GF(2)[x]
$$
where $a_i \in {0,1}$. Hence $GF(2^8)$ has $2^8$ elements.

**The Addition Operation of $GF(2^8)$**

For any two elements
$$
a(x)=a_0+a_1x+a_2x^2+ \cdots +a_7x^7,\ \ b(x)=b_0+b_1x+b_2x^2+ \cdots +b_7x^7,
$$
their addition is defined by
$$
a(x)+b(x)= \sum_{i=0}^7 (a_i+b_i)x^i \in GF(2^8)
$$
**Proposition:** $(GF(2^8),+)$ is an abelian group with identity 0.

**The Multiplication Operation of $GF(2^8)$**
$$
a(x) \times b(x)=a(x)b(x)\ mod\ p(x) \in GF(2^8)
$$
where $p(x)=x^8+x^4+x^3+x+1$.

**Proposition:** $(GF(2^8),\times)$ is an abelian group with identity 1.

<img src="assets/9FEE4D2995AC39B5E3B2CDF19D55DA09.png" alt="img" style="zoom:33%;" />

**The Finite Field $GF(2^8)$**

**Proposition:** $(GF(2^8),+,\times)$ is a finite field with $2^8$ elements.

**Claim:** $S(y)=y^{2^8-2}=y^{254}$ is a permutation on $GF(2^8)$ and is highly nonlinear, and is employed in AES. Note that $S(y)=y^{-1}$ for all $y \neq 0$.

