---
title: The greatest common divisor of natural numbers
---

```agda
module elementary-number-theory.greatest-common-divisor-natural-numbers where

open import elementary-number-theory.addition-natural-numbers using
  ( add-ℕ; is-zero-left-is-zero-add-ℕ; is-zero-right-is-zero-add-ℕ)
open import elementary-number-theory.decidable-types using
  ( is-decidable-bounded-Π-ℕ)
open import elementary-number-theory.distance-natural-numbers using
  ( dist-ℕ; right-unit-law-dist-ℕ)
open import elementary-number-theory.divisibility-natural-numbers using
  ( div-ℕ; refl-div-ℕ; antisymmetric-div-ℕ; concatenate-div-eq-ℕ; div-add-ℕ;
    div-zero-ℕ; transitive-div-ℕ; div-right-summand-ℕ; div-mul-ℕ;
    leq-div-succ-ℕ; preserves-div-mul-ℕ; reflects-div-mul-ℕ; is-one-div-one-ℕ)
open import elementary-number-theory.equality-natural-numbers using
  ( is-decidable-is-zero-ℕ)
open import elementary-number-theory.euclidean-division-natural-numbers using
  ( remainder-euclidean-division-ℕ; quotient-euclidean-division-ℕ;
    eq-quotient-euclidean-division-ℕ; eq-euclidean-division-ℕ;
    strict-upper-bound-remainder-euclidean-division-ℕ)
open import elementary-number-theory.inequality-natural-numbers using
  ( leq-ℕ; concatenate-leq-eq-ℕ; leq-mul-ℕ'; is-zero-leq-zero-ℕ; le-ℕ;
    contradiction-le-ℕ)
open import elementary-number-theory.lower-bounds-natural-numbers using
  ( is-lower-bound-ℕ)
open import
  elementary-number-theory.modular-arithmetic-standard-finite-types using
  ( is-decidable-div-ℕ)
open import elementary-number-theory.multiplication-natural-numbers using
  ( mul-ℕ; left-unit-law-mul-ℕ)
open import elementary-number-theory.natural-numbers using
  ( ℕ; zero-ℕ; succ-ℕ; is-nonzero-ℕ; is-successor-ℕ; is-successor-is-nonzero-ℕ;
    is-zero-ℕ; is-one-ℕ)
open import
  elementary-number-theory.well-ordering-principle-natural-numbers using
  ( minimal-element-ℕ; well-ordering-principle-ℕ)

open import foundation.cartesian-product-types using (_×_; pair')
open import foundation.coproduct-types using (inl; inr)
open import foundation.decidable-types using
  ( is-decidable-fam; is-decidable-prod; is-decidable-function-type';
    is-decidable-neg; dn-elim-is-decidable)
open import foundation.dependent-pair-types using (pair; pr1; pr2)
open import foundation.empty-types using (ex-falso)
open import foundation.functoriality-cartesian-product-types using (map-prod)
open import foundation.identity-types using (_＝_; refl; _∙_; inv; ap; tr)
open import foundation.logical-equivalences using (_↔_)
open import foundation.universe-levels using (UU; lzero)
```

## Idea

The greatest common divisor of two natural numbers `x` and `y` is a number `gcd x y` such that any natural number `d : ℕ` is a common divisor of `x` and `y` if and only if it is a divisor of `gcd x y`.

## Definition

### Common divisors

```agda
is-common-divisor-ℕ : (a b x : ℕ) → UU lzero
is-common-divisor-ℕ a b x = (div-ℕ x a) × (div-ℕ x b)
```

### Greatest common divisors

```agda
is-gcd-ℕ : (a b d : ℕ) → UU lzero
is-gcd-ℕ a b d = (x : ℕ) → (is-common-divisor-ℕ a b x) ↔ (div-ℕ x d)
```

### The predicate of being a multiple of the greatest common divisor

```agda
is-multiple-of-gcd-ℕ : (a b n : ℕ) → UU lzero
is-multiple-of-gcd-ℕ a b n =
  is-nonzero-ℕ (add-ℕ a b) →
  (is-nonzero-ℕ n) × ((x : ℕ) → is-common-divisor-ℕ a b x → div-ℕ x n)
```

## Properties

### Reflexivity for common divisors

