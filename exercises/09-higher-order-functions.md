# 第 9 课练习：高阶函数与 map

布置日期：2026-11-12
状态：全部通过（2026-11-12）

本组为自编练习。先思考，再运行验证；回答时写出类型、值和理由。

## E09-1 · 阅读类型与值

已知：

```ocaml
let apply_twice f x = f (f x);;
let rec map f lst =
  match lst with
  | [] -> []
  | h :: t -> f h :: map f t;;
```

回答：

1. `apply_twice (fun x -> x * 3) 2` 的值是多少？
2. `apply_twice` 的类型是什么？为什么第一个参数是 `'a -> 'a`，而不是 `'a -> 'b`？
3. `map (fun x -> x mod 2 = 0) [1; 2; 3; 4]` 的值和结果类型分别是什么？
4. `map` 的完整类型是什么？

用户答案：1. 18 2. `'a -> 'a -> a -> a`，因为 `apply_twice` 定义中 `f` 的返回值又作为参数传给了 `f`，所以 `f` 的返回值和参数应该是同类型的。3. `[false; true; false; true]`，`bool list`。4. `('a -> 'b) -> 'a list -> 'b list`。

老师反馈：第 1、3、4 问正确；第 2 问理由正确，但类型写法不合法——缺少参数位置的括号，且 `a` 不是 OCaml 类型变量（类型变量必须写成 `'a`）。请重写成合法的 OCaml 类型。评分：90/100。
订正（2026-11-12）：`('a -> 'a) -> 'a -> 'a` 正确。补至 100/100。

## E09-2 · 自己写 map

不看上面的定义，写出列表的 `map`。

要求：

1. 写出函数定义；
2. 写出 `map` 的类型；
3. 计算 `map (fun s -> s ^ "!") ["a"; "b"]` 的值；
4. 如果把递归分支写成 `f (h :: map f t)`，会发生什么？为什么？

用户答案：1. `let rec map f lst = match lst with | [] -> [] | h :: t -> f h :: map f t;;` 2. `('a -> 'b) -> 'a list -> 'b list` 3. `["a!"; "b!"]` 4. `f` 的参数类型会变成 `'a list -> 'b list`，每次递归都会调用 `f` 处理 `'a list`。

老师反馈：第 1、2、3 问正确。第 4 问方向部分正确（`f` 会被要求处理列表），但表述不准确：这个分支实际上**无法通过类型检查**。实际运行报 occurs check：`map f t` 要求 `t : 'a list list`，而 `t : 'a list`。提示：先看 `h : 'a`、`map f t : 'b list`，`h :: map f t` 要求什么？再看递归调用 `map f t` 对 `t` 的元素类型要求什么？请据此重答。评分：85/100。
订正（2026-11-12）：用户指出该分支能通过类型检查，类型为 `('a list -> 'a list) -> 'a list -> 'a list`。单独运行确认正确。老师上一轮把测试函数命名为 `bad_map` 却调用 `map`，实际调用的是先前定义的列表 map，因此得到 occurs check；这是老师的验证错误，特此更正。该版本确实能编译，但 `f` 收到的是整个 `h :: map f t` 列表，不是单个元素，所以它不是逐元素的 map。评分：95/100，待补语义小问：若 `f = fun xs -> 0 :: xs`，`map f [1; 2]` 返回什么？

## E09-3 · option 上的 map

写一个函数 `map_option : ('a -> 'b) -> 'a option -> 'b option`：`Some x` 时对 `x` 应用 `f`，`None` 时返回 `None`。

要求：

1. 写出函数定义；
2. 计算 `map_option (fun x -> x + 1) (Some 7)` 和 `map_option (fun x -> x + 1) None` 的值；
3. 说明为什么 `None` 分支不能对“里面的值”应用 `f`。

用户答案：1. `let rec map_option f opt = match opt with | None -> None | Some x -> Some (f x);;` 2. `Some 8`、`None` 3. 因为 `f` 期望的参数是一个 `'a`，不是一个 option。

老师反馈：定义、两个值和对 `None` 分支的解释均正确（`None` 里没有可提取的 `'a`，不能把 `f` 应用上去）。`let rec` 没有递归调用，可去掉 `rec`，但语法上允许。评分：100/100。

## E09-4 · 树上的 map（复习第 8 课）

沿用第 8 课的类型：

```ocaml
type 'a tree =
  | Leaf
  | Node of 'a * 'a tree * 'a tree;;
```

写一个递归函数 `map_tree : ('a -> 'b) -> 'a tree -> 'b tree`，对每个节点的值应用 `f`，保持树的结构不变。

要求：

1. 写出函数定义；
2. 写出 `map_tree` 的类型；
3. 对第 8 课的 `t` 计算 `map_tree (fun x -> x * 10) t` 的值，写出整棵树；
4. 说明 `map_tree` 与列表 `map` 的对应关系（base case、递归 case 分别对应什么）。

用户答案：1. `let rec map_tree f tree = match tree with | Leaf -> Leaf | Node (v, l, r) -> Node (f v, map f l, map f r);;` 2. `('a -> 'b) -> 'a tree -> 'b tree` 3. `Node (20, Node (10, Leaf, Leaf), Node (30, Leaf, Leaf))` 4. base case：树的 `map` 到 `Leaf`，列表的到空列表，直接返回 `Leaf` 和 `[]`；递归 case：列表用 `f` 处理 `h`，再和 `map` 剩下的元素拼起来，`Node` 这边也参考这样处理，构造一个 `Node` 值，调用 `f` 处理当前值，左子树和右子树分别继续递归调用 `map_tree`。

老师反馈：类型、结果值和第 4 问思路正确。但第 1 问代码里递归调用写成了 `map f l`、`map f r`——`map` 是列表函数，`l`、`r` 是树，实际运行报 `This expression has type 'a tree but an expression was expected of type 'a list`。应递归调用 `map_tree`。请改正并重述该分支。评分：75/100。

本轮首次评分：E09-1 90、E09-2 85、E09-3 100、E09-4 75，合计 88/100。

E09-4 订正（2026-11-12）：`| Node (v, l, r) -> Node (f v, map_tree f l, map_tree f r);;`。正确。实际运行 `map_tree (fun x -> x * 10) t = Node (20, Node (10, Leaf, Leaf), Node (30, Leaf, Leaf))`。E09-4 补至 100/100。

语义补答（2026-11-12）：`map f [] = []`；`map f [2] = [0; 2]`；`map f [1; 2] = [0; 1; 0; 2]`。正确；实际运行确认 `[0;1;0;2]`。E09-2 补至 100/100。

最终评分：E09-1 100、E09-2 100、E09-3 100、E09-4 100，E09 整组 100/100，判定通过。
