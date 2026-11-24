# OCaml 学习空间

从 2026-11-11 开始，以 Cornell CS3110 的《OCaml Programming: Correct + Efficient + Beautiful》为主线，使用中文逐步学习。

教材入口：`https://cs3110.github.io/textbook/index.html`（Fall 2026 版）

## 从这里继续

- [学习进度与续课断点](learning/progress.md)
- [逐次学习记录](learning/journal.md)
- [错题与复习清单](learning/review.md)
- [第 1 课：表达式、值和类型](lessons/01-expressions.md)
- [第 1 课习题与作答区](exercises/01-expressions.md)
- [第 2 课：if 表达式](lessons/02-if-expressions.md)
- [第 2 课习题与作答区](exercises/02-if-expressions.md)

可以直接在聊天中回答习题，也可以编辑习题文件。老师会根据实际作答更新记录。下次在这个目录说“继续学习 OCaml”，从进度文件中的断点继续。

## 学习路线

以下是教学安排，实际速度根据练习表现调整，不预设完成日期。章节对应关系依据 2026-11-19 核对的 Fall 2026 官网目录；完整章节状态与补讲安排见[学习进度](learning/progress.md)和[函数式编程概念地图](lessons/concept-map.md)。

| 阶段 | 教材位置 | 学习内容 |
| --- | --- | --- |
| 函数式基础 | 第 1–2 章 | 不变性、抽象、类型系统、OCaml 工具链、表达式、函数与递归 |
| 数据建模 | 第 3 章 | 列表、模式匹配、记录、变体、option、异常与测试 |
| 高阶编程 | 第 4 章 | 高阶函数、map/filter/fold、管道、柯里化与代数定律 |
| 模块化 | 第 5 章 | 模块系统、签名、抽象数据类型、函子、编译单元与模块组合 |
| 状态与并发 | 第 6–7 章 | refs、可变字段、数组、循环、promises、回调、monads、Applicative 对照与 Lwt |
| 正确性 | 第 8 章 | 规格、文档、测试、性质测试、结构归纳与等式推理 |
| 数据结构 | 第 9 章 | 哈希表、均摊分析、红黑树、序列、记忆化、持久数组与 2-3 树 |
| 语言实现 | 第 10 章 | 解析、解释器、替换/环境模型、类型检查与推断 |
| 选修 | Lagniappe、附录及自编专题 | Curry–Howard、Big-Oh、虚拟机；first-class modules、CPS、GADTs、effect handlers |

练习优先选用教材习题，配合自编题；先独立作答，再根据回答反馈。未覆盖的小节会列入进度文件的补讲安排。

## 当前环境

环境于 2026-11-20 升级到 OCaml 5.5.2。新环境由 opam switch `ocaml-5.5.2` 管理，旧版本已按要求移除。

在新的 shell 中先执行：

```bash
source /home/lwtang/ask/ocaml-env.sh
ocaml
```

当前已安装 OCaml 5.5.2、opam 2.6.1、Dune 3.25.0、utop 2.18.0、ocamlfind 1.9.9~preview、Lwt 6.1.2。OUnit 和 QCheck 尚未安装，相关课程需要时再安装或使用自写测试驱动。

目录原有 `ex_cap1.ml`，含日期判断和 Fibonacci 练习，保留作后续讨论材料。其存在不代表这些概念已经掌握。