```agda
refl-is-common-divisor-ℕ :
  (x : ℕ) → is-common-divisor-ℕ x x x
pr1 (refl-is-common-divisor-ℕ x) = refl-div-ℕ x
pr2 (refl-is-common-divisor-ℕ x) = refl-div-ℕ x
```

### Being a common divisor is decidable

```agda
is-decidable-is-common-divisor-ℕ :
  (a b : ℕ) → is-decidable-fam (is-common-divisor-ℕ a b)
is-decidable-is-common-divisor-ℕ a b x =
  is-decidable-prod
    ( is-decidable-div-ℕ x a)
    ( is-decidable-div-ℕ x b)
```

### Any greatest common divisor is a common divisor

```agda
is-common-divisor-is-gcd-ℕ :
  (a b d : ℕ) → is-gcd-ℕ a b d → is-common-divisor-ℕ a b d
is-common-divisor-is-gcd-ℕ a b d H = pr2 (H d) (refl-div-ℕ d)
```

### Uniqueness of greatest common divisors

```agda
uniqueness-is-gcd-ℕ :
  (a b d d' : ℕ) → is-gcd-ℕ a b d → is-gcd-ℕ a b d' → d ＝ d'
uniqueness-is-gcd-ℕ a b d d' H H' =
  antisymmetric-div-ℕ d d'
    ( pr1 (H' d) (is-common-divisor-is-gcd-ℕ a b d H))
    ( pr1 (H d') (is-common-divisor-is-gcd-ℕ a b d' H'))
```

### If `d` is a common divisor of `a` and `b`, and `a + b ≠ 0`, then `d ≤ a + b`

```agda
leq-sum-is-common-divisor-ℕ' :
  (a b d : ℕ) →
  is-successor-ℕ (add-ℕ a b) → is-common-divisor-ℕ a b d → leq-ℕ d (add-ℕ a b)
leq-sum-is-common-divisor-ℕ' a zero-ℕ d (pair k p) H =
  concatenate-leq-eq-ℕ d
    ( leq-div-succ-ℕ d k (concatenate-div-eq-ℕ (pr1 H) p))
    ( inv p)
leq-sum-is-common-divisor-ℕ' a (succ-ℕ b) d (pair k p) H =
  leq-div-succ-ℕ d (add-ℕ a b) (div-add-ℕ d a (succ-ℕ b) (pr1 H) (pr2 H))

leq-sum-is-common-divisor-ℕ :
  (a b d : ℕ) →
  is-nonzero-ℕ (add-ℕ a b) → is-common-divisor-ℕ a b d → leq-ℕ d (add-ℕ a b)
leq-sum-is-common-divisor-ℕ a b d H =
  leq-sum-is-common-divisor-ℕ' a b d (is-successor-is-nonzero-ℕ H)
```

### Being a multiple of the greatest common divisor is decidable

```agda
is-decidable-is-multiple-of-gcd-ℕ :
  (a b : ℕ) → is-decidable-fam (is-multiple-of-gcd-ℕ a b)
is-decidable-is-multiple-of-gcd-ℕ a b n =
  is-decidable-function-type'
    ( is-decidable-neg (is-decidable-is-zero-ℕ (add-ℕ a b)))
    ( λ np →
      is-decidable-prod
        ( is-decidable-neg (is-decidable-is-zero-ℕ n))
        ( is-decidable-bounded-Π-ℕ
          ( is-common-divisor-ℕ a b)
          ( λ x → div-ℕ x n)
          ( is-decidable-is-common-divisor-ℕ a b)
          ( λ x → is-decidable-div-ℕ x n)
          ( add-ℕ a b)
          ( λ x → leq-sum-is-common-divisor-ℕ a b x np)))
```

### The sum of `a` and `b` is a multiple of the greatest common divisor of `a` and `b`

```agda
sum-is-multiple-of-gcd-ℕ : (a b : ℕ) → is-multiple-of-gcd-ℕ a b (add-ℕ a b)
pr1 (sum-is-multiple-of-gcd-ℕ a b np) = np
pr2 (sum-is-multiple-of-gcd-ℕ a b np) x H = div-add-ℕ x a b (pr1 H) (pr2 H)
```

### The greatest common divisor exists

