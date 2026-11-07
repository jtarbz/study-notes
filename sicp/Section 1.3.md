---

## code: SICP/1_3.rkt

# SICP 1.3

## Intro: expressing the sum of a series as a higher order procedure

```scheme
(define (sum term a next b)
  (if (> a b)
      0
      (+ (term a)
      (sum term (next a) next b))))

(define (inc n) (+ n 1))

(define (cube n) (* n n n))

(define (sum-cubes a b)
  (sum cube a inc b))

(sum-cubes 1 10)

; we can even express an apx of pi
(define (pi-sum a b)
  (define (pi-term x)
    (/ 1.0 (* x (+ x 2))))
  (define (pi-next x)
    (+ x 4))
  (sum pi-term a pi-next b))

(* 8 (pi-sum 1 1000))

; or a numerical apx of an integral!
(define (integral f a b dx)
  (define (add-dx x) (+ x dx))
  (* (sum f (+ a (/ dx 2.0)) add-dx b)
     dx))

(integral cube 0 1 0.01)

(integral cube 0 1 0.001)
```

## Exercise 1.29: numerical approximation of an integral using Simpson's rule

```scheme
(define (simpson f a b n)
  (define h (/ (- b a) n))
  (define (y k) (f (+ a (* k h))))
  (define (simpson-term k)
    (cond ((= k 0) (y k))
          ((= k n) (y k))
          ((= (remainder k 2) 0) (* 2 (y k)))
          (else (* 4 (y k)))))
  (* (/ h 3) (sum simpson-term 0 inc n)))

(simpson cube 0 1 100)
(simpson cube 0 1 1000)
```

## Exercise 1.30: iterative sum

```scheme
(define (sum-iter term a next b)
  (define (iter a result)
    (if (> a b)
        result
        (iter (next a) (+ (term a) result))))
  (iter a 0))

(sum-iter cube 0 inc 10)
```

## Exercise 1.31: higher-order procedure for product

```scheme
(define (product term a next b)
  (define (iter a result)
    (if (> a b)
        result
        (iter (next a) (* (term a) result))))
  (iter a 1))

(define (factorial n)
  (define (identity x) x)
  (product identity 1 inc n))

(factorial 5)
(factorial 6)
```

## Exercise 1.32: express sum and product as special cases of accumulate

```scheme
(define (accumulate combiner null-value term a next b)
  (define (iter a result)
    (if (> a b)
        result
        (iter (next a) (combiner (term a) result))))
  (iter a null-value))

(define (acc-sum term a next b)
  (accumulate + 0 term a next b))

(define (acc-product term a next b)
  (accumulate * 1 term a next b))

(acc-sum cube 0 inc 10)
```

## Exercise 1.33: accumulate with filter

```scheme
(define (filtered-accumulate accept? combiner null-value term a next b)
  (define (iter a result)
    (if (> a b)
        result
        (iter (next a) (cond
                         ((accept? a) (combiner (term a) result))
                         (else result)))))
  (iter a null-value))

; a. sum of the squares of the primes from a to b. i am copying some helpful procedures from earlier chapters.
(define (smallest-divisor n)
  (find-divisor n 2))

(define (find-divisor n test-divisor)
  (cond ((> (* test-divisor test-divisor) n) n)
        ((divides? test-divisor n) test-divisor)
        (else (find-divisor n (+ test-divisor 1)))))

(define (divides? a b)
  (= (remainder b a) 0))

(define (prime? n)
  (= n (smallest-divisor n)))

(define (sum-primes a b)
  (define (square x) (* x x))
  (filtered-accumulate prime? + 0 square a inc b))

(sum-primes 2 23)

; b. product of positive integers less than n that are relatively prime to n.
(define (prod-rel-prime n)
  (define (rel-prime? a) (= 1 (gcd a n)))
  (define (identity x) x)
  (filtered-accumulate rel-prime? * 1 identity 1 inc (- n 1)))

(prod-rel-prime 10)
```

## Example: finding roots of equations by the half-interval method

