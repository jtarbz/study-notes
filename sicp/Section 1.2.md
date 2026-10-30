---

## code: SICP/1_2.rkt

# SICP 1.2

## Example: counting change

```scheme
; how many ways to make change of $1.00 given half-dollars, quarters, dimes, nickels, and pennies?
; see the tree-recursive procedure below
(define (count-change amount)
  (cc amount 5))

(define (cc amount kinds-of-coins)
  (cond ((= amount 0) 1)
        ((or (< amount 0) (= kinds-of-coins 0)) 0)
        (else (+ (cc amount
                     (- kinds-of-coins 1))
                 (cc (- amount
                        (first-denomination kinds-of-coins))
                     kinds-of-coins)))))

(define (first-denomination kinds-of-coins)
  (cond ((= kinds-of-coins 1) 1)
        ((= kinds-of-coins 2) 5)
        ((= kinds-of-coins 3) 10)
        ((= kinds-of-coins 4) 25)
        ((= kinds-of-coins 5) 50)))

(count-change 100)

; challenge - find an iterative recursive solution
; model the problem state: what can we remember?
; --> i worked this out on paper. it's a 2D DP problem.
;     you can cheese with just one or two rows in memory,
;     but time complexity does not beat O(a * c).
;     and it's hard to write in scheme! (for now ...)
```

## Exercise 1.11

```scheme
; recursive f:
(define (f111 n)
  (cond ((< n 3) n)
        (else (+
               (f111 (- n 1))
               (* 2 (f111 (- n 2)))
               (* 3 (f111 (- n 3)))))))

(f111 10)

; iterative f:
; need to keep track of f(n - 3), f(n - 2 ), and f(n - 1).
; (counting down is a cleaner implementation, whatever, this is correct)
(define (f111-fast n)
  (define (f111-iter m1 m2 m3 counter)
    (cond ((= counter n) (+ m1 (* 2 m2) (* 3 m3)))
          ((< counter 3) (f111-iter counter (- counter 1) (- counter 2) (+ counter 1)))
          (else (f111-iter (+ m1 (* 2 m2) (* 3 m3)) m1 m2 (+ counter 1)))))
  (if (< n 3)
      n
      (f111-iter 0 0 0 0)))

(f111-fast 10)
```

## Exponentiation

```scheme
; linear recursive method below
(define (expt b n)
  (if  (= n 0)
       1
       (* b (expt b (- n 1)))))

(expt 2 8)

; we can formulate an equivalent linear iteration (and get better space)
(define (expt-iter b counter product)
  (if (= counter 0)
      product
      (expt-iter b (- counter 1) (* b product))))

; of course, we can do better by the method of successive squaring.
; this method breaks down into cases (even/odd).
(define (square x) (* x x))
(define (even? x)
  (= (remainder x 2) 0))
(define (fast-expt b n)
  (cond ((= n 0) 1)
        ((even? n) (square (fast-expt b (/ n 2))))
        (else (* b (fast-expt b (- n 1))))))

(fast-expt 2 8)
```

## Exercise 1.16

```scheme
; exercise 1.16: adapt fast-expt to an iterative procedure.
; here we accumulate the result in b for each even case; then we shift
; that result over to a in the odd cases. this felt harder than it should
; have to tweak correctly.
(define (fast-expt-iter a b n)
  (cond ((= n 0) a)
        ((even? n) (fast-expt-iter a (square b) (/ n 2)))
        (else (fast-expt-iter (* a b) b (- n 1)))))

(define (fast-expt-iter-express b n)
  (fast-expt-iter 1 b n))

(fast-expt-iter-express 3 7)
```

## Exercise 1.17

```scheme
(define (double x)
  (+ x x))

(define (halve x)
  (/ x 2))

(define (mult-rr x y)
  (cond ((= y 1) x)
        ((even? y) (mult-rr (double x) (halve y)))
        (else (+ (mult-rr x (- y 1)) x))))

(mult-rr 8 7)
```

## Exercise 1.18

```scheme
(define (mult-iter a x y)
  (cond ((= y 0) a)
        ((even? y) (mult-iter a (double x) (halve y)))
        (else (mult-iter (+ a x) x (- y 1)))))

(define (mult x y)
  (mult-iter 0 x y))

(mult 15 15)
```

## Exercise 1.19, fast fib

