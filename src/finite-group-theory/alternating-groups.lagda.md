---
title: Alternating groups
---

```agda
module finite-group-theory.alternating-groups where

open import group-theory.symmetric-groups
open import group-theory.kernels
open import group-theory.groups

open import finite-group-theory.sign-homomorphism

open import elementary-number-theory.natural-numbers using (ℕ)
open import univalent-combinatorics.standard-finite-types using (Fin-Set)
open import univalent-combinatorics.finite-types using (UU-Fin; set-UU-Fin)
```

## Idea

The alternating group on a finite set `X` is the group of even permutations of `X`, i.e. it is the kernel of the sign homomorphism `Aut(X) → Aut(2)`.

## Definition

```agda
module _ {l} (n : ℕ) (X : UU-Fin l n) where
  alternating-Group : Group l
  alternating-Group = group-kernel-hom-Group
    ( symmetric-Group (set-UU-Fin n X))
    ( symmetric-Group (Fin-Set 2))
    ( sign-homomorphism n X)
```
