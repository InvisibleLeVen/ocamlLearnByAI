# 第 14 课练习：函子（Functors）

布置日期：2026-11-17
状态：待作答

本组为自编练习。先思考，再运行验证；回答时写出类型、值、错误位置和理由。

说明：本组代码中直接定义的 `IntSum`、`IntMul`、`StrCat` 等模块没有额外的签名约束，因此它们的 `type t` 等式（如 `IntSum.t = int`）对外是透明的；只有题目明确写 `: MONOID` / `: STACK` 的地方才会隐藏类型表示。

作答前先完成第 12 课的遗留订正：

> E12-1 第 3 步：把 `module Math : Math = ...` 改成正确写法，并用一句话说明为什么冒号右边应写 `MATH` 而不是 `Math`。

## E14-1 · 函子基本用法

已知：

```ocaml
module type MONOID = sig
  type t
  val zero : t
  val add : t -> t -> t
end

module IntSum = struct
  type t = int
  let zero = 0
  let add a b = a + b
end

module IntMul = struct
  type t = int
  let zero = 1
  let add a b = a * b
end

module StrCat = struct
  type t = string
  let zero = ""
  let add a b = a ^ b
end

module Repeat (M : MONOID) = struct
  let rec times n x =
    if n = 0 then M.zero
    else M.add x (times (n - 1) x)
end
```

回答：

1. 定义 `module P = Repeat (IntMul);;` 后，`P.times` 的完整类型是什么？顶层会显示成什么？
2. `P.times 3 5` 的值是多少？写出展开过程。
3. 定义 `module S = Repeat (StrCat);;` 后，`S.times` 的类型和 `S.times 3 "-"` 的值分别是什么？
4. 把 `IntSum` 的 `let add a b = a + b` 改成 `let add a b = a - b`，`Repeat` 的代码需要改吗？`module R = Repeat (IntSum)` 还能通过类型检查吗？`R.times 3 5` 会得到什么？
5. 第 4 问中改过的 `IntSum` 仍然满足 `MONOID` 签名，但它还是数学意义上的“单位元 + 加法”吗？这说明签名检查的是什么、不检查什么？

## E14-2 · 用函子生成栈模块

已知：

```ocaml
module type ELEM = sig
  type t
  val eq : t -> t -> bool
end

module MakeStack (E : ELEM) = struct
  type elt = E.t
  type t = elt list

  let empty : t = []
  let push (x : elt) (s : t) : t = x :: s

  let pop (s : t) : (elt * t) option =
    match s with
    | [] -> None
    | h :: tl -> Some (h, tl)

  let rec contains (x : elt) (s : t) : bool =
    match s with
    | [] -> false
    | h :: tl -> E.eq x h || contains x tl
end

module StringStack = MakeStack (struct
  type t = string
  let eq a b = a = b
end)
```

回答：

1. `StringStack.push "a" StringStack.empty` 的完整类型和值是什么？
2. `StringStack.t` 的表示对外公开吗？说明原因（提示：看 `MakeStack` 有没有结果签名）。
3. 若写成 `module HiddenStack : STACK with type elt = string = MakeStack (...)`，其中 `STACK` 是：

   ```ocaml
   module type STACK = sig
     type elt
     type t
     val empty : t
     val push : elt -> t -> t
     val pop : t -> (elt * t) option
     val contains : elt -> t -> bool
   end
   ```

   那么 `HiddenStack.empty` 在顶层显示成什么？`HiddenStack.push "a" []` 能否通过类型检查？为什么？

   注意：这里的 `: STACK with type elt = string` 是**使用处的模块签名约束**，不是函子定义里的结果签名。若改成在 `MakeStack` 的定义上写 `module MakeStack (E : ELEM) : STACK with type elt = E.t = struct ... end`，效果和作用范围有什么不同？
4. 若把 `let empty : t = []` 改成 `let empty = []`，`StringStack.empty` 在顶层会显示成什么类型？`StringStack.push "a" StringStack.empty` 还能通过类型检查吗？

## E14-3 · 概念题

回答：

