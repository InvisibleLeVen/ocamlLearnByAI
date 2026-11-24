# 第 9 课：高阶函数与 map

日期：2026-11-12
教材：CS3110 的高阶函数与列表抽象部分（具体小节号未核对）。
说明：以下类型和结果已用 OCaml 5.2.1 实际运行核对（2026-11-12）。

## 1. 函数本来就是值

第 4 课已经见过：OCaml 的函数可以像普通值一样绑定、传递、返回。

```ocaml
let add x y = x + y;;
add 3;;          (* int -> int，部分应用得到一个函数 *)
(add 3) 4;;      (* int = 7 *)
```

`add 3` 没有报错，而是得到一个 `int -> int` 的函数。这说明函数可以作为**返回值**，也可以作为**参数**传给别的函数。

## 2. 高阶函数：参数是函数

接收函数作为参数，或返回函数的函数，叫**高阶函数**（higher-order function）。

```ocaml
let apply_twice f x = f (f x);;
```

```ocaml
apply_twice (fun x -> x + 1) 5;;       (* int = 7 *)
apply_twice (fun s -> s ^ "!") "hi";;  (* string = "hi!!" *)
```

类型：

```ocaml
apply_twice : ('a -> 'a) -> 'a -> 'a
```

读法：

- 第一个参数 `f` 是函数，类型 `'a -> 'a`；
- 第二个参数 `x` 是 `'a`；
- 返回值也是 `'a`。

为什么 `f` 是 `'a -> 'a`，不是 `'a -> 'b`？因为 `f` 的结果要再次作为 `f` 的输入：`f (f x)`。第一次输出的类型必须能接上第二次输入的类型，所以输入、输出类型相同。

## 3. 匿名函数 fun

`fun x -> x + 1` 是一个没有名字的函数：

```ocaml
(fun x -> x * 2) 5;;   (* int = 10 *)
```

`fun` 后面是参数，`->` 后面是函数体。它和 `let f x = x + 1` 定义出的函数本质相同，只是不绑定名字，适合作为一次性的参数。

## 4. 用高阶函数抽象“对每个元素做同一件事”

先看两个具体函数：

```ocaml
let rec square_all lst =
  match lst with
  | [] -> []
  | h :: t -> (h * h) :: square_all t;;

let rec negate_all lst =
  match lst with
  | [] -> []
  | h :: t -> (-h) :: negate_all t;;
```

两者结构完全相同，只有“对 `h` 做什么”不同。把这件事抽成参数 `f`，就得到 `map`：

```ocaml
let rec map f lst =
  match lst with
  | [] -> []
  | h :: t -> f h :: map f t;;
```

```ocaml
map (fun x -> x * x) [1; 2; 3];;      (* int list = [1; 4; 9] *)
map (fun x -> x > 0) [1; -2; 3];;     (* bool list = [true; false; true] *)
```

类型：

```ocaml
map : ('a -> 'b) -> 'a list -> 'b list
```

- `f : 'a -> 'b`：输入元素类型 `'a`，输出元素类型 `'b`；
- 输入 `'a list`，输出 `'b list`；
- `'a` 和 `'b` 可以相同，也可以不同（例如把 `int` 映射成 `bool`）。

注意优先级：函数调用比 `::` 结合得更紧，所以

```ocaml
f h :: map f t
```

等价于

```ocaml
(f h) :: (map f t)
```

而不是 `f (h :: map f t)`。

求值过程：

```text
map (fun x -> x * x) [1; 2; 3]
= 1 * 1 :: map (fun x -> x * x) [2; 3]
= 1 :: 4 :: 9 :: []
= [1; 4; 9]
```

## 5. 返回函数的写法

下面两种定义得到同一个类型：

```ocaml
let add x y = x + y;;
let add' x = fun y -> x + y;;
```

两者都是 `int -> int -> int`。`add 3` 和 `add' 3` 都返回一个等待第二个参数的函数。

这和第 4 课的柯里化是同一件事，只是现在从“函数作为返回值”的角度再看一次。

## 6. option 上的 map

同样的抽象可以用于 option：

```ocaml
let map_option f o =
  match o with
  | None -> None
  | Some x -> Some (f x);;
```

```ocaml
map_option : ('a -> 'b) -> 'a option -> 'b option
map_option (fun x -> x * 2) (Some 21);;   (* int option = Some 42 *)
map_option (fun x -> x * 2) None;;        (* int option = None *)
```

`None` 里没有值，所以不需要、也不能把 `f` 应用上去；直接返回 `None`。

标准库中对应的是 `Option.map`，列表上对应的是 `List.map`。

## 7. 与 C++ / Python 对比

| 项目 | C++ / Python | OCaml |
| --- | --- | --- |
| 传递函数 | 函数指针、`std::function`、Python 函数对象 | 直接传函数 |
| 匿名函数 | lambda | `fun x -> ...` |
| 对列表逐元素处理 | `std::transform`、列表推导 | `map` |
| 返回函数 | lambda / `std::bind` | 部分应用或 `fun` |

OCaml 不需要为“函数作为值”引入特殊类型：所有函数本来就都是值，类型由类型推断给出。

## 8. 本课要点

1. 高阶函数接收函数作为参数，或返回函数。
2. `apply_twice : ('a -> 'a) -> 'a -> 'a`，因为函数结果要再次喂给同一个函数。
3. `fun x -> e` 是匿名函数。
4. `map : ('a -> 'b) -> 'a list -> 'b list`，对列表每个元素应用 `f`，保留顺序。
5. 函数调用比 `::` 结合得紧：`f h :: map f t` 是 `(f h) :: (map f t)`。
6. 同样的模式可以用于 option 和树。

## 练习

完成 [第 9 课习题](../exercises/09-higher-order-functions.md)，写出类型、值和理由。
