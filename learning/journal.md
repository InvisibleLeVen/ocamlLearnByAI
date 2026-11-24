# 学习记录

## 2026-11-11 · 第 1 次 · 建档与表达式入门

- 用户目标：参照 CS3110 逐步学习 OCaml，穿插习题，当前目录作为学习空间。
- 教材核对：目录、§2.1 The OCaml Toplevel、§2.3 Expressions。
- 初始观察：目录原有 `ex_cap1.ml`，包含模式匹配和递归；仅记录材料存在，不据此推定掌握情况。
- 环境检查：当前终端未找到 `ocaml`、`ocamlc`、`opam`、`dune`、`utop`。
- 本轮内容：表达式与值，int / float / bool，整数和浮点运算，交互输出的读法。
- 布置练习：E01-1（预测类型和值）、E01-2（类型错误与修正）、E01-3（解释概念）。
- 用户作答：待提交。
- 反馈与掌握结论：尚无；暂不评分。
- 运行验证：未执行 OCaml 示例；讲解中的结果为规则推导的预期结果。
- 下次起点：先批改 E01，再决定补充练习或进入 if 表达式。

## 2026-11-11 · 第 2 次 · 首轮反馈与环境激活

- 用户补充：`source ocaml-env.sh` 可以激活环境；工作使用 C++，业余常用 Python，此前学过 Java 和 C#。
- 环境：找到 `/home/lwtang/ask/ocaml-env.sh` 并激活成功。OCaml / ocamlc 5.2.1、opam 2.2.1、dune 3.23.1 可用，utop 可执行文件存在。
- 教材：复核 §2.3 Expressions 的整数、浮点数、运算符与类型转换规则。
- 实际运行：首课示例得到 `int = 7`、`int = 2`、`float = 2.75`、`bool = true`；E01-1 得到 `int = 2`、`float = 2.25`、`bool = true`。
- E01-1 用户答案：A int 2、B float 2.25、C bool true，全部正确；没有展开求值理由。
- E01-2 用户答案：不能通过，因为 `( + )` 要求 int；给出的修正式为 `2 +. 0.5`。原因正确，修正式仍有类型错误。实际运行确认错误指向左侧的 `2`，要求 float；提示后安排 E01-R1，尚未代填完整修正。
- E01-3 用户答案：“8 + 1 是表达式，9 是值”。两句均正确，包含关系未展开，不推定存在互斥分类的误解；安排 E01-R2 用 `true` 复核。
- 记录：原始答案与反馈分开保存在习题文件；复习清单区分已确认错误和待澄清点。
- 教学调整：减少通用编程基础的重复讲解，突出 OCaml 与用户熟悉语言的规则差异。
- 下次起点：反馈 E01-R1、E01-R2；通过后进入 if 表达式。

## 2026-11-11 · 第 3 次 · 订正与 if 表达式

- E01-R1 用户原文：“2. +. 0.5 两个操作数都是float类型这个是我粗心写错了哈哈哈”。修正和类型解释正确，记录其关于粗心的说明，已标记本项订正通过。
- 实际运行：`2. +. 0.5` 得到 `- : float = 2.5`。
- E01-R2 用户原文：“true是值。”判断本身正确，但完整分类仍未覆盖值也是表达式；不推定用户明确主张 true 不是表达式。
- 本轮反馈：直接解释“值是不需要继续计算的表达式”，用 `true` 与 `3 > 2` 比较；不继续重复原题，安排在 if 练习中迁移验证。
- 新课：CS3110 §2.3.4 If Expressions。讲解 if 的结果、条件必须为 bool、两分支类型一致、类型检查两侧但运行只对选中分支求值。
- 实际验证：两个对照表达式均得到 bool true；if 示例得到 int 3，嵌入加法得到 int 13。
- 新练习：E02-1（类型、结果与值的区别）、E02-2（分支类型与常量条件）、E02-3（条件类型），均待作答，未提前填写答案。
- 记录：创建第 2 课笔记与习题，保留此前作答和反馈，更新进度与复习清单。
- 下一步：先批改 E02，依据回答决定补充练习或进入 let 绑定。

## 2026-11-11 · 第 4 次 · 按用户要求重讲 if

- 用户要求重新讲解 if 知识；此前 E02-1 至 E02-3 仍待作答。
- 本轮重讲：if 是表达式而非语句；条件必须为 bool；两分支类型必须一致；整体类型为分支类型；类型检查覆盖两分支，运行时只对选中分支求值。
- 对照用户熟悉的 C++/Python：无语句式 if、条件不接受 0/1 真值、分支间无隐式数值转换、需要值时 else 不可省略。
- 运行验证：无。用户中断了示例运行；文中结果为按类型与求值规则推导的预期结果。
- 下一步：等待 E02 作答，重点核对 E02-1 的“整段 if 不是值、求值结果才是值”。

## 2026-11-11 · 第 5 次 · E02 批改与常量条件

- E02-1 用户原文：“类型是int，求值结果是10，if代码本身不是值，而是表达式，解释器evaluate之后才是值”。类型、结果与表达式/值的区分均正确，判定通过。
- E02-3 用户原文：“不能通过类型检查，if 后面值的期望类型是bool，1是int”。理由正确，判定通过。
- E02-2 用户原文：“不能通过类型检查，没懂条件写成true会不会影响判断是什么意思，判断什么”。结论正确，但未给出理由；用户明确询问题意。
- 本轮反馈：解释“判断”指类型检查能否通过；类型检查不依据条件的常量值跳过 else 分支，then 与 else 都必须可类型检查；`10 : int` 与 `2.5 : float` 不一致，因此拒绝。运行时只会求值选中分支，但类型错误发生在运行之前。对照 `if true then 10 else 20` 可通过。
- 新练习：E02-R1（`if false then 10 else 2.5`），待作答，未提前给答案。
- 运行验证：无；本轮结论为依据类型规则推导，未实际运行 OCaml。
- 记录：更新习题文件、进度、复习清单；R02 以 E02-1 标记已验证，新增 R03。
- 下一步：批改 E02-R1；通过后进入 let 绑定与作用域。

## 2026-11-11 · 第 6 次 · E02-R1 与 let 绑定

- E02-R1 用户原文：“不能通过类型检查,if表达式 then后面的位置和else后面位置期望值的类型应该是相同的。条件写成false不止会只检查else分支，类型检查会检查两个分支。”
- 反馈：判断与理由正确；补充具体冲突为 `10 : int` 与 `2.5 : float`；措辞上“不止会只检查 else 分支”改为“不是只检查 else 分支，而是两个分支都检查”。判定通过。
- 第 2 课完成：E02-1、E02-2、E02-3、E02-R1 全部通过；if 的条件类型、分支类型与常量条件规则已验证。
- 新课：第 3 课 let 绑定与作用域。讲解 let 是绑定而非赋值；局部 `let x = e1 in e2` 的求值、类型取 body 类型、作用域限于 body；顶层 `let x = e;;` 加入环境；同名 let 是遮蔽且默认非递归；if 与 let 组合。
- 新练习：E03-1（嵌套 let 作用域）、E03-2（遮蔽与非递归）、E03-3（顶层与局部绑定）、E03-4（复习 if 分支求值），均待作答。
- 运行验证：无；结果为按类型与求值规则推导的预期结果。
- 教材核对：尝试访问 CS3110 章节页失败（域名无法验证），本轮未标具体小节号。
- 下一步：批改 E03；依据表现进入函数定义或补强作用域。

## 2026-11-11 · 第 7 次 · E03 批改

- E03-1 用户原文：“类型是int，值是9，y在这个表达式结束后用不了了，x在let y = x * 2的右侧是可见的，在let x = 3 in (作用域)，在作用域范围x都是可见的”。类型、值 9 与作用域说明正确，判定通过。
- E03-2 用户原文：“结果是12，与5绑定，因为新的x还没绑定，现在的x还是旧的绑定，不会造成递归，因为绑定的是旧的x，并不会产生递归”。右侧使用旧绑定、结果 12、非递归的理解正确，判定通过。
- E03-3 用户原文：“输出15，第二个let是新绑定，会输出2”。新绑定判断和 `let b = 1 in b + 1;;` 得 2 正确；未回答第一条 `let a = 10;;` 的输出，也未说明 `b;;` 是否报 Unbound。已提示两点，待补答。
- E03-4 用户原文：“类型是int，值是43， 1 / 0 不会被解释器求值，因为解释器求值过程判断条件是true之后就去求值then分支，不会走到else分支，”。类型、值与分支求值理由正确，判定通过。
- 运行验证：无；结果为按规则推导。
- 记录：更新习题文件、进度与复习清单；新增 R04（顶层/局部绑定的环境差异）。
- 下一步：等待 E03-3 补答；通过后进入函数定义与调用。

## 2026-11-11 · 第 8 次 · E03 补答、评分与函数入门

- E03-3 补答原文：“1.a的值是10，2 最后单独输入b;;解释器会报错，因为let b = 1 in b + 1是一个表达式，b的绑定只在in的作用域内是有效的，b不在顶层环境里了。”补答正确，E03 全部通过。
- 本轮评分：95/100。分项：E03-1 25、E03-2 25、E03-3 20、E03-4 25。E03-3 首答漏“第一条输出 10”和“`b;;` Unbound”两点，补答后掌握；按首次作答保留扣分。若只按补答后的最终答案计，E03-3 满分，整轮 100/100。
- 用户偏好：以后每次作答后按满分 100 打分。暂定评分方式：每题等权，结论与理由并重；首次作答与订正分别记录。
- 新课：第 4 课函数定义与调用。讲解 `let f x = e`、空格调用、箭头类型、参数作用域、无 `return`、函数应用优先级、柯里化预览、默认非递归需 `let rec`。
- 新练习：E04-1（函数类型与参数作用域）、E04-2（多参数与柯里化）、E04-3（调用优先级与括号），均待作答。
- 运行验证：无；结果为按规则推导。
- 下一步：批改 E04；依据表现进入递归与模式匹配或补强函数部分。

## 2026-11-11 · 第 9 次 · E04 批改与递归入门

- E04-1 用户原文：“函数类型是 int -> int,inc 5的类型是int，值是6，x在let结束后不能用了，参数的作用域只限于在函数体内”。正确，判定通过。
- E04-2 用户原文：“int -> int -> int，5, 得到一个类型是int -> int 的函数， (add 2) 3 得到一个值，add 2 返回一个int -> int 的函数然后apply 3这个参数，得到5.”正确，判定通过。
- E04-3 用户原文：“inc 2 * 3 是9， inc (2 * 3)是7，因为优先级，函数apply的优先级大于运算符的优先级，”。正确，判定通过。
- 本轮评分：100/100。三题结论与理由均完整；E04-2 的柯里化解释尤其准确。
- 新课：第 5 课递归函数与 `let rec`。讲解默认非递归导致的 `Unbound value`、`let rec` 的自引用、base case 与递归 case、参数递减、负参数不终止、递归代替循环。
- 新练习：E05-1（fact 的类型、值与 `let rec` 必要性）、E05-2（sum_to 与负参数）、E05-3（自写 pow2），均待作答。
- 运行验证：无；结果为按规则推导。
- 下一步：批改 E05；通过后进入模式匹配与列表。

## 2026-11-11 · 第 10 次 · E05 批改与列表模式匹配入门

- E05-1 用户原文：“fact类型是int -> int，24，会报错Unbound value fact, let 默认非递归，函数体里的 fact 看不到正在定义的自己。”正确，判定通过。
- E05-2 用户原文：“类型int -> int,值是10，sum_to (-1)会无限递归，栈溢出，因为一直到不了终止条件，走不到then分支”。正确，判定通过。
- E05-3 用户原文：“let rec pow2 n = if n = 0 then 1 else pow2 (n - 1) * 2. pow2类型int -> int 要求n >= 0，pow2 4值是16, base case n是0，结果是2^0是1，其他情况，是pow2 (n-1) * 2。”正确，判定通过。
- 本轮评分：100/100。三题结论与理由均完整；E05-3 的自写递归函数正确。
- 新课：第 6 课模式匹配与列表。讲解列表构造 `[]` / `::`、同质列表类型、`match` 表达式、`[]` 与 `h :: t` 模式、列表递归、通配符 `_`、匹配穷尽性与 `Match_failure`。
- 新练习：E06-1（sum 列表与递归过程）、E06-2（自写 length）、E06-3（自写 count_positive）、E06-4（非穷尽匹配的警告与运行结果），均待作答。
- 运行验证：无；结果为按规则推导。
- 下一步：批改 E06；通过后进入 option 与自定义变体类型。

## 2026-11-11 · 第 11 次 · 重新开始第 6 课

- 用户要求：以后每开新的一课先汇报总进度；已将 `ocaml-env.sh` 复制到当前目录；代码较复杂时可实际运行检查；重新开始第 6 课。
- 总进度汇报：第 1–5 课已完成并验证（表达式/值、if、let/作用域、函数、递归）；最近评分 E03 95、E04 100、E05 100；当前第 6 课重新开始。
- 重新讲解第 6 课：列表构造与类型、`match` 与模式、`[]` / `h :: t`、列表递归、base case、通配符、穷尽性与 `Match_failure`、整数模式。
- 尝试运行环境与示例时命令被中断；本轮未实际运行 OCaml，结果为按规则推导。
- E06-1 至 E06-4 仍待作答；用户提交较复杂代码时将实际运行检查。
- 下一步：批改 E06；通过后进入 option 与自定义变体类型。

## 2026-11-11 · 第 12 次 · 调整顺序与尾递归

- 用户质疑：为什么未讲尾递归就进入列表与模式匹配。已说明：尾递归是递归的栈行为/优化主题，不是列表匹配的前置知识；原先按“先递归、再用列表练递归、再优化栈”安排，但没有先说明路线，也未确认用户期望的顺序。
- 决定：暂停第 6 课，先补第 5B 课尾递归与累加器；补完后回到 E06。
- 新课：第 5B 课。讲解尾位置、非尾递归的 pending work、累加器、`fact_acc`、`pow2_acc`、尾调用优化、累加器初值与顺序、常见错误。
- 新练习：E05B-1（判断尾递归与计算 fact_acc）、E05B-2（把 sum_to 改为尾递归）、E05B-3（判断多个递归调用是否在尾位置），均待作答。
- 运行验证：无；结果为规则推导。
- 下一步：批改 E05B；通过后恢复第 6 课 E06。

## 2026-11-11 · 第 13 次 · 澄清顺序并恢复第 6 课

