# 第 13 课练习：代数数据类型（ADT）

布置日期：2026-11-13
状态：待作答

本组为自编练习。先思考，再运行验证；回答时写出类型、值和理由。

## E13-1 · 和、积与取值个数

已知：

```ocaml
type bit = Zero | One;;

type color = Red | Green | Blue;;

type pair = Pair of bit * bit;;

type maybe_color = NoColor | AColor of color;;
```

回答：

1. `bit`、`color`、`pair`、`maybe_color` 分别主要由和类型还是积类型构成？各自有几个构造器？
2. 分别计算 `bit`、`pair`、`maybe_color`、`bit * color` 的取值个数。
3. 如果再加 `type triple = Triple of bit * bit * bit`，`triple` 有多少个值？
4. （进阶）`bit -> color` 有多少个不同的函数？说明为什么是指数。

## E13-2 · 记录类型

要求：

1. 定义记录类型 `student`，字段为 `name : string` 和 `score : int`；
2. 构造 `alice = { name = "Alice"; score = 58 }`；
3. 写 `is_pass : student -> bool`，分数 `>= 60` 为通过；计算 `is_pass alice`；
4. 写 `curve : student -> student`，用 `{ s with score = s.score + 5 }` 给分数加 5；计算 `is_pass (curve alice)`；
5. 写 `name_of : student -> string`，用记录模式 `{ name; _ }` 取出名字；计算 `name_of alice`；
6. 说明记录比元组更适合表示学生的两个理由。

## E13-3 · 扩展表达式树

已知第 13 课的 `expr`：

```ocaml
type expr =
  | Num of int
  | Add of expr * expr
  | Mul of expr * expr;;
```

现在增加减法和取负：

```ocaml
type expr =
  | Num of int
  | Add of expr * expr
  | Mul of expr * expr
  | Sub of expr * expr
  | Neg of expr;;
```

要求：

1. 写出新的 `eval : expr -> int`；
2. 计算 `eval (Sub (Num 10, Neg (Num 3)))`；
3. 如果给 `expr` 加了 `Sub` 构造器，却忘记在 `eval` 里写 `Sub` 分支，编译器会给什么警告？运行到该分支会怎样？
4. 为什么“加构造器”通常要改所有处理 `expr` 的函数，而“加一个新操作（如 `simplify`）”不需要改动已有的 `eval`？
5. 写出 `simplify : expr -> expr` 的一个简单版本：把 `Add (Num 0, e)` 和 `Mul (Num 1, e)` 化简为 `e`，其余情况原样返回。（提示：需要递归处理子表达式。）

## E13-4 · 概念题

回答：

1. 为什么这类类型叫“代数”数据类型？请用和类型、积类型和取值个数解释。
2. 什么是引入（introduction）和消去（elimination）？构造器和模式匹配分别属于哪一种？
3. 为什么编译器能检查 ADT 的模式匹配是否穷尽，却不能检查 `if n = 0 ... else if n = 1 ...` 是否覆盖了所有整数？
4. 代数数据类型（algebraic data type）和抽象数据类型（abstract data type）有什么区别？各举一个本课程中的例子。
5. ADT 的“封闭世界”在扩展时有什么代价？这和 OOP 的类继承相比如何？

---

## 用户作答（2026-11-16，原文保留）

E13-1: bit、color,maybe_color是和类型，pair是ADT，既有积类型也有和类型,2. bit:2 color:3 pair: 2*2 maybe_color:4 . 3. 8 4.不知道为什么是指数，我的想法bit -> color.bit有两种可能，color有三种可能2*3是6种可能 

E13-2: type student = { name : string; score : int};; let alice = {name = "Alice"; score = 58};; let is_pass { _; score} = if score >= 60 then true else false;; false; let curve s = { s with score = s.score + 5};; true;; 5. let name_of { name; _ } = name;; Alice 6.可以用字段明确表明含义，name是名字,score是分数，不容易歧义。 

E13-3:let rec eval exp = 
  match exp with
  | Num n -> n 
  | Neg exp -> - (eval exp)
  | Add (expl, expr) -> (eval expl) + (eval expr)
  | Mul (expl, expr) -> (eval expl) * (eval expr)
  | Sub (expl, expr) -> (eval expl) - (eval expr);; 2.13 3.匹配不穷尽,会抛出异常Match_failure.4.因为构造器的消去靠的是模式匹配，如果新增一种构造器，在模式匹配中需要增加这一种构造器的情况，否则会有匹配不穷尽的warning，并且在解释器执行到这种情况时会发生异常,加一个新操作不会新增构造器，只需要新写一个函数处理就行了.新的操作不会影响原有的函数,因为数据结构没有发生变化， 5.let rec simplify exp = 
  match exp with
  | Num n -> Num n
  | Neg exp -> Neg exp
  | Add (expl, expr) -> if (simplify expl) = Num 0 && (simplify expr) = Num 0 then Num 0 else if (simplify expl) = Num 0 then simplify expr else if (simplify expr) = Num 0 then simplify expl else Add (simplify expl) (simplify expr)
  | Mul (expl, expr) -> if (simplify expl) = Num 1 && (simplify expr) = Num 1 then Num 1 else if (simplify expl) = Num 1 then simplify expr else if (simplify expr) = Num 1 then simplify expl else Mul (simplify expl) (simplify expr)
  | Sub (expl, expr) -> Sub (simplify expl) (simplify expr);; 

