# The Vrui FAQ --- 有关Vrui工具包的问答

Tags：Vrui

[TOC]

## General Questions --- 一般的问题 

## 1. What is this Vrui thing, anyway? --- Vrui 到底是什么

Vrui (Virtual Reality User Interface) is a development toolkit for 3D graphics applications, with a strong focus on interactivity and immersive display environments. In the context of Vrui, immersive display nvironments mean displays consisting of one or more (large) stereoscopic screens, 3D head tracking, and 3D tracked input devices. Canonical examples of immersive display environments are CAVEs or head-mounted displays (HMDs), and, more recently, those composed of 3D TVs or consumer-level HMDs such as Oculus Rift, and gaming input devices.
> Vrui（虚拟现实用户界面）是3D图形应用程序的开发工具包，重点是交互性和沉浸式显示环境。在Vrui的上下文中，身临其境的显示环境意味着由一个或多个（大型）立体屏幕，3D头跟踪和3D跟踪输入设备组成的显示器。沉浸式显示环境的典型例子是CAVE或头戴式显示器（HMD），最近还包括由3D电视或消费级HMD（如Oculus Rift）和游戏输入设备组成的显示器。

Vrui's overriding goal is to support development of correct, portable, and usable applications. In this context, portable means that an application is developed in one environment – typically a desktop environment – but runs correctly in any environment. Usable means that a Vrui application run in a desktop environment is exactly as effective as a native desktop application, and that the same Vrui application run in a CAVE or other immersive environment type is exactly as effective as an application developed natively for that specific environment.
> Vrui的首要目标是支持开发正确，便携和可用的应用程序。在这种情况下，便携式意味着应用程序在一个环境（通常是桌面环境）中开发，但在任何环境中都能正常运行。可用意味着在桌面环境中运行的Vrui应用程序与本机桌面应用程序一样有效，并且在CAVE或其他沉浸式环境类型中运行的相同Vrui应用程序与为该特定环境本机开发的应用程序完全相同。

## 2. How is Vrui different from, say, glut? --- Vrui 与say， glut有什么不同

In certain ways, Vrui is very much like glut. It allows writing graphics applications without having to worry about the details of the underlying hardware or graphics system, and introduces very little overhead when writing simple applications. Also, Vrui allows applications direct access to OpenGL, to employ custom low-level code. In many cases, Vrui is a more powerful replacement for glut. It has little enough overhead that it is easily used for very simple applications, but at the same time already offers much additional functionality, such as simplified window management, multiple built-in 3D navigation metaphors, 3D measurement and annotation tools, virtual 3D input devices, recording and playback facilities, etc.
> 在某些方面，Vrui非常像glut。它允许编写图形应用程序，而不必担心底层硬件或图形系统的细节，并在编写简单应用程序时引入极少的开销。此外，Vrui允许应用程序直接访问OpenGL，以使用自定义的低级代码。在许多情况下，Vrui是更强大的替代品。它具有很少的开销，它很容易用于非常简单的应用程序，但同时已经提供了许多额外的功能，如简化窗口管理，多个内置3D导航隐喻，3D测量和注释工具，虚拟3D输入设备录音和播放设备等

Unlike glut, Vrui is portable to non-desktop environments. Here, portable means that a Vrui application is only written once, and then runs in any environment, without even having to be recompiled. This portability is achieved by shielding application writers much more from the underlying display system as glut does: Vrui applications do not have to open their windows or set up their OpenGL rendering contexts, and they do not directly receive input from mouse or keyboard. In fact, handling of input is probably the biggest difference between Vrui and glut (see How do I receive input from the keyboard?).
> 不像glut，Vrui可移植到非桌面环境。在这里，便携式意味着Vrui应用程序只写一次，然后在任何环境中运行，甚至不必重新编译。这种可移植性是通过屏蔽应用程序编写者从底层显示系统更多地实现的：Vrui应用程序不必打开他们的窗口或设置其OpenGL渲染上下文，并且它们不直接从鼠标或键盘接收输入。事实上，处理输入可能是Vrui和glut之间的最大区别（请参阅如何从键盘接收输入？）。

An additional difference between Vrui and glut is that Vrui consists of an entire hierarchy of layered libraries that work together to support developers in writing correct, portable, and usable applications. For example, Vrui contains a comprehensive cluster-transparent file I/O handling library, explicit high-performance intra-cluster communication, a comprehensive library for affine and projective 3D geometry, OpenGL support classes supporting generic programming, an OpenGL-based GUI widget set, and a scene graph library. While all these are completely optional, and Vrui is intentionally designed to be as compatible as possible with third-party libraries, developers are encouraged to use the highest-level available abstractions provided by the entire Vrui package.
> Vrui和glut之间的另一个区别是，Vrui由分层库的整个层次结构组成，共同支持开发人员编写正确，便携和可用的应用程序。例如，Vrui包含一个全面的集群透明文件I/O处理库，显式的高性能集群内通信，用于仿射和投影3D几何的综合库，支持通用编程的OpenGL支持类，基于OpenGL的GUI小组件集，和场景图库。虽然所有这些都是完全可选的，而Vrui有意设计为与第三方库尽可能兼容，但鼓励开发人员使用整个Vrui软件包提供的最高级别的可用抽象。

## 3. How is Vrui different from Qt, Gtk+, etc.? --- Vrui 与 Qt Gtk+ 等界面有什么不同

Qt, Gtk+, etc. are primarily 2D GUI toolkits, which also happen to offer widgets representing 3D graphics context for 3D rendering. Vrui, on the other hand, is primarily a 3D graphics toolkit, which also happens to offer widgets for (3D) GUIs. In other words, the GUI widget set is only a relatively small part of Vrui.
> Qt，Gtk等主要的2D GUI工具包，它也提供了代表3D渲染3D图形上下文的小部件。另一方面，Vrui主要是一个3D图形工具包，也可以为（3D）GUI提供小部件。换句话说，GUI小部件集只是Vrui的一小部分。

Ignoring everything else, though, the widget set offered by Vrui is comparable to those offered by Qt or Gtk+ or other 2D GUI toolkits, albeit not as complete (yet). Vrui's GUI widgets are three-dimensional, since they are intended to work in a virtual three-dimensional display space, but their functionality and layout is very similar to 2D GUI widgets. There are dialogs, menus, buttons, sliders, etc., just as usual. From a programming point of view, Vrui's GUI widgets more or less follow the approach of OSF/Motif, in that they primarily rely on automatic layout based on a hierarchical description, and on (C++-style) callbacks to connect widgets to application behavior. One could say that Vrui's widget set is a complete rip-off of OSF/Motif, translated to C++ and OpenGL.
> 但是，忽略其他所有内容，Vrui提供的窗口小部件与Qt或Gtk或其他2D GUI工具包提供的组件相当，尽管还不完整。Vrui的GUI小部件是三维的，因为它们旨在在虚拟三维显示空间中工作，但它们的功能和布局与2D GUI小部件非常相似。有对话框，菜单，按钮，滑块等，就像往常一样。从编程的角度来看，Vrui的GUI小部件或多或少遵循OSF/Motif的方法，因为它们主要依靠基于层次描述的自动布局，以及（C-style）回调将小部件连接到应用程序行为。可以说，Vrui的小部件是OSF/Motif的完整翻译，翻译成C和OpenGL。

## 4. How is Vrui different from Open SceneGraph, OpenSG, OSG, etc.? --- Vrui 与 OSG OpenSG 有什么不同？

While Vrui contains a scene graph library, its primary focus is on providing direct access to the underlying OpenGL 3D graphics library for custom applications with specific rendering needs. It is also possible, albeit often difficult in practice, to layer an existing scene graph library on top of Vrui, for the added benefit of portability to non-desktop environments.
> 虽然Vrui包含场景图库，但其主要重点是提供直接访问底层的OpenGL 3D图形库，以满足具有特定渲染需求的自定义应用程序。尽管实际上通常很难在Vrui之上分层现有的场景图库，从而为非桌面环境提供可移植性的附加优势。 

## 5. How is Vrui different from Ogre, Horde, Irrlicht, Sauerbraten, etc.? --- Vrui 与 Ogre, Horde, Irrlicht, Sauerbraten等有什么不同？

Vrui is not a complete game engine, but a toolkit to develop correct, portable, and usable 3D graphics applications. Vrui does not address asset management, handling of 3D modeling data formats, view-dependent or multi-resolution rendering, advanced lighting, collision detection, game logic, artifical intelligence, etc. These aspects are delegated to application code, or to higher-level libraries, which could be part of Vrui itself (like the scene graph library), or developed by third parties.
> Vrui不是一个完整的游戏引擎，而是开发正确，便携和可用的3D图形应用程序的工具包。Vrui不处理资产管理，处理3D建模数据格式，视图依赖或多分辨率渲染，高级照明，碰撞检测，游戏逻辑，人工智能等。这些方面被委托给应用程序代码或更高级别程序库，可以是Vrui本身的一部分（如场景图库），或由第三方开发。

In other words, Vrui is primarily a display and interaction engine, and comparable to the lower levels contained in any high-level game engine. As a result, given a game engine that has a clear separation of responsibilty between its layers, it can be possible to layer an existing game engine on top of Vrui, for the added benefit of being able to run that game engine in non-desktop environments. However, because most commercial or free game engines do not make this clear distinction, doing so might be difficult in practice.
> 换句话说，Vrui主要是一个显示和交互引擎，与任何高级游戏引擎中包含的较低级别相当。因此，给定一个游戏引擎，其层次之间有明确的责任分离，可以在Vrui之上分层现有的游戏引擎，以便在非桌面上运行该游戏引擎的附加益处环境。然而，因为大多数商业或免费的游戏引擎没有明确区分，所以在实践中可能会很困难。

## 6. How is Vrui different from other VR toolkits like Cavelib, FreeVR, VR Juggler, etc.? --- Vrui 与其它VR工具包Cavelib, FreeVR, VR Juggler等有什么不同？

