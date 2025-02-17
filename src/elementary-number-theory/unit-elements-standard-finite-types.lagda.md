---
title: Unit elements in the standard finite types
---

```agda
{-# OPTIONS --without-K --exact-split #-}

module elementary-number-theory.unit-elements-standard-finite-types where

open import elementary-number-theory.congruence-natural-numbers using
  ( concatenate-eq-cong-ℕ)
open import elementary-number-theory.distance-natural-numbers using
  ( right-unit-law-dist-ℕ)
open import elementary-number-theory.divisibility-standard-finite-types using
  ( div-Fin; refl-div-Fin; trans-div-Fin)
open import
  elementary-number-theory.modular-arithmetic-standard-finite-types using
  ( eq-mod-succ-cong-ℕ; mul-Fin; associative-mul-Fin; mul-Fin';
    right-unit-law-mul-Fin; left-unit-law-mul-Fin; commutative-mul-Fin)
open import elementary-number-theory.multiplication-natural-numbers using
  ( mul-ℕ; square-succ-ℕ; commutative-mul-ℕ)
open import elementary-number-theory.natural-numbers using (ℕ; zero-ℕ; succ-ℕ)

open import foundation.dependent-pair-types using (Σ; pair; pr1; pr2)
open import foundation.identity-types using (_∙_; inv; ap)
open import foundation.universe-levels using (UU; lzero)

open import univalent-combinatorics.standard-finite-types using
  ( Fin; one-Fin; neg-one-Fin)
```

## Idea

A unit element in a standard finite type is a divisor of one

## Definition

### Units in the standard finite types

```agda
is-unit-Fin : (k : ℕ) → Fin k → UU lzero
is-unit-Fin (succ-ℕ k) x = div-Fin (succ-ℕ k) x (one-Fin k)

unit-Fin : ℕ → UU lzero
unit-Fin k = Σ (Fin k) (is-unit-Fin k)
```

## Properties

### One is a unit

```agda
is-unit-one-Fin : {k : ℕ} → is-unit-Fin (succ-ℕ k) (one-Fin k)
is-unit-one-Fin {k} = refl-div-Fin (one-Fin k)

one-unit-Fin : {k : ℕ} → unit-Fin (succ-ℕ k)
pr1 (one-unit-Fin {k}) = one-Fin k
pr2 (one-unit-Fin {k}) = is-unit-one-Fin
```

### Negative one is a unit

```agda
is-unit-neg-one-Fin : {k : ℕ} → is-unit-Fin (succ-ℕ k) (neg-one-Fin k)
is-unit-neg-one-Fin {zero-ℕ} = refl-div-Fin (neg-one-Fin 0)
pr1 (is-unit-neg-one-Fin {succ-ℕ k}) = neg-one-Fin (succ-ℕ k)
pr2 (is-unit-neg-one-Fin {succ-ℕ k}) =
  eq-mod-succ-cong-ℕ
    ( succ-ℕ k)
    ( mul-ℕ (succ-ℕ k) (succ-ℕ k))
    ( 1)
    ( concatenate-eq-cong-ℕ
      ( succ-ℕ (succ-ℕ k))
      { x3 = 1}
      ( square-succ-ℕ k)
      ( pair k
        ( ( commutative-mul-ℕ k (succ-ℕ (succ-ℕ k))) ∙
          ( inv (right-unit-law-dist-ℕ (mul-ℕ (succ-ℕ (succ-ℕ k)) k))))))

neg-one-unit-Fin : (k : ℕ) → unit-Fin (succ-ℕ k)
pr1 (neg-one-unit-Fin k) = neg-one-Fin k
pr2 (neg-one-unit-Fin k) = is-unit-neg-one-Fin
```

### Units are closed under multiplication

```agda
is-unit-mul-Fin :
  {k : ℕ} {x y : Fin k} →
  is-unit-Fin k x → is-unit-Fin k y → is-unit-Fin k (mul-Fin k x y)
pr1 (is-unit-mul-Fin {succ-ℕ k} {x} {y} (pair d p) (pair e q)) = mul-Fin (succ-ℕ k) e d
pr2 (is-unit-mul-Fin {succ-ℕ k} {x} {y} (pair d p) (pair e q)) =
  ( associative-mul-Fin (succ-ℕ k) e d (mul-Fin (succ-ℕ k) x y)) ∙
    ( ( ap
        ( mul-Fin (succ-ℕ k) e)
        ( ( inv (associative-mul-Fin (succ-ℕ k) d x y)) ∙
          ( ap (mul-Fin' (succ-ℕ k) y) p ∙ left-unit-law-mul-Fin k y))) ∙
      ( q))

mul-unit-Fin : (k : ℕ) → unit-Fin k → unit-Fin k → unit-Fin k
pr1 (mul-unit-Fin k u v) = mul-Fin k (pr1 u) (pr1 v)
pr2 (mul-unit-Fin k u v) = is-unit-mul-Fin (pr2 u) (pr2 v)
```

### The multiplicative inverse of a unit

```agda
inv-unit-Fin : {k : ℕ} → unit-Fin k → unit-Fin k
pr1 (inv-unit-Fin {succ-ℕ k} (pair u (pair v p))) = v
pr1 (pr2 (inv-unit-Fin {succ-ℕ k} (pair u (pair v p)))) = u
pr2 (pr2 (inv-unit-Fin {succ-ℕ k} (pair u (pair v p)))) =
  commutative-mul-Fin (succ-ℕ k) u v ∙ p
```
