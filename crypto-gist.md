#+STARTUP: content

Course name: Cryptography 1
URL: https://class.coursera.org/crypto-007/class

* Lectures
** Week 1
*** [[https://class.coursera.org/crypto-007/lecture/1][Course Overview (11 min)]]
- HTTPS is actually not a protocol of its own. It's simply regular
  HTTP on top of SSL/TLS.
- Different applications of crypthograhy:
  - Secure communication online
  - Protecting the contents of a disk from outsiders
    - This is analogous to secure communication, where the current
      self is trying to securely communicate with his/her future self
- There's two parts to this course:
  - 1) How to transmit data using a shared secret key
  - 2) How to establish public-key cryptography
- Symmetric encryption:
  - We have a message m, key k and two algorithms E and D (together
    called the cipher). E is used to /encrypt/ the message and D is
    used to /decrypt/ it afterwards. The result of applying E to m is
    called c for ciphertext.
  - You should never use a cipher that's proprietary. Always use
    algorithms that are publicly known.
  - Only k is secret. Everything else is information available for the attacker.
  - Use cases:
    - Single use key: a key is used only once for its task, i.e. for
      sending e-mail.
    - Multi use key: the key is used multiple times, for example to
      encrypt many files. This approach needs more machinery than one
      time key.
- Cryptography follows standards. You should not try to invent stuff
  yourself ad-hoc style. Shit's complicated, yo!
