# Racket
___

**Racket** is one of the Lisp programming languages family.

Racket is a good start to dive in computer science subjects especially programming, and it has a popularity among CS pioneers.

**Summary:** Racket is a Lisp programming language used for educational and learning purposes, programming language design and development, and general-purpose programming. It has a Lisp syntax, integrated language extensibility features, and modern features. Racket can be used as a solution to respond to the needs of an organization, such as programming a custom feature, educating and learning purposes, and developing scientific researching tools. Racket differs from other programming languages due to its Lisp syntax, IDE, and metaprogramming capabilities.	

____

**print statement:**

```
#lang racket
(print "hello world")
```
___

**Making Simple Images in Racket**

```
#lang racket
;Access the type for making simple images in racket
(require 2htdp/image)

; Circle
(circle 20 'outline' "red")

; Rectangle
(rectangle 40 25 'solid "blue")

; opacity of the shape
(beside
    (rectangle 25 40 255 "blue")
    (rectangle 25 40 191 "blue")
    (rectangle 25 40 127 "blue")
    (rectangle 25 40  63 "blue"))

;overlay shapes
(define circles
    (beside
      (circle 10 255 "red")
      (circle 10 191 "red")
      (circle 10 127 "red")
      (circle 10  63 "red")))
(above
    (overlay circles (rectangle 60 20 255 "blue"))
    (overlay circles (rectangle 60 20 191 "blue"))
    (overlay circles (rectangle 60 20 127 "blue"))
    (overlay circles (rectangle 60 20  63 "blue")))

;To create a pen, you use (pen color width style cap join)
(define background (rectangle 80 80 'solid "white"))
(beside
   (overlay (circle 30 'outline (pen "red" 4 'solid 'round 'round))
            background)
   (overlay (circle 30 'outline (pen "red" 6 'dot-dash 'round 'round))
            background)
   (overlay (circle 30 'outline (pen "red" 8 'short-dash 'butt 'round))
            background)
   (overlay (circle 30 'outline (pen "red" 8 'short-dash 'projecting 'round))
            background))

; variety of other basic shapes
(beside
   (ellipse 40 20 'outline "red")
   (ellipse 20 40 'solid "blue")
   (triangle 40 'solid "black")
   (star 30 'solid "teal")
   (star 20 'outline "teal"))

; Colors
(beside (circle 20 'solid (make-color 0 255 0))
          (circle 20 'solid (make-color 0 128 128))
          (circle 20 'solid (make-color 64 0 64)))

;Combining Images
;(define small-gray (circle 10 'solid "gray"))
;(define medium-red (circle 15 'solid "red"))
;(define large-black (circle 20 'solid "black"))
;(beside small-gray medium-red large-black)

;(above small-gray medium-red large-black)

;(overlay small-gray medium-red large-black)

;(overlay large-black medium-red small-gray)

; Align Control
(define small-gray (circle 10 'solid "gray"))
(define medium-red (circle 15 'solid "red"))
(define large-black (circle 20 'solid "black"))
(beside/align 'top small-gray medium-red large-black)

(beside/align 'bottom small-gray medium-red large-black)

(above/align 'left small-gray medium-red large-black)

(above/align 'right small-gray medium-red large-black)

(overlay/align 'left 'top small-gray medium-red large-black)

(overlay/align 'left 'center small-gray medium-red large-black)

(overlay/align 'left 'bottom small-gray medium-red large-black)

(overlay/align 'right 'top small-gray medium-red large-black)

(overlay/align 'right 'top large-black medium-red small-gray)

;another way to overlay images
;(define medium-red (circle 15 'solid "red"))
(define medium-black (circle 15 'solid "black"))
(overlay/offset medium-red 2 6 medium-black)

(overlay/offset medium-red 6 2 medium-black)

(overlay/offset medium-red -3 -3 medium-black)
```
[Making Simple Images in Racket.pdf](https://github.com/Akram-Alaqili/Racket/files/12612711/Making.Simple.Images.in.Racket.pdf)

**Practice:** Create Three Unique design:

```
#lang racket
(require 2htdp/image)


; nested stars
(define xxsmall-yellow (star 45 'solid "yellow"))
(define xsmall-yellow (star 45 'solid "yellow"))
(define small-white (star 55 'solid "white"))
(define medium-black (star 65 'solid "black"))
(define large-red (star 75 'solid "red"))
(define xlarge-gray (star 85 'solid "gray"))
(define xxlarge-pink (star 95 'solid "pink"))

(beside
(overlay xsmall-yellow small-white medium-black large-red xlarge-gray xxlarge-pink)

(rectangle 70 75 255 "white")

;Ukraine Flag <3
(above
(rectangle 300 75 255(make-color 0 87 183))
(rectangle 300 75 255(make-color 255 215 0)))

(rectangle 70 75 255 "white")

;PAC-MAN
(beside
(rotate 30 (wedge 70 300 "solid" "gold"))
(circle 10 'solid "black")
(circle 3 'solid "white")
(circle 10 'solid "black")
(circle 3 'solid "white")
(circle 10 'solid "black")))
```
[Output.pdf](https://github.com/Akram-Alaqili/Racket/files/12614502/Output.pdf)

____
**Making a New Language**
Reference

https://beautifulracket.com/stacker/intro.html

stacker.rkt

```
#lang br/quicklang

(define (read-syntax path port)
  (define src-lines (port->lines port))
  (define src-datums (format-datums '(handle ~a) src-lines))
  (define module-datum `(module stacker-mod "stacker.rkt"
                          ,@src-datums))
  (datum->syntax #f module-datum))
(provide read-syntax)

(define-macro (stacker-module-begin HANDLE-EXPR ...)
  #'(#%module-begin
     HANDLE-EXPR ...
     (display (first stack))))
(provide (rename-out [stacker-module-begin #%module-begin]))

(define stack empty)

(define (pop-stack!)
  (define arg (first stack))
  (set! stack (rest stack))
  arg)

(define (push-stack! arg)
  (set! stack (cons arg stack)))

(define (handle [arg #f])
  (cond
    [(number? arg) (push-stack! arg)]
    [(or (equal? * arg) (equal? + arg))
     (define op-result (arg (pop-stack!) (pop-stack!))) 
     (push-stack! op-result)]))
(provide handle)

(provide + *)
```

stacker-test.rkt

```
#lang reader "stacker.rkt"
4
8
+
3
*
```
![Testing Our Expander With Bindings 1](https://github.com/Akram-Alaqili/Racket/assets/21014412/64e6df80-56b4-49a2-900c-b19dc4341b48)

![Testing Our Expander With Bindings 2](https://github.com/Akram-Alaqili/Racket/assets/21014412/7f76b63d-ebbe-4737-a9ff-f09fc55ce1fa)

___