```agda
abstract
  GCD-ℕ : (a b : ℕ) → minimal-element-ℕ (is-multiple-of-gcd-ℕ a b)
  GCD-ℕ a b =
    well-ordering-principle-ℕ
      ( is-multiple-of-gcd-ℕ a b)
      ( is-decidable-is-multiple-of-gcd-ℕ a b)
      ( pair (add-ℕ a b) (sum-is-multiple-of-gcd-ℕ a b))

gcd-ℕ : ℕ → ℕ → ℕ
gcd-ℕ a b = pr1 (GCD-ℕ a b)

is-multiple-of-gcd-gcd-ℕ : (a b : ℕ) → is-multiple-of-gcd-ℕ a b (gcd-ℕ a b)
is-multiple-of-gcd-gcd-ℕ a b = pr1 (pr2 (GCD-ℕ a b))

is-lower-bound-gcd-ℕ :
  (a b : ℕ) → is-lower-bound-ℕ (is-multiple-of-gcd-ℕ a b) (gcd-ℕ a b)
is-lower-bound-gcd-ℕ a b = pr2 (pr2 (GCD-ℕ a b))
```

### `a + b = 0` if and only if `gcd a b = 0`

```agda
is-zero-gcd-ℕ :
  (a b : ℕ) → is-zero-ℕ (add-ℕ a b) → is-zero-ℕ (gcd-ℕ a b)
is-zero-gcd-ℕ a b p =
  is-zero-leq-zero-ℕ
    ( gcd-ℕ a b)
    ( concatenate-leq-eq-ℕ
      ( gcd-ℕ a b)
      ( is-lower-bound-gcd-ℕ a b
        ( add-ℕ a b)
        ( sum-is-multiple-of-gcd-ℕ a b))
      ( p))

is-zero-gcd-zero-zero-ℕ : is-zero-ℕ (gcd-ℕ zero-ℕ zero-ℕ)
is-zero-gcd-zero-zero-ℕ = is-zero-gcd-ℕ zero-ℕ zero-ℕ refl

is-zero-add-is-zero-gcd-ℕ :
  (a b : ℕ) → is-zero-ℕ (gcd-ℕ a b) → is-zero-ℕ (add-ℕ a b)
is-zero-add-is-zero-gcd-ℕ a b H =
  dn-elim-is-decidable
    ( is-decidable-is-zero-ℕ (add-ℕ a b))
    ( λ f → pr1 (is-multiple-of-gcd-gcd-ℕ a b f) H)
```

### If at least one of `a` and `b` is nonzero, then their gcd is nonzer

```agda
is-nonzero-gcd-ℕ :
  (a b : ℕ) → is-nonzero-ℕ (add-ℕ a b) → is-nonzero-ℕ (gcd-ℕ a b)
is-nonzero-gcd-ℕ a b ne = pr1 (is-multiple-of-gcd-gcd-ℕ a b ne)

is-successor-gcd-ℕ :
  (a b : ℕ) → is-nonzero-ℕ (add-ℕ a b) → is-successor-ℕ (gcd-ℕ a b)
is-successor-gcd-ℕ a b ne =
  is-successor-is-nonzero-ℕ (is-nonzero-gcd-ℕ a b ne)
```

### Any common divisor is also a divisor of the greatest common divisor

```agda
div-gcd-is-common-divisor-ℕ :
  (a b x : ℕ) → is-common-divisor-ℕ a b x → div-ℕ x (gcd-ℕ a b)
div-gcd-is-common-divisor-ℕ a b x H with
  is-decidable-is-zero-ℕ (add-ℕ a b)
... | inl p = concatenate-div-eq-ℕ (div-zero-ℕ x) (inv (is-zero-gcd-ℕ a b p))
... | inr np = pr2 (is-multiple-of-gcd-gcd-ℕ a b np) x H
```

### If every common divisor divides a number r < gcd a b, then r = 0.

```agda
is-zero-is-common-divisor-le-gcd-ℕ :
  (a b r : ℕ) → le-ℕ r (gcd-ℕ a b) →
  ((x : ℕ) → is-common-divisor-ℕ a b x → div-ℕ x r) → is-zero-ℕ r
is-zero-is-common-divisor-le-gcd-ℕ a b r l d with is-decidable-is-zero-ℕ r
... | inl H = H
... | inr x =
  ex-falso
    ( contradiction-le-ℕ r (gcd-ℕ a b) l
      ( is-lower-bound-gcd-ℕ a b r (λ np → pair x d)))
```

### Any divisor of `gcd a b` is a common divisor of `a` and `b`

