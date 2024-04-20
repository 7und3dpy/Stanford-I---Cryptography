Question 1: 

Question 1

An attacker intercepts the following ciphertext (hex encoded): 

 20814804c1767293b99f1d9cab3bc3e7 ac1e37bfb15599e5f40eef805488281d

He knows that the plaintext is the ASCII encoding of the message "Pay Bob 100$" (excluding the quotes).    He also knows that the cipher used is CBC encryption with a random IV using AES as the underlying block cipher.

Show that the attacker can change the ciphertext so that it will decrypt to "Pay Bob 500$".  What is the resulting ciphertext (hex encoded)? 

 This shows that CBC provides no integrity.

**Explaination**: 

We receive the following ciphertext.

20814804c1767293b99f1d9cab3bc3e7 ac1e37bfb15599e5f40eef805488281d

The first 16 bytes are the IV.  We see that on decryption, the IV will be
XORed with the decryption of the first message block.  We simply identify the
position of the number '1' in "Pay Bob 100$",

20814804c1767293b99f1d9cab3bc3e7 
P A Y   B O B   1 0 0 $

then we replace the '1' with a '5' by XORing the correct positon with ord('1')
^ ord('5') = 0x04, where ord(x) is the ASCII encoding of x.

    20814804c1767293b99f1d9cab3bc3e7
xor 00000000000000000400000000000000
____________________________________
   =20814804c1767293bd9f1d9cab3bc3e7

So the resulting ciphertext is
_20814804c1767293bd9f1d9cab3bc3e7 ac1e37bfb15599e5f40eef805488281d_


Question 2: 

Let $(E,D)$ be an encryption system with key space $K$, message space ${0,1}^n$ and ciphertext space ${0,1}^s$. Suppose $(E, D)$ provides authenticated encryption. Which of the following systems provide authenticated encryption: (as usual, we use $\oplus$ to denote string concantenation)

- [[x]] $E′((k_1,k_2),m)=E(k_2,E(k_1,m)) \quad \text{and} \quad D′((k_1,k_2),c)=\begin{cases} D(k_1,D(k_2,c)) \quad \text{if} D(k_2,c) \neq \perp \\ \perp \quad \text{otherwise}\end{cases}$

**Explaination**: This system _does provide Authenticated encryption (AE)_.  Suppose an attacker could for a $c$ that does
not result in $D'$ output bottom.  Then the attacker could could use the
same strategy against $(E,D)$ with by encrypting outgoing messages with $k_2$ and
decrypting incoming messages with $k_2$.  Similarly, if the attacker could mount a CPA against $(E',D')$, he could use the same attack to mount a CPA against
$(E,D)$ by encrypting and decrypting with $k_2$ before sending or receiving data.

- [[]] $E′(k,m) =[c \Leftarrow E(k,m),\text{output}(c,c)] \quad \text{and} \quad D′(k,(c1,c2)) =\begin{cases}D(k,c_1) \quad \text{if} \quad c_1=c_2 \\
\prep \quad \text{otherwise}\end{cases}$

- [[]] $E′(k,m) = (E(k,m),0) \quad \text{and} D′(k,(c,b)) = D(k,c)$


**Explaination**: This system _does provide AE_.  An attacker who can mount a CPA or existential forgery on $(E',D')$ can use the same method on $(E,D)$.  He can send messages to $E$, pad them with 0.  Any ciphertext he submits must have $b==0$ or else $D'$ would have output bottom, so he can simply remove the appended zeros from his forgery from $D'$ to create a forgery for $D$. The process is similar for a CPA attack.


- [[]] $E′(k,m) =(E(k,m),E(k,m)) \quad \text{and} \quad D′(k,(c1,c2) ) =D(k,c1)$


**Ẽplaination**: 

This system _does not provide AE_.  An attacker can mount the following
integrity attack.  He first asks for the encryption of some arbitrary $m$, for
which he receives $(c, c)$.  He then sends the ciphertext $(c, ~c)$.  Since $(c, ~c)$ is
not equal to $(c, c)$ and $D'(k, (c, ~c)) = D(k, c) = m$ (not bottom), the
attacker has successfully forged a valid ciphertext.

Question 3: 

If you need to build an application that needs to encrypt multiple messages using a single key,what encryption method should you use? (for now, we ignore the question of key generationand management)

- [(x)] use a standard implementation of one of the authenticated encryption modes GCM, CCM,EAX or OCB.


**Explaination**: 

This question seems more like a reaffirmation of the lecture contents than a
real question.  Any response that includes "implement ... yourself" or "invent
your own" is clearly wrong.  Also, just using CBC encryption does not provide
message integrity.  Therefore the correct answer is to _use a standard
implementation of one of the authenticated encryption modes GCM, CCM, EAX, or
OCB_.

