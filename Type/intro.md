## 介绍
### **类型论 Type Theory**
类型论是
1. 编程语言
2. 逻辑+集合论模型的替代

### 与逻辑+集合论模型的比较
1. $x∈X$ 是**命题 Proposition**, 而 $t:T$ 是 **判据 Judgement**.
    疑问: 判据与命题的差异, 或许需要等到证明论部分才能理解.
    > Elements are not independent of their type, unlike in set theory. 

    疑问: 元素是居留在类型中的, 不可脱离? 然而, 所谓子类型? 
2. 集合论的编码方式可能带来怪异感.
    > $2⊂3$ 

    疑问: 这个, 是否会导致非公理化呢? 即自然数模型被特化成某一种特殊实现.

3. 类型论的逻辑更直观, 即所谓 Curry-Howard 对应.
4. 类型论中, 函数概念 **原初 primitive**. 
    集合论中函数是特殊的关系: 
    $f⊂X×Y, ∀x∈X, ∃!y∈Y, s.t. (x,y)∈f$
    而类型论中, 函数 $f:X→Y$, 而关系 $R$ 是特殊的函数:
    $R:X→Y→Prop$

### 函数
所谓函数, 是一个黑箱子.

直接先引入 λ-calculus.
```
term t :=   x   (var)
          λx→t  (abs)
          t1 t2 (app)
Type T :=   A   (Var)
          T1→T2 (Arr)
```

(app) 左结合, (Arr) 右结合.
g x y = (g x) y
g x y ≠  g (x y)
A → B → C =  A → (B → C)
A → B → C ≠ (A → B) → C

函数匿名与否.

- **α**-conversion 换名
    λx→t = λy→t[x:=y] if y not in t
- **β**-reduction  简化
    (λx→t) s = t[x:=s]
- **η**-reduction  套壳 
    λx→(f x) = f
    

### 组合子与 SKI
组合子是纯 λ-calculus 的函数.
> Combinators are functions that only use pure lambda calculus. 

**恒等组合子 Identity  I**
**复合组合子 Composition ◦**
```agda
I :  A→A
I x  = x
_ ◦ _ : ( B → C ) → ( A → B ) → ( A → C )
( f ◦ g ) x = f ( g x )

K : A → B → A
K a b = a

S : ( A → B → C) → ( A → B ) → ( A → C )
S f g x = f x ( g x )
```

后两个组合子, **K** 和 **S** 等价于 λ-calculus.

**K** 的性质: λx→y = **K** y
**S** 的性质: λx→m n = **S** (λx→m) (λx→n)

疑问: **S** 和 **K** 的直觉意味很强, 能否推导得出?

下面将 **◦** 嵌入到 **SKI** 中:


◦           = λf → λg → λx → f (g x)
[use **S**] = λf → λg → **S** (λx→f) (λx→g x) 
[use **η**] = λf → λg → **S** (λx→f) g 
[use **η**] = λf → **S** (λx→f)
[use **S**] = **S** (λf→**S**) (λf→λx→f)
[use **K**] = **S** (**K** **S**) **K**


练习: 
1. **η**-reduction 对应的 **SKI** 版本
f:A→B, f = λx→f x = **S** (λx→f) (λx→x) = **S** (**K** f) **I** <br>

2. **I** 可以由 **K** 和 **S** 表示.
**I** = **S** **K** **K**, 验证之。
下面是一些杂念, 尝试推导.
I = **S** (**K** I) I 
**S** (**K** I) 
= (λf→λg→λx→f x(g x)) (λy→I) 
= λg→λx→ I (g x) 
= λg→λx→g x 
= I 
**S** **K** 
= (λf→λg→λx→f x(g x)) (λx→λy→x) 
= λg→λx→(λy→x)(g x) 
= λg→λx→x (**化简方式, g:A→B, x:A**)
= λg→I
g:A→B, **I** = **S** **K** **K**