1. 函子作用在什么层面？它和 `map`、`apply_twice` 这类高阶函数有什么根本区别？
2. `module P = Repeat (IntMul)` 在什么阶段完成？程序运行时还能把同一个 `P.times` 改成“字符串拼接”的行为吗？如果不能，应怎么做？
3. 参数签名 `(M : MONOID)` 起什么作用？如果 `IntMul` 少实现了 `add`，错误会发生在“定义 `Repeat` 时”还是“应用 `Repeat (IntMul)` 时”？说明理由。
4. `module StringMap = Map.Make (String);;` 产生的 `StringMap.t`，其表示对外公开吗？`StringMap.bindings` 返回什么类型？签名在其中起了什么作用？

## 复习题 · R22（不计入 E14 总分，单独更新复习状态）

已知第 12 课的栈签名与实现：

```ocaml
module type STACK = sig
  type 'a t
  val empty : 'a t
  val push : 'a -> 'a t -> 'a t
  val pop : 'a t -> ('a * 'a t) option
end

module Stack : STACK = struct
  type 'a t = 'a list
  let empty = []
  let push x s = x :: s
  let pop s =
    match s with
    | [] -> None
    | h :: t -> Some (h, t)
end
```

回答：

1. `Stack.push 1` 的类型是什么？
2. `Stack.push 1 [2; 3]` 能否通过类型检查？如果不能，写出两边的类型（左边实际是什么，右边期望什么）。

## 用户作答（2026-11-19，原文）

E12-1 第 3 步订正：

```ocaml
module Math : MATH = struct
  let square x = x * x
  let cube x = x * (square x)
end;;
```

E14-1:

1. `int -> IntMul.t -> IntMul.t`，`IntMul.t` 是 int 类型的所以是 `int -> int -> int`，顶层显示 `int -> int -> int`。
2. 15。`times 3 5 -> M.add 5 (times 2 5) -> M.add 5 (M.add 5 (times 1 5)) -> M.add 5 (M.add 5 (M.add 5 (times 0 5))) -> M.add 5 (M.add 5 (M.add 5 0)) -> M.add 5 (M.add 5 5) -> M.add 5 10 -> 15`。
3. `int -> string -> string`，值是 `"---"`。
4. 不用改；可以通过类型检查；因为 `add` 内部改成 `-` 也满足 `MONOID` 签名约束 `t -> t -> t`；`R.times 3 5` 得 5。
5. 满足 `MONOID` 签名，不是数学意义上的“单位元 + 加法”；签名只检查类型，不检查语义。

E14-2:

1. `string list`；`["a"]`。
2. 对外公开的，因为没有结果签名，从 body 推断返回模块的接口 `type t = elt list`，`elt = E.t`；`E.t` 是 string 也没有被签名隐藏。
3. 显示 `<expr>` 抽象类型；不能通过类型检查，因为第二个参数是抽象类型 `t`，但实际参数是一个 `'a list`。使用处的约束只影响使用处这个返回的模块，在函子定义处约束对调用函子返回的所有模块都会有影响。
4. `'a list`；可以通过类型检查；本来 `StringStack.push "a" StringStack.empty` 就可以通过类型检查，`let empty : t = []` 也可以通过类型检查，因为 `StringStack` 的 `t` 是 string，是暴露出来的，没有被签名限制。

E14-3:

1. 函子作用在模块层面；根本区别是函子接收模块返回模块，这些高阶函数接受的是函数。
2. 编译期完成；不能，因为编译期已经确定了 `P` 的行为，运行时没法改变；应该重新调用一次函子传入 `StrCat`。
3. 限制函子调用的模块满足签名 `MONOID` 约束；会发生在定义 `Repeat` 时，因为函子的类型检查没有通过。
4. 对外公开；`'a StringMap.t -> (string * 'a) list`；不知道你说的签名是谁的签名，函子的签名还是 `String` 的签名。

R22:

1. `Stack.push 1` 的类型是 `int Stack.t -> int Stack.t`。
2. 不能，因为 `Stack.push` 期望的参数类型是 `'a -> 'a Stack.t -> 'a Stack.t`，`Stack.t` 是抽象类型。

## 批改反馈（2026-11-19）

首次评分：E12-1 订正 95/100；E14-1 72/100、E14-2 88/100、E14-3 63/100；E14 总评 74/100；R22 复习题 85/100（单独记录）。

