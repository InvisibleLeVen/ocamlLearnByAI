# 第 8 课：递归变体类型（树）

日期：2026-11-12
教材：CS3110 的递归变体类型与树部分（本轮章节页无法访问，具体小节号未核对）。
说明：以下结果为按类型与求值规则推导，本轮未实际运行 OCaml。

## 1. 递归类型定义

列表可以看作“空列表，或者一个元素接上另一个列表”。树也可以这样递归定义：

```ocaml
type 'a tree =
  | Leaf
  | Node of 'a * 'a tree * 'a tree;;
```

含义：

- `Leaf`：空树；
- `Node (v, l, r)`：一个节点，值为 `v`，左子树 `l`，右子树 `r`；
- 两个子树本身又是 `'a tree`，所以类型定义是递归的。

注意：`type` 定义默认就可以引用自己，不需要写 `type rec`。这一点和 `let rec` 不同。

类型：

```ocaml
Leaf;;                    (* 'a tree *)
Node (2, Leaf, Leaf);;    (* int tree *)
```

## 2. 构造一棵树

```ocaml
let t =
  Node (2,
    Node (1, Leaf, Leaf),
    Node (3, Leaf, Leaf));;
```

这棵树是：

```text
      2
     / \
    1   3
```

`t : int tree`。两个叶子节点下面都是 `Leaf`。

## 3. 树上的递归函数

求和：

```ocaml
let rec sum t =
  match t with
  | Leaf -> 0
  | Node (v, l, r) -> v + sum l + sum r;;
```

```ocaml
sum t;;   (* int = 6 *)
```

对应关系：

- base case：`Leaf` 没有值，返回 0；
- recursive case：当前节点的值 `v`，加上左子树的和，再加上右子树的和。

求值过程可以理解为：

```text
sum t
= 2 + sum (Node (1, Leaf, Leaf)) + sum (Node (3, Leaf, Leaf))
= 2 + 1 + 3
= 6
```

## 4. 模式匹配与递归结构

`Node (v, l, r)` 一次绑定三部分：

- `v`：节点值；
- `l`：左子树；
- `r`：右子树。

递归函数通常沿着类型结构写：

- 遇到 `Leaf` 就停止；
- 遇到 `Node` 就处理当前值，并递归处理左右子树。

如果漏掉 `Leaf` 或 `Node`，编译器会给出 Warning 8；运行到未匹配的构造器时会抛 `Match_failure`。

## 5. 多态树

`'a tree` 是类型构造器：

- `int tree`：每个节点是 int；
- `string tree`：每个节点是 string；
- `Leaf` 可以是任意 `'a tree`。

例如：

```ocaml
let s =
  Node ("b",
    Node ("a", Leaf, Leaf),
    Node ("c", Leaf, Leaf));;
```

`s : string tree`。

## 6. 与 C++ 的对比

C++ 里常用节点指针和 `nullptr` 表示空树；OCaml 用 `Leaf` 构造器表示空树，用 `Node` 表示分支。树是不可变的，递归函数通过模式匹配处理每个构造器，不直接操作指针。

## 7. 本课要点

1. 递归变体类型的定义可以引用自身，不需要 `type rec`。
2. `Leaf` 是 base constructor，`Node` 是 recursive constructor。
3. 树上的递归函数通常分别处理 `Leaf` 和 `Node (v, l, r)`。
4. 递归函数仍然要用 `let rec`。
5. `'a tree` 可以实例化为 `int tree`、`string tree` 等。

## 练习

完成 [第 8 课习题](../exercises/08-recursive-variants.md)，写出类型、值和理由。