- 用户澄清：“没事按照你的路线来，我就是问一下原因，以为你漏掉了”。并非要求调换顺序。
- 决定：恢复原路线，第 6 课列表与模式匹配继续进行；第 5B 课尾递归保留为提前预览，E05B 暂缓，待第 6 课之后正式复习。
- 第 6 课已重新讲解，E06-1 至 E06-4 仍待作答。
- 运行验证：无；结果为规则推导。
- 下一步：批改 E06；之后进入 option 与自定义变体类型，再复习尾递归。

## 2026-11-11 · 第 14 次 · E06 批改与实际运行

- E06-1 用户原文：“int list -> int,值是6，sum [1;2;3] (匹配 h :: t) -> 1 + sum [2;3] (匹配 h :: t) -> 1 + 2 + sum [3] (匹配 h :: t) -> 1 + 2 + 3 + sum [] (匹配 []) -> 1 + 2 + 3 + 0”。类型、值、递归过程与模式均正确，判定通过。
- E06-2 用户原文：`let rec length lst = match lst with [] -> 0 | h :: t -> 1 + length t;;`，结果 3。定义与结果正确；未写类型，且用了 `h :: t` 而非题目要求的 `_ :: t`。待补答类型。
- E06-3 用户原文：`let rec count_positive lst = match lst with [] -> 0 | h :: t -> (if h > 0 then 1 + count_positive t else count_positive t);;`，并说明 base/递归 case。定义与说明正确；未写 `count_positive [3; -1; 0; 5]` 的数值。待补答。
- E06-4 用户原文：“能通过类型检查，编译时会报错，因为xs有没有匹配的情况，f [1;2]应该会异常，因为match有case没匹配上。”类型检查与运行异常正确；但把 Warning 8 说成编译错误。待补答警告/错误区别。
- 实际运行用户代码：`sum : int list -> int`，`sum [1;2;3] = 6`；`length : 'a list -> int`，`length [10;20;30] = 3`；`count_positive : int list -> int`，`count_positive [3;-1;0;5] = 2`；`f [1;2]` 触发 Warning 8 后抛 `Match_failure`。
- 本轮评分：85/100（E06-1 25、E06-2 20、E06-3 20、E06-4 20）。代码正确，扣分来自书面答案遗漏与“警告/错误”表述。
- 记录：更新习题、进度与复习清单；新增 R05。
- 下一步：等待三项补答；通过后进入 option 与自定义变体类型。

## 2026-11-12 · 第 15 次 · E06 补答与 Warning 8

- E06-2 补答原文：“'a list -> int，因为length不要求list元素类型一定是int，对于其他类型的list也适用”。正确，判定通过。
- E06-3 补答原文：“值是2，因为0不是正数”。正确，判定通过。
- E06-4 用户未直接复述，转而提问“Warning 8 是什么”。已解释：Warning 8 是 partial-match，表示匹配不穷尽；默认 `ocamlc`/交互环境仍能编译，运行到未匹配输入时抛 `Match_failure`；某些构建配置可将警告提升为错误，但不是类型检查失败。
- 当前评分：95/100（E06-1 25、E06-2 25、E06-3 25、E06-4 20）；若 E06-4 复述正确，E06-4 可补至 25，整轮 100。
- 记录：更新习题、进度与复习清单；R05 已验证，新增 R06（Warning 8）。
- 下一步：等待 E06-4 复述“默认能否编译 / 异常发生在何时”；通过后进入 option 与自定义变体类型。

## 2026-11-12 · 第 16 次 · E06 完成与变体类型入门

- E06-4 补答原文：“可以编译成功，f [1;2]异常发生在运行期”。正确，判定通过。
- E06 最终评分：100/100。E06-1 25、E06-2 25（补答后）、E06-3 25（补答后）、E06-4 25（补答后）。
- 新课：第 7 课 option 与自定义变体类型。讲解 `type` 定义、构造函数、无数据/带数据构造器、模式匹配、`option` 的 `None` / `Some`、穷尽性。
- 新练习：E07-1（direction 变体与匹配）、E07-2（shape 变体与 area）、E07-3（safe_div 与 get_or_zero），均待作答。
- 运行验证：已实际运行第 7 课示例。`color_to_int Green = 1`；`area (Circle 2.0) = 12.56`；`area (Square 3.0) = 9.`；`safe_div 6 2 = Some 3`；`safe_div 1 0 = None`；`has_value (Some 3) = true`、`has_value None = false`。
- 下一步：批改 E07；通过后进入递归变体类型或更多列表函数。

## 2026-11-12 · 第 17 次 · E07-1 首次作答

- E07-1 用户答案原文：“1. 类型是direction -> direction 2. opposite East 的值是 West 3. 会报warning，match 匹配不穷尽。”
- 反馈：类型 `direction -> direction` 与值 `West` 正确；第 3 问指出 Warning 8 正确，但未说明默认仍可编译、调用 `opposite West` 会在运行期抛 `Match_failure`。
- 评分：90/100；补答正确后记满分。
- 记录：更新习题、进度；新增 R07（变体非穷尽匹配的运行时后果）。
- 下一步：等待 E07-1 补答；随后 E07-2、E07-3。

## 2026-11-12 · 第 18 次 · E07-1 补答通过

- E07-1 补答原文：“能编译成功，调用opposite West会抛Match_failure异常。”正确。
- 反馈：补答完整说明默认编译与运行期后果；E07-1 由 90/100 补至 100/100，判定通过。
- 记录：更新习题、进度；R07 已验证。
- 下一步：E07-2、E07-3 一并作答。

## 2026-11-12 · 第 19 次 · E07-2 通过、E07-3 待改错

- E07-2 用户答案：`area` 函数、两个结果与 `Circle r` / `Rectangle (w, h)` 的绑定均正确。实际运行确认 `area (Circle 2.0) = 12.56`、`area (Rectangle (3.0, 4.0)) = 12.`。评分 100/100。
- E07-3 用户答案：`safe_div : int -> int -> int option`、`Some 3` / `None` 正确；`get_or_zero` 的匹配逻辑正确，但参数名 `val` 是 OCaml 保留字，实际运行报 `Syntax error`。
- E07-3 评分：85/100；改成合法参数名后可记 100/100。E07 暂计 95/100。
- 记录：更新习题、进度；新增 R08（保留字 `val`）。
- 下一步：等待 E07-3 改错；通过后进入第 8 课递归变体类型。

## 2026-11-12 · 第 20 次 · E07 完成与递归变体类型

- E07-3 改错答案：`let get_or_zero opt = match opt with | Some x -> x | None -> 0;;`。正确。
- E07-3 补至 100/100；E07 总评 100/100（E07-1、E07-2、E07-3 各 100）。
- 新课：第 8 课递归变体类型（树）。讲解递归类型定义、`Leaf` / `Node` 构造器、树上的递归函数。
- 新练习：E08-1（size）、E08-2（depth）、E08-3（mem），均待作答。
- 运行验证：本轮运行命令未被允许，第 8 课结果均为按规则推导，未实际运行。
- 下一步：批改 E08；通过后进入高阶函数。

## 2026-11-12 · 第 21 次 · E08 首次批改

- E08-1 用户答案：`size` 函数、类型 `'a tree -> int`、结果 3、base/递归 case 说明均正确。评分 100/100。
- E08-2 用户答案：`depth` 函数、类型 `'a tree -> int`、结果 2 正确；第 4 问“题目定义的，理论上可以是任何数”未说明理由。评分 85/100，待补答。
- E08-3 用户答案：`mem` 函数、结果 `true` / `false`、递归检查说明均正确。评分 100/100。
- E08 暂计 95/100；E08-2 补答正确后整组 100。
- 记录：更新习题、进度；新增 R09（树高按边数计的约定）。
- 下一步：等待 E08-2 补答；通过后完成模块 2，进入第 9 课高阶函数。

## 2026-11-12 · 第 22 次 · E08-2 补答通过与高阶函数入门

- E08-2 补答原文：“因为Leaf下面没有边了，高度算的应该是边的数，如果改成Leaf高度为1， depth t 会变成3，但是实际上到leaf只有两条边，单节点高度是1，到leaf只有一条边。”
- 反馈：三问均正确。`Leaf` 没有向下的边，按边数计高度为 0；若 `Leaf=1`，`depth t` 为 3，而实际边数为 2；单节点高度 1 对应一条边。E08-2 由 85/100 补至 100/100；E08 整组 100/100，判定通过。
- 模块状态：模块 2（列表、模式匹配、变体、option、递归变体）已完成。
- 总进度汇报：约 27%（8 个内容模块粗略加权，2 个完整模块约 25%，尾递归预览约 2%）。
- 新课：第 9 课高阶函数。讲解函数作为参数/返回值、匿名函数 `fun`、`apply_twice`、`map` 的递归实现与类型 `('a -> 'b) -> 'a list -> 'b list`、`Option.map` 思路。
- 新练习：E09-1（类型与值阅读）、E09-2（自写 map）、E09-3（map_option）、E09-4（map_tree，复习树），均待作答。
- 运行验证：已实际运行第 9 课示例。`apply_twice (fun x -> x + 1) 5 = 7`；`apply_twice (fun s -> s ^ "!") "hi" = "hi!!"`；`map (fun x -> x * x) [1;2;3] = [1;4;9]`；`map (fun x -> x > 0) [1;-2;3] = [true;false;true]`；`(add 3) 4 = 7`；`map_option (fun x -> x * 2) (Some 21) = Some 42`；`map_option (fun x -> x * 2) None = None`。
- 记录：更新习题、进度与复习清单；R09 已验证。
- 下一步：批改 E09；通过后继续 filter / fold，再复习尾递归并完成 E05B。

## 2026-11-12 · 第 23 次 · E09 首次批改

- E09-1 用户原文：“1. 18 2. 'a -> 'a -> a -> a 因为apply_twice 定义中 f返回值又作为参数传给了f.所以f的返回值和参数应该是同类型的. 3. [false; true; false; true] bool list. 4. ('a -> 'b) -> 'a list -> 'b list”。第 1、3、4 问正确；第 2 问理由正确，但类型写法不合法（缺括号，`a` 不是类型变量）。评分 90/100，待重写类型。
- E09-2 用户原文：定义与类型、`["a!"; "b!"]` 均正确；第 4 问答“f的参数类型会变成 'a list -> 'b list,每次递归都会调用f处理'a list”。方向部分正确，但该分支实际无法通过类型检查。实际运行报 occurs check：`map f t` 要求 `t : 'a list list`，而 `t : 'a list`。评分 85/100，待重答。（此判断有误，见第 24 次更正。）
- E09-3 用户原文：`map_option` 定义、`Some 8` / `None`、`None` 分支理由均正确。评分 100/100。
- E09-4 用户原文：`Node (f v, map f l, map f r)`、类型、`Node (20, Node (10, Leaf, Leaf), Node (30, Leaf, Leaf))`、base/递归对应说明。类型、结果与思路正确，但递归误用列表 `map`；实际运行报 `'a tree` 与 `'a list` 不匹配。评分 75/100，待改正。
- 本轮首次评分：88/100（E09-1 90、E09-2 85、E09-3 100、E09-4 75）。
- 运行验证：已实际运行确认 E09-1/E09-3 的数值，以及两处错误写法的类型错误。
- 记录：更新习题、进度与复习清单；新增 R10（类型写法）、R11（`f (h :: map f t)` 的类型错误）、R12（树递归误用 `map`）。
- 下一步：等待三项订正；通过后继续 filter / fold，再复习尾递归并完成 E05B。

## 2026-11-12 · 第 24 次 · E09-1 订正与 E09-2 老师更正

- E09-1 订正原文：“('a -> 'a) -> 'a -> 'a”。正确，E09-1 补至 100/100。
- E09-2 用户提出异议并给出 utop 结果：`let rec map f lst = ... | h :: t -> f (h :: map f t)` 得到 `val map : ('a list -> 'a list) -> 'a list -> 'a list`。实际单独运行确认用户正确：该定义能通过类型检查。
- 老师更正：上一轮把测试函数命名为 `bad_map`，函数体却调用 `map`，实际调用的是先前定义的列表 map，因此错误地得到 occurs check。已向用户说明并向习题、复习清单、进度与环境记录补充更正。
- 语义核对：该变体不是逐元素 map。`map (fun xs -> xs) [1;2;3] = [1;2;3]`；`map (fun xs -> 0 :: xs) [1;2;3] = [0;1;0;2;0;3]`（实际运行）。E09-2 暂记 95/100，待补语义小问：`f = fun xs -> 0 :: xs` 时 `map f [1;2]` 的值。
- E09-4 第 1 问仍待订正（`map` 改 `map_tree`）。
- 记录：更新习题、进度与复习清单；R10 已验证；R11 改为“能编译但不是逐元素 map”，待补答。
- 下一步：批改 E09-2 语义小问与 E09-4 订正；通过后继续 filter / fold。

## 2026-11-12 · 第 25 次 · E09-4 订正通过

- E09-4 订正原文：`let rec map_tree f tree = match tree with | Leaf -> Leaf | Node (v, l, r) -> Node (f v, map_tree f l, map_tree f r);;`。正确，E09-4 补至 100/100。
- 实际运行确认：`map_tree : ('a -> 'b) -> 'a tree -> 'b tree`；`map_tree (fun x -> x * 10) t = Node (20, Node (10, Leaf, Leaf), Node (30, Leaf, Leaf))`。
- E09-2 补问：用户回答“行为肯定不一样了”，方向正确，但未给出 `map (fun xs -> 0 :: xs) [1;2]` 的具体值。待补答后结算 E09。
- 当前 E09 合计：E09-1 100、E09-2 95、E09-3 100、E09-4 100，暂计 99/100。
- 记录：更新习题、进度与复习清单；R12 已验证。
- 下一步：等待 E09-2 具体值；通过后进入 filter / fold。

## 2026-11-12 · 第 26 次 · E09 完成与 filter / fold_right

- E09-2 语义补答原文：“f [] 是 []. map f [2]是 [0;2] , map f [1;2]是 [0;1;0;2].”正确，实际运行确认 `[0;1;0;2]`。
- E09-2 补至 100/100；E09 最终 100/100（E09-1、E09-2、E09-3、E09-4 各 100），判定通过。
- 用户提问“你是什么模型”：已说明本会话为 Claude Code CLI，环境标注的底层模型是 `deepseek-v4.1-flash`。
- 总进度汇报：约 31%（2 个完整模块约 25%，模块 3 的 map 部分约 4%，尾递归预览约 2%）。
- 新课：第 10 课 filter 与 fold_right。讲解 predicate、filter 的定义与类型、fold_right 的定义与类型、右结合求值顺序、非尾递归、用 fold_right 表达 map / filter。
- 新练习：E10-1（filter）、E10-2（fold_right 减法与求值顺序）、E10-3（count_positive）、E10-4（filter via fold_right），均待作答。
- 运行验证：第 10 课示例与习题值已实际运行。`filter even [1;2;3;4] = [2;4]`；`filter positive [3;-1;0;5] = [3;5]`；`fold_right (+) [1;2;3] 0 = 6`；`fold_right (fun x acc -> x :: acc) [1;2;3] [] = [1;2;3]`；`fold_right (fun x acc -> x - acc) [1;2;3] 0 = 2`；`fold_right (fun x acc -> x - acc) [5;2;1] 0 = 4`；`count_positive [3;-1;0;5] = 2`；`filter_via_fold (fun x -> x >= 0) [3;-1;0;5] = [3;0;5]`；`map_via_fold (fun x -> x * 2) [1;2;3] = [2;4;6]`。
- 记录：更新习题、进度与复习清单；R11 已验证。
- 下一步：批改 E10；通过后讲 fold_left 与尾递归，再完成 E05B。

