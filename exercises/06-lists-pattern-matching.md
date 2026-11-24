# 第 6 课练习：模式匹配与列表

布置日期：2026-11-11
状态：E06-1、E06-2、E06-3、E06-4 均通过；本组已完成（2026-11-12）

本组为自编练习。先思考，再运行验证；回答时写出类型、值和理由。

## E06-1 · 列表求和与递归过程

```ocaml
let rec sum xs =
  match xs with
  | [] -> 0
  | h :: t -> h + sum t;;
sum [1; 2; 3];;
```

`sum` 的类型是什么？`sum [1; 2; 3]` 的值是什么？请写出递归求值过程，并说明每一步匹配的是哪个模式。

用户答案：int list -> int,值是6，sum [1;2;3] (匹配 h :: t) -> 1 + sum [2;3] (匹配 h :: t) -> 1 + 2 + sum [3] (匹配 h :: t) -> 1 + 2 + 3 + sum [] (匹配 []) -> 1 + 2 + 3 + 0

老师反馈：正确。类型 `int list -> int`，值 6；递归过程每一步匹配的模式也正确。实际运行确认 `sum [1; 2; 3] = 6`。判定：通过。

## E06-2 · 自己写 length

写一个递归函数 `length`，返回列表的长度。用 `match` 处理 `[]` 和 `_ :: t`。

要求：

1. 写出函数定义；
2. 写出 `length` 的类型；
3. 计算 `length [10; 20; 30]` 的值。

用户答案：

```ocaml
let rec length lst =
  match lst with
  | [] -> 0
  | h :: t -> 1 + length t
;;
```

结果为3

补答：'a list -> int，因为length不要求list元素类型一定是int，对于其他类型的list也适用

老师反馈：函数定义与结果正确；补答的类型 `'a list -> int` 也正确。`'a` 是类型变量，表示元素可以是任意类型，因此不能写成 `int list -> int`。实际运行确认 `length : 'a list -> int`，`length [10; 20; 30] = 3`。判定：通过（补答后）。

## E06-3 · 自己写 count_positive

写一个递归函数 `count_positive : int list -> int`，统计列表中正数的个数。使用 `match` 和 `if`。

要求：

1. 写出函数定义；
2. 计算 `count_positive [3; -1; 0; 5]` 的值；
3. 说明 base case 和递归 case。

用户答案：

```ocaml
let rec count_positive lst =
  match lst with
  | [] -> 0
  | h :: t -> (
      if h > 0 then
        1 + count_positive t
      else
        count_positive t
    )
;;
```

base case 是[] 没有一个正数，返回0, 递归case，判断h是否大于0，如果大于0则结果是1 + count_positive t，否则是 count_positive t

补答：值是2，因为0不是正数

老师反馈：函数定义与 base/递归 case 说明正确；补答的值 2 也正确。3 和 5 是正数，-1 和 0 不算，所以结果是 2。实际运行确认 `count_positive : int list -> int`，`count_positive [3; -1; 0; 5] = 2`。判定：通过（补答后）。

## E06-4 · 非穷尽匹配

```ocaml
let f xs =
  match xs with
  | [] -> 0;;
f [1; 2];;
```

这段代码能通过类型检查吗？编译时会有什么警告？运行 `f [1; 2]` 会发生什么？说明理由。

用户答案：能通过类型检查，编译时会报错，因为xs有没有匹配的情况，f [1;2]应该会异常，因为match有case没匹配上。

补答：Warning 8 是什么，

后续补答：可以编译成功，f [1;2]异常发生在运行期

老师反馈：类型检查和运行异常判断正确。Warning 8 是“匹配不穷尽”（partial-match）警告；默认 `ocamlc` / 交互环境仍能编译，运行到没有分支匹配的输入 `f [1; 2]` 时才抛出 `Match_failure`。后续补答正确区分了编译与运行。判定：通过（补答后）。
