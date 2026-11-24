# 第 17 课练习：正确性证明与结构归纳

布置日期：2026-11-20
状态：待作答

证明题要求写出：归纳变量的选择、基础、归纳步（含 IH 的使用），以及每一步的依据。推导可以用文字贴出，不必写代码。

## E17-1 · `length` 与 `map`

```ocaml
let rec length = function
  | [] -> 0
  | _ :: t -> 1 + length t

let rec map f = function
  | [] -> []
  | h :: t -> f h :: map f t
```

证明：对任意函数 `f` 和列表 `l`，`length (map f l) = length l`。写明归纳变量、基础、IH 与归纳步的每一步依据。

## E17-2 · `map` 与 `append`

```ocaml
let rec append l1 l2 =
  match l1 with
  | [] -> l2
  | h :: t -> h :: append t l2

let rec map f = function
  | [] -> []
  | h :: t -> f h :: map f t
```

证明：`map f (append l1 l2) = append (map f l1) (map f l2)`。说明对哪个列表归纳、为什么。

## E17-3 · 自然数

```ocaml
type nat = Z | S of nat

let rec add n m =
  match n with
  | Z -> m
  | S p -> S (add p m)
```

1. 证明：对任意 `n`，`add n Z = n`。
2. 说明为什么 `add Z n = n` 不需要归纳。

## E17-4 · 用引理

```ocaml
let rec length = function
  | [] -> 0
  | _ :: t -> 1 + length t

let rec append l1 l2 =
  match l1 with
  | [] -> l2
  | h :: t -> h :: append t l2

let rec rev = function
  | [] -> []
  | h :: t -> rev t @ [h]
```

课上已证：`length (append l1 l2) = length l1 + length l2`（引理）。用它证明 `length (rev l) = length l`。

## 复习题 · R40（不计入 E17 总分，单独更新复习状态）

回到第 16 课的黑盒/白盒：

```ocaml
(** [take n lst] requires [n >= 0].
    ensures the result is the first [min n (length lst)] elements of [lst]. *)
let rec take n lst =
  match lst with
  | [] -> []
  | _ when n <= 0 -> []
  | h :: t -> h :: take (n - 1) t
```

1. 白盒：列出实现里的每个分支，并为每个分支指定至少一条 `(n, lst)` 用例。
2. 黑盒：至少 3 组 `(n, lst)`，说明每组测什么。
3. `take (-1) [1;2]` 的期望结果是什么？它需要 `requires` 吗？