## 2026-11-13 · 第 27 次 · E10 首次批改

- E10-1 用户原文：`filter` 定义、`('a -> bool) -> 'a list -> 'a list`、`[-1;-5]`、base/递归 case 说明。全部正确；base case 中 `p` 不参与，分支返回 `[]`。评分 100/100。
- E10-2 用户原文：类型 `('a -> 'b -> 'b) -> 'a list -> 'b -> 'b`；值 4；展开 `(5 - (2 - (1 - 0)))`，从 base case 开始返回，先处理最后一个元素；递归调用不在尾位置，`f` 调用比递归调用更后。全部正确。评分 100/100。
- E10-3 用户原文：`let count_positive lst = fold_right (fun x rst -> if x > 0 then rst + 1 else rst) lst 0;;`、`int list -> int`、值 2；第 4 问答“不清楚你说的 acc 是什么”。定义、类型、值正确；`acc` 即折叠函数第二参数，用户代码中名为 `rst`，代表尾部后缀已统计的正数个数。评分 85/100，待补答 `x = 3` 时 `rst` 的值及对应后缀。
- E10-4 用户原文：`let filter_via_fold f lst = fold_right (fun h rst -> if f h then h :: rst else rst) lst [];;`、`('a -> bool) -> 'a list -> 'a list`、`[3;0;5]`、两个分支说明。全部正确。评分 100/100。
- 本轮首次评分：96/100（E10-1 100、E10-2 100、E10-3 85、E10-4 100）。
- 运行验证：已实际运行确认 E10 全部习题值：`[-1;-5]`、`4`、`2`、`[3;0;5]`。
- 记录：更新习题、进度与复习清单；新增 R13（`fold_right` 的累加参数含义）。
- 下一步：批改 E10-3 补答；通过后讲 fold_left 与尾递归，再完成 E05B。

## 2026-11-13 · 第 28 次 · E10 完成与 fold_left / 尾递归

- E10-3 补答原文：“当3这一层时rst的值是1，对应的后缀子列表的正数个数是 [-1,0;5]”。正确：`x = 3` 时 `rst = 1`，后缀 `[-1; 0; 5]` 中只有 `5` 是正数；列表元素间应为分号，已指出笔误。
- E10-3 补至 100/100；E10 最终 100/100，判定通过。
- 总进度汇报：约 35%（2 个完整模块约 25%，模块 3 约 8%，尾递归预览约 2%）。
- 新课：第 11 课 fold_left 与尾递归，正式复习第 5B 课。讲解 `fold_left` 定义与类型、左结合求值、与 `fold_right` 的参数顺序和结合方向对比、尾位置、累加器、尾调用优化、用 `fold_left` 实现 `rev`。
- 新练习：E11-1（fold_left 类型与求值）、E11-2（sum_to_acc）、E11-3（rev via fold_left）、E11-4（尾位置判断），均待作答。E05B 不再单独计分，其内容由 E11 覆盖，文件保留为额外练习。
- 运行验证：`fold_left ( + ) 0 [1;2;3] = 6`；`fold_left (fun acc x -> acc - x) 0 [1;2;3] = -6`；`sum_via_fold_left [1;2;3;4] = 10`；`rev [1;2;3] = [3;2;1]`；`sum_to_acc 4 0 = 10`。
- 记录：更新习题、进度与复习清单；R13 已验证；第 5B 课状态更新为已由第 11 课覆盖。
- 下一步：批改 E11；通过后模块 3 完成，进入模块 4（模块、签名、抽象类型、函子）。

## 2026-11-13 · 第 29 次 · E11 首次批改

- E11-1 用户原文：类型 `('a -> 'b -> 'b) -> 'a list -> 'b`；值 `-6`；展开 `((0 - 1) - 2) - 3`，理由写“先 f h acc 求值，再把结果作为 acc 递归”；递归调用在尾位置，因为参数表达式先求值。值、展开式与尾位置判断正确；类型错误（应为 `('a -> 'b -> 'a) -> 'a -> 'b list -> 'a`，用户混淆累加器与元素类型且漏初始值参数）；第 3 问参数顺序应为 `f acc h`。评分 70/100，待订正。
- E11-2 用户原文：`let rec sum_to_acc n acc = match n with | 0 -> acc | x -> sum_to_acc (n - 1) (acc + x);;`、`int -> int -> int`、值 10、`acc` 保存累计结果且递归在尾位置。全部正确。评分 100/100。
- E11-3 用户原文：`rev` 用 `fold_left`、值 `[3;2;1]`、结构 `3 :: (2 :: (1 :: []))`、顺序相反的说明。全部正确。评分 100/100。
- E11-4 用户原文：四问判断为“不是/是/不是/是”，理由分别指出 `+ 1`、`h +` 是 pending work。全部正确。评分 100/100。
- 本轮首次评分：93/100（E11-1 70、E11-2 100、E11-3 100、E11-4 100）。
- 记录：更新习题、进度与复习清单；新增 R14（`fold_left` 类型与参数顺序）。
- 下一步：批改 E11-1 订正；通过后模块 3 完成，进入模块 4。

## 2026-11-13 · 第 30 次 · E11-1 第一次订正

