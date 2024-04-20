1.
Question 1

Consider the following five events:

- [[]] Correctly guessing a random 128-bit AES key on the first try.

- [[]] Winning a lottery with 1 million contestants (the probability is $\frac{1}{10^6}$ ). 

- [[]] Winning a lottery with 1 million contestants 5 times in a row (the probability is $\frac{1}{10^6}^5$ ).

- [[]] Winning a lottery with 1 million contestants 6 times in a row. 

- [[]] Winning a lottery with 1 million contestants 7 times in a row. 

What is the order of these events from most likely to least likely?
1 point

- [(x)] 2, 3, 4, 1, 5

- [()] 2, 3, 1, 5, 4

- [()] 2, 3, 5, 4, 1

- [()] 3, 2, 5, 4, 1

#### Explaination

- Correctly guessing a random 128-bit AES key on the first try. So the probability is $\frac{1}{2^128}$

$$(\frac{1}{10^6})^7 < \frac{1}{2^{128}} < (\frac{1}{10^6})^6 < (\frac{1}{10^6})^5 \frac{1}{10^6}$$



2.
Question 2

Suppose that using commodity hardware it is possible to build a computer for about $200 that can brute force about 1 billion AES keys per second. 

Suppose an organization wants to run an exhaustive search for a single 128-bit AES key and was willing to spend 4 trillion dollars to buy these machines (this is more than the annual US federal budget). How long would it take the organization to brute force this single 128-bit AES key with these machines?  Ignore additional costs such as power and maintenance.
1 point

- [()] More than a billion (109109) years

- [()] More than a week but less than a month
 
- [(x)] More than a million years but less than a billion (109109) years

- [()] More than an hour but less than a day

- [()] More than a month but less than a year


#### Explaination

trillion = $10^12$

billion = $10^9$

=> We can bruteforce at most $\frac{4 \times 10^12}{200} \times 10^9 = 2 \times 10^10 \times 10^9 = 2\times 10^{19}$ AES keys / second

So number seconds we can find the keys is $\frac{\frac{2^128}{2 \times 10^{19}}}{86400} \appro 8$ days


3.
Question 3

Let F:{0,1}n×{0,1}n→{0,1}n be a secure PRF (i.e. a PRF where the key space, input space, and output space are all {0,1}n{0,1}n) and say n=128n=128.

Which of the following is a secure PRF (there is more than one correct answer):
1 point

- [[]] F′(k,x)=F(k,x)  ∥  0    

   (here ∥∥​ denotes concatenation) 

### Explainatinon: 

Not a PRF. A distinguisher will output not a random whenever the last bit of $F(k, 0^n)$ is 0

- [[]] $F′(k,x)=F(k, x)  ⨁  F(k, x⊕1^n)$

#### Explaination: 

Not a PRF. A distinguisher will query at $x = 0^n$ and $x = 1^n$ and output not random whenever the two responses are equal. This is unlikely to happen for a truly random function

- [[x]] $F′(k,x)=F(k,x)[0,…,n−2]$
(i.e., F′(k,x)F′(k,x) drops the last bit of F(k,x)F(k,x))

#### Explaination: 

Correct. A distinguisher for $F'$ gives a distinguisher for $F$. 

- [[]] $F′(k, x)=k \oplus x$

#### Explaination: 

Not a PRF. A distinguisher will querry at $x = 0^n$ and $x = 1^n$ and output <em>not random</em> if the xor or the response is $1^n$. This is unlikely to hold for a truly random function

- [[x]] $F′((k1,k2), x)=F(k1,x)  \oplus  F(k2,x)$ (here $\oplus$ denotes concatenation)

#### Explaination: 

A distinguisher for $F'$ gives a distinguisher for $F$. 

- [[]] $F′((k1,k2), x)=\begin{cases}F(k1,x) \quad \text{when} x \neq 0^n \\
k_2 \quad \text{otherwise}\end{cases}​$

#### Explaination


4.
Question 4

