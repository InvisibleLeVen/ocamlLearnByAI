# 第 12 课练习：模块、签名与抽象类型

布置日期：2026-11-13
状态：已批改（2026-11-17）；E12-1 第 3 步已于 2026-11-19 订正，补一句理由后可满分

本组为自编练习。先思考，再运行验证；回答时写出类型、值和理由。

## E12-1 · 定义模块与签名

要求：

1. 定义模块 `Math`，提供 `square : int -> int` 和 `cube : int -> int`；
2. 定义签名 `MATH`，只列出这两个值；
3. 用 `MATH` 约束 `Math`；
4. 计算 `Math.square 4` 和 `Math.cube 2`；
5. 说明 `Math.square` 的类型，以及为什么用 `Math.` 前缀访问。

## E12-2 · 抽象类型

已知：

```ocaml
module type TALLY = sig
  type t
  val zero : t
  val add : t -> t -> t
  val to_int : t -> int
end;;

module Tally : TALLY = struct
  type t = int
  let zero = 0
  let add a b = a + b
  let to_int x = x
  let double x = x * 2
end;;
```

回答：

1. `Tally.zero` 的类型是什么？顶层会显示成什么？
2. `Tally.add Tally.zero Tally.zero` 的类型和显示值是什么？
3. `Tally.add 1 2` 会发生什么？为什么？
4. `Tally.double 3` 会发生什么？为什么？
5. 怎样得到 `Tally.zero` 对应的 int 值？

## E12-3 · 队列与信息隐藏

已知：

```ocaml
module type QUEUE = sig
  type 'a t
  val empty : 'a t
  val enqueue : 'a -> 'a t -> 'a t
  val dequeue : 'a t -> ('a * 'a t) option
end;;

module Queue : QUEUE = struct
  type 'a t = 'a list
  let empty = []
  let enqueue x q = q @ [x]
  let dequeue q =
    match q with
    | [] -> None
    | h :: t -> Some (h, t)
end;;
```

回答：

1. 计算 `Queue.dequeue (Queue.enqueue 1 (Queue.enqueue 2 Queue.empty))` 的值；
2. 结果中的 `<abstr>` 表示什么？签名隐藏了什么？
3. `Queue.enqueue 1 [2; 3]` 能否通过类型检查？为什么？
4. 如果以后把 `type 'a t` 的实现从列表改成记录，客户端调用 `enqueue` / `dequeue` 的代码需要改吗？说明签名在其中的作用。

## 用户作答（2026-11-17，原文）

E12-1:

```ocaml
module Math = struct
  let square x = x * x
  let cube x = x * (square x)
end;;
```

```ocaml
module type MATH = sig
  val square : int -> int
  val cube : int -> int
end;;
```

```ocaml
module Math : Math = struct
  let square x = x * x
  let cube x = x * (square x)
end;;
```

4. `16`、`8`。
5. `int -> int`；因为 `square` 定义在模块 `Math` 里面，访问模块里的内容就要用模块名 `.`。

E12-2:

1. `Tally.t` 显示成 `- : Tally.t = <abstr>`。
2. 类型是 `Tally.t`，显示值是 `<abstr>`。
3. 会报错，`Tally.add` 期望一个 `Tally.t` 的类型，但是实际是 `int`。
4. 会报错，因为 `double` 没有被 `TALLY` 所暴露出来，被隐藏了。`Error: Unbound value Tally.double`。
5. `Tally.to_int Tally.zero`。

E12-3:

1. `Some (2, <abstr>)`。
2. 表示的是被签名隐藏的抽象数据，隐藏了 `int list`。
3. 不能，因为 `enqueue` 期望的参数类型是 `int -> int Queue.t`，而实际参数是 `int -> int list`。
4. 调用 `enqueue` / `dequeue` 的代码不用改，因为 `enqueue` 的类型 `'a -> 'a t -> 'a t` 和 `dequeue` 的类型 `'a t -> ('a * 'a t) option`，参数和返回值都不依赖 `t` 的实际类型，`t` 是抽象类型，换成什么都不影响这两个函数的上下文。