E13-4:因为这类类型从取值个数来说, type color = red | blue;; 类型的所有的值是类型定义中构造器的和。 如果type的带上数据的话，比如type color = red of float * float * float | blue of float * float * float.color 构造器的值的范围就变成了float值的范围 * float值的范围 * float值的范围.是所有范围的乘积，color的类型的取值范围就变成了积类型的和，所以是这种类型叫做代数数据类型。2.不知道什么是引入和消去，构造器是引入，模式匹配是消去2.因为ADT的类型的值是有限的，int这种类型的值是无限的 4.关注的点不一样，代数数据类型关注的是数据的构成，数据的形状，抽象数据类型是用接口隔离数据的表现形式，tree这种就是代数数据类型，Stack这种就是抽象数据结构，接口指定了数据的操作行为，隐藏了内部实现。拓展时，比如给某种类型新增一个构造器，那么要修改所有对这种类型进行模式匹配的函数，需要增加新的一种构造器情况，OOP新增成员不需要改变原来存在的函数，如果要对数据结构新增一种操作的话，ADT不需要改变原有的代码，直接新写函数对数据进行操作就行，但是OOP需要给所有子类都加上新的函数。

## 批改与反馈（2026-11-16）

首次评分：

- E13-1：60/100
- E13-2：80/100
- E13-3：85/100
- E13-4：78/100
- E13 总评：76/100（四题平均）

### E13-1

- 第 1 问：`bit`、`color`、`maybe_color` 判为和类型正确；`pair` 的说明不够准确。`pair` 外层只有一个构造器 `Pair`，所以和的部分是“平凡的和”（只有一种情况）；主要内容是积类型 `bit * bit`。另外题目要求写出构造器个数，答案漏了：`bit` 2 个、`color` 3 个、`pair` 1 个、`maybe_color` 2 个。
- 第 2 问：`bit = 2`、`color = 3`、`pair = 4`、`maybe_color = 1 + 3 = 4` 都正确；漏了 `bit * color = 2 × 3 = 6`。
- 第 3 问：`triple = 2 × 2 × 2 = 8`，正确。
- 第 4 问：错误。函数类型的值个数是 `|B|^|A|`，不是 `|A| × |B|`。`bit -> color` 要为两个输入各选一个输出：`3 × 3 = 3^2 = 9`。你算的 `2 × 3 = 6` 是 `bit * color` 的取值个数，不是函数个数。
- 建议重做：第 1、2、4 问。

### E13-2

- 第 1、2 问：类型和 `alice` 都正确。
- 第 3 问：思路正确（58 < 60，所以 `false`），但记录模式写成了 `{ _; score }`，OCaml 报 `Syntax error`；通配符 `_` 要放在最后，写成 `{ score; _ }`。另外 `if score >= 60 then true else false` 可以直接写成 `score >= 60`，因为比较结果已经是 `bool`。
- 第 4 问：`curve` 正确；`curve alice` 的分数是 63，所以 `is_pass (curve alice) = true`，正确。
- 第 5 问：`name_of { name; _ }` 正确，结果为 `"Alice"`。
- 第 6 问：只答出了一个理由（字段名自文档化、减少歧义），题目要求两个。可补：记录按字段名访问、与位置无关，不会像元组那样因顺序或位置弄错；记录类型是独立的 `student` 类型，比裸 `string * int` 更能表达意图并参与类型检查。
- 建议订正：第 3 问的语法，第 6 问补第二个理由。

### E13-3

