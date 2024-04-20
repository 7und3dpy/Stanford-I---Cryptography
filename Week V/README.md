Question 1: 

Consider the toy key exchange protocol using an online trusted 3rd party (TTP) discussed in Lecture 9.1. Suppose Alice, Bob, and Carol are three users of this system (among many others) and each have a secret key with the TTP denoted $k_a, k_b, k_c$ respectively. They wish to generate a group session key $k_{ABC}$ that will be known to Alice, Bob and Carol but unknown toan eavesdropper. How would you modify the protocol in the lecture to accomodate a group keyexchange of this type? (note that all these protocols are insecure against active attacks)

- [()] Alice contacts the TTP. TTP generates a random $k_{ABC}$ and sends to Alice $$E(k_a,k_{ABC}),\text{ticket}_1 \Leftarrow k_{ABC},\qquad \text{ticket}_2 \Leftarrow k_{ABC}$$

Alice sends $\text{ticket}_1$ to Bob and $\text{ticket}_2$ to Carol.

- [()] Alice contacts the TTP. TTP generates random $k_{ABC}$ and sends to Alice 

$$E(k_a,k_{ABC}),\qquad \text{ticket}_1 \Leftarrow E(k_b,k_{ABC}), \qquad \text{ticket}_2 \Leftarrow E(k_c,k_{ABC})$$

.Alice sends $\text{ticket}_1$ to Bob and $\text{ticket}_2$ to Carol.

**Explaination**: 

First, Alice contacts the TTP.  At minimum, she must receive from the server
the shared key encrypted with her own private key, or $E(k_a, k_abc)$.

She must also not send the shared key in the clear.  Therefore the TTP should send Alice

$E(k_a, k_abc), ticket1 <- E(k_b, k_abc), ticket2 <- E(k_c, k_abc)$

Alice sends $\text{ticket}_1$ to Bob and $\text{ticket}_2$ to Carol.

- [()] Alice contacts the TTP. TTP generates a random $k_{ABC}$ and sends to Alice 
$$E(k_a,k_{ABC}),\qquad \text{ticket}_1 \Leftarrow E(k_b,k_{ABC}), \qquad \text{ticket}_2 \Leftarrow E(k_c,k_{ABC})$$ 

Alice sends $k_{ABC}$ to Bob and $k_{ABC}$ to Carol.


- [()] Alice contacts the TTP. TTP generates a random $k_{AB}$ and a random $k_{AC}$. It sends to Alice 

$$E(ka,kAB), \qquad \text{ticket}_1 \Leftarrow E(k_b,k_{AB}), \qquad \text{ticket}_2 \Leftarrow E(k_c,k_{AC})$$.

Alice sends $\text{ticket}_1$ to Bob and $\text{ticket}_2$ to Carol.

Question 2: 

Question 2

Let $G$ be a finite cyclic group (e.g. $G=Z_p^∗$​) with generator $g$.

Suppose the Diffie-Hellman function $DH_g(g^x,g^y)=g^{xy}$ is difficult to compute in $G$. Which of the following functions is also difficult to compute?

As usual, identify the $f$ below for which the contra-positive holds:  if f$(⋅,⋅)$ is easy to compute then so is $\text{DH}_g(⋅,⋅)$. If you can show that then it will follow that if $DH_g$​ is hard to compute in $G$ then so must be $f$.

- [[x]] $f(g_x,g_y) = g^{xy+1}

**Explaination**: 

This function _is difficult to compute_.  Suppose an attacker can compute f.
He could then compute DH with the equation

$$DH(g^x, g^y) = g^{xy} = f(g^x, g^y)/(g * g^x * g^y)$$


- [[x]] $f(g_x,g_y) = g^{x(y+1)}

**Explaination**: 

This function _is difficult to compute_.  Suppose an attacker can compute $f$. He could then compute DH with the equation.

$DH(g^x, g^y) = g^xy = f(g^x, g^y)/g^x$


- [[]] $f(g_x,g_y) = (g^2)^{x+y}

**Explaination**: 

This function _is not difficult to compute_. Specifically, an attacker can compute f by simply computing $g^{2x} * g^{2y} = g^{2(x+y)}$.

- [[]] $f(g_x,g_y) = (\sqrt{g})^{x+y}$

**Explaination**: 

