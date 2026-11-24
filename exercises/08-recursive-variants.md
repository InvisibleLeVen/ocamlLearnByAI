# 第 8 课练习：递归变体类型（树）

布置日期：2026-11-12
状态：全部通过（2026-11-12）

本组为自编练习。先思考，再运行验证；回答时写出类型、值和理由。

共用类型和示例树：

```ocaml
type 'a tree =
  | Leaf
  | Node of 'a * 'a tree * 'a tree;;

let t =
  Node (2,
    Node (1, Leaf, Leaf),
    Node (3, Leaf, Leaf));;
```

## E08-1 · 节点个数

写一个递归函数 `size : 'a tree -> int`，返回树中节点的个数。`Leaf` 算 0 个节点。

要求：

1. 写出函数定义；
2. 写出 `size` 的类型；
3. 计算 `size t` 的值；
4. 说明 base case 和递归 case。

用户答案：1. `let rec size t = match t with | Leaf -> 0 | Node (_, l, r) -> size l + size r + 1;;` 2. `'a tree -> int` 3. 3 4. base case Leaf 不算节点返回 0，recursive case 当前节点 + 左子树节点 + 右子树节点。

老师反馈：函数、类型、结果与 base/递归 case 说明均正确。评分：100/100。判定：通过。

## E08-2 · 树的高度

写一个递归函数 `depth : 'a tree -> int`，返回树的高度。约定：

- `Leaf` 的高度是 0；
- `Node (v, l, r)` 的高度是 `1 + max (depth l) (depth r)`。

可以使用 OCaml 内置的 `max`，也可以自己写 `if` 比较。

要求：

1. 写出函数定义；
2. 写出 `depth` 的类型；
3. 计算 `depth t` 的值；
4. 说明为什么 `Leaf` 的高度定义为 0。

用户答案：1. `let rec depth t = match t with | Leaf -> 0 | Node (_, l, r) -> max (depth l) (depth r) + 1;;` 2. `'a tree -> int` 3. 2 4. 题目定义的，理论上可以是任何数。

老师反馈：函数、类型和结果 2 正确。第 4 问不充分：“题目定义的”只是重复约定，没有说明理由；高度按“向下的边数”计，叶子没有向下的边，所以是 0；每个 `Node` 增加一条通向子树的边，所以 `+1`。若把 `Leaf` 定义为 1，则只有叶子的节点高度会算成 2，和边数约定不一致。请补答“为什么 Leaf 是 0 / 若 Leaf=1 会怎样”。评分：85/100，补答正确后记满分。

补答（2026-11-12）：“因为Leaf下面没有边了，高度算的应该是边的数，如果改成Leaf高度为1， depth t 会变成3，但是实际上到leaf只有两条边，单节点高度是1，到leaf只有一条边。”

老师反馈（补答）：三问均正确。`Leaf` 没有向下的边，所以按边数计高度为 0；若 `Leaf` 改为 1，`depth t` 变成 3，而按边数实际只有 2，说明两种约定整体相差 1；单节点树高 1，也正好等于它到 `Leaf` 的一条边。评分：100/100。E08 整组 100/100，判定通过。

## E08-3 · 查找元素

写一个递归函数 `mem : 'a -> 'a tree -> bool`，判断值 `x` 是否出现在树中。

要求：

1. 写出函数定义；
2. 计算 `mem 3 t` 和 `mem 4 t` 的值；
3. 说明 `Leaf` 分支返回什么，`Node` 分支如何递归检查左右子树。

提示：可以用布尔或 `||` 连接“当前节点等于 x”“左子树包含 x”“右子树包含 x”。

用户答案：`let rec mem value t = match t with | Leaf -> false | Node (v, l, r) -> if v = value then true else (mem value l) || (mem value r);;` 2. `true`、`false`，Leaf 返回 false，检查 `mem l` 和 `mem r` 是否有一个返回 true，用 `||` 逻辑。

老师反馈：函数、两个结果与递归检查说明均正确。评分：100/100。判定：通过。