- 第 1 问：`eval` 的五个分支完整、正确；`Neg`、`Add`、`Mul`、`Sub` 的递归求值都对。
- 第 2 问：`eval (Sub (Num 10, Neg (Num 3))) = 13`，正确。
- 第 3 问：结论正确（匹配不穷尽；运行到漏掉的构造器会抛 `Match_failure`）。更准确地说，编译器给出的是 **Warning 8 [partial-match]**，默认仍可编译。
- 第 4 问：正确。新增构造器会改变“消去”时要覆盖的情况集合，所以所有模式匹配都要补分支；新增操作只是新增函数，不改已有数据结构和已有函数。
- 第 5 问：需要重写，有三个问题：
  1. `Add (simplify expl) (simplify expr)` 是语法错误。OCaml 的构造器不是柯里化的；`Add` 接收一个 `expr * expr` 元组，应写 `Add (simplify expl, simplify expr)`。`Mul`、`Sub` 同理。
  2. `| Neg exp -> Neg exp` 没有递归化简子表达式；应写成 `Neg (simplify exp)`，否则 `Neg (Add (Num 0, Num 5))` 化简不进去。
  3. 你额外化简了 `Add (e, Num 0)`、`Mul (e, Num 1)`、两边同时为 0/1 等情况；这些在语义上没错，但题目只要求左侧 `Add (Num 0, e)` 和 `Mul (Num 1, e)`。另外不要反复调用 `simplify expl`/`simplify expr`，可先用 `let l = simplify expl and r = simplify expr in ...` 绑定结果。
- 建议重做第 5 问。

### E13-4

- 第 1 问：核心正确：和类型按构造器个数相加，带数据的构造器内部是积类型、取值个数相乘，所以整体是“积的和”（sum of products）。注意 OCaml 构造器必须大写，例如 `Red`、`Blue`，不能写 `red`、`blue`。
- 第 2 问：构造器是引入、模式匹配是消去，这个对应关系答对了；但“不知道定义”说明概念还没掌握。引入（introduction）是构造该类型值的规则，例如 `Some 3`、`Node (1, Leaf, Leaf)`、记录字面量；消去（elimination）是消费/观察该类型值的规则，例如 `match`、记录模式、字段投影。
- 第 3 问：错误。ADT 不一定是有限类型：`list`、`tree` 都有无限多个值。编译器能检查穷尽性，是因为 ADT 是**封闭世界**：类型定义处已经列出了所有构造器；它不需要值的个数有限。`int` 不是由一组构造器定义的封闭类型，`if n = 0 ... else if n = 1 ...` 用的是布尔条件，编译器不会做算术定理证明来判断这些条件是否覆盖所有整数。
- 第 4 问：正确。代数数据类型关注数据如何构成（如 `tree`），抽象数据类型关注接口和隐藏表示（如 `Stack`，也可举 `Tally`、`Queue`）。
- 第 5 问：基本正确。建议把“OOP 新增成员”说清楚：新增**子类**通常不改已有类；新增**方法**则要改基类/接口和所有子类（除非有默认实现）。ADT 加构造器要改所有模式匹配，加操作只需新增函数，这正是表达式问题的两个方向。
- 建议重做：第 2、3 问。

---

## 订正作答（2026-11-16，原文保留）

E13-1: bit 是和类型两个构造器，color是和类型，三个构造器，pair是积类型，一个构造器最外层是平凡的和，maybe_color是和类型，两个构造器，2.bit 2 pair 4 maybe_color 4 bit * color的取值个数是2*3 = 6 3. bit -> color有3^2，九个不同函数，一种bit可以映射到3个color值，有两种类型的bit，每一种都可以映射到3个值,条件之间相互独立，所以是3*3=9.Zero -> Red One-> Red 、Zero -> Red One -> Green、Zero -> Red One -> Bule、...一共是九种函数. 

E13-2: let is_pass {score,_} = score >= 60. false. 4.记录字段带名字可以明确表示字段含义，记录结构字段的顺序是无关的.元组还需要记住顺序.

E13-3:let rec simplify exp = 
  match exp with
  | Num n -> Num n
  | Neg exp -> Neg (simplify exp)
  | Add (expl, expr) -> let l = simplify exp1 and r = simplify expr in if l = Num 0 && r = Num 0 then Num 0 else if l = Num 0 then simplify expr else if r = Num 0 then simplify expl else Add (l, r)
  | Mul (expl, expr) -> let l = simplify exp1 and r = simplify expr in if l = Num 1 && r = Num 1 then Num 1 else if l = Num 1 then simplify expr else if r = Num 1 then simplify expl else Mul (l, r)
  | Sub (expl, expr) -> Sub (simplify expl , simplify expr);; 

E13-4:引入是构造该类型值的规则，消去是分解该类型值的规则，构造器是引入，模式匹配是消去. 因为ADT是封闭世界，类型定义处已经列出了所有的构造器，编译器可以看是否覆盖了所有构造器，但是整数不是一组构造器定义的封闭类型，编译器不会做算数定理证明，来判断条件是否覆盖了所有正数。

## 订正批改（2026-11-16）

订正后评分：

- E13-1：100/100
- E13-2：90/100
- E13-3：95/100
- E13-4：95/100
- E13 总评：95/100