VR toolkits like Cavelib, FreeVR, VR Juggler, etc. are decidedly low-level toolkits. They shield application developers from some aspects of the underlying display and input device hardware, but typically not enough to support truly portable or usable applications. In a certain way, the analogous comparison between Vrui and, say, Cavelib, in the desktop world is that between X Windows and an X-based GUI toolkit like Qt. While X hides differences in display and input device hardware, and even distribution to some extent, applications based directly on X have to contain their own GUI code. Before GUI toolkits became available, this really happened and resulted in completely different user interfaces between applications. One application might have used the middle mouse button to scroll; another might have used a proto-scroll bar; another might have used clicks into  a "map" of scrollable space; in short, the situation was untenable.
> 像Cavelib，FreeVR，VR Juggler等VR工具包是低级别的工具包。它们可以屏蔽应用程序开发人员的底层显示和输入设备硬件的某些方面，但通常不足以支持真正便携或可用的应用程序。在某种程度上，Vrui和Cavelib之间在桌面世界中的类似比较就是X Windows和基于X的GUI工具包，如Qt。虽然X隐藏了显示和输入设备硬件的差异，甚至在一定程度上分配，但直接基于X的应用程序必须包含自己的GUI代码。在GUI工具包可用之前，这真的发生了，并导致应用程序之间完全不同的用户界面。一个应用程序可能使用鼠标中键滚动;另一个可能使用了一个原卷轴;另一个可能使用点击进入可滚动空间的“地图”;总之，情况是站不住脚的。

GUI toolkits like Qt or Gtk+ remedied that problem by introducing a higher level. Instead of working with windows and mouse events, applications could now use "widgets" with specific purposes and widget events, leading to much better user interfaces, and, more important, consistent interfaces between applications. 
> 像Qt或Gtk这样的GUI工具包通过引入更高级别来解决这个问题。应用程序现在可以使用具有特定目的和小部件事件的“小部件”，而不是使用Windows和鼠标事件，从而导致更好的用户界面，更重要的是应用程序之间的一致接口。

Vrui attempts to do to immersive 3D graphics what Qt et al. did to 2D GUIs. It provides higher-level interfaces, to prevent individual applications from "reinventing the wheel," and instead foster consistent user interfaces. Instead of dealing directly with OpenGL windows and 4x4 matrices representing input devices, Vrui applications render into a toolkit-provided 3D application space, and receive higher-level events from input devices. As a concrete example, in other toolkits navigation, i.e., the mapping from 3D application space to display space, is handled by each application individually, whereas in Vrui it is handled by the toolkit. Overall, the result is that
Vrui applications with widely differing purposes "look & feel" the same.
> Vrui试图为身临其境的3D图形做什么Qt等对2D GUI。它提供了更高级别的接口，以防止各个应用程序“重新发明轮子”，而是建立一致的用户界面。 Vrui应用程序不直接处理OpenGL窗口和代表输入设备的4x4矩阵，而是将其转换为工具包提供的3D应用程序空间，并从输入设备接收更高级别的事件。作为一个具体的例子，在其他工具包导航中，即从3D应用程序空间到显示空间的映射，由每个应用程序单独处理，而在Vrui中，它由工具包处理。总的来说，结果是这样的Vrui应用程序具有广泛的不同目的“看&感觉”一致。

An intended side effect of Vrui's higher-level abstractions is that Vrui applications are portable between vastly different types of display environments. A single Vrui application will run in a CAVE like a native CAVE application, and will run on the desktop very similarly to a native desktop application. In comparison, other toolkits require applications to be developed for a narrower range of target environments, sometimes requiring different code for single-screen and multi-screen environments, and none that we are aware of support running applications on desktop environments in any way that is useful beyond basic debugging.
> Vrui更高级别抽象的一个预期的副作用是，Vrui应用程序可以在大量不同类型的显示环境之间移植。单个Vrui应用程序将在CAVE中运行，如本机CAVE应用程序，并且将与台式机应用程序非常相似地在桌面上运行。相比之下，其他工具包需要为较窄范围的目标环境开发应用程序，有时候需要不同的代码进行单屏幕和多屏幕环境，我们也不知道支持在桌面环境中运行应用程序，有用的超越基本调试。

An additional difference is that Vrui consists of an entire hierarchy of layered libraries that work together to support developers in writing correct, portable, and usable applications. For example, Vrui contains a comprehensive cluster-transparent file I/O handling library, explicit high-performance intra-cluster communication, a comprehensive library for affine and projective 3D geometry, OpenGL support classes supporting generic programming, an OpenGL-based GUI widget set, and a scene graph library. While all these are completely optional, and Vrui is intentionally designed to be as compatible as possible with third-party libraries, developers are encouraged to use the highest-level available abstractions provided by the entire Vrui package.
> 另外一个区别是，Vrui由分层库的整个层次结构组成，它们共同支持开发人员编写正确，便携和可用的应用程序。例如，Vrui包含一个全面的集群透明文件I/O处理库，显式的高性能集群内通信，用于仿射和投影3D几何的综合库，支持通用编程的OpenGL支持类，基于OpenGL的GUI小组件集，和场景图库。虽然所有这些都是完全可选的，而Vrui有意设计为与第三方库尽可能兼容，但鼓励开发人员使用整个Vrui软件包提供的最高级别的可用抽象。

For example, Vrui's underlying geometry library is used throughout Vrui's API instead of passing positions or orientations as flat 3- or 4-element arrays or 4x4 matrices. Vrui uses abstract geometry classes such as Point and Vector for affine points and vectors, respectively, and a hierarchy of abstract transformation classes from translations or rotations only over rigid body transformations to fully-general affine or projective transformations. All these classes have full sets of algebraic operations, which means application code can in almost all cases use them as black boxes. These higher-level classes significantly reduce the burden on application developers to either write their own algebraic operations, such as matrix inversion, or continuously convert back-and-forth between the toolkit's flat representation and the representation of a third-party geometry library they want to use. An intended side-effect of the use of higher-level abstractions at the API is that implicit constraints can be made explicit. For example, tracked 6-DOF input devices can only change position and orientation, and instead of representing those as generic projective transformations, i.e., 4x matrices, Vrui represents them as rigid-body transformations, i.e., a translation vector plus a unit quaternion. This makes arithmetic involving input devices easier, more efficient, and more robust. That said, all classes have methods to convert from/to flat array representations to be backwards-compatible with third-party libraries an application developer might want to use.
> 例如，Vrui的底层几何库用于Vrui的API，而不是将位置或方向作为平面的3或4元素阵列或4x4矩阵。 Vrui分别使用抽象几何类，例如点和向量，用于仿射点和向量，以及仅从刚体转换到完全一般仿射或投影变换的平移或旋转的抽象变换类的层次结构。所有这些类都有全套的代数运算，这意味着应用程序代码在几乎所有情况下都可以使用它们作为黑盒子。这些更高级别的课程大大减轻了应用程序开发人员编写自己的代数运算（如矩阵反演）的负担，或者在工具箱的平面表示和他们想要的第三方几何库的表示之间连续转换使用。在API上使用更高层次的抽象的一个预期的副作用是隐式约束可以被明确。例如，跟踪的6自由度输入设备只能改变位置和方向，而不是将其表示为通用投影变换，即4x矩阵，Vrui将它们表示为刚体变换，即平移向量加单位四元数。这使得涉及输入设备的算法更容易，更高效，更强大。也就是说，所有类都有从/平面数组表示形式转换为与应用程序开发人员可能要使用的第三方库向后兼容的方法。

## User Questions --- 用户的问题 

## 7. On what operating systems does Vrui run? --- Vrui 能够运行在什么操作系统上？

Vrui runs on UNIX-like operating systems. It is primarily being developed on Linux, specifically 64-bit Fedora Linux, but it builds and runs without problems on any other Linux distribution.
> Vrui在类UNIX操作系统上运行。它主要是在Linux上开发的，特别是64位Fedora Linux，但它在任何其他Linux发行版上构建和运行没有问题。

Vrui also builds and runs on Mac OS X, but there are currently some Linux-only features, particularly handling of sound and video, some non-standard required libraries – libpng, libjpeg, libtiff, libusb – that need to be installed from source or using software management systems such as homebrew, and Vrui is generally more thoroughly tested on Linux.
> Vrui还在Mac OS X上构建并运行，但目前有一些仅限Linux的功能，特别是处理声音和视频，一些非标准的必需库（libpng，libjpeg，libtiff，libusb）需要从源或使用软件管理系统（如自制软件）和Vrui通常在Linux上进行更彻底的测试。

Vrui does not build on Windows. Microsoft's Visual C++, at least the most recent version tested, cannot deal with some of the C++ template constructs used in Vrui. Other C++ compilers, such as Intel's, can do those, but they are typically quite expensive. Even with a proper C++ compiler, several of the underlying architecture decisions in Vrui are deeply rooted in the UNIX paradigm. Porting Vrui would take significant initial effort, and continual effort to maintain a split codebase.
> Vrui不在Windows上构建。 Microsoft的Visual C，至少是测试的最新版本，不能处理Vrui中使用的一些C模板结构。其他C编译器，如英特尔，可以做到这些，但它们通常相当昂贵。即使使用适当的C编译器，Vrui中的几个基础架构决策深深植根于UNIX范例。移植Vrui将采取重要的初步努力，并不断努力维护拆分代码库。

Vrui does build and run under UNIX emulation systems such as cygwin, but it will not be particularly useful because no version of cygwin we have tried offers hardware-accelerated 3D graphics. As Vrui applications rely heavily on high-performance graphics, they will barely be usable under cygwin.
> Vrui在UNIX仿真系统（如cygwin）下构建并运行，但并不特别有用，因为我们尝试的没有版本的cygwin提供硬件加速的3D图形。由于Vrui应用程序严重依赖于高性能图形，因此它们在cygwin下几乎无法使用。