```scheme
; this requires the author to be "clever"
; recall that in linear iterative fib, we take a <- a + b and b <- a (call this transformation T).
; so fib(n) is applying T^n to (1, 0).
; general cases are the following ...
; 1. a <- bq + aq + ap
; 2. b <- bp + aq
; for fib, we have the special case p = 0, q = 1.
(define (fib-iter a b p q count)
  (cond ((= count 0) b)
        ((even? count)
         (fib-iter a
                   b
                   (+ (square p) (square q)) ; square cases for p and q are found by symbolically applying T twice (T^2)
                   (* q (+ q (* 2 p)))
                   (/ count 2)))
        (else (fib-iter (+ (* b q) (* a q) (* a p)) ; in the odd subproblem, we shift pending transformations to our state (a, b) and massage out the even case
                            (+ (* b p) (* a q))
                            p
                            q
                            (- count 1)))))

(define (fib x)
  (fib-iter 1 0 0 1 x))

(fib 7)
```

## GCD iterative process

See also: [[Lamé's Theorem]]

```scheme
(define (gcd a b)
  (if (= b 0)
      a
      (gcd b (remainder a b))))

(gcd 28 8)
```

## Example 1.2.6, testing primality

```scheme
(define (smallest-divisor n)
  (find-divisor n 2))

(define (find-divisor n test-divisor)
  (cond ((> (square test-divisor) n) n)
        ((divides? test-divisor n) test-divisor)
        (else (find-divisor n (+ test-divisor 1)))))

(define (divides? a b)
  (= (remainder b a) 0))

(define (prime? n) ; note this procedure has order of growth theta(sqrt(n)) on prime inputs --> with a twist!
  (= n (smallest-divisor n)))

; ...
; since the number n is actually encoded as a k = log(n) bit string, the algorithm above
; does NOT run in polynomial time. the value n is exponential to the length k of the actual input.
; yes, this is confusing.

(prime? 2)

(prime? 8)
```

## Fermat test

See also: [[Fermat's Little Theorem]]

```scheme
; we can build another test for primality on top of Fermat's Little Theorem.
; given n, pick a random number a < n and compute the remainder of a^n mod n.
; if the result is not equal to a, then n is not prime. if it is a, it is likely prime.
; in the latter case, continue to test with random numbers to increase our confidence.
; this is the Fermat test.

(define (expmod base exp m)
  (cond ((= exp 0) 1)
        ((even? exp)
         (remainder (square (expmod base (/ exp 2) m))
                    m))
        (else
         (remainder (* base (expmod base (- exp 1) m))
                    m))))

(define (fermat-test n)
  (define (try-it a)
    (= (expmod a n n) a))
  (try-it (+ 1 (random (- n 1)))))

(define (fast-prime? n times)
  (cond ((= times 0) true)
        ((fermat-test n) (fast-prime? n (- times 1)))
        (else false)))

(fast-prime? 7 100)
(fast-prime? 4294967086 100)
(fast-prime? 127 2)

; notably, this algorithm is only probably correct. however, there are numbers
; that will always fool the fermat test because they are not prime and yet
; have the property that a^n is congruent to a modulo n for all integers a < n.
; they are called carmichael numbers.
```

## Exercise 1.28: implementing the Miller-Rabin test

```scheme
; here we use the adjusted FLT checking a^(n - 1) \equiv 1 (mod n).
; in particular, we are hijacking the expmod procedure to check for
; nontrivial square roots modulo n.
; if such a root exists, n must not be prime. this trick allows us to
; identify carmichael numbers as composite.
(define (nontriv-square x m)
  (define y (remainder (square x) m))
  (if (and (= y 1) (not (= x 1)) (not (= x (- m 1))))
      0
      y))

  
(define (expmod-2 base exp m) ; expmod, but it signals if we find a nontrivial sqrt of 1
  (cond ((= exp 0) 1)
        ((even? exp)
         (nontriv-square (expmod-2 base (/ exp 2) m) m))
        (else
         (remainder (* base (expmod-2 base (- exp 1) m))
                    m))))

(define (miller-rabin n)
  (define (try-it a)
    (= (expmod-2 a (- n 1) n) 1))
  (try-it (+ 1 (random (- n 1)))))

(define (fast-prime-2? n times)
  (cond ((= times 0) true)
        ((miller-rabin n) (fast-prime-2? n (- times 1)))
        (else false)))

(fast-prime-2? 47 100)
```