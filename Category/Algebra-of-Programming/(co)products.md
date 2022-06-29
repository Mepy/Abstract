## (余)积 (Co)Products

### 积与分叉 Products & Forks
集合论里的积是笛卡尔积, 这很直接.
$$ A×B=\{(a,b):a∈A,b∈B\} $$
然而范畴论里, 能复合的有且仅有 **态射 Morphism**, 定义如下:
引入对象 $X$, 以及态射 $fst:X→A, snd:X→B$ .
集合论里, $X=A×B$, $fst(a,b) = a, snd(a,b)=b$ 是 **投影 Projection**.
范畴论里, 为得积的结构, 像引入单射和满射那样,
$C∈Obj(\mathcal{C})$, 以及态射 $f:C→A, g:C→Bf:C→A,g:C→B$, 满足复合条件, 如下图所示.

- 为什么是从 $C$ 射出而不是射入 $C$?
    要求 $h$ 唯一, 如果反向, $f':A→C, g':B→C$
    那么$h'_f=f'∘fst ≠ h'_g=g'∘snd$
```
        X
fst ↙  ↑h ↘ snd
  A ⟵ C ⟶ B
     f    g
```
$$
A×B:=(X, fst:X→A, snd:X→B) : \\
 ∀C, f:C→A, g:C→B, ∃!h, s.t. \\
  f = fst ∘ h ∧ g = snd ∘ h \\
$$
如果 $h$ 存在唯一, 那么称 $h$ 是 $f$ 与 $g$ 的 **分叉 Fork**, 记作 $h=f△gh=f△g$.
从交换图上来看, $f,g,h$ 自 $C$ **分叉** 出去.

性质:
- $fst∘(f△g)=f ∧ snd∘(f△g)=g$
- $h = (fst∘h) △ (snd∘h)$
- $fst△snd = id$
- $(f△g)∘h=(f∘h)△(g∘h)$, 如图所示:
    ```
         D
         ↓ h
         C
    f  ↙   ↘ g
     A        B
    ```

类型论里, 积就是积类型:
```OCaml
type ('a, 'b) product = 'a * 'b;;
```

### 余积与汇聚 Coproducts & Joins
将积定义中的交换图对偶, 我们便能得到余积的定义:
```
        X
inl ↗  ↓h ↖ inr
  A ⟶ C ⟵ B
     f    g
```
$$
A+B:=(X, inl:A→X, snd:B→X) : \\
 ∀C, f:A→C, g:B→C, ∃!h, s.t. \\
  f = h ∘ inl ∧ g = h ∘ inr \\
$$
如果 $h$ 存在唯一, 那么称 $h$ 是 $f$ 与 $g$ 的 **汇聚 Join**, 记作 $h=f▽g$
从交换图上来看, $f,g,h$ **汇聚** 到 $C$.

集合论里, 两个集合的 **余积 Coproduct** 或说 **和 Sum**, 定义为:
$$ A + B = \{(0, a) | a ∈ A\} ∪ \{(1, b) | b ∈ B\} $$

类型论里, 这是和类型, $inl$ 与 $inr$ 分别是左右构造器的名字.
```OCaml
type ('a, 'b) sum =
    | Inl of 'a
    | Inr of 'b
    ;;
```