The same problem applies if Linux is run inside a virtual machine. No virtual machine hypervisor we have tried offers hardware-accelerated 3D graphics inside the virtual machine, and Vrui applications will barely be usable as a result.
> 如果Linux在虚拟机中运行，则同样的问题也适用。没有虚拟机虚拟机管理程序，我们已经尝试在虚拟机内部提供硬件加速的3D图形，因此Vrui应用程序几乎不可用。

## 8. Is it difficult to install Vrui? --- 安装Vrui麻烦不？

Not really. Vrui's build system is intended to make "simple" installations simple. By default, Vrui installs itself into the /usr/local directory hierarchy. The simplest installation procedure is thus (`$` indicates commands entered at the terminal prompt):
> 不是真的。 Vrui的构建系统旨在使“简单”安装变得简单。默认情况下，Vrui将自身安装到/usr/local目录层次结构中。因此，最简单的安装过程（$表示在终端提示符处输入的命令）如下：

Go to the Vrui download web site and make note of the most recent Vrui release, Vrui-[major].[minor]-[build], e.g., Vrui-4.2-001.
> 访问Vrui下载网站，并记录最新的Vrui版本Vrui-[major].[minor]-[build]，例如Vrui-4.2-001。

Download and unpack the current Vrui source tarball into a src directory:
> 将当前Vrui源文件压缩包下载并解压缩到src目录中：

```
$ cd ~
$ mkdir src
$ cd src
$ wget -O - [http://idav.ucdavis.edu/~okreylos/ResDev/Vrui/Vrui-<major>.<minor>-<build>.tar.gz](http://idav.ucdavis.edu/~okreylos/ResDev/Vrui/Vrui-<major>.<minor>-<build>.tar.gz) | tar xfz -
$ ls
Vrui-<major>.<minor>-<build>
```

This created Vrui's build directory, named by major and minor version number and build number, such as /home/alice/src/Vrui-4.2-001. Enter the build directory, and build and install Vrui:
> 这创建了Vrui的构建目录，由主要和次要版本号和内部版本号命名，如/home/alice/src/Vrui-4.2-001。输入构建目录，并构建并安装Vrui：

```
$ cd Vrui-<major>.<minor>-<build>
$ make
... lots of text...
$ sudo make install
(enter administrator user's password)
... lots of text...
```

This created several directories inside /usr/local containing all Vrui header and library files, and some utility programs. To build Vrui's example programs and run a test, follow this directly by:
> 这在/usr/local中创建了几个包含所有Vrui头文件和库文件的目录，以及一些实用程序。要构建Vrui的示例程序并运行测试，请直接按以下方式：

```
$ cd ExamplePrograms
$ make
... lots of text...
$ ./bin/ShowEarthModel
```

Most Vrui-based applications are configured to work with the default installation, so building an application is typically as simple as downloading and unpacking the tarball and running "make."
> 大多数基于Vrui的应用程序都配置为使用默认安装，因此构建应用程序通常与下载和解压缩tarball并运行“make”一样简单。

To install optional system packages for additional Vrui functionality see the "quick installation guide" on the Vrui download page.
> 要安装附加Vrui功能的可选系统软件包，请参阅Vrui上的“快速安装指南”下载页面

## 9. How do I install Vrui in a system location? --- 如何在系统位置安装Vrui?

Note: This question and answer are mostly obsolete; as of version 4.0, Vrui installs itself under /usr/local by default.
> 注意：这个问题和答案大多是过时的;从版本4.0开始，Vrui默认安装在/usr/local下。

Short answer:
> 简答：

```
$ make INSTALLDIR=/usr/local
... lots of text...
$ sudo make INSTALLDIR=/usr/local install
[sudo] password for <user>: (enter password)
... lots of text...
```

Long answer:
> 详答：

By default, Vrui installs itself in ~/Vrui-<major>.<minor>, i.e., in the installing user's home directory. For legacy reasons, Vrui does not use the common ./configure, make, make install toolchain, but Vrui's build system offers the same level of flexibility. Installation targets can be defined as a whole or individually by editing Vrui's makefile, or by passing VARIABLE=<value> arguments on make's command line. When overriding variables via make's command line, it is important to use the same arguments during make and make install (but see Creating Binary Packages for an exception).
> 默认情况下，Vrui安装在〜/Vrui-。，即安装用户的主目录中。出于传统原因，Vrui不使用常见的./configure，make，make安装工具链，但是Vrui的构建系统提供了相同的灵活性。安装目标可以通过编辑Vrui的makefile或通过在make的命令行上传递VARIABLE=参数来整体或单独定义。当通过make的命令行覆盖变量时，在make和make install期间使用相同的参数很重要（但请参阅为异常创建二进制包）。 

The most basic installation override is the INSTALLDIR variable defined at the very top of Vrui's makefile (to the default value `$`(HOME)/Vrui-<major>.<minor>). Unless detail changes are made, Vrui installs itself into `$`(INSTALLDIR), using the canonical include, lib(64), bin, etc, and share subdirectories. The easiest (and most easily un-installed) way to install Vrui system-wide is to set INSTALLDIR to /usr/local/Vrui-<major>.<minor>: 
> 最基本的安装覆盖是在Vrui的makefile顶部定义的INSTALLDIR变量（默认值为`$`（HOME）/Vrui-[major].[minor]）。除非进行细节更改，Vrui会使用规范include（lib），lib（64），bin等安装在`$`（INSTALLDIR）中，并共享子目录。安装Vrui系统的最简单（最容易未安装）的方式是将INSTALLDIR设置为/usr/local/Vrui-[major].[minor]:

```
$ make INSTALLDIR=/usr/local/Vrui-<major>.<minor>
... lots of text...
$ sudo make INSTALLDIR=/usr/local/Vrui-<major>.<minor> install
[sudo] password for <user>: (enter password)
... lots of text...
```

In this case, "make install" has to be run as super-user because it will create a subdirectory in a system directory. This simple installation procedure will create a system-wide installation in /usr/local/Vrui-<major>.<minor>, and un-installing Vrui will be as simple as sudo rm -rf /usr/local/Vrui-<major>.<minor>, but it will not follow the POSIX conventions of installing software in /usr/local. According to POSIX, include files go into /usr/local/include, library files into /usr/local/lib(64), executables into /usr/local/bin, configuration files into subdirectories of /usr/local/etc, shared files into subdirectories of /usr/local/share, configuration files for pkg-config into /usr/local/lib(64)/pkgconfig, and documentation files into subdirectories of /usr/local/share/doc. Vrui's build system has a variable for each of these destinations: HEADERINSTALLDIR, LIBINSTALLDIR, EXECUTABLEINSTALLDIR, ETCINSTALLDIR, SHAREINSTALLDIR, PKGCONFIGINSTALLDIR, and DOCINSTALLDIR, respectively. For example, to install Vrui-4.2-<build> in a POSIX-compliant fashion:
> 在这种情况下，“make install”必须作为超级用户运行，因为它会在系统目录中创建一个子目录。这个简单的安装过程将在/usr/local/Vrui-中创建一个全系统的安装，并且卸载Vrui将与sudo rm -rf /usr/local/Vrui-。一样简单，但不会遵循在/usr/local中安装软件的POSIX约定。根据POSIX，将文件放入/usr/local/include，库文件放入/usr/local/lib（64），将可执行文件放入/usr/local/bin，将配置文件复制到/usr/local/etc的子目录中，共享将文件复制到/usr/local/share的子目录中，将pkg-config的配置文件导入到/usr/local/lib（64）/pkgconfig中，并将文档文件导入到/usr/local/share/doc的子目录中。 Vrui的构建系统对于每个目的地分别有一个变量：HEADERINSTALLDIR，LIBINSTALLDIR，EXECUTABLEINSTALLDIR，ETCINSTALLDIR，SHAREINSTALLDIR，PKGCONFIGINSTALLDIR和DOCINSTALLDIR。例如，要以符合POSIX的方式安装Vrui-4.2-

```
$ make HEADERINSTALLDIR=/usr/local/include/Vrui-4.2 LIBINSTALLDIR=/usr/local/lib64/Vrui-4.2 EXECUTABLEINSTALLDIR=/usr/local/bin \
ETCINSTALLDIR=/usr/local/etc/Vrui-4.2 SHAREINSTALLDIR=/usr/local/share/Vrui-4.2 PKGCONFIGINSTALLDIR=/usr/local/lib64/pkgconfig \
DOCINSTALLDIR=/usr/local/share/doc/Vrui-4.
$ sudo make HEADERINSTALLDIR=/usr/local/include/Vrui-4.2 LIBINSTALLDIR=/usr/local/lib64/Vrui-4.2 EXECUTABLEINSTALLDIR=/usr/local/bin \
ETCINSTALLDIR=/usr/local/etc/Vrui-4.2 SHAREINSTALLDIR=/usr/local/share/Vrui-4.2 PKGCONFIGINSTALLDIR=/usr/local/lib64/pkgconfig \
DOCINSTALLDIR=/usr/local/share/doc/Vrui-4.2 install
```

Those command lines are quite a mouthful (and it's probably more practical to make these changes directly in Vrui's makefile), but they are appropriate for shell scripting, such as when creating binary packages. By the way, Vrui's build system prints all installation directories at the beginning of its build output. A quick way to check is to run make <arguments> config.
> 那些命令行是相当的口（在Vrui的makefile中直接进行这些更改可能更实际），但它们适用于shell脚本，例如在创建二进制包时。顺便说一下，Vrui的构建系统在其构建输出的开头打印所有的安装目录。一个快速检查的方法是运行make config。

That said, Vrui's build system contains logic for several common installation cases. Specifically, there is a shortcut for the exact installation structure spelled out in detail above:
> 也就是说，Vrui的构建系统包含几个常见安装情况的逻辑。具体来说，上面详细说明了确切的安装结构的快捷方式：

```
$ make INSTALLDIR=/usr/local
$ sudo make INSTALLDIR=/usr/local install
```

