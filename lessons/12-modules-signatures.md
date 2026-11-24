# 第 12 课：模块、签名与抽象类型

日期：2026-11-13
教材：CS3110 的模块与签名部分（具体小节号未核对）。
说明：以下类型和结果已用 OCaml 5.2.1 实际运行核对（2026-11-13）。

## 1. 为什么需要模块

前面的函数都直接放在顶层环境里。程序变大后，需要：

- **命名空间**：避免名字冲突；
- **封装**：只暴露想公开的接口，隐藏实现细节；
- **接口**：用签名说明“这个模块提供什么”。

OCaml 用**模块**（module）和**签名**（signature）实现这些。

## 2. 定义一个模块

```ocaml
module Counter = struct
  type t = int
  let zero = 0
  let inc x = x + 1
  let to_int x = x
end;;
```

- `module` 开始模块定义；
- `struct ... end` 里面是模块的实现；
- 模块名习惯大写；
- 用 `Counter.zero`、`Counter.inc` 访问模块里的值。

```ocaml
Counter.zero;;    (* int = 0 *)
Counter.inc 5;;   (* int = 6 *)
```

没有签名限定时，模块里的所有内容都对外可见，`Counter.t = int` 也没有被隐藏。

## 3. 用签名描述接口

```ocaml
module type COUNTER = sig
  type t
  val zero : t
  val inc : t -> t
  val to_int : t -> int
end;;
```

- `module type` 定义模块类型（签名）；
- `sig ... end` 里列出模块对外提供的类型和值；
- `val inc : t -> t` 表示提供一个名为 `inc` 的值，类型是 `t -> t`；
- 签名里只写 `type t`、不写 `= int`，表示 `t` 是**抽象类型**。

## 4. 用签名约束模块

```ocaml
module Hidden : COUNTER = struct
  type t = int
  let zero = 0
  let inc x = x + 1
  let to_int x = x
  let double x = x * 2
end;;
```

`module Hidden : COUNTER = struct ... end` 的意思是：实现按 `struct` 里的写，但对外只暴露 `COUNTER` 签名列出的内容。

```ocaml
Hidden.zero;;                          (* Hidden.t = <abstr> *)
Hidden.inc Hidden.zero;;               (* Hidden.t = <abstr> *)
Hidden.to_int (Hidden.inc Hidden.zero);;  (* int = 1 *)
```

`<abstr>` 表示顶层环境知道值的类型是 `Hidden.t`，但 `t` 的具体表示被隐藏了，所以无法直接打印成整数。

签名之外的内容会被隐藏：

```ocaml
Hidden.double 3;;
(* Error: Unbound value "Hidden.double" *)
```

也不能用普通 `int` 直接构造 `Hidden.t`：

```ocaml
Hidden.inc 5;;
(* Error: This expression has type int
          but an expression was expected of type Hidden.t *)
```

客户端只能通过 `zero`、`inc`、`to_int` 这些公开操作来使用 `Hidden`。

## 5. 抽象类型的意义

`t` 的具体表示是模块的实现细节。签名只承诺：

- 有类型 `t`；
- 提供 `zero : t`、`inc : t -> t`、`to_int : t -> int`。

客户端不能依赖“`t` 就是 int”这一事实。这样实现可以改成记录、字符串或其他结构，只要继续满足签名，客户端代码不用改。这称为**表示独立性**（representation independence）。

## 6. 另一个例子：栈

```ocaml
module type STACK = sig
  type 'a t
  val empty : 'a t
  val push : 'a -> 'a t -> 'a t
  val pop : 'a t -> ('a * 'a t) option
end;;

module Stack : STACK = struct
  type 'a t = 'a list
  let empty = []
  let push x s = x :: s
  let pop s =
    match s with
    | [] -> None
    | h :: t -> Some (h, t)
end;;
```

```ocaml
let s = Stack.push 1 (Stack.push 2 Stack.empty);;
Stack.pop s;;   (* (int * int Stack.t) option = Some (1, <abstr>) *)
```

客户端知道可以用 `empty`、`push`、`pop`，但不知道 `'a t` 是列表。

## 7. 与 C++ / Python 对比

| 项目 | C++ / Python | OCaml |
| --- | --- | --- |
| 命名空间 | `namespace` / 模块 | `module` |
| 接口 | 头文件、类声明 | `sig ... end` / `.mli` |
| 隐藏实现 | `private`、头文件只放声明 | 签名未列出的内容自动隐藏 |
| 抽象类型 | 前向声明、不完整类型 | 签名中只写 `type t` |
| 检查时机 | 编译期（C++）/ 约定（Python） | 编译期由类型检查器强制 |

Python 的下划线命名只是约定；OCaml 的签名隐藏由类型检查器强制执行。

## 8. 本课要点

1. `module M = struct ... end` 定义模块，用 `M.x` 访问。
2. `module type S = sig ... end` 定义签名，用 `val` 列出接口。
3. `module M : S = struct ... end` 用签名约束模块，隐藏签名之外的内容。
4. 签名中只写 `type t` 会得到抽象类型，客户端不能构造或查看其表示。
5. 抽象类型保证表示独立性：实现可换，接口不变。

## 练习

完成 [第 12 课习题](../exercises/12-modules-signatures.md)，写出模块、类型、值和理由。