## 批改反馈（2026-11-17）

评分：E12-1 80/100；E12-2 100/100；E12-3 90/100；E12 总评 90/100（三题等权）。

### E12-1（80/100）

第 1、2、4、5 步正确。第 3 步有实际类型错误：

```ocaml
module Math : Math = struct
```

实际运行报：

```text
Error: Unbound module type "Math"
Hint: There is a module named "Math", but modules are not module types
```

冒号右边要求的是**模块类型**（module type），这里应写签名名 `MATH`，不是模块名 `Math`。OCaml 区分大小写，`Math` 与 `MATH` 是两个不同的名字：

```ocaml
module Math : MATH = struct
  let square x = x * x
  let cube x = x * (square x)
end;;
```

第 4 步的 `16`、`8` 本身正确；第 5 步“通过模块名访问”的理由也正确。但由于第 3 步失败，模块实际上没有被 `MATH` 约束，这一步的“用签名约束模块”尚未完成。只需改这一行后重交。

### E12-2（100/100）

五问均正确：

1. `Tally.zero : Tally.t = <abstr>`，`t` 的表示被隐藏；
2. `Tally.add Tally.zero Tally.zero : Tally.t = <abstr>`；
3. `Tally.add 1 2` 报类型错误，`1 : int` 与期望的 `Tally.t` 不匹配；
4. `Tally.double` 不在 `TALLY` 中，报 `Unbound value`；
5. 用公开的 `Tally.to_int` 得到 `int`，实际为 `0`。

### E12-3（90/100）

第 1、2、4 问正确。第 1 问实际运行结果为：

```text
- : (int * int Queue.t) option = Some (2, <abstr>)
```

第 3 问结论“不能通过类型检查”正确，但理由的类型写法不准确。`Queue.enqueue` 的完整类型是：

```ocaml
'a -> 'a t -> 'a t
```

代入 `1 : int` 后：

```ocaml
Queue.enqueue 1 : int Queue.t -> int Queue.t
```

它期望第二个参数是 `int Queue.t`，而 `[2; 3]` 是 `int list`。实际错误为：

```text
This expression has type "'a list"
but an expression was expected of type "int Queue.t"
```

所以应说“`Queue.enqueue 1` 的类型是 `int Queue.t -> int Queue.t`”，而不是“`enqueue` 期望 `int -> int Queue.t`”；`[2; 3]` 也不是函数 `int -> int list`。关键点是：签名把 `'a t` 隐藏成抽象类型，客户端不能把内部的 `'a list` 当作 `'a Queue.t` 使用。

第 4 问正确：客户端只依赖签名中的 `empty`、`enqueue`、`dequeue` 及其类型；实现从列表改成记录，只要仍满足 `QUEUE`，调用代码不需要改。这就是表示独立性。

## E12-1 第 3 步订正（2026-11-19，原文）

```ocaml
module Math : MATH = struct
  let square x = x * x
  let cube x = x * (square x)
end;;
```

### 批改反馈（2026-11-19）

代码正确：冒号右边写的是签名名 `MATH`，模块定义可以通过。

仍缺要求的一句话：`Math` 是模块名，`MATH` 是模块类型（签名）名；`module M : S = ...` 的冒号右边要求的是模块类型，所以不能写模块名 `Math`。补上这句后可记 100/100。

E12-1 订正后暂计 95/100；E12 总评暂计 95/100（E12-1 95、E12-2 100、E12-3 90）。

补充理由（2026-11-19，原文）：“冒号右边写MATH，因为右边期望是签名而不是Module.”

### 第二轮反馈（2026-11-19）

理由抓住要点，E12-1 记 100/100。更精确的表述是：冒号右边要求的是**模块类型名**（module type name），`MATH` 是模块类型，`Math` 是模块；OCaml 区分大小写，二者是两个名字。E12 总评 97/100（E12-1 100、E12-2 100、E12-3 90）。