And there is yet another shortcut, for deep system-wide installations in 
> 另外还有一个捷径，用于深入系统的安装
```
/usr/include/Vrui-<major>.<minor>,
/usr/lib(64)/Vrui-<major>.<minor>, /usr/bin, /etc/Vrui-<major>.<minor>, /usr/share/Vrui-<major>.<minor>, /usr/lib(64)/pkgconfig, and
/usr/share/doc/Vrui-<major>.<minor>:

$ make SYSTEMINSTALL=1 INSTALLDIR=
$ sudo make SYSTEMINSTALL=1 INSTALLDIR= install
```

(That's not a typo, the value after INSTALLDIR= must be the empty string.) However, the Linux file system standard states that software locally installed from source should not go into /usr and /etc, but into /usr/local. Unless one is...
> （这不是打字错误，INSTALLDIR=之后的值必须是空字符串）。但是，Linux文件系统标准规定，从源代码本地安装的软件不应该进入/ usr和/ etc，而是进入/ usr / local。除非是...

### Creating Binary Packages --- 创建二进制包

Most Linux distributions have a way of distributing software as binary packages, under the auspices of a more or less smart package manager. Installing binary packages has benefits for end users, but Vrui is canonically distributed as source tarballs. However, building binary packages is quite straightforward. While the details depend on the package manager used by a given Linux distribution (rpm, dpkg, ...), the common fundamental approach is to build and prepare software inside a fake root directory, and then package all created files under that fake root into a single archive for distribution.
> 大多数Linux发行版都有一种将软件分发为二进制软件包的方式，由一个或多或少的智能软件包管理器支持。安装二进制包对最终用户有好处，但是Vrui被规范地分发为源压缩包。然而，构建二进制包是非常简单的。虽然详细信息取决于给定Linux发行版（rpm，dpkg，...）使用的程序包管理器，但常见的基本方法是在假根目录下构建和准备软件，然后将所有创建的文件打包在该伪造根目录下成一个档案供发行。

The tricky bit here is that the software has to be configured and built for its final destination, such as /usr/bin etc., but has to be installed into the fake root directory for packaging. Vrui's build system simplifies that by using the combination of the SYSTEMINSTALL and INSTALLDIR variables. To build and configure Vrui for the final destination, run
> 这里的棘手之处在于，必须为其最终目的地（如/usr/bin等）配置和构建软件，但必须将其安装到假根目录中进行打包。 Vrui的构建系统通过使用SYSTEMINSTALL和INSTALLDIR变量的组合来简化。要为最终目的地构建和配置Vrui，请运行

```
$ make SYSTEMINSTALL=1 INSTALLDIR=
```

and to install the built software into a fake root for packaging, run
> 并将构建的软件安装到假根中进行打包，运行

```
$ make SYSTEMINSTALL=1 INSTALLDIR=/path/to/fake/root install
```

(sudo is not necessary here because the fake root directory is assumed to be under the user's home directory.)
> （这里不需要sudo，因为假根目录被假定在用户的主目录下）。

Then, the final step is the distribution-specific equivalent of
> 那么最后一步是分配特定的等价物

```
$ cd /path/to/fake/root
$ tar cfz Vrui-<major>.<minor>.binary.tar.gz *
```

## 10. What is the difference between Vrui.cfg and VRDevices.cfg? --- Vrui.cfg和VRDevices.cfg有什么区别？

Vrui contains its own low-level device driver, VRDeviceDaemon, to talk to input device hardware such as 3D tracking systems, data gloves, wands, joysticks, etc. and convert their data streams into a unified format to be accepted by the Vrui toolkit run-time environment. This device driver is configured for a particular display environment via the VRDevices.cfg configuration file. This file defines what types of input device hardware are present, and how they work together in the same space.
> Vrui包含自己的低级设备驱动程序VRDeviceDaemon，与3D跟踪系统，数据手套，魔杖，操纵杆等输入设备硬件进行通信，并将其数据流转换为统一格式，以被Vrui工具包运行时间环境。该设备驱动程序通过VRDevices.cfg配置文件为特定的显示环境配置。该文件定义了哪些类型的输入设备硬件存在，以及它们如何在同一空间中协同工作。

The Vrui run-time environment itself is configured via the Vrui.cfg configuration file. This file defines the complete display environment, including the positions, orientations, and sizes of all screens, all viewers present in an environment, the OpenGL windows used to render 3D views to those screens, the number and types of 3D input devices presented by the low-level device driver, which tools are available, etc.
> Vrui运行时环境本身通过Vrui.cfg配置文件配置。该文件定义了完整的显示环境，包括所有屏幕的位置，方向和大小，环境中存在的所有观看者，用于将3D视图呈现给这些屏幕的OpenGL窗口，由3D显示屏提供的3D输入设备的数量和类型低级设备驱动程序，哪些工具可用等

Concretely, VRDevices.cfg is read when the low-level device driver is started, whereas Vrui.cfg is read whenever a Vrui application is started. Both configuration files together define the complete set-up of the display environment in which Vrui runs, and need to be created and/or adapted carefully based on the details of a concrete environment. In a shared display environment like a CAVE, this task would typically fall to a dedicated system integrator / administrator, whereas configuration is more or less unnecessary in single-user desktop environments, where the default setup will cover the common cases.
> 具体来说，当低级设备驱动程序启动时读取VRDevices.cfg，而每当启动Vrui应用程序时，都会读取Vrui.cfg。两个配置文件一起定义了Vrui运行的显示环境的完整设置，并且需要根据具体环境的细节进行仔细的创建和/或修改。在像CAVE这样的共享显示环境中，这个任务通常会落在一个专门的系统集成商/管理员身上，而在单用户桌面环境中，配置或多或少是不必要的，默认设置将覆盖常见的情况。

## 11. Why does Vrui go all wonky when I use two monitors in a single-desktop setup? --- 当我在单桌面设置中使用两台显示器时，为什么Vrui全部都不会出现

In its default desktop configuration, Vrui tries to determine the actual size of the environment's display by querying the X Windows system. Normally, this ensures that Vrui's rendering has the correct aspect ratio, e.g., spheres show up as spheres instead of ellipsoids, and that it has the correct size, e.g., the scale bar has the correct length, and that fonts show up at the correct sizes. However, X sometimes lies about the sizes of its attached displays, especially when multiple displays are joined in a single-desktop setup (such as Nvidia's TwinView). This is especially bad when the joined displays have different resolutions, say a laptop display with 100dpi next to a 3D TV with 30dpi.
> 在其默认桌面配置中，Vrui尝试通过查询X Windows系统来确定环境显示的实际大小。通常，这样可以确保Vrui的渲染具有正确的长宽比，例如，球体显示为球体而不是椭圆体，并且具有正确的大小，例如，比例尺具有正确的长度，并且字体显示在正确的大小。然而，X有时候会涉及其附加显示器的尺寸，特别是当多台显示器加入到单桌面设置（如Nvidia的TwinView）中时。当连接的显示器具有不同的分辨率时，这是特别糟糕的，即使用具有30dpi的3D电视旁边的100dpi的笔记本电脑显示器。

In the first scenario, where all displays have comparable resolutions, but X lies, you will have to disable automatic screen size detection (by setting autoScreenSize to false in your window section), and enter the correct screen size in your screen section. In single-desktop setups, the screen size is the combined size of all screens, not that of a single screen.
> 在第一种情况下，所有显示器都具有可比较的分辨率，但X位于，您将不得不禁用自动屏幕尺寸检测（通过将窗口部分中的autoScreenSize设置为false），并在屏幕部分中输入正确的屏幕尺寸。在单桌面设置中，屏幕尺寸是所有屏幕的组合大小，而不是单个屏幕的尺寸。

This will not work if the screens' resolutions are different. If that's the case, it will not be possible to drag Vrui windows between screens without introducing major distortions -- after all, the Vrui run-time now has to deal with a single (virtual) display that suddenly changes resolution in the middle. You will have to decide which of the screens is the main screen, and configure Vrui for that screen. Then, if an application window is dragged to another screen, partially or completely, behavior is undefined.
> 如果屏幕的分辨率不同，这将不起作用。如果是这种情况，则不可能在屏幕之间拖动Vrui窗口，而不会引起严重的扭曲 - 毕竟，Vrui运行时间现在必须处理单个（虚拟）显示屏，突然改变中间的分辨率。您将必须确定哪个屏幕是主屏幕，并为该屏幕配置Vrui。然后，如果应用程序窗口被部分或全部拖动到另一个屏幕，行为是未定义的。

To set up a main screen, add a panningDomain setting to the appropriate window section in Vrui.cfg, and set the panning domain to the position and size of the main screen. For example, if the main screen has a resolution of 1920x1080, and is to the right of a secondary screen with resolution 1440x900, panningDomain would be (1440, 0), (1920, 1080). Then set autoScreenSize to false, and set the proper size of the main screen in the screen section associated with the window. This will correlate the pixel size of the panning domain with the physical size of the screen, and Vrui will render to scale. If Vrui windows are dragged outside the panning domain, things will get weird, however.
> 要设置主屏幕，请将panningDomain设置添加到Vrui.cfg中的相应窗口部分，并将平移域设置为主屏幕的位置和大小。例如，如果主屏幕的分辨率为1920x1080，并且在分辨率为1440x900的二次屏幕的右侧，则panningDomain将为（1440,0），（1920，1080）。然后将autoScreenSize设置为false，并在与窗口相关联的屏幕部分中设置正确的主屏幕大小。这将使平移域的像素尺寸与屏幕的物理尺寸相关联，Vrui将按比例渲染。如果Vrui窗口被拖动到平移域之外，那么事情会变得很奇怪。

Here is an example configuration fragment for a 30 dpi 1920x1080 main screen to the right of a 1440x100dpi secondary screen:
> 以下是1440x100dpi二级屏幕右侧30 dpi 1920x1080主屏幕的示例配置片段：

```
section Screen0 # 100dpi 17" laptop screen, left of main screen
name Screen
origin (-46.4, 0.0, 9.0)
width 14.
height 9.
endsection

section Screen1 # 30 dpi 72" HDTV, centered around the origin
name Screen
origin (-32.0, 0.0, -18.0)
width 64.
height 36.
endsection

section Window
windowPos (1800, 300), (800, 600)
autoScreenSize false
screenName Screen1 # Use HDTV as main screen
panningViewport true
panningDomain (1440, 0), (1920, 1080)
endsection
```

The "Screen0" section is not actually used in this example.
> 在这个例子中没有实际使用“Screen0”部分。

## 12. How do I set up multiple rendering windows, for example to drive multiple projectors? --- 如何设置多个渲染窗口，例如驱动多台投影机

Vrui is quite flexible with regards to multi-window/multi-display rendering. The Vrui run-time environment can dynamically configure itself to run in a cluster with multiple rendering nodes, on a single machine with multiple graphics cards, or on a single machine with a single graphics card and multiple displays connected to multiple video outputs, or any combination of these. For example, when running a "super CAVE" with six walls and two pairs of stacked 4K projectors generating 4000x4000 pixel stereoscopic images per wall, one could use a seven-node cluster, where the head node drives a standard desktop monitor and is the user-facing console, and each of the six render nodes has two high-end graphics cards with two projectors connected to each graphics card. How to configure Vrui for such environments, and how to maximize rendering performance via context sharing and parallel rendering, is explained in detail in the Multi-window Rendering in Vrui document.
> 对于多窗口/多显示渲染，Vrui非常灵活。Vrui运行时环境可以自动配置为在具有多个渲染节点的集群中运行，在具有多个显卡的单个机器上运行，或者在具有单个显卡和单个显示器连接到多个视频输出的单个机器上运行，或任何这些的组合。例如，当使用六个墙壁和两对堆叠的4K投影机运行“超级CAVE”，每个墙壁生成4000x4000像素的立体图像时，可以使用七节点群集，其中头节点驱动标准桌面显示器，并且是用户六个渲染节点中的每一个具有两个高端显卡，两个投影机连接到每个显卡。在Vrui文档中的多窗口渲染中详细介绍了如何通过上下文共享和并行渲染来配置Vrui，以及如何通过上下文共享和并行渲染最大化渲染性能。

As far as application programmers are concerned, the details of managing multiple windows, sharing or replicating OpenGL contexts, and parallel rendering are completely hidden by the Vrui API. See the GLContextData document for a detailed explanation of the underlying abstraction mechanism.
> 就应用程序员而言，Vrui API完全隐藏了管理多个窗口，共享或复制OpenGL上下文以及并行渲染的细节。有关底层抽象机制的详细说明，请参阅GLContextData文档。

## Developer Questions --- 开发者的问题 

## 13. How do I get started developing in Vrui? --- 我如何开始在Vrui开发

Unfortunately, Vrui does not yet have comprehensive developer guides. As a prerequisite, developing for Vrui requires a good understanding of 3D graphics, especially of the OpenGL graphics library on which Vrui is based, and of C++. With these in place, starting Vrui developers are encouraged to think of Vrui as being very similar to glut, and take it from there. Vrui comes with a set of template or example applications showing the (little amount of) boilerplate code required to set up a Vrui application, and detailed comments explaining where one would put application and 3D rendering code, for example. If a developer has code for a glut application, it is usually enough to strip out any OpenGL setup and viewpoint navigation code (OpenGL setup and navigation are handled by Vrui), paste the rest into the simplest Vrui template, and compile that to get a working Vrui application.
> 不幸的是，Vrui还没有全面的开发者指南。作为先决条件，为Vrui开发需要对3D图形，特别是Vrui所基于的OpenGL图形库以及C++的了解。随着这些到位，Vrui开发商开始鼓励Vrui认为非常类似于glut,并从那里拿走。Vrui提供了一组模板或示例应用程序，显示了设置Vrui应用程序所需的（少量）样板代码，以及详细的注释，说明了将应用程序和3D渲染代码放在哪里。如果一个开发人员有一个应用程序的代码，通常足以剥离任何OpenGL设置和视点导航代码（OpenGL设置和导航由Vrui处理），将其余部分粘贴到最简单的Vrui模板中，并编译以获得工作的Vrui应用程序。

After these initial steps, developers are encouraged to use several existing Vrui applications to see how they look and feel, especially as they are run in different display environment types, and then peruse the HTML documentation that does exist. A good starting point is the "Library Overview" section, which lists the component library layers that make up the whole Vrui package, and in turn list all the header files and classes contained in those libraries. The interfaces of all those classes are defined in their respective header files, including detailed comments on all interface methods and functions.
> 在这些初步步骤之后，鼓励开发人员使用几个现有的Vrui应用程序来查看它们的外观和感觉，特别是在不同的显示环境类型下运行，然后仔细阅读存在的HTML文档。一个好的起点是“库概述”部分，其中列出了构成整个Vrui包的组件库层，然后列出了这些库中包含的所有头文件和类。所有这些类的接口都在其各自的头文件中定义，包括对所有接口方法和功能的详细注释。

Another important starting point is the Vrui kernel API in include/Vrui/Vrui.h, which declares all core functions applications use to communicate with Vrui. Due to Vrui's microkernel architecture, most of its functionality is not provided by the kernel itself, but by delegate classes such as Vrui::VRWindow, or by external so-called managers such as the input device manager. References to these managers are retrieved through the kernel API, but afterwards applications communicate directly with those managers. The bottom line is that all of Vrui's functionality is accessed through the kernel API, either directly or indirectly via managers or delegate classes.
> 另外一个重要的起点是包含/Vrui/Vrui.h中的Vrui内核API，它声明所有核心功能应用程序用于与Vrui进行通信。由于Vrui的微内核架构，其大部分功能不是由内核本身提供的，而是通过委托类（如Vrui::VRWindow）或外部所谓的管理器（如输入设备管理器）提供。通过内核API检索对这些管理器的引用，然后应用程序与这些管理器直接通信。底线是通过内核API直接或间接通过管理员或委托类访问Vrui的所有功能。

While the low-level documentation of classes and interfaces provided by the source code comments are reportedly rather good, they don't tell in which scenarios a developer might use a certain class. This will hopefully be addressed by higher-level development guides in the near future. In the meantime, another important resource is the Development Rules document, which lists common pitfalls stemming from Vrui's difference in philosophy to other VR or 3D graphics toolkits, and its focus on portability and usability. For example, the way Vrui applications are supposed to handle user input is Vrui's one feature most distinct from other toolkits.
> 虽然源代码注释提供的类和接口的低级文档据说相当不错，但他们并不知道开发人员可能在哪种情况下使用某个类。这将有望在不久的将来得到更高层次的发展指导。与此同时，另一个重要的资源是“发展规则”文件，其中列出了由Vrui在其他VR或3D图形工具箱中的差异导致的常见缺陷，并将重点放在可移植性和可用性上。例如，Vrui应用程序应该处理用户输入的方式是Vrui与其他工具包最相似的功能。

## 14. How do I set up a new Vrui-based project? --- 如何设置一个新的基于Vrui的项目

The easiest way to set up a new Vrui-based project is to use Vrui's internal build system. This build system, which is used by Vrui itself and all "official" Vrui extension packages and applications, might seem somewhat daunting at first glance. On the upside, it makes building libraries and applications hassle-free and fast, and is very extensible.
> 建立新的基于Vrui的项目的最简单的方法是使用Vrui的内部构建系统。Vrui本身使用的这个构建系统和所有“官方”Vrui扩展程序包和应用程序可能看起来有些令人生畏。在上面，它使建筑图书馆和应用程序轻松快捷，并且是非常可扩展的。

Let's say you want to create a new project called "Foo," containing an executable called "Foo" created by compiling and linking together source files Bar.cpp and Foo.cpp.
> 假设您要创建一个名为“Foo”的新项目，其中包含通过编译和链接源文件Bar.cpp和Foo.cpp而创建的可执行文件“Foo”。

First, create a new directory called "Foo" somewhere, and copy Vrui's template makefile into it:
> 首先，在某个地方创建一个名为“Foo”的新目录，并将Vrui的模板makefile复制到其中：

```
$ mkdir Foo
$ cd Foo
$ cp <Vrui share dir>/make/makefile.
```

where <Vrui share dir> is the share directory underneath Vrui's installation directory, such as ~/Vrui-4.2/share or /usr/share/Vrui-4.2 or /usr/local/share/Vrui-4.2. Then edit the makefile as follows:
> Vrui安装目录下的共享目录在哪里，例如〜/Vrui-4.2/share或/usr/share/Vrui-4.2或/usr/local/share/Vrui-4.2。然后编辑makefile如下：

After the line PACKAGES =, add the name of the main Vrui package, MYVRUI:
> 在PACKAGES行之后，添加主Vrui包的名称，MYVRUI：

```
PACKAGES = MYVRUI 
```

This will tell the build system that all executables in this project will use the Vrui package. After the line ALL =, add the full path to all executables of this project, in this example `$`(EXEDIR)/Foo:
> 这将告诉构建系统，该项目中的所有可执行文件都将使用Vrui软件包。在ALL =之后，添加该项目的所有可执行文件的完整路径，在此示例中为`$`（EXEDIR）/ Foo：

```
ALL = $(EXEDIR)/Foo
```

`$`(EXEDIR) is a pre-defined variable pointing to the destination directory for executables, i.e., ./bin. At the end of the makefile, under the heading "Specify build rules for executables," add the dependencies for the Foo executable: `$`(EXEDIR)/Foo: `$`(OBJDIR)/Bar.o `$`(OBJDIR)/Foo.o
> `$`（EXEDIR）是指向可执行文件的目标目录的预定义变量，即./bin。在makefile的末尾，在“指定可执行文件的构建规则”标题下，添加Foo可执行文件的依赖项：`$`（EXEDIR）/ Foo：`$`（OBJDIR）/Bar.o `$`（OBJDIR）/Foo.o

`$`(OBJDIR) is a pre-defined variable pointing to the destination directory for compiled object files. The root object file directory is ./o, but it contains an entire subdirectory hierarchy to keep object files for different compiler options separate, such as debug and non-debug versions.
> `$`（OBJDIR）是一个预定义的变量，指向编译对象文件的目标目录。根对象文件目录是./o，但是它包含一个完整的子目录层次结构，以便将不同编译器选项的对象文件分开，例如调试和非调试版本。

For executables with many object files, the above is somewhat inconvenient. A better way is to create a list of names of source files needed for an executable, and let make figure out the object file names automatically:
> 对于具有多个目标文件的可执行文件，上述有些不方便。一个更好的方法是创建可执行文件所需的源文件的名称列表，并自动找出对象文件名：

```
FOO_SOURCES = Bar.cpp Foo.cpp
$(EXEDIR)/Foo: $(FOO_SOURCES:%.cpp=$(OBJDIR)/%.o)
```

The code after `$`(EXEDIR)/Foo is a function that changes all names in FOO_SOURCES ending in .cpp into their appropriate object file names in the `$`(OBJDIR) hierarchy. As an aside, Vrui's build system can deal with source files in multiple subdirectories without any issues. Just use a project directory-relative path name for each source or object file. A source file Baz/Baz.cpp will turn into an object file `$`(OBJDIR)/Baz/Baz.o. Create source files Bar.cpp and Foo.cpp and enter your application code.
> `$`（EXEDIR）/ Foo之后的代码是将以.cpp结尾的FOO_SOURCES中的所有名称更改为`$`（OBJDIR）层次结构中适当的对象文件名的函数。除此之外，Vrui的构建系统可以处理多个子目录中的源文件而没有任何问题。只需使用每个源或目标文件的项目目录相对路径名。一个源文件Baz/Baz.cpp将变成一个对象文件`$`（OBJDIR）/Baz/Baz.o。创建源文件Bar.cpp和Foo.cpp并输入应用程序代码。

To build the project, simply type
> 要构建项目，只需键入

```
$ make
```

This will recompile all changed source files, and relink all executables. Vrui's build system automatically takes care of additional dependencies such as included header files. There is no need to explicitly enter source dependencies into the makefile. In projects with multiple executables, make will build all executables by default. To build only a single one, list its path name on make's command line:
> 这将重新编译所有更改的源文件，并重新链接所有可执行文件。Vrui的构建系统会自动处理附加依赖关系，如头文件。没有必要在makefile中明确输入源依赖关系。在具有多个可执行文件的项目中，make将默认构建所有可执行文件。要构建只有一个，请在make的命令行中列出其路径名：

```
$ make bin/Foo
```

Just running make Foo will not do the right thing. To fix that, add the following lines after the build rule for `$`(EXEDIR)/Foo in the makefile:
> 只是运行make Foo不会做正确的事情。要修复它，在makefile中的$（EXEDIR）/Foo的构建规则之后添加以下行：

```
.PHONY: Foo
Foo: $(EXEDIR)/Foo
```

After that addition, make Foo will work as expected. For all this work, you'll get these major benefits: you will never have to worry about source file dependencies, you will probably never have to use make clean, and make will never waste time rebuilding files that don't need rebuilding.
> 之后，让Foo将按预期工作。对于所有这些工作，您将获得以下主要优点：您永远不必担心源文件依赖关系，您可能永远不必使用make clean，并且永远不会浪费时间重建不需要重建的文件。

### But that's too complicated! I just want to compile one source file! --- 但这太复杂了！我只想编译一个源文件！

Oh, OK. In that case, go into the directory containing your source file, say Foo.cpp. Copy Vrui's template makefile into that same directory:
> 哦好的。在这种情况下，进入包含源文件的目录，说Foo.cpp。将Vrui的模板makefile复制到同一目录中：

```
$ cp <Vrui share dir>/make/makefile.
```

where <Vrui share dir> is the share directory underneath Vrui's installation directory, such as ~/Vrui-4.2/share or /usr/share/Vrui-4.2 or /usr/local/share/Vrui-4.2.
> Vrui安装目录下的共享目录在哪里，例如〜/Vrui-4.2/share或/usr/share/Vrui-4.2或/usr/local/share/Vrui-4.2。

Then compile Foo.cpp into ./bin/Foo and execute it:
> 然后将Foo.cpp编译成./bin/Foo并执行它：

```
$ make PACKAGES=MYVRUI Foo
$ ./bin/Foo
```

## 15. How do I use third-party libraries with Vrui's build system? --- 如何在Vrui的构建系统中使用第三方库？

Vrui's internal build system uses the notion of "packages" to set up and use internal or external software libraries. A package defines compiler and linker flags that are needed to compile source files and link executables against a given library. The list of external packages known to Vrui can be found in the make/Packages.System file in Vrui's share directory, and Vrui's own component libraries can be found in make/Packages.Vrui in the same directory.
> Vrui的内部构建系统使用“包”的概念来设置和使用内部或外部软件库。一个包定义了编译器和链接器标志，它们是根据给定的库编译源文件和链接可执行文件所需要的。Vrui已知的外部程序包列表可以在Vrui共享目录中的make/Packages.System文件中找到，Vrui自己的组件库可以在同一目录中的make/Packages.Vrui中找到。

Creating a new package to use some external library in a project based on Vrui's build system is straightforward. As an example, we will use a locally-built version of the ImageMagick library, installed underneath /usr/local/Magick6. First, we need to pick some unique name for the resulting package, say IMAGEMAGICK_LOCAL. We will then add a section for that package somewhere in the makefile of the project using it:
> 在基于Vrui构建系统的项目中创建一个新的包来使用一些外部库是很简单的。例如，我们将使用安装在/usr/local/Magick6下的ImageMagick库的本地构建版本。首先，我们需要为结果包选择一些唯一的名称，比如说IMAGEMAGICK_LOCAL。然后，我们将使用它在项目的makefile中的某个地方添加该包的部分：

```
IMAGEMAGICK_LOCAL_BASEDIR =
IMAGEMAGICK_LOCAL_DEPENDS =
IMAGEMAGICK_LOCAL_INCLUDE =
IMAGEMAGICK_LOCAL_CFLAGS =
IMAGEMAGICK_LOCAL_LIBDIR =
IMAGEMAGICK_LOCAL_LIBS =
IMAGEMAGICK_LOCAL_LINKLIBFLAGS =
```

_BASEDIR is an optional redirect for the root directory containing the package's include and library files. This is usually /usr or /usr/local. _DEPENDS is a list of other packages on which the package depends. For example, the GLU (OpenGL Utility) package listed in Vrui's Packages.System file depends on the GL (OpenGL) package. _INCLUDE is the list of directories containing the package's header files, including the -I prefix before each directory name. What directory names to use depends on common usage. For example, OpenGL's header files are in /usr/include/GL, but most source code includes them via #include <GL/gl.h> etc. As a result, GL_INCLUDE would be set to -I/usr/include. _CFLAGS is a list of additional compiler flags needed to compile source files against the package. _LIBDIR is the list of directories containing the package's library files, including the -L prefix before each directory name. _LIBS is the list of the package's library files, including the -l prefix before each abbreviated library name. For example, the OpenGL library would be listed as -lGL. _LINKLIBFLAGS is a list of additional linker flags needed to link against the package's libraries. Common flags are runtime search paths to find a library file in a non-standard location without having to use the LD_LIBRARY_PATH environment variable or other workarounds.
> _BASEDIR是包含包的包和库文件的根目录的可选重定向。这通常是/usr或/usr/local。_DEPENDS是包所依赖的其他包的列表。例如，Vrui的Packages.System文件中列出的GLU（OpenGL Utility）软件包取决于GL（OpenGL）软件包。_INCLUDE是包含包头文件的目录列表，包括每个目录名前的-I前缀。要使用的目录名称取决于常用的用法。例如，OpenGL的头文件位于/usr/include/GL中，但大多数源代码都包含在其中通过#include.因此，GL_INCLUDE将被设置为-I /usr/include。_CFLAGS是根据程序包编译源文件所需的附加编译器标志的列表。 _LIBDIR是包含包的库文件的目录的列表，包括每个目录名前的-L前缀。_LIBS是包的库文件的列表，包括每个缩写库名之前的-l前缀。例如，OpenGL库将被列为-lGL。_LINKLIBFLAGS是链接到软件包库所需的附加链接器标志的列表。公共标志是运行时搜索路径，用于在非标准位置查找库文件，而不必使用LD_LIBRARY_PATH环境变量或其他解决方法。

This is the resulting package section for our example non-standard ImageMagick package:
> 这是我们的示例非标准ImageMagick包的结果包部分：

```
IMAGEMAGICK_LOCAL_BASEDIR = /usr/local/Magick
IMAGEMAGICK_LOCAL_DEPENDS =
IMAGEMAGICK_LOCAL_INCLUDE = -I$(IMAGEMAGICK_LOCAL_BASEDIR)/include/ImageMagick-
IMAGEMAGICK_LOCAL_CFLAGS = -fopenmp -DMAGICKCORE_HDRI_ENABLE=0 -DMAGICKCORE_QUANTUM_DEPTH=
IMAGEMAGICK_LOCAL_LIBDIR = -L$(IMAGEMAGICK_LOCAL_BASEDIR)/lib
IMAGEMAGICK_LOCAL_LIBS = -lMagick++-6.Q
IMAGEMAGICK_LOCAL_LINKLIBFLAGS = -Wl,-rpath $(IMAGEMAGICK_LOCAL_BASEDIR)/lib
```

The _LINKLIBFLAGS variable is only necessary because the ImageMagick library is in a non-standard place where the dynamic linker would otherwise not find it.
> _LINKLIBFLAGS变量只是必需的，因为ImageMagick库位于动态链接器否则找不到的非标准位置。

By convergent evolution, Vrui's package structure is very similar to that of the pkg-config utility. Another way to populate the package section for packages supported by pkg-config is the following:
> 通过融合演进，Vrui的包结构与pkg-config实用程序非常相似。 pkg-config支持的软件包的另一种方法是：

```
IMAGEMAGICK_LOCAL_BASEDIR =
IMAGEMAGICK_LOCAL_DEPENDS =
IMAGEMAGICK_LOCAL_INCLUDE = $(pkg-config --cflags-only-I)
IMAGEMAGICK_LOCAL_CFLAGS = $(pkg-config --cflags-only-other)
IMAGEMAGICK_LOCAL_LIBDIR = $(pkg-config --libs-only-L)
IMAGEMAGICK_LOCAL_LIBS = $(pkg-config --libs-only-l)
IMAGEMAGICK_LOCAL_LINKLIBFLAGS = $(pkg-config --libs-only-other)
```

To use a package in a project, simply append its name to the PACKAGE variable in the project's makefile. If the package is only required by some executables in a project, its name can be listed in a specific executable's build section. For example:
> 要在项目中使用包，只需将其名称附加到项目的makefile中的PACKAGE变量中即可。如果程序包仅需要项目中的某些可执行文件，则其名称可以列在特定可执行文件的构建部分中。例如：

```
$(EXEDIR)/Foo: PACKAGES += IMAGEMAGICK_LOCAL
$(EXEDIR)/Foo: $(OBJDIR)/Foo.o
```

## 16. How do I receive input from the keyboard? --- 如何从键盘接收输入？

You don't. No, really. Unlike glut et al., Vrui aims to support environment-independent applications, and non-desktop environments do not have keyboards. As a result, Vrui applications receive input in a very different manner than, say, glut applications. Instead of directly receiving events from the keyboard (or the mouse) via dedicated callbacks, Vrui applications receive events from so-called tools, which in turn receive events from actual input devices.
> 你没有不完全是。与glut等不同，Vrui旨在支持与环境无关的应用程序，非桌面环境不具备键盘。因此，Vrui应用程序以与例如glut应用程序非常不同的方式接收输入。Vrui应用程序不是通过专用回调直接从键盘（或鼠标）接收事件，而是从所谓的工具接收事件，从而从实际的输入设备接收事件。

To use a concrete example, assume an application that wants to do something when the user presses a specific key. In glut et al., this would be handled by registering a keyboard event callback, which will in turn check the identity of the just-pressed key when called, and invoke the proper application behavior if the desired key is pressed. In Vrui, application behaviors are implemented as tools. If an application has some behavior X that is to be invoked when some key is pressed, the developer would create a corresponding tool class X and hand it to Vrui's tool manager during start-up. After that, a user can dynamically bind a tool object of class X to any button she desires; after that, if she presses that button, the bound tool object will be called, and can in turn invoke behavior X.
> 要使用一个具体的例子，假设一个应用程序想要在用户按下一个特定的键时做某事。在glut等人中，这将通过注册一个键盘事件回调来处理，该回调将依次在被调用时检查刚按键的标识，如果按下所需的键，则调用正确的应用程序行为。在Vrui中，应用程序行为被实现为工具。如果某个应用程序有某些行为X在某些按键被调用时被调用，则开发人员将创建一个相应的工具类X，并在启动过程中将其交给Vrui的工具管理器。之后，用户可以动态地将类X的工具对象绑定到任何她想要的按钮;之后，如果她按下该按钮，绑定的工具对象将被调用，并且可以依次调用行为X.

While this approach sounds more complicated, it has important benefits. For one, it works in non-desktop environments. Even if there is no keyboard, there will still be some input device that has some ways to signal that a user wants to initiate an event (which could be an actual button, or a gesture, or a voice command, etc.), and Vrui's dynamic tool binding mechanism allows the user to bind a tool of class X to any such event source, without the application developer having to code any support for that. Even if constrained to the desktop, this allows users to map application behaviors to any keys they desire – in other words, the keyboard remapping functionality common in PC games comes for free in Vrui.
> 虽然这种方法听起来更复杂，但它具有重要的益处。一方面，它在非桌面环境中工作。即使没有键盘，仍然会有一些输入设备有一些方式来通知用户想要启动一个事件（可能是一个实际的按钮，一个手势或一个语音命令等），以及Vrui的动态工具绑定机制允许用户将类X的工具绑定到任何此类事件源，而不需要开发人员编写任何对此的支持。即使受限于桌面，这也允许用户将应用程序行为映射到他们期望的任何键-换句话说，PC游戏中常见的键盘重新映射功能在Vrui中自由。

Additionally, in actual code, setting up a tool is no more complicated than writing a callback. For simple cases like the one above, where a key press invokes some behavior, Vrui offers a convenience shortcut that creates a tool class, passes it to Vrui's tool manager, handles dynamic binding, and invokes an arbitrary application callback when an event happens, in a single line of code (see VruiEventToolDemo.cpp in the ExamplePrograms subdirectory for several concrete examples). Only in more complex cases, such as when behaviors require their own internal states, will a developer have to implement an actual tool class, which would be essentially the same amount of work as in other toolkits.
> 另外，在实际的代码中，设置工具并不比编写回调更复杂。对于类似上述的简单情况，按键会调用某些行为，Vrui提供了一个便捷的快捷方式，可以创建一个工具类，将其传递给Vrui的工具管理器，处理动态绑定，并在事件发生时调用任意的应用程序回调一行代码（参见ExamplePrograms子目录中的VruiEventToolDemo.cpp几个具体示例）。只有在更复杂的情况下，例如当行为需要自己的内部状态时，开发人员必须实现一个实际的工具类，这与其他工具包基本上是相同的工作量。

### But I don't want events, I want the user to enter some text, such as a file name or label! --- 但我不想要事件，我想让用户输入一些文本，如文件名或标签！

In that case, still don't query the keyboard. Create a GLMotif dialog box with an editable text field and let Vrui worry about where that text comes from (from a real keyboard, from a virtual keyboard, from a gesture-based text entry method, from speech recognition, ...).
> 在这种情况下，仍然不查询键盘。创建具有可编辑文本字段的GLMotif对话框，并让Vrui担心文本来自何处（来自真实键盘，虚拟键盘，基于手势的文本输入法，语音识别，...）。

## 17. How do I query the display resolution in points per inch etc.? --- 如何查询显示分辨率每英寸等等？

You don't. Due to Vrui's portability, an application might not have a display, might have multiple displays with different resolutions, or might have an immersive display, where display resolution is not even an applicable concept. In Vrui, only very specific applications, such as calibration utilities, would need to know anything about display resolution. In almost all cases, the actual functionality for which a developer would like to know the display resolution can be achieved in a more direct way. One common example is scaling: a developer wants to be able to display 3D data at fixed scales (1:1, 1:100) such that images on the display can be measured. This is supported directly in Vrui: if an application tells Vrui which unit of measurement is used by an application, it will provide an interactive scale bar that shows the exact scale factor from application to real world, and a user interface to adjust the scale factor to common fixed values, such as 1:1.
> 你没有由于Vrui的便携性，应用程序可能没有显示器，可能具有不同分辨率的多个显示器，或者可能具有沉浸式显示器，其中显示分辨率甚至不是适用的概念。在Vrui中，只有非常具体的应用程序（如校准实用程序）才需要了解显示分辨率。在几乎所有情况下，开发人员想要了解显示分辨率的实际功能可以以更直接的方式实现。一个常见的例子是缩放：开发人员希望能够以固定比例（1：1,1：100）显示3D数据，以便可以测量显示器上的图像。这在Vrui中直接支持：如果应用程序告诉Vrui应用程序使用哪个测量单位，它将提供一个交互式比例栏，显示从应用程序到现实世界的确切比例因子，以及用于调整比例因子的用户界面到普通的固定值，如1：1。

Another common example is an application needing to know how big to make a display, like a text label, or how big to make the influence zone around an interaction event, based on display resolution. In Vrui, these parameters are configured by the system integrator/administrator appropriately for a concrete environment, and can be queried directly via the Vrui kernel API.
> 另一个常见的例子是基于显示分辨率，需要知道多大的显示（如文本标签）或多大的影响区域围绕交互事件做出多大的应用。在Vrui中，这些参数由系统集成商/管理员为具体环境进行适当配置，可以通过Vrui内核API直接查询。

In other words, Vrui applications should always render in 3D application space in any unit of measurement they want, advertise that unit to Vrui's coordinate manager, and let Vrui take care of the rest. Any questions about appropriate display sizes, interaction fuzz values, optimal font sizes, etc. should be answered by querying the Vrui kernel API whenever needed.
> 换句话说，Vrui应用程序应该始终以任何所需的测量单位在3D应用程序空间中呈现，向Vrui的坐标经理宣传该单元，并且让Vrui处理其余部分。有关适当显示尺寸，交互模糊值，最佳字体大小等的任何问题都应通过在需要时查询Vrui内核API来回答。

## 18. How do I manage server-side OpenGL state, such as texture objects? --- 如何管理服务器端的OpenGL状态，如纹理对象？

In most other 3D or VR toolkits, application-side state, such as a 3D mesh representation, and server-side OpenGL state, such as texture or buffer objects, are mixed freely. In an application object, a pointer to a mesh structure might directly be followed by a handle to a vertex buffer object holding the mesh's vertices. This, however, prevents portability to environments where one application has to deal with multiple OpenGL contexts, where a single mesh structure might be represented by several vertex buffers, each for a different OpenGL context, with a different handle. Vrui solves the problem by strictly separating application-side state from server-side OpenGL state, and associating multiple copies of an application's server-side state with a single copy of application-side state. Context and state management, and necessary state replication, are completely hidden by the Vrui API. Applications always only see a single copy of server-side state, and only at the time this state is needed for rendering. This process is described in detail in the GLContextData document.
> 在大多数其他3D或VR工具包中，应用程序侧状态（如3D网格表示）和服务器端OpenGL状态（如纹理或缓冲区对象）都可以自由混合。在应用程序对象中，指向网格结构的指针可以直接跟着保持网格顶点的顶点缓冲对象的句柄。然而，这防止了对一个应用程序必须处理多个OpenGL上下文的环境的可移植性，其中单个网格结构可能由多个顶点缓冲区表示，每个顶点缓冲区用于不同的OpenGL上下文，具有不同的句柄。Vrui通过将应用程序侧状态与服务器端OpenGL状态进行严格分离，并将应用程序的服务器端状态的多个副本与应用程序侧状态的单个副本相关联，解决了此问题。Vrui API完全隐藏上下文和状态管理以及必要的状态复制。应用程序始终只能看到服务器端状态的单一副本，并且只在渲染需要此状态时。该过程在GLContextData文档中有详细描述。

## 19. Can I use modern OpenGL in Vrui applications? --- 我可以在Vrui应用程序中使用现代OpenGL吗？

At first glance, it might seem as if Vrui is a ca. 1998 old-school OpenGL 1.0 toolkit. And while Vrui itself does still contain enough legacy code to require running under an OpenGL compatibility profile (removing that is ongoing work in progress), Vrui applications are free to use modern features (and even many components of Vrui do so), as long as their functionality is either directly provided by the underlying OpenGL library or available via OpenGL extensions. There are two main approaches to use modern features:
> 乍看起来，似乎Vrui是一个ca. 1998年老式的OpenGL 1.0工具包。虽然Vrui本身仍然包含足够的遗留代码，要求在OpenGL兼容性配置文件下运行（删除正在进行的工作），Vrui应用程序可以自由使用现代功能（甚至Vrui的许多组件），只要它们的功能由底层的OpenGL库直接提供，也可以通过OpenGL扩展。使用现代功能有两种主要方法：

```
1. Simply call a modern OpenGL entry point and hope for the best.
2. Use Vrui's internal OpenGL extension wrangling mechanism.
```

The first approach might work on a developer's local machine, where the OpenGL driver automatically exports all OpenGL entry points, but it won't be portable. The second approach requires a small amount of additional work, but it much safer and more flexible. Here is how it is used:
> 第一种方法可能适用于开发人员的本地机器，OpenGL驱动程序会自动导出所有OpenGL入口点，但不会移植。第二种方法需要少量额外的工作，但它更安全和更灵活。这是如何使用的：

Let us assume that some module of your project, say a single class, wants to use some OpenGL extension, say GL_ARB_vertex_buffer_object to store some geometry in the GPU's own memory. In Vrui, each supported OpenGL extension is represented as an individual class, with header files in the GL/Extensions subdirectory.
> 让我们假设你的项目的一些模块，就像一个单一的类，想要使用一些OpenGL扩展，说GL_ARB_vertex_buffer_object来存储一些几何在GPU自己的内存中。在Vrui中，每个支持的OpenGL扩展表示为单独的类，头文件在GL / Extensions子目录中。

To use an extension, the source file defining the class first has to include the appropriate header file:
> 要使用扩展名，首先定义类的源文件必须包含相应的头文件：

```
#include <GL/Extensions/GLARBVertexBufferObject.h>
```

(note the conversion from separate_words to CamelCase). Including this file makes all #define constants and function prototypes provided by the extension available to the source file's code. All declarations will use the extension's name space suffix (ARB, EXT, etc.), such as GL_ARRAY_BUFFER_ARB or glGenBuffersARB.
> （注意从separate_words转换为CamelCase）。包括这个文件使所有 #define 常量和由源文件的代码可用的扩展提供的函数原型。所有声明将使用扩展名的名称空格后缀（ARB，EXT等），如GL_ARRAY_BUFFER_ARB或glGenBuffersARB。

After that, the code has to initialize the extension before any of its functions can be used by calling the extension class's static initExtension method:
> 之后，代码必须初始化扩展，然后通过调用扩展类的静态initExtension方法可以使用它的任何函数。

```
GLARBVertexBufferObject::initExtension();
```

This is typically done once per lifetime of each created object, typically in the class's OpenGL context initialization method (see initContext in GLContextData). Vrui's extension manager is designed such that multiple redundant calls to initExtension are safe and fast; developers do not have to worry about managing extension initialization and can leave it completely decentralized, improving code modularity.
> 这通常在每个创建的对象的生命周期中完成一次，通常在类的OpenGL上下文初始化方法中（请参阅GLContextData中的initContext）。Vrui的扩展管理器的设计使得对initExtension的多个冗余调用是安全和快速的;开发人员不必担心扩展初始化的管理，并且可以使其完全分散，从而提高代码模块性。

The initExtension method throws a run-time error exception if an extension is not supported by the current OpenGL context. For more flexibility, especially when using new and rarely-supported extensions, application code can query the availability of an extension via the static isSupported method:
> 如果当前OpenGL上下文不支持扩展名，initExtension方法会引发运行时错误异常。为了更好的灵活性，特别是在使用新的和很少支持的扩展时，应用程序代码可以通过静态的isSupported方法查询扩展的可用性：

```
if(GLARBVertexBufferObject::isSupported())
GLARBVertexBufferObject::initExtension();
else
{
/* Set up a fallback rendering path etc. */
}
```

This pattern only makes sense if there is a reasonable alternative to using the requested extension, such as a fallback rendering path. If a module cannot meaningfully function without a certain extension, it is probably best to let initExtension throw an exception and let the module's client code decide what to do.
> 如果存在使用所请求的扩展名的合理替代方法（如后备渲染路径），则此模式才有意义。如果一个模块无法在没有一定扩展的情况下有意义地运行，那么最好让initExtension抛出一个异常，并让模块的客户端代码决定要做什么。

An important fact about OpenGL extensions is that they are context-dependent state. Due to Vrui's multi-machine, multi-pipe rendering architecture, it is entirely possible (albeit rare) that a certain extension is supported in one OpenGL context used by the application, and not in another. This is normally handled transparently by Vrui's GLContextData mechanism (see GLContextData), but an important effect is that isSupported, initExtension, and all extension functions can only be called when there is a current OpenGL context, i.e., during a class's initContext or rendering method, or when a class's method is called from inside another class's initContext method etc. The GLContextData document contains a more detailed treatment of application/OpenGL state separation and multi-context rendering.
> 关于OpenGL扩展的一个重要事实是它们是与上下文相关的状态。由于Vrui的多机多管渲染架构，应用程序所使用的一个OpenGL上下文中支持特定的扩展是完全可能的（尽管很少），而不是另一个扩展。这通常由Vrui的GLContextData机制（见GLContextData）透明地处理，但是一个重要的作用是，当有一个当前的OpenGL上下文，即在一个类的initContext或者渲染方法中，isSupported，initExtension和所有的扩展函数才能被调用，或者当从另一个类的initContext方法等内部调用类的方法时.GLContextData文档包含对应用程序/OpenGL状态分离和多上下文呈现的更详细的处理。

## 20. What if Vrui's extension manager does not support an OpenGL extension that I need? --- 如果Vrui的扩展管理器不支持我需要的OpenGL扩展，该怎么办？

Vrui strives to support all OpenGL extensions in the long term, but as of right now, due to limited developer time, only those OpenGL extensions that are required by Vrui itself or by "official" Vrui applications, and those that were requested by external Vrui developers, are available. Find the full list of extensions supported by your version of Vrui inside the include/GL/Extensions directory. If an extension you require is not there, you have two options:
> Vrui致力于长期支持所有OpenGL扩展，但由于开发人员时间有限，Vrui本身或“官方”Vrui应用程序所需的OpenGL扩展以及外部Vrui请求的OpenGL扩展开发人员可用。查找您的Vrui版本支持的完整扩展列表，包括在/GL/Extensions目录中。如果您需要的扩展程序不在，则有两个选项：

```
1. Write an extension class using an existing extension class as a template, and ideally submit it back to the Vrui repository.
2. Ask someone to do it for you (yes, please do!).
```
> 1.使用现有扩展类作为模板编写扩展类，并将其理想地提交回Vrui存储库。
> 2.请别人为你做（是的，请做！）。

## 21. Can I use GLEW in Vrui applications? --- 我可以在Vrui应用程序中使用GLEW吗？

Yes, but it is tricky, finicky, and unsupported. GLEW (the OpenGL Extension Wrangler) is an external library with the same goals as Vrui's built-in extension manager (see Can I use modern OpenGL in Vrui applications?). Unfortunately, GLEW's design clashes with Vrui's multi-machine/multi-context rendering architecture. While there is a multi-context capable version of GLEW, its use in an application must be centralized, and requires inclusion of GLEW's header file(s) in every source file of a Vrui application using GLEW, before any OpenGL headers are included. While some external Vrui developers have been successful in using GLEW in their projects, it requires careful management and breaks easily. Most importantly, GLEW cannot be used in a library, unless users of the library add GLEW handling throughout their applications. Vrui's built-in extension manager is much more compatible, but see What if Vrui's extension manager does not support an OpenGL extension that I need?.
> 是的，但它是棘手的，棘手的，不支持的。GLEW（OpenGL Extension Wrangler）是一个外部库，与Vrui的内置扩展管理器具有相同的目标（请参阅我可以在Vrui应用程序中使用现代OpenGL？）。不幸的是，GLEW的设计与Vrui的多机/多上下文渲染架构相冲突。虽然GLEW具有多上下文能力的版本，但它在应用程序中的使用必须集中，并且在包括任何OpenGL头部之前，需要使用GLEW在Vrui应用程序的每个源文件中包含GLEW的头文件。虽然一些外部Vrui开发人员已经在其项目中成功使用GLEW，但它需要仔细的管理和轻松打破。最重要的是，GLEW不能在库中使用，除非库中的用户在其应用程序中添加GLEW处理。Vrui的内置扩展管理器更加兼容，但是如果Vrui的扩展管理器不支持我需要的OpenGL扩展，该怎么办？
