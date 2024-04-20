Question 1: 

Recall that with symmetric ciphers it is possible to encrypt a 32-bit message and obtain a 32-bit ciphertext (e.g. with the one time pad or with a nonce-based system). Can the same be done witha public-key system?

[ ( ) ] Yes, when encrypting a short plaintext the output of the public-key encryption algorithm canbe truncated to the length of the plaintext.

[ ( ) ] It is not possible with the ElGamal system, but may be possible with other systems.

[ (x) ] No, public-key systems with short ciphertexts can never be secure.

**Explaination**: 

Ciphertexts that are the same length as their corresponding plaintext are not
MACed, so they cannot gaurentee authenticated encryption.  Therefore public-key
systems with "short" ciphertexts can never be secure.

[ ( ) ] It is possible and depends on the specifics of the system.


Question 2: 

Let (Gen,$E,D$) be a semantically secure public-key encryption system. Can algorithm $E$ be deterministic?

[ ( ) ] No, but chosen-ciphertext secure encryption can be deterministic.

[ ( ) ] Yes, RSA encryption is deterministic.

[ (x) ] No, semantically secure public-key encryption must be randomized.

[ ( ) ] Yes, some public-key encryption schemes are deterministic.


**Explaination**: 

Suppose $(G, E, D)$ is a deterministic public key encryption system.  An attacker can win the CPA game by submitting any message pair $(m_0, m_1)$ with $m_0 neq m1$ to receive challenge ciphertext $c$.  He then encrypts both $m_0$ and $m_1$ to get $c_0,c_1$. He then outputs $b' = 1$ if $c == 1$ else 0.  He wins the game with advantage 1.
He can use the same strategy to win the CCA game.

Therefore no deterministic public key encryption system can be semantically
secure.

Question 3: 

Let $(Gen,E,D)$ be a chosen ciphertext secure public-key encryption system with message space ${0,1}^128$. Which of the following is also chosen ciphertext secure?

[ [ ] ] $(Gen,E′,D′)$ where $E′(pk,m) =(E(pk,m),E(pk,m))$ and $D′(sk,(c1,c2))=D(sk,c1)$.

**Explaination**: 

Phase 1:
    Attacker sends nothing.
Challenge:
    Attacker picks any distinct m0, m1 with E(pk,m0) = c0 and E(pk,m1) = c1.
    Attacker sends (m0,m1) to receive (cb, cb)
Phase 2:
    Attacker sends (cb, ~cb) to receive D(sk, cb) == mb from which he determines
    b with advantage 1.
So the system is not CCA secure.


[[x]] $(Gen,E′,D′)$ where $E′(pk,m) =E(pk,m \oplus 1^128$ and $D′(sk,c) =D(sk,c)) \oplus 1^128$

**Explaination**: 

Suppose there is a CCA on this system. We can then attack (G,E,D) with the
following transformations:
Phases 1 and 2:
    ci -> ci
    mi -> ~mi
Challenge:
    (m0,m1) -> (~m0,~m1)
    E(pk,~mb) -> c (unchanged)
We then output b' directly to win the CCA game.
So the system is CCA secure.



[[ ]] $(Gen,E′,D′)$ where $E′(pk,m) =(E(pk,m),E(pk,0^128))$ and $D′(sk,(c1,c2))=D(sk,c1)$.

**Explaination**: 

Phase 1:
    Attacker sends nothing.
Challenge:
    Attacker picks any distinct m0, m1 with E(pk,m0) = c0 and E(pk,m1)=c1.
    Attacker sends (m0,m1) to receive (cb, 0^128)
Phase 2:
    Attacker sends (cb, 1^128) to receive D(sk, cb) from which he determines b
    with advantage 1.
So the system is not CCA secure.

[[x]] $(Gen,E′,D′)$ where $E′(pk,m) =[c←E(pk,m),output(c,c)]$ and $$D′(sk,(c1,c2))=\begin{cases}D(sk,c1) 
\quad \text{if} c1=c2\\⊥ \quad \text{otherwise}\end{cases}$$


**Explaination**: 
Suppose there is a CCA on this system. We can then attack (G,E,D) with the
following transformations:
Phase 1:
    ci -> (ci,ci)
    mi -> (mi,mi)
