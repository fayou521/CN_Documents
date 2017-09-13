# Vrui VR Toolkit

> # Vrui 虚拟现实工具集

Tags: Vrui


The task of a virtual reality (VR) development toolkit is to shield an application developer from the particular configuration of a VR environment, such that applications can be developed quickly and in a portable and scalable fashion. Three important parts of this overarching goal are encapsulation of the display environment, encapsulation of the distribution environment, and encapsulation of the input device environment. In more detail, these three partial goals are:

> 虚拟现实（VR）开发工具包的任务是保护应用程序开发人员不受VR环境的特殊配置的影响，从而使应用程序能够快速、灵活、可扩展地开发。这一总体目标的三个重要部分是：显示环境的封装、分布环境的封装和输入设备环境的封装。在更多的细节，这三个部分的目标是：

Display abstraction
A toolkit should provide OpenGL rendering contexts that are set up in such a fashion that rendering a model in user-specific coordinates will display that model on all rendering surfaces (monitors, screens,head-mounted displays) in correct head-tracked stereographic mode.
> 显示抽象
> 一个工具包应该提供OpenGL渲染的上下文，能够将用户指定的模型在所有渲染设备上（显示器，屏幕，头戴式设备），以正确的头部跟踪立体模式显示。

Distribution abstraction
As larger VR environments require more than one computer to operate, the detail aspects of distribution (number of computers, connection topology, etc.) should be hidden by the toolkit. In principle, there are at least three ways to distribute a rendering environment: "split first," where an application is replicated on all computers and synchronized by distributing input device and ancillary data; "split middle," where a shared data structure, e.g., a scene graph, is used to transmit data from one application computer to all rendering computers; and "split last," where the OpenGL API call stream is broadcast from one application computer to all rendering computers, e.g., using Chromium. A toolkit should also hide the difference between a distributed rendering environment running on a cluster of individual computers, and one running on a shared-memory multi-CPU computer with several independent graphics pipes.
> 分布抽象
>由于较大的VR环境需要多台计算机进行操作，所以工具包应该隐藏分发的细节方面（计算机数量，连接拓扑等）。原则上，至少有三种方式来分配渲染环境：“拆分第一”，其中应用程序在所有计算机上复制并通过分发输入设备和辅助数据进行同步;“分割中间”，其中共享数据结构，例如场景图，用于将数据从一个应用计算机发送到所有的呈现计算机;“最后拆分”，其中OpenGL API调用流从一个应用计算机广播到所有呈现计算机，例如使用Chromium。一个工具包还应该隐藏在单个计算机的集群上运行的分布式呈现环境和在具有多个独立图形管道的共享内存多CPU计算机上运行的分布式渲染环境之间的区别。

Input abstraction
There are a wide variety of different vendors, models and protocols to connect VR input devices such as space balls, space mice, 6-DOF trackers, wands, data gloves, etc., to the computers comprising a VR environment. A toolkit must hide these differences in hardware, and provide a uniform view of the set of connected input devices. Furthermore, a toolkit should provide mechanisms not only to hide the hardware details of the input device environment, but also the number and configuration of input devices. An application should be written without aiming for a particular input environment (such as "CAVE wand and head tracker" or "stylus, two pinch gloves, and head tracker"). Instead, the toolkit should provide a layer that allows an application to specify its input requirements at a higher level, and allows a user to map input devices to these requirements.
> 输入方式抽象
>将各种不同的供应商，型号和协议连接到包括VR环境的计算机的VR输入设备，例如空间球，空间鼠标，6自由度跟踪器，魔杖，数据手套等。一个工具包必须在硬件中隐藏这些差异，并提供一组连接的输入设备的统一视图。而且还包括输入设备的数量和配置。应该写一个应用程序，而不用针对特定的输入环境（例如“CAVE棒和头部跟踪器”或“手写笔，两个捏手套和头部跟踪器”）。另外，该工具包应该提供一个层，允许应用程序在更高级别指定其输入要求，并允许用户将输入设备映射到这些要求。

Most existing VR toolkits cover the first aspect well; some VR toolkits cover the second aspect by using any one of the listed distribution schemes, but no toolkit we found covers the third aspect. Although all toolkits have some kind of built-in input device driver to hide hardware details, none provide a higher-level semantic interface that allows to write an application once, and run it in VR environments with widely differing input environments. This implies that no toolkits provide an adequate way to run desktop applications using a regular mouse and keyboard; although many of them have simulators, these are merely awkward low-level debugging tools and are not useful for running and actually using a VR application on a desktop.
> 大多数现有的VR工具包很好地涵盖了第一方面;一些VR工具包通过使用列出的任何一种分发方案来覆盖第二方面，但是我们发现的工具包没有涵盖第三方面。虽然所有工具包都有一些内置的输入设备驱动程序来隐藏硬件细节，但是没有一个提供更高级别的语义接口，允许编写应用程序一次，并在具有不同输入环境的VR环境中运行。这意味着没有工具包提供使用常规鼠标和键盘运行桌面应用程序的足够方法;尽管其中许多都具有模拟器，但这些只是尴尬的低级调试工具，对于在桌面上运行和实际使用VR应用程序无效。

