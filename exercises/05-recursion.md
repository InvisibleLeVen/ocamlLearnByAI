# 第 5 课练习：递归函数与 let rec

布置日期：2026-11-11
状态：E05-1、E05-2、E05-3 均通过；本组已完成

本组为自编练习。先思考，再运行验证；回答时写出类型、值和理由。

## E05-1 · 阶乘与 let rec

```ocaml
let rec fact n = if n = 0 then 1 else n * fact (n - 1);;
fact 4;;
```

`fact` 的类型是什么？`fact 4` 的值是什么？如果把第一行的 `rec` 去掉，会发生什么？为什么？

用户答案：fact类型是int -> int，24，会报错Unbound value fact, let 默认非递归，函数体里的 fact 看不到正在定义的自己。

老师反馈：正确。`fact : int -> int`，`fact 4 = 24`；去掉 `rec` 后函数体无法引用自身，报 `Unbound value fact`。判定：通过。

## E05-2 · 求和与终止条件

```ocaml
let rec sum_to n = if n = 0 then 0 else n + sum_to (n - 1);;
sum_to 4;;
```

`sum_to` 的类型和 `sum_to 4` 的值是什么？如果调用 `sum_to (-1)`，会发生什么？为什么？

用户答案：类型int -> int,值是10，sum_to (-1)会无限递归，栈溢出，因为一直到不了终止条件，走不到then分支

老师反馈：正确。`sum_to : int -> int`，`sum_to 4 = 10`；`sum_to (-1)` 永远到不了 `n = 0` 的 base case，因此无限递归并最终栈溢出。判定：通过。

## E05-3 · 自己写递归函数

用 `let rec` 写一个函数 `pow2`，返回 `2^n`，其中 `n >= 0`。只使用乘法、`if` 和递归。

要求：

1. 写出函数定义；
2. 写出 `pow2` 的类型；
3. 计算 `pow2 4` 的值，并说明 base case 和递归 case。

用户答案：let rec pow2 n = if n = 0 then 1 else pow2 (n - 1) * 2. pow2类型int -> int 要求n >= 0，pow2 4值是16, base case n是0，结果是2^0是1，其他情况，是pow2 (n-1) * 2。

老师反馈：正确。定义、类型 `int -> int`、`pow2 4 = 16`、base case 与递归 case 都正确；写成 `2 * pow2 (n - 1)` 也等价。判定：通过。
