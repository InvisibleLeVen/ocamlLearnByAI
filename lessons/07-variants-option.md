# 第 7 课：option 与自定义变体类型

日期：2026-11-12
教材：CS3110 的变体类型与 option 部分（本轮章节页无法访问，具体小节号未核对）。
说明：以下结果已用 OCaml 5.2.1 实际运行核对（2026-11-12）。

## 1. 为什么需要变体类型

有些值只能是几种固定形状之一：

- 方向：北、南、东、西；
- 形状：圆或正方形；
- 计算结果：有值，或者没有值。

OCaml 用**变体类型**（variant type）表达“几种情况之一”，并可以用模式匹配分别处理。

## 2. 定义没有数据的变体

```ocaml
type color = Red | Green | Blue;;
```

- `type` 开始一个类型定义；
- `color` 是类型名，习惯用小写；
- `Red`、`Green`、`Blue` 是**构造器**（constructor），习惯用大写；
- `Red`、`Green`、`Blue` 都是 `color` 类型的值。

```ocaml
let c = Green;;   (* val c : color = Green *)
```

## 3. 用 match 处理变体

```ocaml
let color_to_int c =
  match c with
  | Red -> 0
  | Green -> 1
  | Blue -> 2;;
```

```ocaml
color_to_int Green;;   (* int = 1 *)
```

每个构造器一个分支；分支结果的类型必须一致（这里都是 int）。

如果漏掉某个构造器，编译器会给出 Warning 8（匹配不穷尽）；运行到漏掉的构造器时会抛 `Match_failure`。

## 4. 带数据的构造器

构造器可以携带数据：

```ocaml
type shape =
  | Circle of float
  | Square of float;;
```

- `Circle 2.0 : shape`；
- `Square 3.0 : shape`。

模式匹配时把数据绑定出来：

```ocaml
let area s =
  match s with
  | Circle r -> 3.14 *. r *. r
  | Square a -> a *. a;;
```

```ocaml
area (Circle 2.0);;   (* float = 12.56 *)
area (Square 3.0);;   (* float = 9. *)
```

注意：

- `Circle` 后面跟一个 float；`Square` 也是；
- 模式 `Circle r` 把半径绑定到 `r`；
- 浮点乘法用 `*.`，不是 `*`。

多个数据可以用元组：

```ocaml
type rectangle = Rect of float * float;;
```

模式写成 `Rect (w, h)`。

## 5. option 类型

OCaml 内置了 `option`：

```ocaml
type 'a option = None | Some of 'a
```

含义：

- `None`：没有值；
- `Some x`：有一个类型为 `'a` 的值 `x`。

```ocaml
Some 3;;        (* int option *)
Some "hi";;     (* string option *)
None;;          (* 'a option *)
```

`option` 常用于“可能失败、可能没有结果”的函数，避免使用 null 或异常表示失败。

## 6. 用 option 表示可能失败的计算

```ocaml
let safe_div x y =
  if y = 0 then None
  else Some (x / y);;
```

```ocaml
safe_div 6 2;;   (* int option = Some 3 *)
safe_div 1 0;;   (* int option = None *)
```

调用者必须处理两种情况：

```ocaml
let has_value o =
  match o with
  | None -> false
  | Some _ -> true;;
```

`Some _` 表示“有值，但不关心具体值”。两个分支都返回 bool。

## 7. 穷尽性与构造器

变体匹配通常要覆盖所有构造器：

```ocaml
let color_to_int c =
  match c with
  | Red -> 0
  | Green -> 1;;
```

漏掉 `Blue` 会得到 Warning 8；运行 `color_to_int Blue` 会抛 `Match_failure`。

这和列表匹配的道理一样：类型检查通过，但匹配可能不穷尽。

## 8. 与 C++ / Python 对比

| 项目 | C++ / Python | OCaml |
| --- | --- | --- |
| 固定几种情况 | `enum` / `std::variant` | 变体类型 |
| 带数据的情况 | `std::variant` / 类层次 | 构造器直接带数据 |
| 可能没有值 | `std::optional` / `None` | `option` 的 `None` / `Some` |
| 分支处理 | `switch` / `if` | `match`，编译器检查穷尽性 |

Python 的 `None` 不强制调用者处理；OCaml 的 `option` 让“没有值”成为类型的一部分，通常要用 `match` 显式处理。

## 9. 本课要点

1. `type t = A | B of int` 定义变体类型；构造器大写，类型名小写。
2. 构造器可以不带数据，也可以带数据。
3. 用 `match` 按构造器分支，分支结果类型一致。
4. `option` 是 `None | Some of 'a`，表示可能有值或没有值。
5. 漏掉构造器会产生 Warning 8；运行到未匹配值会抛 `Match_failure`。

## 练习

完成 [第 7 课习题](../exercises/07-variants-option.md)，写出类型、值和理由。