The Vrui VR toolkit aims to support fully scalable and portable applications that run on a range of VR environments starting from a laptop with a touchpad, over desktop environments with special input devices such as space balls, to full-blown immersive VR environments ranging from a single-screen workbench to a multi-screen tiled display wall or CAVE. Applications using the Vrui VR toolkit are written without a particular input environment in mind, and Vrui-enabled VR environments are configured to map the available input devices to application functions such that the application appears to be written natively for the environment it runs on. For example, a Vrui application running on the desktop should be as usable and intuitive as a 3D application written specifically for the desktop.
> Vrui VR工具包旨在支持完全可扩展和便携式的应用程序，从一系列VR环境开始，从笔记本电脑，触摸板，桌面环境以及特殊输入设备（如空间球）到完整的沉浸式VR环境，从单屏工作台到多屏幕平铺显示墙或CAVE。使用Vrui VR工具包的应用程序在没有特定输入环境的情况下进行编写，启用Vrui的VR环境配置为将可用的输入设备映射到应用程序功能，使得应用程序似乎是为其运行的环境而自己编写的。例如，在桌面上运行的Vrui应用程序应该与专门为桌面编写的3D应用程序一样可用和直观。

```
Figure 1: The same Vrui application, run in a 4-sided CAVE (left) and on a laptop (right).
```

## Project Goals

> ## 项目目标

The main project goals were to design and implement a VR development toolkit for scalable and portable applications providing the following abstractions and additional features:
> 主要项目的目标是为可扩展和便携式应用程序设计和实施VR开发工具包，提供以下抽象和附加功能：

* Display abstraction by setting up and maintaining OpenGL rendering contexts that can display head-tracked stereoscopic images on monitors, fixed single- and multi-screen projections systems, movable monitors or screens, head-mounted displays, or any combinations thereof.
> * 通过设置和维护OpenGL渲染上下文来显示抽象，它能够在显示器上展示头戴跟踪立体图像，固定的单屏幕和多屏幕投影系统，可移动显示器或屏幕，头戴式显示器或其任何组合。

* Distribution abstraction using a split-first distribution of the toolkit itself, with support for transparent multicast communication between an application computer and the rendering computers to allow applications to internally use a split-middle scheme, for example using an external scene graph API or application-specific communication protocols. On a shared-memory multi-CPU/multi-pipe visualization system, applications should automatically make use of parallelism and high-speed communications.
> * 分布抽象使用工具包本身的分裂优先分布，支持应用程序计算机和渲染计算机之间的透明组播通信，以允许应用程序在内部使用分裂中间方案，例如使用外部场景图API或应用程序 - 具体通信协议。在共享内存多CPU /多管道可视化系统中，应用程序应自动使用并行性和高速通信。

* Input abstraction by providing so-called "semantic events" which notify an application of user interaction, and a set of run-time selectable so-called "tools" that map the input devices of a VR environment to the semantic events of an application. Tools not only map from devices to events, but also encapsulate the user interface to evoke certain events. For example, different styles of navigation are implemented as different navigation tools, selectable by users at run-time, while an application only understands a generic "navigate" event.
> * 通过提供通知用户的应用程序的所谓“语义事件”来输入抽象交互，以及一组运行时可选的所谓“工具”，用于映射VR的输入设备环境到应用程序的语义事件。工具不仅从设备映射到事件，而且还封装了用户界面以唤起某些事件。例如，不同的导航方式被实现为不同的导航工具，在运行时可由用户选择，而应用程序只能理解通用的“导航”事件。

* Powerful operating system-independent foundation libraries for functionality commonly used in VR application development, such as distributed file I/O, 3D geometry abstractions, spatial data structures, OpenGL extension management, GLSL shader support, access to video and sound recording hardware, simple scene graphs, etc.
> * 强大的操作系统独立的基础库，用于VR中常用的功能应用程序开发，如分布式文件I / O，3D几何抽象，空间数据结构，OpenGL扩展管理，GLSL着色器支持，访问视频和录音硬件，简单的场景图等。

* A 3D user interface component library that allows application programmers to assemble 3D user interfaces using similar interactions as traditional 2D GUIs.
> * 3D用户界面组件库，允许应用程序员组装3D用户使用与传统2D GUI类似的交互的界面。

* A scalable design paradigm, where applications only need to reference features that they require, such that simple applications have simple (and short) source codes. A complete Vrui "Hello World" application, rendering a cube and a sphere with full viewpoint navigation, can be written in 34 lines of code (see VruiDemoSmall.cpp).
>* 一种可扩展的设计范例，其中应用程序只需要引用它们所需的功能，因此简单的应用程序具有简单（和简短的）源代码。一个完整的Vrui “HelloWorld”应用程序可以用34行代码（见VruiDemoSmall.cpp）编写一个立方体和具有完整视点导航的球体。