```agda
is-divisor-left-div-gcd-ℕ :
  (a b x : ℕ) → div-ℕ x (gcd-ℕ a b) → div-ℕ x a
is-divisor-left-div-gcd-ℕ a b x d with
  is-decidable-is-zero-ℕ (add-ℕ a b)
... | inl p =
  concatenate-div-eq-ℕ (div-zero-ℕ x) (inv (is-zero-left-is-zero-add-ℕ a b p))
... | inr np =
  transitive-div-ℕ x (gcd-ℕ a b) a d
    ( pair q
      ( ( ( α) ∙
          ( ap ( dist-ℕ a)
               ( is-zero-is-common-divisor-le-gcd-ℕ a b r B
                 ( λ x H →
                   div-right-summand-ℕ x (mul-ℕ q (gcd-ℕ a b)) r
                     ( div-mul-ℕ q x (gcd-ℕ a b)
                       ( div-gcd-is-common-divisor-ℕ a b x H))
                     ( concatenate-div-eq-ℕ (pr1 H) (inv β)))))) ∙
        ( right-unit-law-dist-ℕ a)))
  where
  r = remainder-euclidean-division-ℕ (gcd-ℕ a b) a
  q = quotient-euclidean-division-ℕ (gcd-ℕ a b) a
  α = eq-quotient-euclidean-division-ℕ (gcd-ℕ a b) a
  B = strict-upper-bound-remainder-euclidean-division-ℕ (gcd-ℕ a b) a
       ( is-nonzero-gcd-ℕ a b np)
  β = eq-euclidean-division-ℕ (gcd-ℕ a b) a

is-divisor-right-div-gcd-ℕ :
  (a b x : ℕ) → div-ℕ x (gcd-ℕ a b) → div-ℕ x b
is-divisor-right-div-gcd-ℕ a b x d with
  is-decidable-is-zero-ℕ (add-ℕ a b)
... | inl p =
  concatenate-div-eq-ℕ (div-zero-ℕ x) (inv (is-zero-right-is-zero-add-ℕ a b p))
... | inr np =
  transitive-div-ℕ x (gcd-ℕ a b) b d
    ( pair q
      ( ( α ∙ ( ap ( dist-ℕ b)
               ( is-zero-is-common-divisor-le-gcd-ℕ a b r B
                 ( λ x H →
                   div-right-summand-ℕ x (mul-ℕ q (gcd-ℕ a b)) r
                     ( div-mul-ℕ q x (gcd-ℕ a b)
                       ( div-gcd-is-common-divisor-ℕ a b x H))
                     ( concatenate-div-eq-ℕ (pr2 H) (inv β)))))) ∙
        ( right-unit-law-dist-ℕ b)))
  where
  r = remainder-euclidean-division-ℕ (gcd-ℕ a b) b
  q = quotient-euclidean-division-ℕ (gcd-ℕ a b) b
  α = eq-quotient-euclidean-division-ℕ (gcd-ℕ a b) b
  B = strict-upper-bound-remainder-euclidean-division-ℕ (gcd-ℕ a b) b
       ( is-nonzero-gcd-ℕ a b np)
  β = eq-euclidean-division-ℕ (gcd-ℕ a b) b

is-common-divisor-div-gcd-ℕ :
  (a b x : ℕ) → div-ℕ x (gcd-ℕ a b) → is-common-divisor-ℕ a b x
pr1 (is-common-divisor-div-gcd-ℕ a b x d) = is-divisor-left-div-gcd-ℕ a b x d
pr2 (is-common-divisor-div-gcd-ℕ a b x d) = is-divisor-right-div-gcd-ℕ a b x d
```

### The gcd of `a` and `b` is a common divisor

```agda
is-common-divisor-gcd-ℕ : (a b : ℕ) → is-common-divisor-ℕ a b (gcd-ℕ a b)
is-common-divisor-gcd-ℕ a b =
  is-common-divisor-div-gcd-ℕ a b (gcd-ℕ a b) (refl-div-ℕ (gcd-ℕ a b))
```

### The gcd of `a` and `b` is a greatest common divisor

```agda
is-gcd-gcd-ℕ : (a b : ℕ) → is-gcd-ℕ a b (gcd-ℕ a b)
pr1 (is-gcd-gcd-ℕ a b x) = div-gcd-is-common-divisor-ℕ a b x
pr2 (is-gcd-gcd-ℕ a b x) = is-common-divisor-div-gcd-ℕ a b x
```

