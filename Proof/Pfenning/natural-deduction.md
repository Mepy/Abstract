## 自然演绎
### 术语表
#### 原子
根据 Curry-Howard 对应, **命题** 对 **类型 Type**, **证明** 对 **词项 Term** ~~, 大陆对长空~~.
我们总采用 **构造主义** 的观点, 研究 **直觉逻辑 Intuitionistic Logic**.
1. **命题 Proposition** : 能被 **判断** 为真或假的命题.
2. **判断 Judgement** : 可被证明的东西. 一般比命题稍大一些.
    例如 `P`是一个命题, `P 真`是一个判断.
    又如`t:T`(读作`t`类型为`T`)是一个判断.
3. **证明 Proof** : 对一个判断的证明(过程). 术语略有混淆, 但无碍.
    一般来说, 证明由两部分组成:
    - **前提 Premise** : 横线上方的部分, 已有的东西.
    - **结论 Conclusion** : 横线下方的东西, 证得的东西.
4. **公理 Axioms** : 前提空的证明, 来自 PFPL.
5. **假言证明 Hypothetical Proof** : 前提非空的证明.
6. **假言判断 Hypothetical Judgments** : 引入绑定变量 $x$, 见 PFPL, 但符号如下:
    $$
    x : A \\
    \vdots \\
    b : B
     $$

#### 复合与还原
1. **逻辑连接词 Logical Connective** : 复合命题的符号.
    用 PFPL 的术语, 这是类别 $\tau$ (即类型)上的 **算子**.
2. **构造器 Constructor** : 类别 $e$ (即词项)上的 **算子**.
3. **引入规则 Introduction Rule** : **验证视角 Verification View**, 复合的证明规则 $I$.
4. **消去规则 Elimination Rule** : **实用视角 Pragmatist View**, 还原的证明规则 $E$.
5. **有效性 Soundness** : 仅对 $E$ 而言, $E$ **有效**, 即 $E$ 能将 $I$ 复合结果还原, 即 $* /^I * /^E *$. 
6. **完备性 Completeness** : 仅对 $I$ 而言, $I$ **完备**, 即 $I$ 能将 $E$ 还原结果复合, 即 $* /^E * /^I *$.
7. **协调 Harmony** : $E$ **有效**, $I$ **完备**.

### **合取 Conjunction** ∧
#### 规则
引入规则 :
$$
\dfrac
{a:A, b:B}
{(a,b) : A∧B}
∧-I
$$
消去规则 :
$$
\dfrac
{(a,b) : A∧B}
{a:A}
∧-E_1 
,
\dfrac
{(a,b) : A∧B}
{b:B}
∧-E_2 
$$

#### 协调
有效性 : 
$$
\dfrac
{
    \dfrac
    {a:A, b:B}
    {(a,b):A∧B}
    ∧-I
}
{a:A}
∧-E_1 
,
\dfrac
{
    \dfrac
    {a:A, b:B}
    {(a,b):A∧B}
    ∧-I
}
{b:B}
∧-E_2
$$

完备性 : 
$$
\dfrac
{
    \dfrac
    {(a,b):A∧B}
    {a:A}
    ∧-E_1
    ,
    \dfrac
    {(a,b):A∧B}
    {b:B}
    ∧-E_2
    ,
}
{(a,b):A∧B}
∧-I
$$

### 析取 Disjunction ∨
#### 规则
引入规则 :
$$
\dfrac
{a:A}
{ι_1a : A∨B}
∨-I_1
,
\dfrac
{b:B}
{ι_2b : A∨B}
∨-I_2
$$

