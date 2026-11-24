# 第 15 课：规格（Specifications）

日期：2026-11-19
教材：CS3110 规格、测试与正确性部分（具体小节号未核对）。
说明：以下代码与结果已用 OCaml 5.2.1 实际运行核对。

## 0. 一句话

**规格 = 函数的契约：在什么条件下调用（requires），保证得到什么结果（ensures）。**

类型只说明“输入输出是什么形状”；规格说明“行为对不对”。

## 1. 类型管不住的空白

```ocaml
let div x y = x / y;;
(* val div : int -> int -> int *)
```

类型 `int -> int -> int` 完全正确，但它没告诉你：

- `y = 0` 会怎样？（抛 `Division_by_zero`）
- 结果是整数除法还是浮点除法？
- 余数怎么处理？

这些都属于**行为**，类型系统不检查。第 14 课你已经验证过同一点：签名只检查类型，不检查 `add a b = a - b` 是否真的是加法。

## 2. requires / ensures

惯例写法（CS3110 风格，写在 `(** ... *)` 文档注释里）：

```ocaml
(** [div x y] requires [y <> 0].
    ensures [div x y * y + (x mod y) = x]. *)
let div x y = x / y;;
```

- **requires**（前置条件 precondition）：调用者必须保证的条件；
- **ensures**（后置条件 postcondition）：实现者必须保证的结果性质。

实际运行：

```ocaml
div 7 2;;                              (* int = 3 *)
7 = div 7 2 * 2 + (7 mod 2);;          (* bool = true *)
```

责任划分：

| 角色 | 负责 |
| --- | --- |
| 调用者 | 满足 `requires`（比如保证 `y <> 0`） |
| 实现者 | 只要调用者满足了 `requires`，就必须满足 `ensures` |

所以 `requires` 不成立时，函数可以做什么都不算违约——包括抛异常、返回无意义的值。

## 3. 前置条件 vs 用类型消掉前置条件

同一个“查找下标”的功能，有两种契约设计：

### 3.1 带前置条件

```ocaml
(** [index_of x lst] requires [List.mem x lst].
    ensures [index_of x lst] is the smallest [i] with [List.nth lst i = x]. *)
let rec index_of x lst =
  match lst with
  | [] -> assert false
  | h :: t -> if h = x then 0 else 1 + index_of x t;;

index_of 5 [1; 2; 5; 7];;   (* int = 2 *)
```

调用者必须先确认 `x` 在列表里；否则行为无保证（这里用 `assert false` 表示“不该发生”）。

### 3.2 用返回值类型消掉前置条件

```ocaml
(** [index_of_opt x lst] ensures
    - the result is [Some i], where [i] is the smallest index with
      [List.nth lst i = x], if [x] occurs in [lst];
    - the result is [None], if [x] does not occur in [lst]. *)
let rec index_of_opt x lst =
  match lst with
  | [] -> None
  | h :: t -> if h = x then Some 0 else Option.map succ (index_of_opt x t);;

index_of_opt 5 [1; 2; 5; 7];;   (* int option = Some 2 *)
index_of_opt 9 [1; 2; 5; 7];;   (* int option = None *)
```

这里没有前置条件，代价是返回类型多了 `option`，调用者必须处理 `None`。

两种都合理，关键是：**契约要写清楚，不能让调用者猜。**

## 4. 写规格的三条经验

1. **ensures 要能被检查**：写成“返回值满足某条可验证的性质”，而不是“返回正确的结果”。
2. **别写过强的 requires**：前置条件越强，函数越好写，但调用者越难用。
3. **规格与实现分开看**：同一个规格可以有多种实现；实现改了，只要规格不变，调用者不受影响（和第 12 课的表示独立性同源）。

## 5. 常见误解

| 误解 | 实际情况 |
| --- | --- |
| 类型对了，函数就是对的 | 类型只保证形状；`div` 在 `y = 0` 时仍会抛异常 |
| `requires` 是给实现者看的提醒 | 它是调用者的义务；实现者可以假定它成立 |
| `ensures` 可以由实现随便写 | 写下的 `ensures` 就是实现必须兑现的承诺，测试/证明据此检查 |
| 规格必须写成公式 | 日常也可用清晰的自然语言，关键是精确、可判定 |

## 6. 本课要点

1. 规格 = `requires`（调用者保证）+ `ensures`（实现者保证）。
2. 类型描述形状，规格描述行为；两者互补。
3. 前置条件可以通过换返回类型（如 `option`）来消除，但要付出调用方处理的分支。
4. 规格是测试与正确性论证的依据：后面写测试时，测的就是 `ensures`。

## 练习

完成 [第 15 课习题](../exercises/15-specifications.md)。
