# typed-racket-performance

A demonstration of contract-checking overhead when using Typed Racket code from untyped Racket.

tr-pdfs contains a copy of https://github.com/takikawa/tr-pfds. tr-pdfs-nocheck replaces `#lang typed/racket` with `#lang typed/racket/no-check`. I've compiled all racket source files to bytecode in each with `raco make`.

determinance3.rkt contains untyped code that uses data structures from the tr-pfds package, and mathtest-determinance.rkt contains a test and timing code.

## Running

To run with `#lang typed/racket`:

```
racket -S tr-pfds mathtest-determinance.rkt
```

To run with `#lang typed/racket/no-check`:

```
racket -S tr-pfds-nocheck mathtest-determinance.rkt
```

## Results

For me:

```
$ racket -S tr-pfds-nocheck mathtest-determinance.rkt

cpu time: 10 real time: 9 gc time: 0
#t


$ racket -S tr-pfds mathtest-determinance.rkt

cpu time: 14623 real time: 14610 gc time: 232
#t
```

## Feature Profiler

At Leif's suggestion, I also ran the feature profiler:

```
$ racket -S tr-pfds -l feature-profile -t mathtest-determinance-feature-profile.rkt
1393 samples


Contracts
account(s) for 0.49% of total running time
74 / 14952 ms

Cost Breakdown
  43 ms : head (-> (struct/c Queue (recursive-contract (or/c () (box/c (or/c (cons/c any/c (recursive-contract g369904171177 #:chaperone)) (-> (cons/c any/c (recursive-contract g369904171177 #:chaperone)))))) #:chaperone) list? (recursive-contract (or/c () (box/c (or/c (cons/c any/c (recursive-contract g369907174179 #:chaperone)) (-> (cons/c any/c (recursive-contract g369907174179 #:chaperone)))))) #:chaperone)) any)
  41/2 ms : enqueue (-> any/c (struct/c Queue (recursive-contract (or/c () (box/c (or/c (cons/c any/c (recursive-contract g369875151164 #:chaperone)) (-> (cons/c any/c (recursive-contract g369875151164 #:chaperone)))))) #:chaperone) list? (recursive-contract (or/c () (box/c (or/c (cons/c any/c (recursive-contract g369878154166 #:chaperone)) (-> (cons/c any/c (recursive-contract g369878154166 #:chaperone)))))) #:chaperone)) (struct/c Queue any/c any/c any/c))
  21/2 ms : tail (-> (struct/c Queue (recursive-contract (or/c () (box/c (or/c (cons/c any/c (recursive-contract g369926184197 #:chaperone)) (-> (cons/c any/c (recursive-contract g369926184197 #:chaperone)))))) #:chaperone) list? (recursive-contract (or/c () (box/c (or/c (cons/c any/c (recursive-contract g369929187199 #:chaperone)) (-> (cons/c any/c (recursive-contract g369929187199 #:chaperone)))))) #:chaperone)) (struct/c Queue any/c any/c any/c))
#t
```