消去规则 :
$$
\dfrac
{   t:A∨B
,   \begin{matrix}
        a   : A \\
        \vdots  \\
        c_1 : C \\
    \end{matrix}
,   \begin{matrix}
        b   : B \\
        \vdots  \\
        c_2 : C \\
    \end{matrix}
}
{\text{match } t \text{ with } | ι_1a⇒ c_1 | ι_2b⇒c_2 \text{ end }: C}
∨-E
$$
#### 协调
有效性 : 
$$
\dfrac
{
    \dfrac
    {a:A}
    {ι_1a:A∨B}
    ∨-I_1
    ,   \begin{matrix}
            a   : A \\
            \vdots  \\
            c_1 : C \\
        \end{matrix}
    ,   \begin{matrix}
            b   : b \\
            \vdots  \\
            c_2 : C \\
        \end{matrix}
}
{\text{match } ι_1a \text{ with } | ι_1a⇒ c_1 | ι_2b⇒c_2 \text{ end } : C}
∨-E
\\
\dfrac
{
    \dfrac
    {b:B}
    {ι_2b:A∨B}
    ∨-I_2
    ,   \begin{matrix}
            a   : A \\
            \vdots  \\
            c_1 : C \\
        \end{matrix}
    ,   \begin{matrix}
            b   : b \\
            \vdots  \\
            c_2 : C \\
        \end{matrix}
}
{\text{match } ι_2b \text{ with } | ι_1a⇒ c_1 | ι_2b⇒c_2 \text{ end } : C}
∨-E
$$

完备性:
$$
\dfrac
{
    \dfrac
    {   ι_1a:A∨B
    ,   \begin{matrix}
            a   : A \\
            \vdots  \\
            a   : A \\
        \end{matrix}
    ,   \begin{matrix}
            b   : B \\
            \vdots  \\
            a   : A \\
        \end{matrix}
    }
    {a':=\text{match } ι_1a \text{ with } | ι_1a⇒ a | ι_2b⇒a \text{ end } : A }
    ∨-E
}
{ι_1a':A∨B}
∨-I_1
$$
$$
\dfrac
{
    \dfrac
    {   ι_2b:A∨B
    ,   \begin{matrix}
            a   : A \\
            \vdots  \\
            b   : B \\
        \end{matrix}
    ,   \begin{matrix}
            b   : B \\
            \vdots  \\
            b   : B \\
        \end{matrix}
    }
    {b':=\text{match } ι_2b \text{ with } | ι_1a⇒ b | ι_2b⇒b \text{ end } : B }
    ∨-E
}
{ι_1b':A∨B}
∨-I_2
$$

### 蕴含 Implication ⊃
#### 规则
引入规则 :
$$
\dfrac
{   
    \begin{matrix}
        a   : A \\
        \vdots  \\
        b   : B \\
    \end{matrix}
}
{ λa.b:A⊃B }
⊃-I
$$

消去规则 : 
$$
\dfrac
{ λa.b:A⊃B, a:A }
{ (λa.b)a : B}
⊃-E
$$

#### 协调
有效性 : 
$$
\dfrac
{   \dfrac
    {
        \begin{matrix}
            a   : A \\
            \vdots  \\
            b   : B \\
        \end{matrix}
    }
    { λa.b:A⊃B }
    ⊃-I
,   a : A
}
{ (λa.b)a : B}
⊃-E
$$

完备性 : 
$$
\dfrac
{   \dfrac
    { λa.b:A⊃B, a:A }
    { b'=(λa.b)a : B}
    ⊃-E
}
{ λa.b' : A⊃B}
⊃-I
$$

### 真假 Top & Bottom
#### 规则
真命题 $⊤$ 仅有引入规则, 假命题 $⊥$ 仅有消去规则.

引入规则 : 
$$
\dfrac
{}
{():⊤}
⊤-I
$$

消去规则 : 
$$
\dfrac
{\_:⊥}
{c:C}
⊥-E
$$

#### 协调
真命题没有消去规则, 无需证明完备性, 只证有效性:
$$
\dfrac
{}
{():⊤}
⊤-I
$$

假命题没有引入规则, 无需证明有效性, 只证完备性:
$$
\dfrac
{\_:⊥}
{f:⊥}
⊥-E
$$


### 拓展 Extension 
- **否定 Negation** $¬A := A⊃⊥$
- **等价 Equivalence** $A≡B := (A⊃B)∧(B⊃A)$

### 杂言 Misc
- 从范畴角度看, 合取还有一种定义方式, 参见[积](../../Category/Algebra-of-Programming/(co)products.md).
- $\text{Bool}:=⊤∨⊤$

### 练习 Exercise
- 证明 $A⊃(B∨C) ≡ (A⊃B)∨(A⊃C)$.
- 证明 $(A⊃B)⊃C ≡ (A∨C)∧(B⊃C)$.