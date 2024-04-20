Question 1: 

Let $(E, D)$ be an authenticated encryption system built by combining a CPA-secure sym-metric cipher and a MAC. The system is combined with an error-correction code tocorrect random transmission errors. In what order should encryption and error correctionbe applied?

- [()] Apply the error correction code and then encrypt the result.

- [(x)] Encrypt and then apply the error correction code.

**Explaination**: Error correcting codes are designed to be tolerant against random changes during transmission.  If the ECC encoding is permuted randomly by an ncryption function, it won't work correctly.

Even more importantly, decryption needs an error free input to work effectively.  If a system tries to decrypt an erroneous ciphertext, the resulting pre-ECC plaintext will be meaningless.

Therefore you should encrypt and then apply the ECC.

- [()] The order does not matter – neither one can correct errors.

- [()] The order does not matter – either one is fine.

Question 2: 

Let $X$ be a uniform random variable over the set ${0,1}^n$. Let $Y$ be an arbitrary randomvariable over the set ${0,1}^n$ (not necessarily uniform) that is independent of $X$. Define the random variable $Z=X \oplus Y$. What is the probability that $Z$ equals $0^n$

- [()] 1

- [(x)] $1/2^n$

**Explaination**: 

For any bit $X[i]$, we have

$$P(X[i] == 0) = p = 1/2$$

so 

$
\begin{align*}
&P(X[i] \oplus Y[i] == 0)\\
&= P(X[i] == 0 \quad \text{and} \quad Y[i] == 0 \quad \text{or} \quad X[i] == 1 \quad \text{and} \quad Y[i] == 1)\\
&= P(X[i] == 0 \quad \text{and} \quad Y[i] == 0) + P(X[i] == 1 \quad \text{and} \quad Y[i] == 1)\\
&= p * P(Y[i] == 0) + (1 - p) * P(Y[i] == 0)
\end{align*}
$

Letting 
$
\begin{align*}
&q(i, ...) = q = P(Y[i] == 0)\\
&= pq + (1 - p) * (1-q)\\
&= pq + 1 - q - p + pq\\
&= 1+ q - q - 1/2\\
&= 1/2 
\end{align*}
$

Since $i$ was atrbitrary, the result $Z = X ^ Y$ is a uniform distribution over ${0,1}^n$

So $0^n$ occurs with probability $1/2^n$


- [()] $2/2^n$

- [()] $0.5$

Question 3: 

Suppose $(E_1, D_1)$ is a symmetric cipher that uses 128 bit keys to encrypt 1024 bit messages .Suppose $(E_2, D_2)$ is a symmetric cipher that uses 128 bit keys to encrypt 128 bit messages. The encryption algorithms $E_1$ and $E_2$ are deterministic and do not use nonces. Which of the following statements is true?

- [[]] $(E_2, D_2)$ can be perfectly secure, but cannot be one-time semantically secure.
- [[]] $(E_1, D_1)$ can be semantically secure under a chosen plaintext attack.
- [[x]] $(E_1, D_1)$ can be one-time semantically secure, but cannot be perfectly secure.

**Explaination**: Since $(E_1,D_1)$ is deterministic, it cannot offer many time semantic security.

However, it can offer one time semantic security. Since the key space is
smaller than the message space, the cipher cannot be perfectly secure.


- [[x]] $(E_2, D_2)$ can be one-time semantically secure and perfectly secure.

**Explaination**: On the other hand, $(E_2,D_2)$ can be both one time semantically secure and perfectly secure since the key space and message space are the same size.

Question 4: 

Which of the following statements regarding CBC and counter mode is correct?

- [()] Both counter mode and CBC mode can operate just using a PRF.

- [()] Both counter mode and CBC mode require a block cipher (PRP).

- [(x)] CBC mode encryption requires a block cipher (PRP), but counter mode encryption only needs a PRF.

**Explaination**: CBC mode requires the block cipher be reversible, so it must be a PRP.  On the other hand, CTR mode can be configured to only use the encryption directly of the block cipher, so it will work with just a PRF.

- [()] counter mode encryption requires a block cipher (PRP), but CBC mode encryptiononly needs a PRF.

Question 5: 


Let $G:X \Rightarrow X^2$ be a secure PRG where $X={0,1}^256$. We let $G(k)[0]$ denote the lefthalf of the output and $G(k)[1]$ denote the right half. Which of the following statementsis true?

- [()] $F(k, m) =G(k)[0] \oplus m$ is a secure PRF with key space and message space $X$.




Gửi F(k)[m] = 0

