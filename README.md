When I was learning programming, I was always annoyed by solving uninteresting tasks during practical exercises. In this repository, I address the task of implementing RSA encryption and decryption in the MIT Scheme language. For various reasons discussed, the implementation cannot be used for cryptographic information protection; it is solely for educational purposes.

# Key generation involves finding prime numbers.

RSA belongs to asymmetric encryption algorithms: if an open key is used for encryption, a private key is used for decryption, and vice versa. The first property allows anyone to encrypt a message with the public key for the owner of the private key, thus ensuring its confidentiality. The second property allows the key owner to encrypt the message hash with the private key, so that anyone can decrypt the encrypted hash, compare it with the message hash, and determine whether the message has been modified.  

The first stage of key generation involves randomly selecting two sufficiently large prime numbers, p and q. A natural number x is called prime if it has exactly two distinct divisors: 1 and x. All other divisors of the number are located in the range from 2 to the square root of x, but it is sufficient to check for divisibility only by prime divisors belonging to this range.

```
(define (Primes n)
  (define (prime? n primes)
    (define (iter divisors)
      (or (null? divisors)
          (let ([divisor (car divisors)])
               (or (> (* divisor divisor) n)
                   (and (< 0 (remainder n (car divisors))) (iter (cdr divisors))))))
      )
    (iter primes)
    )
  (define (iter primes i candidate)
    (cond 
      ((= i n) primes)
      ((prime? candidate (reverse primes)) (iter (cons candidate primes) (+ i 1) (+ candidate 1)))
      (else (iter primes i (+ candidate 1)))
      )
    )
  (iter '() 0 2)
  )
(define primes (Primes 100))
(define p (car primes))
(define q (car (drop 10 primes)))
```
The product of the found prime numbers is the first element of both the public and private keys. The presented algorithm allows finding only the first million prime numbers within a reasonable time. In RSA implementations used for information security, algorithms for finding prime numbers are employed, enabling the discovery of prime numbers with a large number of digits. Due to the fact that the best-known algorithm for factoring a number into prime factors operates in time proportional to the exponent of the number of digits, it is considered impossible to reconstruct a pair of prime numbers based on the considered public key element.
```
(define n (* p q))
```

# Key Generation — Finding a Mutually Prime Number

To compute the second elements of the public and private keys, the value phi (φ) is utilized, equal to Euler's totient function, calculated on n. Euler's totient function of x is the count of natural numbers less than x that are coprime to x. For n, this count is equal to the product of (p-1) and (q-1).

```
(define fi (* (- p 1) (- q 1)))
```

The second element of the public key is chosen as a random number e in the range from 1 to φ, such that it is coprime with φ, i.e., their greatest common divisor is 1.

```
(define (CoprimesLess n)
  (define (iter accumulator candidate)
    (cond
      ((= 1 candidate) accumulator)
      ((= 1 (gcd n candidate)) (iter (cons candidate accumulator) (- candidate 1)))
      (else (iter accumulator (- candidate 1)))
      )
    )
  (iter '() (- n 1))
  )
(define e (car (drop 5 (CoprimesLess fi))))
```
To find the greatest common divisor, the Euclidean algorithm can be used.

# Key Generation — Operations in the Ring of Residues

One of the objects studied in number theory is the ring of residues. The ring of residues modulo k consists of integers from 0 to k-1 with operations of addition and multiplication. Addition in the ring of residues (a + b mod k) differs from addition in the group of integers in that if the result of the addition becomes greater than k, then k is subtracted from it, bringing the result back into the ring. Intuitively, the ring is obtained from the interval by connecting its ends.

As in the group of integers, multiplication in the ring of residues can be defined using addition, and exponentiation can be defined using multiplication. Like in the group of integers, the resulting addition and multiplication operations will be associative, meaning:  
a + (b + c mod k) mod k = (a + b mod k) + c mod k  
a * (b * c mod k) mod k = (a * b mod k) * c mod k

The second element of the public key should be a number d such that its product with e in the ring of residues modulo n is 1, i.e., it is a multiplicative inverse element. Below is the algorithm for finding such an element using the extended Euclidean algorithm.

```
(define (MultInverseModN a n)
  (define (iter a-prev a r-prev r)
    (if (>= 1 r) a (let* ([r-next (remainder r-prev r)]
                          [q (quotient r-prev r)]
                          [a-next (- a-prev (* q a))])
                         (iter a a-next r r-next)))
    )
  (let ([result (iter 0 1 n a)]) (if (< 0 result) result (+ n result)))
)
(define d (MultInverseModN e fi))
```
# Encryption and Decryption

Using the RSA algorithm, it is possible to encrypt messages represented as a series of numbers \(M\) within the range from 0 to \(n-1\). Encryption involves raising \(M\) to the power of \(e\) in the ring of residues modulo \(n\), while decryption involves raising it to the power of \(d\). Thanks to the associativity of multiplication, we can exponentiate \(x\) in \(log(x)\) operations.

```
(define (PowerModN a b n)
  (define (iter accumulator multiplicator power)
    (if
      (= power 0)
      accumulator
      (let
          ((new_accumulator (if (even? power) accumulator (remainder (* accumulator multiplicator) n))))
          (iter new_accumulator (* multiplicator multiplicator) (quotient power 2))
        )
      )
    )
  (iter 1 a b)
  )
```

# Check Example

In my example, the public key is represented as the pair (250483, 31), and the private key is the pair (250483, 32191). The encrypted message for 123456 is 133240.

## References
en.wikipedia.org/wiki/RSA
en.wikipedia.org/wiki/Integer_factorization
en.wikipedia.org/wiki/Euler%27s_totient_function
en.wikipedia.org/wiki/Euclidean_algorithm
en.wikipedia.org/wiki/Modular_arithmetic
en.wikipedia.org/wiki/Extended_Euclidean_algorithm
en.wikipedia.org/wiki/Exponentiation_by_squaring