Recall that the Luby-Rackoff theorem discussed in The Data  Encryption Standard lecture

 states that applying a three round Feistel network to a secure PRF gives a secure block cipher.  Let's see what goes wrong if we only use a two round Feistel.

Let $F:K×{0,1}32→{0,1}32$ be a secure PRF.

Recall that a 2-round Feistel defines the following PRP 

F2:K2×{0,1}64→{0,1}64F2​:K2×{0,1}64→{0,1}64:

![alt text](image.png)






Here $R_0$​ is the right 32 bits of the 64-bit input and L0L0​ is the left 32 bits.

One of the following lines is the output of this PRP F2F2​ using a random key, while the other three are the output of a truly random permutation f:{0,1}64→{0,1}64f:{0,1}64→{0,1}64. All 64-bit outputs are encoded as 16 hex characters.

Can you say which is the output of the PRP?       Note that since you are able to distinguish the output of F2F2​ from random, F2F2​ is not a secure block cipher, which is what we wanted to show.

Hint: First argue that there is a detectable pattern in the xor of F2(⋅, 064)F2​(⋅, 064) and F2(⋅, 132032)F2​(⋅, 132032).   Then try to detect this pattern in the given outputs.
1 point

- [[]] On input $0^64$ the output is  "e86d2de2 e1387ae9".     

- [[]] On input $1^320^32$ the output is "1792d21d b645c008".

- [[]] On input $0^64$ the output is "5f67abaf 5210722b".     

- [[]] On input $1^320^32$ the output is "bbe033c0 0bc9330e".


### Sol: 
Ý tưởng là xem từng đáp án một, tìm đẳng thức giữa L0 và R2. R0 toàn 0 => L1 toàn 0 -> R2 = F(k2) của R1 , R1 = F(K1)

R2 và R2' là đảo bit của nhau, output ==> Check kiểm tra đảo bit của nhau từng cái output

Ví dụ: 

- 290b6e3a và d6f491c5 ta có 2 = 0010 => Đối của binary là 1101: d...

- 
Observe that two round Feister has the property that the left half of $F(., 0^64) \xor F(., 1^32 0^32)$ is 1^32. The two outputs in the answer are the only ones with this property
5.
Question 5

Nonce-based CBC.  Recall that in Lecture 4.4

 we said that if one wants to use CBC encryption with a non-random unique nonce then the nonce must first be encrypted with an independent PRP key and the result then used as the CBC IV.

Let's see what goes wrong if one encrypts the nonce with the same PRP key as the key used for CBC encryption. 

Let F:K×{0,1}ℓ→{0,1}ℓF:K×{0,1}ℓ→{0,1}ℓ be a secure PRP with, say, ℓ=128ℓ=128.  Let nn be a nonce and suppose one encrypts a message mm by first computing IV=F(k,n)IV=F(k,n) and then using this IV in CBC encryption using F(k,⋅)F(k,⋅).  Note that the same key kk is used for computing the IV and for CBC encryption.  We show that the resulting system is not nonce-based CPA secure.

The attacker begins by asking for the encryption of the two block message m=(0ℓ,0ℓ)m=(0ℓ,0ℓ) with nonce n=0ℓn=0ℓ.  It receives back a two block ciphertext (c0,c1)(c0​,c1​).  Observe that by definition of CBC we know that c1=F(k,c0)c1​=F(k,c0​). 

Next, the attacker asks for the encryption of the one block message m1=c0⨁c1m1​=c0​⨁c1​ with nonce n=c0n=c0​.  It receives back a one block ciphertext c0′c0′​.

What relation holds between c0,c1,c0′c0​,c1​,c0′​?    Note that this relation lets the adversary win the nonce-based CPA game with advantage 1.
1 point

- [] c0′=c0⨁1ℓ

- [] c0=c1⨁c0'

- [x] c1=c0′​
### Explaination: 

This follows from the definition of CBC with an encrypted nonce as defined in the question

- [] c1=c0​
6.
Question 6