This function _is not difficult_ to compute. Specifically, an attacker could
compute $f$ from $\sqrt(g^x * g^y) = f(g^x, g^y)$

Question 3: 

Suppose we modify the Diffie-Hellman protocol so that Alice operates as usual, namely chooses a random $a$ in ${1,...,p−1}$ and sends to Bob $A \Leftarrow g^a$. Bob, however, chooses a random $b$ in ${1,...,p−1}$ and sends to Alice $B \Leftarrow g^{1/b}$. What shared secret can they generate and howwould they do it?

- [()] $\text{secret}=g^{ab}$. Alice computes the secret as $B^a$ and Bob computes $A^b$.

- [(x)] $\text{secret}=g^{a/b}$. Alice computes the secret as $B_a$ and Bob computes $A_1/b$.

**Explaination**: 

Since Bob sends Alice g^(1/b), she has no way of computing g^ab.  The secret
key must therefore be g^(a/b).  They generate this key as B^a and A^(1/b).  In
other words:

_secret = g^(a/b).  Alice computes the secret as B^a and Bob computes A^(1/b).

- [()] $\text{secret}=g^{a/b}$. Alice computes the secret as $B^{1/b}$ and Bob computes $A^a$.

- [()] $\text{secret}=g^{ab}$. Alice computes the secret as $B^{1/a} and Bob computes $A^b$

Question 4: 

Consider the toy key exchange protocol using public key encryption described in Lecture 9.4. Suppose that when sending his reply $c \Leftarrow E(p_k,x)$ to Alice, Bob appends a MAC $t:=S(x,c)$ to the ciphertext so that what is sent to Alice is the pair $(c,t)$. Alice verifies the tag $t$ and rejects the message from Bob if the tag does not verify. Will this additional step prevent the man in the middle attack described in the lecture? 

- [()] it depends on what MAC system is used.

- [()] it depends on what public key encryption system is used.

- [()] yes

- [(x)] no

**Explaination**: 