Challenge:
    (m0,m1) -> ((m0,m0),(m1,m1))
    E(pk,mb) -> (c,c) := (E(pk,mb), E(pk,mb))
Phase 2:
    ci -> (ci,ci)
    mi -> (mi,mi)
    Since ci != c, (ci,ci) != (c,c) 
We then output b' directly to win the CCA game.
So the system is CCA secure.



Question 4: 

Recall that an RSA public key consists of an RSA modulus $N$ and an exponent $e$. One might be tempted to use the same RSA modulus in different public keys. For example, Alice might use $(N,3)$ as her public key while Bob may use $(N,5)$ as his public key. Alice’s secret key is $d_a = 3^−1 \mod \phi(N)$ and Bob’s secret key is $d_b = 5^−1 \mod \phi(N)$. 

In this question and the next we will show that it is insecure for Alice and Bob to use the same modulus $N$. In particular, we show that either user can use their secret key to factor $N$. Alice can use the factorization to compute $\phi(N)$ and then compute Bob’s secret key. 

As a first step, show that Alice can use her public key $(N,3)$ and private key $d_a$ to construct an integer multiple of $\phi(N)$. Which of the following is an integer multiple of $\phi(N)$?

[( )] $3d_a$

[( )] $N+d_a$

[(x)] $3d_a−1$

**Explaination**: 

Alice aims to find an integer multiple of phi(N).  She knows that
3*da mod phi(N) = 1
3*da = k*phi(N) + 1
3*da - 1 = k*phi(N)

So (3*da - 1) is an integer multiple of phi(N).

[( )] $5d_a−1$



Question 5

Now that Alice has a multiple of $\phi(N)$ let’s see how she can factor $N=pq$. Let $x$ be the given multiple of $\phi(N)$. Then for any $g$ in $\Z^∗_N$ we have $g^x=1 in \Z_N$. Alice chooses a random $g$ in $\Z^∗_N$ and computes the sequence $g^x,g^{x/2},g^{x/4},g^{x/8}...in $\Z_N$ and stops as soon as she reaches the first element $y=g^{x/2^i} such that $y \neq 1$ (if she gets stuck because the exponent becomes odd, she picks a new random $g$ and tries again). It can be shown that with probability 1/2 this $y$ satisfies

$$\begin{cases}y=1 \mod p, \qquad and\\ y = −1 \mod q\end{cases} \qquad or \qquad \begin{cases} y = −1 \mod p, \text{and}\\ y = 1 \mod q\end{cases}$$

How can Alice use this $y$ to factor $N$?

[[ ]] compute gcd($N−1,y$)

[[x]] compute gcd($N,y−1$)

[[ ]] compute gcd($N+1,y$)
 
[[ ]] compute gcd($N,y^2$)

**Explaination**: 

Alice has found a value y with the property that 