A more complete list of goals and the architecture / design features implementing them can be found in the Vrui Manifesto.
> Vrui宣言可以找到更完整的目标列表和实现它们的架构/设计功能。

## Project Status

> ## 项目状态

At this point, all building blocks of the Vrui VR toolkit are in place and functional, and specific supported and tested environments are:
> 在这一点上，Vrui VR工具包的所有构建块都已经到位，功能齐全，具体的支持和测试环境是：

* Laptop or desktop computers with mouse and keyboard, and optional additional devices such as joysticks, space balls, game pads, etc.
> * 笔记本电脑或带有鼠标和键盘的台式电脑，以及可选的附加设备，如操纵杆，太空球，游戏垫等

* Responsive Workbench environment with head tracking, stylus, and two pinch gloves. Immersadesk environment with head tracking and CAVE wand.
> * 响应式工作台环境，带头跟踪，手写笔和两个捏手套。Immersadesk环境与头跟踪和CAVE魔杖。

* Tiled 2 x 2 display wall with two pinch gloves run by 4-node cluster (no head tracking, anaglyphic stereo).
> * 平铺2 x 2显示墙，两个夹点手套由4节点集群运行（无头跟踪，浮雕立体声）。

* Upright display wall with optical tracking system and multiple custom-designed input devices.
> * 直立显示墙，配有光学跟踪系统和多种定制设计的输入设备。

* Tiled 3 x 2 display wall with head tracking and updated CAVE wand run by 7-node cluster.
> * 平铺3 x 2显示墙与头跟踪和更新CAVE棒由7节点集群运行。

* Four-sided CAVE with head tracking, updated CAVE wand, and pinch gloves run by 5-node cluster.
> * 四面CAVE带头跟踪，更新的CAVE棒和夹点手套由5节点集群运行。

* Four-sided CAVE with head tracking and updated CAVE wand run by SGI PRISM shared-memory multi-CPU/multi-pipe vizualization system.
> * 四面CAVE与头跟踪和更新的CAVE棒由SGI PRISM共享内存多CPU /多管虚拟化系统。

* Low-cost fully immersive VR environment consisting of a 3D TV and an optical tracking system, see Low-Cost VR.
> * 由3D电视和光学跟踪系统组成的低成本完全身临其境的VR环境，看低成本VR。

* Low-cost VR environment consisting of a 3D TV and a Razer Hydra desktop 6-DOF input device, see Low-Cost VR.
> * 低成本VR环境由3D电视和Razer Hydra桌面6自由度输入设备组成，看低成本VR。

* Head-mounted displays (eMagin Z800 3DVisor, Sony HMZ-T1) tracked by Intersense tracking system, with updated CAVE wand.
> * Intersense追踪系统追踪的头戴式显示器（eMagin Z800 3DVisor，Sony HMZ-T1）与更新的CAVE魔杖。

* Oculus Rift DK1 and DK2, using built-in head trackers, with a wide variety of input devices (mouse/keyboard, joysticks, spaceballs, Wiimote, desktop 6-DOF trackers such as Razer Hydra).
> * Oculus Rift DK1和DK2，使用内置的头部跟踪器，具有各种各样的输入设备（鼠标/键盘，操纵杆，太空球，Wiimote，桌面6自由度追踪器，如Razer Hydra）。

* HTC Vive
> * HTC Vive

Application-level streaming to layer shared graphics data structures in a split-middle architecture over Vrui's internal split-first distribution is finally working reliably. The first application using split-middle is ProtoShop. Instead of computing the inverse kinematics to manipulate proteins on all nodes of a cluster, the computation is only done on the cluster's head node, and the results are broadcast to the render nodes. This ensures cluster-wide synchronization in all cases, and improves responsiveness on clusters where the head node has multiple CPUs (or CPU cores), and the render nodes only have single CPUs.
> 通过Vrui的内部分裂首次分发，分层中间架构中的应用级流式传输到层共享图形数据结构终于可靠地工作。第一个使用分裂的应用程序是ProtoShop。不是计算逆运动学来处理集群的所有节点上的蛋白质，而是仅在集群的头节点上进行计算，并将结果广播到渲染节点。这确保了在所有情况下的群集范围同步，并提高了头节点具有多个CPU（或CPU内核）的群集的响应性，并且渲染节点仅具有单个CPU。

As of 04/08/2009, the Vrui VR toolkit has been released publicly under the GNU General Public License version 2, and the most recent and several older versions are available for download from the download page.
> 2009年8月4日，Vrui VR工具包已在GNU通用公共许可证版本2下公开发布，最新版本和多个旧版本可从下载页面下载。

As of 11/25/2010, the Vrui VR toolkit has moved to version 2.0.

As of 06/07/2011, the Vrui VR toolkit has moved to version 2.1.

As of 12/16/2012, the Vrui VR toolkit has moved to version 2.6.

As of 08/12/2013, the Vrui VR toolkit has moved to version 3.0.

As of 12/14/2013, the Vrui VR toolkit has moved to version 3.1.

As of 10/13/2016, the Vrui VR toolkit has moved to version 4.2.
