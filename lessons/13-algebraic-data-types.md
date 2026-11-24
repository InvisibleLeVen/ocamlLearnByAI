# 第 13 课：代数数据类型（ADT）

日期：2026-11-13
教材：CS3110 的数据类型 / 代数数据类型部分（具体小节号未核对）。
说明：本课补讲第 6–8 课遗漏的概念框架。此前已经用了构造器、模式匹配、option、list、tree，但没有正式命名“代数数据类型”，也没有解释它的代数结构。以下示例已用 OCaml 5.2.1 实际运行核对。

## 0. 先说明：这是一节“补概念”的课

第 7、8 课教了：

- 用 `type` 定义变体；
- 用构造器构造值；
- 用 `match` 分解值；
- 用递归类型定义树。

这些机制你已经会用了。但“什么是 ADT、为什么叫 algebraic、sum/product 是什么、introduction/elimination 是什么”没有系统讲。本课补上这些概念，并把 records 和表达式树串起来。

## 1. 一句话定义

**代数数据类型（algebraic data type, ADT）**是用构造器（constructor）组合已有类型来定义新类型的方式。

组合方式主要有两种：

- **和类型（sum type）**：值属于若干种情况之一，读作“或”；
- **积类型（product type）**：值同时包含若干部分，读作“且”。

“代数”来自类型表达式里的 `+` 和 `×`：和类型对应加法，积类型对应乘法，函数类型对应指数。下面会看到这个类比。

## 2. 和类型：几种情况之一

```ocaml
type color = Red | Green | Blue;;
```

- `Red`、`Green`、`Blue` 是三个**构造器**；
- 一个 `color` 值恰好是其中之一；
- `|` 读作“或”；
- 这种不带数据的构造器叫**零元构造器**（nullary constructor）。

```ocaml
let c = Green;;   (* color = Green *)
```

取值个数：`Red`、`Green`、`Blue` 共 3 个。若把每种情况看作 1 个值，则

```text
|color| = 1 + 1 + 1 = 3
```

这就是“和”的来源。

## 3. 积类型：同时包含若干部分

### 3.1 元组

```ocaml
let p = (3, "hi");;   (* int * string *)
```

一个 `(3, "hi")` 同时包含一个 `int` 和一个 `string`。取值个数相乘：

```text
|int * string| = |int| × |string|
```

（这里 `int`、`string` 的取值个数是无限的，所以只是形式上的乘法；用有限类型更容易看清，见第 5 节。）

### 3.2 记录

记录（record）是**带字段名的积类型**：

```ocaml
type point = { x : float; y : float };;

let p = { x = 1.0; y = 2.0 };;
p.x;;   (* float = 1. *)
```

字段访问、记录模式和函数式更新：

```ocaml
let move_right p = { p with x = p.x +. 1.0 };;
move_right p;;   (* point = {x = 2.; y = 2.} *)

let norm2 { x; y } = x *. x +. y *. y;;
norm2 p;;        (* float = 5. *)
```

要点：

- `{ x = ...; y = ... }` 构造记录；
- `p.x` 读字段；
- `{ p with x = ... }` 创建一个新记录，只改指定字段，原记录不变；
- `{ x; y }` 是记录模式，把字段绑定到同名变量。

记录和元组的区别：元组靠位置，记录靠字段名。字段名让代码更易读，也让类型更有区分度。

## 4. 和类型 + 积类型：最常见的组合

```ocaml
type shape =
  | Circle of float
  | Rectangle of float * float;;
```

读法：

- 一个 `shape` 是 `Circle` 或 `Rectangle` 之一（和类型）；
- `Circle` 携带一个 `float`；
- `Rectangle` 携带一个 `float * float`（积类型）。

```ocaml
let area s =
  match s with
  | Circle r -> 3.14 *. r *. r
  | Rectangle (w, h) -> w *. h;;

area (Circle 2.0);;              (* float = 12.56 *)
area (Rectangle (3.0, 4.0));;    (* float = 12. *)
```

大多数 ADT 都是“若干种情况，每种情况带一些数据”这个形状。

## 5. 为什么叫“代数”：用有限类型数一数

取有限类型：

```ocaml
type bit = Zero | One;;          (* 2 个值 *)
type color = Red | Green | Blue;; (* 3 个值 *)
```

| 类型 | 值个数 | 原因 |
| --- | --- | --- |
| `bit` | 2 | `1 + 1` |
| `color` | 3 | `1 + 1 + 1` |
| `bit * color` | 6 | `2 × 3` |
| `bit option` | 3 | `1 + 2`（`None` 或 `Some b`） |
| `bit * bit` | 4 | `2 × 2` |
| `bit -> color` | 9 | `3^2`（每个输入选一个输出） |

和类型用 `+`，积类型用 `×`，函数类型用指数，所以这套类型构造被称为“代数的”。

递推类型也有对应的方程：

```ocaml
type 'a list = [] | (::) of 'a * 'a list;;
(* L = 1 + A × L *)

type 'a tree = Leaf | Node of 'a * 'a tree * 'a tree;;
(* T = 1 + A × T × T *)
```

注意：递归类型的“值个数”通常是无限的，这些方程是类型结构的类比，不是普通有限算术。

