# 第 14 课：函子（Functors）

日期：2026-11-17（2026-11-18 重排）
教材：CS3110 模块进阶部分（函子；具体小节号未核对）。
说明：以下类型和结果已用 OCaml 5.2.1 实际运行核对。

## 0. 一句话

**函子 = 接收模块、返回模块的编译期函数。**

普通函数：值 → 值。  
函子：模块 → 模块。

## 1. 最小例子：Repeat

```ocaml
module type MONOID = sig
  type t
  val zero : t
  val add : t -> t -> t
end

module IntSum = struct
  type t = int
  let zero = 0
  let add a b = a + b
end

module Repeat (M : MONOID) = struct
  let rec times n x =
    if n = 0 then M.zero
    else M.add x (times (n - 1) x)
end
```

拆开看：

```ocaml
module Repeat (M : MONOID) = struct ... end
(*            ^^^^^^^^^^ 参数签名：输入必须满足 MONOID *)
(*                          ^^^^^^^^^^^^ 实现：函子的 body *)
```

- `M` 是参数模块，类型是 `MONOID`；
- 函子体里只能使用 `MONOID` 公开的 `M.zero` 和 `M.add`；
- 顶层显示：

```ocaml
module Repeat : functor (M : MONOID) -> sig val times : int -> M.t -> M.t end
```

## 2. 应用函子

```ocaml
module R = Repeat (IntSum);;
R.times 3 5;;   (* IntSum.t = 15 *)
```

`R` 是一个普通模块。`R.times` 的完整类型是：

```ocaml
int -> IntSum.t -> IntSum.t
```

因为 `IntSum` 没有加签名，`IntSum.t = int` 是透明的，所以它等价于 `int -> int -> int`。

**顶层显示成什么，取决于前端是否开启 `-short-paths`**（2026-11-19 实测更正）：

| 前端 | `P.times` 的显示 |
| --- | --- |
| 纯 `ocaml` 顶层（默认） | `int -> IntSum.t -> IntSum.t` |
| `ocaml -short-paths` | `int -> int -> int` |
| **utop**（`-short-paths` 是默认值） | `int -> int -> int` |

两种显示指的是**同一个类型**：`IntSum.t = int` 是类型等式，可以互换；差别只是打印器要不要把别名展开。

### 关键：参数模块的成员不会自动导出

`Repeat` 的 body 里只定义了 `times`，所以结果模块也只有 `times`：

```ocaml
R.times 3 5;;  (* IntSum.t = 15 *)
R.zero;;       (* Error: Unbound value "R.zero" *)
R.add;;        (* Error: Unbound value "R.add" *)
```

`M.zero`、`M.add` 只是被**调用**，没有在结果模块里被**定义**。参数签名管的是输入，不是自动导出。

想让结果模块也提供它们，要在 body 里显式定义：

```ocaml
module Repeat2 (M : MONOID) = struct
  let zero = M.zero
  let add = M.add
  let rec times n x =
    if n = 0 then M.zero
    else M.add x (times (n - 1) x)
end

module R2 = Repeat2 (IntSum);;
R2.zero;;      (* IntSum.t = 0 *)
R2.add 2 3;;   (* IntSum.t = 5 *)
```

也可以写 `include M`，把 `M` 的内容整体包含进来。

## 3. 两个签名位置

```ocaml
module Make (X : S) : R = struct ... end
(*             参数签名   结果签名 *)
```

- **参数签名** `(X : S)`：输入契约，限制函子体能使用什么；
- **结果签名** `: R`：输出契约，限制返回模块对外暴露什么；
- 结果签名可以省略。

省略结果签名时，结果接口由 body **推断**：body 里定义了什么，就暴露什么，类型等式也公开。

## 4. 结果签名示例：MakeStack

### 4.1 不写结果签名

```ocaml
module type ELEM = sig
  type t
  val eq : t -> t -> bool
end

module MakeStack (E : ELEM) = struct
  type elt = E.t
  type t = elt list
  let empty : t = []
  let push (x : elt) (s : t) : t = x :: s
end

module IntStack = MakeStack (struct
  type t = int
  let eq a b = a = b
end)
```

编译器从 body 推断出的结果接口包含：

```ocaml
type elt = int
type t = elt list     (* 即 int list，公开 *)
val empty : t
val push : elt -> t -> t
```

所以客户端可以把 `IntStack.t` 当 `int list` 用：

```ocaml
IntStack.push 1 [];;   (* 能通过 *)
```

### 4.2 写结果签名

```ocaml
module type STACK = sig
  type elt
  type t
  val empty : t
  val push : elt -> t -> t
end

module MakeHiddenStack (E : ELEM) : STACK with type elt = E.t = struct
  type elt = E.t
  type t = elt list
  let empty : t = []
  let push (x : elt) (s : t) : t = x :: s
end

module H = MakeHiddenStack (struct
  type t = int
  let eq a b = a = b
end)
```

