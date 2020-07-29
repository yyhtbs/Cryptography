# Piblic Key Encryption Methods

## Diffe-hellman Key Exchange

Scenario: <code>Alice <-> Bob</code>

Alice and Bob share the same generator $g = 3$, mod $e = 17$

For Alice:
Alice selects $p = 15$ (Alice's secret)

$3 ^ {15} \mod 17 \equiv 6$

For Bob: 
Bob selects $q = 13$ (Bob's secret)

$3 ^ {13} \mod 17 \equiv 12$

Bob sends the $b = 12$ to Alice. 

Alice uses takes the result $b = 12$ and do the follows:

$12^{15} \mod 17 \equiv 10$

Bob uses takes the result $b = 6$ and do the follows:

$6^{13} \mod 17 \equiv 10$

The reason that Alice and Bob reaches the same shared secret is because,

$$3^{13^{15}} \mod 17 = 3^{15^{13}} \mod 17$$

In this case, the DH key is $10$. It can be understand to be a random number, not Alice or Bob can control it.  

## RSA Public Key Encryption

For symmetric encryption, two parties need to share the encyrption key. 
It is not efficient if Alice and Bob cannot meet each other. 

With DH key exchange, the comminication overhead is high. Especially when Alice wants to talk to multiple nodes concurrently. It needs to have a different encryption key with each node. Otherwise, the information will leak between the nodes. 

James Ellis a British engineer and mathematician works on the idea called "non-secret encryption". Unfortunately, he does not reach the math solution. 

**Concept**: Lock and Unlock are inverse operations.

Alice buys a lock with key. She opens the lock and sends it to Bob. Bob locks a message $m$ and sends it back to Alice. Alice can unlock the message using its key. In this case no keys are exchanged. 

Alice can dupliate the lock and sends it to every want to send Alice a message. 

The solution is done by another British mathematician and cryptographer Clifford Cocks.
**Idea**: Split the key into two parts: the public part and the private part. 

Cock constructs a special one way function called trapdoor oneway function. 

Without the trapdoor, it is a one-way function (hard to inverse). Otherwise it is not (easy to inverse). 

### Math

Recall DH algorithm:
$$g ^ e \mod d$$
Here $g$ is called the base, $e$ is called the exponent and $d$ is called the modulus.

Bob has a message $m$, it takes the exp and divides the result by the random number $N$, 

$$m^e \mod N \equiv c$$

The one way function means given $m$, $e$ and $N$ it is easy to work out $c$. However, given $c$, $N$ and $e$, it is hard to work out $m$.

**Concept UNDO:**

For encryption, Alice takes the public key $e$ and $N$ from Alice to encyrpt the message $m$
$$m^e \mod N \equiv c$$

For decryption, Bob takes the cipher $c$ and decrypts it using the trapdoor $d$ (another key) also known as the private key. 
$$c^d \mod N \equiv m \rightarrow m^{ed} \mod N \equiv m$$

So the core is for Alice to find a pair of keys $e$ and $d$ that allows the lock-then-unlock operation while no one can receover $d$ from $e$ and $c$. 

**Another One-way function to generate $d$**

It is a factorisation function that Cock used to generate the keys because prime factorisation is difficult to compute. 

Alice generates a random number $P_1$ is 150 digits long and another one $P_2$. Then she multiply them and get $N = P_1 * P_2$

Why it is difficult? Because the factorisation of $N$ is hard. 

**Euler's $\Phi$ Function**

Euler is working on the distribution of prime numbers. 

The Phi function measures the breakability of a number. 

$\Phi(N)$ outputs the number of numbers that do not share common factor with $N$

For example, $\Phi(8) = 4 = |1, 3, 5, 7|$ because these numbers do not share the common 8' factor 2.

The Phi function is special for the prime numbers $p$, i.e. the prime number has no factors greater than 1. 

$$\Phi(p) = p-1$$

For the product $N$ of two primes $p_1$ and $p_2$, its $\Phi$ function is,
$$\Phi(N) = \Phi(p_1) * \Phi(p_1) = (p_1 - 1) * (p_2 - 1)$$

So if we know the factors of $N$ it is easy to calculate $\Phi(N)$.

For example, we have $p_1 = 7$ and $p_2 = 11$, we can easily calculate $\Phi(77) = \Phi(7) * \Phi(11) = 6 * 10 = 60$

### Euler's theorem: Connection Between $\Phi$ and Modular Exponentiation

For a $\Phi$ function, it has the following property:

$$m^{\Phi(n)} \equiv 1 \mod n$$

It uses the $\Phi$ function as the exponentiation term. 

For example, let us have $m = 5$ and $n = 8$,

$$5^{4} = 625 \rightarrow 625 \equiv 1 \mod 4$$

### Some Property of This formula

$$m^{\Phi(n)} \equiv 1 \mod n \rightarrow \forall k \in \mathbb{Z}, m^{k*\Phi(n)} \equiv 1 \mod n$$

$$m^{k*\Phi(n)} \equiv 1 \mod n \rightarrow m*m^{k*\Phi(n)} \equiv m \mod n \rightarrow m^{k*\Phi(n)+1} \equiv m \mod n$$

Now we find the value d depends on $\Phi(N)$

$$m^{e*d} \equiv m \mod n \rightarrow e*d = k* \Phi(n) + 1 \rightarrow d = \frac{k* \Phi(n) + 1}{e}$$

Here $d$ is the Alice's private key to undo the encryption of public key $e$.

### Example: How to generate a pair of RSA keys 

Alice generates two keys:
* $p_1 = 53$
* $p_2 = 59$
* $n = 53*59 = 3127$
* $\Phi(n) = 52*58 = 3016$
* $e = 3$, $e$ is small public key which should be a odd number and should not share factors with $\Phi(N)$ 
* $d = \frac{2 * (3016) + 1}{3} = 2011$, $d$ is the private key. 
**P.S.** Not sure how to get $e$. Maybe it is easy to find $e$ can divide ${k* \Phi(n) + 1}$

Alice hides everything except $n = 3127$ and $e = 3$, then sends $n|e$t as the public key to Bob. 

Bob uses the key to encrypt its message $m = 89$:
$$c = 89^{3} \mod 3127 = 1394$$

Then Bob sends $c$ to Alice.

Alice decrypts the message using her private key $d = 2011$.
$$\hat{m} = 1394^{2011} \mod 3127 = 89 = m$$

















