
# Message Integrity 

## Message Authentication Codes (MAC)

A MAC is defined as (S,V) and defined over (K, M, T) is a pair of algorithms:
- S(k,m), the signing algorithm, outputs t (tag) in T 
- V(k,m,t), the verification algorithm, outputs yes or no

— 

Consistency requirement: for every message and for every key: V(k, m, S(k,m)) = yes

—

Integrity requires a shared key between Alice and Bob. Algorithms like CRC do detect random errors but not malicious errors! 

### Secure MACs 

The attacker’s power : chose message attack:
- for m_1 … m_q attacker is given t_i <- S(k,m_i)

The attacker’s goal: Existential forgery
- produce some new valid message/tag pair (m,t): (m,t) different than any pair that is given to him.

What does this mean?
- The attacker cannot produce a valid tag for a new message.
- Given (m,t) attacker cannot even produce (m,t’) for t != t’

—

Definition: I=(S,V) is a secure MAC if for all “efficient” A: Adv_{MAC} [A, I] = Pr[Chal. outputs 1] is negligible.

—

A secure PRF => Secure MAC if the output space of the PRF is big enough. In practice, 80-bits is good security for a PRF. 

We can use AES for 16-byte messages but how can we convert a MAC for small inputs and scale them for bigger inputs. The output of a n bit PRF can be truncated. 

### (encrypted) CBC-MAC

Let F:K x X -> X be a PRP, define a new PRF F_ECBC:K^2 x X^{=< L} -> X

