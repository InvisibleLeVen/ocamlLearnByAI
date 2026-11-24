# 第 17 课：正确性与结构归纳

日期：2026-11-20
教材：CS3110 第 8 章 Correctness，8.7–8.9（正确性证明）；补 4.8 高阶函数的代数定律、3.12 自然数。
说明：本讲义以等式推导为主；文中代码定义已于 2026-11-20 用 OCaml 5.5.2 实际编译运行核对。

## 0. 一句话

**结构归纳 = 对函数递归所“消耗”的那个结构做数学归纳。**

第 16 课测试抽样，覆盖有限输入；本课证明，覆盖所有有限输入。

## 1. 代入推理的三个动作

纯函数式代码满足引用透明：相等的表达式可以在任何位置互换而不改变结果。所以证明里只做三件事：

1. **展开**：用定义替换函数调用。只有当实参的构造器已知、`match` 能确定走哪一支时才可展开。例如 `append [] l2` 可直接展成 `l2`；`append l1 l2`（`l1` 未知）不能展开。
2. **代换**：用已知等式（归纳假设 IH、先前证过的引理）替换子表达式。
3. **算术/逻辑事实**：如 `x + 0 = x`、`(a + b) + c = a + (b + c)`。OCaml 的 `int` 加法按 2^63 取模，仍满足结合律。

每一步都要注明依据。

## 2. 列表上的结构归纳

本课使用的定义：

```ocaml
let rec length = function
  | [] -> 0
  | _ :: t -> 1 + length t

let rec append l1 l2 =
  match l1 with
  | [] -> l2
  | h :: t -> h :: append t l2

let rec map f = function
  | [] -> []
  | h :: t -> f h :: map f t

let rec rev = function
  | [] -> []
  | h :: t -> rev t @ [h]
```

（`@` 就是 `append`。）

归纳原理：要证 P(l) 对所有列表 l 成立，需要证：

- **基础**：P([])；
- **归纳**：任取 `h` 与 `t`，在假设 P(t)（归纳假设，IH）下证明 P(h :: t)。

合理性：每个列表都是 `[]` 经过有限次 `::` 构造出来的；基础覆盖 `[]`，归纳步覆盖“再加一个元素”的每一次。

## 3. 例 1：`length (append l1 l2) = length l1 + length l2`

**归纳变量取 `l1`**——`append` 的递归在 `l1` 上。`l2` 任取并固定。

**基础** `l1 = []`：

```
length (append [] l2)
= length l2                       (* 展开 append，匹配 [] *)
= 0 + length l2                   (* 0 + x = x *)
= length [] + length l2           (* length [] = 0 *)
```

**归纳** `l1 = h :: t`，IH：`length (append t l2) = length t + length l2`：

```
length (append (h :: t) l2)
= length (h :: append t l2)       (* 展开 append，匹配 h :: t *)
= 1 + length (append t l2)        (* 展开 length *)
= 1 + (length t + length l2)      (* IH *)
= (1 + length t) + length l2      (* + 的结合律 *)
= length (h :: t) + length l2     (* 展开 length，反向 *)
```

□

三个要点：归纳变量必须是递归消耗的参数；IH 必须用上；每一步注明依据。

## 4. 例 2（4.8 的代数定律）：map 对 append 的分配律

定理：`map f (append l1 l2) = append (map f l1) (map f l2)`。

归纳变量仍取 `l1`（`append` 的递归在 `l1` 上）。

**基础**：

```
map f (append [] l2)
= map f l2                                   (* append [] l2 = l2 *)
= append [] (map f l2)                       (* append [] x = x *)
= append (map f []) (map f l2)               (* map f [] = [] *)
```

**归纳**（IH：`map f (append t l2) = append (map f t) (map f l2)`）：

```
map f (append (h :: t) l2)
= map f (h :: append t l2)                   (* 展开 append *)
= f h :: map f (append t l2)                 (* 展开 map *)
= f h :: append (map f t) (map f l2)         (* IH *)
= append (f h :: map f t) (map f l2)         (* append 反向：append (a :: X) Y = a :: append X Y *)
= append (map f (h :: t)) (map f l2)         (* map 反向 *)
```

□

这条定律说明 `map` 是列表结构上的“同态”：先拼接再映射，等于先分别映射再拼接。

（“幺半群”指：一个集合 + 满足结合律的二元运算 + 单位元。列表在 `@` 与 `[]` 下满足这三条：`@` 封闭且结合，`[]` 是单位元。`map f` 同时保持运算与单位元，故称同态；`@` 不要求交换律。）

## 5. 另一种归纳类型：自然数（3.12）

```ocaml
type nat = Z | S of nat

let rec add n m =
  match n with
  | Z -> m
  | S p -> S (add p m)
```

`add` 的递归在第一个参数 `n` 上。

定理：`add n Z = n`。**对 n 归纳**。

**基础**：

```
add Z Z = Z                    (* 展开 add，匹配 Z → m = Z *)
```

**归纳**（IH：`add t Z = t`，`n = S t`）：

```
add (S t) Z
= S (add t Z)                  (* 展开 add，匹配 S p *)
= S t                          (* IH *)
```

□

对比 `add Z n = n`：一步展开即得，**不需要归纳**——因为递归不在第二个参数上。两个命题看似对称，证明方式不同，原因就是递归位置不同。

常见疑问：`S p` 里的 `p` 不是 `n`，而是 `n` 去掉一层 `S` 后的前驱。每次递归都在更小的结构上：`n` 依次为 3、2、1、0，匹配 `Z` 时停止。若误写成 `S (add n m)`（递归传 `n`），才会无限递归——OCaml 不检查终止性，终止性靠“递归作用在更小的结构上”来论证（2026-11-23 实测跟踪：正确版本 n 递减到 0；传 `n` 的版本 n 恒为 3）。

## 6. 常见错误

| 错误 | 表现 |
| --- | --- |
| 对错误的参数归纳 | 如对 `append` 的 `l2` 归纳，归纳步推不动 |
| 不用 IH | 归纳步只是“重做一遍特例”，不构成证明 |
| 循环论证 | 归纳步直接引用要证的命题 |
| 省略依据 | 结论对，但读者无法检查 |
| 只证基础 | 只覆盖 `[]`，没有 `h :: t` |
| 误用展开 | 实参构造器未知时就展开（如直接展开 `append l1 l2`） |

## 7. 证明与测试的分工

- 测试：有限样本，能发现错误；
- 证明：所有（有限）输入，能排除错误——但前提是规格写对、对代码语义的理解也正确；
- 两者互补：证明也依赖测试来确认定义与规格没有笔误。

## 8. 练习

完成[第 17 课习题](../exercises/17-correctness-proofs.md)。