### E12-1 第 3 步订正（95/100）

`module Math : MATH = struct ... end` 正确，模块名与模块类型名的用法已经改对。仍缺要求的一句话：为什么冒号右边应写 `MATH`、不能写 `Math`。补一句理由后可记 100/100。E12 暂计 95/100（E12-1 95、E12-2 100、E12-3 90）。

### E14-1（72/100）

- 第 1 问 15/20：`int -> IntMul.t -> IntMul.t` 正确；但“顶层显示 `int -> int -> int`”不对。`IntMul.t = int` 是类型等式，不等于顶层打印时会把别名展开。运行 `P.times;;` 观察实际显示。
- 第 2 问 0/20：`P = Repeat (IntMul)`，展开时却用了 `IntSum` 的 `M.zero = 0` 和 `M.add = +`。请按 `IntMul` 的 `zero` 与 `add` 重新展开。
- 第 3 问 17/20：值 `"---"` 正确；类型本质上是 `int -> string -> string`，但顶层同样会保留 `StrCat.t` 别名。
- 第 4 问 20/20：不改函子、能过类型检查、结果 5，均正确。
- 第 5 问 20/20：签名只检查类型、不检查语义，正确。

### E14-2（88/100）

- 第 1 问 22/25：值 `["a"]` 正确；顶层显示的类型是 `StringStack.t`（它等于 `string list`），写 `string list` 可接受，但不够精确。
- 第 2 问 25/25：正确。
- 第 3 问 24/25：结论与作用范围都正确；顶层显示是 `<abstr>`，不是 `<expr>`。
- 第 4 问 17/25：`'a list` 与“能通过”都正确；但理由不是“`t` 是 string 且未被签名限制”。关键在于 `empty` 去掉注解后被泛化成 `'a list`，而 `StringStack.t = string list`，`'a list` 可以实例化成 `string list`，所以 `push` 仍接受它。请改写理由。

### E14-3（63/100）

- 第 1 问 25/25：正确。
- 第 2 问 25/25：正确。
- 第 3 问 0/25：错误发生在**应用** `Repeat (IntMul)` 时。定义 `Repeat` 时，`M` 只是满足 `MONOID` 的抽象参数，body 只使用 `M.zero`、`M.add`，定义本身可以通过；实参模块缺少 `add` 只有在应用时才被检查。提示：用 `module NoAdd = struct type t = int let zero = 0 end` 再 `module B = Repeat (NoAdd)` 看错误位置。
- 第 4 问 13/25：`bindings` 的类型写对了；但 `StringMap.t` 的表示**不公开**，它是抽象类型（`StringMap.empty : 'a StringMap.t = <abstr>`，`List.length StringMap.empty` 不能通过）。这里说的签名是 `Map.Make` 的**结果签名**（约为 `Map.S with type key = String.t`），不是 `String` 的参数签名；`String` 只是满足 `Map.OrderedType` 的实参。请重新回答“表示是否公开”并说明理由。

### R22（85/100，单独记录）

- 第 1 问正确：`Stack.push 1 : int Stack.t -> int Stack.t`。这是上次 E12-3 出错的关键点，已经改对。
- 第 2 问结论正确，但没有写出题目要求的两侧类型：`[2; 3] : int list`，而期望的是 `int Stack.t`。补上即可。复习状态记为已验证（关键点），下次可再抽问。

### 需要重交的部分

1. E12-1：补一句为什么写 `MATH` 而不是 `Math`。
2. E14-1 第 1、2 问：顶层显示的实际类型；按 `IntMul` 重新展开。
3. E14-2 第 4 问：改写“为什么 push 仍能通过”的理由。
4. E14-3 第 3 问：重新判断错误发生的位置并说明。
5. E14-3 第 4 问：重新回答 `StringMap.t` 是否公开，并说明签名的作用。
6. R22 第 2 问：补写两侧类型。

## 订正作答（2026-11-19 第二轮，原文）

E12-1:冒号右边写MATH，因为右边期望是签名而不是Module.