The additional step _does not_ prevent the MitM attack.  An attacker can intercept the initial message from Alice to Bob, and replace $pk$ with $pk'$. He
sends $pk'$ to Bob, who responds with $(E(pk', x), S(x, E(pk', x)))$.  The attacker
then decrypts $E(pk', x)$ to determine $x$, computes $E(pk, x)$, and sends $(E(pk,x),S(x,E(pk,x)))$ to Alice. Alice sees that the MAC is correct and communicates insecurely with Bob.

Question 5

The numbers 7and 23 are relatively prime and therefore there must exist integers $a$ and $b$ such that $7a+23b = 1$. Find such a pair of integers $(a,b)$ with the smallest possible $a > 0$. Given this pair, can you determine the inverse of 7 in $\Z_{23}$ ?

**Explaination**: 

Using some simple python, we see that $(a,b) = (10,-3)$

In $Z_{23}, 7a + 23b = 7a$ and $1 = 1$.
So $7a = 1$, or _7^-1 = 10_


Question 6

Solve the equation $3x+2=7$ in $Z_{19}$.

**Explaination**: 
3x + 2 = 7
3x = 5

x = 5 * (3^-1) = 5 * 13 = 65 mod 19
= 8


Question 7

How many elements are there in $Z^∗_35$?

**Explaination**: 


We aim to find |Z*_35|
Since 35 = 7*5, |Z*_35| = 35 - 7 - 5 + 1 = 24

Question 8
How much is $2^10001 \mod 11$? (please do not use a calculator for this)

**Explaination**: 


We aim to find 2^(10001) mod 11
Femat's theorem says that x^(p-1) = 1 in Zp.

So consider the ring Z_11.
x^(10) = 1 in Z_11

We notice that 2^(10001) = 2*2^(10000) = 2*((((2^10)^10)^10)^10) = 2*1 =
2 in Z_11


Question 9

While we are at it, how much is $2^245 \mod 35$?*Hint*: use Euler’s theorem (you should not need a calculator)

**Explainaion**: 

We aim to compute 2^245 mod 35

Since 35 = 7*5, phi(35) = 24.

2^245 = 2^5 * 2^240 = 2^5 * (2^24)^10

From Euler's theorem, we see that 2^24 = 2^phi(35) = 1 in Z_35

So we have 2^245 = 2^5 * 1 =
32 in Z_35

Question 10

What is the order of 2 in $Z^∗_35$?

**Explaination**: 
We aim to compute ord_35(2)
By Lagranges theorem, ord_5(2) | 4 and ord_7(2) | 6.

So ord_35(2) | 24.
Clearly ord_2(35) > 5, so we try 6, 8, and 12.

2^6 = 64 = 29 in Z*_35
2^8 = 256 = 11 in Z*_35
2^12 = 4096 = 1 in Z*_35

So ord_2(35) = 12

Question 11

Which of the following numbers is a generator of $Z^∗_{13}?

1. $7, \qquad <7>={1,7,10,5,9,11,12,6,3,8,4,2}$

2. $5, \qquad <5>={1,5,12,8}$

3. $9, \qquad <9>={1,9,3}$

4. $2, \qquad <2>={1,2,4,8,3,6,12,11,9,5,10,7}$ 

5. $3, \qquad <3>={1,3,9}$

**Explaination**: 

The generated groups shown are not trickery.  Since 13 is prime, all postive
integers less than 13 are in Z*_13. So the generators are 2, 6, and 7.


Question 12

Solve the equation $x^2 + 4x + 1 = 0     \in \Z_23$. Use the method described in lecture 9.3 using the quadratic formula.

**Explaination**: 

We aim to find the solution to x^2 + 4x + 1 = 0 in Z_23

We use the formula

x = (-b +- sqrt(b^2 - 4ac))/2a
= (-b +- sqrt(b^2 -4ac) * (2a)^-1
= (-4 +- sqrt(16 - 4)) * 2^-1
= (-4 +- sqrt(12)) * 2^-1

The inverse of 2 in Z*_23 can be found easily since 2^-1 in Z_N for any odd N
is just (N+1)/2.  Here N=23, so 2^-1 = 12. Thus we have

= (-4 +- sqrt(12)) * 12

Now since 23 = 3 mod 4, sqrt(12) can be computed using the simple relation
sqrt(c) = c^((p+1)/4) in Zp where p is 23. So
sqrt(12) = 12^(24/4) mod 23 = 12^6 mod 23 = 9
So now we have

x = (-4 +- 9) * 12
= {5*12, -13*12} = {60, -156}
= {14, 5} in Z_23

We can verify easily:
14^2 + 4*14 + 1 = 253 = 23*11
5^2 + 4*5 + 1 = 46 = 23*2 


Question 13

What is the 11th root of 2 in $\Z_{19}$? (i.e. what is $2^{1/11}$ in $\Z_{19}$)

Hint: observe that $11^{−1} = 5$ in $Z_{18}$.

**Explaination**: 


We aim to find 2^(1/11) in Z_19.

We have a theorem that says when gcd(e, p-1) = 1, then c^(1/e) is c^(e^-1)
Fortunately, 11 and 18 are coprime.  That is, gcd(11, 18) = 1.

So 2^(1/11) = 2^(d), where d = the inverse of 11 in Z_18

The inverse of 11 in Z_18 can be found efficiently using euclid's algorithm.
We compute 11^-1 = 5

so the 11th root of 2 in Z_19 is 2^5
= 13 in Z_19.

Question 14

What is the discete log of 5 base 2 in $\Z_{13]$? (i.e. what is Dlog2(5))

Recall that the powers of 2 in $Z_{13}$ are 

<2> = {1,2,4,8,3,6,12,11,9,5,10,7}

**Explaination**: 

We aim to find $Dlog_2(5)$ in $Z_13$.

That is, we aim to find x so that $5 = 2^x \text{in} Z_13$.

From inspection of the generated group <2>, we see that $2^9 = 5$ in $Z_13$, so
$Dlog_2(5) = 9$ in $Z_13$.


Question 15

If *p* is a prime, how many generators are there in $Z^∗_p$ ?

- [()] $(p − 1)/2$

- [()] $p − 1$

- [()] $\phi(p)$

- [(x)] $\phi(p − 1)$

**Explaination**: 

Let p be a prime number.

An element g in $Z^*_p$ is a generator if |<g>| = |Z*_p|.
The set of generators of $Z^*_p$ is precisely the set {g : |<g>| = |Z*_p| = p-1}

TODO: Finish
phi(p-1)