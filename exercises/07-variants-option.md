# 第 7 课练习：option 与自定义变体类型

布置日期：2026-11-12
状态：E07-1、E07-2、E07-3 均通过；本组已完成（2026-11-12）

本组为自编练习。先思考，再运行验证；回答时写出类型、值和理由。

## E07-1 · 变体与模式匹配

```ocaml
type direction = North | South | East | West;;

let opposite d =
  match d with
  | North -> South
  | South -> North
  | East -> West
  | West -> East;;
```

`opposite` 的类型是什么？`opposite East` 的值是什么？如果删掉 `| West -> East` 这一行，会发生什么？说明理由。

用户答案：1. 类型是direction -> direction 2. opposite East 的值是 West 3. 会报warning，match 匹配不穷尽。

补答：能编译成功，调用opposite West会抛Match_failure异常。

老师反馈：第 1、2 问正确。第 3 问方向正确：删掉后匹配不穷尽，编译器给出 Warning 8。补答“能编译成功，调用 `opposite West` 会抛 `Match_failure` 异常”正确，完整说明了默认编译与运行时后果。评分：100/100。判定：通过。

## E07-2 · 带数据的构造器

```ocaml
type shape =
  | Circle of float
  | Rectangle of float * float;;
```

写一个函数 `area : shape -> float`，计算圆或矩形的面积。圆面积用 `3.14 *. r *. r`。

要求：

1. 写出函数定义；
2. 计算 `area (Circle 2.0)` 和 `area (Rectangle (3.0, 4.0))` 的值；
3. 说明模式 `Circle r` 和 `Rectangle (w, h)` 分别绑定了什么。

用户答案：1. `let area s = match s with | Circle r -> 3.14 *. r *. r | Rectangle (w, h) -> w *. h ;;` 2. `float = 12.56`，`float = 12.` 3. r 绑定 2.0，w 绑定 3.0，h 绑定 4.0。

老师反馈：函数定义正确，两个结果正确，三个绑定也正确。实际运行确认 `area (Circle 2.0) = 12.56`、`area (Rectangle (3.0, 4.0)) = 12.`。评分：100/100。判定：通过。

## E07-3 · option 与安全除法

```ocaml
let safe_div x y =
  if y = 0 then None
  else Some (x / y);;
```

1. `safe_div` 的类型是什么？
2. `safe_div 6 2` 和 `safe_div 1 0` 的值分别是什么？
3. 写一个函数 `get_or_zero : int option -> int`：`None` 返回 0，`Some q` 返回 q。

用户答案：1. `int -> int -> int option`；2. `Some 3`、`None`；3. `let get_or_zero val = match val with | Some x -> x | None -> 0;;`

改错：

```ocaml
let get_or_zero opt =
  match opt with
  | Some x -> x
  | None -> 0;;
```

老师反馈：第 1、2 问正确；第 3 问的匹配逻辑正确。首次把保留字 `val` 用作参数名，导致 `Syntax error`；改成 `opt` 后代码正确，类型为 `int option -> int`，`get_or_zero (Some 3) = 3`、`get_or_zero None = 0`。评分：100/100（改错后）。判定：通过。