E14-1:实际打印int -> int -> int 2.M.add 取的是IntMul模块里的,我不想重写整个过程，上面我已经写过了，我只是把IntMul看成IntAdd了 4.这一问我没明白你的问题.5.定义Repeat时编译期对M的知道仅仅是签名部分的内容,实参少实现add这件事在定义看不出来，Error: Modules do not match: sig type t = int val zero : t end is not included in MONOID The value add is required but not provided 出现在调用函子的地方. 6.'a StringMap.t = <abstr>. 是抽象类型.List.length期望一个'a list.Map.Make 的结果签名里，key 被共享成了 String.t，那 t 有没有被写出来？这里的t被写出来是什么意思.哪个t被写出来Map有一个t,String也有一个t. 7.期望的参数类型是 'a -> 'a Stack.t .实际类型是 int -> int list

（编号说明：上述 4、5、6、7 分别对应 E14-2 第 4 问、E14-3 第 3 问、E14-3 第 4 问、R22 第 2 问。）

## 第二轮批改（2026-11-19）

本轮分数：E12-1 100/100（E12 总评 97/100）；E14-1 维持 72/100；E14-2 维持 88/100；E14-3 95/100；E14 总评 85/100；R22 本轮 50/100。

### E12-1（100/100）

“右边期望是签名而不是模块”抓住了要点，可以作为要求的那一句话。更精确的说法：`module M : S = ...` 冒号右边要求的是**模块类型名**；`MATH` 是模块类型，`Math` 是同名的模块，二者在命名空间上不同。E12 总评 97/100（100、100、90）。

### E14-1（维持 72/100）

- 第 1 问仍未解决：实际运行（OCaml 5.2.1）输出为

  ```text
  module P : sig val times : int -> IntMul.t -> IntMul.t end
  # P.times;;
  - : int -> IntMul.t -> IntMul.t = <fun>
  ```

  已把转录发给用户，待其据此修正“顶层显示 `int -> int -> int`”的判断。
- 第 2 问：用户承认是把 `IntMul` 看成 `IntSum`，陈述“`M.add` 取的是 `IntMul` 模块里的”，但未给出正确值。待补一行结果。

### E14-2（维持 88/100）

第 4 问回“没明白问题”，未作答；已用更具体的问法重述（删掉注解后 `[]` 的类型、`push` 参数的要求、二者如何匹配），待重答。

### E14-3（95/100）

- 第 3 问（25/25）：正确——定义 `Repeat` 时只看得到签名部分，实参缺 `add` 在应用处报 `Modules do not match ... The value "add" is required but not provided`。
- 第 4 问（20/25）：`StringMap.empty : 'a StringMap.t = <abstr>`、抽象类型、`List.length` 期望 `'a list`，结论与证据正确；但未说明结果签名的作用，并追问“哪个 `t`”。已澄清两个 `t` 的区别，待补一句结果签名的说明。

### R22（本轮 50/100）

第 1 问维持正确。第 2 问写成“期望的参数类型是 `'a -> 'a Stack.t`，实际类型是 `int -> int list`”，与 E12-3 是同一类错误：

- `Stack.push 1` 的类型是 `int Stack.t -> int Stack.t`，它剩下的参数要求 `int Stack.t`；
- `[2; 3]` 是 `int list`，不是函数 `int -> int list`。

待按“先看部分应用后的类型，再看实参自身的类型”重答。

## 第三轮作答（2026-11-19，原文）

用户粘贴 utop 5.2.1 实际会话：

```text
utop # module P = Repeat (IntMul);;
module P : sig val times : int -> int -> int end
utop # P.times;;
- : int -> int -> int = <fun>
```

1. “仔细看清楚了，这是我在utop下运行的log，`P.times` 的显示就是 `int -> int -> int`。125”
2. （E14-2 第 4 问）“把上下文代码也带上，我懒得往上翻了”
3. （E14-3 第 4 问）“结果签名把key类型共享出去了”
4. （R22 第 2 问）“也是把全部代码都带上，我往回翻很累的，以后这种需要我重新回答的题目都麻烦把上下文代码带上。”

## 第三轮批改（2026-11-19）

本轮分数：E14-1 订正为 **100/100**；E14-3 订正为 **100/100**；E14-2 维持 88/100；E14 总评 **96/100**。

### 教师更正：顶层打印与 `-short-paths`