*** [[https://class.coursera.org/crypto-007/lecture/2][What is cryptography? (15 min)]]
- Cryptography use cases:
  - Secure communication
  - Storing files
  - Digital signatures
  - Anonymous communication (using mix net):
    - Two parties are able to communicate with each other without
      knowing who or where each other are located.
  - Anonymous digital cash (Bitcoin):
    - Cryptography allows us to get around the problem of double
      spending.
  - Voting problem: How to hold a legitimate elections where we can
    authentically count the amount of votes without revealing
    information about who voted for who.
  - Private auctions: how to hold legitimate auctions without making public the
    amount of the bids.
  - More generally these two are examples of problems related to
    Secure Multi-Party Computation:
    - We have a group of individuals with their inputs (votes, bids)
      [x1, x2, x3... xn] and our goal is to compute a function (voting
      -> the majority, Vickrey auctioin -> the second highest number)
      out of those inputs securely.
    - One (bad) solution to this problem is to use a central trusted
      authority to count the inputs and compute the function we are
      interested in.
    - There is a theorem in crypthography stating that: Any
      computation that you can do with a trusted authority, you can
      also do it without such central authority.
    - This is done by establishing a protocol among the individuals,
      that results in the desired computation being performed, withoug
      disclosing private information (voting preferences, bidding
      prices) amongs themselves.
  - Privately outsourced computation: example with Google search.
  - Zero knowledge (proof of knowledge): [[https://news.ycombinator.com/item?id%3D5545377][examples]], an [[http://www.mit.edu/~rothblum/papers/sudoku.pdf][academic paper]]
    demonstrating the technique on a sudoku puzzle (I claim that I can
    solve a puzzle but you don't trust me. The application of
    cryptography allows me to demonstrate this to you without
    revealing what the solution to the puzzle is).
- Cryptography is a rigorous science and follows a three step
  protocol:
  - 1) Preciseley outlining a threat model
  - 2) Proposing a construction to solve the threat
  - 3) Prove that breaking the construction under threat mode will
    solve an underplying hard problem
*** [[https://class.coursera.org/crypto-007/lecture/3][History of cryptography (19 min)]]
- A book: [[http://www.amazon.com/The-Codebreakers-Comprehensive-Communication-ebook/dp/B001D201IK/ref%3Dsr_1_1?s%3Ddigital-text&ie%3DUTF8&qid%3D1371460285&sr%3D1-1&keywords%3Ddavid%2Bkahn%2Bthe%2Bcode%2Bbreakers][The Codebreakers]]
- Some historic examples of cryptography:
  - Substitution cipher: simple technique where you substitute one
    letter for another based on a rule.
    - Caesar Cipher (no key): the rule -> shift by three, so: a -> d,
      b -> e, c -> f etc.
    - How to break:
      - The /keyspace/ of this cipher is 26! so around 2^88. This in
        itself is actually ok, but..
      - We can easily break the code by analyzing letter frequency:
        - We know that "e" is the most frequent letter in the English
          language at around 12.7%. Therefore the most frequent letter
          in the ciphered text is likely to be "e".
        - After individual letters we can move to the frequency of
          letter pairs (digrams)
        - This scheme is very weak because we only need access to the
          ciphertext in order to crack it.
    - Vigener cipher (16th century):
      - You choose a key and repeat it over the length of the message.
        Then you add the (+ mod 26) of the two letters together (so
        once you go over Z you start at A again) and thus you get your
        ciphertext.
    - Rotor Machines (first mechanical form of cryptography) (1870-1943)
      - The key is a rotor in a machine that rotates, thus creating a
        new key map for each key press.
      - Early example: the Hebern machine
      - The most famous example: The Enigma
      - Also breakable with cipher-only attacks.
    - Data Encryption Standard (1974):
      - The government establised a standard for encryption
      - DES uses 2^56 keys with block size(?) of 64 bits, which while
        suffecient when it was introduced, should not be used in
        todays world because it's past its days.
      - Today we have AES (2001), Salsa20 (2008) and others
*** [[https://class.coursera.org/crypto-007/lecture/67][Discrete probability (Crash course) (18 min)]]
- Discrete probability is always defined over a finite universe,
  denoted by U.
- Probability distribution P over U is a function that assigns a
  probability to each element in U so that the sum total for all of
  them is 1. Probability distributions can be categorized based on
  their characteristics:
  - 1) Uniform distribution: the probability of each element is equal,
    i.e. 1/|U|
  - 2) Point distributio at x0, where P(x0) = 1,  x ∀ x0: P(x) = 0. (∀
    stands for 'for all')
- Events are subsets of U. For example for U = {0,1}^8 (|U| = 256), we
  can define event A such that it's { all x in U such that lsb2(x) (last
  two significant bits) are 11 }. For the uniform distribution on
  {0,1}^8 the probability of A is 1/4. (11 is one out of four [00, 01,
  10, 11])
- The Union Bound: the probability for events A1 and A2 is Pr[ A1   A2
  ] <= Pr[A1] + Pr[A2]
- A Randomized Algorithm is an algorithm which given the same input
  returns a random output. So instead of y <- A(m) we have y <-
  A(m;r), where r <-R- {0,1}^n output is a random variable.
- We can think of Randomized Algorithms as outputting a randomized variable.
- In a cryptographic algorithm where algorithm A takes Message m and
  Key k as its arguments, and outputs encrypted message E(k, m), we
  can think of the algorithm as being a randomized algorithm where y
  <-R- A(m).
*** [[https://class.coursera.org/crypto-007/lecture/66][Discrete probability (crash course, cont.) (14 min)]]
- Independence: events A and B are independent if the occurence of one
  event tells nothing about the occurence of the other. So in other
  words if Pr[ A and B ] = Pr[A] * Pr[B]
- Random variables x and y taking values V are independent if  a,b V:
  Pr[X=a and Y=b] = Pr[X=a] * Pr[Y=b]
- XOR has an important property from cryptographys point of view:
  - For a random variable over {0,1}^n and an independent uniform
    variable on {0,1}^n, then for Z := Y XOR X is a uniform variable.
*** [[https://class.coursera.org/crypto-007/lecture/4][Information theoretic security and the one time pad (19 min)]]
- The definition of a working cipher is defined over (/K/, /M/, /C/):
  - Where /K/ stands for the key space, or the set of all possible
  keys
  - /M/ for the message space. or the set of all possible messages
  - /C/ fro the cipher space, or the set of all possible ciphers
- The formal definition of a cipher is thus defined so that for a
  pair of "efficient" algorithms:
  - Encryption algorithm E: /K/ x /M/ -> /C/ 
  - Decryption algorithm D: /K/ x /C/ -> /M/
  - For all messages m in message space /M/, and all keys k in key
    space /K/, D( k, E( k, m )) = m
- E is often randomized. D is always deterministic.
- The One Time Pad (OTP) (Verman 1917) is our first example of a
  "secure" cipher:
  - C := E(k, m) = k XOR m
         D(k, c) = k XOR c
  - It's very fast encode and decoding, but keys are long (as long as
    the message)
- Information Theoretic Security (Shannon 1949):
  - Basic idea: cipher text (CT) should reveal no "info" about plain
    text (PT)
  - Definition: A cipher (E, D) over (/K/, /M/, /C/) has perfect
    secrecy if for all m0, m1, that are elements of /M/, where
    len(m0) = len(m1) and for all c that are elements of /C/, the Pr[
    E(k, m0) = c] = Pr[ E(k, m1) = c], where k is uniform in /K/ (k
    <-r- /K/)
  - I.e. if all the adversary has is the cipher text, then he should have
    no idea if it originated from which message.
  - There are no CT only attacks! (but there might be other attacks)
  - The bad news: If we have perfect secrecy, then |K| >= |M|
  - Therefore it's not very practical.
*** [[https://class.coursera.org/crypto-007/lecture/5][Stream ciphers and pseudo random generators (20 min)]]
- Idea: replace "random" key by "pseudorandom" key:
  - PRG: is a function G (for generator) that takes a seed {0,1}^s
    (s-bit seed), that maps it into a larger string {0,1}^n that we
    can expand to string where n >> s.
  - Needs to be "effectively" computable by a deterministic alrogithm
  - Stream ciphers can't have prefect secrecy:
    - We therefore need a different definition of security
    - Security is dependent on the strength of the RPG
- The PRG must be unpredictable:
  - What does it mean for PRG to be predictable?
    - There exist some i such that given the first i bits of a
      sequence generated by G(k)|1,...,i, there exists some algorithm A that
      can predict the next i+1,...,n bits of the sequence.
    - Example: in the SMTP protocol (e-mail), every message begins
      with the word "from,". The attacker could XOR the cipher text
      with the pre-fix "from,", and thus he would get a pre-fix for
      the pseudorandom sequence. From the prefix we can predict the
      rest of the message.
    - Even if you can only predict one bit from a prefix, this
      becomes a problem.
  - PRG is predictable if there exists some algorithm A that given
    some a bit i from a position 1 <= i <= n-1, the Pr[
    A(G(k))|1,..,i = G(k)|i+1 ] >= 1/2 + epsilon.
  - Usually an epsilon of >= 0.5^30 would be considered non-negligeable.
- Some examples of weak PRGs (not suitable for crypto):
  - [[https://en.wikipedia.org/wiki/Linear_congruential_generator][Linear congruential generators]]
  - [[http://www.mathstat.dal.ca/~selinger/random/][glibc random()]]
- Neglible vs. non-neglibgle:
  In practice:
  - epsilon of >= 0.5^30 will is non-neglible because it is likely to
    take place over 1GB of data
  - epsilon of >= 0.5^80 is neglible because it won't happen over a
    life time of the key
  - These practical definitions of neglible can be problematic
  In theory:
  - We don't talk about epsilon in terms of scalars, but in terms of functions:
    - These functions map on non negative integers, and they are
      bigger than some 1/polynomial infinitely often.
    - Neglible means that for degree polynomial d, λ >= λd:
      epsilon(λ) <= 1/λ^d.
*** [[https://class.coursera.org/crypto-007/lecture/6][Attacks on stream ciphers and the one time pad (24 min)]]
- Attack 1: Using a two time pad is insecure:
  - If we have C1 <- m1 XOR PRG(k) and C2 <- m2 XOR PRG(k), then an
    attacker who has access to both C1 and C2 can take the XOR of the
    two, yielding him the XOR of m1 and m2. The English language and
    ASCII encoding has enough redundancy that you can figure out the
    plain text from that.
  - Never use a stream cipher key more than once!
  - Some real world examples:
    - Project Verona (1941-1946): Russians were encoding their
      messages using a key generated by throwing dice, but the people
      generating the keys got lazy and used the same pad for more than
      one message. This allowed the US Intelligence agency to decrypt
      something like 3k messages.
    - MS-PPTP (Point to Point Transfer Protocol) in Windows NT: A
      client is communicating with a service by sending messages i.e.
      m1, m2, m3, and the server responds with r1, r2, r3. The
      messages are treated as a stream, and encrypted using a stream
      cipher. The problem is that the same is also happening on the
      server side, using the same cipher key. The way to do this
      securely would be to have one key for C->S and S->C.
    - 802.11b WEP (WIFI): We have a client and and access point
      (router): When a client sends a message to the server, it gets
      encrypted by PRG( IV || k ) where IV is a 24 bit string that
      starts repeating after 2^24 = ~16M frames. The problem is that
      once the IV cycles we have a two time pad. On some 802.11 cards
      the IV resets to 0 after a reset.
    - Another mistake WEP makes: The changing IV bit is 24 bits and
      the key is 104 bits but never changes. An attacker listening in
      on the stream can perform an attack that takes advantage of the
      similarity of the total seed-keys and the PRG (RC4) used in WEP,
      which allows him to recover the key after only 40k frames.
    - Disk encryption: If we make an edit to a file using the same
      pad, the attacker is able to identify the part of the file where
      the edit occured. In order to prevent this, the change of a
      small part of the file should result in the contents of the
      whole file getting changed.
  - Summary:
    - Never use a stream cipher key more than once
    - For network traffic: negotiate a new key for every session (e.g. TLS)
    - For disk encryption: avoid using a stream cipher.
- Attack 2: no integrity (OTP is malleable)
  - An attacker who has has access to the cipher text and the ability
    to send messages to the receiving party can spoof the message by
    XOR'ing a new pad P the the cipher, which leaves the receiver with
    m XOR p. This is a problem because the impact on the plaintext can
    be predictable.
  - Example: Bob sends an e-mail to Alice using encryption. The
    message gets intercepted, and the attacker, knowing that a message
    from Bob will start with the form: "From: Bob", can use this to
    spoof a fake message, that appears to be "From: Eve" instead. He
    can do so by XOR'ing "Bob" with "Eve", and replacing the part of
    the message with the name of the sender with such a pad. Alice
    will then receive the modified message, displaying as though it
    was from Eve.
  - This is an example of the cipher scheme lacking integrity, or in
    other words of being malleable.
*** [[https://class.coursera.org/crypto-007/lecture/7][Real-world stream ciphers (20 min)]]
- RC4 (1987) (Broken):
  - Takes a 128 bit seed that gets expanded into 2048 bits and then
    executes a loop that produces one byte at a time.
  - Used in HTTPS and WEP
  - Has several weaknesses and is not recommended for use in modern applications:
    - Bias in initial output: Pr[ 2nd byte = 0 ] = 2/256 instead of
      1/256. The same case is true not just for the second byte, for
      for all bytes before 256. You should therefore ignore the first
      256 bytes altogether.
    - Prob. of sequential 0,0 is 1/256^2 + 1/256^3 when it should be
      only 1/256^2. This appears after several gigabytes of data is produced.
    - Related key attacks: Using keys that are similar to one another
      leaves you vulnerable to an attack.
- CSS (Content Scrambling System) (hardware) (Broken):
  - Based on a concept called Linear Feedback Shift Register
  - Seed is the initial state of the LSFR, which is a collection of
    sequential bits. Predetermined bytes get XOR'd together, the
    sequence of bits gets shifted right, and the XOR'd bit becomes
    the first bit of the sequence.
  - CSS is used in DVD encryption (2 LFSR), GSM  encryption (3 LSFR)
    and Bluetooth (4 LFSR). They are all badly broken.
  - CSS in DVD's:
    - seed = 5 bytes = 40 bits
    - Two LSFR's: first a 17-bit and the second a 25-bit. The first
      2 bytes of the 17-bit LSFR get concatenated, as is done for
      the last three bytes of the 25-bit key. Then the two LSFR's
      are run for 8 rounds, and the result is added together +
      (mod 256) plus a carry from the previous block. Then one byte
      per round is outputted.
    - Easy to break in time ~= 2^17 time.
    - Movie files start with a predictable plain text, which the
      attacker can XOR with the first x = (length plain-text) bits,
      which gets him the first 20 bits of the LSFR. The attacker can
      then try each 2^17 of the possible values the first LSFR can
      take, and once he lands on the right one, he can retrieve the
      first x bits of the 25-bit LFSR as well. When he has both, he
      can decrypt those first x bits.
    - Many open source systems that can decrypt CSS encrypted data.
  - Modern stream ciphers: eStream (2008) (Not broken!):
    - In addition to a seed, the algorithm also takes a {0,1}^s x R
      (Nonce) -> {0,1}^n, n>>s.
    - Nonce: a non-repeating value for a given key.
    - The encryption scheme then becomes E(k, m: r) = m XOR PRG(k; r),
      where the pair (k, r) is never used more than once. We can thus
      re-use the key, because the nonce makes it "fresh"
    - eStream: Salsa 20 (Software + Hardware):
      - {0,1}^128 or 256 bit key x {0,1}^64 nonce -> {0,1}^n (max n is
        2^73 bits)
      - Salsa20(k, r) := H( k, (r,0)) || H( k, (r,1)) || ... . H takes
        key, a nonce and an incremental counter.
*** [[https://class.coursera.org/crypto-007/lecture/8][PRG Security Definitions (25 min)]]
- What does it mean for a PRG to be secure? Our goal is as follows:
  - Define what it means that [ k <-r- /K/ output G(k) ] is
    "indistinguishable" from [ r <-r- {0,1}n, output r ]
  - In other words: The uniform output of G must be indistinguishable
    from the uniform distribution of all possible random bits that
    could be produced, despite the key-bits being a tiny minority of
    the entire set.
- Statistical Tests:
  - A statistical test is an algorithm A that takes an n-bit string,
    and outputs "1" or "0" for random, or not random.
  - Examples:
    - 1) A(x) = 1 iff (if and only if) | x.count(0) - x.count(1) | <=
      10*sqrt(n)
    - 2) A(x) = 1 iff | x.count("00") - n/4 | <= 10 * sqrt(n)
    - 3) A(x) = 1 iff x.max-run-of(0) <= 10 * log2(n). Note that if
      you give an all 11111... string to this test, it will also
      output 1.
  - Statistical tests can't do pretty much whatever they want. In the
    old days, you would take a battery of statistical tests and if G
    passed a required number of them, then you could say that G was
    secure. This is not a good standard by modern standards though.
- Advantage:
  - Defined:
    - Adv(prg)[A, G] := | Pr[ A(G(k)) = 1 ], (where k <-R- /K/) - Pr[
      A(r) = 1 ], (where r <-r- {0,1}^n) |
    - The advantage is therefore always between 0 and 1
    - If Adv. is close to 1 -> the statistical test A behaved
      differently when we gave it pseudo random input, than when we
      gave it truly random input. This means that A is able to
      distinguish the output of G from random, and is thus breaking G.
      If the Adv. is 0 then A can not break G.
  - Example:
    - 1) A(x) = 0 => Adv(PRG)[A,G] = 0
    - 2) Suppose we have G: K -> {0,1}^n satisfies msb(G(k)) = 1 for
      2/3 of keys in K. Define a stat. test A(x) as: if [ msb(x)=1 ]
      output 1, else 0. Then Adv(PRG) = | Pr[ A(G(k))=1] - Pr[ A(r)=1
      ] | = | 2/3 - 1/2 | = 1/6. We can say that A breaks G with an
      advantage of 1/6.
- Secure PRGs: crypto definition
  - Definition: We say that G:K -> {0,1}^n is secure PRG if there
    exists no such statistical test A that can distinguish its output
    from truly random input efficiently.
  - ∀ "eff" A: Adv(prg)[A,G] is "negligible"
  - Are there provably secure PRGs? We actually can't say. Because of
    the P = NP problem.
  - A secure PRG is unpredictable; you can't infer the probability of
    any of the bits from its previous input.
  - Theorem (Yao, -82): An unpredictable PRG is secure. Let G:K ->
    {0,1}^n be PRG. If ∀ i ∈ {0, ..., n-1} PRG G is unpredictable at
    pos. i, then G is secure PRG.
  - If next-bit predictors cannot distinguish G from random, then no
    statistical test can.
  - It is possible to know that a G is unsecure / predictable, without
    knowing in which way it is so.
  - More Generally:
    - Let P1 and P2 be two distributions over {0,1}^n. We say that P1
      and P2 are computationally indistinguishable (denoted, P ~=p P2)
    - If ∀ "eff" stat. tests A, the probability that A(x) = 1 for all
      x <- P1 minus the probability that A(x) = 1 for x <- P2 is less
      than negligible.
    - A PRG is secure if {k <-r- K: G(k)} ~=p uniform({0,1}^n)
*** [[https://class.coursera.org/crypto-007/lecture/9][Semantic Security (16 min)]]
- What is a secure cipher?
  - try #1: attacker cannot recover secret key? NOPE. E(k,m) = m would
    pass the test
  - try #2: attacker cannot recover all of plaintext? NOPE. revealing
    even some of the text is not very cool.
  - Shannon's idea: cipher text should reveal no information about
    plain text
  - Shannon's theorem of perfect secrecy is too strong though. Stream
    ciphers are not secure by that definition. What we need is a
    weaker definition that we can call semantically secure.
  - Compare:
    - Shannon's: (E,D) has perfect secrecy if ∀ m0, m1 ∈ /M/ (m0, m1
      are same length), {E(k, m0)} = {E(k, m1)} where k <- /K/
    - Semantic security: (E,D) has perfect secrecy if ∀ m0, m1 ∈ /M/
      (same length), {E(k,m0} ~=p {E(k,m1)} where k <- /K/
  - Semantic security (for one-time key)
    - For two messages m0 and m1 that get encrypted by E. If an
      adversary is able to distinguish between their encrypted forms,
      then E is not secure.
    - In other words, for all explicit m0, m1 ∈ M: {E(k, m0)} ~=p
      {E(k, m1)}.
  - OTP is semantically secure, because the distribution of XOR'ing
    anything with a key is always uniform (special property of XOR
    mentioned earlier).
** Week 2
*** [[https://class.coursera.org/crypto-007/lecture/12][What are block ciphers? (17 min)]]
- Block ciphers:
  - The E,D cipher takes a key (k-bits), a n-bit length PT block, and returns a
    n-bit (same length) CT block.
  - Canonical examples:
    - 1) 3DES: n=64bits, k=168bits
    - 2) AES: n=128bits, k=128, 192, 256bits
  - Block ciphers are build by iteration. The key k is expanded into
    'round keys' k1, k2, ... kn. The message m is then encoded using R
    that stands for round function, which takes one of the round keys,
    and the current state of the message /mi/. This sequence of steps
    is applied iteratively until we reach /kn/, and the concatenation
    of the output of R is our cipher text.
  - Different ciphers have different amount of rounds. For 3DES it's
    48, and for AES-128 it is 10.
  - Block ciphers are rather slow compared to stream ciphers. The
    longer the key, the slower they are. However we can do many things
    with block ciphers that we couldn't do with stream ciphers.
  - Abstract: PRPs (Pseudo Random Purmutation) and PRFs (Pseudo Random
    Function):
    - Pseudo Random Function is defined over a key-space K,
      input-space X and output-space Y: F: K * X -> Y, such that there
      exists "efficient" algorithm to evaluate F(k,x).
    - Pseudo Random Permutations are defined over K (key-space) and X
      (set X): E: K * X -> X,
      such that:
      - 1) There exists "efficient" deterministic algorithm to
        evaluate E(k,x)
      - 2) The function E(k,⋅) is one-to-one
      - 3) There exists "efficient" inversion algorithm D(k,y)
    - PRP can be used interchangably with the term 'block cipher'
    - Example PRPs: 3DES, AES, ...:
      - AES: K * X -> X, where K = X = {0,1}^128
      - 3DES: K * X -> X, where X = {0,1}^64, K = {0,1}^168
    - Fuectionally, any PRP is also a PRF. A PRP is a PRF where X=Y
      and is efficiently invertible.
    - Secure PRFs:
      - Let F: K * X -> Y be a PRF
      - Let Funs[X,Y] be the set of all functions from X to Y (really big)
      - Sf = { F(k,⋅), such that k ∈ K } ⊆ Funs[X,Y], or in other
        words Sf is a set of functions that were curried with k, which
        is a subset of the set of all functions (that take X and
        output Y)
      - Intuition: we can say that PRF is secure if a random function
        in Funs[X,Y] is indistinguishable from a random function in Sf.
    - An easy application PRF => PRG:
      - Let F: K x {0,1}^n -> {0,1}^n be a secure PRF. Then G: K ->
        {0,1}^nt is a secure PRG:
        - G(k) = F(k,0) || F(k,1) || ... || F(k,t)
        - This process should be parallelizable
        - The security of G follows from the property of PRF.
*** [[https://class.coursera.org/crypto-007/lecture/13][The Data Encryption Standard (22 min)]]
- In order to describe how a specific block cipher works, we need to
  specify how its Key Expansion and Round Function works
- The Data Encryption Standard:
  - Based on an earlier standard called Lucifer, developed by a man
    called Horst Feistel at IBM.
  - Lucifer had key-length of 128 bits and a block length of 128 bits
  - The modified version that was adopted by NBS, now called DES, has
    a key length of 56 bits and a block length of 64 bits.
  - Limiting the key length to only 56 bits came back to bite DES in
    the ass, as it is vulnerable to an exhaustive search.
  - Broken in 1997
  - NIST (new NBS) adopts Rijndael as AES to replace DES.
  - Widely used in banking and commerce
- DES: core-idea: Feistel Network:
  - Given functions f_1, ..., f_d: {0,1}^n -> {0,1}^n that are
    arbitraty functions, that is, they don't have to be reversible, we
    want to build an invertible function F: {0,1}^2n -> {0,1}^2n
  - Feistel network is a series of steps that follow the structure:
    - We start with R_0 for the first Right Bit, L_0 for the first
      Left Bit, and f_1 for the first function.
    - We get L_1 from applying f_1 to R_1, and R_1 from XOR'ing L_0
      with the output of f_1(R_0).
    - We keep applying the same process until we reach R_d-1, L_d-1
      and fd, and the output of the process (the function F) is Rd and Ld.
    - We claim that for all arbitrary functions f_1 to f_d, the
      outlined functiond F is invertible.
    - We can proove this by constructing the inverse of the process:
      - The inverse of one round of input is:
        - R_i = L_i+1, L_i = f_i+1(L_i+1) XOR R_i+1
      - By repeating this step from d to 0, we have completed the inverse.
      - Because the process is so similar, this is very attractive
        because the hardware that encrypts can be used for decrypting.
    - Used in many block ciphers, although not in AES.
  - Theorem (Luby-Rackoff '85):
    - If f: K x {0,1}^n -> {0,1}^n is a secure PRF, then:
    - 3-round Feistel F: K^3 x {0,1}^2n -> {0,1}^2n is a secure PRP.
    - In other words, you end up with a inversible function that is
      truly indistinguishable from a truly random inversible function.
    - As you recall, the definition of a seceure block cipher is that
      it's a secure Pseudo Random Permutation.
  - DES is a 16 round Feistel network:
    - f_1, ..., f_16: {0,1}^32 -> {0,1}^32, f_i(x) = F(k_i, x), where
      k_i is the round key that we get from key expansion.
    - We start with a 64-bit input that gets fed into a Initial
      Permutation, that mixes the input bits around, this then gets
      fed into the 16-round Feistel network, which gets fed into Final
      Permutation that undoes the shuffling around of IP. We are left
      with a 64-bit output.
  - The function F(k_i, x):
    - Takes a 32-bit value and a 48-bit round key
    - x gets expanded into 48 bits by duplicating and mixing
    - This expanded x gets xor'ed with the round key
    - The resulting 48-bits are split into eight slots of 6-bits, that get
      fed into eight "S-boxes".
    - S-boxes spit out 4-bit values based on lookup tables
    - The resulting 32-bit value goes into yet another permutator,
      where it gets mixed and matched around.
    - Essentially by using different round keys, we get different,
      arbitrary round functions.
  - S-boxes:
    - There are good and bad versions of S-boxes.
    - Mainly we don't want the S-boxes to be calculated via a linear
      function, because this would mean that all that DES does is
      xor'ing and moving bits around.
    - If DES was wholly linear, this would mean that it's not fully random.
    - Choosing the S-boxes at random would not be enough to make them
      secure, as the chances of them being linear would be too high.
      The key could be recovered after about 2^24 outputs.
*** [[https://class.coursera.org/crypto-007/lecture/14][Exhaustive search attacks (20 min)]]
- Our goal: given a few input output pairs (m_i, c_i = E(k, m_i)), i =
  1, ..., 3, find key k. (m_1, m_2, m_3 -> c_1, c_2, c_3)
- Lemma: Suppose that DES is an ideal cipher (2^56 random invertible functions)
  - Then for all m, c; there is at most one key that satisfies c =
    DES(k,m), with prob. 1 >= 1 - 1/256 (99.5%)
  - For two DES pairs (m_1, c_1 = DES(k, m_1)), (m_2, c_2 = DES(k,
    m_2)), the probability is ~= 1 - 1/2^71
  - For AES-128: given two inp/out pairs, the probability is 1 - 1/2^128
  - When the probability is close to one, it is practical to use an
    Exhaustive Search attack
- A company called RSA published the DES challenge, where they
  published a number of cipher texts, of which three of them contained
  known plain text "The unknown message is: XXXXX", where XXXXX was an
  unknown part of the message.
  - The challenge was to find the key that maps m_i for c_i for i = 1,
    2, 3, where i = 1-3 is the known plain text divided into three parts
    (8-bits each)
  - In 1997 Internet search (I guess he just means connected machines?)
    could solve the challenge in three months
  - In 1998 EFF build a custom hardware machine called Deep Crack which
    solved the next iteration of the challenge in three days
  - In 1999 a combined search using the two methods solved the challenge
    in just 22 hours.
  - In 2006: COPACOBANA (120 FPGAs) that cost $10k could solve it in 7 days
  - Result --> DES is completely broken by modern standards. 56 bit
    keys should not be used.
- What to do? Strenghening the DES:
  - Method 1: Triple-DES:
    - Instead of the usual E: K * M -> C, we can use 3E: K^3 x M -> C
      where 3E((k1, k2, k3), m) = E(k1, D(k2, E(k3, m))).
    - If you set all three keys to be the same, using 3DES is the same
      as normal DES, although three times slower.
    - For 3DES: key-size = 3x56 = 168 bits.
    - Why not double DES? Because it's suspectible to a Meet In The Middle attack
      - We need to find k_1, k_2 such that E(k_1, E(k_2, M)) = C,
        which is the same as E(k_2, M) = D(k_1, C).
      - First we'll build a table that takes M, and encrypts it for
        all possible values of k_2 (56-bits). This takes time 2^56
        (for building) times log(2^56) (sorting)
      - Then we brute force k_1 and see if it matches any of the
        values in our table.
      - Running time for the attack: 2^56 + 2^56 * log(2^56) < 2^63,
        which is much less than we'd expect from our original 2^112 key.
      - The same attack on 3DES reduces the time from 2^168 to 2^118.
    - Method 2: DESX:
      - Define EX -> EX((k_1, k_2, k_3), m) = k_1 XOR E(k_2, m XOR k_3)
      - Key-length for DESX is 184 bits, but easy attack time is
        2^(64+56) = 2^120.
  - 3DES is a national standard.
*** [[https://class.coursera.org/crypto-007/lecture/15][More attacks on block ciphers (16 min)]]
- You should never ever design your own block cipher.
- Attacks on the implementation:
  - Measuring time to do enc/dec
  - Measuring power for enc/dec
  - Fault attacks: If you can cause the hardware that
    decrypts/encrypts to malfunction (i.e. by heating), you might be
    able to get access to the cipher before the last round of the
    encryption process, and this can reveal the key.
- Linear and differential attacks:
  - Given many inp/out pairs, can we recover the key in time less than
    what exhaustive search would yield us.
  - If the attacker has the message, and the cipher text, and he XORs
    sub bits of the message together, and subsets of the cipher text
    together, does this reveal any information about the xors of the
    sub bits of the key?
    - For DES this exists with epsilon 1/2^21 ~= 0.0000000477
  - Pr[ m[i_1] xor ... xor m[i_r] XOR c[j_j] xor ... xor c[j_v] =
    k[l_1] xor ... xor k[l_u] ] = 1/2 + epsilon
  - Given 1/epsilon^2 random (m, c=DES(k, m)) pairs, then:
    - k[l_1, ..., l_u] = MAJ [ m[i_1, ..., i_r] XOR c[j_j, ... j_v] ],
      with prob. >= 97.7
  - For DES: epsilon is 1/2^21, so with 2^42 inp/out pairs we can find
    k[l_1, ... l_u] in time 2^42
  - So roughly speaking we can find the remaining 56-14=42 bits in
    time 2^42. Then we can brute force the rest via exhaustive search,
    giving us total attack time of ~= 2^43 (<< 2^56)
  - This is because a tiny bit of linearity in S_5-box lead to the
    2^42 attack.
- Quantum attacks:
  - Hypothetical quantum computers excel at the kind of search task
    that is required with cryptography. This cuts the search time down
    by x^(1/2).
  - This would cut down the search space: With DES: ~= 2^28, with
    AES-128 ~= 2^64, which would still be not secure.
  - If quantum computing becomes a reality, then AES-256 would still
    be usable, as 2^128 is still too big for exhaustive search.
*** [[https://class.coursera.org/crypto-007/lecture/16][The AES block cipher (14 min)]]
- Timeline:
  - 1997: NIST publishes a request for proposal
  - 1998: 15 submissions of which five claimed attacks
  - 2000: NIST chooses Rijndael as AES (designed in Belgium)
- AES offers key sizes of 128, 192 and 256 bits, and a block size of
  128 bits.
- AES is a Subs-Perm network (not Feistel). The general idea is:
  - In a Feistel network, half the bits don't change every round. In a
    Subs-Perm network all the bits change every round.
  - 1) We xor input with the first round key.
  - 2) We go to a substitution layer (S-boxes) with lookup tables
  - 3) We go to permutation layer, where the bits get shuffled around.
  - 4) We xor the result with the next round key, and we go through
    steps 2-4 again.
  - 5) We xor with the last round key, and this is our output.
- For AES:
  - The input, which is 128 bits or 16 bytes, gets set up in a 4x4
    byte matrix.
  - The 16 byte key gets expanded into 176 bytes (11 x 16)
  - The xor of input and r-key_1 go to an invertible function (steps
    2-3) that performs ByteSub, ShiftRow and MixColumn procedures
  - We repeat this process ten times, and then xor with the eleventh
    round key.
  - The round function:
    - ByteSub: a 1-byte S-box containing 256 bytes (easily computable).
      - We use the index of our 4x4 byte matrix to lookup from the 256
        byte table the corresponding return values.
    - ShiftRows: Each row gets shifted c+1 positions
    - MixColumns: Each column gets mixed and matched.
  - The cool thing about AES is that you can write code that
    generates the tables instead of distributing the tables
    themselves. This is used in browsers, where we send the code to
    generate the tables to save bandwith, and then those tables are
    generated on the client by the browser.
- AES instructions are now included in the Intel Westmere processor,
  which claims a 14x speed-up over OpenSSL on the same hardware.
- Attacks:
  - Best key recovery attack: four times better than ex. search, which
    would make it 2^128 -> 2^126 -> not very effective.
  - Related key attack on AES-256:
    - Given 2^99 inp/out pairs from four related keys you can recover
      the key in time 2^99.
*** [[https://class.coursera.org/crypto-007/lecture/17][Block ciphers from PRGs(12 min)]]
- Can we build block ciphers from pseudo random functions?
- Let's start by finding out if we can build PRF from a PRG?
  - Let G: K -> K^2 be a secure PRG
  - Define 1-bit PRF F: K x {0,1} -> K as F(k, x∈{0,1}) = G(k)[x]
  - F: if 0 choose G(k)[0], if 1, choose G(k)[1]
  - Theorem: If G is a secure PRG, then F is a secure PRF.
  - Can we expand F?
  - We can expand G's output by making G_1 = G(G(k)[0]) || G(G(k)[1]),
    and now we have a two bit PRF: F(k, x∈{0,1}^2) = G_1(k)[x]
  - Using this same principle we can beep expanding G, and thus F.
- The GGM PRF:
  - Let G: K->K^2. define PRF F: K x {0,1}^n -> K as:
    - For input = x_0, x_1, ... x_n-i ∈ {0,1}^n, do:
    - For each K, take G(k_i)[x_i] until G(k_n-1)[x_n-1]
    - Since G is a secure PRG then F is a secure PRF over {0,1}^n
    - Not used much in practice because it's slow.
- Can we build a secure PRP from a secure PRG?
  - Yes! We just plug our GGM PRF into the Luby-Rackoff theorem, which states:
    - Three rounds of a Feistel network makes a PRF into a PRG.
*** [[https://class.coursera.org/crypto-007/lecture/18][Review: PRPs and PRFs (12 min)]]
** Week 3
*** [[https://class.coursera.org/crypto-007/lecture/22][Message Authentication Codes (16 min)]]
- Examples where confidentiality is not important, but integrity matters:
  - Protecting public binaries on a disk (vs. viruses)
  - Protecting banner ads on web pages
- Message Authentication Code (MAC):
  - Alice can ensure the integrity of her message to Bob by
    concatenating a tag to the end of her message. The tag is
    generated with tag <- S(k, m), and Bob can verify the integrity
    with a function V(k, m, t), that outputs 'yes' or 'no'.
  - MAC is the pair of algorithms S(k, m) that outputs t in /T/, and
    V(k, m, t) that outputs 'yes' or 'no'
- Integrity requires a shared secret key
  - Cyclic Redundancy Check (CRC) is an example algorithm that can
    calculate tags from messages but is keyless and is therefore used
    to check for random, not malicious errors.
  - An attacker can cancel both the message and the tag, calculate a
    new message and its CRC tag, and then forward this message
- Secure MAC's:
  - Attacker's power: chosen message attack: For m_1, m_2, ..., m_q,
    attacker is given t_i <- S(k, m_i)
  - Attacker's goal: Existential Forgery
    - Produce some new valid message/tag pair (m,t), that are ∉
      {(m_1, t_1), ..., (m_q, t_q)}
  - MAC is secure if attacker cannot produce a valid tag for a new message
  - Given (m,t), attacker should not be able to even produce (m,t')
    for n ≠ t'
  - The security game: Challenger gives the attacker [m_1, ..., m_q]
    and [t_1, ..., t_q] for those messages, and the attacker will try
    to produce a new (m,t) pair that isn't part of any of the pairs he
    received from before.
  - I = (S,V) is a secure MAC if for all "efficient" A: Adv_MAC[A,I] =
    Pr[Challenger outputs 1] is "negligible"
  - In order for A's advantage to be negligible, the tag has to be
    sufficiently long (64, 96, 128... bits), lest the attacker be able
    to just guess the tag.
*** [[https://class.coursera.org/crypto-007/lecture/23][MACs Based On PRFs (10 min)]]
- If F: K*K -> Y is a secure PRF and 1/|Y| is negligible (i.e. |Y| is
  large), then I_F is a secure MAC.
- For every eff. MAC adversary A attacking I_F, there exists an eff.
  PRF adversary B attacking F so that Adv_MAC[A, I_F] <= Adv_PRF[B,
  F] + 1/|Y|
- Therefore I_F is secure as long as |Y| is large, say |Y| = 2^80
- Examples:
  - AES: a MAC for 16-byte messages
  - The McDonalds problem: how do we convert a Small-Mac into a Big-MAC?
  - Two main constructions used in practice:
    - CBC-MAC (banking)
    - HMAC (Internet protocols: SSL, IPsec, SSH, ...)
  - If (S,V) is a MAC that is based on a secure PRF outputting n-bit
    tags, the truncated MAC outputting w bits is secure as long as
    1/2^w is still negligible (say w >= 64)
*** [[https://class.coursera.org/crypto-007/lecture/24][CBC-MAC and NMAC (20 min)]]
- Our goal: given a PRF for short messages (AES), construct a PRF for
  long messages
- Encrypted CBC-MAC (ECBC)
  - The message gets split into blocks and each block gets fed to F(k,_) (a
    random PRP), which then gets xored with the next message block,
    that in turn gets fed to F(k, _). This process goes on until the
    last message block, and it's called raw CBC.
  - The result of raw CBC gets fed once more into F, but this time the
    key is different. The final output is the tag.
- NMAC (nested MAC):
  - F is a PRF. The message gets split into blocks, and F(k_0, m_0)
    becomes the key for the next step F(k, m_1). This goes on and on
    until we reach the last step. This process is called `cascade`.
  - The final output of cascade is t, which is still in the key-space
    /K/. To make cascade secure we have to concat a fixed pad to t,
    and input it to F once more, that uses a new key.
- Why the last step?
  - Cascade is vulnerable to the /extension attack/ (in fact, it's the
    only attack against it).
  - cascade(k,m) can gives the attacker cascade(k, m||w) for any w.
    This is because by adding w, the attacker can calculate F(t, w)
    and get a new tag, breaking the MAC.
  - For raw CBC can be broken with a one chosen msg attack as well:
    - A chooses one block message m and requests a tag /t/ for it, which
      is F(k,m)
    - A then can output the received tag as the MAC forgery for the
      2-block message m || t xor m
    - rawCBC(k, (m, t xor m)) = F(k, F(k,m) xor (t xor m)) = F(k, t
      xor (t xor m)) = t
- CMC-MAC is secure as long as q << sqrt(|X|), NMAC is secure as long
  as q << sqrt(|X|) (64 for AES-128), where q is the number of
  messages MAC'd with k
- Both CMC-MAC and NMAC are vulnerable to a generic extension attack
  if we overcross these bounds:
  - The extension property states that for all x, y, w F_Big(k,x) =
    F_Big(k,y) => F_Big(k, x||w) = F_Big(k, y||w)
  - For a sample of q > sqrt(|X|) messages, we are likely to find a
    m_u and m_v that share the same tag. This is because of the
    [[https://en.wikipedia.org/wiki/Birthday_paradox][Birthday Paradox]]
  - Knowing the tag for m_u and m_u||w allows the attacker to know the
    (m_v||w, t)
- ESBC-MAC is commonly used as an AES-based MAC
- NMAC is usually not based on AES or 3DES because it's slow. NMAC is
  a basis of a popular MAC called HMAC
*** [[https://class.coursera.org/crypto-007/lecture/25][MAC padding (9 min)]]
- Problem: What if message length is not a multiple of the block size?
  - Can we use a fixed padding?
    - Nope! Because pad(m) = pad(m||0). The tag for m000 is the same
      as m0|00, and the attacker can thus perform an existential
      forgery attack
    - This would be very risky when dealing with amounts of money for example
- CBC MAC padding
  - For security, padding must be invertible, i.e. m_0 ≠ m_1 =>
    pad(m_0) ≭ pad(m_1).
  - ISO proposition: pad with "100...00" and add a new dummy block if
    needed. The "1" indicates the beginning of pad. A scanning
    algorithm will delete everything to the right of "1" including the
    "1" itself.
  - Important! If the message is evenly divisible by the block length
    you must include the dummy block, or the MAC is broken.
- Alternative to ISO proposal: CMAC
  - No final encryption step (extension attack is thwarted by last
    keyed xor)
  - We instead use three keys (k, k_1, k_2), where k_1 and k_2 are
    derived from k via a PRF.
  - If the last block needs padding, we pad it with "100..." and
    encrypt it with k_1.
  - If the last block doesn't need padding, we encrypt it with k_2.
  - This removes the need for the dummy block, as the ambiguity is
    resolved by alternating between k_1 or k_2.
*** [[https://class.coursera.org/crypto-007/lecture/26][PMAC and the Carter-Wegman MAC (16 min)]]
- PMAC (Parallel MAC)
  - The previous MAC's mentioned are sequential and can't thus be run
    on parallel.
  - PMAC gets around this by using the following construction:
    - 1) Message gets broken down to blocks
    - 2) Each block is xor'ed with the output of an algorithm P that
      takes a key and a step value (from 1 to the amount of blocks in
      the message)
    - 3) Each result gets fed into F(k_1, _), except for the last
      block, but this is a technicality
    - 4) The outputs of the previous step are all xor'ed together
    - 5) The result gets fed once more into F(k_1, _), and the output
      is our tag
  - Why do we need P?
    - The attacker can perform an existential forgery by swapping two
      blocks in a message
    - P enforces order into the process
    - P is an easy to compute function and doesn't add much to the
      running time of PMAC
  - PMAC uses a padding similar to CMAC
  - PMAC Security Theorem: PMAC is secure as long as qL << sqrt(|X|)
  - PMAC is incremental
    - There is an added benefit: If a block inside the message
      changes, the previous MAC's would have to recompute the entire
      message. PMAC can compute a new message based only on the
      changed block.
    - Suppose F is a PRP. We can then perform the change by xor'ing
      F-1(k_1,tag) with F(k_1, m[1] xor P(k,1)) and F(k_1, m'[1] xor
      P(k,1)), and then applying F(k_1, _) again.
- One time MAC (analog of one time pad)
  - The security "game" from earlier is changed with the stipulation
    that the Adversary is only able to request one (m,t) pair, after
    which he has to deliver a valid (m', t') pair back to the Challenger.
  - This limitation allows One-time MAC to be secure against
    infinitely powerful adversaries, while being much faster than
    PRF-based MAC's.
  - Let q be a large prime, and our key be two random integers in
    [1,q]. Our message is divided into blocks m[1],...m[L].
  - Then our signing function S = S(k,msg) = P_msg(k)+a (mod q), where
    P_msg(x) is polynomial m[L] * x^L + ... + m[1] * x of the degree L.
  - Given S(key, msg_1), A has no info about S(key, msg_2), but given
    the two, A can break the tag for m_3 if the same key is used
- Carter-Wegman MAC
  - We can build a secure Many-time MAC from a One-time MAC:
    - Let (S,V) be a secure one-time MAC over (/K/, /M/ and tag in {0,1}^n)
    - Let F: K_F * {0,1}^n -> {0,1}^n be a secure PRF
    - r be a random nonce r <-- {0,1}^n
    - CW((k1, k2), m) = (r, F(k_1, r) xor S(k_2, m))
    - In other words, we use the one-time MAC S(k_2, m), which is fast
      even when m is long, and then we xor it with the output of our
      PRF that accepts k_1 and the random nonce.
    - Theorem: If (S,V) is a secure one-time MAC and F a secure PRF
      then CW is a secure MAC outputting tags in {0,1}^2n.
    - CW MAC is an example of a randomized MAC: If you compute the tag
      for the same message twice, you get two different tags that are
      both valid.
    - To verify CW with V you run V(k_2, m, F(k_1,r) xor tag)
*** [[https://class.coursera.org/crypto-007/lecture/27][Introduction (11 min)]]
- Collision Restistance:
  - Let H: M -> be a hash function, where (|M| >> |T|)
  - A collision for H is a pair m_0, m_1 ∈ M such that H(m_0) = H(m_1)
    and m_0 ≠ m_1
  - A function H is collision resistant if it's hard to find
    collisions for H, or in other words for all explicit "eff" algs A
    Adv_CR[A,H] = Pr[ A outputs collision for H ] is "neg".
  - Example: SHA-256
- MAC's from Collision Resistance
  - Let I be a MAC for short messages over (/K/, /M/, /T/) (e.g. AES)
  - Let H be a hash function: M_big -> M, i.e. it takes a long message
    and outputs a smaller hash message of it
  - Our new MAC I_big = (S_big, V_big) is S_big(k,m) = S(k, H(m)) ;
    V_big(k,m,t) = V(k, H(m), t).
  - Theorem: If I is a secure MAC and H is collision resistant then
    I_big is a secure MAC.
  - Example: S(k,m) = AES_2-block-cbc(k, SHA-256(m)) is a secure MAC
- Why is Collision Resistance necessary?
  - Suppose A can find m_0 ≠ m_1 where H(m_0) = H(m_1), then S_big is
    insecure under one chosen message attack where:
    - 1) A asks for t <- S(k, m_0)
    - 2) A outputs (m_1, t) as a forgery
*** [[https://class.coursera.org/crypto-007/lecture/28][Generic birthday attack (16 min)]]
- Generic attack on C.R. functions
  - Let H: M -> {0,1}^n be a hash function where |M| >> 2n, i.e. the
    message space is much much larger than the output space
  - Generic algorithm for finding collisions in time O(2^(n/2))) hashes:
    - 1) Choose 2^(n/2) random messages in M: m_1, ..., m_2^(n/2)
    - 2) For i = 1, ..., 2^(n/2) compute t_i = H(m_i) ∈ {0,1}^n
    - 3) Look for a collision (t_i = t_j), if not found, go back to
      step 1.
  - How many times do we have to iterate this process before we find a
    collision?
  - The Birthday Paradox:
    - Let r_1, ..., r_n {1,...,B} be independent identically
      distributed integers, then our theorem is that when n = 1.2 *
      sqrt(B), then Pr[ ∃i≠j, r_i = r_j ] >= 0.5
    - Uniform distribution is the worst case scenario for the birthday
      paradox. This is to say, that if, as in the real world is the
      case, the birthdays are biased towards some time period, then
      this factor makes the chances of finding a matching pair more likely.
    - If you put N people in a room, then:
      | # People    | Pr[collision] |
      |-------------+---------------|
      | 1.2k/10e6   |           50% | n = 1.2 * sqrt(B)
      | 500/10e6    |           10% | 
      | sqrt(B)/B   |           40% |
      | 2*sqrt(B)/B |           90% |
      In other words, the chances of a collision progress slow until
      we reach about sqrt(B), and then it increases very fast.
  - So to answer our previous question, since the chances of finding a
    collision for 2^(n/2) items is roughly 0.5, then we only need to
    expect to run this algorithm 2 times to find a match. This puts
    the running time of the algorithm at O(2^(n/2)). This algorithm
    takes a lot of space O(2^(n/2)).
- Sample C.R. hash functions:
  | function  | digest size | speed (MB/s) | attack time |
  |-----------+-------------+--------------+-------------|
  | SHA-1*    |         160 |          153 |        2^80 |
  | SHA-256   |         256 |          111 |       2^128 |
  | SHA-512   |         512 |           99 |       2^256 |
  | Whirlpool |         512 |           57 |       2^256 |
  SHA-1 is not considered safe by todays standards, eventhough a
  collision from it has never been found.
*** [[https://class.coursera.org/crypto-007/lecture/29][The Merkle-Damgard Paradigm (12 min)]]
- Our goal: Build a collision resistant (C.R.) hash functions
- Step 1: Given C.R. function for _short_ messages, construct C.R.
  function for _long_ messages
- In the Merkle-Damgard iterated construction we have:
  - A fixed IV
  - h: T x X -> T, a compression function
  - m[0],...,m[n] message blocks
  - A padding block, BP: A sequence of 100...0 || msg length coded as
    64 bits
  - We obtain H: X^(>=L) -> T, a tag in the tag-space.
*** [[https://class.coursera.org/crypto-007/lecture/30][Constructing compression functions (8 min)]]
- Building a compression function from a Block Cipher
  - Let E: K x {0,1}^n -> {0,1}^n be an ideal block cipher
  - The Davies-Meyer compression function: h(H, m) = E(m, H) xor H
  - Finding a collision h(H,m)=h(H',m') takes O(2^(n/2)) evaulations
    of (E,D), which is the best we could have hoped for.
  - The SHA-functions all use Davies-Meyer compression function.
  - There are other block cipher constructions:
    - Miyaguchi-Preneel: h(H,m) = E(m,H) xor H xor m
    - h(H,m) = E(H xor m, m) xor m
    - In total there are 12 variants like this
    - The other variations are insecure
- Case study: SHA-256
  - Building blocks:
    - Merkle-Damgard function
    - Davies-Meyer compression function
    - Block cipher: SHACAL-2
- Provable compression functions:
  - Built using "hard" problems from Number Theory.
  - Called provable because finding a collision would mean solving a
    "hard" number theoretical problem, and because these problems are
    considered intractable, then this property makes them collision resistant.
  - We choose a random 2000-bit (huge) prime, and a random 1 <= u, v
    <= p integers. For m,h ∈ {0,...,p-1}, define h(H,m) = u^h * v^m
    (mod p).
  - Finding a collision for h(.,.) is as hard as solving
    "discrete-log" modulo p, which everyone agrees is a "hard" problem.
*** [[https://class.coursera.org/crypto-007/lecture/31][HMAC (7 min)]]
- 