```scheme
(define (average x y) (/ (+ x y) 2))

(define (search f neg-point pos-point)
  (define (close-enough? x y) (< (abs (- x y)) 0.001))
  (let ((midpoint (average neg-point pos-point)))
    (if (close-enough? neg-point pos-point)
        midpoint
        (let ((test-value (f midpoint)))
          (cond ((positive? test-value)
                 (search f neg-point midpoint))
                ((negative? test-value)
                 (search f midpoint pos-point))
                (else midpoint))))))

(define (half-interval-method f a b)
  (let ((a-value (f a))
        (b-value (f b)))
    (cond ((and (negative? a-value) (positive? b-value))
           (search f a b))
          ((and (negative? b-value) (positive? a-value))
           (search f b a))
          (else (error "values are not of opposite sign" a b)))))

(half-interval-method sin 2.0 4.0)
```

## Example: finding fixed points of functions

```scheme
(define tolerance 0.00001)

(define (fixed-point f first-guess)
  (define (close-enough? v1 v2) (< (abs (- v1 v2)) tolerance))
  (define (try guess)
    (let ((next (f guess)))
      (if (close-enough? guess next)
          next
          (try next))))
  (try first-guess))

(fixed-point cos 1.0)
(fixed-point (lambda (y) (+ (sin y) (cos y))) 1.0)

(define (sqrt x)
  (fixed-point (lambda (y) (average y (/ x y))) 1.0)) ; note we use average damping, the averaging of successive apxs to the soln, to help the convergence of the search

(sqrt 64)
```

## Exercise 1.35: compute phi using the fixed-point procedure

```scheme
; phi^2 = phi + 1
; so we want the fixed point of phi -> (phi + 1) / phi
(fixed-point (lambda (phi) (average phi (/ (+ phi 1) phi))) 1.0)
```

## Exercise 1.37: k-term finite continued fractions

```scheme
(define (cont-frac n d k)
  (define (iter frac i)
    (cond ((= i k) (iter (/ (n i) (d i)) (- i 1)))
          ((> i 0) (iter (/ (n i) (+ (d i) frac)) (- i 1)))
          (else frac)))
  (iter 0 k))

(cont-frac (lambda (i) 1.0)
           (lambda (i) 1.0)
           100)
```

## Exercise 1.38: approximating e with a continued fraction based on Euler's expansion

```scheme
(+ 2 (cont-frac (lambda (i) 1.0)
                (lambda (i)
                  (let ((x (remainder i 3)))
                    (cond ((= x 1) 1)
                          ((= x 2) (- i (floor (/ i 3))))
                          ((= x 0) 1))))
                100))
```

## Exercise 1.39: approximating tan(x) using Lambert's formula

```scheme
(define (tan-cf x k)
  (cont-frac (lambda (i) (cond
                           ((= i 1) x)
                           (else (- (* x x)))))
             (lambda (i) (- (* 2 i) 1))
             k))

(tan-cf 3 10)
```

## Section 1.3.4: procedures as returned values

```scheme
(define dx 0.00001)

(define (deriv g)
  (lambda (x)
    (/ (- (g (+ x dx)) (g x))
       dx)))

((deriv cube) 5)

(define (newton-transform g)
  (lambda (x)
    (- x (/ (g x)((deriv g) x)))))

(define (newtons-method g guess)
  (fixed-point (newton-transform g) guess))
```

## Exercise 1.40

```scheme
(define (cubic a b c)
  (lambda (x) (+ (cube x) (* a (* x x)) (* b x) c)))

(newtons-method (cubic 1 2 3) 1)
```

## Exercise 1.41: doubling a procedure

```scheme
(define (double g)
  (lambda (x) (g (g x))))

(((double (double double)) inc) 5) ; 21
```

## Exercise 1.42: composition of f after g

```scheme
(define (compose f g)
  (lambda (x) (f (g x))))

((compose (lambda (x) (* x x)) inc) 6)
```

## Exercise 1.43: nth repeated application of f

```scheme
(define (repeated f n)
  (define (repeat-iter fn k)
    (cond ((= k n) fn)
          (else (repeat-iter (compose f fn) (+ k 1)))))
  (repeat-iter f 1))

((repeated (lambda (x) (* x x)) 2) 5)
```

## Exercise 1.44: smoothing a function

```scheme
(define (smooth f)
  (lambda (x) (average (f (- x dx)) (f x) (f (+ x dx)))))

(define (folded-smooth f n)
  ((repeated smooth n) f))
```