上一轮教师断言“顶层显示保留 `int -> IntMul.t -> IntMul.t`”，并据此扣分，这是**错的**。实际核对：

- `ocaml`（默认，不带 `-short-paths`）：`- : int -> IntMul.t -> IntMul.t = <fun>`
- `ocaml -short-paths` 与 **utop**（`-short-paths` 是 utop 的默认值）：`- : int -> int -> int = <fun>`
- `S.times` 同理：utop / `-short-paths` 下为 `int -> string -> string`。

用户在 utop 下的观察完全正确，E14-1 第 1、3 问按正确计分。`IntMul.t = int` 是类型等式，两种显示指的是同一个类型；差别只在前端打印器是否把别名展开。

### E14-1（100/100）

- 第 1 问 20/20：utop 下 `int -> int -> int` 正确。
- 第 2 问 20/20：125 正确。
- 第 3 问 20/20：`int -> string -> string` 正确。
- 第 4、5 问 20/20 + 20/20（前轮已通过）。

### E14-2（维持 88/100）

第 4 问尚未作答：用户要求重发时带上完整上下文代码，已照办。

### E14-3（100/100）

第 4 问（25/25）：“结果签名把 key 类型共享出去了”正确；配合前轮已答出的 `'a StringMap.t = <abstr>` 与 `List.length` 期望 `'a list`，该问通过。

### R22（第 2 问待重答）

用户要求带上完整代码后重答，已连同 `STACK` / `Stack` 定义与调用表达式一并发出。

### 已记录的用户要求

以后凡需要用户重答或订正的题目，必须在提问处**附上完整上下文代码**，不让用户回翻历史。

## 第四轮作答（2026-11-19，原文）

E14-2 第 4 问：

> 1. 'a list 2.能通过, empty的类型是'a list push的参数要求是elt -> t elt是E.t 是string类型，"a"满足，t 是elt list -> string list StringStack.empty类型是 'a list 满足.

R22：

> 'a t -> 'a t 2. 要求是'a t 3. [2; 3] 是int list. 不能通过，要求是'a t 但是实际上给的是int list,第二个参数不匹配.

## 第四轮批改（2026-11-19）

本轮分数：E14-2 **95/100**；E14 总评 **98/100**；R22 本轮 **90/100**。

### E14-2（95/100）

- 第 4 问（24/25）：结论 `'a list`、能通过、理由（`t` 等于 `string list`，而 `empty` 的 `'a list` 能满足它）均正确。只差把机制点名：`empty` 去掉注解后被**泛化**成 `'a list`，用到 `push` 期望 `string list` 的位置时 `'a` 被**实例化**为 `string`。这一点在反馈中补充，不再要求重交。
- 第 1 问维持 22/25：实测 `StringStack.push "a" StringStack.empty` 在 utop（`-short-paths`）与默认 `ocaml` 下都显示 `- : StringStack.t = ["a"]`，不会展开为 `string list`。

### R22（90/100）

- 第 1、2 问（各 20/25）：写作 `'a t -> 'a t` / 要求 `'a t`。方向已对（参数位置要求的是栈类型，不再是函数类型），但漏了实例化：实参 `1 : int` 已经把 `'a` 定成 `int`，所以 `Stack.push 1 : int Stack.t -> int Stack.t`，参数要求 `int Stack.t`。
- 第 3 问（25/25）：`[2; 3] : int list` 正确。
- 第 4 问（25/25）：不能通过、第二个参数不匹配，正确。实际报错：

  ```text
  Error: This expression has type "'a list"
         but an expression was expected of type "int Stack.t"
  ```

- 说明：连续两轮卡住的关键误解（把参数位置当成 `int -> int list` 这类函数类型）本轮已消除。只剩“把 `'a` 实例化为 `int`”这一处精度问题，用一小题确认。

## R22 最后一问（2026-11-19，原文）

> int t -> int t

### 反馈

实例化正确：`1 : int` 使 `'a = int`，剩下 `int t -> int t` 中的 `t` 属于 `Stack`。utop 会显示完整路径：

```text
- : int Stack.t -> int Stack.t = <fun>
```

R22 记 95/100（只差模块前缀的写法）。模块 4 关闭。
