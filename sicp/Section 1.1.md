---

## code: SICP/1_1.rkt

# SICP 1.1

```scheme
(define (square x) (* x x))

(square 25)

(define (abs x)
  (cond ((> x 0) x)
        ((= x 0) 0)
        ((< x 0) (- x))))

(abs 10)

(abs -100)
```

## Exercise 1.3

```scheme
(define (max a b)
  (if (> a b) a b))

(define (max3 a b c)
  (max (max a b) c))

(define (ss_three a b c)
  (if (> a b)
      (+ (square a) (square (max b c)))
      (+ (square b) (square (max a c)))))

(ss_three 1 2 3)

(ss_three 3 2 1)
```

## Example 1.1.7

```scheme
(define (sqrt-iter guess x)
  (if (good-enough? guess x)
      guess
      (sqrt-iter (improve guess x) x)))

(define (improve guess x)
  (average guess (/ x guess)))

(define (average x y)
  (/ (+ x y) 2))

(define (good-enough? guess x) ; improved good-enough?, check 
  (or
   (= (- guess (improve guess x)) 0)
   (< (/ x (abs (- guess (improve guess x)))) 0))) ; as you can see ... this side of the or is probably useless when we have fixed precision numbers

(define (sqrt x)
  (sqrt-iter 1.0 x))

(sqrt 9)
(sqrt 2)
```

## Exercise 1.8

```scheme
(define (cbrt-iter guess x)
  (if (cbrt-good-enough? guess x)
      guess
      (cbrt-iter (cbrt-improve guess x) x)))

(define (cbrt-improve guess x) ; this improvement formula is the only difference really. annoying to sub in different predicate and improve procedures (need higher order ...)
  (/ (+ (/ x (square guess)) (* 2 guess)) 3))

(define (cbrt-good-enough? guess x)
  (< (abs (- guess (cbrt-improve guess x))) (* guess 1e-12)))

(define (cbrt x)
  (cbrt-iter 1.0 x))

(cbrt 81)
```