- [()] implement OCB by yourself

- [()] implement Encrypt-and-MAC yourself

- [()] use a standard implementation of randomized counter mode.

Question 4: 


Let $(E,D)$ be a symmetric encryption system with message space $M$ (think of $M$ as only consisting for short messages, say 32 bytes). Define the following MAC $(S,V)$ for messages in $M$:

$$S(k,m):=E(k,m);V(k,m,t):=\begin{cases}1 \quad \text{if} \quad D(k,t) = m \\
0 \quad \text{otherwise}\end{cases}$$

What is the property that the encryption system $(E,D)$ needs to satisfy for this MAC system to be secure?

- [()] semantic security under a chosen plaintext attack

- [(x)] authenticated encryption

**Explaination**: In order for $(E,D)$ to create a secure MAC, it must not be possible for an
attacker to create a forgery. Chosen ciphertext security isn't enough. Need Authenticated Encryption.

- [()] perfect secrecy

- [()] semantic security


Question 5: 

In lecture 8.1 we discussed how to derive session keys from a shared secret. The problem iswhat to do when the shared secret is non-uniform. In this question we show that using a PRF with a _non-uniform_ key may result in non-uniform values. This shows that session keys cannot be derived by directly using a _non-uniform_ secret as a key in a PRF. Instead, one has to use akey derivation function like HKDF.

Suppose $k$ is a _non-uniform_ secret key sampled from the key space ${0,1}^256$. In particular,$k$ is sampled uniformly from the set of all keys whose most significant 128 bits are all 0. In other words,$k$ is chosen uniformly from a small subset of the key space. More precisely,

$$\text{for all} \quad c \in {0,1}^256: \qquad Pr[k=c] =\begin{cases}1/2^128 \quad \text{if} MSB_{128}(c) =0^128\\0 \quad \text{otherwise}\end{cases}$$

Let $F(k,x)$ be a secure PRF with input space ${0,1}^256$. Which of the following is a secure PRF when the key $k$ is uniform in the key space ${0,1}^256$, but is insecure when the key is sampled from the _non-uniform_ distribution described above?

- [(x)] $F′(k,x) =\begin{cases}F(k,x) \quad \text{if} MSB_{128}(k) =0^128\\0^256 \quad \text{otherwise}\end{cases}$



- [()] $F′(k,x) =F(k,x)$

- [()] $F′(k,x) =\begin{cases}F(k,x) \quad \text{if} MSB_{128}(k) \neq 1^128\\0^256 \quad \text{otherwise}\end{cases}$

- [(x)] $F′(k,x) =\begin{cases}F(k,x) \quad \text{if} MSB_{128}(k) \neq 0^128\\1^256 \quad \text{otherwise}\end{cases}$

**Explaination**: 

When the key is sampled from the non-uniform distribution, it will always be
the case that $MSB_128(c) = 0^128$.  Any PRF that becomes non-random for inputs
with $MSB_128(c) = 0^128$ is insecure when taking inputs from the distribution.

The PRF $F′(k,x) =\begin{cases}F(k,x) \quad \text{if} MSB_{128}(k) \neq 0^128\\1^256 \quad \text{otherwise}\end{cases}$ will almost always
evaluate to $F(k,x)$, as long as $k$ is sampled uniformly over $K$.  However, when $k$
is sampled only from the subset of $K$ where the most significant 128 bits are
zero, $F'$ will always evaluate to $1^256$ and will thus be insecure.


F′(k,x) is a secure PRF because for a uniform key kk the

probability that MSB128(k)=0128MSB128​(k)=0128 is negligible.

However, for the *non-uniform* key kk this PRF always outputs 00

and is therefore completely insecure.    This PRF cannot be used as a

key derivation function for the distribution of keys described in the problem.

## Question 6: 

In what settings is it acceptable to use _deterministic_ authenticated encryption (DAE) like SIV?

- [()] to encrypt many records in a database with a single key when the same record may repeatmultiple times.

- [(x)] when messages have sufficient structure to guarantee that all messages to be encrypted are unique.

**Explaination**: 

DAE can be used whenever messages are very likely to be unique.  It shouldn't
be used if there is a chance that identical messages will be encrypted.  

- [()] when a fixed message is repeatedly encrypted using a single key.

- [()] to individually encrypt many packets in a voice conversation with a single key.

Question 7: 


Let $E(k,x)$ be a secure block cipher. Consider the following tweakable block cipher:
$$E′((k1,k2),t,x)=E(k1,x) \oplus E(k2,t)$$.

Is this tweakable block cipher secure?

- [()] yes, it is secure assuming $E$ is a secure block cipher.

