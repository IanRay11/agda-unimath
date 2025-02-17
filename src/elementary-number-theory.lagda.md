---
title: Elementary Number Theory
---

```agda
{-# OPTIONS --without-K --exact-split #-}

module elementary-number-theory where

open import elementary-number-theory.absolute-value-integers public
open import elementary-number-theory.addition-integers public
open import elementary-number-theory.addition-natural-numbers public
open import elementary-number-theory.addition-rationals public
open import elementary-number-theory.arithmetic-functions public
open import elementary-number-theory.bezouts-lemma public
open import elementary-number-theory.binomial-coefficients public
open import elementary-number-theory.bounded-sums-arithmetic-functions public
open import elementary-number-theory.catalan-numbers public
open import elementary-number-theory.collatz-bijection public
open import elementary-number-theory.collatz-conjecture public
open import elementary-number-theory.congruence-integers public
open import elementary-number-theory.congruence-natural-numbers public
open import elementary-number-theory.decidable-types public
open import elementary-number-theory.difference-integers public
open import elementary-number-theory.dirichlet-convolution public
open import elementary-number-theory.distance-integers public
open import elementary-number-theory.distance-natural-numbers public
open import elementary-number-theory.divisibility-integers public
open import elementary-number-theory.divisibility-modular-arithmetic public
open import elementary-number-theory.divisibility-natural-numbers public
open import elementary-number-theory.divisibility-standard-finite-types public
open import elementary-number-theory.equality-integers public
open import elementary-number-theory.equality-natural-numbers public
open import elementary-number-theory.euclidean-division-natural-numbers public
open import elementary-number-theory.eulers-totient-function public
open import elementary-number-theory.factorials public
open import elementary-number-theory.falling-factorials public
open import elementary-number-theory.fibonacci-sequence public
open import elementary-number-theory.finitary-natural-numbers public
open import elementary-number-theory.finitely-cyclic-maps public
open import elementary-number-theory.goldbach-conjecture public
open import elementary-number-theory.greatest-common-divisor-integers public
open import elementary-number-theory.greatest-common-divisor-natural-numbers public
open import elementary-number-theory.group-of-integers public
open import elementary-number-theory.groups-of-modular-arithmetic public
open import elementary-number-theory.half-integers public
open import elementary-number-theory.inequality-integers public
open import elementary-number-theory.inequality-natural-numbers public
open import elementary-number-theory.inequality-standard-finite-types public
open import elementary-number-theory.infinitude-of-primes public
open import elementary-number-theory.integer-partitions public
open import elementary-number-theory.integers public
open import elementary-number-theory.maximum-natural-numbers public
open import elementary-number-theory.maximum-standard-finite-types public
open import elementary-number-theory.mersenne-primes public
open import elementary-number-theory.minimum-natural-numbers public
open import elementary-number-theory.minimum-standard-finite-types public
open import elementary-number-theory.modular-arithmetic-standard-finite-types public
open import elementary-number-theory.modular-arithmetic public
open import elementary-number-theory.multiplication-integers public
open import elementary-number-theory.multiplication-natural-numbers public
open import elementary-number-theory.multiset-coefficients public
open import elementary-number-theory.natural-numbers public
open import elementary-number-theory.nonzero-natural-numbers public
open import elementary-number-theory.ordinal-induction-natural-numbers public
open import elementary-number-theory.prime-numbers public
open import elementary-number-theory.products-of-natural-numbers public
open import elementary-number-theory.proper-divisors-natural-numbers public
open import elementary-number-theory.pythagorean-triples public
open import elementary-number-theory.rational-numbers public
open import elementary-number-theory.relatively-prime-integers public
open import elementary-number-theory.relatively-prime-natural-numbers public
open import elementary-number-theory.repeating-element-standard-finite-type public
open import elementary-number-theory.retracts-of-natural-numbers public
open import elementary-number-theory.square-free-natural-numbers public
open import elementary-number-theory.stirling-numbers-of-the-second-kind public
open import elementary-number-theory.strong-induction-natural-numbers public
open import elementary-number-theory.telephone-numbers public
open import elementary-number-theory.triangular-numbers public
open import elementary-number-theory.twin-prime-conjecture public
open import elementary-number-theory.unit-elements-standard-finite-types public
open import elementary-number-theory.unit-similarity-standard-finite-types public
open import elementary-number-theory.universal-property-natural-numbers public
open import elementary-number-theory.w-type-of-natural-numbers public
open import elementary-number-theory.well-ordering-principle-natural-numbers public
open import elementary-number-theory.well-ordering-principle-standard-finite-types public
