# Least elements in preorders

```agda
{-# OPTIONS --without-K --exact-split #-}

module order-theory.least-elements-preorders where

open import foundation.dependent-pair-types using (Σ; pair; pr1; pr2)
open import foundation.propositions using
  ( Prop; Π-Prop; type-Prop; is-prop; is-prop-type-Prop)
open import foundation.universe-levels using (Level; UU; _⊔_)

open import order-theory.preorders using
  ( Preorder; element-Preorder; leq-preorder-Prop)
```

## Definition

```agda
module _
  {l1 l2 : Level} (X : Preorder l1 l2)
  where

  is-least-element-preorder-Prop : element-Preorder X → Prop (l1 ⊔ l2)
  is-least-element-preorder-Prop x =
    Π-Prop (element-Preorder X) (leq-preorder-Prop X x)

  is-least-element-Preorder : element-Preorder X → UU (l1 ⊔ l2)
  is-least-element-Preorder x = type-Prop (is-least-element-preorder-Prop x)

  is-prop-is-least-element-Preorder :
    (x : element-Preorder X) → is-prop (is-least-element-Preorder x)
  is-prop-is-least-element-Preorder x =
    is-prop-type-Prop (is-least-element-preorder-Prop x)

  least-element-Preorder : UU (l1 ⊔ l2)
  least-element-Preorder = Σ (element-Preorder X) is-least-element-Preorder
```