{ y = 1 mod p = kp + 1
{ y = -1 mod q = lq - 1

or

{ y = -1 mod p = kp - 1
{ y = 1 mod q = lq + 1

Therefore $gcd(N, y-1)$ is either $p$ or $q$.  In either case, she can easily find the
factors of N by letting $p = gcd(N, y-1)$ and $q = N/p$.


Question 6: 

In standard RSA the modulus $N$ is a product of two distinct primes. Suppose we choose themodulus so that it is a product of three distinct primes, namely $N=pqr$. Given an exponent $e$ relatively prime to $\phi(N)$ we can derive the secret key as $d=e^−1 \mod \phi(N)$. The public key $(N,e)$ and secret key $(N,d)$ work as before. What is $\phi(N)$ when $N$ is a product of three distinct primes?

[(x)] $\phi(N) = (p−1)(q−1)(r−1)$

[( )] $\phi(N) = (p−1)(q−1)$

[( )] $\phi(N) = (p−1)(q−1)r$

[( )] $\phi(N) = (p−1)(q−1)(r+1)$

**Explaination**: 

The totient function is multiplicative, so
phi(N)
= phi(pqr)
= phi(p)phi(q)phi(r)
= (p-1)(q-1)(r-1)

Question 7: 

An administrator comes up with the following key management scheme: he generates an RSA modulus $N$ and an element $s$ in $\Z^∗_N$. He then gives user number $i$ the secret keys $s_i=s^{r_i}$ in $\Z_N$ where $r_i$ is the $i$’th prime (i.e. 2 is the first prime, 3 is the second, and so on).

Now, the administrator encrypts a file that is accssible to users $i,j$ and $t$ with the key $k=s^{r_ir_jr_t} in $\Z_N$. It is easy to see that each of the three users can compute $k$. For example, user $i$ computes $k$ as $k= (s_i)^{r_jr_t}. The administrator hopes that other than user $s_i,j$ and $t$, no other user can computekand access the file. Unfortunately, this system is terribly insecure. Any two colluding users can combine their secret keys to recover the master secrets and then access all files on the system. Let’s see how. Suppose users 1 and 2 collude. Because $r_1$ and $r_2$ are distinct primes there are integers $a$ and $b$ such that $ar_1+br_2=1$. Now, users 1 and 2 can computes from the secret keys $s_1$ and $s_2$ as follows:

[( )] $s=s_1^a \cdot s_2^b$ in $\Z_N$.

[( )] $s=s_1^b \cdot s_2^a$ in $\Z_N$.

[( )] $s=s_1^b + s_2^a$ in $\Z_N$.

[( )] $s=s_2^a$ in $\Z_N$

**Explaination**: 

We can compute $s_1^a * s_2^b = s^(ar_1) * s^(br_2) = s^(ar_1 + br_2) = s^1 = s$

Question 8

Let $G$ be a finite cyclic group of order $n$ and consider the following variant of ElGamal encryption in $G$:

- Gen: choose a random generator $g$ in $G$ and a random $x$ in $\Z_n$. Output pk= $(g,h = g^x)$ and sk = $(g,x)$.

- $E(pk, m \in G)$: choose a random $r$ in $\Z_n$ and output $(g^r,m \cdot h_r)$.

- $D(sk,(c0,c1))$: output $c_1/c^x_0$.

This variant, called plain ElGamal, can be shown to be semantically secure under an appropriateassumption aboutG. It is however not chosen-ciphertext secure because it is easy to computeon ciphertexts. That is, let $(c0,c1)$ be the output of $E(pk,m_0)$ and let $(c2,c3)$ be the output of $E(pk,m1)$. Then just given these two ciphertexts it is easy to construct the encryption of $m_0 \cdot m_1$ as follows:

[( )] $(c_0c_3,c_1c_2)$ is an encryption of $m0 \cdot m1$.

[(x)] $(c_0c_2,c_1c_3)$ is an encryption of of $m_0 \cdot m_1$.

[( )] $(c_0 + c_2, c_1 + c_3)$ is an encryption of $m_0 \cdot m_1$.

[( )] $(c_0/c_3,c_1/c_2)$ is an encryption of $m_0 \cdot m$




**Explaination**: 

The attacker asks for the encryption of m0 and m1, which are
E(pk,m0) = (c0,c1) = (g^r, m0*g^xr) for random r

and

E(pk,m1) = (c2,c3) = (g^r, m1*g^xr) for random r

He wishes to compute
E(pk,m0*m1) = (g^r, m0*m1*g^xr) for random r

He can compute this value by picking a random r in Zn, then computing
(c0*c2, c1*c3) = (g^(r1+r2), m0*m1*h^(r1+r2))

Question 9

Let $G$ be a finite cyclic group of ordernand let $pk= (g,h=ga)$ and $sk= (g,a)$ be an ElGamal public/secret key pair in $G$ as described in Segment 12.1. Suppose we want to distribute thesecret key to two parties so that both parties are needed to decrypt. Moreover, during decryptionthe secret key is never reconstructed in a single location. A simple way to do so it to choose random numbers $a_1,a_2$ in $\Z_n$ such that $a_1+a_2 = a$. One party is given $a_1$ and the other party is given $a_2$. Now, to decrypt an ElGamal ciphertext $(u,c)$ we send $u$ to both parties. What do the two parties return and how do we use these values to decrypt?

[( )] party 1 returns $u_1 \Leftarrow u^{−a_1}, party 2 returns $u_2 \Leftarrow u^{−a_2}$ and the results are combined by computing $v \Leftarrow u_1 \cdot u_2$.

[( )] party 1 returns $u_1 \Leftarrow u^{a_1}$, party 2 returns $u_2 \Leftarrow u^{a_2}$ and the results are combined by computing $v \Leftarrow u_1 +u_2$.

[( )] party 1 returns $u1 \Leftarrow u^(a^2_1)$, party 2 returns $u_2 \Leftarrow u^(a^2_2)$ and the results are combined by computing $v \Leftarrow u_1 \cdot u_2$.

[(x)] party 1 returns $u1 \Leftarrow u^{a_1}$, party 2 returns $u_2 \Leftarrow u^{a_2}$ and the results are combined by computing $v \Leftarrow u_1 \cdot u_2$.

**Explaination**: 

If $a1 + a2 = a, then we can can share the computation of v by recognizing that
v = u^a = u^(a1 + a2) = u^a1 * u^a2.

So party 1 computes $u_1 = u^{a_1}$, party 2 computes $u_2 = u^{a_2}$, and the results are
combined using v = u1 * u2.

Question 10

Suppose Alice and Bob live in a country with 50 states. Alice is currently in state $a \in {1,...,50}$ and Bob is currently in state $b \in {1,...,50}$. They can communicate with one another and Alice wants to test if she is currently in the same state as Bob. If they are in the same state, Alice should learn that fact and otherwise she should learn nothing else about Bob’s location. Bob should learn nothing about Alice’s location.They agree on the following scheme:

- They fix a group $G$ of prime orderpand generator $g$ of $G$ 

- Alice chooses random $x$ and $y$ in$\Z_p$ and sends to $Bob(A_0,A_1,A_2) = (g^x,g^y,g^{xy+a})$

- Bob choose random $r$ and $s$ in $\Z_p$ and sends back to Alice $(B1,B2) =(A^r_1g^s,(A_2/g^b)^rA^s_0)

What should Alice do now to test if they are in the same state (i.e. to test if $a=b$) ? 

Note that Bob learns nothing from this protocol because he simply recieved a plain ElGamal encryption of $g^a$ under the public key $g^x$. One can show that if $a \neq b$ then Alice learns nothing else from this protocol because she recieves the encryption of a random value.

[( )] Alice tests if $a=b$ by checking if $B_1/B^x_2=1$.

[( )] Alice tests if $a=b$ by checking if $B_2B^x_1=1$.

[(x)] Alice tests if $a=b$ by checking if $B_2/B^x_1=1$.

[( )] Alice tests if $a=b$ by checking if $B^x_1B_2=1$.


**Explaination**: 

Since $B_2 = (A_2/g^b)^r * A_0^s$ and $A_2 = g^(xy + a)$,

$B_1 = (A_1^r * g^s) = g^(yr+s)$ 
$B_2 = (A_2/g^b)^r * A_0^s = g^(r(xy+a-b) + sx)$

So considering the exponents
B1^x = rxy + sx
B2 = rxy + (ra - rb) + sx

If a == b then 
$B_2/B_1^x = rxy + (0) + sx - rxy - sx = 0$ in the exponent

So she checks that $B_2/B_1^x  == 1$

Question 11

What is the bound ondfor Wiener’s attack when $N$ is a product of three equal size distinct primes?

[(x)] $d < N^{1/6}/c$ for some constant c.

[( )] $d < N^{1/4}/c$ for some constant c.

[( )] $d < $N^{1/2}/c$ for some constant c.

[( )] $d < N^{2/3}/c$ for some constant c.$

**Explaination**: 

In the proof of the bounds on Wiener's attack where $N=pq$, we found that 
$|N - \phi(N)| <= p + q <= 3 sqrt(N)$

When $N$ is product of 3 primes p,q,r we see that
$\phi(N) = pqr - pq - pr - qr + p + q + r - 1
|N - \phi(N)| <= 4*N^(2/3)$

Nothing else in the proof changes, so we require d <= $N^(1/6)/c$