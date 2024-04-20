Question 1: 

Recall that with symmetric ciphers it is possible to encrypt a 32-bit message and obtain a 32-bit ciphertext (e.g. with the one time pad or with a nonce-based system). Can the same be done witha public-key system?

- [()] Yes, when encrypting a short plaintext the output of the public-key encryption algorithm canbe truncated to the length of the plaintext.

- [()] It is not possible with the ElGamal system, but may be possible with other systems.

- [(x)] No, public-key systems with short ciphertexts can never be secure.

**Explaination**: 

Ciphertexts that are the same length as their corresponding plaintext are not
MACed, so they cannot gaurentee authenticated encryption.  Therefore public-key
systems with "short" ciphertexts can never be secure.

- [()] It is possible and depends on the specifics of the system.


Question 2: 

Let (Gen,$E,D$) be a semantically secure public-key encryption system. Can algorithm $E$ be deterministic?

- [()] No, but chosen-ciphertext secure encryption can be deterministic.

- [()] Yes, RSA encryption is deterministic.

- [()] No, semantically secure public-key encryption must be randomized.

- [()] Yes, some public-key encryption schemes are deterministic.


**Explaination**: 

Suppose $(G, E, D)$ is a deterministic public key encryption system.  An attacker can win the CPA game by submitting any message pair $(m_0, m_1)$ with $m_0 neq m1$ to receive challenge ciphertext $c$.  He then encrypts both $m_0$ and $m_1$ to get $c_0,c_1$. He then outputs $b' = 1$ if $c == 1$ else 0.  He wins the game with advantage 1.
He can use the same strategy to win the CCA game.

Therefore no deterministic public key encryption system can be semantically
secure.

Question 3: 

Let $(Gen,E,D)$ be a chosen ciphertext secure public-key encryption system with message space ${0,1}^128$. Which of the following is also chosen ciphertext secure?

- [[]] $(Gen,E′,D′)$ where $E′(pk,m) =(E(pk,m),E(pk,m))$ and $D′(sk,(c1,c2))=D(sk,c1)$.

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


- [[]] $(Gen,E′,D′)$ where $E′(pk,m) =E(pk,m \oplus 1^128$ and $D′(sk,c) =D(sk,c)) \oplus 1^128$

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



- [[x]] $(Gen,E′,D′)$ where $E′(pk,m) =(E(pk,m),E(pk,0^128))$ and $D′(sk,(c1,c2))=D(sk,c1)$.

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

- [[x]] $(Gen,E′,D′)$ where $E′(pk,m) =[c←E(pk,m),output(c,c)]$ and $$D′(sk,(c1,c2))=\begin{cases}D(sk,c1) 
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