- E11-1 订正原文：“('a -> 'b -> 'a) -> 'a -> 'b list  f的第一个参数'b是acc，fold_right的结果,第二个参数是 'a 是fold的list元素的类型.x先对 0 1调用f得到-1作为acc结果,然后调用f -1 2得到-3，以此类推。”
- 第 3 问已订正：正确按 `f acc h` 顺序计算 `0-1=-1`、`-1-2=-3`、`-3-3=-6`。
- 类型仍不完整：`('a -> 'b -> 'a) -> 'a -> 'b list` 缺少最后的结果类型 `-> 'a`；变量映射写反，`f` 的第一参数应为 `'a`（acc），第二参数为 `'b`（元素），返回 `'a`。
- E11-1 由 70 补至 85；E11 暂计 96/100。
- 记录：更新习题、进度与复习清单；R14 仍待订正。
- 下一步：等待补全类型与变量映射；通过后模块 3 完成，进入模块 4。

## 2026-11-13 · 第 31 次 · E11 完成与模块入门

- E11-1 第二次订正原文：“('a -> 'b -> 'a) -> 'a -> 'b list -> 'a .f第一个参数acc，存储递归fold结果，第二个参数是'b list中的元素。”正确：类型完整，`f` 第一参数 `'a`（acc）、第二参数 `'b`（元素）。
- E11-1 补至 100/100；E11 最终 100/100，判定通过。模块 3 完成。
- 总进度汇报：约 40%（3 个完整模块约 37.5%，尾递归约 2%）。
- 新课：第 12 课模块、签名与抽象类型。讲解 `module ... struct ... end`、`module type ... sig ... end`、`val`、模块约束 `module M : S = ...`、抽象类型 `type t`、封装与表示独立性、栈/队列例子。
- 新练习：E12-1（Math 模块与签名）、E12-2（Tally 抽象类型）、E12-3（Queue 信息隐藏），均待作答。
- 运行验证：第 12 课示例已实际运行。`Counter.inc 5 = 6`；`Hidden.zero : Hidden.t = <abstr>`；`Hidden.to_int (Hidden.inc Hidden.zero) = 1`；`Hidden.double 3` 报 Unbound；`Hidden.inc 5` 报 int 与 Hidden.t 不匹配；`Stack.pop (Stack.push 1 (Stack.push 2 Stack.empty)) = Some (1, <abstr>)`。习题值：`Math.square 4 = 16`、`Math.cube 2 = 8`；`Tally.zero : Tally.t = <abstr>`；`Tally.add 1 2` 类型错误、`Tally.double 3` Unbound、`Tally.to_int Tally.zero = 0`；`Queue.dequeue (Queue.enqueue 1 (Queue.enqueue 2 Queue.empty)) = Some (2, <abstr>)`；`Queue.enqueue 1 [2;3]` 类型错误。
- 记录：更新习题、进度与复习清单；R14 已验证；模块 3 标记完成，模块 4 进行中。
- 下一步：批改 E12；通过后讲函子，继续模块 4。

## 2026-11-13 · 第 32 次 · 课程计划澄清（ADT 与函数式概念）

- 用户提问：什么时候讲 ADT 之类的函数式概念，整体计划是什么。
- 说明：ADT 有两个常见含义。代数数据类型（algebraic data type）已在第 6–8 课覆盖：变体、构造器、模式匹配、option、递归树；抽象数据类型（abstract data type）正在第 12 课通过模块签名隐藏表示。
- 尚未覆盖的相关内容：records（带字段的积类型）、用 ADT 构建表达式 AST 与 evaluator、函子、规格/测试/归纳证明、复杂度与数据结构。
- 调整后的计划：E12 批改后，第 13 课讲 records 与 ADT 综合（表达式树 + 求值器）；第 14 课讲函子；之后模块 5 规格与测试、模块 6 效率与复杂度、模块 7 数据结构、模块 8 异常/可变状态/项目。
- 记录：更新进度中的模块状态（模块 4 进行中、模块 6 尾递归已验证）与后续计划。
- 下一步：等待 E12 作答；按调整后的顺序继续。

## 2026-11-13 · 第 33 次 · 补讲 ADT 概念（插入课）

- 用户指出：此前只教了 `type`、带数据的构造器、option/list/tree 的用法，没有正式介绍 ADT 概念；教材中有专门介绍这一主题的内容。承认这是教学遗漏。
- 决定：暂停 E12，插入第 13 课“代数数据类型（ADT）”，补讲概念后再回到第 12 课。
- 讲解内容：
  1. ADT 定义：用构造器组合已有类型；和类型（或）与积类型（且）；
  2. 和类型：`color`、零元构造器、`|` 读作 or；
  3. 积类型：元组与 records（定义、构造、字段访问、记录模式、`{ p with ... }` 函数式更新）；
  4. 和 + 积组合：`shape`；
  5. 代数解释：有限类型取值个数的 `+`、`×`、指数；list/tree 的递归类型方程；
  6. 引入（构造器）与消去（模式匹配）；封闭世界与穷尽性检查；
  7. 递归 ADT 与结构化递归；
  8. 表达式树 `expr` 与 `eval`；
  9. 代数数据类型 vs 抽象数据类型；
  10. 与 OOP 的表达式问题对比。
- 新练习：E13-1（和/积与取值个数）、E13-2（records）、E13-3（扩展 expr 的 Sub/Neg、simplify、穷尽性）、E13-4（概念题），均待作答。
- 运行验证：`type point` 记录、`move_right`、`norm2`；`eval (Add (Num 2, Mul (Num 3, Num 4))) = 14`；`is_pass (curve alice) = true`；扩展 `expr` 的 `eval (Sub (Num 10, Neg (Num 3))) = 13`。
- 记录：新增 `lessons/13-algebraic-data-types.md`、`exercises/13-algebraic-data-types.md`；更新进度与概念状态；E12 标记暂停。
- 下一步：批改 E13；之后回到 E12，再讲函子。

## 2026-11-13 · 第 34 次 · records 语法答疑

- 用户问 `{ p with x = p.x +. 1.0 }` 和 `let norm2 { x; y } = ...` 是什么语法。
- 已解释：前者是 record update（复制旧记录并替换指定字段，原记录不变），`p.x` 是字段投影，`+.` 是浮点加法；后者是记录模式 + 字段 punning，`{ x; y }` 等价于 `{ x = x; y = y }`，在函数入口解构记录。
- 已实际运行演示：`q = move_right p` 后 `p = {x=1.; y=2.}`、`q = {x=2.; y=2.}`；`norm2 p = 5.`；显式写法 `p.x *. p.x +. p.y *. p.y` 与 `{ x = a; y = b }` 均得 5；`{ x; _ }` 只取 x。
- 下一步：继续 E13。

## 2026-11-13 · 第 35 次 · records 多字段更新与字段顺序续问

- 用户问：`{ p with ... }` 要更新很多字段怎么写；`let { x; y .. } = p` 是否按 record 顺序构造；若字段顺序换了，是否会报“找不到这样顺序的 record”。
- 已解释并实际运行核对：
  1. 多字段更新用 `;` 分隔：`{ p with x = 10; y = 20.0; z = "yes" }`；只列要改的字段，未列字段从 `p` 复制。字段名可用 punning：`{ p with x; y }`。所有字段都列出时，`with` 多余（Warning 23），可直接构造新记录。
  2. `let { x; y; _ } = p` 是记录**模式**（解构/消去），不是构造；`{ x; y }` 是字段 punning 模式，等价于 `{ x = x; y = y }`；`_` 表示忽略其余字段。OCaml 没有 `..` 这种“剩余字段”语法；写成 `..` 是语法错误。只写 `{ x; y }` 也是合法模式，但开启 Warning 9 时会提示漏写字段，`_` 表明是有意忽略。
  3. 记录按字段名（label）匹配，不按位置或书写顺序。`{ z = "no"; y = 2.0; x = 1 }` 与 `{ x = 1; y = 2.0; z = "no" }` 相等（实测 `p = q` 为 `true`）；类型声明中的字段顺序也不影响按名访问。顶层打印会按声明顺序重排，容易误以为按位置。
  4. 记录是 nominal 类型：类型由记录类型名/定义决定，不是单纯由字段集合决定。若作用域里有另一个同名字段的记录类型，可能需要 `(q : test)` 或返回类型标注来消歧。若在同一个顶层会话中重定义同名 `type test` 且改变字段顺序，旧值属于旧类型、新值属于新类型（错误信息显示 `test/2`）；这是重定义类型造成的，不是字段顺序造成的。
- 运行验证：`p = q`（q 以 z,y,x 顺序构造）= `true`；`{ p with x = 10; z = "yes" }` 保留 `y = 2.`；`let { x; y } = p` 在 `#warnings "+9"` 下产生 Warning 9，`let { x; y; _ } = p` 无警告；`{ p with x; y }` 的 punning 更新可用；全字段 `with` 产生 Warning 23。
- 记录：更新 progress 的 records 概念行；E13 仍待作答。
- 下一步：继续 E13。

## 2026-11-16 · 第 36 次 · 表达式问题与抽象数据类型答疑

- 用户问“表达式问题（expression problem）是什么，抽象数据类型是什么”。
- 已解释 expression problem：按 case 定义数据类型时，希望同时能加新 case（数据扩展）和新操作（操作扩展），且不修改已有代码、保持静态类型安全。OCaml 的 ADT + 模式匹配使“加操作容易、加数据要改所有 match 函数”；OOP 类层次通常相反，“加子类容易、加方法要改基类和所有子类”。用 `expr` 加 `Sub` 的例子实际验证：漏写分支产生 Warning 8，运行到该值抛 `Match_failure`。提到 visitor、object algebra、type class、多态变体、模块/函子等缓解方案，但各有代价。
- 已解释抽象数据类型：由操作接口和行为规范定义、隐藏内部表示；客户端只能通过签名中的操作使用，不能依赖具体表示。用 `STACK` 签名和 `Stack` 实现（内部 `'a list`）说明 `type 'a t` 抽象、`<abstr>`、不能对客户端暴露 list 操作、替换实现不影响客户端、可维护不变量。
- 已区分代数数据类型（构造器/字段公开，模式匹配，封闭世界）与抽象数据类型（表示隐藏，操作为接口，表示独立）；二者可组合：模块内部用 ADT/record 实现，签名对外暴露抽象类型。
- 说明这不属于 E13-4 的作答，暂不评分；E13-4 仍待用户作答后批改。
- 记录：更新 progress 的日期与概念行；E13 仍待作答。
- 下一步：继续 E13。

## 2026-11-16 · 第 37 次 · E13 首次作答与批改

- 用户提交 E13-1 至 E13-4；原文已保存到 `exercises/13-algebraic-data-types.md` 的“用户作答”一节，反馈单独记录。
- 首次评分：E13-1 60/100、E13-2 80/100、E13-3 85/100、E13-4 78/100；E13 总评 76/100。
- E13-1：`bit`/`color`/`maybe_color` 判为和类型、`pair` 的取值 4、`triple` 8 正确；漏 `bit * color = 6` 与各类型构造器个数，`pair` 未说明“单构造器 + 积载荷”；第 4 问把 `bit -> color` 算成 `2 × 3 = 6`，应为 `3^2 = 9`（6 是 `bit * color` 的个数）。
- E13-2：类型、`alice`、`curve`、`name_of` 与结果正确；`is_pass { _; score }` 实际运行报 `Syntax error`，通配符须放最后 `{ score; _ }`；`if ... then true else false` 可简化为 `score >= 60`；第 6 问只给了一个理由。
- E13-3：`eval` 五分支正确、`eval (Sub (Num 10, Neg (Num 3))) = 13`、Warning 8/`Match_failure`、加构造器 vs 加操作的理由均正确。第 5 问 `simplify` 实际运行报语法错误：`Add (simplify expl) (simplify expr)` 等构造器应用必须写成元组 `Add (..., ...)`；`Neg exp -> Neg exp` 未递归化简；另有重复调用 `simplify` 的效率问题。
- E13-4：第 1 问 sum-of-products 核心正确但构造器大小写写错；第 2 问对应关系正确但自述不知道定义；第 3 问错误地把穷尽性检查归因于“ADT 值有限”，实际是封闭世界（`list`/`tree` 值无限），`int` 不具备构造器封闭性，编译器也不做算术定理证明；第 4 问代数 vs 抽象数据类型正确；第 5 问两个扩展方向基本正确。
- 实际运行验证：`is_pass { score; _ } alice = false`；`curve alice = {name = "Alice"; score = 63}`；`is_pass (curve alice) = true`；`name_of alice = "Alice"`；修正语法并递归处理 `Neg` 后 `simplify (Add (Num 0, Add (Num 0, Num 5))) = Num 5`、`simplify (Neg (Add (Num 0, Num 5))) = Neg (Num 5)`。
- 记录：更新习题文件、progress、review（R15–R20）；E13 标记为待订正。
- 下一步：批改 E13 订正；重点重做 E13-1 第 1/2/4、E13-2 第 3/6、E13-3 第 5、E13-4 第 2/3。

## 2026-11-16 · 第 38 次 · `pair` 的和/积结构追问

- 用户追问：`type pair = Pair of bit * bit` 虽然只有一个构造器，但 `bit` 有两个构造器，难道 `pair` 不是和类型吗？不是“先积后和”吗？
- 已解释：广义上 `pair` 的外层确实是和类型，但只有 **一个分支**（`Pair`），是平凡和；`bit` 的两个构造器属于字段类型，不是 `pair` 的顶层分支。嵌套结构从外到内是：外层单分支和 → 载荷 `bit * bit` 的积 → 两个 `bit` 各自的和。基数为 `1 × ((1+1) × (1+1)) = 4`。
- 已用实际运行展示：`pair` 的四个值为 `Pair (Zero, Zero)`、`Pair (Zero, One)`、`Pair (One, Zero)`、`Pair (One, One)`，都以 `Pair` 开头；另定义 `type either = Left of bit | Right of bit` 作为真正的两分支和类型，值为 `Left Zero`、`Left One`、`Right Zero`、`Right One`。
- 已说明“构造器个数”与“取值个数”不是一回事：`pair` 1 个构造器但有 4 个值；`either` 2 个构造器也有 4 个值，但结构是选择而非组合。
- 记录：更新 review R16 与 progress 的 ADT 概念行；评分不变，仍待订正。
- 下一步：继续 E13 订正。

## 2026-11-16 · 第 39 次 · “看最外层非平凡结构”的分类追问

- 用户追问：是否“看最外层是什么类型，数据就是什么类型”；`pair` 外层是平凡和，所以不考虑，仍算积类型？
- 已确认大方向正确，但修正措辞：不是“外层不存在”，而是单构造器变体在结构与基数上可约掉；`pair ≅ 1 × (bit × bit) ≅ bit × bit`。`Pair` 仍是值构造器，提供 nominal 包装、模式匹配和与裸元组的类型区分，只在分类/基数意义下忽略。
- 已给出判断启发式：看最外层的**非平凡**结构；外层变体有 ≥2 构造器 → 和类型；只有 1 个构造器 → 退化和，继续看载荷；元组/记录 → 积；函数 → 指数。并强调“取值个数相同”不能决定结构（`pair` 与 `either` 都是 4 个值，但一个是积、一个是和）。
- 记录：更新 progress 的 ADT 概念行；评分不变，仍待 E13 订正。
- 下一步：继续 E13 订正。

## 2026-11-16 · 第 40 次 · 函数类型指数再讲解

- 用户仍不理解 E13-1 第 4 问为什么 `bit -> color` 是 `3^2 = 9` 而不是 `2 × 3 = 6`。
- 改变讲法：先明确“函数是一张完整映射表，不是一对 (输入, 输出)”。先枚举 `bit -> bool` 的 4 个函数（两列真值表），再列 `bit -> color` 的 9 行映射表；每行是一个完整函数，第一列 `f Zero` 有 3 种选择，第二列 `f One` 有 3 种选择，独立相乘得 9。
- 解释 `2 × 3` 的实际含义：从 2 个输入中选一个、从 3 个输出中选一个，得到一个数对，如 `(Zero, Red)`；这是 `bit * color` 的元素，不是覆盖两个输入的函数。函数必须同时给 `Zero` 和 `One` 指定输出。
- 给出公式 `|A -> B| = |B|^|A|`，并对比 `color -> bit` 为 `2^3 = 8`，说明输入/输出方向会影响结果。
- 记录：更新 review R15；评分不变，仍待 E13-1 第 4 问订正。
- 下一步：继续 E13 订正。

## 2026-11-16 · 第 41 次 · E13 订正批改

- 用户提交 E13-1 第 1/2/4、E13-2 第 3/6、E13-3 第 5、E13-4 第 2/3 的订正；原文已保存到习题文件“订正作答”一节。
- 订正后评分：E13-1 100、E13-2 90、E13-3 95、E13-4 95；E13 总评 95/100。
- E13-1：构造器个数、`pair` 的“单构造器平凡和 + 积载荷”、`bit * color = 6` 全部正确；`bit -> color = 3^2 = 9` 及两输入独立选择的理由、9 个函数枚举正确。小笔误 `Bule` → `Blue`。
- E13-2：第 6 问两个理由已补；第 3 问逻辑和 `false` 正确，但记录模式写成 `{score,_}`（逗号），实际运行报 `Syntax error: "}" expected`，应写 `{ score; _ }`；代码中的 `60` 不应写成 `60.`（float）。
- E13-3：第 5 问的构造器元组写法、`Neg` 递归、`l`/`r` 绑定均已改对；但两处 `simplify exp1`（数字 1）应为 `expl`（字母 l），实际运行报 `Unbound value "exp1"`。改成 `expl` 后编译并验证：`simplify (Add (Num 0, Add (Num 0, Num 5))) = Num 5`、`simplify (Neg (Add (Num 0, Num 5))) = Neg (Num 5)`、`simplify (Mul (Num 1, Num 7)) = Num 7`。
- E13-4：第 2 问引入/消去定义正确；第 3 问封闭世界、构造器覆盖、`int` 非封闭、编译器不做算术证明均正确；仅“正数”应为“整数”。
- 记录：更新习题文件、progress、review；E13 只剩两个代码笔误，改掉后可判满分。
- 下一步：确认 E13-2 `{ score; _ }` 与 E13-3 `exp1` → `expl`；通过后回到 E12-1 至 E12-3。

## 2026-11-16 · 第 42 次 · E13 完成与函数式概念缺口盘点

- 用户提交修正后的 `simplify` 与 `is_pass { score; _ }`；`simplify` 已通过类型检查并实际运行：`Add (Num 0, Add (Num 0, Num 5)) → Num 5`、`Neg (Add (Num 0, Num 5)) → Neg (Num 5)`、`Mul (Num 1, Num 7) → Num 7`；`is_pass alice = false`、`is_pass { name = "Bob"; score = 60 } = true`。E13-2、E13-3 订正通过，E13 按 100/100 计。
- 用户问“除了 ADT 还有什么比较重要的函数式概念你漏掉了吗”。已做缺口盘点，而不是声称已覆盖全部：
  1. 规格、测试与正确性：前置/后置条件、不变量、性质测试、结构归纳/等式推理；当前只有零散的类型与运行验证。
  2. 函子与模块进阶：`Set.Make`/`Map.Make`、参数化模块、签名约束；第 12 课只讲了基础模块/签名/抽象类型，函子未讲。
  3. 效果与求值：纯度/引用透明、eager vs lazy、异常、`result`、可变状态/引用/数组/IO；目前只用了 `option` 和纯函数。
  4. 参数多态与类型推断：`'a`、泛型、Hindley–Milner 推断、值限制；目前只是使用过多态函数，未系统讲规则。
  5. 闭包与词法作用域：高阶函数已用，但闭包捕获环境、部分应用的作用域没有正式讲。
  6. 惰性求值与流：`lazy`、`Seq`、无限序列/按需求值；未讲。
  7. 效率与持久数据结构：复杂度、共享、均摊分析、持久 vs 瞬时数据结构；未讲。
  8. 数据结构：栈、队列、BST、平衡树、映射、集合；未讲。
  9. 高级/可选：monad/applicative、CPS、GADT、effect handlers、first-class modules；不属于当前 CS3110 主线，暂不排入。
- 说明：第 1–3 项是接下来最该补的；第 2 项是模块 4 的下一步；第 1 项是模块 5；第 3 项与模块 8 相关；第 7–8 项在模块 6–7。用户已有 C++/Python 经验，闭包/多态的概念可能熟悉，但 OCaml 的具体规则仍需单独讲。
- 记录：更新 progress 的 E13 完成状态、records/表达式树概念行；review R17/R18 标为已验证。
- 下一步：E12-1 至 E12-3；之后函子，再按缺口盘点调整模块 5/6/8 的顺序。

## 2026-11-17 · 第 43 次 · 回到 E12：模块、签名与抽象类型复习

- 用户说“继续学习 ocaml”；按断点回到第 12 课 E12（模块、签名、抽象类型），E13 已完成。
- 汇报总进度约 42%：模块 1–3 完成（约 37.5%），模块 4 已讲模块/签名/抽象类型与 ADT（约 3%），尾递归约 2%；函子未讲，E12 未作答。
- 简短复习：`module M = struct ... end` 暴露实现；`module type S = sig ... end` 定义接口；`module M : S = struct ... end` 用签名封住模块；签名中的 `type t` 无 `= ...` 时是抽象类型。
- 实际运行核对 `Counter` 与 `Hidden : COUNTER`：`Hidden.zero : Hidden.t = <abstr>`、`Hidden.inc Hidden.zero : Hidden.t = <abstr>`、`Hidden.to_int (Hidden.inc Hidden.zero) = 1`；`Hidden.double` 报 `Unbound value`；`Hidden.inc 5` 报 int 与 `Hidden.t` 类型不匹配。用这些结果说明表示隐藏与访问限制。
- 重发 E12-1（Math 模块与签名）、E12-2（Tally 抽象类型）、E12-3（Queue 信息隐藏）；均待用户作答，未提前填答案。
- 下一步：批改 E12；通过后讲函子。

## 2026-11-17 · 第 44 次 · 重新开始 E12：模块、签名、抽象类型从头讲

- 用户要求“重新开始 E12”；按新起点从头讲，不假设上次讲解已掌握。
- 讲解结构：
  1. `module Counter = struct ... end`：模块是命名空间 + 实现，未加签名时所有内容可见，`Counter.t = int` 透明；
  2. `module type COUNTER = sig ... end`：签名用 `val` 列接口，`type t` 不写 `= int` 即抽象；
  3. `module Hidden : COUNTER = struct ... end`：签名约束/封住模块，签名外内容隐藏；
  4. 实际运行 `Hidden.zero : Hidden.t = <abstr>`、`Hidden.inc Hidden.zero : Hidden.t = <abstr>`、`Hidden.to_int (Hidden.inc Hidden.zero) = 1`；`Hidden.double` 报 `Unbound value`，`Hidden.inc 5` 报 int 与 `Hidden.t` 不匹配；
  5. 抽象类型与表示独立性：客户端不能依赖内部表示，实现可换而接口不变；
  6. 常见坑：`sig` 里用 `val`、`struct` 里用 `let`；`type t` 与 `type t = int` 的区别；`module M : S = struct ... end` 的语序。
- 重发 E12-1 至 E12-3，均待用户作答；未提前填答案。
- 下一步：批改 E12；通过后讲函子。

## 2026-11-17 · 第 45 次 · E12 首次作答批改

- 用户提交 E12-1 至 E12-3；原文已保存到 `exercises/12-modules-signatures.md` 的“用户作答（2026-11-17，原文）”一节，反馈单独记录。
- 评分：E12-1 80/100、E12-2 100/100、E12-3 90/100；E12 总评 90/100（三题等权）。
- E12-1：第 1、2、4、5 步正确。第 3 步写成 `module Math : Math = struct ... end`，实际运行报 `Error: Unbound module type "Math"`，提示 `There is a module named "Math", but modules are not module types`。冒号右边要写签名名 `MATH`；模块名与模块类型名是不同名字，且 OCaml 区分大小写。已给出正确写法 `module Math : MATH = struct ... end`，待重交该行。
- E12-2：五问全部正确。实际运行核对 `Tally.zero : Tally.t = <abstr>`、`Tally.add Tally.zero Tally.zero : Tally.t = <abstr>`、`Tally.add 1 2` 报 int 与 `Tally.t` 不匹配、`Tally.double 3` 报 `Unbound value "Tally.double"`、`Tally.to_int Tally.zero = 0`。
- E12-3：第 1、2、4 问正确；第 1 问实际运行 `Some (2, <abstr>)`，完整类型 `(int * int Queue.t) option`。第 3 问结论正确，但理由的类型写法不准确：`Queue.enqueue : 'a -> 'a t -> 'a t`，代入 `1 : int` 后 `Queue.enqueue 1 : int Queue.t -> int Queue.t`；`[2; 3] : int list`，与 `int Queue.t` 不匹配。实际错误为 `This expression has type "'a list" but an expression was expected of type "int Queue.t"`。已说明不能把内部表示 `'a list` 当抽象类型 `'a Queue.t`。
- 记录：更新习题文件、progress、review（R21 模块/模块类型混淆，R22 部分应用后的参数类型）。
- 下一步：等用户订正 E12-1 第 3 步；确认后讲第 14 课函子。

## 2026-11-17 · 第 46 次 · 第 14 课函子

- 用户说“继续学习 ocaml”；按断点检查：E12 已批改（总评 90/100），E12-1 第 3 步订正仍未提交，R22 仍为需巩固。E12-1 订正继续作为本组作答的前置小项，未代填。
- 总进度汇报：约 42%，模块 4 进行中；本课讲解函子后内容覆盖约 44%，但函子尚未经 E14 验证。
- 新课：第 14 课函子。讲解函子是从模块到模块的编译期构造、`module Repeat (M : MONOID) = struct ... end` 语法、参数签名是契约、函数应用 `module R = Repeat (IntSum)`、结果签名与信息隐藏、签名共享约束 `with type elt = int`、标准库 `Set.Make` / `Map.Make`、与高阶函数及 C++ 模板的层次差异。
- 实际运行核对（OCaml 5.2.1）：
  - `module Repeat : functor (M : MONOID) -> sig val times : int -> M.t -> M.t end`；
  - `R.times : int -> IntSum.t -> IntSum.t`，`R.times 3 5 = 15`；`S.times 2 "ab" = "abab"`；
  - `Repeat (IntSum).times` 报 `Unbound constructor "Repeat"`，说明函子结果必须先绑定成模块；
  - 实参模块缺少 `add` 时，错误发生在应用 `Repeat (Bad)` 时：`Modules do not match ... The value "add" is required but not provided`；函子体使用签名外的 `M.mul` 则在定义时报 `Unbound value`；
  - `MakeStack` 透明结果：`IntStack.push 1 IntStack.empty : IntStack.t = [1]`；`IntStack.t = elt list` 公开；
  - `HiddenStack : STACK with type elt = int`：`HiddenStack.empty : HiddenStack.t = <abstr>`，`HiddenStack.push 1 []` 报 `'a list` 与 `HiddenStack.t` 不匹配；
  - 把 `add` 改成减法后签名仍匹配，`Repeat (IntSub)` 编译通过，`R.times 3 5 = 5`、`R.times 2 5 = 0`，说明签名只检查类型，不检查代数律；
  - `Set.Make (Int)`：`IntSet.elements (IntSet.add 3 (IntSet.add 1 IntSet.empty)) = [1; 3]`；`IntSet.mem 3 IntSet.empty = false`。
- 新练习：E14-1（Repeat 的类型、值与签名只查类型）、E14-2（MakeStack、结果签名与隐藏）、E14-3（函子概念与 `Map.Make`）；另布置 R22 相近复习题（`Stack.push 1` 的部分应用类型）。E14 三题等权计分，R22 单独更新复习状态。
- 用户尚未作答，本轮不评分，不推定掌握。
- 下一步：批改 E12-1 订正与 E14；确认模块 4 完成后进入模块 5（规格、测试与正确性）。

## 2026-11-17 · 第 47 次 · C++ / Python 中的“函子”类比答疑

- 用户问：C++ 或 Python 里有没有类似 OCaml functor 的东西。此为概念答疑，不是 E14 作答，暂不评分。
- 先澄清术语冲突：C++ 里的 “functor” 通常指**函数对象**（定义了 `operator()` 的类），对应 OCaml 的闭包/高阶函数，不是 OCaml 的函子（module → module）。
- C++ 最接近的机制：
  1. 类模板 `template <typename T> class Stack { ... };`，接近 `MakeStack` 的类型参数化；
  2. 策略类 / 模板模板参数，如 `std::map<Key, T, Compare>` 的 `Compare`，接近“传入一组操作”；
  3. C++20 `concept` 给模板参数写显式约束，接近参数签名的契约作用，但检查时机和严格程度仍不同；
  4. C++20 modules 只组织代码，不参数化，没有函子等价物。
- Python 没有编译期模块函子；最接近的是运行时的类工厂/模块工厂（函数接收类或模块、返回类或模块）、元类，以及 `typing.Protocol`/`abc.ABC` 表达的接口契约。Python 的模块是运行时对象，可以直接作为参数传递，但没有 OCaml 那样的签名检查、表示隐藏和编译期函子应用。
- 已强调 OCaml 函子的独特点：参数是满足签名的模块、函子体按签名检查、结果是可被结果签名约束的模块、表示独立性由类型检查器保证；这些在 C++ 模板和 Python 动态机制中都没有一一对应。
- 记录：更新 progress 概念表；E14 仍待作答。
- 下一步：继续等 E12-1 订正、E14 与 R22 复习题。

## 2026-11-18 · 第 48 次 · 参数签名、结果签名与使用处约束

- 用户提出两个疑问：
  1. 函子返回的模块是否只暴露函子定义时提供的签名？例如 `Repeat` 返回的模块只能访问 `times`，访问不到 `add`；
  2. 不理解“没有结果签名时 `IntStack.t = int list` 公开，想隐藏表示就用结果签名”，并指出此前没有讲清“结果签名”是什么。
- 已澄清：
  1. 参数签名 `(M : MONOID)` 是**输入契约**，只限制函子体能使用什么，不会把 `M` 的成员自动导出。结果模块暴露的是 `struct ... end` 里定义的内容；`Repeat` 的 body 只有 `times`，所以 `R.zero`、`R.add` 报 `Unbound value`。想要导出，需在 body 中写 `let zero = M.zero`、`let add = M.add`，或 `include M`。
  2. “结果签名”是函子定义中参数列表之后、`= struct` 之前的 `: ...` 部分：`module Make (X : S) : R = struct ... end`。不写时结果接口由实现推断（`type t = elt list` 公开，`IntStack.push 1 []` 可过）；写了则结果模块只暴露 `R` 的内容，签名中的 `type t` 抽象化，未列出的值隐藏。`with type elt = E.t` 共享参数中的类型，否则 `elt` 也会抽象，客户端无法 `push 1`。
  3. 补充区分第三种写法：`module H : STACK with type elt = int = MakeStack (...)` 是**使用处的模块签名约束**（ascription），约束的是应用结果，只影响 `H`；它与函子定义上的结果签名作用范围不同。此前讲义第 6 节把这两种写法混在一起，已改写。
- 实际运行核对：
  - `R.zero`、`R.add` 均报 `Unbound value`；
  - `Repeat2` 显式导出后 `R2.zero : IntSum.t = 0`、`R2.add 2 3 = 5`；
  - 无结果签名的 `IntStack.push 1 []` 能通过（`IntStack.t = int list`）；
  - `module MakeHiddenStack (E : ELEM) : STACK with type elt = E.t = struct ... end` 后 `H.empty : H.t = <abstr>`，`H.push 1 H.empty` 可用，`H.push 1 []` 报 `'a list` 与 `H.t` 不匹配；
  - 使用处约束 `HiddenStack2 : STACK with type elt = int` 效果相同；未在签名中的 `size` 报 `Unbound value`。
- 记录：改写 `lessons/14-functors.md` 第 3、6、9 节；在 E14-2 第 3 问补充“结果签名 vs 使用处约束”的追问。用户仍未提交 E14 作答，本轮不评分。
- 下一步：继续等 E12-1 订正、E14 与 R22 复习题。

## 2026-11-18 · 第 49 次 · 函子参数类型的抽象性与实参模块的透明度

- 用户指出：`R2.add 2 3` 中 `add` 的参数类型是 `MONOID.t`，而 `MONOID` 签名里的 `t` 是抽象的，为什么还能传 `2` 和 `3`？并要求重新发 E14 题目。
- 澄清：在**函子体内部**，`M.t` 确实抽象，`let x = M.add 2 3` 实际报 `int` 与 `M.t` 不匹配。但 `IntSum` 定义时**没有**加签名，所以 `IntSum.t = int` 是透明等式；应用 `Repeat2 (IntSum)` 后 `M.t` 被实例化为 `IntSum.t = int`，`R2.add` 的完整类型是 `IntSum.t -> IntSum.t -> IntSum.t`，因此 `R2.add 2 3` 能过，结果显示 `IntSum.t = 5`。
- 对照实验：`module IntSumHidden : MONOID = struct type t = int ... end` 后，`R2h.zero : IntSumHidden.t = <abstr>`，`R2h.add 2 3` 报 `int` 与 `IntSumHidden.t` 不匹配。结论：参数签名内部的 `M.t` 始终抽象；应用后能否使用 `int` 字面量，取决于实参模块自身是否公开 `t` 的表示。
- 记录：在 `lessons/14-functors.md` 第 3 节补充该对照；重新在对话中发出 E14 题目。用户尚未作答，本轮不评分。
- 下一步：等 E12-1 订正、E14 与 R22 复习题。

## 2026-11-18 · 第 50 次 · 结果接口推断与实参模块透明度

- 用户追问：`MakeStack` 的结果接口是否由 body 推断，`elt = int`、`t = int list` 是否因为匿名实参模块没有被 `ELEM` 封住。
- 已澄清：
  1. `MakeStack` 未写结果签名，函子结果接口由 body 推断，顶层显示 `type elt = E.t`、`type t = elt list`；此时 `E.t` 在函子内部是抽象的。
  2. 应用匿名实参 `struct type t = int ... end` 时，该模块自身签名含 `type t = int`（透明），应用把 `E.t` 替换成 `int`，所以结果中 `elt = int`。`t = elt list` 的结构来自 body。
  3. 参数注解 `(E : ELEM)` 只约束函子内部视角，不会抹掉实参模块的类型等式；真正封住实参要在实参定义处写 `: ELEM`。
  4. 对照实验：`module HiddenArg : ELEM = ...` 后 `HiddenStack.elt = HiddenArg.t` 抽象，`HiddenStack.push 1 ...` 报 `int` 与 `HiddenStack.elt` 不匹配；但因为没有结果签名，`type t = elt list` 仍公开，`List.length HiddenStack.empty = 0`、`HiddenStack.empty = []` 仍可用。
- 记录：更新 progress 概念表；用户仍未作答 E14，不评分。
- 下一步：等 E12-1 订正、E14 与 R22 复习题。

## 2026-11-18 · 第 51 次 · 结果签名为什么能隐藏列表表示

- 用户追问：`module MakeHiddenStack (E : ELEM) : STACK with type elt = E.t = struct ... end` 为什么能把列表表示也隐藏。
- 已解释签名归属（ascription）的“投影”语义：
  1. `STACK` 中 `type elt`、`type t` 都没有 `= ...`，二者都是抽象类型；
  2. `STACK with type elt = E.t` 只给 `elt` 增加等式，`t` 仍是抽象的；
  3. 结果签名是客户端能看到的全部内容。实现里虽然有 `type t = elt list`，但这个等式不在结果签名中，归属后被“抹掉”；客户端只知道 `t` 是某个抽象类型，不能当列表用；
  4. 这不是运行时包装：底层仍是列表，隐藏发生在类型检查层面；
  5. 若在结果签名中显式写 `and type t = E.t list`，列表表示就会公开。
- 实际运行核对：
  - `MakeHiddenStack : STACK with type elt = E.t` 后，`H.t` 抽象：`List.length H.empty` 报 `H.t` 与 `'a list` 不匹配；未在签名中的 `H.size` 报 `Unbound value`；
  - `MakeVisibleStack : STACK with type elt = E.t and type t = E.t list` 后，`V.t = int list`，`List.length V.empty = 0`。
- 记录：更新 progress 概念表；用户仍未作答 E14，不评分。
- 下一步：等 E12-1 订正、E14 与 R22 复习题。

## 2026-11-18 · 第 52 次 · 重排第 14 课教程并重发 E14

- 用户反馈概念看得有点晕，要求重新发一遍 E14 的教程和题目。
- 已将 `lessons/14-functors.md` 重排为一条主线：
  1. 一句话定义：函子 = 模块 → 模块的编译期函数；
  2. 最小例子 `Repeat`：参数签名、body、应用、结果只含 body 中定义的 `times`；
  3. 两个签名位置：参数签名 `(X : S)` 管输入，结果签名 `: R` 管输出；
  4. 结果签名示例 `MakeStack`：无结果签名时接口由 body 推断、`type t = elt list` 公开；有 `STACK with type elt = E.t` 时 `t` 抽象、列表表示隐藏；对照 `and type t = E.t list` 时公开；
  5. 三个透明度视角：函子体内 `M.t` 抽象、实参模块自身是否封住、函子结果签名；
  6. `Set.Make` / `Map.Make`；
  7. 常见误解表与要点。
- 习题保持原内容重发：E12-1 遗留订正、E14-1、E14-2（含结果签名 vs 使用处约束追问）、E14-3、R22 复习题。
- 补做运行核对：`let empty = []` 未标注时 `U.empty : 'a list = []`，`U.push "a" U.empty : U.t = ["a"]` 仍可通过。
- 用户尚未作答，本轮不评分，不推定掌握。
- 下一步：按先 E14-1、再 E14-2、最后 E14-3 的顺序作答；批改时逐题给出分数与证据。

## 2026-11-19 · 第 53 次 · E12-1 订正与 E14 首次批改

- 用户提交 E12-1 第 3 步订正与 E14-1、E14-2、E14-3、R22 的答案；聊天内容重复发送两次，按同一份答案记录一次，原文保存到对应习题文件。
- 评分：
  - E12-1 第 3 步订正 95/100：`module Math : MATH = ...` 代码正确，仍缺一句“`Math` 是模块名、`MATH` 是模块类型名”的理由。E12 总评暂计 95/100（E12-1 95、E12-2 100、E12-3 90）。
  - E14-1 72/100、E14-2 88/100、E14-3 63/100；E14 首次总评 74/100。
  - R22 复习题 85/100：`Stack.push 1 : int Stack.t -> int Stack.t` 正确，这是 E12-3 的关键错误点，已纠正；第 2 问未写出 `[2; 3] : int list` 与 `int Stack.t` 两侧类型。
- E14-1 主要问题：
  - Q1 类型正确，但称“顶层显示 `int -> int -> int`”；实际显示保留别名 `int -> IntMul.t -> IntMul.t`。
  - Q2 用了 `IntSum` 的 `zero = 0` 与 `add = +` 展开 `P = Repeat (IntMul)`，答案 15；正确应为 `IntMul.zero = 1`、`IntMul.add = ( * )`，`P.times 3 5 = 125`。
  - Q3 类型以 `int -> string -> string` 表述可接受，值 `"---"` 正确；实际显示保留 `StrCat.t`。
  - Q4 不改函子、可通过、结果 5 正确；Q5 签名只查类型不查语义正确。
- E14-2 主要问题：
  - Q1 值正确；类型写 `string list`，实际顶层显示 `StringStack.t`（等价）。
  - Q2 正确；Q3 结论与“使用处约束只影响当前模块、函子结果签名影响所有应用”正确；`<expr>` 应为 `<abstr>`。
  - Q4 结论 `'a list` 与“push 仍能通过”正确；理由写成“因为 `t` 是 string 且未被签名限制”，未点出 `empty` 去掉注解后泛化为 `'a list`、可实例化为 `string list` 才是关键。
- E14-3 主要问题：
  - Q1、Q2 正确。
  - Q3 错误：实参 `IntMul` 缺少 `add` 时，错误发生在**应用** `Repeat (IntMul)`，不是定义 `Repeat` 时；定义时只检查 body 是否满足 `MONOID`。实际运行 `module NoAdd = struct type t = int let zero = 0 end` 后 `module B = Repeat (NoAdd)` 报 `Modules do not match ... The value "add" is required but not provided`。
  - Q4：`bindings` 类型正确；但 `StringMap.t` 表示不公开，`StringMap.empty : 'a StringMap.t = <abstr>`，`List.length StringMap.empty` 报 `'a StringMap.t` 与 `'b list` 不匹配。这里的签名是 `Map.Make` 的结果签名（约 `Map.S with type key = String.t`），不是 `String` 的参数签名。
- 实际运行核对（OCaml 5.2.1）：
  - `P.times : int -> IntMul.t -> IntMul.t`，`P.times 3 5 = 125`；
  - `S.times : int -> StrCat.t -> StrCat.t`，`S.times 3 "-" = "---"`；
  - 减法版 `Rsub.times 3 5 = 5`；
  - `StringStack.push "a" StringStack.empty : StringStack.t = ["a"]`；
  - `HiddenStack.empty : HiddenStack.t = <abstr>`，`HiddenStack.push "a" []` 报 `'a list` 与 `HiddenStack.t` 不匹配；
  - 去掉 `empty` 注解后 `U.empty : 'a list = []`，`U.push "a" U.empty : U.t = ["a"]`；
  - `StringMap.empty : 'a StringMap.t = <abstr>`，`StringMap.bindings : 'a StringMap.t -> (StringMap.key * 'a) list`；
  - `Stack.push 1 : int Stack.t -> int Stack.t`，`Stack.push 1 [2; 3]` 报 `'a list` 与 `int Stack.t` 不匹配。
- 布置订正：E12-1 补理由；E14-1 Q1/Q2 重答；E14-2 Q4 改写理由；E14-3 Q3/Q4 重答；R22 Q2 补两侧类型。未代填答案。
- 记录：更新两份习题文件、progress、review。review 中 R21 改为“代码已验证，理由待补”、R22 改为“关键点已验证，表述待补”，新增 R23（顶层保留类型别名）、R24（`Repeat (IntMul)` 展开用错实参成员）、R25（参数签名错误位置在应用处）、R26（`Map.Make` 结果模块 `t` 抽象）、R27（未注解 `empty` 的泛化与实例化）、R28（`<abstr>` 与 `<expr>`）。progress 中模块 4 行、概念状态 8 行与近期表现已更新，估算维持约 44%。E14 仍未通过，模块 4 保持进行中。
- 下一步：批改上述订正；通过后完成模块 4，进入模块 5（规格、测试与正确性）。

## 54. 2026-11-19 第二轮订正批改（E12-1 理由、E14-2 Q4 追问、E14-3、R22）

- 用户提交第二轮订正（原文保存到 `exercises/12-modules-signatures.md` 与 `exercises/14-functors.md`）。E14-1 第 1 问仍称顶层显示 `int -> int -> int`；第 2 问承认把 `IntMul` 看成 `IntSum`，但未给值；E14-2 第 4 问回“没明白问题”；E14-3 第 3、4 问重答；R22 第 2 问重答。
- 评分：E12-1 100/100（E12 总评 97/100）；E14-1 维持 72/100；E14-2 维持 88/100；E14-3 95/100；E14 总评 85/100；R22 本轮 50/100。
- E12-1：“右边期望是签名而不是模块”要点正确，接受为要求的那句理由；已补充更精确的表述（模块类型名 vs 模块名）。
- E14-1：为纠正“顶层显示 `int -> int -> int`”，实际运行并抄录输出（OCaml 5.2.1）：
  - `module P : sig val times : int -> IntMul.t -> IntMul.t end`
  - `P.times;;` → `- : int -> IntMul.t -> IntMul.t = <fun>`
  - `P.times 3 5;;` → `- : IntMul.t = 125`
  结论：顶层打印保留类型路径 `IntMul.t`，不展开为 `int`；同时确认 `Repeat (IntMul)` 的正确结果 125。已请其补一行结果与展开。
- E14-2 第 4 问：用户未理解提问方式，已重述为三步（删掉注解后 `[]` 的类型是什么；`push` 的参数要求什么；`'a list` 为什么能用在要求 `t` 的位置），关键术语点明“泛化 / 实例化”。
- E14-3 第 3 问：订正正确（25/25），错误发生在应用处，附实参缺 `add` 的 `Modules do not match` 报错。第 4 问（20/25）：`StringMap.empty : 'a StringMap.t = <abstr>`、`List.length` 期望 `'a list` 正确；未说明结果签名作用，并追问“哪个 `t`”。
- 澄清两个 `t`：`String.t` 是实参模块的键类型（= `string`），`Map.S` 里的 `'a t` 是映射本身的类型；`Map.Make` 的结果签名形如 `Map.S with type key = String.t`，只把 `key` 与实参的 `t` 共享，映射的 `t` 不写出来。实际运行 `module type of StringMap` 显示 `type key = String.t` 与 `type 'a t = 'a Map.Make(String).t`，客户端看不到其表示。
- R22 第 2 问仍错：写成“期望 `'a -> 'a Stack.t`，实际 `int -> int list`”，与 E12-3 同类（把 `push` 的完整类型当成参数类型，并把元素类型接到列表字面量上）。已给出“先看部分应用后的类型，再看实参自身类型”的分解步骤，待重答。
- 记录：更新两份习题文件、journal、progress、review。E14 仍未通过（总评 85/100），模块 4 保持进行中。
- 下一步：批改 E14-1 第 1、2 问、E14-2 第 4 问、E14-3 第 4 问补充、R22 第 2 问；通过后完成模块 4，进入模块 5（规格、测试与正确性）。

## 55. 2026-11-19 教师更正：顶层打印与 `-short-paths`（E14-1 改判 100/100）

- 用户粘贴 utop 5.2.1 实际会话，显示 `module P : sig val times : int -> int -> int end` 与 `P.times;; - : int -> int -> int = <fun>`，同时给出 `P.times 3 5 = 125`；并指出 E14-3 第 4 问答案“结果签名把 key 类型共享出去了”。用户还要求：以后需要重答的题目必须附上完整上下文代码，不要让其回翻历史。
- 教师核实并更正（本轮实测，OCaml 5.2.1）：
  - `ocaml`（默认）：`- : int -> IntMul.t -> IntMul.t = <fun>`
  - `ocaml -short-paths`：`- : int -> int -> int = <fun>`；`S.times` 为 `- : int -> string -> string = <fun>`
  - utop 的 `--help` 显示 `-short-paths` 为默认值，因此与用户 utop 输出一致。
  - 结论：上一轮“顶层保留别名、E14-1 第 1 问扣分”的判断错误，用户观察正确；`IntMul.t = int` 是类型等式，两种显示是同一类型，差别只在前端打印器。
- 改判：E14-1 由 72/100 改为 **100/100**（第 2 问 125 正确、第 3 问 utop 下的 `int -> string -> string` 正确）；E14-3 由 95/100 改为 **100/100**（第 4 问“结果签名共享 `key`”正确）；E14-2 维持 88/100（第 4 问待答）；E14 总评由 85/100 改为 **96/100**。
- 另核对：`StringStack.push "a" StringStack.empty` 在 `-short-paths` 与默认下均显示 `- : StringStack.t = ["a"]`，故 E14-2 第 1 问“写成 `string list` 不够精确”的判分维持不变。
- 更新：`lessons/14-functors.md` 第 2 节加入 `-short-paths` / utop 的显示对照表；`exercises/14-functors.md` 追加第三轮作答与批改；本 journal；待更新 progress 与 review。
- 用户要求已记录：重答题目必须自带完整上下文代码（写入 progress 的学习者信息，并保存为长期记忆）。
- 下一步：用带完整上下文的题面重新发 E14-2 第 4 问与 R22 第 2 问；通过后完成模块 4，进入模块 5。

## 56. 2026-11-19 第四轮批改（E14-2 第 4 问、R22）

- 用户按带完整上下文的题面重答两题（原文保存到 `exercises/14-functors.md`）。
- 评分：E14-2 **95/100**（第 4 问 24/25，第 1 问维持 22/25）；E14 总评 **98/100**；R22 本轮 **90/100**。
- E14-2 第 4 问：`'a list`、能通过、理由（`t` 等于 `string list`，`empty` 的 `'a list` 能满足）均正确；仅未点明“泛化 / 实例化”这两个术语，已在反馈中点出，不再要求重交。
- R22：方向已纠正——参数位置要求栈类型，不再写成 `int -> int list` 这类函数类型；剩余精度问题是没把 `'a` 实例化成 `int`。实际运行核对（OCaml 5.2.1，`-short-paths` 与默认输出相同）：
  - `Stack.push 1 : int Stack.t -> int Stack.t`
  - `Stack.push 1 [2; 3]` 报 `This expression has type "'a list" but an expression was expected of type "int Stack.t"`
- 记录：更新 `exercises/14-functors.md`、journal；待更新 progress 与 review（R22 改为关键点已验证、细节待确认）。
- 下一步：用一小题确认 `'a` 实例化为 `int`（题面自带完整代码）；通过后模块 4 关闭，进入模块 5（规格、测试与正确性）。

## 57. 2026-11-19 模块 4 关闭

- 用户答 `int t -> int t`，`'a` 实例化为 `int` 正确；utop 实际显示 `- : int Stack.t -> int Stack.t = <fun>`，只差模块前缀。R22 记 95/100，状态改为已验证。
- E14 最终：E14-1 100、E14-2 95、E14-3 100，总评 98/100；E12 97/100。模块 4（模块、签名、抽象类型、函子）完成。
- 进度估算：4 个模块完成（4 × 12.5% = 50%），加尾递归约 2%，整体约 **52%**。
- 下一步：模块 5 第 15 课——规格（requires / ensures、前置与后置条件、规格与类型的区别）。

## 58. 2026-11-19 第 15 课首次作答（规格）

- 用户作答（原文保存到 `exercises/15-specifications.md`）：E15-1 认为无需 `requires`，但不会写 `ensures`，并按“非负数”改了代码；E15-2 答“恒等成立”；E15-3 给出设计 A 的 `requires [List.exists (fun x -> x > 0) lst]`，设计 B 未写 `ensures`（只写“返回 int option”），选择 option 设计；R29 答“签名不规定结合律，只检查类型，规格管语义”。
- 评分：E15-1 40/100；E15-2 10/100；E15-3 40/100；E15 总评 30/100；R29 70/100。
- 判定的核心缺口：不会把行为写成可检查的 `ensures`（第 15 课的主要技能），且把“返回 `int option`”当作后置条件（那是类型）。E15-2 把 `abs x >= 0` 当成恒成立，忽略了 `min_int`。
- 教师核实（OCaml 5.2.1）：`abs min_int = -4611686018427387904`，`abs min_int >= 0 = false`；第二条 `abs x = x || abs x = -x` 对所有 `int` 成立（`min_int` 时靠第一个析取项成立）。
- 下一步：用“等式型 / 范围型 / 分情况型”三种句式教写 `ensures`，给一个不同于习题的例子（`sum` 的 `ensures` 用 `List.fold_left` 表达）；E15-2 让用户在 utop 里自查 `min_int`；E15-3 用 `index_of_opt` 的分情况形状示范后重答。

## 59. 2026-11-19 第 15 课第二轮（ensures 句式、min_int、E15-3）

- 用户作答（原文保存到 `exercises/15-specifications.md`）：E15-1 用 `List.length (List.filter (fun x -> x > 0) [lst])` 写 `ensures`，第 3 问把谓词改成 `>= 0`；E15-2 在 utop 实测 `min_int`、`abs min_int`、`abs min_int >= 0 = false` 并问原因；E15-3 的两个 `ensures` 用了 `List.hd (List.filter ... [lst])`，第 4 问四个结果全部正确。
- 评分：E15-1 55/100；E15-2 30/100；E15-3 87/100；E15 总评 57/100。
- 主要问题：`[lst]` 与 `lst` 混淆（`[lst]` 是含一个元素的列表，元素是 `lst` 本身）。实际报错 `This expression has type "'a list" but an expression was expected of type "int"`。三处 `ensures` 均需改为直接传 `lst`。
- E15-2：实测正确；已解释 `min_int`：OCaml `int` 为 63 位，范围 `-2^62 .. 2^62-1`；`min_int = -2^62`，其相反数 `2^62` 超出范围，二进制补码下回绕为 `min_int`，故 `- min_int = min_int`、`abs min_int < 0`。实际核对：`max_int = 4611686018427387903`、`1 lsl 62 = min_int`、`min_int = - min_int`。
- E15-3 第 4 问：`first_positive [0;-1;2] = 2`；`first_positive [0;-1]` 抛 `Assert_failure`；`first_positive_opt [0;-1;2] = Some 2`；`first_positive_opt [0;-1] = None`，全部正确。
- 待完成：E15-2 第 3 问（修补方案）；三处 `[lst]` 修正；E15-3 第 2 问把 `Some i` 改成“第一个正数的值”；R29 第 2 问补“签名与规格为何不能互相替代”。

## 60. 2026-11-19 第 15 课收尾

- 用户作答：E15-2 第 3 问用 `requires [x > min_int]` 排除溢出输入；E15-3 第 1 问自行把 `[lst]` 改为 `lst`，并要求教师代改其余两处；R29 第 2 问答“签名检查类型、规格检查行为，无法互相替代”。
- 评分：E15-1 95/100；E15-2 85/100；E15-3 97/100；E15 总评 **92/100**；R29 最终 85/100。E15 通过。
- 教师代办：按用户明确要求，改写了 E15-1 第 2、3 问与 E15-3 第 2 问的 `ensures`（`[lst]` → `lst`，`Some i` → `Some h`）；用户原答案完整保留，代办部分在习题文件中注明。这是本轮唯一的“代答”，理由是用户已独立修正同类错误并明确要求。
- 遗留提醒：E15-1 第 3 问的函数名 `count_positive` 与“非负数”语义不符，建议改名（扣 5 分）。
- 第 15 课结束，模块 5 进入测试部分。

## 61. 2026-11-19 按教材实际目录重排课程（用户指出大纲遗漏）

- 用户质疑为何大纲没有 CPS、monad、GADT、promise、mutable ref，要求核对 `https://cs3110.github.io/textbook/cover.html` 并按教材讲授、习题用教材现成的。
- 实际核对（本日联网）：教材共 10 章 + 附录 + Curry–Howard。完整目录与各章 Exercises 标题已抓取核对。
  - 结论：**教材没有 CPS / continuation，也没有 GADT 章节**；monads 仅 7.8；promises 在 7.2–7.4、7.7；refs 在 6.1；可变字段 6.2；数组与循环 6.3。
  - 教师此前大纲的确实遗漏：整个第 6 章（可变性）、第 7 章（并发）、第 9 章 9.4–9.7（序列、记忆化、持久数组、2-3 树）、第 10 章（解释器）、附录 Big-Oh/VM。
  - 另核实：第 3–5 章有若干小节未讲（2.2/2.5/2.6/2.7、3.3/3.5/3.6/3.8/3.10/3.12、4.6/4.8、5.5–5.8）。
- 新计划：第 16 课测试（8.4–8.6）→ 第 17 课正确性（8.7–8.9）→ 第 6 章 → 第 7 章 → 第 9 章 → 第 10 章 → 附录；练习改用教材 Exercises（有 ★ 难度与官方解答），配合自编题；漏讲小节随对应课程补讲。
- 环境核实：lwt 6.1.2 已安装（第 7 章可用）；OUnit、QCheck 未安装，第 8.6/3.3 需要时先安装或用自写驱动。
- 已更新 `learning/progress.md`：新增“教材章节对照”表，重写后续计划与下轮计划。

## 62. 2026-11-19 补上 4.6 管道等漏讲小节

- 用户再次指出大纲遗漏：教材 4.6 Pipelining（`|>` 管道）未安排进课程表（前一轮只在“漏讲”段落提到，未排课）。已核对教材 4.6 内容：由 `sum_sq` 的 for 循环 → 尾递归 → 高阶函数 + 管道操作符，含自定义中缀 `--`、函数组合 `<<`。
- 处理：新增第 18 课“补讲”，集中安排 4.6 管道、2.2 编译、5.5 编译单元、5.6 函数式数据结构、5.7 模块类型约束、5.8 includes、3.6 类型同义词、2.5–2.7 文档/打印/调试；4.8 代数定律与 3.12 自然数并入第 17 课（与 8.7–8.9 等式推理同主题）；3.3 OUnit 与 8.6 QCheck 并入第 16 课；3.10 异常放第 19 课；3.8 关联列表放第 26 课；3.5 高级模式匹配放第 30 课。
- 已更新 `learning/progress.md` 的后续计划与章节对照表。

## 63. 2026-11-19 找回函数式概念表并落盘

- 用户问：“之前我问你为什么没讲 ADT 概念的时候你总结了一个函数式概念的表，由简单到进阶的。你还有吗？”
- 已检索学习记录与历史会话：原表在 2026-11-16 第 42 次作答后的回复中，标题是**“按重要性排序”**（不是“由简单到进阶”），共 9 行：规格/测试/正确性、函子与模块进阶、效果与求值策略、参数多态与类型推断、闭包与词法作用域、惰性求值与流、效率与持久数据结构、经典数据结构、高级主题（monad/CPS/GADT/effect handlers/first-class modules）。
- 处理：新建 `lessons/concept-map.md`，保留原表，另按 L1 基础 / L2 中阶 / L3 进阶 / L4 高级重排，补上核对过的 CS3110 章节号与当前状态（函子、规格已验证；测试进行中；其余未开始）。
- 已在 `progress.md` 加一行指向该文件。
- 下一步：等 E16-1 至 E16-3 与 R34；并等用户在 A（先做完第 8 章）与 B（先做第 6、7 章）之间选定顺序。

## 64. 2026-11-19 选修专题排入大纲 + 惰性求值/流的位置

- 用户要求：把 CPS、GADT、effect handlers、first-class modules 这些选修也做进教程大纲；并问惰性求值和流放在哪一章学。
- 核对教材（本地已下载的 102 个章节页）：**没有 CPS、GADT、effect handlers 专章**；first-class modules 只在 5.9 函子的 `StackTester` 测试示例中出现（`let stacks = [ (module ListStack : Stack) ; ... ]`），属顺带提及。
- 惰性求值与流的位置：第 9 章 **9.4 Sequences**，小节顺序为 9.4.1 反例（`let rec ones = 1 :: ones` 这类写法的问题）→ 9.4.2 thunk → 9.4.3 用序列编程 → 9.4.4 Laziness（`Lazy` 模块）→ 9.4.4.1 Lazy Sequences；**9.5 Memoization** 紧接着用 `Lazy` 做记忆化。本课程排在第 **28** 课（数据结构第 3 讲）。
- 排课：新增第 33–36 课选修，按依赖排序：33 first-class modules（依赖第 14 课函子/5.9）→ 34 CPS 与 defunctionalization（依赖高阶函数、第 10 章解释器、联系 7.5–7.7 回调）→ 35 effect handlers（依赖第 6 章可变性、7.8 monads、34 课续延）→ 36 GADTs（依赖第 13 课 ADT、10.5–10.6 类型检查与推断）。
- 环境实测（OCaml 5.2.1）：first-class modules 可编译运行（打包 `(module struct let x = 1 end : S)`、解包 `(val m : S)` 得 `1`）；effect handlers 可用（`perform Ask` + `continue k 41` 得 `42`）；`Seq.take 4 (Seq.iterate succ 0)` → `0 1 2 3`；自写 `'a stream = Nil | Cons of 'a * 'a stream Lazy.t` 的 `take 5 (nats 0)` → `0 1 2 3 4`。
- 已更新 `learning/progress.md`（后续计划第 6 项细化到 9.1–9.7 的课次、新增第 33–36 课、章节对照表补 5/9 行与“教材没有的选修专题”行、新增“选修课（第 33–36 课）”一节含依赖与环境实测）与 `lessons/concept-map.md`（L3 惰性行、L4 拆成四行选修、核对结论与顺序问题）。
- 下一步：仍等 E16-1 至 E16-3 与 R34；并等用户在顺序 A/B 之间选定。

## 65. 2026-11-19 Curry–Howard 排入大纲（第 37 课）

- 用户要求把 Curry–Howard 也排进大纲。原先它只写在“附录（可选）”，现提升为**第 37 课**（选修五）。
- 核对教材 `adv/curry-howard`（本地页面）：**教材有专章**，标题 The Curry-Howard Correspondence，小节为 Computing with Evidence → The Correspondence（Types Correspond to Propositions / Programs Correspond to Proofs / Evaluation Corresponds to Simplification）→ What It All Means → Exercises。内容基于 Wadler 的 *Propositions as Types*；语言是 ML 原本作为定理证明器的 meta language。
- 对应关系（教材原文）：`/\`↔`*`、`->`↔`->`、`\/`↔“和类型且指明是哪一支”、空类型 `type empty = |` ↔ 假、`unit` ↔ 真；“类型被占据（inhabited）”= 命题成立。
- 教材习题：propositions as types [★★]、programs as proofs [★★★]、evaluation as simplification [★★★]，有官方解答——第 37 课优先用这些题，不另编。
- 依赖：第 13 课 ADT（积/和/函数类型）、第 17 课结构归纳与等式推理、10.3 替换模型（small-step 求值）。因此排在第 36 课 GADT 之后。
- 环境实测（OCaml 5.2.1）：`type empty = |` 与 `let absurd (e : empty) = match e with _ -> .`（空匹配消去）编译通过并运行。
- 已更新 `learning/progress.md`（后续计划第 8 项加 37 课、附录项改为只剩 Big-Oh/虚拟机、章节对照表新增 adv 行、选修课表新增第 37 行与来源标注）与 `lessons/concept-map.md`（L4 Curry–Howard 行、核对结论）。
- 下一步：仍等 E16-1 至 E16-3 与 R34；并等顺序 A/B 的选择。

## 66. 2026-11-19 按 Fall 2026 官网目录复核并补全课程大纲

- 用户要求重新核对官网当前教程大纲，找出遗漏并补入本地课程路线。
- 实际核对 Cornell CS3110 Fall 2026 官方教材目录与主题页。发现此前排课表漏了第 8 章 8.2 Function Documentation（returns/requires/raises/examples、specification game、comments）、8.3 Module Documentation（abstraction function、representation invariant、`rep_ok`）；第 5.1 Module Systems 只在第 12、14 课的模块/函子内容中零散涉及，未单独归纳；第 1 章函数式编程视角也只在既有学习中分散出现，未做整体回顾。
- 另发现模块进阶的章节状态需要细分：5.6 尚未开始；5.7 的 `with type` 与 5.8 的 `include` 已在第 14 课用过，但缺少系统讲解和验证；已更正概念地图与进度表，安排第 18B 课补齐。5.6 函数式数据结构移入第 26 课，与第 9 章并讲。
- 补课安排：第 18 课拆为 18A（第 1 章回顾、2.2、2.5–2.7、3.6、4.6）、18B（5.1、5.5、5.7、5.8）与 18C（8.2–8.3）。既有 16–17 课与 19 课以后顺序不变。
- 已更新 `README.md` 总路线、`learning/progress.md` 章节对照和课程计划、`lessons/concept-map.md` 状态与缺口说明。未记录新的掌握情况或用户作答。
- 下一步仍按当前断点处理 E16-1 至 E16-3 与 R34。

## 67. 2026-11-20 细化选修课程依赖与 OCaml 版本范围

- 用户要求落实对 GADT、CPS、effect handlers 等选修课程的评审建议。
- 按依赖重排选修：33 first-class modules → 34 CPS/defunctionalization → 35 Curry–Howard → 36 GADTs → 37 effect handlers。Curry–Howard 不依赖 GADT，先用普通积、和、函数类型讲对应关系；effect handlers 放在 GADT 后，因为 effect 操作具有类型索引。
- 第 34 课改为围绕同一个 `eval` 做直接风格 → CPS → defunctionalization，并要求用相同输入比较结果；保留与 callbacks 的联系，不要求实现异步调度器。
- 第 36 课补充目标：GADT 分支中的类型细化、局部抽象类型、存在类型作用域、显式多态递归签名；相等见证作为选做。示例需在本机 OCaml 5.2.1 实测，避免依赖更新版的类型检查细节。
- 第 37 课明确本机版本 API：OCaml 5.2.1 使用 `Effect.Deep.match_with` 的 `retc` / `exnc` / `effc` 记录式深处理器；不使用 OCaml 5.3 新增的 `match ... with effect ...` 语法。主例为 `Yield` 生成器，状态为选做，异步调度只阅读；每个捕获 continuation 应恢复或终止一次，运行时检查重复使用但不检查从未恢复。
- 第 7.8 monad 单元加短 Applicative 对照：`let+` / `and+` 与 `let*`，用可累积错误的 `Validation` 表示独立计算的组合；不新增一门独立选修。
- 已更新 `README.md`、`learning/progress.md`、`lessons/concept-map.md`。本轮只调整课程计划，没有新增教学或用户掌握证据。
- 核对依据：OCaml 5.2 Manual §12.24 与 `Effect` API 文档；OCaml 5.3 发布说明确认新增处理器语法；OCaml 5.2 GADT 教程确认类型细化、存在类型作用域和显式多态递归要求。后续讲义和代码按已安装的 5.2.1 验证。
- 下一步仍按当前学习断点批改 E16-1 至 E16-3 与 R34。

## 68. 2026-11-20 升级 OCaml 环境并清理旧版

- 用户要求升级 OCaml 环境到最新版，随后明确要求删除旧版本、全部切换到新版本。
- 核实当日最新稳定编译器为 OCaml 5.5.2；opam 2.6.1、Dune 3.25.0、utop 2.18.0、Lwt 6.1.2 已安装。opam 将旧根目录从 2.2 迁移到 2.6 前，先备份了 1.1 GB 根目录；新 switch 和脚本验证通过后按用户要求删除备份。
- 新建唯一 switch `ocaml-5.5.2`，安装 Dune 3.25.0、utop 2.18.0、ocamlfind 1.9.9~preview、Lwt 6.1.2 与 utop 依赖；opam 2.6.1 已替换原 2.2.1，原二进制备份删除。
- 两个环境脚本（`/home/lwtang/ask/ocaml-env.sh` 与工作区副本）均改为激活 `ocaml-5.5.2`。随后删除旧 `default` / OCaml 5.2.1 switch 和 opam 2.2.1 根目录备份；opam 当前只列出 `ocaml-5.5.2`。
- 验证：source 常用环境脚本后，`ocaml` / `ocamlc` 显示 5.5.2，opam 显示 2.6.1，Dune 显示 3.25.0，utop 显示基于 5.5.2 编译，opam 查询确认 Lwt 6.1.2；GADT typed AST evaluator 与 effect handler 示例均实际编译运行得到 42。
- 已同步 `AGENTS.md`、`README.md`、`learning/progress.md` 和 `lessons/concept-map.md`；教学记录中的旧环境测试保留为历史记录，不代表旧 switch 仍安装。OUnit、QCheck 仍未安装。
- 下一步仍从当前断点批改 E16-1 至 E16-3 与 R34。

## 69. 2026-11-20 应用户要求从头重讲第 16 课

- 用户在会话开始时表示“继续学习”，教师按断点给出第 16 课要点回顾与原题（E16-1 至 E16-3、R34）；用户随后要求“重新给我开始 16 课”，即从头重讲而非只回顾。本轮无任何用户作答，E16 与 R34 保持待作答状态。
- 重讲原则：教学例子全部换成与习题不同的函数，避免讲义直接给出习题答案。
  - 单元测试、用例挑选、黑盒/白盒：教学例子 `clamp lo hi x`（三分支实现 + 五条 assert + 退化区间 `clamp 10 0 5`）；讲解“危险位置”选例表（范围中间 / 恰好边界 / 边界外一格 / 远离边界 / `lo = hi`）。
  - 性质测试强弱：教学例子 `length` / `append` 与假实现 `append_bad l1 _ = l1`（丢掉第二个列表）；弱性质 `length (append_bad l1 l2) >= length l1` 全过、强性质 `length (append_bad l1 l2) = length l1 + length l2` 第一条样本即抛 `Assert_failure`。
- 全部示例于 2026-11-20 用 OCaml 5.5.2 实测：clamp 五条测试通过；`clamp 10 0 5 = 10`（违反 `requires [lo <= hi]` 的调用无保证，借机讲解“测试用例必须在 requires 范围内”）；`-noassert` 编译后本该失败的强性质也未失败，验证 `assert` 可被整体关闭、只是开发期工具。
- 讲解要点顺序：测试与规格的关系（ensures 给出期望值）→ 单元测试与 `Assert_failure` → 按危险位置挑用例 → 黑盒（从规格推）vs 白盒（从分支推）及各自盲点 → 性质测试与“性质要够强” → 测试不能证明正确（为第 17 课归纳证明铺垫）。
- 习题更新（题组重编，旧 E16 版本废弃）：
  - E16-1 改为给 `minimum` 写测试（原版用 `count_positive`，与第 16 课讲义示例重复）。`minimum` 显式处理 `[]`（返回 `max_int`，已注明这是教学简化、真实写法应交由 `requires [lst <> []]` 排除），测试仍限非空列表。
  - E16-2 改为 `count` / `count_bug`（假实现恒返回 0）；给定的弱性质 `count x l <= List.length l` 实测对 `count_bug` 成立。
  - E16-3 改为 `remove`（黑盒 4 组输入 + 白盒分支 + “只删第一个出现”的假实现分析）。
  - R34 题面不变（`div` / `safe_div` 的 requires、`div 1 0` 行为、safe_div 是否需要 requires）。
- 参考代码实测：`minimum [3;1;4;1;5] = 1`、`minimum [2;2] = 2`、`minimum [-1;7] = -1`；`remove 1 [1;2;1;3] = [2;3]`、`remove 5 [1;2] = [1;2]`；`count_bug` 弱性质通过、强性质抛 `Assert_failure`。
- 文件变更：重写 `lessons/16-testing.md`（重讲版，标注实测环境 5.5.2，新增“测试与 requires 的关系”一节）；更新 `exercises/16-testing.md`（重讲日期与重编后的 E16-1 至 E16-3，R34 不变）。
- 注意：原 E16-1 版本（`count_positive`）从未被作答，本次替换不丢失任何用户作答记录。
- 下一步：等用户作答新 E16-1 至 E16-3 与 R34 后批改；之后进入第 17 课（8.7–8.9 正确性证明与结构归纳，并补 4.8 代数定律、3.12 自然数）。

## 70. 2026-11-20 批改第 16 课首次作答（E16-1/E16-2/E16-3·R34）

- 用户提交 E16-1 至 E16-3 与 R34 的首次作答。原文按 AGENTS.md 追加到 `exercises/16-testing.md`（用户作答记录一节），教师反馈放在其后，与原答案分开。
- 评分：E16-1 70/100（第 1 问 75、第 2 问 75、第 3 问 60）、E16-2 92/100（第 1 问 85、第 2 问 98、第 3 问 93）、R34 64/100（第 1 问 75、第 2 问 40、第 3 问 78）；E16-3 因教师题面 `requires` 错误，未评分，订正后等待重答。E16 总评待定。
- 实测核对（OCaml 5.5.2，`/tmp/l16g/`）：
  - 用户 E16-1 原码报 `File "./u1.ml", line 8 ... Error: Syntax error`（断言间缺 `;`）；补分号后 4 条全部通过；
  - `count_bug` 的弱性质在 `[]`、`[1]`、`[1;1;1]`、`[1;2;3]` 上全部通过；用户的 oracle 强性质在 `(1, [1])` 立刻失败；`count 1 [1;2;1;1] = 3`；
  - `min_int = -4611686018427387904`、`2 * min_int = 0`、`min_int - 1 = max_int`；`div 7 3 = 2`、`div (-5) 2 = -2`、`div 1 0` 抛 `Division_by_zero`；
  - `minimum` 的 `<`→`<=` 变异在所有样本上与正确实现同值，确认题面原提示不可观测。
- 主要问题：断言序列缺 `;`；测试越出 `requires`（`minimum []`）；`2 * min_int` 溢出理解；R34 第 2 问把“调用方违反 requires”判为“实现违反规格”。
- 已答对：E16-2 的 oracle 强性质与性质测试局限说明；R34 第 3 问 safe_div 无需 requires；E16-1 用例类别意识；E16-3 主动指出题面 requires 看不懂（正确）。
- 教师自查并订正两处题面错误：E16-3 的 `requires [(x, x) is not in [lst]]`（无意义，删除）；E16-1 第 2 问提示中的 `<=` 变异不可观测，改为 `<`→`>`。均在 review.md 记为教师错误（R37、R38）。
- 文件更新：`exercises/16-testing.md`（作答记录与反馈、两处题面订正）、`lessons/16-testing.md`（用例表与谓词边界说明修正）、`learning/review.md`（R34–R38）、`learning/progress.md`（断点与概念状态）。
- 下一步：等 E16-3 重答后批改并给出 E16 总评，再进第 17 课。

## 71. 2026-11-20 第 16 课收尾：E16-3 重答与 R34 改判

- 用户重答 E16-3 并对 R34 第 2 问提出澄清。作答原文已追加到 `exercises/16-testing.md`（第二次作答一节），反馈分开记录。
- E16-3 82/100（第 1 问 85、第 2 问 72、第 3 问 90）：四组黑盒用例（空表、x 不存在、出现一次、出现多次）选得好；白盒结论“无需补用例”在分支覆盖层面正确，但未给出分支→用例映射；第 3 问正确指出“只删第一个”会被黑盒抓到，并说明黑盒检查的是规格。
- 实测（OCaml 5.5.2，`/tmp/l16g2/`）：`remove 0 [0;0;1] = [1]`；`remove_first 0 [0;0;1] = [0;1]`（与期望 [1] 不符）；补充用例 `remove 2 [1;2;3] = [1;3]`、`remove 0 [1;0] = [1]`、`remove 0 [0;1;0] = [1]`、`remove 0 [0;0] = []`。
- R34 第 2 问改判 40 → 80，R34 修订 **78/100**：用户澄清原意即“调用方违反 requires”，教师把“违反规格了”误读为“实现违反规格”，属误读用户措辞（记录于 review.md R34 行）。仍缺异常名 `Division_by_zero` 与“实现不受责”表述。
- E16 总评 81/100（E16-1 70、E16-2 92、E16-3 82）；模块 5 的规格与测试部分完成，下一课进入 8.7–8.9 正确性证明与结构归纳。
- 新增 review 条目 R39（白盒缺分支→用例映射）。
- 已更新 `exercises/16-testing.md`、`learning/review.md`、`learning/progress.md`、`learning/journal.md`。
- 下一步：第 17 课（8.7–8.9 正确性证明与结构归纳，补 4.8 代数定律、3.12 自然数）。

## 72. 2026-11-20 第 17 课开讲（正确性与结构归纳）

- 用户要求开始第 17 课。对应 CS3110 第 8 章 8.7–8.9（章节对照沿用 2026-11-19 对 Fall 2026 官网目录的核对），补 4.8 代数定律与 3.12 自然数。
- 新建 `lessons/17-correctness-proofs.md` 与 `exercises/17-correctness-proofs.md`。
- 讲义内容：引用透明与代入推理三动作（展开/代换/算术事实）；列表结构归纳原理与合理性；完整证明 `length (append l1 l2) = length l1 + length l2`（说明归纳变量取 l1 的原因）；完整证明 `map f (append l1 l2) = append (map f l1) (map f l2)`；`type nat = Z | S of nat` 上 `add n Z = n` 的归纳证明，并对比 `add Z n = n` 不需归纳；常见错误表；证明与测试的分工。
- 定义与示例已用 OCaml 5.5.2 实测：`length (append [1;2] [3]) = 3`；`length (map (fun x -> x * 10) [1;2;3]) = 3`；`map (fun x -> x + 1) (append [1;2] [3;4]) = [2;3;4;5]`；`length (rev [1;2;3]) = 3`；`add (S (S Z)) Z = S (S Z)`；`add Z (S Z) = S Z`；`take 2 [1;2;3] = [1;2]`、`take 0 [1;2] = []`、`take (-1) [1;2] = []`、`take 5 [] = []`。
- 布置练习：E17-1 `length (map f l) = length l`；E17-2 map 对 append 的分配律；E17-3 `add n Z = n` 与 `add Z n` 不需归纳的理由；E17-4 用引理证 `length (rev l) = length l`；R40 复习白盒分支→用例映射与 requires 语义（`take`，`requires n >= 0`，第 3 问 `take (-1) [1;2]`）。
- 未记录任何作答；E17 与 R40 待提交。
- 已更新 `learning/progress.md`（断点、模块 5 状态、进度估算约 58%、第 8 章对照行、概念状态）与 `lessons/concept-map.md`（测试行完成、正确性论证行进行中）。
- 下一步：批改 E17-1 至 E17-4 与 R40；之后第 18A 课。

## 73. 2026-11-20 用户询问 IH 的含义

- 用户问“IH 是什么”。已答：IH = Induction Hypothesis（归纳假设），即归纳原理中“任取 t、假设 P(t)”的那个假设。
- 用第 17 课例 1 说明：归纳步要证 `length (append (h :: t) l2) = length (h :: t) + length l2`，IH 是 `length (append t l2) = length t + length l2`，证明链第 3 行标注 `(* IH *)`。
- 强调两点：IH 只针对尾部 `t`，不能假设 `h :: t`（循环论证）；多变量命题中 IH 只覆盖归纳变量，对 `l2` 归纳时 `append` 不拆 `l2`、归纳步推不动。
- 用户尚未作答 E17-1 至 E17-4 与 R40，记录保持待作答；概念状态不变。

## 74. 2026-11-23 用户询问“同态”的含义

- 用户问第 17 课例 2 中“map 是列表结构上的同态”是什么意思。已答：homomorphism = 保持结构的映射；一般定义为 `h e = d` 且 `h (a₁ ⊕ a₂) = h a₁ ⊗ h a₂`。
- 列表在 `@` 与 `[]` 下构成幺半群，`map f [] = []`、`map f (l1 @ l2) = map f l1 @ map f l2`，因此 `map f` 是幺半群同态；给出 `f x = x + 10` 的数值例子。
- 说明用途：分块并行 map、`map` 融合、简化证明；并对照 `rev (l1 @ l2) = rev l2 @ rev l1` 属反同态，为 E17-4 铺垫。澄清“同态”与“相同状态”无关；Haskell 文献的 list homomorphism 定义即 `h (x ++ y) = h x ++ h y`。
- 用户仍未提交 E17-1 至 E17-4 与 R40；跨日至 2026-11-23，未记录新的作答证据，概念状态不变。

## 75. 2026-11-23 用户询问 `add` 是否会无限递归

- 用户问 `type nat` 的 `add`：`S p -> S (add p m)` 中 `p` 是否仍是 `n`，是否不向 `Z` 靠近、导致无限递归。
- 已答：`S p` 是解构模式，`p` 是 `n` 去掉一层 `S` 后的前驱，不是 `n`；每次调用重新匹配自己的参数，结构逐层变小。
- 实测（OCaml 5.5.2，`/tmp/l17t/nat.ml`）：正确版本 `add (S (S (S Z))) (S Z)` 的实参 n 依次为 3、2、1、0，结果 4；故意写成 `S (add n m)`（传 `n`）的版本 n 恒为 3，4 步燃料耗尽。
- 强调 OCaml 的 `let rec` 不检查终止性，终止靠“递归作用在结构上更小的值”论证；与结构归纳中“归纳变量 = 递归消耗的结构”相呼应。
- 已在 `lessons/17-correctness-proofs.md` 第 5 节补“常见疑问”一段；`progress.md` 概念状态行补充该证据。
- 用户仍未提交 E17-1 至 E17-4 与 R40。

## 76. 2026-11-23 用户询问幺半群（monoid）

- 用户问“列表在拼接下构成一个幺半群”中 monoid 是什么。
- 已答三条件：封闭性、结合律、单位元；给出例子 `(int,+,0)`、`(int,*,1)`、`(list,@,[])`、`(string,^,"")`、`(nat,add,Z)`、函数复合、`(int,max,min_int)`；反例 `(int,-,0)`（实测 `(1-2)-3 = -4` 与 `1-(2-3) = 2`）与 `::`。
- 说明与 fold 的关系：单位元处理空列表、结合律使分组无关；实测 `fold_left (+) 0 [1;2;3]` 与 `fold_right (+) [1;2;3] 0` 都得 6，而 `-` 得 -6 与 2，与第 10、11 课的观察互相印证。另说明分块并行折叠的依据。
- 澄清幺半群不要求交换律（`@` 不交换）；同态即保持运算与单位元的映射，`map f` 是列表幺半群同态，`rev` 是反同态。
- 已在 `lessons/17-correctness-proofs.md` 第 4 节补幺半群定义；`progress.md` 新增“代数结构（幺半群、同态）”概念行。
- 用户仍未提交 E17-1 至 E17-4 与 R40。
