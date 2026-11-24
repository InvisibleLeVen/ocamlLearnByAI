# 第 5 课：递归函数与 let rec

日期：2026-11-11
教材：CS3110 递归函数部分（本轮章节页无法访问，具体小节号未核对）。
说明：以下结果为按类型与求值规则推导的预期结果，本轮未实际运行 OCaml。

## 1. 为什么需要 `let rec`

先看一个普通 `let`：

```ocaml
let fact n = if n = 0 then 1 else n * fact (n - 1);;
```

这会报 `Unbound value fact`。原因是 `let` 默认**非递归**：函数体里的 `fact` 看不到正在定义的自己。

要自引用，必须写 `let rec`：

```ocaml
let rec fact n = if n = 0 then 1 else n * fact (n - 1);;
```

交互环境显示：

```ocaml
val fact : int -> int = <fun>
```

## 2. 求值过程

以 `fact 3` 为例：

```text
fact 3
= 3 * fact 2
= 3 * (2 * fact 1)
= 3 * (2 * (1 * fact 0))
= 3 * (2 * (1 * 1))
= 6
```

递归调用不断把参数减小，直到到达 `fact 0`，再逐层把结果乘回去。

## 3. base case 与递归 case

`fact` 有两个部分：

- **base case**：`n = 0` 时返回 `1`，不再递归；
- **递归 case**：`n > 0` 时返回 `n * fact (n - 1)`。

递归要能结束，必须保证每次递归都更接近 base case。这里 `n - 1` 让参数逐步减小。

如果 `n < 0`，这个定义永远到不了 `0`，会一直递归下去（最终栈溢出）。因此它的隐含前提是 `n >= 0`。

## 4. 另一个例子：求和

```ocaml
let rec sum_to n =
  if n = 0 then 0
  else n + sum_to (n - 1);;
```

```text
sum_to 3
= 3 + sum_to 2
= 3 + (2 + sum_to 1)
= 3 + (2 + (1 + sum_to 0))
= 6
```

同样地，base case 是 `n = 0`，递归 case 让 `n` 减小。

## 5. `let rec` 的作用域

`let rec` 和普通 `let` 一样，名字在定义之后可用；区别是它在函数体内也能看到自己。

顶层定义：

```ocaml
let rec fact n = if n = 0 then 1 else n * fact (n - 1);;
fact 5;;
```

局部定义：

```ocaml
let rec fact n = if n = 0 then 1 else n * fact (n - 1) in
fact 5
```

局部 `let rec ... in` 的绑定仍然只在 body 内有效。

## 6. 递归代替循环

C++ / Python 里常用 `for`、`while` 累加；OCaml 的函数式写法通常用递归：

- 先写 base case（什么时候停）；
- 再写递归 case（如何缩小问题）；
- 保证每次递归都向 base case 靠近。

核心 OCaml 不强调循环语法；后续还会学尾递归，避免深递归耗尽栈。

## 7. 常见错误

| 错误 | 结果 |
| --- | --- |
| 忘记 `rec` | `Unbound value fact` |
| 忘记 base case | 无限递归 / 栈溢出 |
| 递归参数没有靠近 base case | 无限递归 |
| base case 与递归分支类型不一致 | 类型错误 |

例如下面这个函数永远到不了 `0`（从 3 开始会经过 1、-1、-3……）：

```ocaml
let rec f n = if n = 0 then 0 else f (n - 2);;
```

## 8. 与 C++ / Python 对比

| 项目 | C++ / Python | OCaml |
| --- | --- | --- |
| 自引用 | 函数名通常直接可见 | 默认不可见，要写 `let rec` |
| 循环 | `for` / `while` | 主要用递归 |
| 返回值 | `return` | 函数体的值 |
| 终止条件 | 循环条件 | base case |

## 9. 本课要点

1. 函数要调用自己，必须用 `let rec`。
2. 递归函数要有 base case 和递归 case。
3. 每次递归都要让参数更接近 base case。
4. `n < 0` 时 `fact` 不会终止；写递归时要明确前提。
5. OCaml 常用递归代替循环。

## 练习

完成 [第 5 课习题](../exercises/05-recursion.md)，写出类型、值和理由。