- [()] no because for $t \neq t′$ we have $$E′((k1,k2),t,0) \oplus E′((k1,k2),t′,1) = E′((k1,k2),t′,1) \oplus E′((k1,k2),t′,0)$$

- [()] no because for $x \neq x′$ we have $$E′((k1,k2),0,x) \oplus E′((k1,k2),0,x) = E′((k1,k2),0,x′) \oplus E′((k1,k2),0,x′)$$

- [(x)] no because for $x \neq x′$ we have $$E′((k1,k2),0,x) \oplus E′((k1,k2),1,x) =E′((k1,k2),0,x′) \oplus E′((k1,k2),1,x′)$$

- [()] no because for $x \neq x′$ and $t \neq t′$ we have $$E′((k1,k2),t,x) \oplus E′((k1,k2),t′,x) =E′((k1,k2),t,x′) \oplus E′((k1,k2),t′,x)$$


**Explaination**: 

Let $E(k,x)$ be a secure block cipher and $E'((k1,k2),t,x) = E(k1,x) \oplus E(k2,t)$

A tweakable block cipher is insecure if an adversary can distinguish the cipher from a random permutation in the tweak space.

$E'$ is not secure.  An adversary can request the encryptions of 

a. [0](m1)     ->      $E(k1,m1) \oplus E(k2,0)$
b. [1](m1)     ->      $E(k1,m1) \oplus E(k2,1)$
c. [0](m2)     ->      $E(k1,m2) \oplus E(k2,0)$
d. [1](m2)     ->      $E(k1,m2) \oplus E(k2,1)$

He then computes

a ^ b = E(k2,0) ^ E(k2,1)
c ^ d = E(k2,0) ^ E(k2,1)

The attacker checks that a^b==c^d.  With very high probability, he will determine whether or not the challenger is using $E'$. Therefore his advantage
in the tweakable CPA game is ~1 and $E'$ is not secure.

Question 8: 


In lecture 8.5 we discussed format preserving encryption which is a PRP on a domain ${0,...,s−1}$ for some pre-specified value of $s$. Recall that the construction we presented worked in two steps, where the second step worked by iterating the PRP until the output fell into the set ${0,...,s−1}$. Suppose we try to build a format preserving credit card encryption system from AES using only the second step. That is, we start with a PRP with domain ${0,1}^128$ from which we wantto build a PRP with domain $10^16$. If we only used step (2), how many iterations of AES would be needed in expectation for each evaluation of the PRP with domain $10^16$?


- [()] $2^128$

- [()] $10^16/2^128$

- [(x)] $2^128/10^16≈3.4×10^22$

**Explaination**:

Each iteration is an independent, identically distributed bernoulli random
variable, so the number of steps in the whole algorithm is a geometric random
variable.  The probability of "success" for each trial is $p = 10^16/2^128$, so
the expected number of trials is 1/p = _2^128/10^16_.

- [()] 4


Question 9: 

Let $(E,D)$ be a secure tweakable block cipher. Define the following MAC$(S,V)$:

$$S(k,m):=E(k,m,0); \qquad V(k,m,tag):=\begin{cases}1 \quad \text{if}E(k,m,0) = \text{tag}\\0 \quad \text{otherwise}\end{cases}$$

In other words, the message $m$ is used as the tweak and the plaintext given to $E$ is always set to 0. Is this MAC secure?

- [(x)] yes

**Explaination**: 

The MAC _is secure_. Suppose an adversary could forge a message tag pair
$(m,t)$.  He could then mount attack $(E,D)$ by making the requests needed to find
$(m,t)$, then asking for the encryption of [m](0).  If $E(k, m, 0) == t$, then the
adversary knows that the challenger is using $E$ instead of a random function.

- [()] it depends on the tweakable block cipher.

- [()] no

Question 10:

In Lecture 7.6 we discussed padding oracle attacks. These chosen-ciphertext attacks can breakpoor implementations of MAC-then-encrypt. Consider a system that implements MAC-then-encrypt where encryption is done using CBC with a random IV using AES as the block cipher. Suppose the system is vulnerable to a padding oracle attack. An attacker intercepts a 64-byte ciphertext $c$ (the first 16 bytes of $c$ are the IV and the remaining 48 bytes are the encrypted payload). How many chosen ciphertext queries would the attacker need _in the worst case_ in order to decrypt the entire 48 byte payload? Recall that padding oracle attacks decrypt the payload one byte at a time.


- [(x)] 12288

**Explaination**: 

A padding oracle guesses all 256 possibilities of each byte's value for each byte.  In the worst case an attacker would ahve to make 256*48 = _12288_.

- [()] 256

- [()] 12240

- [()] 65536