实际运行：

```ocaml
H.empty;;                 (* H.t = <abstr> *)
H.push 1 H.empty;;        (* H.t = <abstr> *)
H.push 1 [];;
(* Error: This expression has type "'a list"
          but an expression was expected of type "H.t" *)
```

为什么列表表示被隐藏？

1. `STACK` 里 `type elt`、`type t` 都没有 `= ...`，都是抽象类型；
2. `with type elt = E.t` **只**给 `elt` 加等式，`t` 仍然抽象；
3. 结果签名是客户端能看到的全部内容，实现里的 `type t = elt list` 不在签名中，被挡在外面。

如果结果签名把 `t` 也写出来，列表表示就会公开：

```ocaml
module MakeVisibleStack (E : ELEM)
  : STACK with type elt = E.t and type t = E.t list = struct
  type elt = E.t
  type t = elt list
  let empty : t = []
  let push (x : elt) (s : t) : t = x :: s
end

module V = MakeVisibleStack (struct type t = int let eq a b = a = b end);;
V.empty;;             (* V.t = [] *)
List.length V.empty;; (* int = 0 *)
```

### 4.3 使用处的签名约束

```ocaml
module H2 : STACK with type elt = int =
  MakeStack (struct
    type t = int
    let eq a b = a = b
  end)
```

这是对 `MakeStack (...)` **应用结果**的签名约束，不是函子定义上的结果签名：

- 结果签名写在函子定义上 → 对该函子的所有应用生效；
- 使用处约束只影响当前这个模块绑定。

## 5. 三个“透明度”视角

这三件事经常混在一起：

| 视角 | 看到什么 |
| --- | --- |
| 函子体内部 `(M : MONOID)` | `M.t` 抽象，不能写 `M.add 2 3` |
| 实参模块自身 | 没有 `: MONOID` → `type t = int` 透明；写了 `: MONOID` → 抽象 |
| 函子结果 | 无结果签名 → 接口由 body 推断；有结果签名 → 只暴露签名内容 |

例子：

```ocaml
(* 函子体内部：M.t 抽象 *)
module Bad (M : MONOID) = struct
  let x = M.add 2 3
end
(* Error: int 与 M.t 不匹配 *)

(* 实参透明：IntSum.t = int，所以 R2.add 2 3 可用 *)
module IntSum = struct type t = int let zero = 0 let add a b = a + b end

(* 实参被签名封住：IntSumHidden.t 抽象，R2h.add 2 3 不可用 *)
module IntSumHidden : MONOID = struct
  type t = int
  let zero = 0
  let add a b = a + b
end

module R2h = Repeat2 (IntSumHidden);;
R2h.zero;;      (* IntSumHidden.t = <abstr> *)
R2h.add 2 3;;
(* Error: int 与 IntSumHidden.t 不匹配 *)
```

记住：**参数签名里的 `M.t` 始终抽象；应用后能否用 `int`，取决于实参模块自己有没有公开 `t`。**

## 6. 标准库中的函子

```ocaml
module IntSet = Set.Make (Int);;

let s = IntSet.add 3 (IntSet.add 1 IntSet.empty);;
IntSet.elements s;;          (* int list = [1; 3] *)
IntSet.mem 3 IntSet.empty;;  (* bool = false *)
```

`Set.Make` 的结果签名大致是 `Set.S with type elt = Ord.t`：元素类型绑定到参数模块，集合本身的表示保持抽象。`Map.Make` 同理。

## 7. 常见误解

| 误解 | 实际情况 |
| --- | --- |
| 函子返回的模块会带上参数模块的成员 | 不会；body 里定义了什么才有什么，除非显式 `include M` 或重新定义 |
| 没写结果签名就是抽象类型 | 相反；接口由 body 推断，类型等式往往公开 |
| `with type elt = E.t` 会把实现泄露出去 | 它只给 `elt` 加等式；`t` 仍抽象，除非签名也写 `type t = ...` |
| `module H : S = F (X)` 和函子的结果签名是一回事 | 不是；前者是使用处约束，只影响 `H`，后者写在函子定义上 |

## 8. 本课要点

1. 函子：`module Make (X : S) = struct ... end`，模块 → 模块，编译期应用。
2. 参数签名 `(X : S)` 管输入；参数成员不会自动导出到结果模块。
3. 结果签名 `: R` 管输出；不写时接口由 body 推断。
4. 结果签名中的 `type t` 抽象时，实现里的表示被隐藏；`with type elt = ...` 只共享指定的类型。
5. 应用后能否使用具体类型（如 `int`），还取决于实参模块自身是否公开了类型等式。

## 练习

完成 [第 14 课习题](../exercises/14-functors.md)，写出类型、值、错误位置和理由。
