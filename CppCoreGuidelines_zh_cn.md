# <a name="main"></a>C++ 核心指南

此文根据[CppCoreGuidelines](https://github.com/isocpp/CppCoreGuidelines) 2019-03-07版本翻译

翻译时间：2019-04-05

编辑者:

* [Bjarne Stroustrup](http://www.stroustrup.com)
* [Herb Sutter](http://herbsutter.com/)

中文译者：

* [Flymachine](https://www.jianshu.com/u/73f96a1506f0)

这是一个持续丰富的活跃文档。
作为一份开源项目，它已经迭代到release 0.8版本了。
从这个项目复制、使用、修改和创建衍生均需遵循MIT协议。贡献此项目需要同意一份贡献者协议。查看同目录下的[LICENSE](LICENSE)文件了解详情。
我们使这个项目可供“友好用户”使用，复制，修改和派生，希望能够获取建设性的意见。

欢迎提出改进意见和建议。
我们计划修改和扩展此文档，因为我们的理解得到了提升，语言和可用库集(libraries)也得到了改进。
提出建议时，请注意[导言](#S-introduction)，它概述了我们的目标和一般方法。
贡献者列表在[这里](#SS-ack)。

问题集：

* 尚未完全检查规则集的完整性，一致性或可执行性。
* 三重问号(???)标记已知的缺失信息
* 更新参考部分；许多pre-C++11源代码太旧了。
* 对于或多或少的最新待办事项列表，请参阅：[待办事项：未分类的原则规则](#S-unclassified)

您可以[阅读本指南的范围和结构的解释](#S-abstract)或直接跳入：

* [In：Introduction 简介](#S-introduction)
* [P：Philosophy 信条](#S-philosophy)
* [I：Interfaces 接口](#S-interfaces)
* [F：Functions 函数](#S-functions)
* [C：Classes and class hierarchies 类和类层次结构](#S-class)
* [Enum：Enumerations 枚举](#S-enum)
* [R：Resource management 资源管理](#S-resource)
* [ES：Expressions and statements 表达式和语句](#S-expr)
* [Per：Performance 执行](#S-performance)
* [CP：Concurrency and parallelism 并发和并行](#S-concurrency)
* [E：Error handling 错误处理](#S-errors)
* [Con：Constants and immutability 常数和不变性](#S-const)
* [T：Templates and generic programming 模板和泛型编程](#S-templates)
* [CPL：C-style programming C风格编程](#S-cpl)
* [SF：Source files 源文件](#S-source)
* [SL：The Standard Library 标准库](#S-stdlib)

支持部分：

* [A：Architectural ideas 架构方法集](#S-A)
* [NR：Non-Rules and myths 无用的规则和杜撰故事集](#S-not)
* [RF：References 参考文献](#S-references)
* [Pro：Profiles 概况](#S-profile)
* [GSL：Guidelines support library 规范支持库](#S-gsl)
* [NL：Naming and layout rules 命名和布局规则](#S-naming)
* [FAQ： Answers to frequently asked questions 常见问题解答](#S-faq)
* [Appendix A：Libraries 库集](#S-libraries)
* [Appendix B：Modernizing code 现代化代码](#S-modernizing)
* [Appendix C：Discussion 讨论](#S-discussion)
* [Appendix D：Supporting tools 支持工具集](#S-tools)
* [Glossary 词汇表](#S-glossary)
* [To-do: Unclassified proto-rules 未分类的原则规则](#S-unclassified)

您可以为特定的语言功能采用规则：

* assignment（赋值）：
[regular types 常规类型](#Rc-regular) --
[prefer initialization 首选初始化](#Rc-initialize) --
[copy 复制](#Rc-copy-semantic) --
[move 移动](#Rc-move-semantic) --
[other operations 其他操作](#Rc匹配) --
[default 默认](#RC-eqdefault)
* `class`（类）：
[data 数据](#Rc-org) --
[invariant 不变式](#Rc-struct) --
[members 成员](#Rc-member) --
[helpers 助手](#Rc-helper) --
[concrete types 具体类型](#SS-混凝土) --
[ctors, =, and dtors 构造函数，=和析构函数](#S-ctor) --
[hierarchy 层次结构](#SS-hier) --
[operators 运算符](#SS-过载)
* `concept`（泛型约束）：
[rules 规则](#SS-concepts) --
[in generic programming 在泛型编程中](#Rt-raise) --
[template arguments 模板参数](#Rt-concepts) --
[semantics 语义](#RT-low)
* constructor（构造函数）：
[invariant 不变式](#Rc-struct) --
[establish invariant 建立不变式](#Rc-ctor) --
[`throw` 抛出](#Rc-throw) --
[default 默认](#Rc-default0) --
[not needed 不需要](#Rc-default) --
[`explicit` 显式](#Rc-explicit) --
[delegating 委托](#Rc-delegating) --
[`virtual` 虚](#RC-构造函数虚拟)
* derived `class`（派生类）：
[when to use 何时使用](#Rh-domain) --
[as interface 作为界面](#Rh-abstract) --
[destructors 析构函数](#Rh-dtor) --
[copy 拷贝](#Rh-copy) --
[getters and setters getter函数和setter函数](#Rh-get) --
[multiple inheritance 多重继承](#Rh-mi-interface) --
[overloading 重载](#Rh-using) --
[slicing 切割](#Rc-copy-virtual) --
[`dynamic_cast` 强制类型转换](#Rh的dynamic_cast的)
* destructor（析构函数）：
[and constructors 和构造函数](#Rc匹配) --
[when needed? 何时需要？](#Rc-dtor) --
[may not fail 可能不会失败](#Rc-dtor-fail)
* exception（异常）：
[errors 错误](#S-errors) --
[`throw` 抛出](#Re-throw) --
[for errors only 仅限错误](#Re-errors) --
[`noexcept` 不抛异常](#Re-noexcept) --
[minimize `try` 最小化`try`](#Re-catch) --
[what if no exceptions? 没有异常咋办？](#Re-no-throw-codes)
* `for` （循环）：
[range-for and for 范围`for`和一般`for`](#Res-for-range) --
[for and while `for`和`while`](#Res-for-while) --
[for-initializer `for`初始化](#Res-for-init) --
[empty body 空包体](#Res-empty) --
[loop variable 循环变量](#Res-loop-counter) --
[loop variable type ??? 循环变量类型???](#Res-???)
* function（函数）：
[naming 命名](#Rf-package) --
[single operation 单一操作](#Rf-logical) --
[no throw 不抛异常](#Rf-noexcept) --
[arguments 参数](#Rf-smart) --
[argument passing 参数传递](#Rf-conventional) --
[multiple return values 多个返回值](#Rf-out-multi) --
[pointers 指针](#Rf-return-ptr) --
[lambdas lambda表达式](#Rf-capture-vs-overload)
* `inline`（内联）：
[small functions 短函数](#Rf-inline) --
[in headers 在头文件中](#Rs-inline)
* initialization（初始化）：
[always 总是](#Res-always) --
[prefer `{}` 最好用花括号](#Res-list) --
[lambdas lambda表达式](#Res-lambda-init) --
[in-class initializers 类内初始化](#Rc-in-class-initializer) --
[class members 类成员](#Rc-initialize) --
[factory functions 工厂函数](#Rc-factory)
* lambda expression（lambda表达式）：
[when to use 何时使用](#SS-lambdas)
* operator（运算符）：
[conventional 常规](#Ro-conventional) --
[avoid conversion operators 避免转换运算符](#Ro-conversion) --
[and lambdas 和lambdas表达式](#Ro-lambda)
* `public`，`private`和`protected`（访问权限）：
[information hiding 信息隐藏](#Rc-private) --
[consistency 一致性](#Rh-public) --
[`protected` 保护](#Rh-protected)
* `static_assert`（静态断言）：
[compile-time checking 编译时检查](#Rp-compile-time) --
[and concepts 和泛型约束](#Rt-check-class)
* `struct`（结构）：
[for organizing data 用于组织数据](#Rc-org) --
[use if no invariant 如果没有不变式则使用](#Rc-struct) --
[no private members 没有私有成员](#Rc-class)
* `template`（模板）：
[abstraction 抽象](#Rt-raise) --
[containers 容器](#Rt-cont) --
[concepts 泛型约束](#Rt-concepts)
* `unsigned`（无符号）：
[and signed 和有符号](#Res-mix) --
[bit manipulation 位操作](#Res-unsigned)
* `virtual`（虚）：
[interfaces 接口](#Ri-abstract) --
[not `virtual` 不是'virtual'](#Rc-具体) --
[destructor 析构函数](#Rc-dtor-virtual) --
[never fail 永不失败](#Rc-dtor-fail)

您可以查看用于表达这些规则的设计概念：

* assertion 断言：???
* error 错误：???
* exception 异常：exception guarantee 异常保证(???)
* failure 失败：???
* invariant 不变式：???
* leak 泄漏： ???
* library 库： ???
* precondition 先决条件：???
* postcondition 后置条件：???
* resource 资源：???

# <a name="S-abstract"></a>Abstract 概括

本文档是一套很好地使用C++的指南。本文档的目的是帮助人们有效地使用现代（modern）C++。“modern C++”是指有效使用ISO C++标准（目前是C++17，但几乎所有的建议也适用于C++14和C++11）。换句话说，如果你能现在就开始，你希望你的代码在5年后看起来怎么样？10年后呢？

这份指南主要关注相对高级的问题，例如接口（interfaces），资源管理，内存管理和并发（concurrency）。这些规则会影响应用程序架构和库设计。遵循规则将使静态类型安全的代码不再有资源泄漏，并且捕获到比当今代码中常见的更多的编程逻辑错误。
而且它还会运行得更快————你可以做正确的事情。

我们不太关心低级问题，例如命名约定和缩进样式。
不过，只要是能帮助到程序员的主题，我们都会涉及。

我们的初始规则强调安全性（各种形式）和简洁性。
他们可能太过严格了。
我们希望能引入更多例外以更好地满足现实世界的需求。
我们还需要更多规则。

你会发现一些违背你期望的规则，甚至违背你的经验。
如果我们没有建议你以任何方式改变你的编码风格，我们就失败了！
请尝试验证或反驳规则！
特别是，我们真的希望通过测量或更好的示例来支持我们的一些规则。

你会发现一些显而易见甚至微不足道的规则。
请记住，指南的一个目的是帮助那些经验不足或来自不同背景或语言的人去加快学习速度。

许多规则特意设计成可以被一种分析工具支持。
违反规则（violations of rules）将用相关规则的引用（或链接）进行标记。
我们不期望您在尝试编写代码之前能记住所有规则。
思索这些指南的一种方式是作为为人类可读性设计的工具的规范。

这些规则旨在逐步引入代码库。
我们计划为此建立一些工具，并希望其他人也这样做。

欢迎提出改进意见和建议。
我们计划修改和扩展此文档，因为我们的理解得到了提升，语言和可用库集也得到了改进。

# <a name="S-introduction"></a>In: Introduction 导言

这是现代C++（目前为C++17）的一套核心指南，它考虑到了未来的增强功能和ISO技术规范（TSs）。目的是帮助C++程序员编写出更简洁，更高效，更易维护的代码。

导言摘要：

* [In.target: Target readership 目标读者](#SS-readers)
* [In.aims: Aims 目的](#SS-aims)
* [In.not: Non-aims 这些不是目的](#SS-non)
* [In.force: Enforcement 强制性](#SS-force)
* [In.struct: The structure of this document 这份文档的结构](#SS-struct)
* [In.sec: Major sections 主要章节](#SS-sec)

## <a name="SS-readers"></a>In.target: Target readership 目标读者

所有的C++程序员。这里包括[或许在考虑C的程序员](#S-cpl)。

## <a name="SS-aims"></a>In.aims: Aims 目的

本文档的目的是帮助开发人员去采用现代C++（目前为C++17）并在代码库中实现更统一的风格。

我们不会妄图这些规则中的每一个都可以有效地应用于每个代码库。升级旧系统很难。但是，我们确实认为使用规则的程序比不使用规则的程序更不容易出错且更易于维护。通常，规则也会导致更快/更容易的初期开发。
据我们所知，这些规则约束下的代码的表现与旧的，更传统的技术一样好或更好;它们意味着遵循零开销原则（“what you don't use, you don't pay for 不使用，不开销”或“当你适当地使用抽象机制时，你获得的性能就和你手写的较低级的语言结构代码一样良好）。在新代码上考虑这些规则；在旧代码上找机会利用这些规则；并尽可能地向这些规则靠拢。
记住：

### <a name="R0"></a>In.0: Don't panic! “不要恐慌”（译注：《银河系漫游指南》梗）

要花点时间了解指南规则对你的程序的影响。

这些指南是根据“subset of superset”（超集的子集）原则设计的([Stroustrup05](#Stroustrup05))。
它们不是简单地定义要使用的C++子集（比如可靠性，安全性，性能等）。
相反，它们强烈推荐使用一些简单的“extensions”（扩展）([library components 库组件](#S-gsl))，这些扩展使用C++冗余的最容易出错的特性，因此可以禁止它们（在我们的规则集中）。

这些规则强调静态类型安全和资源安全。因此，他们强调范围检查（range checking）的可能性，避免解引用`nullptr`，避免悬空指针，以及系统地使用异常（通过RAII）。
部分是为了实现这一目标，部分是为了最大限度地减少易混淆的代码作为错误根源，规则还强调简洁性和隐藏在指定好的接口背后的必要复杂性。

许多规则都是约定俗成的。
我们对那些简单地说“不要那样做！”却没有提供替代方案的规则感到不安。
其结果之一是某些规则只能通过启发式方法（heuristics）得到支持，而不是通过精确和机械可验证的检查。
其他规则阐明了一般原则。对于这些更一般的规则，更详细和具体的规则能提供部分检查。

这些指南解决了C++及其使用的核心问题。
我们觉得大多数大型组织，特定应用领域甚至大型项目都需要进一步的规则，可能还需要进一步的限制，以及进一步的库支持。
例如，硬件实时（hard-real-time）程序员通常不能自由使用自由存储区（动态内存），并且在他们可选的库里受到限制。我们鼓励制定更具体的规则作为这些核心准则的附录。
构建你自己的理想的小型基础库并使用它，远胜于降低你的编程水平以获得美化的汇编代码。

这些规则被设计成允许 [gradual adoption 逐步采用](#S-modernizing).

一些规则旨在增加各种形式的安全性，而其他规则旨在减少事故发生的可能性，许多规则则是两者皆有。
旨在预防事故的指导方针通常会禁止完全合法的C++。
然而，当有两种表达想法的方式，一种表现出共同的错误根源而另一种却没有，我们就会试图引导程序员走向后者。

## <a name="SS-non"></a>In.not: Non-aims 这些不是目的

这些规则不打算是最小的或正交的。
特别是，通则可以很简单，却不可执行。
此外，通常很难理解一个通则的含义。
更专业的规则通常更容易理解和执行，但不包含通则，它们只是一长串的特殊情况。
我们提供旨在帮助新手的规则以及支持专家使用的规则。
一些规则可以完全实施，但其他规则基于启发式。

这些规则并不意味着需要像书本一样连续阅读。
您可以使用链接浏览它们。
但是，它们的主要用途是被工具使用。
也就是说，某个工具会查找违规行为，并且该工具会返回违反的规则（violated rules）的链接。
然后，这些规则便提供了理由，违规的潜在后果的例子以及建议的补救措施。

这些指南并非旨在替代C++的基础教程。如果你需要一些给定经验水平的教程，请看[参考文献](#S-references)。

这不是如何将旧的C++代码转换为更现代的代码的指南。它旨在以具体的方式阐明新型代码的点子。
但是，请参阅[现代化部分]（＃s-modernizing）了解现代化/恢复/升级的一些可能方法。
重要的是，这些规则支持逐步采用：毕竟一口气完全转换大型代码库通常是不可行的。

这些指南并不意味着在每种语言技术细节上都是完整或准确的。
关于语言定义问题的最终解释，包括通例的每个例外和每个特性，请参阅ISO C++标准。

这些规则并不是为了强迫你在一个的贫瘠的C++子集下写代码。
它们只是**强调**并不意味着是定义一个，比方说，java样式的C++子集。
它们并不意味着是定义一个单一的“一个真正的C++”语言。
我们重视表现力和无法妥协的执行（We value expressiveness and uncompromised performance）。

这些规则不是价值中立的。
它们旨在使代码比大多数现有的C++代码更简洁，更正确/更安全，而不会降低性能。
它们旨在禁止那些与错误，虚假的复杂性和糟糕性能相关的完全有效的C++代码。

这些规则并不精确到一个人（或机器）可以盲目跟随它们的程度。
实施部分（The enforcement parts）试图做到这一点，但我们宁愿让规则或定义有点模糊，并且对解释持开放态度，而不是指出正确和错误的东西。
有时，精确度只来自时间和经验。
设计（还）不是一种数学形式。

这些规则并不完美。
某项规则可能会因为禁止了在特定情况下有用的东西而造成伤害（do harm）。
某项规则可能会因为未能禁止在特定情况下出现严重错误的内容而造成伤害。
某项规则可能会因为模糊，歧义，不可执行或使每个解决方案出现某个问题的方式造成很大的伤害。
不可能完全达到“不伤害”的标准。
相反，我们的目标是不那么雄心勃勃：“Do the most good for most programmers 为大多数程序员做最好的事情”;
如果你不能遵守规则，反对它，忽略它，但不要把它弄清楚，直到它变得毫无意义。
（if you cannot live with a rule, object to it, ignore it, but don't water it down until it becomes meaningless.）
同样，建议提交一个改进。

## <a name="SS-force"></a>In.force: Enforcement 强制性

对于大型代码库，不能强制执行的规则是无法管理的。
只有一套又少又弱的规则或者一个特定的用户社区才有可能强制执行全部规则。

* 但我们需要很多规则，而且我们想让每个人都可以使用这些规则。
* 但不同的人有不同的需求。
* 但是人们不喜欢阅读很多规则。
* 但人们无法记得很多规则。

所以，我们需要构造子集来满足各种需求。

* 但随心所欲的构造子集会导致混乱。

我们想要指南能够帮助很多人，使代码更加统一，并能强烈鼓励人们使代码现代化。我们希望鼓励最佳实践，而不是将所有实践留给个人选择和管理压力。
理想的是使用所有规则;这给了最大的好处。

这些想法综合起来相当困难。
我们尝试使用工具解决这些问题。
每条规则都有**执行（Enforcement）**部分列出执行方法。
执行可以通过代码审查，静态分析，编译器或运行时检查来完成。
在可能的情况下，我们更喜欢“机械”（mechanical）检查（人类很慢，不准确，容易厌倦）和静态检查。
在没有替代方案的情况下，很少建议运行时检查;我们不想介绍“碎片化的肥大”（distributed fat）。
在适当的情况下，我们使用相关规则组（称为“profiles”（配置））的名称标记规则（在**执行**部分中）。
规则可以从属于多个配置（profiles），也可以不是。
首先，我们有一些与常见需求（渴望，理想）相对应的配置（profiles）：

* **type(类型)**：无类型违规（通过转换操作符(casts)、联合变量(unions)或者可变参数(varargs)来把一个`T`重新解释为一个`U`）
* **bounds(边界)**：无边界违规（越界访问一个数组）
* **lifetime(生命周期)**：无泄漏（`delete`失败或重复`delete`），也无访问无效对象的情况（解引用`nullptr`，使用一个悬空引用）。

这些配置（profiles）旨在供工具使用，但也可以作为人类读者的帮助。
我们不会将**执行**部分中的意见限制在我们知道如何执行的内容中（We do not limit our comment in the **Enforcement** sections to things we know how to enforce）;
某些意见只是希望可能激发某些工具制造者的灵感。

实现这些规则的工具应遵循以下语法来明确禁止规则：

    [[gsl::suppress(tag)]]

"tag"处放得是这个强制规则显示的地方锚的名字（the anchor name）（例子：对[C.134](#Rh-public) 来说"tag"就是"Rh-public"）、规则组配置（profile）的名称 ("type", "bounds", or "lifetime")、或者是在某个配置（profile）中的一条特殊规则([type.4](#Pro-type-cstylecast)，或是[bounds.2](#Pro-bounds-arrayindex))。

## <a name="SS-struct"></a>In.struct: The structure of this document 这个文档的结构

每个规则（指南、建议）可以有以下几个部分：

* 这个规则本身————例子，**no naked `new` 不要赤裸裸地`new`**
* 一个规则引用编号————例子，**C.7**（和类(classes)有关系的第7条规则）。
  由于主要部分不是固有的排序，我们使用字母作为规则引用“编号”的第一部分。我们在编号中留下空白，以便在添加或删除规则时最大限度地减少“disruption”（中断）。
* **Reason 原因**集（理由）————因为程序员们发现遵循他们不理解的规则太难了
* **Example 样例**集————因为抽很难以理解；例子可以是正面的或反面的
* **Alternative 替代**集————为“不要这样做”的规则们准备的
* **Exception 例外**集————我们更喜欢简单的通则。但是，很多规则应用广泛，却不是处处适用的，所以必须列出其例外集。
* **Enforcement 强制**————关于如何可能地“机械”检查规则的想法
* **See also 参阅**集————参考相关规则和（或）进一步讨论（在本文件或其他地方）
* **Note 注意**集（注释集）————某些需要说但不适合其他分类的东西
* **Discussion 详述**————援引更广泛的在主要规则列表之外的基础理论和（或）示例

有些规则难以进行机械检查，但它们都符合任一专家程序员都可以毫不费力地发现许多违规行为的最低标准。
我们希望“机械”（mechanical）工具能够随着时间的推移而改进，以接近这样的专家程序员的检查能力(notices)。此外，我们假设规则将随着时间的推移而得到改进，以使其更加精确和可检查。

一条规则旨在简洁而不是为了去提及每一个替代和特殊情况而谨慎地措辞。
这些替代和特殊情况可以在**Alternative 替代**段落和[Discussion 详述](#S-discussion)部分找到。
如果你不理解或不同意某个规则，请访问它的**Discussion 详述**。
如果你觉得有某条论述遗漏了或不完整，请填写一条[Issue 问题](https://github.com/isocpp/CppCoreGuidelines/issues)解释你的担忧，然后请教(possibly)相应的公关(PR)。

这不是一本语言手册。
它旨在对技术细节或现有代码指南提供有用而非完全、充足的准确性。
推荐的信息资源可以在[the references 参考文献](#S-references)中找到。

## <a name="SS-sec"></a>In.sec: Major sections 主要章节

* [In: Introduction 简介](#S-introduction)
* [P: Philosophy 信条](#S-philosophy)
* [I: Interfaces 接口](#S-interfaces)
* [F: Functions 函数](#S-functions)
* [C: Classes and class hierarchies 类和类层次结构](#S-class)
* [Enum: Enumerations 枚举](#S-enum)
* [R: Resource management 资源管理](#S-resource)
* [ES: Expressions and statements 表达式和语句](#S-expr)
* [Per: Performance 执行](#S-performance)
* [CP: Concurrency and parallelism 并发和并行](#S-concurrency)
* [E: Error handling 错误处理](#S-errors)
* [Con: Constants and immutability](#S-const)
* [T: Templates and generic programming 模板和泛型编程](#S-templates)
* [CPL: C-style programming C风格编程](#S-cpl)
* [SF: Source files 源文件](#S-source)
* [SL: The Standard Library 标准库](#S-stdlib)

支持部分：

* [A: Architectural ideas 架构方法集](#S-A)
* [NR: Non-Rules and myths 无用的规则和杜撰故事集](#S-not)
* [RF: References 参考文献](#S-references)
* [Pro: Profiles 概况](#S-profile)
* [GSL: Guidelines support library 规范支持库](#S-gsl)
* [NL: Naming and layout rules 命名和布局规则](#S-naming)
* [FAQ: Answers to frequently asked questions 常见问题解答](#S-faq)
* [Appendix A: Libraries 库集](#S-libraries)
* [Appendix B: Modernizing code 现代化代码](#S-modernizing)
* [Appendix C: Discussion 讨论](#S-discussion)
* [Appendix D: Supporting tools 支持工具集](#S-tools)
* [Glossary 词汇表](#S-glossary)
* [To-do: Unclassified proto-rules 未分类的原则规则](#S-unclassified)

这些章节并不是正交的（orthogonal）。

每个部分（例如，“P”表示“Philosophy 哲学”）和每个子部分（例如，“C.hier”表示“Class Hierarchies 类层次结构（OOP）”）具有易于搜索和引用的缩写。主要部分缩写也用于规则编号（例如，“C.11”用于“Make concrete types regular 使具体类型规则化”）。

# <a name="S-philosophy"></a>P: Philosophy 信条

在此章节的规则都非常普适。

信条（Philosophy）规则摘要：

* [P.1: Express ideas directly in code 直接在代码中表达想法](#Rp-direct)
* [P.2: Write in ISO Standard C++ 依照ISO标准C++写代码](#Rp-Cplusplus)
* [P.3: Express intent](#Rp-what)
* [P.4: Ideally, a program should be statically type safe](#Rp-typesafe)
* [P.5: Prefer compile-time checking to run-time checking](#Rp-compile-time)
* [P.6: What cannot be checked at compile time should be checkable at run time](#Rp-run-time)
* [P.7: Catch run-time errors early](#Rp-early)
* [P.8: Don't leak any resources](#Rp-leak)
* [P.9: Don't waste time or space](#Rp-waste)
* [P.10: Prefer immutable data to mutable data](#Rp-mutable)
* [P.11: Encapsulate messy constructs, rather than spreading through the code](#Rp-library)
* [P.12: Use supporting tools as appropriate](#Rp-tools)
* [P.13: Use support libraries as appropriate](#Rp-lib)

（译者注：暂时翻译到这里，剩下的请等容我慢慢翻译）