Let mm be a message consisting of ℓℓ AES blocks

(say ℓ=100ℓ=100).  Alice encrypts mm using CBC mode and transmits

the resulting ciphertext to Bob.  Due to a network error,

ciphertext block number ℓ/2ℓ/2 is corrupted during transmission.

All other ciphertext blocks are transmitted and received correctly.

Once Bob decrypts the received ciphertext, how many plaintext blocks

will be corrupted?
1 point

- [x] 2

### Answer: 
Take a look at the CBC decryption circuit. Each ciphertext blocks affects only the current plaintext block and the next. 

- [] 0

- [] ℓℓ

- [] 3

- [] ℓ/2ℓ/2
7.
Question 7

Let mm be a message consisting of ℓℓ AES blocks (say ℓ=100ℓ=100).  Alice encrypts mm using randomized counter mode and

transmits the resulting ciphertext to Bob.  Due to a network error,

ciphertext block number ℓ/2ℓ/2 is corrupted during transmission.

All other ciphertext blocks are transmitted and received correctly.

Once Bob decrypts the received ciphertext, how many plaintext blocks

will be corrupted?
1 point

- [x] 1

### Explaination: 

Take a look at the counter mode decryption circuit. Each ciphertext block affects only the current plaintext block. 

- [] 0

- [] ℓ/2ℓ/2

- [] 2

 1+ℓ/21+ℓ/2
8.
Question 8

Recall that encryption systems do not fully hide the length of transmitted messages.  Leaking the length of web requests  hasbeen used  to eavesdrop on encrypted HTTPS traffic to a number of web sites, such as tax preparation sites, Google searches, and healthcare sites.

Suppose an attacker intercepts a packet where he knows that the packet payload is encrypted using AES in CBC mode with a random IV.  The encrypted packet payload is 128 bytes.  Which of the following messages is plausibly the decryption of the payload:
1 point

- [x] 93: In this letter I make some remarks on a general principle relevant to enciphering in general and my machine.'

### Explaination: 

The length of the string is 107 bytes, which after padding becomes 112 bytes, and after prepending the IV becomes 128 bytes. 

- [] 125: The significance of this general conjecture, assuming its truth, is

easy to see. It means that it may be feasible to design ciphers that

are effectively unbreakable.'

- [] 89: The most direct computation would be for the enemy to try 

 all 2^r possible keys, one by one.'

- [] 109: If qualified opinions incline to believe in the exponential

conjecture, then I think we cannot afford not to make use of it.'
9.

Bản mã là 128 bytes

- 109 thêmm 2IV nếu p padding tối đa thêm 16 bytes nữa => 125 bytes nhỏ hơn 128 bytes => padding thêm 3 bytes là 128 bytes

- 93 + 2 IV = 99

- 125 + 2IV = Vượt quá 128 bytes

- 89 
Question 9

Let R:={0,1}4R:={0,1}4 and consider the following PRF F:R5×R→RF:R5×R→R defined as follows:

F(k,x):={t=k[0] for i=1 to 4 doif (x[i−1]==1)t=t⊕k[i] output tF(k,x):=⎩⎪⎪⎪⎨⎪⎪⎪⎧​t=k[0] for i=1 to 4 doif (x[i−1]==1)t=t⊕k[i] output t​ 

That is, the key is k=(k[0],k[1],k[2],k[3],k[4])k=(k[0],k[1],k[2],k[3],k[4]) in R5R5 and the function at, for example, 01010101 is defined as F(k,0101)=k[0]⊕k[2]⊕k[4]F(k,0101)=k[0]⊕k[2]⊕k[4].

For a random key kk unknown to you, you learn that 

F(k,0110)=0011 F(k,0110)=0011  and  F(k,0101)=1010  F(k,0101)=1010  and  F(k,1110)=0110  F(k,1110)=0110 . 

What is the value of F(k,1101)F(k,1101)?        Note that since you are able to predict the function at a new point, this PRF is insecure.
1 point

### Answer: 

1111