### E13-1

第 1 问现在完整正确：`bit` 2 个构造器、`color` 3 个、`pair` 1 个且外层是平凡和、`maybe_color` 2 个。第 2 问 `bit = 2`、`pair = 4`、`maybe_color = 4`、`bit * color = 6` 全对。第 3 问 `bit -> color = 3^2 = 9`，并正确说明两个输入各自独立选择输出，枚举也正确。

小笔误：`Bule` 应为 `Blue`；“有两种类型的 bit”更准确地说“`bit` 有两个输入值（`Zero`、`One`）”。不扣分。

### E13-2

第 3 问的思路和结果正确（58 < 60，所以 `false`），但记录模式仍然写成了：

```ocaml
let is_pass {score,_} = score >= 60;;
```

OCaml 记录模式的字段分隔符是分号，不是逗号；会报：

```text
Error: Syntax error: "}" expected
```

应写成：

```ocaml
let is_pass { score; _ } = score >= 60;;
```

另外，代码中的 `60` 不要写成 `60.`；`60.` 是 `float`，而 `score` 是 `int`。如果句末的点只是标点，可以忽略。

第 6 问现在给出了两个理由：字段名表达含义、记录字段顺序无关而元组要记顺序。正确。

### E13-3

第 5 问的结构已经正确：构造器改用元组写法，`Neg` 也递归了，`l`/`r` 也绑定了。但代码里有一个变量名笔误：

```ocaml
let l = simplify exp1 and r = simplify expr in ...
```

`exp1`（数字 1）应为 `expl`（字母 l），否则会报：

```text
Error: Unbound value "exp1"
```

把两处 `exp1` 改成 `expl` 后即可编译。实际运行验证：

```ocaml
simplify (Add (Num 0, Add (Num 0, Num 5))) = Num 5
simplify (Neg (Add (Num 0, Num 5))) = Neg (Num 5)
simplify (Mul (Num 1, Num 7)) = Num 7
```

另外，在 `if l = Num 0 then ...` 分支里用 `r`（已化简的右子表达式）比再次调用 `simplify expr` 更直接，也避免重复计算；这不是语义错误。

### E13-4

第 2 问现在正确：引入是构造该类型值的规则，消去是分解/消费该类型值的规则；构造器是引入，模式匹配是消去。

第 3 问也正确：ADT 是封闭世界，所有构造器在类型定义处列完，编译器检查模式是否覆盖这些构造器；`int` 不是构造器封闭类型，编译器也不会做算术定理证明。小笔误：最后应说“所有整数”，不是“所有正数”。

第 1 问的“构造器必须大写”仍是代码书写注意点；概念解释本身正确。

### 下一步

只剩两个代码笔误：

1. E13-2 第 3 问：`{score,_}` → `{ score; _ }`；
2. E13-3 第 5 问：两处 `exp1` → `expl`。

改掉后 E13 可按满分计。之后回到 E12-1 至 E12-3。

---

## 最终订正（2026-11-16）

用户提交：

```ocaml
let rec simplify exp = 
  match exp with
  | Num n -> Num n
  | Neg exp -> Neg (simplify exp)
  | Add (expl, expr) -> let l = simplify expl and r = simplify expr in if l = Num 0 && r = Num 0 then Num 0 else if l = Num 0 then simplify expr else if r = Num 0 then simplify expl else Add (l, r)
  | Mul (expl, expr) -> let l = simplify expl and r = simplify expr in if l = Num 1 && r = Num 1 then Num 1 else if l = Num 1 then simplify expr else if r = Num 1 then simplify expl else Mul (l, r)
  | Sub (expl, expr) -> Sub (simplify expl , simplify expr);;

let is_pass { score; _ } = score >= 60;;
```

批改：

- `simplify` 已通过类型检查，实际运行：
  - `simplify (Add (Num 0, Add (Num 0, Num 5))) = Num 5`
  - `simplify (Neg (Add (Num 0, Num 5))) = Neg (Num 5)`
  - `simplify (Mul (Num 1, Num 7)) = Num 7`
  - `simplify (Add (Num 3, Num 4)) = Add (Num 3, Num 4)`
- `is_pass { score; _ }` 已通过类型检查；`is_pass alice = false`，`is_pass { name = "Bob"; score = 60 } = true`。
- 分支里用 `simplify expr`/`simplify expl` 而不是已绑定的 `r`/`l`，结果正确，只是会重复计算；可改成 `then r` / `then l`，但不是必须。
- E13-2、E13-3 订正通过；E13 总评按 100/100 计。E13-4 的“正数”应为“整数”，属于笔误，不扣实质分。
- 下一步：回到 E12-1 至 E12-3。
