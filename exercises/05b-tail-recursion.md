# 第 5B 课练习：尾递归与累加器

布置日期：2026-11-11
状态：暂缓——按原路线，列表/模式匹配之后再正式复习（2026-11-11）；2026-11-13 已在第 11 课正式复习，尾递归能力由 E11-2、E11-3、E11-4 验证；E05B 保留为额外练习，不单独计分。

本组为自编练习。先思考，再运行验证；回答时写出类型、值和理由。

## E05B-1 · 判断尾递归

```ocaml
let rec fact n =
  if n = 0 then 1
  else n * fact (n - 1);;

let rec fact_acc n acc =
  if n = 0 then acc
  else fact_acc (n - 1) (n * acc);;
```

哪一个函数是尾递归？说明递归调用在哪里、返回后是否还有待做的工作。计算 `fact_acc 3 1` 的值。

用户答案：待填写。

老师反馈：待作答后填写。

## E05B-2 · 把 sum_to 改成尾递归

```ocaml
let rec sum_to n =
  if n = 0 then 0
  else n + sum_to (n - 1);;
```

写一个尾递归版本 `sum_to_acc n acc`，并计算 `sum_to_acc 4 0` 的值。说明 `acc` 保存的是什么。

用户答案：待填写。

老师反馈：待作答后填写。

## E05B-3 · 判断尾位置

判断下列递归调用是否在尾位置，并说明理由：

1. `let rec f n = if n = 0 then 0 else f (n - 1)`
2. `let rec f n = if n = 0 then 0 else 1 + f (n - 1)`
3. `let rec f n = if n = 0 then 1 else n * f (n - 1)`
4. `let rec f n = if n = 0 then 0 else g (f (n - 1))`

用户答案：待填写。

老师反馈：待作答后填写。
