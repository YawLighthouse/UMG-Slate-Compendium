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

## Repository Page Links

> - [常见问题页面](FAQ.md)
> - [外部链接页面](EXTERNAL_LINKS.md)

<a name="table-of-contents"></a>

## 目录

> 1.0 [Introduction](#introduction)
>  2.0 [Performance &amp; Design Considerations](#performance--design-considerations)
>        2.1 [CPU Considerations](#cpu-considerations)
>              2.1.1 [Performance regarding Animations](#perf-animations)
>              2.1.2 [Performance regarding Widget Components](#perf-widget-components)
>        2.2 [GPU Considerations](#gpu-considerations)
>  3.0 [Slate &amp; UMG in Unreal](#slate--umg-in-unreal)
>  4.0 [Slate](#slate)
>        4.1 [Slate Widget Casting &amp; Storing](#slate-widget-casting--storing)
>        4.2 [Slate Units &amp; DPI Scaling](#slate-units--dpi-scaling)
>        4.3 [Slate Users](#slate-users)
>        4.4 [Widget Layout](#widget-layout)
>        4.5 [On Paint](#on-paint)
>        4.6 [Widget Ticking](#widget-ticking)
>        4.7 [Widget Hierarchy](#widget-hierarchy)
>        4.8 [Invalidation](#invalidation)
>        4.9 [Slate Attributes and Events](#slate-attributes-and-events)
>        4.10 [Slate Widget Examples(Slate Test Suite/Starship)](#slate-widget-examples)
>        4.11 [Helpful Console Commands for Slate](#helpful-console-commands-for-slate)
>  5.0 [UMG (Unreal Motion Graphics)](#umg)
>        5.1 [User Widget Hierarchy](#user-widget-hierarchy)
>        5.2 [User Widget Animation](#user-widget-animation)
>        5.3 [User Widget Events](#user-widget-events)
>  6.0 [Common Widgets](#common-widgets)
>  7.0 [Common Widget Functionality](#common-widget-functionality)
>  8.0 [UMG in relation to Levels/Worlds](#umg-in-relation-to-levels-worlds)
>        8.1 [HUD Actors](#hud-actors)
>              8.1.1 [HUD Drawing](#hud-drawing)
>              8.1.2 [HUD HitBoxes](#hud-hitboxes)
>        8.2 [Widget Components](#widget-components)
>              8.2.1 [Widget Interaction Components](#widget-interaction-components)
>              8.2.2 [How Widget Components are Rendered](#widget-components-rendering)
>  9.0 [Development &amp; Debug Tools for UMG/Slate](#dev-debug-tools)
>        9.1 [Debug Console Commands](#debug-console-commands)
>        9.2 [Widget Reflector](#widget-reflector)
>        9.3 [Slate Console Debugger](#slate-console-debugger)
>  10.0 [Input Framework of Unreal Engine(relating to UMG/Slate)](#input-framework-of-unreal-engine)
>        10.1 [Input Flow of Unreal Engine](#input-flow-of-unreal-engine)
>        10.2 [Input Components](#input-components)
>        10.3 [Input Event Types](#input-event-types)
>        10.4 [Input Modes](#input-modes)
>  11.0 [Unreal's Focusing System](#unreals-focusing-system)
>        11.1 [Navigation Grid Explanation](#navigation-grid-explanation)
>        11.2 [Navigation Genesis](#navigation-genesis)
>  12.0 [Split Screen](#split-screen)
>        12.1 [Game Layer Manager](#game-layer-manager)
>        12.2 [Viewport Layout](#viewport-layout)
>        12.3 [Local Players](#local-players)
>              12.3.1 [Gamepad ID(Controller ID)](#gamepad-id)
>  13.0 [Important File Names](#important-file-names)

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
- **Slate Framework**
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

UI Complexity is usually in relation to the number of active and visible widgets (so offscreen widgets don’t tick and should not run functionality if not on screen). A common technical design with UI is to be reactive to gameplay code(it is also suggested if you're game isn't UI dependent) but not to drive the gameplay code so it is performant and reusable, normally its fine for the UI to be the entry point for executing the gameplay code but then it should listen to how the gameplay code responded and to react to it.

<a name="cpu-considerations"></a>

### 2.1 CPU注意事项

Slate/UMG 使用[失效](#invalidation)和缓存的概念，而不是不断轮询数据。原因是有时轮询要么性能不佳，要么功能不正确（例如，使用多线程代码时）。通常，Unreal 中的高性能 UI 旨在**基于事件**。 UMG 具有**属性绑定**，但不应使用它们，因为它们将在每一帧进行更新，这比使用基于事件的框架对 CPU 的负担要大得多。

**CPU技术设计场景**
:-:
在 RTS 中，你需要有一个 UI 标记来查看在世界上放置一队士兵的位置，<br>玩家在世界中选择一个有效的位置来放置 UI 标记，然后部队将移动到该位置。

**好例子** | **坏榜样**
:-: | :-:
A player manager object creates the UI marker widget and manages where to move the troops.<br>The world position that is chosen by the player is fed into the UI marker’s widget(from the player manager object) and the widget itself handles positioning itself in screen space. | 玩家选择一个世界位置并将其直接提供给 UI 标记，然后 UI 标记直接告诉部队移动到哪里并处理他们的移动。

<a name="perf-animations"></a>

#### 2.1.1 动画性能

Invalidating the desired size of multiple widgets by animating a large amount of widgets can have downstream effects of spending a lot of CPU time re-calculating the [Widget Layout](#widget-layout).

- In previous versions of Unreal Engine when playing an animation with a UMG widget they would be separate from Sequencer(after Sequencers release it was quickly updated to use Sequencer as the underlying animation system).
- In previous versions of Unreal Engine post Sequencer update, the UMG widget would be set to volatile when animating.
- In current versions of Unreal Engine UMG widgets do not switch to volatile when animating.

为什么我应该使用易失性？

> Honestly its not the answer everytime, what happens when you animate a UMG widget or change its visual state it will invalidate the widget for this frame and re-cache its new state until it is invalided again later on. Setting it to volatile will not invalidate or cache the widget, it goes through a separate code path for volatile widgets which is a case-by-case basis of improving performance. Some avenues to use volatile for is by turning it on for a period of time and then turning it off when you no longer need it to be volatile.

- 关于使用 volatile 的一些提醒：
    - It does not cache the widget state, it is polled every frame it is on screen.
    - It tends to affect the widget hierarchy so be aware of downstream effects to parent widgets AND child widgets as well.

<a name="perf-widget-components"></a>

#### 2.1.2 Widget 组件的性能

Widget Components are great for prototyping and for in world VR/AR UI. They are not great when trying to have the UI be within the world while still respecting screen size, logic controlling layout, etc.
 Typically its good to use widget component if you have available texture budget because each widget component is really a static mesh plane with a render target texture applied to its material slot zero.
 Each widget component will create the UMG widget object and then render it out to a texture to show it in the world.

It is recommended to not use Widget Components heavily(excluding VR/AR) due to texture memory use. An alternative(which is more complicated) is to create a custom Slate Widget to handle rendering multiple "Widgets" at once you can do this using `SMeshWidget` which is 1 draw call per widget but it requires competent technical art skills to be able to draw multiple widget elements as 1 texture.

What is a `SMeshWidget`?
 It is a widget that has rendering capability of 1 draw call, it was used in Paragon to draw each status bar and icon on screen. It is extremely powerful but also requires a good understanding of how to draw these elements in code which is why you don't see it oftenly used, this is intended to not be exclusive to just textures or particles but can be used with 3D models/meshes or anything you want to render.

Nick Darnell 实际上整理了一个在 UE4 中使用`SMeshWidget`绘制粒子的示例：

- [论坛链接](https://forums.unrealengine.com/t/smeshwidget-hardware-instanced-slate-meshes-thread/58020/5)
- [Dan Treble was kind enough to turn the example project into a Github Repository](https://github.com/dantreble/MeshWidgetExample)

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

Pros of Slate:

- **完全使用 C++ 可以提​​高性能**
- **可用于通用应用程序**（例如 Epic Game 的启动器和虚幻编辑器本身完全是用 Slate 构建的）
- **用于扩展编辑器**（这是因为虚幻编辑器首先是从 Slate 构建的）
- **不与虚幻的垃圾收集系统一起使用**

Cons of Slate:

- **完全使用 C++** （没有视觉设计器，这才是 UMG Designer 的真正用途）
- **Not able to have sequencer animations** (everything is hard coded so longer iteration times)
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
- **Able to extend the editor** (Unreal Engine 4.23+ using Editor Utility Widgets)

UMG的缺点：

- **仅限于虚幻引擎**（无法在常规操作系统应用程序的引擎之外使用）
- **易用性还会导致开发人员承担技术债务并导致 UI 变得混乱**

![UMG 设计器示例](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/umg_designer.png?raw=true)

**[<span>⬆</span>返回顶部](#table-of-contents)**
<a name="slate"></a>

## 4.0 Slate

Slate 框架有两个核心部分：

- **Slate Renderer**
    - Slate Widgets 使用自己的渲染器（称为 Slate Renderer）显示，它处理游戏视口前面的 UI 元素渲染，并与 Unreal 的世界渲染管道结合来显示 UI 元素。
- **Slate Application**
    - The Slate Application is an object that manages all the CPU related tasks of Slate Widgets such as their viewport positions, user focus navigation, widget hierarchy placement and tracking, receiving input and routing that input to either Slate Widgets or to the rest of the game engine (yes this is where the raw inputs are received before UI element’s get them) and registering (creating)/tracking the Slate Users.

<a name="slate-widget-casting--storing"></a>

### 4.1 Slate Widget 投射和存储

由于**Slate Widgets****<u>不是</u>**从`UObject`派生的，这意味着您无法使用 Unreal 的对象转换系统来获取特定的 Slate 类型，也无法利用`UObject`的垃圾回收。相反，Slate 使用 Unreal 的共享指针（也称为智能指针）框架来管理 Widget 的内存。

共有三种共享指针模板类型：

- `TSharedRef` ：不可空、引用计数、非侵入式权威智能指针。
- `TSharedPtr` ：引用计数、非侵入式权威智能指针。
- `TWeakPtr` ：引用计数、非侵入式弱指针引用。

> 一个重要的注意事项是，当共享引用的所有引用计数都消失时；该对象将被删除，并且一个限制是它只会调用默认析构函数，因此带有参数的自定义析构函数不可用。

Using shared pointers allows you to assume ownership of these slate widgets without having to call delete on them.
 *I recommend taking a look at [Epic's Documentation on Smart Pointers](https://docs.unrealengine.com/5.0/en-US/smart-pointers-in-unreal-engine/)*

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

- **Slate Units**: Unreal’s way of making a UI that is independent of pixel density so your application can support multiple platforms easily. This allows for it to be more precise and independent of the pixel density of the user’s monitor. A single slate unit can vary in physical size but by default it is conveniently set to 1 pixel for each slate unit. To set the default value; it is recommended to adjust the base DPI Scaling instead.
- **DPI Scaling**: How Unreal modifies the slate units conversion at runtime by scaling each slate unit by this value, so for example if you’re slate units are set to 1 unit = 1 pixel, and the dpi scaling is set to a value of 2.5 then each slate unit would be 1 unit = 2.5 pixels. You can change the DPI scaling depending on the resolution via a curve table in the project settings under the “Engine-User Interface” category.

Epic 自己承认它并不完美，但它适用于大多数用例。
 [Epic 的 AnswerHub 解释 Slate 单位](https://forums.unrealengine.com/t/what-are-slate-units/310703)

<a name="slate-users"></a>

### 4.3 Slate Users

**Slate Users** are classes that represent that a local individual input-providing user (for example; in split screen co-op with 3 players then there are 3 Slate Users, but in an online game with 32 players and no split screen then the only local player is the only Slate User on this device). Each **Platform’s SDK** will tell the **Slate Application** to register (create) a new Slate User when a new connection is added (for example when a new controller is plugged in). When a new connection is added, a new Slate User is created but not when a connection is removed to account for a controller disconnecting accidentally (to keep the settings for that controller just in case they reconnect). When a connection is removed, that Slate User is just not updated. The Slate User instance keeps track of the widget that the user is currently focusing on, and controls the cursor/has pointer information to account for gestures (this is only for the first Slate User because you can’t plug in multiple mouses and if you are… why?).

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

### 4.4 Widget Layout

Slate Widgets 布局的计算方式分两遍完成（按执行顺序列出）：

1. **Cache Desired Size**: Calculate how much space each widget wants to occupy, this occurs through a *Bottom-Up* approach where it guarantees when this pass happens for a widget, its children have already computed and cached their desired size.

![Cache Desired Size Example](images/cache_desired_size.png)
 *Example of Desired Size for a Horizontal Box with Textblock and Image widget's*

For the desired size example we have a horizontal box holding a text block and an image widget. In this case we compute the desired size for the text block (which is measured by the string that is displaying) and the image widget (which is measured based on the image data that is shown). Then the horizontal box is computed by combining the text block and image widget’s(we are assuming that the text block is 14 slate units and the image widget is 8 slate units) desired sizes, so for this example 14 slate units + 8 slate units = 22 Slate Units.

1. **Arrange Children**: This occurs in a *Top-Down* approach where the widget is asked to arrange its children based on their desired sizes and the desired size of this widget (which all occurred in the first pass).

![Arrange Children](images/arrange_children.png)
 *Example of Arranged Children using Allotted Size for a Horizontal Box with Textblock and Image widget’s*

For the arranged children example the horizontal box was allotted 25 slate units by its parent widget (not shown to keep things simple). The first horizontal box slot indicates that it wants the desired size of the child which is 14 slate units from the text block, while the second slot wants to fill the available width which is 11 slate units remaining for the image widget.

<a name="on-paint"></a>

### 4.5 On Paint

**Drawing Slate** is the process where Slate will iterate over all visible widgets and create a list of **Draw Elements** to send to the rendering system, this list is created every frame.

这发生在 On Paint 函数中，它将执行两件事：

- 根据所有孩子的**几何形状**（所需尺寸）排列他们。
- Paint the actual visuals related to this widget.

**[<span>⬆</span>返回顶部](#table-of-contents)**
<a name="widget-ticking"></a>

### 4.6 Widget Ticking

Slate Widget's(which also means UMG widget's) themselves do not tick, they do not have tick components and do not have tick groups. The order that Slate Widget's tick occurs is during the Paint pass with these calls originating from the Slate Application, so that a widget's tick function will only be called when it is on screen and being rendered:

1. `FSlateApplication::Tick`
2. `FSlateApplication::TickAndDrawWidgets` ...
3. `SWidget::Paint`
4. `SWidget::OnPaint` ...
5. `SObjectWidget::Tick`
6. `UUserWidget::NativeTick`(this is when Blueprint will receive tick too)

<a name="widget-hierarchy"></a>

### 4.7 Widget Hierarchy

The concept of the **Widget Hierarchy** is done using child slots, which are optional objects that can be tied to a Slate Widget (because some Widgets are not designed to have children such as the Image widget(**Leaf Widget**)) but requires the slot to be custom build for tracking each widgets child such as how the Button widget(**Compound Widget**) only accepts 1 child widget meanwhile an Overlay widget can have multiple child widgets.

Widgets usually come in 3 main types:

- **Leaf Widgets**: Widgets with no child slots.
     ![Leaf Widgets Example](images/leaf_widgets.png)
- **Panels Widgets**: with a dynamic number of child slots.
     ![Panel Widgets Example](images/panel_widgets.png)
- **Compound Widgets**: Widgets with a fixed number of explicitly named child slots.
     ![Compound Widgets Example](images/compound_widgets.png)

任何 Slate Widget（也称为 SWidget，其中每个 Slate Widget 在 C++ 中都以大写“S”为前缀）的关键元素是函数和值的组合：

- **计算所需尺寸**（函数）：负责计算所需尺寸，作为布局的第一遍。
    - **Slate 矩形**（值）：原点位于左上角的矩形，由左上角和右下角定义。原点位于左上角，Y 轴向下，X 轴向右。这用于计算所需的大小和边界。
- **排列子组件**（函数）：负责排列子组件作为布局的第二遍。
- **On Paint** (Function): Responsible for the actual rendered appearance of the widget.
- **事件处理程序**（委托值和/或函数）：这些是基于事件的钩子，用于 UI 元素在运行时更改，通常以“OnSomething”的形式。

<a name="invalidation"></a>

### 4.8 无效

[Epic 关于 Slate 和 UMG 失效的官方文档](https://docs.unrealengine.com/latest/INT/invalidation-in-slate-and-umg-for-unreal-engine/)

To avoid having to calculate the desired size of the widget every frame (which can tax the CPU if it’s happening to a lot of Widgets at once), Widgets have the concept of caching their desired size, but at runtime if the size of the widget changes (either through an animation or through game code) then it will **Invalidate** the widget to tell Slate that this widget needs to recalculate its **Desired Size** and then **Rearrange the Layout** that widget is in (or has). This is an optimization to avoid polling for each widget's desired size every frame when it doesn’t need to recalculate it.

There are different types of invalidation reasons that you can specify when invalidating a widget located in `InvalidateWidgetReason.h`:

- Layout: Use Layout invalidation if your widget needs to change desired size. This is an expensive invalidation so do not use if all you need to do is redraw a widget.
- Paint: Use when the painting of widget has been altered, but nothing affecting sizing.
- Volatility: Use if just the volatility of the widget has been adjusted.
- ChildOrder：添加或删除子项（这意味着预传递和布局）。
- RenderTransform: A Widgets render transform changed.
- 可见性：更改可见性（这意味着布局）。
- AttributeRegistration：属性已绑定或未绑定（由 SlateAttributeMetaData 使用）。
- Prepass: Re-cache desired size of all of this widget's children recursively (this implies layout).
- PaintAndVolatility: Use Paint invalidation if you're changing a normal property involving painting or sizing. Additionally if the property that was changed affects Volatility in anyway, it's important that you invalidate volatility so that it can be recalculated and cached.
- LayoutAndVolatility: Use Layout invalidation if you're changing a normal property involving painting or sizing. Additionally if the property that was changed affects Volatility in anyway, it's important that you invalidate volatility so that it can be recalculated and cached.

<a name="slate-attributes-events"></a>

### 4.9 Slate Attributes and Events

Slate(and through Slate; UMG) support the usage of Attribute's for binding properties/functions/lambdas to a widget's property(s). Widget Attributes are only updated if the widget is visible/not collapsed, so setting its visibility to hidden will cause it not to update.

Attributes are particularly useful with widget styling where you can specify the different visual styles of widgets across the project as general theme. Which improves productivity across the project for both engineering and art teams.

- `TAttribute` ：虚幻引擎的基本属性类型，不用于 SWidget 中的成员属性。
    - 与失效不兼容，因为数据更改时不会广播。
    - 不适合缓存（这意味着 CPU 可能会变慢）。
    - 如果您使用`TAttribute`更改 SWidget 的状态，则需要覆盖`ComputeVolatility` （ `TSlateAttribute`和`TSlateManagedAttribute`不需要）。
- `TSlateAttribute` ：应与 SWidget 成员属性一起使用，允许属性与 Slate 的失效系统一起使用，并且对于 Slate 特定代码具有更高的性能，同时保持属性在引擎中的安全。
    - 不继承自`TAttribute` ，它继承自`FSlateAttributeBase` / `TSlateMemberAttribute` 。
    - 不可复制，但如果您需要它可移动，则建议使用`TSlateManagedAttribute` 。
    - 在 PrePass 更新阶段每帧更新一次，因此性能更加友好。
    - 成员属性按照 SWidget 定义中定义变量的顺序更新（默认情况下）。
    - Allows for the invalidation reason to be a predicate and/or can be overriden per SWidget but use this with precaution since it can break invalidation for parent widget's.
- `TSlateManagedAttribute` ：应与数组或其他移动数据结构内部的 SWidget 成员属性一起使用。
    - 不继承自`TAttribute` ，它继承自`FSlateAttributeBase` / `TSlateMemberAttribute` 。
    - 只能移动而不能复制，因此它们会消耗更多内存。

> - `TAttributes`具有较高的内存开销并且不适合缓存。所以请根据您的判断使用。
> - 所有 slate 属性都保存在`SlateAttributeMetaData`中，以便在每个 SWidget 中轻松访问。

在 SWidget 内声明事件和属性宏时，您需要将它们放在其他两个宏之间：

- `SLATE_BEGIN_ARGS` OR `SLATE_USER_ARGS`: The difference is that `SLATE_USER_ARGS` requires the user to have all widget implementation within the source file, so the header can only hold declaration information where all handlers are essentially truly private and can be inlined(so less boilerplate code).
- `SLATE_END_ARGS`

Using these macros allows the widget authors to add support for widget construction via `SNew` and `SAssignNew`.

Slate 中的属性还具有在声明它们时必须使用的特定宏，并且如果您想在运行时创建 Slate Widget 时公开这些属性，则必须使用这些宏。

- `SLATE_ATTRIBUTE` ：
    - 允许属性与值或函数一起使用。
    - 将属性类型作为第一个参数，将属性名称作为第二个参数（建议匹配成员属性以避免混淆）。
- `SLATE_ARGUMENT` ：
    - 允许属性仅与值一起使用。
    - 将属性类型作为第一个参数，将属性名称作为第二个参数（建议匹配成员属性以避免混淆）。
- `SLATE_ARGUMENT_DEFAULT` ：与`SLATE_ARGUMENT`相同，但也支持默认值，语法： `SLATE_ARGUMENT_DEFAULT(float, WheelScrollMultiplier) = 1.0f;`
- `SLATE_STYLE_ARGUMENT`: Same as `SLATE_ARGUMENT` but they can only be used with types that inherit from `FSlateWidgetStyle` for a widget's visual styling purposes.

Here is an example where we're making a custom button widget using these macros.

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

Slate Events are basically delegates in SWidgets for binding on widget creation, you would declare your delegate in C++ with this macro in the Arguments range macro declaration.

- `SLATE_EVENT`: Adds event handler support for this widget with a specific member variable, this exposes delegates for binding on creation. Expects that the widget has a delegate of the `EventDelegateType` that is named the same as the event's name that was inputted in.

Here is an example widget that is using the event macro for when it is hovered.

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

### 4.10 Slate Widget Examples(Slate Test Suite/Starship Suite)

**Slate Widget Examples** （如果使用 UE4，也称为**Slate Test Suite；**如果使用 UE5，则称为**Starship Suite** ）是 Slate 构建示例的集合，例如单选按钮、响应式网格、色轮等。

您可以通过以下方式访问虚幻编辑器中的测试套件：

1. 根据您是否使用 UE4/UE5，这会有所不同
    - UE4： `Window>Developer Tools>Debug Tools`
    - UE5: `Tools/Debug/Debug Tools`
         ![Slate Widget Examples Step 1](images/slate_widget_examples_step1.png)
2. Select `Test Suite`
     ![Slate Widget Examples Step 2](images/slate_widget_examples_step2.png)

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

- **UVisual**: The base class for elements in UMG Slots and Widgets.
    - **UWidget**: The base class for all Widgets, they create Slate Widgets and handle routing functionality from Blueprint/UObject based classes to Slate. These are widgets like TextBlock, ScrollBox, Button, etc.
    - **UUserWidget** ：用于设计 UI、动画 UI 以及将该功能连接到游戏代码的基类。

User Widgets are built out of Widgets except how a User Widget does not require a root widget, basically like how Actors are built out of multiple Actor Components and require a root Actor Component(known as Root Component).

User Widgets cannot inherit their **Widget Hierarchy** like how Actors do with Components but they can inherit class functionality (so making a User Widget abstract will allow for other classes to inherit from it or creating the class in C++ will allow for inheritance).

<a name="user-widget-hierarchy"></a>

### 5.1 用户控件层次结构

Each **User Widget** is the root `UWidget` by design, so a User Widget can have no widgets inside it and is by default a **Compound Widget** that can only have 1 child, but that child can have other children under it and causes the cascading effect of child widgets within each User Widget's **Tree Hierarchy**.

设计器/层次结构编辑器视图 | 运行时结果
:-: | :-:
![User Widget Hierarchy Example](images/user_widget_hierarchy.png)<br>*`Health_Bar` and `Health_Text` are **bold** because they have their `Is Variable` flag enabled*<br>![Is Variable Flag](images/is_variable.png) | ![User Widget Hierarchy Example Result](images/user_widget_hierarchy_result.png)

![User Widget Hierarchy Example Diagram](images/umg_hierarchy_diagram.png) *Example of that hierarchy as a diagram*

<a name="user-widget-animation"></a>

### 5.2 User Widget Animation

Every User Widget is able to create custom animations using the widgets within that User Widget via **Sequencer**. You can create these animations inside the Widget Designer and you’re able to modify things relating to that widget specifically such as render transform, widget visibility, etc. You can also modify properties of widgets such as material parameters, runtime values within the widget, etc.

![用户小部件动画设计器示例](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/umg_animation_designer.gif?raw=true)
 *UMG中的动画设计器示例*

If a User Widgets **Tick Frequency** is set to **Never** instead of **Auto** in the class defaults then it will never run animation’s because for an animation to play at all, the user widget needs to be able to tick that animation and if the user widgets ability to tick is turned off(by setting its tick frequency to never) then it will not tick the animation object.

<a name="user-widget-events"></a>

### 5.3 User Widget Events

Every user widget has built in events that you can implement and add your own functionality from;

- **Pre Construct**: This occurs both in editor in the designer and before the widget is actually created, similar to the Construction Script found on Actors.

```c++
virtual void UUserWidget::NativePreConstruct()
{
    // Call BP version
    PreConstruct(IsDesignTime());
}
```

![预构建](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_events/preconstruct.png?raw=true)

- **On Initialized**: This occurs only once at runtime when the non template instance is created(when you spawn a user widget).

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

- **Construct**: This can occur multiple times on a single user widget because it is based on when it is Constructed to the screen (Add to Viewport or Add to Player Screen). So if you're removing the widget from parent and re-adding it later then it's recommended not to put first time initialization code in this but instead put that in “On Initialized”.

```c++
virtual void UUserWidget::NativeConstruct()
{
    // Call BP version
    Construct();
    UpdateCanTick();
}
```

![构造](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_events/construct.png?raw=true)

- **Destruct**: Occurs when this widget is no longer on screen, can be called multiple times as its the inverse of Construct where it occurs when this widget is removed from parent.

```c++
virtual void UUserWidget::NativeDestruct()
{
    StopListeningForAllInputActions();
    // Call BP version
    Destruct();
}
```

![破坏](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_events/destruct.png?raw=true)

- **On Paint**: Occurs when this widget is painted every frame, different from Tick as it is meant to handle Paint Context information.

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

- **Tick**: This will occur every frame that this widget is on screen, it will not tick if the widget is not being shown(even if it still exists, the only thing that matters is that it is being shown then its ticking).

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

- **On Animation Started**: Occurs when an widget animation starts playing, it will give you that widget that started playing in case you need to check against it or use it later. (For Blueprint users; recent versions of Unreal require animation finished events to be in the event graph while prior ones allow for them to show up as overridable functions)

```c++
virtual void UUserWidget::OnAnimationStartedPlaying(UUMGSequencePlayer& Player)
{
    // Call BP version
    OnAnimationStarted(Player.GetAnimation());

    BroadcastAnimationStateChange(Player, EWidgetAnimationEvent::Started);
}
```

![动画开始时](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_events/animation_started.png?raw=true)

- **On Animation Finished**: Occurs when an widget animation finishes playing, it will give you that widget finished playing in case you need to check against it or use it later. (For Blueprint users; recent versions of Unreal require animation finished events to be in the event graph while prior ones allow for them to show up as overridable functions)

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

- **On Focus Received**: (If you're having trouble finding it in Blueprint, it has to return an Event Reply struct so you have to override it in the functions list and not in the event graph) Occurs when focus is given to this User Widget(only this widget). It requires that you return an Event Reply struct which you can choose to either return Handled or Unhandled.

```c++
virtual FReply UUserWidget::NativeOnFocusReceived( const FGeometry& InGeometry, const FFocusEvent& InFocusEvent )
{
    // Call BP version and return what BP returns
    return OnFocusReceived( InGeometry, InFocusEvent ).NativeReply;
}
```

![焦点收到](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_events/focus_received.png?raw=true)![两种处理方式均获得焦点](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_events/focus_received_both.png?raw=true)

- **On Added to Focus Path**: Occurs when this widget or a child widget within this User Widget is added to the focus path(or focused) and wasn’t previously part of it.

```c++
virtual void UUserWidget::NativeOnAddedToFocusPath(const FFocusEvent& InFocusEvent)
{
    // Call BP version
    OnAddedToFocusPath(InFocusEvent);
}
```

![添加到焦点路径](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_events/added_to_focus_path.png?raw=true)

- **On Focus Lost**: Occurs when this User Widget(only this widget) loses focus.

```c++
virtual void UUserWidget::NativeOnFocusLost( const FFocusEvent& InFocusEvent )
{
    // Call BP version
    OnFocusLost( InFocusEvent );
}
```

![关于失去焦点](https://github.com/YawLighthouse/UMG-Slate-Compendium/blob/main/images/user_widget_events/focus_lost.png?raw=true)

- **On Removed from Focus Path**: Similar to On Focus Lost except it can occur when a child widget within this User Widget or this widget itself is no longer part of the focus path.

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

## 6.0 Common Widgets

There is a large amount of commonly used widgets in Unreal Engine, at its basic core elements.
 Most/All of your UI that used UMG/Slate will probably be built from a combination of these widgets:

- **Text Block** *[Leaf Widget]*: Handles displaying static text that can be changed at runtime by setting it to another text value. TextBlock widgets allow for a custom font to be used(including its typeface if the font has any others), customizing its text size, letter spacing(also known as kerning), its outline settings(this will offset the rendered text), apply materials to the letters themselves, add a shadow offset(this will offset the rendered text), set its justification(how the text is aligned), etc.
     ![Text Block Widget](images/common_widgets/w_textblock.png)
- **Rich Text Block** *[Leaf Widget]*: Works similarly to TextBlock widgets but allows for the use of custom images, glyphs, multiple fonts, etc to be used within the same text value at once.
     ![Rich Text Block Widget](images/common_widgets/w_richtextblock.png)
- **Image** *[Leaf Widget]*: Handles displaying either a texture or a material that uses the UI domain to display it.
     ![Image Widget](images/common_widgets/w_image.png)
- **Border** *[Compound Widget]*: Can only have 1 child widget. Displays child widget in front of this widget, basically an Image widget that can have a child.
     ![Border Widget](images/common_widgets/w_border.png)
- **Button** *[Compound Widget]*: Can only have 1 child widget. Displays that child in front of this widget, can be interacted with and focused. Returns when it is clicked/pressed/released/hovered/unhovered. You can also set its click(mouse button) method, touch(touch screen’s) method, and press(keyboard &amp; gamepad) method.
     ![Button Widget](images/common_widgets/w_button.png)
- **Check Box** *[Leaf Widget]*: Handles displaying a specific image/material depending on what state the check box is in, it can be clicked(or set to a specific state) and meant to show if something is on/off.
     ![Checkbox Widget](images/common_widgets/w_checkbox.png)
- **Progress Bar** *[Leaf Widget]*: Handles displaying an image/material in front of this widget via a scaled 0-1 fill percentage value via its Bar Fill Type &amp; Bar Fill Style.
    - 填充类型：
        - **Left to Right**: Fills the progress bar from left to right.
               <img src="images/common_widgets/progress_bars/w_progressbar_left_right_mask.png" width="305" height="144">
               *Masked Version*
               <img src="images/common_widgets/progress_bars/w_progressbar_left_right_scale.png" width="318" height="146.5">
               *Scaled Version*
    - **Right to Left**: Fills the progress bar from right to left.
           <img src="images/common_widgets/progress_bars/w_progressbar_right_left_mask.png" width="299" height="144.5">
           *Masked Version*
           <img src="images/common_widgets/progress_bars/w_progressbar_right_left_scale.png" width="295.5" height="144.5">
           *Scaled Version*
    - **Fill from Center**: Linearly fills the progress bar on both X and Y from the center towards the edges.
           <img src="images/common_widgets/progress_bars/w_progressbar_center_mask.png" width="297.5" height="136.5">
           *Masked Version*
           <img src="images/common_widgets/progress_bars/w_progressbar_center_scale.png" width="290" height="140">
           *Scaled Version*
    - **Fill from Center Horizontal**: Linearly fills the progress bar on the X axis from the center towards the edges.
           <img src="images/common_widgets/progress_bars/w_progressbar_center_horizontal_mask.png" width="283.5" height="134.5">
           *Masked Version*
           <img src="images/common_widgets/progress_bars/w_progressbar_center_horizontal_scale.png" width="298" height="144">
           *Scaled Version*
    - **Fill from Center Vertical**: Linearly fills the progress bar on the Y axis from the center towards the edges.
           <img src="images/common_widgets/progress_bars/w_progressbar_center_vertical_mask.png" width="298" height="147.5">
           *Masked Version*
           <img src="images/common_widgets/progress_bars/w_progressbar_center_vertical_scale.png" width="290" height="133">
           *Scaled Version*
    - **Top to Bottom**: Fills the progress bar from top to bottom.
           <img src="images/common_widgets/progress_bars/w_progressbar_top_bottom_mask.png" width="300" height="138.5">
           *Masked Version*
           <img src="images/common_widgets/progress_bars/w_progressbar_top_bottom_scale.png" width="303.5" height="140.5">
           *Scaled Version*
    - **Bottom to Top**: Fills the progress bar from bottom to top.
           <img src="images/common_widgets/progress_bars/w_progressbar_bottom_top_mask.png" width="297.5" height="136">
           *Masked Version*
           <img src="images/common_widgets/progress_bars/w_progressbar_bottom_top_scale.png" width="293" height="142.5">
           *Scaled Version*
    - 填充样式：
        - **Mask** ：根据填充百分比和填充类型遮盖进度条的填充图像。
        - **缩放**：进度条的填充图像根据填充百分比和填充类型进行缩放和拉伸/压缩。
    - **Slider** *[Leaf Widget]*: Similar to the progress bar except it is interactable and you can change the orientation of it from horizontal to vertical and set its step size(for keyboard/gamepad presses).
         ![Slider Widget](images/common_widgets/w_slider.png)
- **Editable Text** *[Leaf Widget]*: A field for the user to be able to type in one line of text, allows for hint text and can be set to read only, is password(so it only shows a dot instead of the actual text), as well as being able to adjust settings on it like a normal text block widget.
     ![Editable Text Widget](images/common_widgets/w_editable_text.png)
- **Editable Text(Multi-Line)** *[Leaf Widget]*: Same as Editable Text widget except the user can type in multiple lines of text instead of one.
     ![Editable Text (Multi-Line) Widget](images/common_widgets/w_editable_text_multi.png)
- **Text Box** *[Leaf Widget]*: Same as Editable Text except it is wrapped with an image/material behind the text.
     ![Text Box Widget](images/common_widgets/w_textbox.png)
- **Text Box(Multi-Line)** *[Leaf Widget]*: Same as Editable Text(Multi-Line) except it is wrapped with an image/material behind the text.
     ![Text Box (Multi-Line) Widget](images/common_widgets/w_textbox_multi.png)
- **Spin Box** *[Leaf Widget]*: Displays a number(can be set to allow decimals or not) that the user can input or use the mouse and interact with it to slide and increase/decrease the number.
     ![Spin Box Widget](images/common_widgets/w_spinbox.png)
- **Combo Box(String)** *[Leaf Widget]*: A drop down box widget that displays a string(NOT TEXT, so its not localizable) value when selected and displays its options as well.
     ![Combo Box Widget](images/common_widgets/w_combobox.png)
- **Invalidation Box** *[Compound Widget]*: Can only have 1 child widget. This will control when that child widget is invalided for its layout/geometry passes, very useful for performance optimization. *Can't really use a picture for this because it wraps entirely around the widget and is invisible*.
- **Retainer Box** *[Compound Widget]*: Can only have 1 child widget. This will render a material over its child widget and only its child widget so it will not render that material over background space that the child widget does not occupy with its render.
     For example if you wrap a Text Block with a Retainer Box, the material will only apply over the text and not the space between each letter.
     ![Retainer Box Widget](images/common_widgets/w_retainerbox.png)
     *The retainer box's hierarchy in the designer*
     ![Retainer Box Material Widget](images/common_widgets/w_retainerbox_material.png)
     *The retainer box's material*
     ![Retainer Box Result Widget](images/common_widgets/w_retainerbox_result.png)
     *The retainer box at runtime(when running play in editor)*
- **Throbber** *[Leaf Widget]*: Displays an image/material in a specific animated pattern. Useful for showing something is loading.
     ![Throbber Widget](images/common_widgets/w_throbber.png)
- **Circular Throbber** *[Leaf Widget]* ：以圆形方式移动图像/材质的 throbber 版本。
    ![圆形颤动小部件](images/common_widgets/w_throbber_circular.png)
    - > *作者的注释； “如果可能的话，请在最终产品中使用默认图像以外的其他图像来更改颤动者的图像。我厌倦了在完整发布的产品中看到默认的 throbber，你可以做得更好！谢谢。”*
- **Spacer** *[Leaf Widget]*: This widget does not have a visual representation and it just provides padding and space between other widgets, recommended to use this rather than padding within slots if your UI design is changing constantly to make rapid iteration easier.
- **Background Blur** *[Compound Widget]*: Can only have 1 child widget. Basically an image widget with its child rendered behind it, and blurs the rendered result of that child widget using Gaussian Blur.
     Recommended to use this with proper clipping area’s setup otherwise use a custom material to make it easier for artists to modify.
     ![Background Blur Widget](images/common_widgets/w_background_blur.png)
- **Input Key Selector** *[Leaf Widget]*: Allows for the user to press an input while focusing this widget and it will display what input that is.
     ![Input Key Selector Widget](images/common_widgets/w_input_key_selector.png)
- **Canvas Panel** *[Panel Widget]*: This is the default widget found in newly created User Widgets, allows for the designer to place child widgets at arbitrary locations, anchored and z-ordered with other children of this canvas.
     It uses absolute layout for its placement so it's good for on screen indicators that follow a specific in-world object or something that can move around the entire screen.
     ![Canvas Panel Widget](images/common_widgets/w_canvas_panel.png)
- **Horizo​​ntal Box** *[Panel Widget]* ：允许其子部件以从左到右的水平流布局，索引 0 为最左边，最后一个部件为最右边。
    ![水平框小部件](images/common_widgets/w_horizontalbox.png)
- **Vertical Box** *[Panel Widget]*: Works the same as horizontal boxes except it lays out its children in a vertical flow moving from top to bottom, with 0 index as the farthest top widget and its last widget being the farthest bottom widget.
     *Vertical Box and Horizontal Box do not scroll, to allow for that you would have to use a scroll box widget or something similar.*
     ![Vertical Box Widget](images/common_widgets/w_verticalbox.png)
- **Scroll Box** *[Panel Widget]*: Works the same way as the vertical box AND the horizontal box(has to be set to either vertical or horizontal) but allows them to be scrollable. Does not support virtualization.
     ![Scroll Box Widget](images/common_widgets/w_scrollbox.png)
- **Size Box** *[Compound Widget]*: Can only have 1 child widget. Allows for this widget to specify the desired size of its child widget(since not all widgets will report a desired size because they are dependent on their own child widgets).
     ![Size Box Widget](images/common_widgets/w_sizebox.png)
- **Scale Box** *[Compound Widget]*: Can only have 1 child widget. Allows for this widget to have its child scaled to fit a constrained size on this box's allotted area.
     ![Scale Box Widget](images/common_widgets/w_scalebox.png)
     *In this example the scale box is resizing the image to fit uniformly*
- **Overlay** *[Panel Widget]*: Displays widgets stacked on top of each other based on their index within the child widgets. This widget is extremely useful to quickly overlay a widget over another widget quickly.
     ![Overlay Widget](images/common_widgets/w_overlay.png)
     *The text and the image are children of the overlay widget*
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
    - **Right to Left**: Forces a Right to Left layout flow.
         ![Flow Direction Preference](images/common_widget_func/flow_direction_preference.png)

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

Each widget interaction component has a virtual user associated with it that handles providing input to slate widgets.
 When activated the component will create an actual `FSlateUser` to simulate input and such.
 By default the engine will use Slate User index 8(the maximum allowed amount of slate users) instead of 0 and increments up from there,
 allowing for slate users and virtual users to not have conflicts.

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

For drawing a user widget to a `UTextureRenderTarget2D` the general process of it that Widget Component's do(along with extra work because it is a component and thus has to provide a scene proxy and such); is by creating a `FWidgetRenderer` and then getting the slate widget from the user widget and having the widget renderer draw it as a texture.
 Here is example code of using the widget renderer to create a texture from a `UUserWidget`(there are multiple implementations of `FWidgetRenderer::DrawWindow` &amp; `FWidgetRenderer::DrawWidget`so this is one of the ways):

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
- Widget Details: Widget details that such as visibility, focus, etc(anything that the slate widget exposes as an exposed property).
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

1. **Engine Heartbeat Tick**`FEngineLoop`: Unreal Engine’s heartbeat tick that notifies the platform SDKs to tick/update every frame.
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

- **Do Not Lock**: Do not lock the mouse cursor to the viewport.
     ![Do Not Lock Example](images/mouse_lock_modes/mouse_lock_mode_do_not_lock.png)
- **Lock On Capture**: Only lock the mouse cursor to the viewport when the mouse is captured(clicking or interacting with the viewport).
     ![Lock on Capture Example](images/mouse_lock_modes/mouse_lock_mode_lock_capture.png)
- **Lock Always**: Always lock the mouse cursor to the viewport, not allowing it to leave the viewport.
     ![Lock Always Example](images/mouse_lock_modes/mouse_lock_mode_lock_always.png)
- **Lock in Fullscreen**: Always lock the cursor if we're in fullscreen.
     ![Lock in Fullscreen Example](images/mouse_lock_modes/mouse_lock_mode_lock_fullscreen.png)

> 我想指定鼠标锁定模式是基于每个视口的，它负责每个玩家视口的分屏，而窗口则负责整个窗口的所有视口。

**[<span>⬆</span>返回顶部](#table-of-contents)**
<a name="unreals-focusing-system"></a>

## 11.0 Unreal的聚焦系统

虚幻引擎关于 Slate/UMG 的聚焦系统是**用户聚焦**的概念，其中称为**Slate User**的特定用户一次只能聚焦一个**Slate Widget** 。

Multiple Slate Users can focus on the same widget but a user cannot focus on two widgets at once (to do so would require you to have to build that functionality out on your own but at that point you’re probably fighting with it rather than working with it and saving time having to manage both the game you’re building and your custom focusing system).

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

1. **Does Not Intersect**: If the bounding rectangle of the widget is not intersecting with the sweep.
2. **Previous Widget Is Better**: If the widget isn’t closer than the previously checked widget then we failed because the previous widget was closer, the closest widget is always favored.
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
- **User**: The navigation event is a user generated event that was caused by game code, widgets, etc.

**[<span>⬆</span>返回顶部](#table-of-contents)**
<a name="split-screen"></a>

## 12.0分屏

Split Screen works where it has a **Game Layer Manager** that manages the viewport slate widget, which player that’s using that slate widget and how to shape that slate widget. The game layer manager is instanced by the game engine on initialization inside `GameEngine.cpp`(`UGameEngine`) and in `PlayLevel.cpp`(`UEditorEngine`) using a hard coded class so to build your own it would require modifying the engine’s source code in `UGameEngine::CreateGameViewportWidget` &amp; `UEditorEngine::GeneratePIEViewportWindow` (Epic if you see this, please change this to be a configurable class that can be specified in the project settings, you have it setup for the Game Viewport Client so it's already halfway there!).

> 根据您的用例，一种可能的途径是调用`UGameViewportClient::SetGameLayerManager` ，但请注意这可能会产生下游影响，并且可能需要来自不可重写的引擎代码的一些解决方法。

当在屏幕上显示小部件时，您有两个选择：

- **添加到视口**：将其添加到整个游戏视口，用该小部件覆盖两个玩家视口；对于暂停菜单或任何需要完全占据屏幕的东西很有用，并且应该影响所有其他本地玩家。
- **添加到玩家屏幕**：将其添加到特定玩家的视口，并且不会用该小部件覆盖其他玩家的视口；对于 HUD 或特定于该本地玩家而非所有其他玩家的任何内容很有用。

<a name="game-layer-manager"></a>

### 12.1 游戏层管理器

The Game Layer Manager is an interface that has the barebones information for getting the geometry of the viewport, getting the local player using this widget, adding layers of widgets(not recommended unless you know what you’re doing) which holds all slate widgets that have been added to that specific viewport, and for holding the actual game viewport slate widget. The Game Layer Manager is located in `SGameLayerManager.h/cpp` and you can find the interface (`IGameLayerManager`) and a compound widget that is the `SGameLayerManager` which handles displaying the game viewport widget(also useful as a basic example implementation of the interface) using a simple rectangle layout that is retrieved from the game viewport client. The `SGameLayerManager` also routes changes to DPI scale and for scaling the viewport based on the DPI scale value including all of its widget layers.

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
`FSceneViewport`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/Engine/Slate/FSceneViewport/) | SceneViewport.h | SceneViewport.cpp | This is where our viewport's slate widget is essentially housed.
`IInputProcessor`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/Slate/Framework/Application/IInputProcessor/) | IInputProcessor.h | [None] | 这是输入预处理器的基本接口类，如果您要创建一个，则将从该类继承。有关如何设置的示例，请查找 FAnalogCursor。
`FAnalogCursor`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/Slate/Framework/Application/FAnalogCursor/) | AnalogCursor.h | AnalogCursor.cpp |
`FSlateUser`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/Slate/Framework/Application/FSlateUser/) | SlateUser.h | SlateUser.cpp |
`ULocalPlayer`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/Engine/Engine/ULocalPlayer/) | LocalPlayer.h | LocalPlayer.cpp | 它还包含`FLocalPlayerContext` ，它对于传递到 UI 和其他对象以获得本地玩家、其玩家控制器、游戏实例、世界等的上下文非常有用。
`UWidgetLayoutLibrary`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/UMG/Blueprint/UWidgetLayoutLibrary/) | WidgetLayoutLibrary.h | WidgetLayoutLibrary.cpp |
`USlateBlueprintLibrary`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/UMG/Blueprint/USlateBlueprintLibrary/) | SlateBlueprintLibrary.h | SlateBlueprintLibrary.cpp | 请注意，脚本名称也写为“SlateLibrary”，以防您找不到它。
`UWidgetBlueprintLibrary`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/UMG/Blueprint/UWidgetBlueprintLibrary/) | WidgetBlueprintLibrary.h | WidgetBlueprintLibrary.cpp | 请注意，脚本名称也写为“WidgetLibrary”，以防您找不到它。
`UUserinterfaceSettings`<br> [文档](https://docs.unrealengine.com/latest/INT/API/Runtime/Engine/Engine/UUserInterfaceSettings/) | UserinterfaceSettings.h | UserinterfaceSettings.cpp | 这也是声明渲染焦点规则和您在这些设置中找到的一些其他数据类型的地方。

**[<span>⬆</span>返回顶部](#table-of-contents)**
