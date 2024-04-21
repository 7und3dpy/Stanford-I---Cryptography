Question 1

Suppose a MAC system (S,V)(S,V) is used to protect files in a file system by appending a MAC tag to each file.     The MAC signing algorithm $S$ is applied to the file contents and nothing else. What tampering attacks are not prevented by this system?


- [()] Changing the name of a file.

- [()] Changing the first byte of the file contents. 

- [(x)] Appending data to a file.

**Explaination**: The MAC signing algorithm is only applied to the file contents and does not protect to the file meta data. 

- [()] Replacing the contents of a file with the concatenation of two files on the file system.  

Question 2

Let $(S, V)$ be a secure MAC defined over $(K, M, T)$ where $M = {0,1}^n$ and $T = {0,1}^128$. That is, the key space is $K$, and tag space is ${0,1}^128$. 

Which of the following is a secure MAC: (as usual, we use || to denote string concatenation)

- [[x]] $S'(k, m) = [t \leftarrow S(k, m), \ \text{output}(t, t)]$ \ and \ $V'(k,m,(t_1, t_2)) = \begin{cases} V(k,m,t_1) \quad \text{if} \ t_1 = t_2\\
"0"  \quad \text{otherwise}\end{cases}$

(i.e, $V'(k,m,(t_1,t_2))$ only outputs "1" if $t_1$ and $t_2$ are equal and valid)

**Explaination**: a forger for ($S', V'$) gives a forger for $(S,V)$

- [[]] $S'(k,m) = S(k, m \oplus m) \ \text{and} \ V'(k, m, t) = V(k, m \oplus m, t)$

**Explaination**: 

- [[]] $S'(k,m) = (S(k,m), S(k, 0^n))$ and $V'(k,m,(t_1,t_2)) = [V(k,m,t_1) \ \text{and} \ V(k,0^n,t_2)]$

(i.e, $V'(k,m,(t_1,t_2))$ outputs "1" if both $t_1$ and $t_2$ are valid tags)

**Explaination**: This construction is insecure because the adversary can query for the tag of the message $1^n$ and then obtain a valid tag for the message $0^n$. The adversary can then output an existential forgery for the message $0^n$. 

- [[]] $S'(k,m) = S(k,m[0,...,n - 2] || 0)$ and $V'(k,m,t) = V(k, m[0,...,n-2 || 0,t])$

**Explaination**: This construction is insecure because the tag on $m = 0^n$ and $m = 0^{n-1}1 are the same. Consequently, the attacker can request the tag on $m = 0^n$ and output an existential forgery for $m = 0^{n-1}1$. 

- [[x]] $S'(k,m) = S(k, m \oplus 1^n) \quad \text{and} \quad V'(k,m,t) = V(k, m \oplus 1^n, t)$

**Explaination**: a forger for ($S', V'$) gives a forger $(S,V)$

- [[x]] $S'(k,m) = S(k,m || m)$ and $V'(k,m,t) = V(k,m || m, t)$

**Explaination**: a forger for $(S', V')$ gives a forger for $(S,V)$

Question 3: 

Recall that the ECBC-MAC uses a fixed IV (in the lecture we simply set the IV to 0). Suppose instead we chose a random IV for every message being signed and include the IV in the tag. In other words, $S(k,m):=(r,\text{ECBC}_r(k,m))$ where $text{ECBC}_r(k,m)$ refers to the ECBC function using $r$ as the IV. The verification algorithm $V$ given key $k$, message $m$, and tag $(r,t)$ output "1" if $t = \text{ECBC}_r(k,m)$ and outputs "0" otherwise. The resulting MAC system is insecure. An attacker can querry for the tag of the 1-block message $m$ and obtain the tag $(r,t)$. He can then generate the following existential forgery; (we assume that the underlying block cipher operates on $n$-bit blocks)

- [(x)] The tag $(r \oplus 1^n, t)$ is a valid tag for the 1-block message $m \oplus 1^n$.

**Explaination**: The CBC chain initiated with the IV $r \oplus m$ and applied to the message $0^n$ will produce exactly the same output as the CBC chain initiated with the IV $r$ and applied to the message $m$. Therefore, the tag $(r \oplus 1^n, t)$ is a valid existential forgery for the message $m \oplus 1^n$

- [()] The tag $(r, t \oplus r)$ is a valid tag for the 1-block message $0^n$. 

- [()] The tag $(r \oplus t, m)$ is a valid tag for the 1-block message $0^n$. 

- [()] The tag $(m \oplus t, r)$ is a valid tag for the 1-block message $0^n$


Question 4: Suppose Alice is broadcasting packets to 6 recipients $B_1, ..., B_6$. Privacy is not important but integrity is. In other words, each of $B_1, ..., B_6$ should be assured that the packets he receiving were sent by Alice. Alice decides to use a MAC. Suppose Alice and $B_1, ..., B_6$ all share a secret key $k$. Alice computes a tag for every packet she sends using key $k$. Each user $B_i$ verifies the tag when receiving the packet and drops the packet if the tag is invalid. Alice notices that this scheme is insecure because user $B_1$ can use the key $k$ to send packets with a valid tag to users $B_2, ..., B_6$ and they will all be fooled into thinking that these packets are from Alice. 

Instead, Alice sets up a set of 4 secret keys $S = {k_1, ..., k_4}$. She gives each user $B_i$ some subsets $S_i \sub S$ of the keys. When Alice transmits a packet she appends 4 tags to it by computing the tag with each of her 4 keys. When user $B_i$ receives a packet he accepts it as valid only if all tags corresponding to his keys in $S_i$ are valid. For example, if user $B_1$ is given keys ${k_1, k_2}$ he will accept an incoming packet only if the first and second tags are valid. Note that $B_1$ cannot validate the 3rd and 4th tags because he does not have $k_3$ or $k_4$. 

How should Alice assign keys to the 6 users so that no single user can forge packets on behalf of Alice and fool some other user ?

- [[]] $S_1 = {k_1, k_2}, S_2 = {k_1, k_3}, S_3 = {k_1, k_4}, S_4 = {k_2, k_3, k_4}, S_5 = {k_2, k_3}, S_6 = {k_3, k_4}$

- [[x]] $S_1 = {k_2, k_3}, S_2 = {k_2, k_4}, S_3 = {k_3, k_4}, S_4 = {k_1, k_2}, S_5 = {k_1, k_3}, S_6 = {k_1, k_4}$

**Explaination**: Every user can only generate tags with the two keys he has. Since no set $S_i$ is contained in another set $S_j$, no user $i$ can fool a user $j$ into accepting a message sent by $i$. 

- [[]] $S_1 = {k_1, k_2}, S_2 = {k_1, k_3, k_4}, S_3 = {k_1, k_4}, S_4 = {k_2, k_3}, S_5 = {k_2, k_3, k_4}, S_6 = {k_3, k_4}$

- [[]] $S_1 = {k_1}, S_2 = {k_2, k_3}, S_3 = {k_3, k_4}, S_4 = {k_1, k_3}, S_5 = {k_1, k_2}, S_6 = {k_1, k_4}$


Question 5: 

Consider the encrypted CBC MAC build from AES. Suppose we compute the tag for a long message $m$ comprising of $n$ AES blocks. Let $m'$ be the $n$-block message obtained from $m$ by flippiing the last bit of $m$ (i.e, if the last bit of $m$ is $b$ then the last bit of $m'$ is $b \oplus 1$). How many calls to AES would it take to compute tag for $m'$ from the tag for $m$ and the MAC key ? (in this question please ignore the message padding and simply assume that the message length is always a multiple of AES block size)

- [()] 3

- [(x)] 4

**Explaination**: You would decrypt the final CBC MAC encryption step done using $k_2$, the decrypt the last CBC MAC encryption step done using $k_1$, flip the last bit of the result, and re-apply the two encryptions.

- [()] $n + 1$

- [()] 2

Question 6: Let $H: M \Rightarrow T$ be a collision resistant hash function. Which of the following is collision resistant: (as usual, we use || to denote string concatenation)

- [[x]] $H'(m) = H(m||0)$

**Explaination**: 

If $H'(m) = H'(m'), then $H(m || 0) = H(m' || 0)$$, and we have a collision $(x || 0, x' || 0)$ for $H$. Therefore, $H'$ is collision resistant

- [[]] $H'(m) = H(m) \oplus H(m \oplus 1^{|m|})$

**Explaination**: 

This construction is not collision resistant because $H(000) = H(111)$

- Preimage resistance: given $y$, it is computationally infeasible to find an $x$ such that $H(x) = y$.

- Second preimage resistance: given $x$, it is computationally infeasible to find an $x' \neq x$ such that $H(x) = H(x′)$

- Collision resistance: it is computationally infeasible to find $x,x′$ such that $x \neq x′$ and $H(x) = H(x′)$.

Given $m$, let $m' = m \oplus 1^|x|$. Then $H'(x') = H(x \oplus 1^{|x|}) \oplus H(x) = H'(x)$. Therefore, $H'$ is not second preimage resistant and thus also not collision resistant.

- [[]] $H'(m) = H(m[0,...,|m| - 2])$

**Explaination**: This construction is not collision resistant because $H(00) = H(01)$

- [[]] $H'(m) = H(0)$

**Explaination**: 

This construction is not collision resistant because $H(0) = H(1)$

Constant function, not collision resistant.

- [[]] $H'(m) = H(m)[0,...,31]$

(i.e, output the first 32 bits of the hash)

**Explaination**: 

By the birthday attack, we can find a collision in $O(2^{n′/2})$ time. Since $n′=32$, this gives us $O(2^16)$ computations - this can done efficiently, so $H′$ is not collision resistant.

- [[]] $H'(m) = H(m) \oplus H(m)$

**Explaination**: This construction is not collision resistant because $H(0) = H(1)$

- [[x]] $H'(m) = H(m || m)$

**Explaination**: a collision finder for $H'$ gives a collision finder for $H$

Question 7: Suppose $H_1$ and $H_2$ are collision resistant hash functions mapping inputs in a set $M$ to ${0,1}^256$. Our goal is to show that the function $H_2(H_1(m))$ is also collision resistant. We prove the contra-positive: suppose $H_2(H_1(\cdot))$ is not collision resistant, that is, we are given $x \neq y$ such that $H_2(H_1(x)) = H_2(H_1(y))$. We build a collision for either $H_1$ or for $H_2$. This will prove that if $H_1$ and $H_2$ are collision resistant then so is $H_2(H_1(\cdot))$. Which of the following must be trueL 

- [()] Either $x,y$ are a collision for $H_2$ $\quad$ or $\quad H_1(x), H_1(y)$ are a collision for $H_1$. 

- [(x)] Either $x, y$ are a collision for $H_1 \quad $ or $\quad H_1(x), H_1(y)$ are a collision for $H_2$

**Explaination**: If $H_2(H_1(x)) = H_2(H_1(y)) then either $H_1(x) = H_1(y)$ and $x \neq y$, thereby giving us a collision on $H_1$. Or $H_1(x) \neq H_1(y)$ but $H_2(H_1(x)) = H_2(H_1(y))$ giving us a collision on $H_2$. Either way we obtain a collision on $H_1$ or $H_2$ as required. 

- [()] Either $x, y$ are a collision for $H_1$ or $x,y$ are a collision for $H_2$. 

- [()] Either $H_2(x), H_2(y)$ are a collision for $H_1 \quad$ or $\quad x,y$ are a collision for $H_2$. 

Question 8: In this question you are asked to find a colision for the compression function. $f_1(x,y) = \text{AES}(y,x) \oplus y$, where $\text{AES}(x,y)$ is the AES-128 encryption of $y$ under key $x$. Your goal is find two distinct pairs $(x_1, y_1)$ and $(x_2, y_2)$ such that $f_1(x_1, y_1) = f_2(x_2, y_2)$. 

Which of the following methods finds the required $(x_1, y_1)$ and $(x_2, y_2)$ ? 

- [(x)] Choose $x_1, y_1, y_2$ arbitrarily (with $y_1 \neq y_2$) and let $v := \text{AES}(y_1, x_1)$. Set $x_2 = \text{AES}^-1(y_2, v \oplus y_1 \oplus y_2)$

- [()] Choose $x_1, y_1, x_2$ arbitrarily (with $x_1 \neq x_2$) and let $v := \text{AES}(y_1, x_1)$. Set $y_2 = \text{AES}^-1(x_2, v \oplus y_1 \oplus x_2)$

- [()] Choose $x_1, y_1, y_2$ arbitrarily (with $y_1 \neq y_2$) and let $v := \text{AES}(y_1, x_1)$. Set $x_2 = \text{AES}^-1(y_2, v \oplus y_1)$

- [()] Choose $x_1, y_1, y_2$ arbitrarily (with $y_1 \neq y_2$) and let $v := \text{AES}(y_1, x_1)$. Set $x_2 = \text{AES}^-1(y_2, v \oplus y_2)$

Question 9: Repeat the previous question, but now to find a collision for the compression function $f_2(x,y) = \text{AES}(x,x) \oplus y$

Which of the following methods finds the required $(x_1,y_1)$ and $(x_2, y_2)$?

- [()] Choose $x_1, x_2, y_1$ arbitrarily (with $x_1 \neq x_2$) and set $y_2 = y_1 \oplus x_1 \oplus \text{AES}(x_2, x_2)$

- [()] Choose $x_1, x_2, y_1$ arbitrarily (with $x_1 \neq x_2$) and set $y_2 = \text{AES}(x_1, x_1) \oplus \text{AES}(x_2, x_2)$

- [(x)] Choose $x_1, x_2, y_1$ arbitrarily (with $x_1 \neq x_2$) and set $y_2 = y_1 \oplus \text{AES}(x_1, x_1) \oplus \text{AES}(x_2, x_2)$

- [()] Choose $x_1, x_2, y_1$ arbitrarily (with $x_1 \neq x_2$) and set $y_2 = y_1 \oplus \text{AES}(x_1, x_1)$


Question 10: 

Let $H: M \Rightarrow T$ be a random hash function where $|M| >> |T|$ (i.e, the size of $M$ is much larger than the size of $T$). 

In lecture we showed that finding a collision on $H$ can be done with $O(|T|^{1/2})$ random samples of $H$. How many random samples would it take until we obtain a three way collision, namely distinct strings $x, y, z$ in $M$ such that $H(x) = H(y) = H(z)$ ?

- [(x)] $O(|T|^{2/3})$

**Explaination**: An informal argument for this is as follows: suppose we collect $n$ random samples. The number of triples among the $n$ samples is $n$ choose 3 which is $O(n^3)$. for a particular triple $x, y, z$ to be a 3-way collision we need $H(x) = H(y)$ and $H(x) = H(z)$. Since each one of these two events happens with probability $1/|T|$ (assuming $H$ behaves like a random function) the probability that a particular triple is a 3-way collision is $O(1/|T|^2)$. Using the union bound, the probability that some triple is a 3-way collision is $O(n^3/|T|^2)$ and since we want this probability to be close to 1, the bound on $n$ follows. 

- [()] $O(|T|^{3/4})$

- [()] $O(|T|)$

- [()] $O(|T|^{1/4})$
