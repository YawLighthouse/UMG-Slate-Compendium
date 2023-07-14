<a name="page-top"></a>

# UMG-Slate 纲要

通过[GitLocalize](https://gitlocalize.com/)本地化（请阅读有关成为翻译者的[常见问题解答](FAQ.md)页面！）

<a name="document-version"></a>

###### 文件版本

<!---Major.Minor.Patch--->

*0.6.0*

<!---
Section naming:
Major.Minor.Mini

For creating diagrams I use lucidchart, it has a nice user interface and style for both creating charts and exporting(and it has a good free tier).

For the back to top arrow:
**[<span>&#11014;</span> Back to Top](#table-of-contents)**
Place this at the end of major sections and minor sections should only have it if its after 5+ mini sections for UX purposes.
Mini sections should only have it if its not the last section in the major and only on the last mini section.

UE5 Editor Background Color:
- Hex Linear: 040404FF
- Hex sRGB: 242424FF
- Value: 0.017642
- Red: 0.017642 OR 24.0
- Green: 0.017642 OR 24.0
- Blue: 0.017642 OR 24.0
- Alpha: 1.0

To add a fake indent, this adds a bunch of forced spaces(I hate this, would be nice to have a simple "&tab" or something):
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

Some rules regarding linking table of contents information:
- "--" is used for symbols like %, $, #, @, &
- Adding a new main category requires you to update the numbers so for the link we don't include the number just to save needless effort

This document uses "split screen" and not "split-screen"(excluding HTML linkage) or "splitscreen".

For categories we don't include the words in parenthesis just to keep it short and reasonable.

HTML Notes:
- <tr></tr> This will end the line horizontally and start a new line vertically.
- <td></td> Will end the line vertically and start a new line horizontally.
- Use <i></i> To italicize text.
- Use <b></b> To bold text.
- <a href="LINK" target="_blank">DISPLAY TEXT</a> For links in HTML. `target="_blank"` will make a new tab/window when link is selected.
- <br> for line breaks.
- <ul><li></li></ul> for non-ordered lists.
- <ol><li></li></ol> for ordered numbered lists.

--->

<a name="repo-page-links"></a>

## 仓库页面链接

> - [常见问题页面](FAQ.md)
> - [外部链接页面](EXTERNAL_LINKS.md)

<a name="table-of-contents"></a>

## 目录

> 1.0 [简介](#introduction) <br>2.0 [性能和设计考虑](#performance--design-considerations)<br>2.1 [CPU注意事项](#cpu-considerations)<br>2.1.1[动画性能](#perf-animations)<br>2.1.2 [Widget 组件的性能](#perf-widget-components)<br>2.2 [GPU注意事项](#gpu-considerations)<br>3.0 [虚幻中的 Slate 和 UMG](#slate--umg-in-unreal)<br> 4.0 [Slate](#slate)<br>4.1 [Slate Widget投射和存储](#slate-widget-casting--storing)<br>4.2 [Slate单位和DPI缩放](#slate-units--dpi-scaling)<br>4.3 [Slate用户](#slate-users)<br>4.4 [Widget布局](#widget-layout)<br>4.5 [关于油漆](#on-paint)<br>4.6 [Widget勾选](#widget-ticking)<br>4.7 [Widget 层次结构](#widget-hierarchy)<br>4.8 [无效](#invalidation)<br>4.9 [Slate属性和事件](#slate-attributes-and-events)<br>4.10 [Slate Widget示例（Slate 测试套件/Starship）](#slate-widget-examples)<br> 4.11 [Slate有用的控制台命令](#helpful-console-commands-for-slate)<br>5.0 [UMG（虚幻动态图形）](#umg)<br> 5.1 [User Widget层次结构](#user-widget-hierarchy)<br>5.2 [User Widget动画](#user-widget-animation)<br>5.3 [User Widget事件](#user-widget-events)<br>6.0 [常用Widgets](#common-widgets)<br>7.0 [常用Widget功能](#common-widget-functionality)<br>8.0 [UMG与关卡/世界的关系](#umg-in-relation-to-levels-worlds)<br>8.1 [HUD参与者](#hud-actors)<br>8.1.1 [HUD绘图](#hud-drawing)<br>8.1.2 [HUD碰撞盒](#hud-hitboxes)<br>8.2 [Widget组件](#widget-components)<br>8.2.1 [Widget交互组件](#widget-interaction-components)<br>8.2.2 [Widget组件如何渲染](#widget-components-rendering)<br>9.0 [UMG/Slate 开发和调试工具](#dev-debug-tools)<br>9.1 [调试控制台命令](#debug-console-commands)<br>9.2 [Wideget 反射器](#widget-reflector)<br>9.3 [Slate 控制台调试器](#slate-console-debugger)<br>10.0 [Unreal Engine输入框架（涉及UMG/Slate）](#input-framework-of-unreal-engine)<br> 10. [虚幻引擎的输入流程](#input-flow-of-unreal-engine)<br>10.2 [输入组件](#input-components)<br>10.3 [输入事件类型](#input-event-types)<br>10.4 [输入模式](#input-modes)<br>11.0 [Unreal的聚焦系统](#unreals-focusing-system)<br>11.1 [导航网格说明](#navigation-grid-explanation)<br>11.2 [导航起源](#navigation-genesis)<br>12.0 [分屏](#split-screen)<br>12.1 [游戏层管理器](#game-layer-manager)<br>12.2 [视口布局](#viewport-layout)<br>12.3 [本地玩家](#local-players)<br>12.3.1 [手柄ID（控制器ID）](#gamepad-id)<br> 13. [重要文件名](#important-file-names)

<a name="introduction"></a>

## 1.0 简介

本纲要旨在教授 UMG 和 Slate 的基础知识，并提供对如何使用虚幻引擎的 UI 框架的基本了解。如果本文档有任何需要改进的地方，请告诉我，因为这旨在帮助社区！

如果您对本文档（或与本文档相关的任何内容）有其他疑问，请参阅常见问题[解答页面](FAQ.md)（也链接在文档顶部）。

> 关于编程技能水平及其在本文档中的使用方式的重要说明：通常，当有人将一个概念称为*“高水平”*时，他们的意思是它非常复杂，需要高技能水平或大量经验，**但**在编程中，它翻转了; *“高级”*概念实际上非常容易使用，并且是一个简单的概念，几乎没有经验，而*“低级”*概念越多，概念就变得越复杂，需要更多的经验。例如; **Blueprint**是一种*“高级”*编码语言， **C++**是一种*“低级”*编码语言。
>
> GeeksForGeeks 有一篇很棒而简单的文章解释了其中的区别：
>  *<u>https://www.geeksforgeeks.org/difference- Between-high-level-and-low-level-languages/</u>*

> 关于两种类型的虚幻引擎的重要说明：
>
> - **启动器版本**：您将从 Epic Games 启动器下载的引擎版本；该引擎公开可见，但无法修改，并且无法通过 GitHub 向 Epic 提交代码更改。
> - **源代码版本**：您将从 GitHub 克隆/下载的引擎版本；这提供了对虚幻引擎的完全可修改的访问，并能够通过 GitHub Pull 请求向 Epic 提交代码更改，以便将它们集成到虚幻引擎的未来版本中。

*本文档中编辑器的所有图片均来自引擎的启动器版本。*

本纲要涵盖的内容：

- **性能和设计考虑因素**
- **Slate框架**
- **UMG框架**
- **Unreal Engine的输入框架（涉及UMG/Slate）**
- **内置对焦系统的工作原理**
- **分屏通常如何工作**

它**并不是**为了教初学者如何一般性地使用虚幻引擎，而是为了教初学者如何使用虚幻引擎，而只是教初学者如何使用虚幻引擎的特定方面，其中包含了引擎的很多内容。

本纲要要求您对虚幻引擎的这些领域有基本的了解：

- **如何使用Blueprint和Unreal的C++**
- **虚幻引擎的垃圾收集框架**
- **虚幻引擎的游戏框架**

本纲要将包含取自官方文档的信息，但旨在阐明一些官方文档的含义，但它并不是对已经解释过的内容的重新定义。

*文档底部是有用的链接和文件列表，供您参考
（您可以使用文件顶部的[外部链接页面](EXTERNAL_LINKS.md)链接轻松访问它们）。*

**[<span>⬆</span>返回顶部](#table-of-contents)**
<a name="performance--design-considerations"></a>

## 2.0 性能和设计考虑

[Epic 关于 Slate 和 UMG 性能和优化指南的官方文档](https://docs.unrealengine.com/latest/INT/optimization-guidelines-for-umg-in-unreal-engine/)

**UMG**和**Slate**是渲染 (GPU) 和计算 (CPU) 方面性能极高的框架。但是在构建 UI 时应该考虑这两方面，以便为用户提供最佳体验。

> 这不是“如何提高性能”指南，这些是使用 UMG/Slate 构建应用程序时需要注意的注意事项以及帮助提高性能的可能途径。

UI 复杂性通常与活动和可见Widget的数量有关（因此屏幕外widget不会tick，如果不在屏幕上，则不应运行功能）。 UI 的常见技术设计是对游戏代码做出反应（如果您的游戏不依赖于 UI，也建议这样做），但不驱动游戏代码，因此它是高性能且可重用的，通常情况下 UI 可以是执行游戏代码的入口点，但它应该监听游戏代码如何响应并做出反应。

<a name="cpu-considerations"></a>

### 2.1 CPU注意事项

Slate/UMG 使用[失效](#invalidation)和缓存的概念，而不是不断轮询数据。原因是有时轮询要么性能不佳，要么功能不正确（例如，使用多线程代码时）。通常，Unreal 中的高性能 UI 旨在**基于事件**。 UMG 具有**属性绑定**，但不应使用它们，因为它们将在每一帧进行更新，这比使用基于事件的框架对 CPU 的负担要大得多。

**CPU技术设计场景**
:-:
在 RTS 中，你需要有一个 UI 标记来查看在世界上放置一队士兵的位置，<br>玩家在世界中选择一个有效的位置来放置 UI 标记，然后部队将移动到该位置。

**好例子** | **坏榜样**
:-: | :-:
玩家管理器对象创建 UI 标记widget并管理将部队移动到哪里。<br>玩家选择的世界位置被馈送到 UI 标记的widget（来自玩家管理器对象），并且widget本身负责在屏幕空间中定位自身。 | 玩家选择一个世界位置并将其直接提供给 UI 标记，然后 UI 标记直接告诉部队移动到哪里并处理他们的移动。

<a name="perf-animations"></a>

#### 2.1.1 动画性能

通过对大量Widget进行动画处理来使多个Widget的所需尺寸无效可能会产生下游影响，即花费大量 CPU 时间重新计算[Widget布局](#widget-layout)。

- 在虚幻引擎的早期版本中，当使用 UMG Widget播放动画时，它们将与 Sequencer 分开（在 Sequencers 发布后，它很快更新为使用 Sequencer 作为底层动画系统）。
- 在 Sequencer 更新后的虚幻引擎早期版本中，UMG widget在动画时将被设置为易失性。
- 在虚幻引擎 UMG 的当前版本中，动画处理时widget不会切换到易失性。

为什么我应该使用易失性？

> 老实说，这并不是每次的答案，当您为 UMG widget设置动画或更改其视觉状态时会发生什么，它将使该帧的widget无效并重新缓存其新状态，直到稍后再次无效。将其设置为易失性不会使widget失效或缓存，它会为易失性widget通过单独的代码路径，这是提高性能的具体情况的基础。使用易失性的一些途径是将其打开一段时间，然后在不再需要它处于易失性时将其关闭。

- 关于使用 volatile 的一些提醒：
    - 它不缓存widget状态，它在屏幕上的每一帧都会被轮询。
    - 它往往会影响Widget层次结构，因此请注意对父Widget和子Widget的下游影响。

<a name="perf-widget-components"></a>

#### 2.1.2 Widget 组件的性能

Widget 组件非常适合原型设计和现实 VR/AR UI。当试图让 UI 处于现实世界中，同时仍然尊重屏幕尺寸、逻辑控制布局等时，它们并不是很好。<br>通常，如果您有可用的纹理预算，则最好使用Widget组件，因为每个widget组件实际上都是一个静态网格平面，渲染目标纹理应用于其材质槽零。<br>每个widget组件将创建 UMG Widget对象，然后将其渲染为纹理以将其显示在世界中。

由于纹理内存的使用，建议不要大量使用 Widget 组件（不包括 VR/AR）。另一种选择（更复杂）是创建一个自定义 Slate Widget 来处理渲染多个“Widget”，您可以使用`SMeshWidget`来完成此操作，每个Widget 1 次绘制调用，但它需要熟练的技术艺术技能才能绘制多个Widget元素作为 1 个纹理。

什么是`SMeshWidget` ？<br>它是一个具有 1 个绘制调用渲染能力的widget，在 Paragon 中使用它来绘制屏幕上的每个状态栏和图标。它非常强大，但也需要很好地理解如何在代码中绘制这些元素，这就是为什么你看不到它经常使用的原因，这不仅仅限于纹理或粒子，而是可以与 3D 模型/网格或任何你想要渲染的东西。

Nick Darnell 实际上整理了一个在 UE4 中使用`SMeshWidget`绘制粒子的示例：

- [论坛链接](https://forums.unrealengine.com/t/smeshwidget-hardware-instanced-slate-meshes-thread/58020/5)
- [Dan Treble 非常友善地将示例项目变成了 Github 仓库](https://github.com/dantreble/MeshWidgetExample)

Carey Hickling 发表了 Unreal Fest 演讲“为 AAA 游戏优化和构建 UI”，并讨论了`SMeshWidget`的优点/缺点： [YouTube 链接](https://youtu.be/OyY3OYbNK7s)

对于另一个可能的途径，建议查看 Epic 的 Lyra 示例项目和指标系统以及该项目中的相关文件：

- `SActorCanvas` /.cpp
- `IndicatorLayer` .h/.cpp
- `IActorIndicatorWidget` /.cpp
- `IndicatorDescription` .h/.cpp
- `IndicatorLibrary` .h./cpp
- `LyraIndicatorManagerComponent` .h/.cpp

<a name="gpu-considerations"></a>

### 2.2 GPU注意事项

在 3D 空间中工作时，您需要担心的大多数 GPU 一般注意事项都是相同的，例如：

- **纹理内存大小**
- **材质着色器的计算复杂度**
- **绘图调用**

如果您的框架构建正确，那么当涉及到 Slate/UMG 时，GPU 优化不会成为问题，因为它使用了一些 Unreal 的 3D 渲染功能来统一一些工作。

虚幻引擎的一个重要问题是它没有根据平台类型在不同纹理之间进行交换的可靠方法。
例如;从移动设备切换到 PC，您将必须自己构建这些系统，以便未使用的纹理不会被烘焙、加载、
引用，甚至存储在您不希望它们所在的平台上的硬盘上。

**[<span>⬆</span>返回顶部](#table-of-contents)**
<a name="slate--umg-in-unreal"></a>

## 3.0 虚幻中的 Slate 和 UMG

虚幻引擎使用**Slate** ；其定制的**UI 编程框架**。它可用于使用 Slate Widget 的游戏和非游戏应用程序。
一些例子包括：

- 虚幻编辑器完全使用 Slate 构建。
- （感谢 Nick Darnell 的解释）Epic Games Launcher 使用 Slate 作为框架并显示 Chromium 应用程序。
- Halo Master Chief Collection 使用 UMG/Slate（TBD）作为其 UI（这是基于公开公告而不是内部知识）。

**Slate**完全仅在使用声明性语法的 C++ 中使用，并且**<u>不是</u>**从`UObject`层次结构派生的。

Slate的优点：

- **完全使用 C++ 可以提​​高性能**
- **可用于通用应用程序**（例如 Epic Game 的启动器和虚幻编辑器本身完全是用 Slate 构建的）
- **用于扩展编辑器**（这是因为虚幻编辑器首先是从 Slate 构建的）
- **不与虚幻的垃圾收集系统一起使用**

Slate的缺点：

- **完全使用 C++** （没有视觉设计器，这才是 UMG Designer 的真正用途）
- **无法拥有sequencer动画**（一切都是硬编码的，因此迭代时间更长）
- **需要全面的知识才能将其用于全面生产**
- **不能与蓝图一起使用**
- **不与虚幻的反射系统一起使用，这意味着它不能与虚幻的垃圾收集系统一起使用**

![星舰画廊示例](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/starship_gallery.png?raw=true)<br> *Slate 测试套件（在 UE5 中称为 Starship Gallery）*

**UMG** （**虚幻动态图形**）源自`UObject`层次结构，但不是`Actor`的层次结构，用于创建和显示 Slate Widget（基本上是虚幻引擎友好的包装器，用于 Slate 与蓝图一起使用）。

UMG的优点：

- **可与蓝图和 C++ 一起使用**
- **允许艺术家驱动的动画**
- **拥有视觉设计师**
- **与虚幻的反射系统一起使用，意味着它与虚幻的垃圾收集系统一起使用**
- **能够扩展编辑器**（虚幻引擎 4.23+ 使用编辑器Utility Widgets）

UMG的缺点：

- **仅限于虚幻引擎**（无法在常规操作系统应用程序的引擎之外使用）
- **易用性还会导致开发人员承担技术债务并导致 UI 变得混乱**

![UMG 设计器示例](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/umg_designer.png?raw=true)

**[<span>⬆</span>返回顶部](#table-of-contents)**
<a name="slate"></a>

## 4.0  Slate

Slate 框架有两个核心部分：

- **Slate渲染器**
    - Slate Widgets 使用自己的渲染器（称为 Slate Renderer）显示，它处理游戏视口前面的 UI 元素渲染，并与 Unreal 的世界渲染管道结合来显示 UI 元素。
- **Slate 应用**
    - Slate 应用程序是一个管理 Slate Widgets 的所有 CPU 相关任务的对象，例如它们的视口位置、用户焦点导航、Widget层次结构放置和跟踪、接收输入并将输入路由到 Slate Widgets 或游戏引擎的其余部分（是的，这是在 UI 元素获取原始输入之前接收原始输入的地方）并注册（创建）/跟踪 Slate 用户。

<a name="slate-widget-casting--storing"></a>

### 4.1 Slate Widget 投射和存储

由于**Slate Widgets****<u>不是</u>**从`UObject`派生的，这意味着您无法使用 Unreal 的对象转换系统来获取特定的 Slate 类型，也无法利用`UObject`的垃圾回收。相反，Slate 使用 Unreal 的共享指针（也称为智能指针）框架来管理 Widget 的内存。

共有三种共享指针模板类型：

- `TSharedRef` ：不可空、引用计数、非侵入式权威智能指针。
- `TSharedPtr` ：引用计数、非侵入式权威智能指针。
- `TWeakPtr` ：引用计数、非侵入式弱指针引用。

> 一个重要的注意事项是，当共享引用的所有引用计数都消失时；该对象将被删除，并且一个限制是它只会调用默认析构函数，因此带有参数的自定义析构函数不可用。

使用共享指针允许您取得这些 slate widget的所有权，而不必对它们调用删除。<br>*我建议您查看[Epic 的智能指针文档](https://docs.unrealengine.com/5.0/en-US/smart-pointers-in-unreal-engine/)*

> 需要注意的是，在转换 Slate Widget 时，使用`static_cast`和`const_cast`等标准转换是完全可以的，但我建议使用下面列出的 Unreal 模板化转换版本来节省时间。

还有一些辅助类和函数可以让使用 Unreal 的共享指针框架变得更加轻松，其中包括使用 Slate。

- `MakeShareable` ：用于从 C++ 指针初始化共享指针。
    ```c++
    TSharedPtr<FMyCustomCalculator> calculator = MakeShareable(new FMyCustomCalculator());
    ```
- `TSharedFromThis` ：与自定义 C++ 类一起使用，作为您继承的对象，以允许它与 Unreal 的共享指针框架一起使用。
    ```c++
    /** Special class to handle custom calculations */
    class FMyCustomCalculator : public TSharedFromThis<FMyCustomCalculator>
    {
    public:
      FMyCustomCalculator()
      { }
      // ...
    };
    ```
- `StaticCastSharedRef` ：静态转换实用函数，通常用于在获取智能引用时向下转换为派生类型。
    ```c++
    void MyFunction(FMySubCalculator& CalculatorItem)
    {
      TWeakPtr<FMyCustomCalculator> calculator = StaticCastSharedRef<FMyCustomCalculator>(CalculatorItem.AsShared());
      // ...
    }
    ```
- `ConstCastSharedRef` ：将`const`引用转换为`mutable`智能引用。
    ```c++
    void MyFunction(TSharedRef<const FMyCustomCalculator> CalculatorItem)
    {
      TSharedRef<FMyCustomCalculator> calculator = ConstCastSharedRef<FMyCustomCalculator>(CalculatorItem);
      // ...
    }
    ```
- `StaticCastSharedPtr` ：动态转换实用函数，通常用于向下转换为派生类型。这就是您将使用的而不是`static_cast` （它不是必需的，但需要更少的输入）。
    ```c++
    void MyFunction(TWeakPtr<FMyCustomCalculator> CalculatorItem)
    {
      TSharedPtr<FMyCustomCalculator> calculator = StaticCastSharedPtr<FMyCustomCalculator>(CalculatorItem.Pin());
      // ...
    }
    ```
- `ConstCastSharedPtr` ：将`const`智能指针转换为`mutable`智能指针。这就是您将使用的而不是`const_cast<>` （它不是必需的，但需要更少的输入）。
    ```c++
    void MyFunction(TSharedPtr<const FMyCustomCalculator> CalculatorItem)
    {
      TWeakPtr<FMyCustomCalculator> calculator = ConstCastSharedPtr<FMyCustomCalculator>(CalculatorItem);
      // ...
    }
    ```

有关更多信息，请参阅这些文件：

- 使用不同实用程序函数和模板的示例： `Engine/Source/Runtime/Core/Public/Templates/SharedPointerTesting.ini`
- 虚幻引擎的共享指针系统说明： `Engine/Source/Runtime/Core/Public/Templates/SharedPointer.h`

<a name="slate-units--dpi-scaling"></a>

### 4.2 Slate 单位和 DPI 缩放

- **Slate Units** ：Unreal 制作独立于像素密度的 UI 的方式，因此您的应用程序可以轻松支持多个平台。这使得它更加精确并且独立于用户显示器的像素密度。单个Slate单元的物理尺寸可能有所不同，但默认情况下，每个Slate单元可以方便地设置为 1 像素。设置默认值；建议改为调整基本 DPI 缩放比例。
- **DPI 缩放**：Unreal 如何在运行时通过按此值缩放每个Slate单位来修改Slate单位转换，例如，如果您将Slate单位设置为 1 单位 = 1 像素，并且 dpi 缩放设置为值 2.5那么每个Slate单位将为 1 单位 = 2.5 像素。您可以通过“引擎-用户界面”类别下的项目设置中的曲线表根据分辨率更改 DPI 缩放比例。

Epic 自己承认它并不完美，但它适用于大多数用例。
 [Epic 的 AnswerHub 解释 Slate 单位](https://forums.unrealengine.com/t/what-are-slate-units/310703)

<a name="slate-users"></a>

### 4.3 Slate用户

**Slate 用户**是表示本地个人输入提供用户的类（例如，在 3 名玩家的分屏合作游戏中，则有 3 个 Slate 用户，但在有 32 名玩家且没有分屏的在线游戏中，则只有本地用户）玩家是该设备上唯一的 Slate 用户）。每个**平台的 SDK**将告诉**Slate 应用程序**在添加新连接时（例如，插入新控制器时）注册（创建）新的 Slate 用户。添加新连接时，会创建一个新的 Slate 用户，但在删除连接时不会创建新的 Slate 用户，以防止控制器意外断开连接（保留该控制器的设置，以防它们重新连接）。当连接被删除时，该 Slate 用户不会更新。 Slate User 实例跟踪用户当前关注的Widget，并控制光标/具有指针信息以考虑手势（这仅适用于第一个 Slate User，因为您无法插入多个鼠标，并且如果您是……为什么？）。

您可以通过 2 种主要方式访问 Slate 用户：

- 从 Slate 应用程序使用该本地 Slate 用户的整数索引。
- 来自`ULocalPlayer`对象，您可以从任何播放器控制器获取该对象。

> 需要注意的重要一点是，本地玩家可以在玩家控制器生成之前存在（玩家控制器中有一些函数，当设置本地玩家时您可以覆盖），并且本地玩家存在于级别之间，而不是像玩家控制器那样存在于每个级别，因为它们是 UObject，而不是 Actor。

```c++
// The way you get the local player is dependent on your own needs
// This is meant to be an example,
// NOT INTENDED FOR FINAL/RELEASE CODE
ULocalPlayer* localPlayer = nullptr;
if(UWorld* const world = GetWorld())
{
    if(APlayerController* const pc = world->GetFirstPlayerController())
    {
        localPlayer = pc->GetLocalPlayer();
    }
}
// Valid check the local player pointer
if(!IsValid(localPlayer))
{
    // If its not valid then exit the function
    return;
}
// Make sure Slate is initialized and working properly
if(FSlateApplication::IsInitialized())
{
    // This function will return the Slate User that this local player is tied to
    FSlateApplication::Get().GetUser(localPlayer->GetControllerId());
}
```

*有关如何使用玩家控制器的本地玩家对象获取 Slate User 的代码示例*

<a name="widget-layout"></a>

### 4.4 Widget布局

Slate Widgets 布局的计算方式分两遍完成（按执行顺序列出）：

1. **缓存所需大小**：计算每个Widget想要占用多少空间，这是通过*自下而上的*方法发生的，它保证当Widget发生此传递时，其子级已经计算并缓存了它们所需的大小。

![缓存所需大小示例](../../images/cache_desired_size.png)<br>*具有文本块和图像Widget的水平框所需尺寸的示例*

对于所需大小的示例，我们有一个水平框，其中包含文本块和图像Widget。在本例中，我们计算文本块（通过显示的字符串测量）和图像Widget（根据显示的图像数据测量）所需的大小。然后通过组合文本块和图像Widget（我们假设文本块是 14 个 slate 单位，图像Widget是 8 个 slate 单位）所需的大小来计算水平框，因此在本例中 14 个 slate 单位 + 8 个 slate 单位 = 22 个Slate单元。

1. **排列子项**：这发生在*自上而下的*方法中，其中要求Widget根据所需的大小和该Widget的所需大小（全部发生在第一遍中）来排列其子项。

![安排孩子](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/arrange_children.png?raw=true)<br>*使用文本块和图像Widget的水平框的分配大小来排列子项的示例*

对于排列的子示例，水平框由其父窗口Widget分配了 25 个Slate单元（为了简单起见，未显示）。第一个水平框槽表示它想要子项的所需大小，即距离文本块 14 个Slate单位，而第二个槽想要填充可用宽度，即图像Widget剩余的 11 个Slate单位。

<a name="on-paint"></a>

### 4.5 On Paint

**绘制 Slate**是 Slate 迭代所有可见Widget并创建**绘制元素**列表以发送到渲染系统的过程，该列表每帧都会创建。

这发生在 On Paint 函数中，它将执行两件事：

- 根据所有孩子的**几何形状**（所需尺寸）排列他们。
- 绘制与此控件相关的实际视觉效果。

**[<span>⬆</span>返回顶部](#table-of-contents)**
<a name="widget-ticking"></a>

### 4.6 Widget Ticking

Slate Widget（也指 UMG Widget）本身没有tick，它们没有tick组件，也没有tick组。 Slate Widget 的tick发生的顺序是在 Paint 过程中，这些调用源自 Slate 应用程序，因此只有当Widget位于屏幕上并正在渲染时才会调用它的tick函数：

1. `FSlateApplication::Tick`
2. `FSlateApplication::TickAndDrawWidgets` ...
3. `SWidget::Paint`
4. `SWidget::OnPaint` ...
5. `SObjectWidget::Tick`
6. `UUserWidget::NativeTick` （此时蓝图也将收到Tick）

<a name="widget-hierarchy"></a>

### 4.7 Widget层次结构

**Widget 层次结构**的概念是使用子槽完成的，子槽是可以绑定到 Slate Widget 的可选对象（因为某些 Widget 没有设计为具有子项，例如 Image widget（ **Leaf Widget** ）），但要求槽是用于跟踪每个widget子widget的自定义构建，例如按钮widget（**Compound Widget**）如何仅接受 1 个子widget，同时Overlay widget可以有多个子widget。

Widgets通常有 3 种主要类型：

-  **Leaf Widgets** ：没有子槽的Widget。 <br>![叶小部件示例](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/leaf_widgets.png?raw=true)
-  **Panels Widget**：具有动态数量的子插槽。 <br>![面板小部件示例](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/panel_widgets.png?raw=true)
-  **Compound Widgets**：具有固定数量的显式命名子槽的Widget。 <br>![复合小部件示例](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/compound_widgets.png?raw=true)

任何 Slate Widget（也称为 SWidget，其中每个 Slate Widget 在 C++ 中都以大写“S”为前缀）的关键元素是函数和值的组合：

- **计算所需尺寸**（函数）：负责计算所需尺寸，作为布局的第一遍。
    - **Slate 矩形**（值）：原点位于左上角的矩形，由左上角和右下角定义。原点位于左上角，Y 轴向下，X 轴向右。这用于计算所需的大小和边界。
- **排列子组件**（函数）：负责排列子组件作为布局的第二遍。
- **On Paint** （函数）：负责Widget的实际渲染外观。
- **事件处理程序**（委托值和/或函数）：这些是基于事件的钩子，用于 UI 元素在运行时更改，通常以“OnSomething”的形式。

<a name="invalidation"></a>

### 4.8 无效

[Epic 关于 Slate 和 UMG 失效的官方文档](https://docs.unrealengine.com/latest/INT/invalidation-in-slate-and-umg-for-unreal-engine/)

为了避免必须每帧计算Widget所需的大小（如果同时发生在许多Widget上，这可能会加重 CPU 负担），Widget具有缓存其所需大小的概念，但在运行时如果Widget的大小如果发生更改（通过动画或游戏代码），那么它将使Widget**无效**，以告诉 Slate 该Widget需要重新计算其**所需大小**，然后**重新排列该Widget所在（或具有）的布局**。这是一种优化，以避免在不需要重新计算时每帧轮询每个Widget的所需大小。

使`InvalidateWidgetReason.h`中的Widget失效时，您可以指定不同类型的失效原因：

- 布局：如果您的Widget需要更改所需的大小，请使用布局失效。这是一个昂贵的失效，所以如果您需要做的只是重绘一个Widget，请不要使用。
- Paint：当Widget的绘制已更改但不影响大小时使用。
- 波动性：如果仅调整了widget的波动性，则使用。
- ChildOrder：添加或删除子项（这意味着预传递和布局）。
- RenderTransform：Widget渲染变换已更改。
- 可见性：更改可见性（这意味着布局）。
- AttributeRegistration：属性已绑定或未绑定（由 SlateAttributeMetaData 使用）。
- Prepass：递归地重新缓存该Widget的所有Widget的所需大小（这意味着布局）。
- PaintAndVolatility：如果您要更改涉及绘制或大小调整的普通属性，请使用绘制Volatility失效。此外，如果更改的属性无论如何都会影响，那么重要的是使Volatility无效，以便可以重新计算和缓存它。
- LayoutAndVolatility：如果您要更改涉及绘画或大小调整的普通属性，请使用布局失效。此外，如果更改的属性无论如何都会影响Volatility，那么重要的是使Volatility无效，以便可以重新计算和缓存它。

<a name="slate-attributes-events"></a>

### 4.9 Slate属性和事件

Slate（以及通过 Slate；UMG）支持使用属性将属性/函数/lambda 绑定到Widget的属性。仅当Widget可见/未折叠时，Widget属性才会更新，因此将其可见性设置为隐藏将导致其不更新。

属性对于Widget样式特别有用，您可以将整个项目中Widget的不同视觉样式指定为通用主题。这提高了工程和艺术团队整个项目的生产力。

- `TAttribute` ：虚幻引擎的基本属性类型，不用于 SWidget 中的成员属性。
    - 与失效不兼容，因为数据更改时不会广播。
    - 不适合缓存（这意味着 CPU 可能会变慢）。
    - 如果您使用`TAttribute`更改 SWidget 的状态，则需要覆盖`ComputeVolatility` （ `TSlateAttribute`和`TSlateManagedAttribute`不需要）。
- `TSlateAttribute` ：应与 SWidget 成员属性一起使用，允许属性与 Slate 的失效系统一起使用，并且对于 Slate 特定代码具有更高的性能，同时保持属性在引擎中的安全。
    - 不继承自`TAttribute` ，它继承自`FSlateAttributeBase` / `TSlateMemberAttribute` 。
    - 不可复制，但如果您需要它可移动，则建议使用`TSlateManagedAttribute` 。
    - 在 PrePass 更新阶段每帧更新一次，因此性能更加友好。
    - 成员属性按照 SWidget 定义中定义变量的顺序更新（默认情况下）。
    - 允许失效原因作为谓词和/或可以被每个 SWidget 覆盖，但要谨慎使用，因为它可能会破坏父Widget的失效。
- `TSlateManagedAttribute` ：应与数组或其他移动数据结构内部的 SWidget 成员属性一起使用。
    - 不继承自`TAttribute` ，它继承自`FSlateAttributeBase` / `TSlateMemberAttribute` 。
    - 只能移动而不能复制，因此它们会消耗更多内存。

> - `TAttributes`具有较高的内存开销并且不适合缓存。所以请根据您的判断使用。
> - 所有 slate 属性都保存在`SlateAttributeMetaData`中，以便在每个 SWidget 中轻松访问。

在 SWidget 内声明事件和属性宏时，您需要将它们放在其他两个宏之间：

- `SLATE_BEGIN_ARGS`或`SLATE_USER_ARGS` ：区别在于`SLATE_USER_ARGS`要求用户在源文件中实现所有Widget，因此标头只能保存声明信息，其中所有处理程序本质上都是真正私有的并且可以内联（因此更少的样板代码）。
- `SLATE_END_ARGS`

使用这些宏允许Widget作者通过`SNew`和`SAssignNew`添加对Widget构造的支持。

Slate 中的属性还具有在声明它们时必须使用的特定宏，并且如果您想在运行时创建 Slate Widget 时公开这些属性，则必须使用这些宏。

- `SLATE_ATTRIBUTE` ：
    - 允许属性与值或函数一起使用。
    - 将属性类型作为第一个参数，将属性名称作为第二个参数（建议匹配成员属性以避免混淆）。
- `SLATE_ARGUMENT` ：
    - 允许属性仅与值一起使用。
    - 将属性类型作为第一个参数，将属性名称作为第二个参数（建议匹配成员属性以避免混淆）。
- `SLATE_ARGUMENT_DEFAULT` ：与`SLATE_ARGUMENT`相同，但也支持默认值，语法： `SLATE_ARGUMENT_DEFAULT(float, WheelScrollMultiplier) = 1.0f;`
- `SLATE_STYLE_ARGUMENT` ：与`SLATE_ARGUMENT`相同，但它们只能与继承自`FSlateWidgetStyle`类型一起使用，以实现Widget的视觉样式目的。

这是我们使用这些宏制作自定义按钮Widget的示例。

```c++
class SMyButtonWidget : public SMyParentWidget
{
    SLATE_DECLARE_WIDGET(SMyButtonWidget, SMyParentWidget)
public:

    /** Setup default values for these arguments, underscore is to avoid shadowing of member names */
    SLATE_BEGIN_ARGS( SMyButtonWidget )
        : _Style(&FCoreStyle::Get().GetWidgetStyle< FButtonStyle >( "Button" ))
        , _AdditionalPadding(FMargine(4.0f, 2.0f))
        , _CanBounce(true)
    { }

    /** Scaling for after the button is customized */
    SLATE_ARGUMENT_DEFAULT( float, CustomizedScaling ) = 1.0f;

    /** Visual style of the button */
    SLATE_STYLE_ARGUMENT( FButtonStyle, Style )
    
    /** Additional padding for what this button needs to display its visuals. */
    SLATE_ATTRIBUTE( FMargin, AdditionalPadding )
    
    /** For knowing if the button should be able to bounce when clicked. */
    SLATE_ARGUMENT( bool, CanBounce )

    SLATE_END_ARGS()
    
    /**
	 * Construct this widget
	 *
	 * @param	InArgs	The declaration data for this widget
	 */
	void Construct( const FArguments& InArgs )
	{
	    // Order does not matter here but to get the property we need the underscore infront of the value we're getting.
	    bCanBounce = InArgs._CanBounce;
	    Style = InArgs._Style;
	    AdditionalPadding = InArgs._AdditionalPadding;
	    CustomizedScaling = InArgs._CustomizedScaling;
	}

private:

    /** Scaling for after the button is customized */
    float CustomizedScaling;

    /** Style resource for this custom button. */
    const FButtonStyle* Style;
    
    /** Additional padding for what this button needs to display its visuals. */
	TSlateAttribute<FMargin> AdditionalPadding;

    /** Flag to know if this button can be bounced when clicked. */
    TSlateAttribute<bool> bCanBounce;
}
```

Slate 事件基本上是 SWidget 中用于绑定Widget创建的委托，您可以在 C++ 中使用参数范围宏声明中的宏来声明您的委托。

- `SLATE_EVENT` ：使用特定成员变量为此Widget添加事件处理程序支持，这公开了用于在创建时绑定的委托。期望Widget具有`EventDelegateType`的委托，其名称与输入的事件名称相同。

下面是一个示例Widget，它在悬停时使用事件宏。

```c++
class SMyWidget : public SMyParentWidget
{
    SLATE_DECLARE_WIDGET(SMyWidget, SMyParentWidget)
public:

    SLATE_USER_ARGS( SMyWidget )
    { }

    /** Called when this widget is hovered. */
    SLATE_EVENT( FSimpleDelegate, OnHovered)

    SLATE_END_ARGS()

private:

    /** Called when this widget is hovered. */
    FSimpleDelegate OnHovered; // FSimpleDelegate is within the engine BTW
}
```

<a name="slate-widget-examples"></a>

### 4.10 Slate Widget示例（Slate 测试套件/Starship 套件）

**Slate Widget Examples** （如果使用 UE4，也称为**Slate Test Suite；**如果使用 UE5，则称为**Starship Suite** ）是 Slate 构建示例的集合，例如单选按钮、响应式网格、色轮等。

您可以通过以下方式访问虚幻编辑器中的测试套件：

1. 根据您是否使用 UE4/UE5，这会有所不同
    - UE4： `Window>Developer Tools>Debug Tools`
    - UE5： `Tools/Debug/Debug Tools`
        ![Slate 小部件示例第 1 步](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/slate_widget_examples_step1.png?raw=true)
2. 选择`Test Suite`
    ![Slate 小部件示例第 2 步](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/slate_widget_examples_step2.png?raw=true)

如果您有引擎的源代码版本并构建测试套件程序（这将在`[EnginePath]/Engine/Binaries/Win64/`中创建可执行文件），您还可以将测试套件作为其自己的应用程序运行，而无需打开虚幻编辑器。

- 虚幻引擎4
    - `[EnginePath]/Engine/Source/Runtime/AppFramework/Public/Widgets/Testing/STestSuite.h`
    - `[EnginePath]/Engine/Source/Runtime/AppFramework/Private/Widgets/Testing/STestSuite.cpp`
- 虚幻引擎5
    - `[EnginePath]/Engine/Source/Runtime/AppFramework/Public/Widgets/Testing/SStarshipSuite.h`
    - `[EnginePath]/Engine/Source/Runtime/AppFramework/Private/Widgets/Testing/SStarshipSuite.cpp`

![UE4 测试套件示例](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/ue4_test_suite.png?raw=true)
*来自UE4的测试套件，目前UE4版本的测试套件比UE5中的Starship Suite功能更丰富。*

<a name="helpful-console-commands-for-slate"></a>

### 4.11 Slate 有用的控制台命令

> 对于调试控制台命令，请导航至[UMG/Slate 开发和调试工具](#dev-debug-tools)部分的[调试控制台命令](#debug-console-commands)。

- `Slate.GlobalScrollAmount [float value]` （默认 = 32.0）：每次单击鼠标滚轮时用于滚动的量（以 Slate 单位为单位）。

**[<span>⬆</span>返回顶部](#table-of-contents)**
<a name="umg"></a>

## 5.0 UMG（虚幻动态图形）

UMG 框架是 UObject，必须绑定到称为**拥有玩家的**特定**玩家控制器**（以考虑分屏），如果没有为拥有玩家输入任何内容，那么它将自动连接到关卡中的第一个本地玩家。

UMG 对象类型的层次结构如下所示：

- **UVisual** ：UMG 插槽和Widget中元素的基类。
    - **UWidget** ：所有 Widget 的基类，它们创建 Slate Widget 并处理从基于 Blueprint/UObject 的类到 Slate 的路由功能。这些是 TextBlock、ScrollBox、Button 等Widget。
    - **UUserWidget** ：用于设计 UI、动画 UI 以及将该功能连接到游戏代码的基类。

User Widget是由Widget构建的，除了User Widget不需要根Widget之外，基本上就像Actor如何由多个Actor组件构建并需要根Actor组件（称为根组件）。

User Widget无法像 Actor 处理组件那样继承其**Widget层次结构**，但它们可以继承类功能（因此，使User Widget Abstract将允许其他类继承它，或者在 C++ 中创建类将允许继承）。

<a name="user-widget-hierarchy"></a>

### 5.1 用户控件层次结构

每个**用户控件**在设计上都是根`UWidget` ，因此用户控件内部不能有任何控件，并且默认情况下是一个**复合控件**，只能有 1 个子控件，但该子控件下面可以有其他子控件，并导致子控件的级联效果每个User Widget的**树层次结构**中的Widget。

设计器/层次结构编辑器视图 | 运行时结果
:-: | :-:
 ![User Widget层次结构示例](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_hierarchy.png?raw=true)<br> *`Health_Bar`和`Health_Text`为**粗体**，因为它们启用了`Is Variable`标志*<br>![是变量标志](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/is_variable.png?raw=true) |  ![用户小部件层次结构示例](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_hierarchy.png?raw=true)<br> *`Health_Bar`和`Health_Text`为**粗体**，因为它们启用了`Is Variable`标志*<br>![是变量标志](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/is_variable.png?raw=true)

 ![User Widget层次结构示例](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/umg_hierarchy_diagram.png?raw=true)<br> *`Health_Bar`和`Health_Text`为**粗体**，因为它们启用了`Is Variable`标志*<br>![是变量标志](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/is_variable.png?raw=true)

<a name="user-widget-animation"></a>

### 5.2 User Widget动画

每个User Widget都能够通过**Sequencer**使用该User Widget内的Widget创建自定义动画。您可以在Widget设计器中创建这些动画，并且可以专门修改与该Widget相关的内容，例如渲染变换、Widget可见性等。您还可以修改Widget的属性，例如材质参数、Widget内的运行时值等等。

![用户小部件动画设计器示例](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/umg_animation_designer.gif?raw=true)
 *UMG中的动画设计器示例*

如果User widget的**tick 频率**在类默认值中设置为**“从不”**而不是**“自动**”，那么它将永远不会运行动画，因为要播放动画，User Widget需要能够勾选该动画，并且如果User Widget能够勾选关闭（通过将其Tick频率设置为从不），则它将不会勾选动画对象。

<a name="user-widget-events"></a>

### 5.3 User Widget事件

每个User Widget都有内置事件，您可以从中实现和添加自己的功能；

- **预构造**：这发生在设计器的编辑器中以及实际创建Widget之前，类似于 Actor 上的构造脚本。

```c++
virtual void UUserWidget::NativePreConstruct()
{
    // Call BP version
    PreConstruct(IsDesignTime());
}
```

![预构建](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_events/preconstruct.png?raw=true)

- **初始化时**：这只在运行时创建非模板实例时（当您生成User Widget时）发生一次。

```c++
virtual void UUserWidget::NativeOnInitialized()
{
    // Bind any input delegates that may be on this widget to its owning player controller
    if(APlayerController* PC = GetOwningPlayer())
    {
    	UInputDelegateBinding::BindInputDelegates(GetClass(), PC->InputComponent, this);
    }
    
    OnInitialized();
}
```

![初始化时](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_events/oninitialized.png?raw=true)

- **Construct**：这可以在单个user widget上多次发生，因为它基于它何时构造到屏幕（添加到视口或添加到播放器屏幕）。因此，如果您要从父级中删除Widget并稍后重新添加它，那么建议不要将首次初始化代码放入其中，而是将其放入“On initialized”中。

```c++
virtual void UUserWidget::NativeConstruct()
{
    // Call BP version
    Construct();
    UpdateCanTick();
}
```

![构造](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_events/construct.png?raw=true)

- **Destruct** ：当此Widget不再在屏幕上时发生，可以多次调用，因为它与从父级中删除此Widget时发生的构造相反。

```c++
virtual void UUserWidget::NativeDestruct()
{
    StopListeningForAllInputActions();
    // Call BP version
    Destruct();
}
```

![破坏](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_events/destruct.png?raw=true)

- **On Paint** ：每帧绘制此控件时发生，与 Tick 不同，因为它旨在处理 Paint Context 信息。

```c++
virtual int32 UUserWidget::NativePaint(const FPaintArgs& Args, const FGeometry& AllottedGeometry, const FSlateRect& MyCullingRect, FSlateWindowElementList& OutDrawElements, int32 LayerId, const FWidgetStyle& InWidgetStyle, bool bParentEnabled ) const
{
    // If BP implemented the function
    if ( bHasScriptImplementedPaint )
    {
    	FPaintContext Context(AllottedGeometry, MyCullingRect, OutDrawElements, LayerId, InWidgetStyle, bParentEnabled);
    	// Call BP version
    	OnPaint( Context );

    	return FMath::Max(LayerId, Context.MaxLayer);
    }

    return LayerId;
}
```

![On Paint](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_events/onpaint.png?raw=true)

- **Tick**：此Widget在屏幕上的每一帧都会发生这种情况，如果Widget未显示，则不会Tick（即使它仍然存在，唯一重要的是它正在显示，然后Tick）。

```c++
virtual void UUserWidget::NativeTick(const FGeometry& MyGeometry, float InDeltaTime)
{
    //...
    
    // If BP implemented the function in the event graph
    if (bHasScriptImplementedTick)
    {
        // Call BP version
    	Tick(MyGeometry, InDeltaTime);
    }
}
```

![On Paint](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_events/onpaint.png?raw=true)

- **动画开始时**：当Widget动画开始播放时发生，它会为您提供开始播放的Widget，以便您需要检查它或稍后使用它。 （对于蓝图用户；虚幻的最新版本要求动画完成事件位于事件图中，而之前的版本允许它们显示为可重写函数）

```c++
virtual void UUserWidget::OnAnimationStartedPlaying(UUMGSequencePlayer& Player)
{
    // Call BP version
    OnAnimationStarted(Player.GetAnimation());

    BroadcastAnimationStateChange(Player, EWidgetAnimationEvent::Started);
}
```

![动画开始时](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_events/animation_started.png?raw=true)

- **动画完成时**：当Widget动画完成播放时发生，它会告诉您该Widget已完成播放，以便您需要检查它或稍后使用它。 （对于蓝图用户；虚幻的最新版本要求动画完成事件位于事件图中，而之前的版本允许它们显示为可重写函数）

```c++
virtual void UUserWidget::OnAnimationFinishedPlaying(UUMGSequencePlayer& Player)
{
    // This event is called directly by the sequence player when the animation finishes.

    // Call BP version
    OnAnimationFinished(Player.GetAnimation());

    BroadcastAnimationStateChange(Player, EWidgetAnimationEvent::Finished);

    if ( Player.GetPlaybackStatus() == EMovieScenePlayerStatus::Stopped )
    {
    	StoppedSequencePlayers.Add(&Player);

    	if (AnimationTickManager)
    	{
    		AnimationTickManager->AddLatentAction(FMovieSceneSequenceLatentActionDelegate::CreateUObject(this, &UUserWidget::ClearStoppedSequencePlayers));
    	}
    }

    UpdateCanTick();
}
```

![动画完成时](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_events/animation_finished.png?raw=true)

- **On Focus Received** ：（如果您在蓝图中找不到它，它必须返回一个事件回复结构，因此您必须在函数列表中而不是在事件图中覆盖它）当焦点被授予此User Widget时发生（仅限此Widget）。它要求您返回一个事件回复结构，您可以选择返回已处理或未处理。

```c++
virtual FReply UUserWidget::NativeOnFocusReceived( const FGeometry& InGeometry, const FFocusEvent& InFocusEvent )
{
    // Call BP version and return what BP returns
    return OnFocusReceived( InGeometry, InFocusEvent ).NativeReply;
}
```

![焦点收到](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_events/focus_received.png?raw=true)![两种处理方式均获得焦点](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_events/focus_received_both.png?raw=true)

- **添加到焦点路径时**：当此Widget或此User Widget中的子Widget添加到焦点路径（或聚焦）并且以前不是其中的一部分时发生。

```c++
virtual void UUserWidget::NativeOnAddedToFocusPath(const FFocusEvent& InFocusEvent)
{
    // Call BP version
    OnAddedToFocusPath(InFocusEvent);
}
```

![添加到焦点路径](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_events/added_to_focus_path.png?raw=true)

- **On Focus Lost** ：当此User Widget（仅此Widget）失去焦点时发生。

```c++
virtual void UUserWidget::NativeOnFocusLost( const FFocusEvent& InFocusEvent )
{
    // Call BP version
    OnFocusLost( InFocusEvent );
}
```

![关于失去焦点](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_events/focus_lost.png?raw=true)

- **从焦点路径中删除时**：与焦点丢失时类似，但当此User Widget内的子Widget或此Widget本身不再是焦点路径的一部分时，可能会发生这种情况。

```c++
virtual void UUserWidget::NativeOnRemovedFromFocusPath(const FFocusEvent& InFocusEvent)
{
    // Call BP version
    OnRemovedFromFocusPath(InFocusEvent);
}
```

![从焦点路径中移除](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_events/removed_from_focus_path.png?raw=true)

**[<span>⬆</span>返回顶部](#table-of-contents)**
<a name="common-widgets"></a>

## 6.0 常用Widget

虚幻引擎中存在大量常用的Widget，其基本核心元素。<br>大多数/所有使用 UMG/Slate 的 UI 可能都是由这些Widget的组合构建的：

-  **文本块***[Leaf Widget]* ：处理显示静态文本，可以通过将其设置为另一个文本值在运行时进行更改。 TextBlock Widget允许使用自定义字体（如果该字体有其他字体，则包括其字体）、自定义其文本大小、字母间距（也称为字距调整）、其轮廓设置（这将偏移渲染的文本）、应用材质对于字母本身，添加阴影偏移（这将偏移渲染的文本），设置其对齐方式（文本如何对齐）等。 <br>![文本块小部件](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/w_textblock.png?raw=true)
-  **富文本块***[Leaf Widget]* ：与 TextBlock widget类似，但允许在同一文本值中同时使用自定义图像、字形、多种字体等。 <br>![富文本块widget](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/w_richtextblock.png?raw=true)
-  **图像***[Leaf Widget]* ：处理显示纹理或使用 UI 域来显示它的材质。
    ![图片小工具](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/w_image.png?raw=true)
-  **边框***[Compound Widget]* ：只能有 1 个子Widget。在此Widget前面显示子Widget，基本上是可以有子Widget的图像Widget。 <br>![边框小工具](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/w_border.png?raw=true)
- **按钮***[Compound Widget]* ：只能有 1 个子Widget。在此Widget前面显示该子项，可以与之交互并获得焦点。单击/按下/释放/悬停/取消悬停时返回。您还可以设置其单击（鼠标按钮）方法、触摸（触摸屏）方法和按下（键盘和游戏手柄）方法。 <br>![按钮小部件](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/w_button.png?raw=true)
-  **复选框***[Leaf Widget]* ：根据复选框所处的状态来处理显示特定图像/材料，可以单击它（或设置为特定状态），并用于显示某些内容是否打开/关闭。
    ![复选框小部件](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/w_checkbox.png?raw=true)
- **进度条***[Leaf Widget]* ：通过其条形填充类型和条形填充样式，通过缩放的 0-1 填充百分比值来处理在此Widget前面显示图像/材料。
    - 填充类型：
        -  **从左到右**：从左到右填充进度条。
            <img src="https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/progress_bars/w_progressbar_left_right_mask.png?raw=true">
            *蒙面版*
            <img src="https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/progress_bars/w_progressbar_left_right_scale.png?raw=true" class="">
            *缩放版本*
    -  **从右到左**：从右到左填充进度条。
        <img src="https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/progress_bars/w_progressbar_right_left_mask.png?raw=true" class="">
        *蒙面版*
        <img src="https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/progress_bars/w_progressbar_right_left_scale.png?raw=true" class="">
        *缩放版本*
    -  **从中心填充**：从中心向边缘线性填充 X 和 Y 上的进度条。
        <img src="https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/progress_bars/w_progressbar_center_mask.png?raw=true" class="">
        *蒙面版*
        <img src="https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/progress_bars/w_progressbar_center_scale.png?raw=true" class="">
        *缩放版本*
    -  **从中心水平填充**：在 X 轴上从中心向边缘线性填充进度条。
        <img src="https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/progress_bars/w_progressbar_center_horizontal_mask.png?raw=true" class="">
        *蒙面版*
        <img src="https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/progress_bars/w_progressbar_center_horizontal_scale.png?raw=true" class="">
        *缩放版本*
    -  **从中心垂直填充**：在 Y 轴上从中心向边缘线性填充进度条。
        <img src="https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/common_widgets/progress_bars/w_progressbar_center_vertical_mask.png?raw=true" class="">
        *蒙面版*
        <img src="https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/progress_bars/w_progressbar_center_vertical_scale.png?raw=true" class="">
        *缩放版本*
    -  **从中心垂直填充**：在 Y 轴上从中心向边缘线性填充进度条。
        <img src="https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/progress_bars/w_progressbar_top_bottom_mask.png?raw=true" class="">
        *蒙面版*
        <img src="https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/progress_bars/w_progressbar_center_vertical_scale.png?raw=true">
        *缩放版本*
    -  **从下到上**：从下到上填充进度条。
        <img src="https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/progress_bars/w_progressbar_bottom_top_mask.png?raw=true" class="">
        *蒙面版*
        <img src="https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/progress_bars/w_progressbar_bottom_top_scale.png?raw=true" class="">
        *缩放版本*
    - 填充样式：
        - **Mask** ：根据填充百分比和填充类型遮盖进度条的填充图像。
        - **缩放**：进度条的填充图像根据填充百分比和填充类型进行缩放和拉伸/压缩。
    -  **滑块***[Leaf Widget]* ：与进度条类似，但它是可交互的，您可以将其方向从水平更改为垂直并设置其步长（对于键盘/游戏手柄按下）。
        ![滑块小部件](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/w_slider.png?raw=true)
-  **可编辑文本***[Leaf Widget]* ：用户能够输入一行文本的字段，允许提示文本并且可以设置为只读，是密码（因此它只显示一个点而不是实际文本） ，以及能够像普通文本块Widget一样调整其设置。 <br>![可编辑文本Widget](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/w_editable_text.png?raw=true)
-  **可编辑文本（多行）** *[Leaf Widget]* ：与可编辑文本Widget相同，只是用户可以输入多行文本而不是一行。 <br>![可编辑文本（多行）Widget](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/w_editable_text_multi.png?raw=true)
-  **文本框***[Leaf Widget]* ：与可编辑文本相同，只是它在文本后面用图像/材料包裹。
    ![文本框小部件](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/w_textbox.png?raw=true)
-  **文本框（多行）** *[Leaf Widget]* ：与可编辑文本（多行）相同，只不过它在文本后面用图像/材料包裹。 <br>![文本框（多行）Widget](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/w_textbox_multi.png?raw=true)
-  **Spin Box** *[Leaf Widget]* ：显示一个数字（可以设置是否允许小数），用户可以输入或使用鼠标与其交互来滑动和增加/减少数字。
    ![旋转框小部件](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/w_spinbox.png?raw=true)
-  **Combo Box(String)** *[Leaf Widget]* ：一个下拉框Widget，在选择时显示字符串（非文本，因此不可本地化）值并显示其选项。 <br>![组合框小部件](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/w_combobox.png?raw=true)
- **失效框***[Compound Widget]* ：只能有 1 个子Widget。这将控制子窗口Widget何时因其布局/几何通道而无效，这对于性能优化非常有用。*不能真正为此使用图片，因为它完全围绕Widget并且是不可见的*。
-  **Retainer Box** *[Compound Widget]* ：只能有 1 个子Widget。这将在其子Widget上渲染材质，并且仅在其子Widget上渲染材质，因此不会在子Widget渲染时不占用的背景空间上渲染该材质。<br>例如，如果您使用固定框包裹文本块，则材质将仅应用于文本，而不应用于每个字母之间的空间。 <br>![固定盒Widget](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/w_retainerbox.png?raw=true)<br>*设计器中固定盒的层次结构*<br>![固定盒材质Widget](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/w_retainerbox_material.png?raw=true)<br>*固定盒材质*<br>![固定盒结果Widget](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/w_retainerbox_result.png?raw=true)<br>*运行时的固定框（在编辑器中运行游戏时）*
-  **Retainer Box** *[Compound Widget]* ：只能有 1 个子Widget。这将在其子Widget上渲染材质，并且仅在其子Widget上渲染材质，因此不会在子Widget渲染时不占用的背景空间上渲染该材质。<br>例如，如果您使用固定框包裹文本块，则材质将仅应用于文本，而不应用于每个字母之间的空间。 <br>![固定盒Widget](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/w_retainerbox.png?raw=true)<br>*设计器中固定盒的层次结构*<br>![固定盒材质Widget](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/w_retainerbox_material.png?raw=true)<br>*固定盒材质*<br>![固定盒结果Widget](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/w_retainerbox_result.png?raw=true)<br>*运行时的固定框（在编辑器中运行游戏时）*
- **Circular Throbber** *[Leaf Widget]* ：以圆形方式移动图像/材质的 throbber 版本。
    ![圆形颤动小部件](images/common_widgets/w_throbber_circular.png)
    - > *作者的注释； “如果可能的话，请在最终产品中使用默认图像以外的其他图像来更改颤动者的图像。我厌倦了在完整发布的产品中看到默认的 throbber，你可以做得更好！谢谢。”*
- **Spacer** *[Leaf Widget]* ：此Widget没有视觉表示，它仅提供其他Widget之间的填充和空间，如果您的 UI 设计不断变化以使快速迭代更容易，建议使用此功能而不是在插槽内填充。
- **背景模糊***[Compound Widget]* ：只能有 1 个子Widget。基本上是一个图像Widget，其子Widget在其后面渲染，并使用高斯模糊模糊该子Widget的渲染结果。<br>建议将此功能与适当的剪切区域设置一起使用，否则使用自定义材质以使艺术家更容易修改。 <br>![背景模糊Widget](images/common_widgets/w_background_blur.png)
- **输入键选择器***[Leaf Widget]* ：允许用户在聚焦此Widget时按下输入，它将显示是什么输入。 <br>![输入键选择器Widget](images/common_widgets/w_input_key_selector.png)
- **画布面板***[Panel Widget]* ：这是新创建的User Widget中的默认Widget，允许设计者将子Widget放置在任意位置，与该画布的其他Widget锚定并按 z 顺序排列。<br>它使用绝对布局来放置，因此它非常适合跟随特定的世界中对象或可以在整个屏幕上移动的对象的屏幕指示器。 <br>![画布面板Widget](images/common_widgets/w_canvas_panel.png)
- **Horizo​​ntal Box** *[Panel Widget]* ：允许其子部件以从左到右的水平流布局，索引 0 为最左边，最后一个部件为最右边。
    ![水平框小部件](images/common_widgets/w_horizontalbox.png)
- **垂直框***[Panel Widget]* ：与水平框工作方式相同，只是它将其子项布置在从上到下移动的垂直流中，其中 0 索引作为最远的顶部Widget，其最后一个Widget是最远的底部Widget。<br>*垂直框和水平框不滚动，为了允许您必须使用滚动框Widget或类似的东西。* <br>![垂直框Widget](images/common_widgets/w_verticalbox.png)
- **滚动框***[Compound Widget]* ：与垂直框和水平框的工作方式相同（必须设置为垂直或水平），但允许它们可滚动。不支持虚拟化。 <br>![滚动框Widget](images/common_widgets/w_scrollbox.png)
- **大小框***[Compound Widget]* ：只能有 1 个子Widget。允许此Widget指定其子Widget所需的大小（因为并非所有Widget都会报告所需的大小，因为它们依赖于自己的子Widget）。 <br>![尺寸框Widget](images/common_widgets/w_sizebox.png)
- **比例框***[Compound Widget]* ：只能有 1 个子Widget。允许此Widget对其子级进行缩放，以适应此框分配区域的约束尺寸。 <br>![比例框小部件](images/common_widgets/w_scalebox.png)<br>*在此示例中，缩放框正在调整图像大小以均匀地适合*
-  **覆盖***[面板小部件]* ：根据子小部件中的索引显示彼此堆叠的小部件。这个小部件对于快速将一个小部件快速覆盖在另一个小部件上非常有用。
    ![叠加小部件](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widgets/w_overlay.png?raw=true)
    *文本和图像是覆盖小部件的子项*
- **网格面板***[面板小部件]* ：允许子小部件自动放置在类似于表格的网格图案中，保留每列的宽度。
    ![网格面板小部件](images/common_widgets/w_gridpanel.png)*该网格已配置为填充每列和行之间的空间，您的网格可能看起来有所不同，具体取决于您的配置方式*
- **统一网格面板***[面板小部件]* ：基本上是网格面板，但它将在其所有子面板之间均匀划分可用空间。
    ![统一网格面板小部件](images/common_widgets/w_uniformgrid.png)
- **小部件切换器***[面板小部件]* ：这将通过其子索引一次仅显示其子小部件之一，但它将加载所有这些小部件（对主页不好，只是较小的东西）并初始化，当小部件切换器被加载、初始化、构造。
- **安全区***[复合小部件]* ：只能有 1 个子小部件。该小部件很特殊，它将向内向其子小部件的顶部/底部/左侧/右侧应用填充，以说明该小部件显示在什么设备上，例如在某些一侧有凹口的移动设备上，安全区域将考虑到这一点，并为其子小部件添加填充，这样它就不会被凹口切断，并考虑到边框下有额外像素的电视、隐藏在黑色边框后面的额外像素列的投影仪，一个很好的例子是，对于某些带有凹口的手机（您知道我在谈论哪些手机），安全区域将为您填充屏幕的那一侧，这样您的小部件就不会被凹口覆盖。

> 您还可以使用一些有用的调试控制台命令在[UMG/Slate 开发和调试工具](#dev-debug-tools)的[调试控制台命令](#debug-console-commands)部分的编辑器中模拟 PC 上的安全区域。

![安全区小部件](images/common_widgets/w_safezone.png)
*在此示例中，我们用保存区域包裹了画布面板，以便它将画布推离屏幕区域，该区域由于屏幕的凹口或操作系统而无法访问*

**[<span>⬆</span>返回顶部](#table-of-contents)**
<a name="common-widget-functionality"></a>

## 7.0 常用小部件功能

每个小部件都有自己的功能和用途，同时仍然包含所有小部件的通用功能，例如：

- **可访问性**：每个小部件将侦听项目的默认可访问性设置，每个小部件都可以覆盖这些设置，并且可以影响其自己的子项，
    讨论可访问性超出了本文档的范围，但这里有一个 Epic 官方批准的课程来讨论它：
     [史诗级认可的无障碍设计入门课程](https://www.unrealengine.com/en-US/onlinelearning-courses/introduction-to-accessible-design-with-unreal-engine)。
    ![辅助功能字段概述](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widget_func/accessibility_overview.png?raw=true)
    - **覆盖可访问默认值**：启用后将覆盖此小部件的默认可访问行为和文本。
        - **子项是否可访问**：标记以了解此小部件的子项是否应作为不同的可访问小部件。
        - **Accessible Behaviour** ：设置此小部件是否可访问，包括如何描述它。使用“自定义”选项时，您可以输入自己的可访问文本。 ![辅助功能行为概述](images/common_widget_func/accessibility_behavior.png)
        - **可访问摘要行为**：设置当通过父窗口小部件的摘要呈现该窗口小部件时如何描述该窗口小部件。使用“自定义”选项时，您可以输入自己的可访问文本。 ![辅助功能摘要行为概述](images/common_widget_func/accessibility_summary_behavior.png)
- **工具提示文本**：一种工具提示文本小部件，它使用默认小部件或您自己的自定义小部件，当用户将鼠标悬停在小部件上时显示。
    ![工具提示文本字段](images/common_widget_func/tooltip_text.png)
- **已启用**：标记以了解此小部件是否已启用/禁用以及用户是否可以交互修改。
    ![已启用复选框](images/common_widget_func/is_enabled.png)
- **可见性**：小部件的渲染可见性和交互可见性。
    - **Visible** ：渲染小部件并使其难以用光标处理。
    - **Collapsed** ：不渲染，不可交互，并且不占用布局空间。
    - **Hidden** ：不渲染且不可交互，但占用布局空间。
    - **Not Hit-Testable/Hit Test Invisible(Self &amp; All Children)** ：渲染小部件但不能与其交互，并且不允许其子项可交互。
    - **Not Hit-Testable/Hit Test Invisible(Self Only)** ：渲染小部件，但只有此小部件无法与之交互。
        ![可见性枚举器概述](images/common_widget_func/visibility_overview.png)
- **渲染不透明度**：此小部件的渲染不透明度也会影响其子级的不透明度。 0 = 不可见，1 = 完全可见。
    ![渲染不透明度](images/common_widget_func/render_opacity.png)
- **渲染变换**：每个小部件都有一个渲染变换，它可以影响小部件的碰撞形状，但不会覆盖原始布局和绘制信息。
    可以将其视为原始变换信息的修改器，但不会更改其布局。
    - **翻译**：X 和 Y 位置偏移。
    - **比例**：小部件的 X 和 Y 比例。
    - **剪切**：小部件的 X 和 Y 剪切，也称为倾斜。
    - **角度**：小部件的旋转。您只需要 1 个轴即可在 2D 空间中旋转，但它可以在 -180 到 180 度之间旋转。
    - **Pivot** ：小部件的枢轴偏移量，用于控制应用变换的位置。
        实际值是一个标准化量，从 X/Y 上的 0.5 开始，作为小部件的中心，0.0 = 左/上方向，1.0 = 右/下方向。
        ![渲染变换](images/common_widget_func/render_transform.png)
- **Is Volatile** ：此标志设置为 true 时将将此小部件标记为不可缓存，因此它始终必须更新其布局和几何碰撞，它默认为 false，因为它不常用，但会在您需要时公开。![不稳定](images/common_widget_func/is_volatile.png)
- **裁剪**：这与布局和几何信息有关，您可以在其中指定小部件的裁剪方式，
    这不会将剪切空间批处理在一起，因此这可能会产生性能成本，具体取决于屏幕上有多少重叠和剪切小部件。
    ![剪辑概述](images/common_widget_func/clipping_overview.png)
    - **Inherit** ：此剪切空间不允许窗口小部件剪切其子级，但此窗口小部件及其所有子级继承在其上方剪切的最后一个窗口小部件的剪切区域。
        ![继承剪辑示例](images/common_widget_func/clipping_inherit.png)
        *在此示例中，有一个`Scroll Box`小部件，其剪辑设置为`Inherit` ，并且它是`Canvas Panel`小部件的子项，其剪辑设置为`Clip to Bounds` 。*
    - **剪切到边界**：此剪切空间剪切到该小部件的边界，它使这些边界与其上方的任何先前剪切区域相交。
        ![剪辑到边界示例](images/common_widget_func/clipping_clip_to_bounds.png)
        *在此示例中，有一个`Scroll Box`部件，其剪辑设置为`Clip to Bounds` ，因此其子小部件（即`Button` ）在超出`Scroll Box`的边界时将被相交*
    - **剪切到边界 - 不相交（高级）** ：此剪切区域也剪切到其边界，但它**<u>不</u>**与任何现有的剪切几何体相交，它将始终推送自己的新剪切状态。
        允许此小部件在剪辑它的层次结构的边界之外进行渲染。但这**<u>不会</u>**让您忽略设置为*“始终”的*剪切区域。
        ![剪辑到边界而不相交示例](images/common_widget_func/clipping_clip_to_bounds_intersect.png)
        *在此示例中，有一个`Scroll Box`部件，其剪辑设置为`Clip to Bounds` ，其子小部件之一的剪辑设置为`Clip to Bounds - Without Intersecting(Advanced)`*
    - **剪切到边界 - 始终（高级）** ：此剪切区域也剪切到边界，并且它始终与任何先前的剪切区域相交。
        这个剪切区域**<u>不能</u>**被忽略，它总是会剪切它的子区域。对于用户界面中的硬障碍很有用，您永远不希望动画或其他效果闯入/冲出该区域。
        ![始终剪辑到边界示例](images/common_widget_func/clipping_clip_to_bounds_always.png)
        *在此示例中，有 3 个主要小部件；
         `Canvas Panel`的裁剪设置为`Clip to Bounds - Always(Advanced)`
         `Canvas Panel`中的`Scroll Box` ，其剪辑设置为`Clip to Bounds`
         `Scroll Box`的`Button` ，其剪辑设置为`Clip to Bounds - Without Intersecting(Advanced)`*
    - **按需（高级）** ：当所需大小大于布局中分配的几何图形时，此剪切区域将剪切到其边界。
        如果它确实发生在需要剪裁的地方，那么它将被视为*“始终”* 。根据源代码，此模式主要是为文本添加的，因为当文本被放置在容器内时，容器最终会调整大小以无法支持​​文本的长度。
        ![按需剪辑示例](images/common_widget_func/clipping_on_demand.gif)
        *在此示例中，我们有一个`Vertical Box`小部件，其剪辑设置为`Inherit` ”，还有一个来自 Epic 通用 UI 插件的自定义`Textblock`小部件，该小部件的剪辑设置为`On Demand(Advanded)` ，以便文本可以滚动并正确剪辑（此示例也可找到）在 Epic 的 Context Examples 项目中的 Common UI 示例图中）*
- **导航**：您可以在此处添加导航挂钩，以便在使用虚幻的聚焦框架时导航到哪个小部件。您可以在运行时在蓝图和 C++ 中设置这些。
    有关导航流程的更详细说明，请参阅[虚幻的聚焦系统](#unreals-focusing-system)部分。
    ![导航概览](images/common_widget_func/navigation_overview.png)
    - **Escape** ：此导航类型将导航到另一个小部件或尝试到达任何其他小部件，并在朝该方向导航时逃出此小部件的边界。
        ![退出导航示例](images/common_widget_func/navigation_escape.png)
    - **Stop** ：当尝试朝该方向导航出此小部件时，导航将停止。
        ![停止导航示例](images/common_widget_func/navigation_stop.png)
    - **Wrap** ：导航将尝试换行到该小部件的相反边界（例如，在垂直框中，导航到底部小部件并在最后一个小部件上设置向下的换行设置会将导航发送到垂直框的顶部小部件） 。
        ![环绕导航示例](images/common_widget_func/navigation_wrap.png)
    - **Explicit** ：当沿该方向导航到特定选择的小部件时，导航到该小部件。
        ![显式导航示例](images/common_widget_func/navigation_explicit.png)
        *在设计器中选择它时，您只能选择在编辑器中编辑了名称的其他小部件，否则在代码中您可以只提供小部件本身，无论它是否被重命名。*
    - **自定义**：允许您使用可以返回小部件或不返回小部件（模拟*“停止”*导航类型）的函数来覆盖要导航到的小部件。适用于从该**小部件**导航或从该小部件导航到该特定方向的另一个小部件时。
        ![自定义导航示例](images/common_widget_func/navigation_custom.png)
    - **自定义边界**：允许您使用可以返回小部件或不返回小部件（模拟*“停止”*导航类型）的函数来覆盖要导航到的小部件。适用于从特定方向的另一个小部件导航**到此**小部件时，
        因此，如果方向是向左并且您将其设置为自定义边界，那么当您从另一个小部件导航到这个小部件并且它是向左方向时，它将运行它绑定到的这个函数。
        ![自定义边界导航示例](images/common_widget_func/navigation_custom_boundary.png)
- **流向**：对于本地化，允许您设置此小部件的流向是从左到右还是从右到左。
    只有某些小部件实际使用它，但它存在于所有小部件中，以防您想根据流向添加自己的功能。例如，文本小部件可以从左到右/从右到左翻转，具体取决于其流向以及指定其流向的语言。
    - **Inherit** ：继承父窗口小部件设置的流向。
    - **文化**：开始使用当前文化布局方向首选项来布局小部件，翻转流的方向性。
    - **从左到右**：强制从左到右的布局流。
    -  **从右到左**：强制从右到左布局流。
        ![流向偏好](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/common_widget_func/flow_direction_preference.png?raw=true)

**[<span>⬆</span>返回顶部](#table-of-contents)**
<a name="umg-in-relation-to-levels-worlds"></a>

## 8.0 UMG 与关卡/世界的关系

<a name="hud-actors"></a>

### 8.1 HUD 参与者

HUD Actor 是仅通过游戏模式生成到关卡中的演员（我不建议手动生成它们，除非您覆盖游戏模式功能并知道自己在做什么）。这些参与者不会复制，默认情况下隐藏，并且仅在每个本地玩家控制器上生成（以考虑分屏），并且不会在专用服务器上生成。

随着虚幻引擎 4 的发布，HUD Actor 的用途发生了变化（虚幻引擎 5 保持不变），而在虚幻引擎 3 中，HUD Actor 曾经是关卡中 UI 元素功能的主要来源。一个播放器，但随着 UMG 和 Slate 的发布，这导致 HUD Actor 成为 UMG/Slate UI 元素的管理器对象，而不是驱动 UI 功能的各个方面，但它们对于您想要如何构建代码来说是完全可选的。

<a name="hud-drawing"></a>

#### 8.1.1 HUD 绘图

HUD Actor 与特定的玩家控制器绑定，并具有调试模式信息，以及手动将 UI 元素绘制到屏幕的功能。以下是可绘制 UI 元素的列表：

- 文本：在视口画布上绘制字符串。
- 线：在 2D 屏幕空间中的视口画布上绘制 2D 线。
    - 2D 线：使用像素坐标在视口画布上绘制 2D 线（由于 Unreal 使用 Slate 单位，其精度可能低于屏幕空间）。
    - 3D Line：告诉世界的调试画线系统在世界空间中画一条线。
- 矩形：在视口画布上绘制无纹理的四边形（正方形/矩形）。
- 纹理：在视口画布上绘制带纹理的四边形（正方形/矩形）。
    - 简单纹理：在视口画布上绘制纹理四边形（正方形/矩形），假设纹理像素密度为 1:1。
- 材质：在视口画布上绘制材质纹理的四边形（正方形/矩形）。
    - 材质简单：在视口画布上绘制材质纹理的四边形（正方形/矩形），但假设 UV 为 (0.0, 0.0) 和 (1.0, 1.0)。
    - 材质三角形：在视口画布上绘制材质纹理的三角形。

> 需要注意的是，HUD Actor 绘制图像和其他 UI 元素的方式是跳过 UMG 的一些步骤，直接获取画布并告诉它绘制项目。

<a name="hud-hitboxes"></a>

#### 8.1.2 HUD 碰撞盒

HUD Actor 的一个有趣功能是，它们能够知道屏幕的某些部分是否被鼠标悬停/单击/按下和释放。他们这样做的方法是使用一个名为`FHUDHitBox`的类。

`FHUDHitBox`是一个仅 C++ 的类，具有以下属性：

- Coords `FVector2D` ：命中框左上角的坐标。
- Size `FVector2D` ：命中框的大小。
- 名称`FName` ：命中框的名称。
- bConsumesInput `boolean` ：不消耗游戏的实际输入。
    - True：防止对其他命中框进行命中检查。
    - False：允许命中检查传递到其他命中框。
- Priority `int32` : 命中框的优先级（也可以被认为是 Z-Order）。较高的命中框优先。

使用`FHUDHitBox`的好处是 HUD 具有与之配合的鼠标事件，每个事件提供 HitBox 的名称：

- 单击：单击命中框时发生。
- 释放：当不再单击点击框时发生。
- 开始光标悬停：当鼠标悬停在命中框上时发生。
- End Cursor Over：当鼠标不再位于命中框上时发生。

HUD Actor 本质上有一个 HUD HitBox 数组，可以在运行时添加/删除。该数组是所有 HUD Actor 中的公共变量，但只能在 C++ 中访问： `HitBoxMap` （我知道它说的是地图，但它不是地图数组）。
添加 HitBox 有两种方法：

- 调用`AddHitBox`并输入参数以在函数内创建`FHUDHitBox`类。
- 手动创建`FHUDHitBox`类并将其添加到`HitBoxMap`数组中。

要删除 HitBox，只需将其从`HitBoxMap`数组中删除即可。

<a name="widget-components"></a>

### 8.2 小部件组件

Widget 组件是`UMeshComponent` （actor 组件，是可以渲染网格的原始组件），它基本上在世界中创建一个程序静态网格，其纹理是 widget 的绘制纹理（您可以使用`GetRenderTarget`访问该渲染目标）。

> 小部件组件在专用服务器上不勾选。
> 这是显而易见的，因为组件的大部分功能是更新用户小部件的呈现。
> 该组件还处理基于用户小部件的碰撞，因此如果该小部件未在专用服务器上生成，则该组件的碰撞将无法正常工作。

使用小部件组件的一个重要性能影响是每个小部件组件都是在组件的更新时更新的渲染目标。
因此，如果您有 100 个高分辨率的小部件组件，那么每帧都会更新 100 个渲染目标（您可以在绘制时让组件勾选，但这仍然占用 GPU 内存）。
为了避免此类 GPU 内存影响，以下是一些常见情况和解决方案（这些不是解决这些问题的唯一方法，这些只是我自己的经验的建议）：

- 情况一：NPC头顶上方有大量图片或进度条
    - 解决方案：使用具有图像纹理参数的材质的静态网格物体组件，以及通过基于纹理或基于数学的方法创建进度条的材质（例如，建议使用 Unreal 查找 SDF）。
- 情况 2：在世界范围内渲染指示器，但随着玩家的摄像机靠近/远离指示器位置而缩放。
    - 解决方案：创建一个材质来忽略深度通道，在最新版本的虚幻引擎中还有一个布尔值来跳过运动模糊通道，但您可以通过修改以前版本中的引擎来做到这一点。您可以使用静态网格物体组件或粒子系统来绘制静态网格物体、轮廓并得出您自己的结论。

对于与小部件组件一起使用的建筑材料，以下是自动与小部件组件一起使用的纹理参数：

- `SlateUI` ：输入小部件的渲染目标。
- `TintColorAndOpacity` ：输入小部件组件的 TintColorAndOpacity 属性。
- `OpacityFromTexture` ：输入小部件组件的 OpacityFromTexture 属性。

需要记住的一个重要方面是，虽然这是一个组件，但它实际上创建了一个用户小部件。
要访问用户小部件，您可以使用`GetUserWidgetObject` （您也可以使用`GetSlateWidget`来获取与其绑定的`SWidget` ）。

> 您不能在 actor 的构建脚本中使用`GetUserWidgetObject` ，因为该小部件仅在`BeginPlay`中有效。

默认情况下，它从游戏实例中获取第一个本地玩家：

```c++
if (UWorld* LocalWorld = GetWorld())
{
	UGameInstance* GameInstance = LocalWorld->GetGameInstance();
	check(GameInstance);

	return GameInstance->GetFirstGamePlayer();
}
```

这可以通过调用`SetOwnerPlayer`并输入`ULocalPlayer`来更改。

要从小部件组件上的世界位置获取 2D 平面小部件空间位置，您可以使用`GetLocalHitLocation` ，它通过执行以下数学运算来计算出来：

> 1. 通过使用`InverseTransformPosition`将世界位置转换为小部件组件的相对空间
> 2. 构建一个简单的 2D 位置，其中 X 轴为相对位置的负 Y 轴，相对位置的负 Z 轴为 2D Y 轴。
> 3. 将 2D X 轴偏移（添加）当前绘制尺寸的 X 轴 * 枢轴的 X 轴。
> 4. 将 2D Y 轴偏移（添加）当前绘制尺寸的 Y 轴 * 枢轴的 Y 轴。
> 5. 缓存 2D 位置除以当前绘制尺寸的标准化位置。
> 6. 将 2D 位置的 Y 轴更新为当前绘制尺寸的 Y 轴 * 标准化位置的 Y 轴，这是为了考虑抛物线失真。

<a name="widget-interaction-components"></a>

#### 8.2.1 Widget交互组件

为了能够与小部件组件进行交互，有一个小部件交互组件
旨在模拟用户输入和鼠标指针（或虚拟指尖）输入作为与小部件交互的激光笔风格。

每个widget交互组件都有一个与之关联的虚拟用户，负责向平板小部件提供输入。<br>激活后，该组件将创建一个实际的`FSlateUser`来模拟输入等。<br>默认情况下，引擎将使用 Slate 用户索引 8（允许的最大 Slate 用户数量）而不是 0 并从那里递增，<br>允许slate用户和虚拟用户不发生冲突。

小部件交互组件将跟踪滴答声以确定其与哪个小部件交互。
以下是组件的刻度帧顺序，用于确定`UWidgetInteractionComponent::TickComponent`中其线路跟踪所针对的组件：

> 1. `UWidgetInteractionComponent::SimulatePointerMovement`
>     1. 检查我们是否可以通过检查`bEnableHitTesting`进行命中测试
>     2. 检查组件是否能够在`CanSendInput`中发送输入
>         1. slate 应用程序是否已初始化且虚拟用户是否已设置。
>     3. `UWidgetInteractionComponent::DetermineWidgetUnderPointer`
>         1. 缓存之前悬停的小部件组件。
>         2. `UWidgetInteractionComponent::PerformTrace`
>             1. 基于`InteractionSource`类型的线路跟踪：
>             - World：通过`TraceChannel`在交互组件位置的正向方向上进行多线跟踪，
>                 忽略拥有者 Actor 中的任何组件，因此它不会击中自身（不包括小部件组件）。
>             - 鼠标：通过`TraceChannel`从从屏幕到世界的鼠标位置进行多线跟踪。
>             - 中心屏幕：通过`TraceChannel`从视口的中心位置进行多线跟踪，从屏幕到世界进行反投影。
>             - Custom：使用`CustomHitResult`的属性，可以在运行时设置。
>                 关于此结构的一个重要注意事项是，在第一个刻度帧中，它可能尚未设置，因为调用此结构后可能会发生蓝图刻度。
>                 您可以在 BeginPlay 中设置它，并且会提前为第一个刻度帧设置好。
>             1. 如果我们不使用自定义`InteractionSource`类型，则通过不可见的小部件进行过滤。
>             2. 获取命中小部件组件并输入命中结果，小部件组件将从那里返回小部件空间中的 2D 命中位置。
>             3. 从返回的小部件空间命中位置查找`FWidgetPath` 。
>             4. 返回`PerformTrace`的跟踪结果。
>         3. 告诉新悬停的小部件组件重绘。
>         4. 从跟踪中检索到的小部件路径中迭代排列的小部件列表，并根据最终的小部件更新这些标志：
>             - `bIsHoveredWidgetInteractable` ：如果小部件是可交互的。
>             - `bIsHoveredWidgetFocusable` ：如果小部件是键盘可聚焦的。
>             - `bIsHoveredWidgetHitTestVisible` ：如果小部件是可测试的。
>         5. 如果新悬停的小部件组件与先前悬停的小部件组件不同，则告诉先前的组件重绘。并为其他系统广播`OnHoveredWidgetChanged` 。
>         6. 从跟踪中返回小部件路径。
>     4. 通知 slate 应用程序（发送到引擎的其余部分）正在该小部件上模拟输入，或者如果我们不再悬停任何内容，则告诉它指针已移离旧小部件。

<a name="widget-components-rendering"></a>

#### 8.2.2 Widget组件如何渲染

为了将用户小部件绘制到`UTextureRenderTarget2D` ，小部件组件执行的一般过程（以及额外的工作，因为它是一个组件，因此必须提供场景代理等）；方法是创建一个`FWidgetRenderer` ，然后从用户小部件获取slate widget，并让widget渲染器将其绘制为纹理。<br>以下是使用小部件渲染器从`UUserWidget`创建纹理的示例代码（ `FWidgetRenderer::DrawWindow`和`FWidgetRenderer::DrawWidget`有多种实现，因此这是方法之一）：

```c++
bool UExampleFunctionLibrary::DrawWidgetToTarget(UTextureRenderTarget2D*& DrawnWidgetRenderTarget,
    UUserWidget* WidgetToRender, const FVector2D DrawSize, const float DeltaTime)
{
    // Reset the variable for Blueprint users to avoid reusing previous calls
    DrawnWidgetRenderTarget = nullptr;
    // Are we using a valid widget to grab from for the render target
    if(!IsValid(WidgetToRender))
    {
        UE_LOG(LogExampleFunctionLibrary, Error, TEXT("UExampleFunctionLibrary::DrawWidgetToTarget: Inputted NULL WidgetToRender"));
    	return false;
    }
    // Make sure we're using a valid draw size
    if(DrawSize.X <= 0 || DrawSize.Y <= 0)
    {
        UE_LOG(LogExampleFunctionLibrary, Error, TEXT("UExampleFunctionLibrary::DrawWidgetToTarget: Inputted INVALID DrawSize(%s)"), *DrawSize.ToString());
    	return false;
    }
    // Create the render target object using the user widget as the outer object(as a safety measure)
    DrawnWidgetRenderTarget = NewObject<UTextureRenderTarget2D>(WidgetToRender);
    // Setup the render target's size and formatting
    DrawnWidgetRenderTarget->InitCustomFormat(DrawSize.X, DrawSize.Y,
    	FSlateApplication::Get().GetRenderer()->GetSlateRecommendedColorFormat(),
    	true); // If we set this to false then it will not use linear gamma, but then we wouldn't get accurate coloring
    
    // This is the object that handles talking with the slate renderer to draw widgets as textures
    FWidgetRenderer* WidgetRenderer = new FWidgetRenderer(true, false); // FWidgetRenderer(bool bUseGammaCorrection = false, bool bInClearTarget = true)
    WidgetRenderer->DrawWidget(DrawnWidgetRenderTarget,
        WidgetToRender->TakeWidget(),
        DrawSize,
        DeltaTime,
        false); // THIS PARAMETER IS EXTREMELY IMPORTANT, this is for if you want to immediately update the render target.
    // bDeferRenderTargetUpdate: Whether or not the update is deferred until the end of the frame when it is potentially less expensive to update the render target.
    
    // Flush any queued rendering commands to immediately have the GPU draw the widget and get the texture information filled out
    FlushRenderingCommands(); // This is ANOTHER way of forcing the GPU to update the render target.
    
    // HINT HINT ^^^
    // Deferred cleanup of the widget renderer to be deleted AFTER the render command queue has been flushed
    BeginCleanup(WidgetRenderer);
    return true;
}
```

为了在世界空间中绘制小部件，它使用上述方法，但它不是直接使用`DrawWidget`而是使用`DrawWindow` 。

为了在屏幕空间中绘制小部件，它将把用户小部件添加到第二个游戏层小部件，该小部件仅处理名为`FWorldWidgetScreenLayer`的小部件组件，该组件直接与`SWorldWidgetScreenLayer`通信。

**[<span>⬆</span>返回顶部](#table-of-contents)**
<a name="dev-debug-tools"></a>

## 9.0 UMG/Slate 开发和调试工具

虚幻引擎具有用于 UMG 和 Slate 的开发和调试工具，可帮助创建 UI，并可在编辑器和打包开发版本（包括控制台）中使用。

<a name="debug-console-commands"></a>

### 9.1 调试控制台命令

[Console Slate 调试器的官方文档](https://docs.unrealengine.com/latest/INT/console-slate-debugger-in-unreal-engine/)（也位于[外部链接页面](EXTERNAL_LINKS.md)）。

- 请参阅[Slate 控制台调试器控制台命令](#slate-console-debugger)部分，了解 Slate 调试器特定的控制台命令。
- `Slate.HitTestGridDebugging [0/1]` ：用于显示 UMG/Slate 聚焦命中测试网格的标志。
- `SlateDebugger.Invalidate.[Disable/Enable]` ：启用或禁用失效调试器可视化工具。
- 安全区命令：
    - `r.DebugSafeZone.TitleRatio 0.96` ：默认值为`1.0` 。在没有定义安全区域的平台上`FDisplayMetrics::GetDisplayMetrics`将返回的安全区域比率介于 0-1 之间。
    - `r.DebugActionZone.ActionRatio 0.96` ：默认值为`1.0` 。在没有定义 0-1 之间安全区域的平台上`FDisplayMetrics::GetDisplayMetrics`将返回的操作区域比率。
    - `r.DebugSafeZone.Mode [integer between 0 and 2]`
        - `0` ：不显示安全区覆盖
        - `1` ：显示标题安全区的叠加层
        - `2` ：显示操作安全区的叠加层
- `Slate.ThrottleWhenMouseIsMoving [0/1]` ：默认为`false` 。是否尝试根据鼠标光标移动来提高 UI 响应能力。
    - `True` ：允许根据鼠标移动活动进行限制。
- `Slate.TargetFrameRateForResponsiveness [integer]` ：默认值为每秒`35`帧。在我们认为编辑器能够“响应”以获得流畅的 UI 体验之前，所需的最低持续平均帧速率。
- `Slate.AllowSlateToSleep [0/1]` ：当没有活动计时器且用户空闲时，Slate 是否应该进入睡眠状态。
- `Slate.SleepBufferPostInput [float]` ：默认值为`0.0` 。在 Slate 进入睡眠状态之前，在没有任何用户操作的情况下必须经过的时间量（假设没有活动的计时器）。
- `Slate.RequireFocusForGamepadInput [0/1]` ：默认为`false` 。如果应用程序当前未处于活动状态，引擎是否应忽略游戏手柄输入。
- `Slate.Transform.FullscreenMouseInput [0/1]` ：默认值为`true` 。设置 true 来转换鼠标输入，以考虑显示器本身不支持的全屏分辨率下的视口拉伸。
- `Slate.EnableTooltips [0/1]` ：如果平台需要 UI 工具提示，则默认为`true` ，否则默认为`false` 。是否允许生成工具提示。
- `Slate.TriggerInvalidate` ：触发所有小部件的全局无效。不在运输版本上运行。

<a name="widget-reflector"></a>

### 9.2 部件反射器

[Widget Reflector 的官方文档](https://docs.unrealengine.com/latest/INT/using-the-slate-widget-reflector-in-unreal-engine/)（也位于[外部链接页面](EXTERNAL_LINKS.md)）。

> 要打开小部件反射器，您可以导航到`Tools/Debug/Widget Reflector` ，或者在使用虚幻引擎的源版本时将小部件反射器构建为单独的应用程序。

widget Reflector工具旨在帮助开发者优化和调试UI，允许开发者调试：

- 小部件层次结构：小部件的层次结构，显示小部件的父级和子级。使用小部件层次结构时，您可以检查以下属性：
    - 小部件名称
    - 前景可见度(FG Visibility)
    - 重点
    - 剪裁
    - 来源：小部件的源代码位置，以便于访问。
    - 地址：slate 在计算小部件层次结构和树时使用的原始小部件路径。
- widget详细信息：widget详细信息，例如可见性、焦点等（slate widget作为公开属性公开的任何内容）。
- 小部件事件：
    - 输入
    - 重点
    - 导航
    - 警告
    - 鼠标捕捉
- 小部件导航和点击测试网格
- 无效
- 小部件更新
- 小部件绘制
- 剪裁
- 剔除
- 缓存

![小部件反射器示例](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/widget_reflector_example.png?raw=true)

> 对于控制台或手机等调试设备，您可以使用引擎内的“远程会话”插件（正式名称为“Slate Remote”）将调试器连接到设备，以使用小部件反射器实时调试 Slate 和 UMG。

<a name="slate-console-debugger"></a>

### 9.3 Slate 控制台调试器

Slate 控制台调试器是控制台命令的列表，可用于调试 Slate 的不同部分以达到调试目的。每个控制台命令都以`SlateDebugger.` ，当启用一个选项时，这将导致其调试信息被打印到输出日志中。

好消息是您可以从小部件反射器启用这些标志，这样您就不必输入差异调试命令。 ![Widget Reflector Slate 控制台调试器标志](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/widget_reflector_console_debugger.png?raw=true)

以下是每个 Slate 调试器控制台命令的列表：

- Slate Trace，位于`SlateTrace.cpp` ：
    - `SlateDebugger.bCaptureRootInvalidationCallstacks [0/1]` ：只要某个小部件是失效的根本原因，就捕获调用堆栈以获取 slate 见解（属于 Unreal Insights 的另一个调试工具）。
- 事件，位于`ConsoleSlateDebugger.cpp`中：
    - `SlateDebugger.Event.Start` OR `SlateDebugger.Start` ：启动事件调试器。
    - `SlateDebugger.Event.Stop` OR `SlateDebugger.Stop` ：停止事件调试器。
    - `SlateDebugger.Event.LogWarning` ：记录警告事件。
    - `SlateDebugger.Event.LogInputEvent` ：记录输入事件。
    - `SlateDebugger.Event.LogFocusEvent` ：记录焦点事件。
    - `SlateDebugger.Event.LogAttemptNavigationEvent` ：记录尝试的导航事件。
    - `SlateDebugger.Event.LogExecuteNavigationEvent` ：记录执行的导航事件。
    - `SlateDebugger.Event.LogCaptureStateChangeEvent` ：记录输入捕获状态更改事件。
    - `SlateDebugger.Event.LogCursorChangeEvent` ：记录光标更改事件。
    - `SlateDebugger.Event.CaptureStack` ：当发生事件时，这会切换捕获和记录调用堆栈。
    - `SlateDebugger.Event.InputRoutingModeEnabled` ：这会切换记录输入事件所采用的路线。
    - `SlateDebugger.Event.SetInputFilter [Filter]` ：切换特定输入过滤器：
        - 过滤器：
            - `MouseMove`
            - `MouseEnter`
            - `MouseLeave`
            - `PreviewMouseButtonDown`
            - `MouseButtonDown`
            - `MouseButtonUp`
            - `MouseButtonDoubleClick`
            - `MouseWheel`
            - `TouchStart`
            - `TouchEnd`
            - `TouchForceChanged`
            - `TouchFirstMove`
            - `TouchMoved`
            - `DragDetected`
            - `DragEnter`
            - `DragLeave`
            - `DragOver`
            - `DragDrop`
            - `DropMessage`
            - `PreviewKeyDown`
            - `KeyDown`
            - `KeyUp`
            - `KeyChar`
            - `AnalogInput`
            - `TouchGesture`
            - `MotionDetected`
    - `SlateDebugger.Event.DisableAllInputFilters` ：禁用所有启用的输入过滤器。
    - `SlateDebugger.Event.EnableAllInputFilters` ：启用所有输入过滤器。
    - `SlateDebugger.Event.SetFocusFilter [Filter]` ：切换特定焦点过滤器：
        - 过滤器：
            - `FocusChanging`
            - `FocusLost`
            - `FocusReceived`
    - `SlateDebugger.Event.ClearFocusFilters` ：禁用所有启用的焦点过滤器。
    - `SlateDebugger.Event.EnableAllFocusFilters` ：启用所有焦点过滤器。
- 断点，这对于其他工具的使用很有用，并且需要将调试器附加到编辑器以充当断点。位于`ConsoleSlateDebuggerBreak.cpp`中：
    - `SlateDebugger.Break.OnWidgetInvalidation Reason=[Reason][WidgetPtr][WidgetId]` ：当输入的小部件无效时中断。
        - 无效原因：
            - `Layout`
            - `Paint`
            - `Volatility`
            - `ChildOrder`
            - `RenderTransform`
            - `Visibility`
            - `AttributeRegistration`
            - `Prepass`
            - `PaintAndVolatility`
            - `LayoutAndVolatility`
    - `SlateDebugger.Break.OnWidgetBeginPaint [WidgetPtr][WidgetId]` ：在小部件开始绘制之前中断。
    - `SlateDebugger.Break.OnWidgetEndPaint [WidgetPtr][WidgetId]` ：在刚绘制小部件时中断。
    - `SlateDebugger.Break.RemoveAll` ：删除所有要中断的请求。
- 绘制，位于`ConsoleSlateDebuggerPaint.cpp`中：
    - `SlateDebugger.Paint.Start` ：启动绘制的小部件调试工具。用于显示已在该框架中绘制的小部件。
    - `SlateDebugger.Paint.Stop` ：停止绘制的小部件调试工具。
    - `SlateDebugger.Paint.Enable` ：切换（启动/停止）绘制的小部件调试工具以显示绘制的小部件。
    - `SlateDebugger.Paint.LogOnce` ：记录工具上次更新期间绘制的所有小部件的名称。
    - `SlateDebugger.Paint.MaxNumberOfWidgetDisplayedInList [MaxNumberOfWidgetsInList]` ：当 DisplayWidgetNameList 在工具中处于活动状态时将显示的小部件的最大数量。
    - `SlateDebugger.Paint.ToggleWidgetNameList` ：可切换选项，用于显示已在工具中绘制的小部件的名称。
    - `SlateDebugger.Paint.LogWarningIfWidgetIsPaintedMoreThanOnce` ：如果在工具的单个帧中多次绘制小部件，则切换记录警告。
    - `SlateDebugger.Paint.OnlyGameWindow` ：在工具中切换仅调试游戏窗口的小部件。
- 更新，位于`ConsoleSlateDebuggerUpdate.cpp` ：
    - `SlateDebugger.Update.Start` ：启动更新小部件调试工具以显示小部件何时更新。
    - `SlateDebugger.Update.Stop` ：停止更新小部件调试工具。
    - `SlateDebugger.Update.Enable` ：切换（启动/停止）更新小部件调试工具以显示小部件何时更新。
    - `SlateDebugger.Update.ToggleLegend` ：切换以在工具中显示颜色图例。
    - `SlateDebugger.Update.ToggleWidgetNameList` ：切换以显示工具中已更新的小部件的名称。
    - `SlateDebugger.Update.ToggleUpdateFromPaint` ：切换以同时显示没有更新标志但作为其他小部件的副作用而更新的小部件。
    - `SlateDebugger.Update.SetWidgetUpdateFlagsFilter` ：启用/禁用特定的小部件更新标志过滤器。
        - 过滤器，位于`WidgetUpdateFlags.h` ：
            - `None`
            - `Tick` ：Widget 具有勾选功能。
            - `ActiveTimer` ：Widget 有一个需要更新的活动计时器。
            - `Repaint` ：需要重新绘制，因为小部件脏了。
            - `VolatilePaint` ：需要重新绘制，因为小部件是易失性的。
            - `Any`
        - `SlateDebugger.Update.SetInvalidationRootIdFilter` ：切换以仅显示属于失效根的小部件。
        - `SlateDebugger.Update.OnlyGameWindow` ：切换为仅显示游戏窗口的调试信息。
- 失效，位于`ConsoleSlateDebuggerInvalidate.cpp` ：
    - `SlateDebugger.InvalidationRoot.Start` ：启动失效小部件调试工具。它显示无效的小部件。
    - `SlateDebugger.InvalidationRoot.Stop` ：停止失效小部件调试工具。
    - `SlateDebugger.InvalidationRoot.Enabled` ：切换（启动/停止）失效小部件调试工具以显示失效的小部件。
    - `SlateDebugger.InvalidationRoot.bShowLegend` ：切换以显示颜色图例。
    - `SlateDebugger.InvalidationRoot.bShowWidgetList` ：切换以显示失效小部件的名称。
    - `SlateDebugger.InvalidationRoot.bLogInvalidatedWidget` ：切换以将无效的小部件记录到控制台。
    - `SlateDebugger.InvalidationRoot.ThresholdPerformanceMS` ：对于`bUsePerformanceThreshold` ，在记录和/或显示无效小部件之前要达到的阈值（以毫秒为单位）。
    - `SlateDebugger.InvalidationRoot.bUsePerformanceThreshold` ：仅在性能低于阈值（以毫秒为单位）时才显示无效的小部件和/或记录它们。
    - `SlateDebugger.InvalidationRoot.SetInvalidateRootReasonFilter` ：启用/禁用特定的无效根本原因过滤器。
        - 过滤器，位于`SlateDebugging.h`中：
            - `None`
            - `ChildOrder`
            - `Root`
            - `ScreenPosition`
- 失效根，位于`ConsoleSlateDebuggerInvalidationRoot.cpp`中：
    - `SlateDebugger.InvalidationRoot.Start` ：启动失效根小部件调试工具。它显示无效根何时使用慢速或快速路径。
    - `SlateDebugger.InvalidationRoot.Stop` ：停止失效根小部件调试工具。
    - `SlateDebugger.InvalidationRoot.Enable` ：切换（启动/停止）失效根小部件调试工具以显示失效根何时使用慢速或快速路径。
    - `SlateDebugger.InvalidationRoot.ToggleLegend` ：切换以显示颜色图例。
    - `SlateDebugger.InvalidationRoot.ToggleWidgetNameList` ：切换以显示失效根的名称。

**[<span>⬆</span>返回顶部](#table-of-contents)**
<a name="input-framework-of-unreal-engine"></a>

## 10.0 Unreal Engine输入框架（涉及UMG/Slate）

<a name="input-flow-of-unreal-engine"></a>

### 10.1 虚幻引擎的输入流程

这是从最低级别到最高级别的输入的高级概述流程，按以下顺序列出，按照为每个用户路由输入的顺序（每个阶段都会调用虚幻引擎输入流的下一个阶段）：

1. **引擎心跳Tick**`FEngineLoop` ：虚幻引擎的心跳Tick，通知平台 SDK Tick/更新每一帧。
2. **平台的 API** `GenericApplication`和`FGenericApplicationMessageHandler` ：适用于 Windows/Mac/Xbox/Playstation/etc 的 SDK，它在引擎初始化时创建 Slate 应用程序，并向每个用户的每一帧发送输入。
3. **Slate 应用程序**`FSlateApplication` ：处理输入处理器、Slate UI 和游戏引擎之间的输入路由，以便游戏代码接收该输入。
4. **输入处理器***可选*`IInputProcessor` ：这些是 C++ 对象，可以从 Slate 应用程序内的输入预处理器列表中动态添加/删除，并在其他任何操作之前接收输入，并控制输入是被消耗还是继续向下路由。建议您为您的项目制作一个，因为它可以让您在引擎中的其他任何内容获取输入之前完全控制输入，引擎中甚至有一个名为**AnalogCursor**的 Epic 制作示例！即使您使用仅输入模式 UI 并在编辑器中接收输入，这也将获得输入！
5. **Slate UI Elements** `SWidget` ：屏幕上任何应接收输入并可以使用该输入的聚焦 UI 元素。
    - 这也是**UMG Widgets** `UWidget`将接收输入的地方，因为 UMG Widgets 只是 Slate Widgets 的基于 UObject 的包装器。
6. **游戏视口客户端**`UGameViewportClient` ：在查看代码时，如果 Slate 应用程序将其发送到 Slate 元素而没有将其发送到 Slate 元素，那么弄清楚输入路由到游戏视口客户端的位置可能会有点令人困惑，但基本上是在其核心级别被消耗，然后它被路由到视口小部件，这是一个 Slate 小部件（也是游戏最终渲染图像的视觉表示），然后被路由到游戏视口客户端，该客户端处理将该输入传播到其余部分游戏引擎并与游戏代码连接。此时，根据您使用的输入模式，输入模式游戏/输入模式游戏和 UI 将允许 pawn 接收输入。
7. **玩家控制器**`APlayerController` ：一旦游戏视口接收到输入，它必须经过一些检查以确保它可用于游戏代码，然后告诉玩家控制器将其添加到`ProcessPlayerInput`内的**输入堆栈**（从玩家控制器的 ProcessPlayerInput 中调用） `Tick` 。
8. **玩家输入**`UPlayerInput` ：玩家控制器具有的玩家输入对象，用于将输入路由到其输入堆栈上的 pawn 和其他对象。
9. **输入组件**`UInputComponent` ：这是接收游戏代码输入的常用方法，也可以在 Epic 的官方文档中找到，它是每个与玩家控制器的输入堆栈连接的参与者上的一个对象，以接收通过游戏代码监听和接收输入的引擎。

![输入流程图](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/input_flow_diagram.png?raw=true)
*输入流程图*

<a name="input-components"></a>

### 10.2 输入组件

输入组件是`UActorComponent` ，存在于所有 actor( `AActor` ) 中。这些组件将作为项目中的绑定与 AxisMappings 和 ActionMappings 链接以运行功能。每个绑定都可以使用输入事件，从而阻止输入堆栈上的其他组件处理同一输入事件。输入组件使参与者能够将输入事件绑定到由两个类自动处理的委托函数：

> - `APlayerController` :: 处理构建输入堆栈并控制顺序和优先级。
> - `UPlayerInput` :: 处理广播这些委托函数并处理确定是否正在使用绑定。
>
> 这是每帧输入组件的实际操作顺序。*下图*
>
> 1. [ `APlayerController::TickPlayerInput` ]
>     1. [ `UPlayerInput::Tick` ]
>     2. 收集鼠标悬停事件。
>     3. 收集触摸事件。
>     4. [ `APlayerController::ProcessPlayerInput` ]
>         1. [ `APlayerController::BuildInputStack` ]：构建输入组件的堆栈以及它们的处理顺序。可以覆盖以控制顺序。
>         2. [ `UPlayerInput::ProcessInputStack` ]：实际开始处理输入。
>             1. [ `APlayerController::PreProcessInput` ]
>             2. 复制非轴键的状态。
>                 1. 开始从上到下遍历输入组件堆栈；一个接一个（现在考虑我们正在使用单个输入组件，但在一个循环中，直到没有更多的输入组件）。
>                     1. 构建此输入组件的键盘映射，以了解哪些操作/轴与特定键绑定一起使用以创建输入线。
>                     2. 查找触摸绑定并确定是否发生了输入操作绑定。如果是这种情况，请跟踪它。
>                     3. 查找手势绑定并确定是否发生输入操作绑定。如果是这种情况，请跟踪它。
>                     4. 查找轴绑定并确定是否发生输入轴绑定。如果是这种情况，请跟踪它。
>                     5. 决定是否使用密钥（基于构建键盘映射和到达此点之间完成的簿记）。
>             3. 将每个输入组件的轴绑定重置为零。
>             4. 将输入操作广播到应该运行功能的输入组件。
>             5. 将输入轴广播到应该运行功能的输入组件。
>             6. [ `APlayerController::PostProcessInput` ]
>             7. [ `UPlayerInput::FinishProcessingPlayerInput` ]：如果现在保持输入，则通过保存来完成此帧的输入处理。清理下一帧的值。
>             8. 清除所有广播的绑定。
>         3. 重置下一帧的输入堆栈。
>     5. [ `APlayerController::ProcessForceFeedbackAndHaptics` ]

![输入组件操作顺序](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/input_component_order_operations.png?raw=true)
 *`APlayerController`内部从`TickPlayerInput`开始的级别中如何勾选和处理输入的操作顺序*

输入组件还可以具有优先级堆栈，以便更高优先级的参与者可以使用输入。
输入组件的优先级堆栈如下（优先级最高的在前）：

1. 启用“接受输入”的参与者，从最近启用到最近最少启用。
    1. 要将 Actor 移动到该堆栈中的优先级顶部，您可以重新启用其“接受输入”值，它将被移动到优先级堆栈的顶部。
2. 玩家控制器
3. 关卡脚本
4. 棋子

![输入组件堆栈](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/input_component_stack.png?raw=true)
*输入组件堆栈，由[Epic 文档](https://docs.unrealengine.com/5.0/en-US/input/)提供*

<a name="input-event-types"></a>

### 10.3 输入事件类型

每次发生输入时，都会为从**FInputEvent**继承的每种类型的输入使用一个结构体：

- **FInputEvent** ：所有鼠标、按键事件、触摸/运动事件的基本结构。
    - **修饰键**`FModiferKeysState` ：在此帧期间发生此事件时修饰键的状态。
    - **Is Repeat** `bool` ：用于了解此输入事件是否自动重复（保持然后重复触发）的标志。
    - **用户索引**`uint32` ：引起此事件的**Slate 用户**的整数索引。
    - **事件路径**`const FWidgetPath*` ：与此事件一起发送的事件的路径。

所有其他事件类型都来自`FInputEvent` ：

- **FKeyEvent** `FInputEvent` ：按下/释放键盘/游戏手柄的按键操作。它被传递给处理此键输入的事件处理程序。
    - **Key** `FKey` ：按下的键的名称。
    - **CharacterCode** `uint32` ：所按下按键的 Unreal 友好版本的字符代码。如果没有考虑到，则字符键返回零。
    - **KeyCode** `uint32` ：从硬件/SDK 接收的原始字符代码。
- **FAnalogInputEvent** `FKeyEvent` ：描述模拟键值以说明摇杆。
    - **AnalogValue** `float` ：0-1 值代表摇杆轴，0 = 未按下，1 表示完全按下。
- **FCharacterEvent** `FInputEvent` ：输入 UTF-16 代码（Unicode 16 位编码）的键盘操作，用于 OnKeyChar 功能。
    - **字符**`TCHAR` ：按下的字符。
- **FPointerEvent** `FInputEvent` ：鼠标/触摸输入（因为触摸被视为鼠标输入，并且仅使用鼠标左键作为触摸输入），用于按下/释放/移动等。其中一些值您可能甚至不需要使用，但它提供了很多有用的信息。
    - **ScreenSpacePosition** `FVector2D` ：此指针事件的当前屏幕空间位置。
    - **LastScreenSpacePosition** `FVector2D` ：此指针事件的最后一帧的屏幕空间位置。
    - **CursorDelta** `FVector2D` ：当前和最后一个屏幕空间位置之间的距离。
    - **PressedButtons** `const TSet<FKey>*` ：此指针事件正在使用的当前按下的鼠标按钮。
    - **EffectingButton** `FKey` ：此指针事件所代表的鼠标按钮（触摸事件将始终使用鼠标左键）。
    - **PointerIndex** `uint32` ：哪个指针（手指）索引适用于该 Slate 用户。
    - **TouchpadIndex** `uint32` ：当使用带有触摸板鼠标的笔记本电脑时，哪个指针（手指）索引适用于该 Slate 用户。
    - **Force** `float` ：在此触摸板/指针输入上施加了多少力。
    - **IsTouchEvent** `bool` ：这是基于触摸（包括触控板）的指针事件还是基于鼠标的事件。
    - **GestureType** `EGestureType` ：此指针事件使用哪种类型的手势，例如滑动、滚动、放大、旋转、长按等。
    - **WheelOrGestureDelta** `FVector2D` ：自上次相同类型的手势事件以来手势值的变化。
    - **IsDirectionInvertedFromDevice** `bool` ：手势增量是否反转。
    - **IsTouchForceChanged** `bool` ：此事件是否为特殊的力改变触摸事件。
    - **IsTouchFirstMove** `bool` ：此事件是否为特殊的首次移动触摸事件。
- **FMotionEvent** `FInputEvent` ：使用内部陀螺仪描述触摸板事件，例如按下/移动/抬起/旋转等。
    - **Tilt** `FVector` ：设备/控制器的当前倾斜度。
    - **RotationRate** `FVector` ：设备/控制器的旋转速度。
    - **重力**`FVector` ：设备/控制器报告的现实世界中的重力方向（向下指向地面）。
    - **Acceleration** `FVector` ：设备/控制器的 3D 加速度。
- **FNavigationEvent** `FInputEvent` ：左/右/上/下的焦点导航事件，传递给**用户焦点**。
    - **NavigationType** `EUINavigation` ：此事件的导航方向，如果是上/下/左/右/等。
    - **NavigationGenesis** `ENavigationGenesis` ：用于了解此导航事件来自何处（由其引起）的枚举，例如键盘/控制器/用户。

<a name="input-modes"></a>

### 10.4 输入模式

**WidgetBlueprintLibrary**有 3 个函数用于在玩家控制器上设置称为**输入模式的**功能，这三个状态解释了与步骤 6 有关的输入流中实际发生的情况；游戏视口客户端。重要的是要了解，没有专用的输入模式，但这些是通过玩家控制器更改游戏视口客户端上的值的快捷方式。

对输入模式值所做的任何更改都将在关卡/地图旅行之间持续存在，无论您是使用默认的虚幻引擎功能来更改它还是您自己在游戏视口客户端中手动更改了这些值。

- **仅输入模式 UI** ：基本上告诉游戏视口客户端忽略输入，因此游戏视口客户端接收到的任何输入都将被丢弃，这样输入就不会路由到输入流中的后续步骤，并释放鼠标，以便您可以在视口（如果**鼠标锁定模式**如此规定，则在视口之外）。
- **仅游戏输入模式**：告诉游戏视口客户端它可以接收输入，因此当游戏视口客户端收到这些输入时，它们会正确路由到输入流中的后续步骤，并锁定鼠标，使其无法在视口中单击。
- **输入模式游戏和 UI** ：告诉游戏视口客户端它可以接收输入并释放鼠标，以便您可以在视口中单击（如果**鼠标锁定模式**如此规定，则可以在视口外部单击）。

[](https://youtu.be/ktIDz1wCe0Y)![输入模式视频示例](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/video_thumbnails/input_modes_video_thumbnail.png?raw=true)

鼠标锁定模式`EMouseLockMode`是鼠标光标锁定（意味着光标不能移出边界）到视口的方式，具体取决于其所处的模式：

-  **不锁定**：不将鼠标光标锁定到视口。
    ![不锁定示例](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/mouse_lock_modes/mouse_lock_mode_do_not_lock.png?raw=true)
-  **锁定捕获**：仅在捕获鼠标（单击或与视口交互）时将鼠标光标锁定到视口。
    ![锁定捕获示例](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/mouse_lock_modes/mouse_lock_mode_lock_capture.png?raw=true)
-  **始终锁定**：始终将鼠标光标锁定在视口上，不允许其离开视口。
    ![始终锁定示例](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/mouse_lock_modes/mouse_lock_mode_lock_always.png?raw=true)
-  **锁定全屏**：如果处于全屏状态，请始终锁定光标。
    ![锁定全屏示例](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/mouse_lock_modes/mouse_lock_mode_lock_fullscreen.png?raw=true)

> 我想指定鼠标锁定模式是基于每个视口的，它负责每个玩家视口的分屏，而窗口则负责整个窗口的所有视口。

**[<span>⬆</span>返回顶部](#table-of-contents)**
<a name="unreals-focusing-system"></a>

## 11.0 Unreal的聚焦系统

虚幻引擎关于 Slate/UMG 的聚焦系统是**用户聚焦**的概念，其中称为**Slate User**的特定用户一次只能聚焦一个**Slate Widget** 。

多个 Slate 用户可以专注于同一个Widget，但用户不能同时专注于两个Widget（这样做需要您自己构建该功能，但此时您可能正在与它斗争而不是工作有了它，可以节省管理您正在构建的游戏和自定义聚焦系统的时间）。

**Slate 应用程序**使用每个**Slate 用户**的整数索引以及用户当前关注的**Slate Widget**的小部件焦点路径来跟踪用户焦点，这些是该应用程序上用于说明分屏播放器的本地用户。焦点将在关卡/地图旅行之间持续存在，因此最好通过将焦点发送回游戏视口来重置焦点。

虚幻引擎的聚焦系统由 4 个关键要素组成：

- **Slate 应用程序**：它处理跟踪当前处于焦点的小部件（包括在焦点更改时发出通知）并告诉聚焦系统按下了输入。
- **HittestGrid** （是的，这就是该类的命名方式，我认为这是 HitTestGrid 的拼写错误）：通过使用函数 FindNextFocusableWidget 处理查找下一个可聚焦小部件并将其返回到 Slate 应用程序。
- **SWidget** ：这是所有小部件中的基本功能，应该被覆盖：
    - `OnKeyDown` ：当按下某个键并且焦点位于该小部件上时，这是该小部件应该执行的功能。
    - `OnNavigation` ：当小部件获得焦点/失去焦点等时。
- **导航配置**：根据输入确定要使用的导航方向。

<a name="navigation-grid-explanation"></a>

### 11.1 导航网格说明

> 为了可视化命中测试网格，控制台命令是`Slate.HitTestGridDebugging [0/1]`它也位于[UMG/Slate 的开发和调试工具](#dev-debug-tools)部分的[调试控制台命令](#debug-console-commands)中。

命中测试网格基本上是它处理导航的方式，它将有助于每次使用时导航到哪个小部件（因此每个用户都有自己的命中测试网格），导航仅沿着沿网格指定的方向计算（分辨率为 128） ，没有理由更改它，因为它检查小部件是否位于直线上均匀间隔的网格内），该网格由每个选择的小部件的可碰撞边界框组成（因此所需的大小和几何形状在这里发挥作用）并且这个网格是根据选择加入此命中测试网格的特定小部件填充的。

![命中测试网格示例 1](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/hittest_grid_1.png?raw=true) | ![命中测试网格示例 2](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/hittest_grid_2.png?raw=true)
:-: | :-:


*在此示例中，我们展示了按下导航方向时可以导航到哪些按钮。在这种情况下，按钮 2 将始终导航到最近的小部件，因为它可以导航到其下方的任何小部件。*

![导航网格调试视图](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/nav_grid_debug.png?raw=true)
*这是使用`Slate.HitTestGridDebugging 1`控制台命令时示例中导航网格的实际外观。*

当**导航创世**发生时，它基本上获取当前聚焦的小部件所在的单元格，然后根据导航方向，命中测试网格沿直线检查，通过扫描每个单元格以查看是否有小部件来找到可聚焦的小部件它包含在其中的边界框，并按指定的顺序运行一系列检查，如果它们未通过这些检查，则我们返回它的调试结果并跳过它：

1. **不相交**：如果Widget的边界矩形不与扫描相交。
2. **上一个Widget更好**：如果该Widget不比之前检查的Widget更接近，那么我们就会失败，因为前一个Widget更接近，所以总是优先选择最接近的Widget。
3. **不是后代**：如果小部件的边界导航规则不是 Escape 并且该小部件不是我们的边界条件小部件的后代（因此我们不应该首先导航到该小部件）。
4. **已禁用**：如果该小部件未启用。
5. **不支持键盘焦点**：此小部件不支持键盘焦点。

小部件通过这些测试后，它被保存为最佳小部件并保存其**Slate 矩形**（稍后用于导航），然后检查小部件边界导航规则是什么（显式、自定义、自定义边界、停止、换行）并每个不是 Escape 的句柄。一旦扫描到达屏幕的窗口边缘，它将把焦点发送到最好的小部件。

您还可以通过获取 Slate 应用程序并使用`OnFocusChanging()`访问器函数获取其`FocusChangingDelegate` （取决于您的 Unreal 版本，您可能只能获取委托变量本身）并监听焦点变化。

您可以通过委托输出的`FFocusEvent`过滤哪个用户更改了焦点，该事件具有更改焦点的用户索引（如果您需要知道特定用例的情况，它也有焦点更改的原因）。

*向右滚动找到头文件 -&gt;*


<table>
<tr>
<td style="text-align: center;">源文件(.cpp)</td>
<td style="text-align: center;">头文件(.h)</td>
</tr>
<comment></comment><td> ```c++ void AMyPlayerController::BeginPlay() { Super::BeginPlay();<p data-md-type="paragraph"> // 有效检查 slate 应用程序已初始化 if(FSlateApplication::IsInitialized()) { // 根据您的虚幻引擎版本， // 您可能只需要执行“FSlateApplication::Get().FocusChangingDelegate”，而不是使用函数来获取它</p>
<pre data-md-type="block_code" data-md-language=""><code>// Bind for when focus changes, if you're having issues with this then I recommend learning about Unreal's delegate framework
// or looking at the slate application's header and source file regarding the focus changing delegate
FSlateApplication::Get().OnFocusChanging().AddUObject(this, &amp;AMyPlayerController::FocusChanged);
</code></pre>
<p data-md-type="paragraph">}<br data-md-type="linebreak"> }</p>
<p data-md-type="paragraph"> void AMyPlayerController::FocusChanged(const FFocusEvent&amp; FocusEvent, const FWeakWidgetPath&amp; OldFocusedWidgetPath, const TSharedPtr<swidget data-md-type="raw_html"> &amp; OldFocusedWidget, const FWidgetPath&amp; NewFocusedWidgetPath, const TSharedPtr<swidget data-md-type="raw_html"> &amp; NewFocusedWidget) { // 检查此玩家控制器是否具有有效的本地玩家对象 if(!IsValid(GetLocalPlayer())) { // 如果我们没有有效的本地玩家对象，则不要继续，因为我们'尚未完成玩家控制器返回的设置； } // 检查改变焦点的玩家是否是我们自己 if(GetLocalPlayer()-&gt;GetControllerId() != FocusEvent.GetUser()) { // 如果 self 没有进行这次焦点改变，则不继续 return; } // 做东西 }</swidget></swidget></p>
<pre data-md-type="block_code" data-md-language=""> <code>&lt;/td&gt;
&lt;td&gt;
```c++
UCLASS()
class MYGAME_API AMyPlayerController : public APlayerController
{
GENERATED_BODY()
protected:

// AActor interface
virtual void BeginPlay() override;
// ~AActor interface

/**
* Occurs when a Slate User changes widget focus.
* @param FocusEvent The type of focus event that occured.
* @param OldFocusedWidgetPath The previously focused widget's path in the widget tree.
* @param OldFocusedWidget The previously focused slate widget.
* @param NewFocusedWidgetPath The widget path of the slate widget we're going to focus.
* @param NewFocusedWidget The slate widget we're going to focus.
*/
virtual void FocusChanged(const FFocusEvent&amp; FocusEvent,
const FWeakWidgetPath&amp; OldFocusedWidgetPath,
const TSharedPtr&lt;SWidget&gt;&amp; OldFocusedWidget,
const FWidgetPath&amp; NewFocusedWidgetPath,
const TSharedPtr&lt;SWidget&gt;&amp; NewFocusedWidget);
};
</code></pre>
</td>
</table>
<div data-md-type="block_html"></div>

<a name="navigation-genesis"></a>

### 11.2 导航起源

导航可以由 3 种类型引起，称为**导航起源**：

- **键盘**：导航事件是由键盘输入引起的。
- **控制器**：导航事件是由游戏手柄输入引起的。
- **用户**：导航事件是由游戏代码、widgets等引起的用户生成的事件。

**[<span>⬆</span>返回顶部](#table-of-contents)**
<a name="split-screen"></a>

## 12.0分屏

分屏功能的工作原理是，它有一个**游戏层管理器**来管理视口slate widget、哪个玩家正在使用该slate widget以及如何塑造该slate widget。游戏层管理器由游戏引擎在`GameEngine.cpp` ( `UGameEngine` ) 和`PlayLevel.cpp` ( `UEditorEngine` ) 中初始化时使用硬编码类进行实例化，因此要构建您自己的，需要修改`UGameEngine::CreateGameViewportWidget`中引擎的源代码&amp; `UEditorEngine::GeneratePIEViewportWindow` （史诗般的，如果你看到这个，请将其更改为可以在项目设置中指定的可配置类，你已经为游戏视口客户端设置了它，所以它已经完成了一半！）。

> 根据您的用例，一种可能的途径是调用`UGameViewportClient::SetGameLayerManager` ，但请注意这可能会产生下游影响，并且可能需要来自不可重写的引擎代码的一些解决方法。

当在屏幕上显示小部件时，您有两个选择：

- **添加到视口**：将其添加到整个游戏视口，用该小部件覆盖两个玩家视口；对于暂停菜单或任何需要完全占据屏幕的东西很有用，并且应该影响所有其他本地玩家。
- **添加到玩家屏幕**：将其添加到特定玩家的视口，并且不会用该小部件覆盖其他玩家的视口；对于 HUD 或特定于该本地玩家而非所有其他玩家的任何内容很有用。

<a name="game-layer-manager"></a>

### 12.1 游戏层管理器

游戏层管理器是一个界面，其中包含用于获取视口几何形状、让本地玩家使用此widget、添加widget层（不推荐，除非您知道自己在做什么）的准系统信息，其中包含所有slate widget已添加到该特定视口，并用于保存实际的游戏视口slate widget。游戏层管理器位于`SGameLayerManager.h/cpp`中，您可以找到接口 ( `IGameLayerManager` ) 和一个复合小部件，即`SGameLayerManager` ，它处理显示游戏视口小部件（也可用作界面的基本示例实现），使用从游戏视口客户端检索的简单矩形布局。 `SGameLayerManager`还将更改路由到 DPI 比例，并根据 DPI 比例值（包括其所有小部件层）缩放视口。

<a name="viewport-layout"></a>

### 12.2 视口布局

默认情况下，布局设计是一个矩形形状，可通过其 X/Y 尺寸及其在屏幕上的 X/Y 位置（中心比例为 0-1）进行自定义。这是在默认`UGameViewportClient`的构造函数中设置的。如果您想创建自己的自定义视口形状；您必须创建自己的游戏层管理器类才能计算出自定义形状并将其应用到视口小部件。

![游戏视口层框架](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/game_layer_general.png?raw=true)
*这是游戏层管理器如何工作的总体概述，以及 Slate 应用程序如何与这些系统通信的一些可视化解释。*

<a name="local-players"></a>

### 12.3 本地玩家

单台机器上的每个本地玩家都有一个可以存在于关卡之间的`ULocalPlayer`对象。这是玩家作为`UObject`类型的字面表示，每个本地玩家对于了解该玩家正在使用哪个游戏手柄、该玩家正在使用哪个视口客户端、获取其 Slate 用户、获取在线子系统信息等很有用。

对于应用程序的不同元素，使用不同的 ID 来跟踪本地玩家，它们如下所列：

- 平台ID：玩家在本机上的ID，因此索引0为初始玩家。
- 控制器 ID：虽然这里说的是控制器，但实际上是游戏手柄 ID。
- 唯一网络 ID：该玩家在在线多人游戏关卡中的唯一网络 ID。

<a name="gamepad-id"></a>

#### 12.3.1 手柄ID（控制器ID）

> 游戏手柄 ID 实际上是控制器 ID，但为了避免混淆，从现在开始我们将使用游戏手柄 ID。此外，游戏手柄 ID 功能均未向蓝图公开，因此您需要自己执行此操作，但它们都是`ULocalPlayer`中可公开访问的值和函数。

本地玩家可以通过调用`ULocalPlayer::GetControllerId()`知道他们正在使用哪个游戏手柄 ID。要将其更改为另一个游戏手柄，您可以调用`ULocalPlayer::SetControllerId()` ，如果另一个玩家正在使用该 ID，那么它将强制交换该玩家的游戏手柄 ID，并且也会更新平台用户 ID。要获取游戏手柄 ID 更改时的委托，您可以使用`ULocalPlayer::OnControllerIdChanged()` 。

> 为什么是控制器 ID 而不是游戏手柄 ID？老实说，我不知道，我确实希望 Epic 修复这个命名约定，因为这可能会与谈论玩家控制器而不是游戏手柄控制器混淆（老实说，只说游戏手柄就可以 100% 消除混淆）。代码注释也没有清楚地解释这一点，因为它们将其称为物理 ControllerID，这又可能与玩家控制器混淆。

**[<span>⬆</span>返回顶部](#table-of-contents)**
<a name="important-file-names"></a>

## 13.0 重要文件名

以下是虚幻引擎中重要/有用的 C++ 文件的列表（排名不分先后），建议您查看，有些文件甚至可能不会在本纲要中讨论。

您不需要立即理解它，但这有助于了解有用的文件以供参考。

对象名称 | 标头 | 来源 | 笔记
:-: | :-: | :-: | :--
`FEngineLoop`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/Launch/FEngineLoop/) | LaunchEngineLoop.h | LaunchEngineLoop.cpp | 这是整个虚幻引擎应用的核心，建议只看不碰。
[多种的] | SharedPointer.h | SharedPointerInternals.h | 这是 Epic 的共享（智能）指针框架所在的位置，还建议查看`SharedPointerTesting.ini` ，例如使用共享指针的示例。
`FSlateApplication`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/Slate/Framework/Application/FSlateApplication/) | SlateApplication.h | SlateApplication.cpp |
`FHittestGrid`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/SlateCore/Input/FHittestGrid/) | HittestGrid.h | HittestGrid.cpp |
`SWidget`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/SlateCore/Widgets/SWidget/) | SWidget.h | SWidget.cpp |
`UWidget`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/UMG/Components/UWidget/) | Widget.h | Widget.cpp |
[多种的] | DeclarativeSyntaxSupport.h | [None] | 保存 Slate 使用的所有声明性语法宏，例如 SNew、SLATE_ARGUMENT 等。需要研究的极其重要的文件。
`SUserWidget`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/SlateCore/Widgets/SUserWidget/) | SUserWidget.h | SUserWidget.cpp |
`UUserWidget`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/UMG/Blueprint/UUserWidget/) | UserWidget.h | UserWidget.cpp |
`FNavigationConfig`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/Slate/Framework/Application/FNavigationConfig/) | NavigationConfig.h | NavigationConfig.cpp | 这是大多数导航相关类型所在的位置，包括结构体和枚举。
[多种的] | SlateEnums.h | [None] | 这是大多数与 Slate 相关的枚举类型所在的位置。
`TAttribute`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/Core/Misc/TAttribute/) | Attribute.h | [None] |
`TSlateAttribute`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/SlateCore/Widgets/TSlateAttribute_FText_EInvalidat-/) | SWidget.h 和 SlateAttribute.h |  | 虽然这是在`SWidget.h`中声明的，但实际上是在`SlateAttribute.h`中解释的
[多种的] | InputCoreTypes.h | InputCoreTypes.cpp | 这是大多数输入相关类型所在的位置，包括结构和枚举。
`FInputEvent`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/SlateCore/Input/FInputEvent/) | Events.h | Events.cpp | 这是`FInputEvent`类型的层次结构所在的位置。
`SGameLayerManager`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/Engine/Slate/SGameLayerManager/) | SGameLayerManager.h | SGameLayerManager.cpp |
`UGameViewportClient`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/Engine/Engine/UGameViewportClient/) | GameViewportClient.h | GameViewportClient.cpp |
[多种的] | UnrealClient.h | UnrealClient.cpp | 这个很有趣，因为它同时保存用于视口渲染的管理器对象及其一些功能。<br>这也是处理屏幕截图的地方（包括带/不带 UI 的屏幕截图）。
`FSceneViewport`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/Engine/Slate/FSceneViewport/) | SceneViewport.h | SceneViewport.cpp | 这是我们视口的slate widget的本质所在。
`IInputProcessor`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/Slate/Framework/Application/IInputProcessor/) | IInputProcessor.h | [None] | 这是输入预处理器的基本接口类，如果您要创建一个，则将从该类继承。有关如何设置的示例，请查找 FAnalogCursor。
`FAnalogCursor`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/Slate/Framework/Application/FAnalogCursor/) | AnalogCursor.h | AnalogCursor.cpp |
`FSlateUser`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/Slate/Framework/Application/FSlateUser/) | SlateUser.h | SlateUser.cpp |
`ULocalPlayer`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/Engine/Engine/ULocalPlayer/) | LocalPlayer.h | LocalPlayer.cpp | 它还包含`FLocalPlayerContext` ，它对于传递到 UI 和其他对象以获得本地玩家、其玩家控制器、游戏实例、世界等的上下文非常有用。
`UWidgetLayoutLibrary`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/UMG/Blueprint/UWidgetLayoutLibrary/) | WidgetLayoutLibrary.h | WidgetLayoutLibrary.cpp |
`USlateBlueprintLibrary`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/UMG/Blueprint/USlateBlueprintLibrary/) | SlateBlueprintLibrary.h | SlateBlueprintLibrary.cpp | 请注意，脚本名称也写为“SlateLibrary”，以防您找不到它。
`UWidgetBlueprintLibrary`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/UMG/Blueprint/UWidgetBlueprintLibrary/) | WidgetBlueprintLibrary.h | WidgetBlueprintLibrary.cpp | 请注意，脚本名称也写为“WidgetLibrary”，以防您找不到它。
`UUserinterfaceSettings`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/Engine/Engine/UUserInterfaceSettings/) | UserinterfaceSettings.h | UserinterfaceSettings.cpp | 这也是声明渲染焦点规则和您在这些设置中找到的一些其他数据类型的地方。

**[<span>⬆</span>返回顶部](#table-of-contents)**