Gửi F(k, m) và F(k, m') thì G(k)[0]

- PRG: k luôn dùng 1 lần
PRG: G(k) nhìn như ngẫu nhiên

Trong định nghĩa semantic security, Stream cipher chỉ dùng k 1 lần

An toàn PDF: 

Input m: F(m_1) ngẫu nhiên, F(m_2) ra cái ngẫu nhiên khác

m đc giữ bí mật bởi khóa k, số lượng chỉ tương đương khóa k => ko nhiều như TH trên

=> Khóa k được sử dụng nhiều lần để test khả năng 

- PRPs: 


Secure PRFs

Có 2TH 

+ (2^128)^{2^128}

+ 1^{2^128}

- [()] $F(k, m) =G(m)[0] \oplus k$ is a secure PRF with key space and message space $X$.

**Explaination**: 

Gửi m sau đó gửi m' với m' là đảo bit của m


- [()] $F(k, m) =m \oplus k$ is a secure PRF with key space and message space $X$.

- [(x)] $F(k, m) =G(k)[m]$ is a secure PRF with key space $X$ and message space $m \in {0,1}$

**Explaination**: The function $G(k)[m]$ with $m \in {0,1}$ is a secure PRF whenever $G$ is a secure PRG. The other options are insecure because they are clearly distinguishable from random (change a single bit in the $m$ and see what happens to the output.)

Question 6: 


Let $(E, D)$ be a nonce-based symmetric encryption system (i.e. algorithm $E$ takes as input a key, a message, and a nonce, and similarly the decryption algorithm takes a nonceas one of its inputs). The system provides chosen plaintext security (CPA-security) as long as the nonce never repeats. Suppose a single encryption key is used to encrypt $2^32$ messages and the nonces are generated independently at random for each encryption, how long should the nonce be to ensure that it never repeats with high probability?

- [()] 32 bits

- [()] 16 bits

- [()] 48 bits

- [(x)] 128 bits

**Explaination**: 

If the same key is used to encrypt $2^32$ messages, we need to have at least $(2^32)^2$ or $2^64$ distinct, uniformly sampled nonces in order to have a $< 1/2$ probability of not having a nonce collision.  So the nonce space should be $2^128$.

Question 7: 

Same as question 6 except that now the nonce is generated using a counter. The counterresets to 0 when a new key is chosen and is incremented by 1 after every encryption. What is the shortest nonce possible to ensure that the nonce does not repeat when encrypting $2^32$ messages using a single key?

- [()] the nonce must be chosen at random, otherwise the system cannot be CPA secure.

- [()] 16 bits

- [(x)] 32 bits

**Explaination**: Since the nonces will not collide until the entire nonce space has been used, it is acceptable to use a nonce space the same size as the number of messages that are expected to be sent, or just $2^32$ nonces.

- [()] 48 bits

Question 8: 

Let (S, V) be a deterministic MAC system with message spaceMand key space $K$. Which of the following properties is implied by the standard MAC security definition?

- [(x)] For any two distinct messages $m_0$ and $m_1$, given $m_0$, $m_1$ and $S(k, m_0)$ it is difficult to compute $S(k, m_1)$.

**Explaination*: That a MAC is secure implies that an attacker cannot create any forgery. This implies the weaker version of security that says an attacker cannot forge the
message $S(k,m_1)$ given $m_0$, $m_1$, and $S(k,m_0)$.

- [()] $S(k, m)$ preserves semantic security of $m$. That is, the adversary learns nothing about $m$ given $S(k, m)$.

- [()] Given a key $k$ in $K$ it is difficult to find distinct messages $m_0$ and $m_1$ such that $S(k, m_0) = S(k, m_1)$.

- [()] The function $S(k, m)$ is a secure PRF.

Question 9: 

Let $H: M \Rightarrow T$ be a collision resistant hash function where $|T|$ is smaller than $|M|$ .Which of the following properties is implied by collision resistance?

- [()] H(m) preserves semantic security ofm(that is, given $H(m)$ the attacker learns nothing about $m$).

- [()] it is difficult to find $m_0$ and $m_1$ such that $H(m_0) =H(m_1) + 1$. (here we treat the outputs of $H$ as integers)

- [(x)] It is difficult to construct two distinct messages $m_0$ and $m_1$ such that $H(m_0) = H(m_1)$.

**Explaination**: Collision Resistance implies that it is difficult to find any

$m_0, m_1$ such that $H(m_0) = H(m_1)$

- [()] For all $m$ in $M$, $H(m)$ must be shorter than $m$. 

Question 10: 

Recall that when encrypting data you should typically use a symmetric encryption system that provides authenticated encryption. Let $(E, D)$ be a symmetric encryption system providing authenticated encryption. Which of the following statements is implied by authenticated encryption?

- [[x]] Given $m$ and $E(k, m)$ it is difficult to find $k$.

- [[]] Given $m$ and $E(k, m)$ the attacker cannot create a valid encryption of $m + 1$. (here we treat plaintexts as integers)

- [[]] The attacker cannot create a ciphertext $c$ such that $D(k, c) =⊥$.

- [[x]] Given $c=E(k, m)$ for some secret $k$ , $m$, the attacker cannot find $k′$, $m′$ such that $c=E(k′, m′)$.

**Explaination**: Authenticated Encryption implies many weaker security definitions. For instance, it implies:

True: 
> Given $m$ and $E(k,m)$ it is difficult to find k (weak semantic security) 

> $(E,D)$ provides chosen-ciphertext security.

False

> Given $c = E(k,m)$ for some $k$, $m$, the attacker cannot find $k'$,$m'$ such that $c=E(k',m')$.  That would imply some notion of "collision resistance" between messages encrypted with different keys. 

> Given $k,m$ and $E(k,m)$ the attacker cannot create a valid encryption of $m+1$ under key $k$. 


Question 11: 

Which of the following statements is true about the basic Diffie-Hellman key-exchange protocol.

- [[x]] The basic protocol enables key exchange secure against eavesdropping, but is insecure against active adversaries that can inject and modify messages.

- [[]] The protocol is based on the concept of a trapdoor function.

- [[]] The basic protocol provides key exchange secure against active adversaries that can inject and modify messages.

- [[x]] The protocol can be converted to a public-key encryption system called the ElGamal public-key system.

**Explaination**: 

The following are true of DH key exchange:
> The basic protocol enables key exchange secure against eavesdropping, but is insecure against active adversaries that can inject and modify messages.

> The protocol provides security against eavesdropping in any finite group in which the Hash Diffie-Hellman (HDH) assumption holds.

The following are not true of DH:
> The protocol is based on the concept of a trapdoor function. (Even though the protocol relies on a one-way function, there is no trapdoor to the discrete log problem.)

> As with RSA, the protocol only provides eavesdropping security in the group $Z*N$ where $N$ is an RSA modulus. (RSA provides eavesdropping security in other groups such as the elliptic curve groups, as does $DH$.)


Question 12: 

Suppose $n+ 1$ parties, call them $B, A_1, ... , A_n$, wish to setup a shared group key. They want a protocol so that at the end of the protocol they all have a common secret key $k$, but an eavesdropper who sees the entire conversation cannot determine $k$. The parties agree on the following protocol that runs in a group $G$ of prime order $q$ with generator $g$:

- for $i= 1, ... , n$ party $A_i$ chooses a random $a_i$ in ${1, ..., q}$and sends to Party $B$ the quantity $Xi \Leftarrow g^{a_i}$.

- Party $B$ generates a random $b$ in ${1, ... , q}$ and for $i= 1, ... ,n$ responds to Party $A_i$ with the messages $Yi \Leftarrow X_b^i$.

The final group key should be $g^b$. Clearly Party $B$ can compute this group key. How would each Party$A_i$ compute this group key?

- [(x)] Party $A_i$ computes $g^b$ as $Y_i^{1/a_i}$


- [()] Party $A_i$ computes $g^b$ as $Y_i^{−a_i}

- [()] Party $A_i$ computes $g^b$ as $Y_i^{a_i}

- [()] Party $A_i$ computes $g^b$ as $Y_i^{-1/a_i}

- [()] Party $A_i$ computes $g^b$ as $Y_i^{−1/a_i}

**Explaination**: Each party $A_i$ has access to the values ai and of course $X_i = g^a_i$ and $Y_i =
X_i^b = g^{ba_i}$.  $A_i$ can therefore compute $g^b$ as
$(g^{ba_i}) ^ 1/a^i = X_i^b ^ 1/a_i = Y_i^{1/a_i}$


Question 13: 

Recall that the RSA trapdoor permutation is defined in the group $\Z^∗_N$ where $N$ is aproduct of two large primes. The public key is $(N, e)$ and the private key is $(N, d)$ where $d$ is the inverse of $e$ in $Z∗\phi(N)$. Suppose RSA was defined modulo a prime $p$ instead of an RSA composite $N$. Show that in that case anyone can compute the private key $(N, d)$ from the public key $(N, e)$ by computing:

- [()] $d \Leftarrow e^{−1} (mod p+ 1)$.

- [()] $d \Leftarrow e^{−1} (mod p^2)$.

- [(x)] $d \Leftarrow e^{−1} (mod p − 1)$.

- [()] $d \Leftarrow e^2 (mod p)$ 


**Explaination**: 

For any prime p, totient(p) = p-1, so d is the inverse of e in the group
Z*_{p-1}, or

d <- e^-1 (mod p-1)