### The gcd is commutative

```agda
is-commutative-gcd-ℕ : (a b : ℕ) → gcd-ℕ a b ＝ gcd-ℕ b a
is-commutative-gcd-ℕ a b =
  antisymmetric-div-ℕ
    ( gcd-ℕ a b)
    ( gcd-ℕ b a)
    ( pr1 (is-gcd-gcd-ℕ b a (gcd-ℕ a b)) (σ (is-common-divisor-gcd-ℕ a b)))
    ( pr1 (is-gcd-gcd-ℕ a b (gcd-ℕ b a)) (σ (is-common-divisor-gcd-ℕ b a)))
  where
  σ : {A B : UU lzero} → A × B → B × A
  pr1 (σ (pair x y)) = y
  pr2 (σ (pair x y)) = x
```

### If `d` is a common divisor of `a` and `b`, then `kd` is a common divisor of `ka` and `kb`. 

```agda
preserves-is-common-divisor-mul-ℕ :
  (k a b d : ℕ) → is-common-divisor-ℕ a b d →
  is-common-divisor-ℕ (mul-ℕ k a) (mul-ℕ k b) (mul-ℕ k d)
preserves-is-common-divisor-mul-ℕ k a b d =
  map-prod
    ( preserves-div-mul-ℕ k d a)
    ( preserves-div-mul-ℕ k d b)

reflects-is-common-divisor-mul-ℕ :
  (k a b d : ℕ) → is-nonzero-ℕ k →
  is-common-divisor-ℕ (mul-ℕ k a) (mul-ℕ k b) (mul-ℕ k d) →
  is-common-divisor-ℕ a b d
reflects-is-common-divisor-mul-ℕ k a b d H =
  map-prod
    ( reflects-div-mul-ℕ k d a H)
    ( reflects-div-mul-ℕ k d b H)
```

### `gcd-ℕ 1 b ＝ 1`

```agda
is-one-is-gcd-one-ℕ : {b x : ℕ} → is-gcd-ℕ 1 b x → is-one-ℕ x
is-one-is-gcd-one-ℕ {b} {x} H =
  is-one-div-one-ℕ x (pr1 (is-common-divisor-is-gcd-ℕ 1 b x H))

is-one-gcd-one-ℕ : (b : ℕ) → is-one-ℕ (gcd-ℕ 1 b)
is-one-gcd-one-ℕ b = is-one-is-gcd-one-ℕ (is-gcd-gcd-ℕ 1 b)
```

### `gcd-ℕ a 1 ＝ 1`

```agda
is-one-is-gcd-one-ℕ' : {a x : ℕ} → is-gcd-ℕ a 1 x → is-one-ℕ x
is-one-is-gcd-one-ℕ' {a} {x} H =
  is-one-div-one-ℕ x (pr2 (is-common-divisor-is-gcd-ℕ a 1 x H))

is-one-gcd-one-ℕ' : (a : ℕ) → is-one-ℕ (gcd-ℕ a 1)
is-one-gcd-one-ℕ' a = is-one-is-gcd-one-ℕ' (is-gcd-gcd-ℕ a 1)
```

### `gcd-ℕ 0 b ＝ b`

```agda
is-id-is-gcd-zero-ℕ : {b x : ℕ} → gcd-ℕ 0 b ＝ x → x ＝ b
is-id-is-gcd-zero-ℕ {b} {x} H = antisymmetric-div-ℕ x b 
  (pr2 (is-common-divisor-is-gcd-ℕ 0 b x 
    (tr (λ t → is-gcd-ℕ 0 b t) H (is-gcd-gcd-ℕ 0 b)))) 
  (tr (λ t → div-ℕ b t) H 
    (div-gcd-is-common-divisor-ℕ 0 b b 
      (pair' (div-zero-ℕ b) (refl-div-ℕ b))))
```

### `gcd-ℕ a 0 ＝ a`

```agda
is-id-is-gcd-zero-ℕ' : {a x : ℕ} → gcd-ℕ a 0 ＝ x → x ＝ a
is-id-is-gcd-zero-ℕ' {a} {x} H = is-id-is-gcd-zero-ℕ {a} {x} 
  ((is-commutative-gcd-ℕ 0 a) ∙ H)

```
