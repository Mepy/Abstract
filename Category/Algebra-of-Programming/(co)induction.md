## (余)归纳 (Co)Induction

### 函子 Functor
范畴论里, 函子是范畴之间的映射 $F:\mathcal{C}→\mathcal{D}$,
将对象映射到对象, 将对象间的态射映射到对应对象间的态射.
$$
\frac 
{f:A→B}
{F f:FA→FB}
$$

此处, 出于构造性的倾向, 强调定义域 $\mathcal{C}$, 而不强调值域 $\mathcal{D}$.
或者说, **自函子 Endo-Functor** $F:\mathcal{C}→\mathcal{C}$.

**多项式自函子 Polynomial Endo-Functor** 是一族自函子,
其定义域是特定基范畴 $\mathcal{B}$ 在 $×,+$ 下的闭包范畴 $\mathcal{C}$,
其由 $×,+$ 以及 $A∈Obj(\mathcal{B})$ 经有限次复合而成.
除非特殊说明, 下文所谓 **函子**, 均指 **多项式自函子**.

例如:(其中$1∈Obj(\mathcal{B})$ 是 $×$ 的幺元, 即 **Unit** 类型)

- $N(X)=1+XN(X)=1+X$
    自然数 Nat
- $L_A(X)=1+A×X$
    序列 List
- $T_A(X)=A+X×X$
    树 Tree


### 不动点 Fixpoint
既然是多项式自函子 $F:\mathcal{C}→\mathcal{C}$,
有必要考虑其不动点 $X∈\mathcal{C}, s.t.FX=X$.

