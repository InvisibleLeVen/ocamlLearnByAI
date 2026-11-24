# 第 10 课练习：filter 与 fold_right

布置日期：2026-11-12
状态：全部通过（2026-11-13）

本组为自编练习。先思考，再运行验证；回答时写出类型、值和理由。

## E10-1 · filter

写一个递归函数 `filter : ('a -> bool) -> 'a list -> 'a list`，保留让 predicate 返回 `true` 的元素。

要求：

1. 写出函数定义；
2. 写出 `filter` 的类型；
3. 计算 `filter (fun x -> x < 0) [3; -1; 0; -5]` 的值；
4. 说明 `p` 在 base case 和递归 case 中分别起什么作用。

用户答案：1. `let rec filter p lst = match lst with | [] -> [] | h :: t -> if p h then h :: filter p t else filter p t;;` 2. `('a -> bool) -> 'a list -> 'a list` 3. `[-1; -5]` 4. base case 是空列表，`p` 没作用；递归 case 用 `p` 判断 `h` 是否满足条件，满足就加入列表并和 tail 的 filter 结果拼起来，否则直接返回 tail 的 filter 结果。

老师反馈：定义、类型、值与递归 case 说明均正确。base case 中确实没有元素可判断，`p` 不参与；补充一句该分支返回 `[]` 会更完整。评分：100/100。

## E10-2 · fold_right 的求值顺序

已知：

```ocaml
let rec fold_right f lst init =
  match lst with
  | [] -> init
  | h :: t -> f h (fold_right f t init);;
```

要求：

1. 写出 `fold_right` 的类型；
2. 计算 `fold_right (fun x acc -> x - acc) [5; 2; 1] 0` 的值；
3. 展开求值过程，说明为什么结果不是 `(5 - 2) - 1`；
4. 说明 `fold_right` 的递归调用是否在尾位置。

用户答案：1. `('a -> 'b -> 'b) -> 'a list -> 'b -> 'b` 2. 4 3. `(5 - (2 - (1 - 0)))`，因为从 base case 开始返回，会先处理列表最后一个元素。4. 递归调用不在尾位置，`f` 调用比递归调用更后。

老师反馈：类型、值、展开式和尾位置判断均正确。实际运行 `fold_right (fun x acc -> x - acc) [5;2;1] 0 = 4`。递归调用是 `f` 的第二个参数，递归返回后还要应用 `f`，所以不在尾位置。评分：100/100。

## E10-3 · 用 fold_right 数正数

用 `fold_right` 写一个函数 `count_positive : int list -> int`，统计列表中正数的个数。

要求：

1. 写出函数定义；
2. 写出 `count_positive` 的类型；
3. 计算 `count_positive [3; -1; 0; 5]` 的值；
4. 说明 `acc` 在每个递归层代表什么。

用户答案：1. `let count_positive lst = fold_right (fun x rst -> if x > 0 then rst + 1 else rst) lst 0;;` 2. `int list -> int` 3. 2 4. 不清楚 `acc` 指什么。

老师反馈：定义、类型和值 2 正确；实际运行确认 `count_positive [3;-1;0;5] = 2`。第 4 问的 `acc` 就是 `fold_right` 折叠函数的第二个参数，在你的代码里叫 `rst`：它代表“当前元素之后的后缀已经统计出的正数个数”。请补答：以 `[3; -1; 0; 5]` 为例，在 `x = 3` 这一层，`rst` 的值是多少？它对应哪个后缀子列表的正数个数？评分：85/100，补答正确后记满分。

补答（2026-11-13）：“当3这一层时rst的值是1，对应的后缀子列表的正数个数是 [-1,0;5]”。正确：后缀是 `[-1; 0; 5]`，其中只有 `5` 是正数。列表写法小笔误：元素之间应为分号 `[-1; 0; 5]`。E10-3 补至 100/100。

## E10-4 · 用 fold_right 写 filter

用 `fold_right` 重新实现 `filter`。

要求：

1. 写出函数定义；
2. 写出类型；
3. 计算 `filter_via_fold (fun x -> x >= 0) [3; -1; 0; 5]` 的值；
4. 说明 `if p x then x :: acc else acc` 中，两个分支分别表示什么。

用户答案：1. `let filter_via_fold f lst = fold_right (fun h rst -> if f h then h :: rst else rst) lst [];;` 2. `('a -> bool) -> 'a list -> 'a list` 3. `[3; 0; 5]` 4. 如果 `x` 满足条件，就把当前元素放到 `acc` 列表中；否则返回 `acc` 列表。

老师反馈：定义、类型和值均正确；实际运行确认 `filter_via_fold (fun x -> x >= 0) [3;-1;0;5] = [3;0;5]`。第 4 问的两个分支正确；`acc` 是尾部已经过滤好的列表，满足条件时用 `h :: acc` 把当前元素接在它前面。评分：100/100。

本轮首次评分：E10-1 100、E10-2 100、E10-3 85、E10-4 100，合计 96/100。

最终评分：E10-1 100、E10-2 100、E10-3 100、E10-4 100，E10 整组 100/100，判定通过。
