# 第 10 课：filter 与 fold_right

日期：2026-11-12
教材：CS3110 的高阶函数与列表抽象部分（具体小节号未核对）。
说明：以下类型和结果已用 OCaml 5.2.1 实际运行核对（2026-11-12）。

## 1. filter：保留满足条件的元素

`filter` 接收一个判断函数（predicate）`p : 'a -> bool`，只保留让 `p` 返回 `true` 的元素：

```ocaml
let rec filter p lst =
  match lst with
  | [] -> []
  | h :: t -> if p h then h :: filter p t else filter p t;;
```

类型：

```ocaml
filter : ('a -> bool) -> 'a list -> 'a list
```

```ocaml
filter (fun x -> x mod 2 = 0) [1; 2; 3; 4];;   (* int list = [2; 4] *)
filter (fun x -> x > 0) [3; -1; 0; 5];;        (* int list = [3; 5] *)
```

和 `map` 的区别：

- `map` 对每个元素做变换，结果长度不变；
- `filter` 不改变元素，只决定保留或丢弃，结果长度可能变短。

求值过程（保留偶数）：

```text
filter even [1;2;3;4]
= filter even [2;3;4]                  (* 1 不满足 *)
= 2 :: filter even [3;4]
= 2 :: filter even [4]
= 2 :: 4 :: filter even []
= [2; 4]
```

## 2. fold_right：从右往左把列表折叠成一个值

`fold_right` 用一个函数 `f` 和初始值 `init`，把整个列表合并成一个结果：

```ocaml
let rec fold_right f lst init =
  match lst with
  | [] -> init
  | h :: t -> f h (fold_right f t init);;
```

类型：

```ocaml
fold_right : ('a -> 'b -> 'b) -> 'a list -> 'b -> 'b
```

读法：

- `f : 'a -> 'b -> 'b`：第一个参数是当前元素 `h`，第二个参数是“尾部已经折叠好的结果”；
- `init : 'b`：空列表时的结果；
- 返回 `'b`。

```ocaml
fold_right ( + ) [1; 2; 3] 0;;                       (* int = 6 *)
fold_right (fun x acc -> x :: acc) [1; 2; 3] [];;   (* int list = [1; 2; 3] *)
fold_right (fun x acc -> x * acc) [1; 2; 3; 4] 1;;  (* int = 24 *)
```

第二个例子说明：`fold_right` 也能“重建”列表；`acc` 在这里是已经处理好的尾部。

## 3. fold_right 的求值顺序

以减法为例：

```ocaml
fold_right (fun x acc -> x - acc) [1; 2; 3] 0;;   (* int = 2 *)
```

展开：

```text
fold_right f [1;2;3] 0
= f 1 (fold_right f [2;3] 0)
= f 1 (f 2 (fold_right f [3] 0))
= f 1 (f 2 (f 3 0))
= 1 - (2 - (3 - 0))
= 1 - (2 - 3)
= 1 - (-1)
= 2
```

注意两点：

1. `fold_right` 先把右半部分折叠完，再回来处理当前元素；
2. 因此 `f` 是**右结合**的：`1 - (2 - (3 - 0))`，不是 `(1 - 2) - 3`。

这也意味着 `fold_right` 的递归调用不在尾位置，长列表可能占用较多栈空间。尾递归版本是 `fold_left`，下一课讲。

## 4. 用 fold_right 表达 map

`map` 可以看成“把每个元素变换后，再用 `::` 收集起来”：

```ocaml
let map_via_fold f lst =
  fold_right (fun x acc -> f x :: acc) lst [];;
```

```ocaml
map_via_fold : ('a -> 'b) -> 'a list -> 'b list
map_via_fold (fun x -> x * 2) [1; 2; 3];;   (* int list = [2; 4; 6] *)
```

这里 `acc` 是“尾部已经 map 好的列表”，`f x :: acc` 把当前元素的变换结果接到它前面。

`filter` 也可以用同样方式表达：在 `f` 里根据 predicate 决定是否把 `x` 接到 `acc` 前面。

## 5. 与 C++ / Python 对比

| 项目 | C++ / Python | OCaml |
| --- | --- | --- |
| 按条件保留 | `std::copy_if`、`filter` | `filter` |
| 合并成一个值 | `std::accumulate`、`reduce` | `fold_right` / `fold_left` |
| 从右结合 | `std::accumulate` 默认左结合 | `fold_right` 右结合 |

C++ 的 `std::accumulate` 默认从左往右；OCaml 的 `fold_right` 明确从右往左，`fold_left` 从左往右。

## 6. 本课要点

1. `filter : ('a -> bool) -> 'a list -> 'a list`，按 predicate 保留元素。
2. `fold_right : ('a -> 'b -> 'b) -> 'a list -> 'b -> 'b`，把列表折叠成一个值。
3. `f` 的第一个参数是当前元素，第二个参数是尾部已折叠的结果。
4. `fold_right` 右结合，先处理尾部，再处理当前元素。
5. `map` 和 `filter` 都可以用 `fold_right` 表达。

## 练习

完成 [第 10 课习题](../exercises/10-filter-fold.md)，写出类型、值和理由。
