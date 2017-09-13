# Library Overview --- 各个库概况

Tags: Vrui

[TOC]

## Misc -- Miscellaneous Library --- 杂项

Contains assorted abstraction classes such as error handling, file I/O, containers, configuration file management, etc. Used extensively throughout the higher-level libraries.
> 包含各种抽象类，如错误处理，文件I / O，容器，配置文件管理等。广泛应用于更高级的库。

## Threads -- Thread Support Library ---  线程库

Contains classes providing a C++ wrapper around the pthreads library.
> 包含围绕pthreads库提供C包装的类。

## USB -- USB Support Library --- USB支持库

Contains abstraction classes for access to USB devices using the libusb library version 1.0. The classes mostly provide exception safety via the RAII paradigm, and a simplified interface to raw libusb functions.
> 包含使用libusb库版本1.0访问USB设备的抽象类。这些类大多通过RAII范例提供异常安全性，以及对原始libusb功能的简化接口。

## RawHID -- Raw HID Support Library --- 原始HID支持库

Contains abstraction classes for raw access to human interface devices (HID). The classes mostly provide exception safety via the RAII paradigm, and a simplified interface to raw HID kernel interfaces.
> 包含用于原始访问人机界面设备（HID）的抽象类。这些类大多通过RAII范例提供异常安全性，并且简化了原始HID内核接口的界面。

## IO -- I/O Support Library --- I/O支持库

Contains abstraction classes for file I/O. Used extensively throughout the higher-level libraries.
> 包含文件I / O的抽象类。广泛应用于更高层次的程序库。

## Plugins -- Plugin Handling Library --- 插件处理库

Contains classes supporting dynamic loading of C++ classes from shared libraries or bundles.
> 包含支持从共享库或软件包动态加载C类的类。

## Realtime -- Real-time Alarm Library --- 实时报警库

Contains a class to schedule per-process events with high temporal resolution.
> 包含一个调度具有高时间分辨率的每个进程事件的类。

## Comm -- Communications Library --- 通信库

Contains classes to simplify communications via serial ports and UDP/TCP sockets.
> 包含类以简化通过串行端口和UDP/TCP套接字的通信。

## Cluster -- Cluster Abstraction Library --- 集群抽象库

Contains classes supporting cluster programming by providing a multicast-based intra-cluster communications mechanism, and derived cluster-transparent versions of file and pipe abstraction classes provided by the IO and Comm libraries.
> 包含通过提供基于组播的集群内通信机制支持集群编程的类，以及IO和Comm库提供的文件和管道抽象类的派生簇透明版本。

## Math -- Mathematics Library --- 数学库

Contains classes and functions to simplify type-independent mathematics programming.
> 包含类和函数，以简化类型独立的数学编程。

## Geometry -- Templatized Geometry Library --- 模板化几何库

Contains classes to represent and manipulate geometric objects. All classes are as generic as possible to simplify integration into other data structures.
> 包含用于表示和操纵几何对象的类。所有类都是通用的，以简化与其他数据结构的集成。

## MacOSX -- Mac OSX Support Library --- Mac OSX支持库

Contains classes to simplify programming under Mac OS X.
> 包含类以简化Mac OS X下的编程。

## GLWrappers -- OpenGL Wrapper Library --- OpenGL包装库

Contains functions and classes providing a C++ wrapper around the OpenGL library.
> 包含围绕OpenGL库提供C包装的函数和类。

## GLSupport -- OpenGL Support Library --- OpenGL支持库

Contains classes to simplify certain aspects of OpenGL programming, such as per-context application state, extensions, texture-based font rendering, etc.
> 包含类以简化OpenGL编程的某些方面，例如每个上下文应用程序状态，扩展，基于纹理的字体渲染等。

## GLXSupport -- OpenGL over X11 Support Library --- OpenGL over X11支持库

Contains a class abstracting X11 OpenGL windows.
> 包含一个抽象X11 OpenGL窗口的类。

## GLGeometry -- Templatized Geometry OpenGL Bindings Library --- 模板化几何OpenGL绑定库

Contains classes and functions to integrate templatized geometry classes and the OpenGL API.
> 包含用于集成模板化几何类和OpenGL API的类和函数。

## GLMotif -- OpenGL 3D Widget Library --- OpenGL 3D窗口库

Contains classes to implement 3D user interfaces using OpenGL.
> 包含使用OpenGL实现3D用户界面的类。

## Images -- RGB Image Abstraction Library --- RGB图像抽象库

Contains classes and functions to represent RGB images and load/save images from/to a variety of image file formats.
> 包含用于表示RGB图像的类和函数，并从各种图像文件格式加载/保存图像。

## Sound -- Low-Level Sound Library --- 低级声音库

Contains classes and functions to record and play back sound.
> 包含记录和播放声音的类和功能。

## ALSupport -- OpenAL Support Library --- OpenAL支持库

Contains classes to simplify audio programming using the OpenAL library.
> 包含类以简化使用OpenAL库的音频编程。

## Video -- Basic Video Library --- 基本视频库

Contains classes and functions to capture and encode / decode video streams.
> 包含捕获和编码/解码视频流的类和功能。

## SceneGraph -- Simple Scene Graph Renderer Library --- 简单场景图渲染器库

Contains classes and functions to represent dynamic 3D geometry as scene graphs.
> 包含用于将动态3D几何体表示为场景图的类和功能。

## Vrui -- Virtual Reality User Interface Development Library --- 虚拟现实用户界面开发库

Contains classes to simplify development of portable VR applications.
> 包含类以简化便携式VR应用程序的开发。
