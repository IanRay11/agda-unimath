---
title: Shriek of concrete group homomorphisms
---

```agda
{-# OPTIONS --without-K --exact-split #-}

module group-theory.shriek-concrete-group-actions where

open import foundation.cartesian-product-types
open import foundation.dependent-pair-types
open import foundation.identity-types
open import foundation.set-truncations
open import foundation.sets
open import foundation.universe-levels

open import group-theory.concrete-group-actions
open import group-theory.concrete-groups
open import group-theory.homomorphisms-concrete-group-actions
open import group-theory.homomorphisms-concrete-groups
open import group-theory.substitution-functor-concrete-group-actions
```

## Definition

### Operations on group actions

```agda
module _
  {l1 l2 : Level} (G : Concrete-Group l1) (H : Concrete-Group l2)
  (f : hom-Concrete-Group G H)
  where

  left-adjoint-subst-action-Concrete-Group :
    {l : Level} → (action-Concrete-Group l G) →
    (action-Concrete-Group (l1 ⊔ l2 ⊔ l) H)
  left-adjoint-subst-action-Concrete-Group X y =
    trunc-Set
      ( Σ ( classifying-type-Concrete-Group G)
          ( λ x →
            type-Set (X x) × Id (classifying-map-hom-Concrete-Group G H f x) y))
```