类型范畴里, 不动点有两个:(参见[TaPL](https://www.cis.upenn.edu/~bcpierce/tapl/)第21章)

- 最小不动点 $μF$ 也称
    **归纳类型 Inductive Types**
    **初代数 Initial Algebra**
- 最大不动点 $νF$ 也称
    **余归纳类型 Coinductive Types**
    **终代数 Terminal Algebra**

### * 函子代数 F-Algebra 
关于 **初代数** 与 **终代数**, 本节函子不限于多项式自函子.

所谓[函子代数](https://en.wikipedia.org/wiki/F-algebra), 给定一个范畴 $\mathcal{C}$ 及其上的自函子 $F:\mathcal{C}→\mathcal{C}$,
$(A, α:F(A)→A)$ 是一个**函子代数 F-Algebra**,
其中 $A$ 称为 **载子 Carrier**, $α$ 即上文的 $in$.
上下文自明时, 可以称载子 $A$ 是一个函子代数.

函子代数构成范畴, 函子代数 $(A, α)$ 与 $(B, β)$ 之间的态射 $f:A→B$ 满足:
$$ f∘α = β∘F(f) $$ 

```
    F(f)
F(A) ⟶ F(B)
 α↓       ↓β
  A  ⟶  B
      f
```

所谓[初对象](https://en.wikipedia.org/wiki/Initial_and_terminal_objects), 给定一个范畴 $\mathcal{C}$, **初对象 Initial Object** $I$ 满足:
$$ ∀X∈Obj(\mathcal{C}), ∃!f:I→X $$

**初代数 Initial Algebra** 是函子代数范畴中的初对象.

[Lambek's theorem](https://ncatlab.org/nlab/show/initial+algebra+of+an+endofunctor) 证明了初代数是不动点,另见[Algebraic Approaches to Program Semantics ](https://link.springer.com/book/10.1007/978-1-4612-4962-7).

**终代数 Terminal Algebra** 是对偶的.

### 折叠 Folds
范畴论里, 能复合的有且仅有 **态射**, $μF$ 如下构造:
$$
μF∈Obj(\mathcal{C}), in:F(μF)→μF, \\
∀A∈Obj(\mathcal{C}), f:F(A)→A, \\ 
∃!h:μF→A, s.t. h∘in = f∘Fh \\
$$
```
       Fh
 F(μF) ⟶ F(A)
in ↓        ↓ f
   μF  ⟶  A
       h
```
称 $μF≈F(μF)$, $h$是 $f$ 的 **折叠 Fold** 或说 **下 Cata**, 记作$h=\text{cata}f$.
$$ h=\text{cata}f ⇔ h∘in=f∘Fh $$

其中 $\text{cata}$ 称为 **向下态射 catamorphism**.

举例:
$F=L_\N, f=add:L_\N(\N)→\N$
```OCaml
(* L_A(X) = 1 + A × X *)
type ('a, 'x) _L =
    | Nil
    | Cons of 'a * 'x
;;
(* F = L_\N *)
type 'x _F = (int, 'x) _L ;;
(* f = add *)
let f : int _F -> int
= fun l -> match l with
    | Nil -> 0
    | Cons(x, y) -> x + y
;;
(*
type muF =
    | Nil
    | Cons of int * muF
;;
let rec cataf : muF -> int
= fun l -> match l with
    | Nil -> 0
    | Cons(hd, tl) -> hd + (cataf tl)
;;
*)
```

原文是用 Haskell 描述的, 如下:
```Haskell
data L a x = Nil | Cons a x -- L a x ↔ L_A(X)
data Mu f a = In (f a (Mu f a))
{-
f a ↔ endo-functor F
Mu f a = In (f a (Mu f a))
↔ μF = In (F(μF))
↔ μF <---- F(μF)
In
-}
type List a = Mu L a
```
### 展开 Unfolds
对偶地, $νF$ 如下构造:
$$
μF∈Obj(\mathcal{C}), out:νF→F(νF), \\
∀A∈Obj(\mathcal{C}), f:A→F(A), \\
∃!h:A→νF, s.t. out∘h = Fh∘f \\
$$
```
         Fh
  F(νF) ⟵ F(A)
out ↑       ↑ f
    νF  ⟵  A
         h
    Fh
```
称 $F(νF)≈ν$, $h$ 是$f$ 的 **展开 Unfold** 或说 **上 Ana**, 记作 $h=\text{ana}f$.
$$ h=\text{ana}f ⇔ out∘h = Fh∘f $$


其中 $\text{ana}$ 称为 **向上态射 anamorphism**.
```Haskell
data L a x = Nil | Cons a x -- L a x ↔ L_A(X)
data Nu f a = Out (f a (Nu f a))
{-
f a ↔ endo-functor F
Nu f a = Out (f a (Nu f a))
↔ νF = Out (F(μF))
↔ νF ----> F(νF)
Out
-}
out :: Nu f a -> f a (Nu f a)
out (Out x) = x
-- An idiomatic alternative:
data Nu f a = UnOut {out :: f a (Nu f a)}
type Colist a = Nu L a
```
关于 `In`, `Out` 与 `UnOut`, 其实不太懂.

### 向下态射与向上态射 Catamorphism & Anamorphism
上述两节, 仅有类型(对象)的 Haskell 实现, 未有函数(态射)的 Haskell 实现,
即如何实现 $h$, 或更确切地说, 实现向下态射 $\text{cata}$ 与向上态射 $\text{ana}$.
注意, 向下态射与向上态射是另一个范畴(即函子代数范畴)的态射.

前文说的自函子, 均是一元运算, 我们需要二元函子
$F:(\mathcal{C},\mathcal{C})→\mathcal{C}$


二元函子仍然保持范畴结构:
$$
\frac 
{f:A→C, g:B→D}
{F(f, g) : F(A, B)→F(C, D)}
$$

```Haskell
class Functor f where
fmap :: (a -> b) -> (f a -> f b)

class Bifunctor f where
bimap :: (a -> c) -> (b -> d) -> (f a b -> f c d)

-- construct Functor (Mu f) with Bifunctor f
instance Bifunctor f => Functor (Mu f) where
fmap f (In x) = In (bimap f (fmap f) x)

instance Bifunctor L where
bimap f g Nil = Nil
bimap f g (Cons x y) = Cons (f x) (g y)
```
实现 $\text{cata}$:
```Haskell
cata :: Bifunctor f => (f a b->b) -> (Mu f a -> b)
-- use phi to denote f:F(A)→A in communitative diagram
cata phi (In x) = phi (bimap id (cata phi) x)
```
实现 $\text{ana}$:
```Haskell
ana :: Bifunctor f => (b -> f a b) -> (b -> Nu f a)
-- use phi to denote f:A→F(A) in communitative diagram
ana phi z = UnOut (bimap id (ana phi) (phi z))
```
举例
- `range`
    ```Haskell
    -- range (0, 3) = [0, 1, 2]
    -- range (0, 0) = []
    -- range (3, 2) = [3, 4, 5, ...]
    range :: (Int, Int) -> Colist Int
    range = ana next where
    next :: (Int, Int) -> L Int (Int, Int)
    next(m, n)
    | m == n = Nil
    | otherwise = Cons m (m+1, n)
    ```
另外还有二叉树与利用 Maybe 的例子, 略去.