![CBC-MAC construction](http://cl.ly/The8/Screen%20Shot%202014-02-03%20at%2015.36.37.png)

### NMAC

Let F:K x X -> K be a PRF, define a new PRF F_NMAC : K^2 x X^{=<L} -> K.

![NMAC Construction](http://cl.ly/TiX2/Screen%20Shot%202014-02-03%20at%2015.44.55.png) 

![CBC-MAC vs NMAC](http://cl.ly/Tiod/Screen%20Shot%202014-02-03%20at%2015.36.37.png)

### MAC padding

Errors in padding can have disastrous consequences, just imagine if someone can forge a banking transaction with an additional 0! 

So, how do we pad?  Padding must be reversible (1 to 1) to make sure it’s unique. We pad with “100…0”. Add a new dummy block if the message size is already a multiple of the block size.

### CMAC

Variant of CBC-MAC where no additional encryption step is necessary and no need to add a dummy block. CMAC uses key = (k, k_1, k_2) where k_1 and k_2 are derived from K. 

![CMAC construction](http://cl.ly/TiW6/Screen%20Shot%202014-02-03%20at%2017.07.21.png)

### PMAC - Parallel MAC

All the PRFs seen so far for MACs are sequential. PMAC is parallel and incremental (if one block changes, no need to recompute everything). 

![PMAC Construction](http://cl.ly/Thwf/Screen%20Shot%202014-02-03%20at%2017.23.06.png)

### One-time MAC

![One Time MAC example](http://cl.ly/Til8/Screen%20Shot%202014-02-03%20at%2017.51.37.png)

### Many-time MAC

![Many Time MAC](http://cl.ly/TiMx/Screen%20Shot%202014-02-03%20at%2017.53.44.png)

## Collision Resistance

Let H:M -> T be a hash function (|M| >> |T|)

A **collision** for H is a pair m_0, m_1 in M such that: H(m_0) = H(m_1) and m0 != m1

A function H is **collision resistant** if for all efficient algorithms A: Adv_CR [A,H] = PR[A outputs collision for H] is negligible. 

If we have a collision-resistant function, we can build MACs for bigger messages from secure MACs that work on smaller messages.

### Protecting file integrity

If we have a public read-only space and no key, we can use a collision resistant hash function to verify integrity of a package.

### Birthday attack

Using the birthday paradox, we can predict collisions with probability: when n = 1.2 * B^{1/2} then Pr[collision] => 1/2
with n, number of items in attack set and B, number of items in universe.

### Using Hash-functions

Use SHA-512 is recommended. 

### The Merkle-Damgard Paradigm

![Merkle Damgard](http://cl.ly/Tiap/Screen%20Shot%202014-02-04%20at%2001.27.14.png)

Security guaranteed by theorem that claims that if h is collision resistant then so is H

### Compression functions

#### Davies-Meyer compression function based on block ciphers

The Davies-Meyer compression function: h(H,m) = E(m, H) XOR H

Theorem: Suppose E is an ideal block cipher. Finding collisions h(H,m)=h(H’,m’) takes O(2^(n/2)) - same as birthday attack, best possible, evaluations of (E,D).

### SHA-256

SHA-256 is a Merkle-Damgard function that uses the Davies-Meyer compression function and where the block cipher used is SHACAL-2 (512-bit keys) and block size of 256 bits. 

### HMAC - MAC from SHA256

HMAC: S(k,m) = H( k XOR opad, H( k XOR ipad || m))

![HMAC In Pictures](http://cl.ly/Ti2P/Screen%20Shot%202014-02-04%20at%2001.47.50.png)

ipad and opad are 512 bits constants. 

### Verification timing attacks

Make sure that verification is constant time.

1) Iterate through every set of bytes. Be careful of compiler optimisations!
2) Compute correct MAC, we hash the correctly computed MAC again and then compare byte by byte. This way the attacker doesn’t know what is being compared!

Lesson: Don’t implement your own crypto!

## Authenticated Encryption

**Confidentiality (CPA secure ciphers) without integrity cannot guarantee secrecy under active attacks.**

If message needs integrity but no confidentiality -> MAC. If message needs both integrity and confidentiality, use authenticated encryption modes.

Example of attack: Altering destination port of IP packet even if CPA secure cipher is possible by changing IV (if in CBC mode for instance).

—

Authenticated encryption  was introduced in 2000 but before then, many crypto libraries supported CPA-secure encryption and MAC. Every developer had to find his own 	way of combining both to provide AE.

### Definition

An authenticated encryption system (E,D) is a cipher where E:K x M x N -> C and D: K x C x N -> M ∪ {ciphertext is rejected}. 

The system must provide both semantic security under a CPA attack and cipher text integrity (attacker cannot create new cipher texts that decrypt properly).

### Implications

Authenticated ciphers do provide:
- Authenticity: Attacker cannot fool Bob into thinking a message was sent from Alice. (doesn’t protect against replay/side channels attacks though)
- Security against chosen cipher text attack

### Chosen cipher text security

Adversary’s power : both CPA and CCA
- Can obtain the encryption of arbitrary messages of his choice
- Can decrypt any cipher text of his choice other than the challenge
Adversary goal : break semantic security.

Let (E,D) be a cipher that provides authenticated encryption, then (E,D) is CCA secure. 

### Encrypt-then-Mac vs MAC-then-Encrypt

Three approaches:
- MAC-then-Encypt (SSL): May be insecure against CCA attacks. If you’re using rand-CTR mode or rand-CBC, you have authenticated encryption. And for rand-ctr mode, one time MAC is sufficient.
- **Encrypt-then-Mac** (IPSec): This is the favorite method. Because encryption provides confidentiality on the message and then the MAC algorithm provides the signing. Always correct. 
- Encrypt and MAC (SSH) : This means that MACs (of the message) are concatenated with the cipher text. This is an issue if the MACing algorithm reveals information about the plaintext. (MAC algorithms are not designed for confidentiality, only integrity). No issues with the specifics of SSL though but should really not be used.

### Standards

All these modes are AEAD (auth enc. with associated data, partial encryption but fully authenticated) and nonce-based.
- GCM (Galois counter mode - NIST): CTR mode encryption then CW-MAC. Recommended way to provide authenticated encryption if code size is not an issue (non-embedded systems).
- CCM (CBC counter mode - NIST): CBC-MAC then CTR mode encryption (used for 802.11)
- EAX : CTR mode encryption then CMAC.

### New constructions

After authenticated encryption got formalised. People started thinking about newer constructions that would provide AE without combining an encryption mode and a MAC algorithm. 

OCB is an example of that. OCB is parallelizable. Sadly, OCB is not used because of patents :(
 
### TLS & WEP

See course & slides

### Avoiding implementation mistakes

Encrypt-then-MAC would completely avoid issues of MAC verifications in TLS because the MAC is checked first and ciphertext discarded if invalid. MAC-then CBC provides AE but padding oracle destroys it. 

### Attacking non-atomic decryption

The attacker can “stream” the bits of the ciphertext and then see when the MAC is verified because it’s probably going to be false and hence learn the first LSB of a message.

Lesson: Never partially decrypt ciphertext, always take blocks.