## 6. 引入与消去：ADT 的两个方向

类型理论里常把操作分成两类：

- **引入（introduction）**：用构造器**构造**值。
  - `Some 3`
  - `Node (2, Leaf, Leaf)`
  - `{ x = 1.0; y = 2.0 }`
- **消去（elimination）**：用模式匹配**消费**值。
  - `match o with None -> ... | Some x -> ...`
  - `match t with Leaf -> ... | Node (v, l, r) -> ...`
  - `let { x; y } = p`

ADT 的一个重要性质是**封闭世界**（closed world）：一个类型的所有构造器都在定义处列完。因此编译器可以检查模式匹配是否穷尽；漏掉构造器会给出 Warning 8，运行到未匹配值会抛 `Match_failure`。

对比：对 `int` 写 `if n = 0 then ... else if n = 1 then ...`，编译器无法知道你是否漏了某个整数，因为 `int` 不是封闭的构造器集合。

## 7. 递归 ADT 与结构化递归

`list` 和 `tree` 都是递归 ADT：

```ocaml
type 'a list = [] | (::) of 'a * 'a list;;
type 'a tree = Leaf | Node of 'a * 'a tree * 'a tree;;
```

类型在定义中引用自己，函数通常也按结构递归：

```ocaml
let rec sum lst =
  match lst with
  | [] -> 0
  | h :: t -> h + sum t;;

let rec size t =
  match t with
  | Leaf -> 0
  | Node (_, l, r) -> 1 + size l + size r;;
```

- base case 对应非递归构造器（`[]`、`Leaf`）；
- recursive case 对应递归构造器（`::`、`Node`）；
- 对 `list` 的 `map` / `filter` / `fold`，本质上都是消去 list 的方式。

## 8. 表达式树：ADT 的经典应用

```ocaml
type expr =
  | Num of int
  | Add of expr * expr
  | Mul of expr * expr;;

let rec eval e =
  match e with
  | Num n -> n
  | Add (a, b) -> eval a + eval b
  | Mul (a, b) -> eval a * eval b;;
```

```ocaml
eval (Add (Num 2, Mul (Num 3, Num 4)));;   (* int = 14 *)
```

`expr` 是递归 ADT：一个表达式是数字、加法或乘法之一；加法和乘法又各自包含两个子表达式。

`eval` 是结构化递归：

- base case：`Num n` 直接返回 `n`；
- recursive case：先递归求左右子表达式，再做运算。

这就是解释器/编译器的基本模式：用 ADT 表示语法树，用递归函数处理树。

## 9. ADT 与“抽象数据类型”

两个缩写的区别：

| 名称 | 含义 | 例子 |
| --- | --- | --- |
| 代数数据类型（algebraic data type） | 用构造器公开定义数据形状 | `type expr = Num ... \| Add ...` |
| 抽象数据类型（abstract data type） | 用接口隐藏表示，只暴露操作 | `module Tally : TALLY` 中的 `type t` |

第 12 课的签名隐藏属于后者；本课的变体、记录、表达式树属于前者。两者经常一起用：模块内部用 ADT 实现，签名对外隐藏具体表示。

## 10. ADT 与面向对象

| 项目 | ADT + 模式匹配 | OOP 类层次 |
| --- | --- | --- |
| 扩展新数据种类 | 加构造器，必须更新所有匹配函数 | 加子类，已有方法可继承 |
| 扩展新操作 | 加新函数，不用改已有函数 | 加方法，可能改所有子类 |
| 分派方式 | 模式匹配，编译期检查穷尽性 | 虚函数/动态分派 |
| 空值 | 用 `option` 显式表示 | `null` / `None` 常见 |

这就是所谓的“表达式问题”（expression problem）：数据扩展和操作扩展，哪种方向更容易，取决于语言和设计。OCaml 的 ADT 让“加操作”很容易，但“加构造器”要改所有相关匹配。

## 11. 你已经学过的内容，现在有了名字

| 之前的内容 | ADT 术语 |
| --- | --- |
| `type color = Red \| Green \| Blue` | 和类型、零元构造器 |
| `type shape = Circle of float \| ...` | 和类型 + 带数据的构造器 |
| `option` | `1 + 'a` 的和类型 |
| `list` | 递归和类型，`1 + 'a × list` |
| `tree` | 递归和类型，`1 + 'a × tree × tree` |
| `match` | 消去（elimination） |
| `map` / `filter` / `fold` | 对 list 的消去/抽象 |
| 记录（本课） | 带字段名的积类型 |

## 12. 本课要点

1. ADT 用构造器组合类型；主要组合是和（或）与积（且）。
2. 和类型用 `|`，积类型用元组或记录；记录是带字段名的积。
3. 有限类型的值个数满足 `+`、`×`、指数的代数规律。
4. 构造器是引入，模式匹配是消去。
5. ADT 是封闭世界，所以编译器能检查匹配穷尽性。
6. 递归 ADT 对应结构化递归；表达式树 + evaluator 是经典应用。
7. 代数数据类型（公开结构）与抽象数据类型（隐藏表示）不是同一件事。

## 练习

完成 [第 13 课习题](../exercises/13-algebraic-data-types.md)，写出类型、值和理由。
