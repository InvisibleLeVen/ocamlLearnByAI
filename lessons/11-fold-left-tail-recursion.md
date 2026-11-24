# 第 11 课：fold_left 与尾递归

日期：2026-11-13
教材：CS3110 的高阶函数、折叠与尾递归部分（具体小节号未核对）。
说明：以下类型和结果已用 OCaml 5.2.1 实际运行核对（2026-11-13）。
本课正式复习第 5B 课的尾递归与累加器。

## 1. fold_left：从左往右折叠

```ocaml
let rec fold_left f acc lst =
  match lst with
  | [] -> acc
  | h :: t -> fold_left f (f acc h) t;;
```

类型：

```ocaml
fold_left : ('a -> 'b -> 'a) -> 'a -> 'b list -> 'a
```

和 `fold_right` 的参数顺序不同：

- `fold_right f lst init`：`f` 先接元素，再接累加结果；
- `fold_left f acc lst`：`f` 先接累加结果，再接元素。

```ocaml
fold_left ( + ) 0 [1; 2; 3];;                    (* int = 6 *)
fold_left (fun acc x -> acc - x) 0 [1; 2; 3];;   (* int = -6 *)
```

## 2. fold_left 的求值顺序

```text
fold_left f 0 [1; 2; 3]
= fold_left f (f 0 1) [2; 3]
= fold_left f (f (f 0 1) 2) [3]
= fold_left f (f (f (f 0 1) 2) 3) []
= ((0 - 1) - 2) - 3
= -6
```

对比 `fold_right`：

```ocaml
fold_right (fun x acc -> x - acc) [1; 2; 3] 0   (* 1 - (2 - (3 - 0)) = 2 *)
fold_left  (fun acc x -> acc - x) 0 [1; 2; 3]   (* ((0 - 1) - 2) - 3 = -6 *)
```

| 项目 | `fold_right` | `fold_left` |
| --- | --- | --- |
| 参数顺序 | `fold_right f lst init` | `fold_left f init lst` |
| `f` 的参数 | `f 元素 累加结果` | `f 累加结果 元素` |
| 结合方向 | 右结合 | 左结合 |
| 递归调用位置 | 不是尾位置 | 尾位置 |

## 3. 尾递归复习

**尾位置**：一个调用的结果直接作为当前函数的结果返回，后面没有待做的工作。

```ocaml
let rec fact n =
  if n = 0 then 1
  else n * fact (n - 1);;      (* 递归返回后还要乘 n：不在尾位置 *)
```

用累加器改成尾递归：

```ocaml
let rec fact_acc n acc =
  if n = 0 then acc
  else fact_acc (n - 1) (n * acc);;   (* 递归调用就是分支结果：尾位置 *)
```

```text
fact_acc 3 1
= fact_acc 2 (3 * 1) = fact_acc 2 3
= fact_acc 1 (2 * 3) = fact_acc 1 6
= fact_acc 0 (1 * 6) = fact_acc 0 6
= 6
```

要点：

- 尾递归调用自己时，OCaml 会做尾调用优化，复用栈帧，栈空间不随深度增长；
- 尾递归主要解决**栈空间**问题，不改变时间复杂度；
- 有 pending work（如 `n * ...`、`1 + ...`、`g (...)`）就不是尾调用。

`fold_left` 的递归调用就是 `match` 分支的整个结果，所以它是尾递归；`fold_right` 在递归返回后还要应用 `f`，不是尾递归。标准库的 `List.fold_left` 是尾递归的，`List.fold_right` 不是。

## 4. 用 fold_left 反转列表

```ocaml
let rev lst =
  fold_left (fun acc x -> x :: acc) [] lst;;
```

```ocaml
rev : 'a list -> 'a list
rev [1; 2; 3];;   (* int list = [3; 2; 1] *)
```

为什么能反转？每个新元素 `x` 都被放到累加列表的**最前面**：

```text
rev [1;2;3]
= fold_left f [] [1;2;3]
= fold_left f [1] [2;3]
= fold_left f [2;1] [3]
= fold_left f [3;2;1] []
= [3;2;1]
```

这是累加器常见用法：用 `::` 构建结果时，顺序会和遍历顺序相反。

## 5. 与 C++ / Python 对比

| 项目 | C++ / Python | OCaml |
| --- | --- | --- |
| 左折叠 | `std::accumulate`、`functools.reduce` | `fold_left` |
| 右折叠 | 手写递归或反向迭代 | `fold_right` |
| 循环 | `for` / `while` | 常用尾递归模拟 |
| 栈安全 | 取决于实现 | 尾递归可复用栈帧 |

在 OCaml 中，处理长列表时通常优先用 `fold_left`，因为它是尾递归的；`fold_right` 更贴近“从右往左”的定义，但可能消耗较多栈。

## 6. 本课要点

1. `fold_left : ('a -> 'b -> 'a) -> 'a -> 'b list -> 'a`。
2. `fold_left` 左结合，`f` 先收累加结果，再收元素。
3. `fold_left` 的递归调用在尾位置，是尾递归。
4. 尾递归用累加器保存部分结果，避免 pending work。
5. 尾递归节省栈空间，不改变时间复杂度。
6. `rev` 可以用 `fold_left` 加 `fun acc x -> x :: acc` 实现。

## 练习

完成 [第 11 课习题](../exercises/11-fold-left-tail-recursion.md)，写出类型、值和理由。第 5B 课的尾递归内容由本课习题一并验证。
