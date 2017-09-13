# The Vrui FAQ

> # 有关Vrui工具包的问答

## General Questions
> ## 一般的问题

```
1. What is this Vrui thing, anyway
2. How is Vrui different from, say, glut?
3. How is Vrui different from Qt, Gtk+, etc.?
4. How is Vrui different from Open SceneGraph, OpenSG, OSG, etc.?
5. How is Vrui different from Ogre, Horde, Irrlicht, Sauerbraten, etc.?
6. How is Vrui different from other VR toolkits like Cavelib, FreeVR, VR Juggler, etc.?
```
 > <font color=#0099ff size=4 face="楷体">在大多数其他3D或VR工具包中，应用程序侧状态（如3D网格表示）和服务器端OpenGL状态（如纹理或缓冲区对象）都可以自由混合。在应用程序对象中，指向网格结构的指针可以直接跟着保持网格顶点的顶点缓冲对象的句柄。然而，这防止了对一个应用程序必须处理多个OpenGL上下文的环境的可移植性，其中单个网格结构可能由多个顶点缓冲区表示，每个顶点缓冲区用于不同的OpenGL上下文，具有不同的句柄。Vrui通过将应用程序侧状态与服务器端OpenGL状态进行严格分离，并将应用程序的服务器端状态的多个副本与应用程序侧状态的单个副本相关联，解决了此问题。Vrui API完全隐藏上下文和状态管理以及必要的状态复制。应用程序始终只能看到服务器端状态的单一副本，并且只在渲染需要此状态时。该过程在GLContextData文档中有详细描述。
</font>


## User Questions

```
7. On what operating systems does Vrui run?
8. Is it difficult to install Vrui?
9. How do I install Vrui in a system location?
10. What is the difference between Vrui.cfg and VRDevices.cfg?
11. Why does Vrui go all wonky when I use two monitors in a single-desktop setup?
12. How do I set up multiple rendering windows, for example to drive multiple projectors?
```

## Developer Questions

```
13. How do I get started developing in Vrui?
14. How do I set up a new Vrui-based project?
15. How do I use third-party libraries with Vrui's build system?
16. How do I receive input from the keyboard?
17. How do I query the display resolution in points per inch etc.?
18. How do I manage server-side OpenGL state, such as texture objects?
19. Can I use modern OpenGL in Vrui applications?
20. What if Vrui's extension manager does not support an OpenGL extension that I need?
21. Can I use GLEW in Vrui applications?
```

## 1. What is this Vrui thing, anyway?

Vrui (Virtual Reality User Interface) is a development toolkit for 3D graphics applications, with a strong focus on interactivity and immersive display environments. In the context of Vrui, immersive display nvironments mean displays consisting of one or more (large) stereoscopic screens, 3D head tracking, and 3D tracked input devices.
Canonical examples of immersive display environments are CAVEs or head-mounted displays (HMDs), and, more
recently, those composed of 3D TVs or consumer-level HMDs such as Oculus Rift, and gaming input devices.

Vrui's overriding goal is to support development of correct, portable, and usable applications. In this context,
portable means that an application is developed in one environment – typically a desktop environment – but
runs correctly in any environment. Usable means that a Vrui application run in a desktop environment is exactly
as effective as a native desktop application, and that the same Vrui application run in a CAVE or other immersive
environment type is exactly as effective as an application developed natively for that specific environment.

## 2. How is Vrui different from, say, glut?

In certain ways, Vrui is very much like glut. It allows writing graphics applications without having to worry about
the details of the underlying hardware or graphics system, and introduces very little overhead when writing
simple applications. Also, Vrui allows applications direct access to OpenGL, to employ custom low-level code. In
many cases, Vrui is a more powerful replacement for glut. It has little enough overhead that it is easily used for
very simple applications, but at the same time already offers much additional functionality, such as simplified
window management, multiple built-in 3D navigation metaphors, 3D measurement and annotation tools, virtual
3D input devices, recording and playback facilities, etc.

Unlike glut, Vrui is portable to non-desktop environments. Here, portable means that a Vrui application is only
written once, and then runs in any environment, without even having to be recompiled. This portability is


achieved by shielding application writers much more from the underlying display system as glut does: Vrui
applications do not have to open their windows or set up their OpenGL rendering contexts, and they do not
directly receive input from mouse or keyboard. In fact, handling of input is probably the biggest difference
between Vrui and glut (see How do I receive input from the keyboard?).

An additional difference between Vrui and glut is that Vrui consists of an entire hierarchy of layered libraries that
work together to support developers in writing correct, portable, and usable applications. For example, Vrui
contains a comprehensive cluster-transparent file I/O handling library, explicit high-performance intra-cluster
communication, a comprehensive library for affine and projective 3D geometry, OpenGL support classes
supporting generic programming, an OpenGL-based GUI widget set, and a scene graph library. While all these
are completely optional, and Vrui is intentionally designed to be as compatible as possible with third-party
libraries, developers are encouraged to use the highest-level available abstractions provided by the entire Vrui
package.

## 3. How is Vrui different from Qt, Gtk+, etc.?

Qt, Gtk+, etc. are primarily 2D GUI toolkits, which also happen to offer widgets representing 3D graphics context
for 3D rendering. Vrui, on the other hand, is primarily a 3D graphics toolkit, which also happens to offer widgets
for (3D) GUIs. In other words, the GUI widget set is only a relatively small part of Vrui.

Ignoring everything else, though, the widget set offered by Vrui is comparable to those offered by Qt or Gtk+ or
other 2D GUI toolkits, albeit not as complete (yet). Vrui's GUI widgets are three-dimensional, since they are
intended to work in a virtual three-dimensional display space, but their functionality and layout is very similar to
2D GUI widgets. There are dialogs, menus, buttons, sliders, etc., just as usual. From a programming point of
view, Vrui's GUI widgets more or less follow the approach of OSF/Motif, in that they primarily rely on automatic
layout based on a hierarchical description, and on (C++-style) callbacks to connect widgets to application
behavior. One could say that Vrui's widget set is a complete rip-off of OSF/Motif, translated to C++ and
OpenGL.

## 4. How is Vrui different from Open SceneGraph, OpenSG, OSG, etc.?

While Vrui contains a scene graph library, its primary focus is on providing direct access to the underlying
OpenGL 3D graphics library for custom applications with specific rendering needs. It is also possible, albeit often
difficult in practice, to layer an existing scene graph library on top of Vrui, for the added benefit of portability to
non-desktop environments.

## 5. How is Vrui different from Ogre, Horde, Irrlicht, Sauerbraten, etc.?

Vrui is not a complete game engine, but a toolkit to develop correct, portable, and usable 3D graphics
applications. Vrui does not address asset management, handling of 3D modeling data formats, view-dependent
or multi-resolution rendering, advanced lighting, collision detection, game logic, artifical intelligence, etc. These
aspects are delegated to application code, or to higher-level libraries, which could be part of Vrui itself (like the
scene graph library), or developed by third parties.

In other words, Vrui is primarily a display and interaction engine, and comparable to the lower levels contained
in any high-level game engine. As a result, given a game engine that has a clear separation of responsibilty
between its layers, it can be possible to layer an existing game engine on top of Vrui, for the added benefit of
being able to run that game engine in non-desktop environments. However, because most commercial or free
game engines do not make this clear distinction, doing so might be difficult in practice.

## 6. How is Vrui different from other VR toolkits like Cavelib, FreeVR, VR

## Juggler, etc.?

VR toolkits like Cavelib, FreeVR, VR Juggler, etc. are decidedly low-level toolkits. They shield application
developers from some aspects of the underlying display and input device hardware, but typically not enough to
support truly portable or usable applications. In a certain way, the analogous comparison between Vrui and, say,
Cavelib, in the desktop world is that between X Windows and an X-based GUI toolkit like Qt. While X hides
differences in display and input device hardware, and even distribution to some extent, applications based
directly on X have to contain their own GUI code. Before GUI toolkits became available, this really happened and


resulted in completely different user interfaces between applications. One application might have used the
middle mouse button to scroll; another might have used a proto-scroll bar; another might have used clicks into
a "map" of scrollable space; in short, the situation was untenable.

GUI toolkits like Qt or Gtk+ remedied that problem by introducing a higher level. Instead of working with
windows and mouse events, applications could now use "widgets" with specific purposes and widget events,
leading to much better user interfaces, and, more important, consistent interfaces between applications.

Vrui attempts to do to immersive 3D graphics what Qt et al. did to 2D GUIs. It provides higher-level interfaces,
to prevent individual applications from "reinventing the wheel," and instead foster consistent user interfaces.
Instead of dealing directly with OpenGL windows and 4x4 matrices representing input devices, Vrui applications
render into a toolkit-provided 3D application space, and receive higher-level events from input devices. As a
concrete example, in other toolkits navigation, i.e., the mapping from 3D application space to display space, is
handled by each application individually, whereas in Vrui it is handled by the toolkit. Overall, the result is that
Vrui applications with widely differing purposes "look & feel" the same.

An intended side effect of Vrui's higher-level abstractions is that Vrui applications are portable between vastly
different types of display environments. A single Vrui application will run in a CAVE like a native CAVE
application, and will run on the desktop very similarly to a native desktop application. In comparison, other
toolkits require applications to be developed for a narrower range of target environments, sometimes requiring
different code for single-screen and multi-screen environments, and none that we are aware of support running
applications on desktop environments in any way that is useful beyond basic debugging.

An additional difference is that Vrui consists of an entire hierarchy of layered libraries that work together to
support developers in writing correct, portable, and usable applications. For example, Vrui contains a
comprehensive cluster-transparent file I/O handling library, explicit high-performance intra-cluster
communication, a comprehensive library for affine and projective 3D geometry, OpenGL support classes
supporting generic programming, an OpenGL-based GUI widget set, and a scene graph library. While all these
are completely optional, and Vrui is intentionally designed to be as compatible as possible with third-party
libraries, developers are encouraged to use the highest-level available abstractions provided by the entire Vrui
package.

For example, Vrui's underlying geometry library is used throughout Vrui's API instead of passing positions or
orientations as flat 3- or 4-element arrays or 4x4 matrices. Vrui uses abstract geometry classes such as Point and
Vector for affine points and vectors, respectively, and a hierarchy of abstract transformation classes from
translations or rotations only over rigid body transformations to fully-general affine or projective
transformations. All these classes have full sets of algebraic operations, which means application code can in
almost all cases use them as black boxes. These higher-level classes significantly reduce the burden on
application developers to either write their own algebraic operations, such as matrix inversion, or continuously
convert back-and-forth between the toolkit's flat representation and the representation of a third-party
geometry library they want to use. An intended side-effect of the use of higher-level abstractions at the API is
that implicit constraints can be made explicit. For example, tracked 6-DOF input devices can only change
position and orientation, and instead of representing those as generic projective transformations, i.e., 4x
matrices, Vrui represents them as rigid-body transformations, i.e., a translation vector plus a unit quaternion.
This makes arithmetic involving input devices easier, more efficient, and more robust. That said, all classes have
methods to convert from/to flat array representations to be backwards-compatible with third-party libraries an
application developer might want to use.

## 7. On what operating systems does Vrui run?

Vrui runs on UNIX-like operating systems. It is primarily being developed on Linux, specifically 64-bit Fedora
Linux, but it builds and runs without problems on any other Linux distribution.

Vrui also builds and runs on Mac OS X, but there are currently some Linux-only features, particularly handling of
sound and video, some non-standard required libraries – libpng, libjpeg, libtiff, libusb – that need to be installed
from source or using software management systems such as homebrew, and Vrui is generally more thoroughly
tested on Linux.

Vrui does not build on Windows. Microsoft's Visual C++, at least the most recent version tested, cannot deal
with some of the C++ template constructs used in Vrui. Other C++ compilers, such as Intel's, can do those, but
they are typically quite expensive. Even with a proper C++ compiler, several of the underlying architecture


decisions in Vrui are deeply rooted in the UNIX paradigm. Porting Vrui would take significant initial effort, and
continual effort to maintain a split codebase.

Vrui does build and run under UNIX emulation systems such as cygwin, but it will not be particularly useful
because no version of cygwin we have tried offers hardware-accelerated 3D graphics. As Vrui applications rely
heavily on high-performance graphics, they will barely be usable under cygwin.

The same problem applies if Linux is run inside a virtual machine. No virtual machine hypervisor we have tried
offers hardware-accelerated 3D graphics inside the virtual machine, and Vrui applications will barely be usable
as a result.

## 8. Is it difficult to install Vrui?

Not really. Vrui's build system is intended to make "simple" installations simple. By default, Vrui installs itself into
the /usr/local directory hierarchy. The simplest installation procedure is thus ($ indicates commands entered at
the terminal prompt):

Go to the Vrui download web site and make note of the most recent Vrui release, Vrui-<major>.<minor>-
<build>, e.g., Vrui-4.2-001.

Download and unpack the current Vrui source tarball into a src directory:

$ cd ~
$ mkdir src
$ cd src
$ wget -O - [http://idav.ucdavis.edu/~okreylos/ResDev/Vrui/Vrui-<major>.<minor>-<build>.tar.gz](http://idav.ucdavis.edu/~okreylos/ResDev/Vrui/Vrui-<major>.<minor>-<build>.tar.gz) | tar xfz -
$ ls
Vrui-<major>.<minor>-<build>

This created Vrui's build directory, named by major and minor version number and build number, such as
/home/alice/src/Vrui-4.2-001. Enter the build directory, and build and install Vrui:

$ cd Vrui-<major>.<minor>-<build>
$ make
... lots of text...
$ sudo make install
(enter administrator user's password)
... lots of text...

This created several directories inside /usr/local containing all Vrui header and library files, and some utility
programs. To build Vrui's example programs and run a test, follow this directly by:

$ cd ExamplePrograms
$ make
... lots of text...
$ ./bin/ShowEarthModel

Most Vrui-based applications are configured to work with the default installation, so building an application is
typically as simple as downloading and unpacking the tarball and running "make."

To install optional system packages for additional Vrui functionality see the "quick installation guide" on the Vrui
download page.

## 9. How do I install Vrui in a system location?

Note: This question and answer are mostly obsolete; as of version 4.0, Vrui installs itself under /usr/local by
default.

Short answer:

$ make INSTALLDIR=/usr/local
... lots of text...
$ sudo make INSTALLDIR=/usr/local install
[sudo] password for <user>: (enter password)
... lots of text...

Long answer:

By default, Vrui installs itself in ~/Vrui-<major>.<minor>, i.e., in the installing user's home directory. For legacy reasons,
Vrui does not use the common ./configure, make, make install toolchain, but Vrui's build system offers the same level


of flexibility. Installation targets can be defined as a whole or individually by editing Vrui's makefile, or by
passing VARIABLE=<value> arguments on make's command line. When overriding variables via make's command line, it is
important to use the same arguments during make and make install (but see Creating Binary Packages for an
exception).

The most basic installation override is the INSTALLDIR variable defined at the very top of Vrui's makefile (to the
default value $(HOME)/Vrui-<major>.<minor>). Unless detail changes are made, Vrui installs itself into $(INSTALLDIR), using
the canonical include, lib(64), bin, etc, and share subdirectories. The easiest (and most easily un-installed) way to
install Vrui system-wide is to set INSTALLDIR to /usr/local/Vrui-<major>.<minor>:

$ make INSTALLDIR=/usr/local/Vrui-<major>.<minor>
... lots of text...
$ sudo make INSTALLDIR=/usr/local/Vrui-<major>.<minor> install
[sudo] password for <user>: (enter password)
... lots of text...

In this case, "make install" has to be run as super-user because it will create a subdirectory in a system directory.
This simple installation procedure will create a system-wide installation in /usr/local/Vrui-<major>.<minor>, and un-
installing Vrui will be as simple as sudo rm -rf /usr/local/Vrui-<major>.<minor>, but it will not follow the POSIX
conventions of installing software in /usr/local. According to POSIX, include files go into /usr/local/include, library
files into /usr/local/lib(64), executables into /usr/local/bin, configuration files into subdirectories of /usr/local/etc,
shared files into subdirectories of /usr/local/share, configuration files for pkg-config into /usr/local/lib(64)/pkgconfig,
and documentation files into subdirectories of /usr/local/share/doc. Vrui's build system has a variable for each of
these destinations: HEADERINSTALLDIR, LIBINSTALLDIR, EXECUTABLEINSTALLDIR, ETCINSTALLDIR, SHAREINSTALLDIR, PKGCONFIGINSTALLDIR, and
DOCINSTALLDIR, respectively. For example, to install Vrui-4.2-<build> in a POSIX-compliant fashion:

$ make HEADERINSTALLDIR=/usr/local/include/Vrui-4.2 LIBINSTALLDIR=/usr/local/lib64/Vrui-4.2 EXECUTABLEINSTALLDIR=/usr/local/bin \
ETCINSTALLDIR=/usr/local/etc/Vrui-4.2 SHAREINSTALLDIR=/usr/local/share/Vrui-4.2 PKGCONFIGINSTALLDIR=/usr/local/lib64/pkgconfig \
DOCINSTALLDIR=/usr/local/share/doc/Vrui-4.
$ sudo make HEADERINSTALLDIR=/usr/local/include/Vrui-4.2 LIBINSTALLDIR=/usr/local/lib64/Vrui-4.2 EXECUTABLEINSTALLDIR=/usr/local/bin \
ETCINSTALLDIR=/usr/local/etc/Vrui-4.2 SHAREINSTALLDIR=/usr/local/share/Vrui-4.2 PKGCONFIGINSTALLDIR=/usr/local/lib64/pkgconfig \
DOCINSTALLDIR=/usr/local/share/doc/Vrui-4.2 install

Those command lines are quite a mouthful (and it's probably more practical to make these changes directly in
Vrui's makefile), but they are appropriate for shell scripting, such as when creating binary packages. By the way,
Vrui's build system prints all installation directories at the beginning of its build output. A quick way to check is
to run make <arguments> config.

That said, Vrui's build system contains logic for several common installation cases. Specifically, there is a
shortcut for the exact installation structure spelled out in detail above:

$ make INSTALLDIR=/usr/local
$ sudo make INSTALLDIR=/usr/local install

And there is yet another shortcut, for deep system-wide installations in /usr/include/Vrui-<major>.<minor>,
/usr/lib(64)/Vrui-<major>.<minor>, /usr/bin, /etc/Vrui-<major>.<minor>, /usr/share/Vrui-<major>.<minor>, /usr/lib(64)/pkgconfig, and
/usr/share/doc/Vrui-<major>.<minor>:

$ make SYSTEMINSTALL=1 INSTALLDIR=
$ sudo make SYSTEMINSTALL=1 INSTALLDIR= install

(That's not a typo, the value after INSTALLDIR= must be the empty string.) However, the Linux file system standard
states that software locally installed from source should not go into /usr and /etc, but into /usr/local. Unless one
is...

### Creating Binary Packages

Most Linux distributions have a way of distributing software as binary packages, under the auspices of a more or
less smart package manager. Installing binary packages has benefits for end users, but Vrui is canonically
distributed as source tarballs. However, building binary packages is quite straightforward. While the details
depend on the package manager used by a given Linux distribution (rpm, dpkg, ...), the common fundamental
approach is to build and prepare software inside a fake root directory, and then package all created files under
that fake root into a single archive for distribution.

The tricky bit here is that the software has to be configured and built for its final destination, such as /usr/bin etc.,
but has to be installed into the fake root directory for packaging. Vrui's build system simplifies that by using the
combination of the SYSTEMINSTALL and INSTALLDIR variables. To build and configure Vrui for the final destination, run

$ make SYSTEMINSTALL=1 INSTALLDIR=


and to install the built software into a fake root for packaging, run

$ make SYSTEMINSTALL=1 INSTALLDIR=/path/to/fake/root install

(sudo is not necessary here because the fake root directory is assumed to be under the user's home directory.)

Then, the final step is the distribution-specific equivalent of

$ cd /path/to/fake/root
$ tar cfz Vrui-<major>.<minor>.binary.tar.gz *

## 10. What is the difference between Vrui.cfg and VRDevices.cfg?

Vrui contains its own low-level device driver, VRDeviceDaemon, to talk to input device hardware such as 3D
tracking systems, data gloves, wands, joysticks, etc. and convert their data streams into a unified format to be
accepted by the Vrui toolkit run-time environment. This device driver is configured for a particular display
environment via the VRDevices.cfg configuration file. This file defines what types of input device hardware are
present, and how they work together in the same space.

The Vrui run-time environment itself is configured via the Vrui.cfg configuration file. This file defines the
complete display environment, including the positions, orientations, and sizes of all screens, all viewers present
in an environment, the OpenGL windows used to render 3D views to those screens, the number and types of 3D
input devices presented by the low-level device driver, which tools are available, etc.

Concretely, VRDevices.cfg is read when the low-level device driver is started, whereas Vrui.cfg is read whenever a
Vrui application is started. Both configuration files together define the complete set-up of the display
environment in which Vrui runs, and need to be created and/or adapted carefully based on the details of a
concrete environment. In a shared display environment like a CAVE, this task would typically fall to a dedicated
system integrator / administrator, whereas configuration is more or less unnecessary in single-user desktop
environments, where the default setup will cover the common cases.

## 11. Why does Vrui go all wonky when I use two monitors in a single-

## desktop setup?

In its default desktop configuration, Vrui tries to determine the actual size of the environment's display by
querying the X Windows system. Normally, this ensures that Vrui's rendering has the correct aspect ratio, e.g.,
spheres show up as spheres instead of ellipsoids, and that it has the correct size, e.g., the scale bar has the
correct length, and that fonts show up at the correct sizes. However, X sometimes lies about the sizes of its
attached displays, especially when multiple displays are joined in a single-desktop setup (such as Nvidia's
TwinView). This is especially bad when the joined displays have different resolutions, say a laptop display with
100dpi next to a 3D TV with 30dpi.

In the first scenario, where all displays have comparable resolutions, but X lies, you will have to disable
automatic screen size detection (by setting autoScreenSize to false in your window section), and enter the
correct screen size in your screen section. In single-desktop setups, the screen size is the combined size of all
screens, not that of a single screen.

This will not work if the screens' resolutions are different. If that's the case, it will not be possible to drag Vrui
windows between screens without introducing major distortions -- after all, the Vrui run-time now has to deal
with a single (virtual) display that suddenly changes resolution in the middle. You will have to decide which of
the screens is the main screen, and configure Vrui for that screen. Then, if an application window is dragged to
another screen, partially or completely, behavior is undefined.

To set up a main screen, add a panningDomain setting to the appropriate window section in Vrui.cfg, and set
the panning domain to the position and size of the main screen. For example, if the main screen has a resolution
of 1920x1080, and is to the right of a secondary screen with resolution 1440x900, panningDomain would be
(1440, 0), (1920, 1080). Then set autoScreenSize to false, and set the proper size of the main screen in the screen
section associated with the window. This will correlate the pixel size of the panning domain with the physical
size of the screen, and Vrui will render to scale. If Vrui windows are dragged outside the panning domain, things
will get weird, however.

Here is an example configuration fragment for a 30 dpi 1920x1080 main screen to the right of a 1440x
100dpi secondary screen:


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

The "Screen0" section is not actually used in this example.

## 12. How do I set up multiple rendering windows, for example to drive

## multiple projectors?

Vrui is quite flexible with regards to multi-window/multi-display rendering. The Vrui run-time environment can
dynamically configure itself to run in a cluster with multiple rendering nodes, on a single machine with multiple
graphics cards, or on a single machine with a single graphics card and multiple displays connected to multiple
video outputs, or any combination of these. For example, when running a "super CAVE" with six walls and two
pairs of stacked 4K projectors generating 4000x4000 pixel stereoscopic images per wall, one could use a seven-
node cluster, where the head node drives a standard desktop monitor and is the user-facing console, and each
of the six render nodes has two high-end graphics cards with two projectors connected to each graphics card.
How to configure Vrui for such environments, and how to maximize rendering performance via context sharing
and parallel rendering, is explained in detail in the Multi-window Rendering in Vrui document.

As far as application programmers are concerned, the details of managing multiple windows, sharing or
replicating OpenGL contexts, and parallel rendering are completely hidden by the Vrui API. See the
GLContextData document for a detailed explanation of the underlying abstraction mechanism.

## 13. How do I get started developing in Vrui?

Unfortunately, Vrui does not yet have comprehensive developer guides. As a prerequisite, developing for Vrui
requires a good understanding of 3D graphics, especially of the OpenGL graphics library on which Vrui is based,
and of C++. With these in place, starting Vrui developers are encouraged to think of Vrui as being very similar to
glut, and take it from there. Vrui comes with a set of template or example applications showing the (little
amount of) boilerplate code required to set up a Vrui application, and detailed comments explaining where one
would put application and 3D rendering code, for example. If a developer has code for a glut application, it is
usually enough to strip out any OpenGL setup and viewpoint navigation code (OpenGL setup and navigation are
handled by Vrui), paste the rest into the simplest Vrui template, and compile that to get a working Vrui
application.

After these initial steps, developers are encouraged to use several existing Vrui applications to see how they
look and feel, especially as they are run in different display environment types, and then peruse the HTML
documentation that does exist. A good starting point is the "Library Overview" section, which lists the
component library layers that make up the whole Vrui package, and in turn list all the header files and classes
contained in those libraries. The interfaces of all those classes are defined in their respective header files,
including detailed comments on all interface methods and functions.

Another important starting point is the Vrui kernel API in include/Vrui/Vrui.h, which declares all core functions
applications use to communicate with Vrui. Due to Vrui's microkernel architecture, most of its functionality is not
provided by the kernel itself, but by delegate classes such as Vrui::VRWindow, or by external so-called managers
such as the input device manager. References to these managers are retrieved through the kernel API, but
afterwards applications communicate directly with those managers. The bottom line is that all of Vrui's
functionality is accessed through the kernel API, either directly or indirectly via managers or delegate classes.


While the low-level documentation of classes and interfaces provided by the source code comments are
reportedly rather good, they don't tell in which scenarios a developer might use a certain class. This will
hopefully be addressed by higher-level development guides in the near future. In the meantime, another
important resource is the Development Rules document, which lists common pitfalls stemming from Vrui's
difference in philosophy to other VR or 3D graphics toolkits, and its focus on portability and usability. For
example, the way Vrui applications are supposed to handle user input is Vrui's one feature most distinct from
other toolkits.

## 14. How do I set up a new Vrui-based project?

The easiest way to set up a new Vrui-based project is to use Vrui's internal build system. This build system,
which is used by Vrui itself and all "official" Vrui extension packages and applications, might seem somewhat
daunting at first glance. On the upside, it makes building libraries and applications hassle-free and fast, and is
very extensible.

Let's say you want to create a new project called "Foo," containing an executable called "Foo" created by
compiling and linking together source files Bar.cpp and Foo.cpp.

First, create a new directory called "Foo" somewhere, and copy Vrui's template makefile into it:

$ mkdir Foo
$ cd Foo
$ cp <Vrui share dir>/make/makefile.

where <Vrui share dir> is the share directory underneath Vrui's installation directory, such as ~/Vrui-4.2/share or
/usr/share/Vrui-4.2 or /usr/local/share/Vrui-4.2. Then edit the makefile as follows:

```
After the line PACKAGES =, add the name of the main Vrui package, MYVRUI:
PACKAGES = MYVRUI
```
```
This will tell the build system that all executables in this project will use the Vrui package.
After the line ALL =, add the full path to all executables of this project, in this example $(EXEDIR)/Foo:
ALL = $(EXEDIR)/Foo
```
```
$(EXEDIR) is a pre-defined variable pointing to the destination directory for executables, i.e., ./bin.
At the end of the makefile, under the heading "Specify build rules for executables," add the dependencies
for the Foo executable:
$(EXEDIR)/Foo: $(OBJDIR)/Bar.o $(OBJDIR)/Foo.o
```
```
$(OBJDIR) is a pre-defined variable pointing to the destination directory for compiled object files. The root
object file directory is ./o, but it contains an entire subdirectory hierarchy to keep object files for different
compiler options separate, such as debug and non-debug versions.
```
```
For executables with many object files, the above is somewhat inconvenient. A better way is to create a list
of names of source files needed for an executable, and let make figure out the object file names
automatically:
FOO_SOURCES = Bar.cpp Foo.cpp
$(EXEDIR)/Foo: $(FOO_SOURCES:%.cpp=$(OBJDIR)/%.o)
```
```
The code after $(EXEDIR)/Foo is a function that changes all names in FOO_SOURCES ending in .cpp into their
appropriate object file names in the $(OBJDIR) hierarchy.
```
```
As an aside, Vrui's build system can deal with source files in multiple subdirectories without any issues. Just
use a project directory-relative path name for each source or object file. A source file Baz/Baz.cpp will turn
into an object file $(OBJDIR)/Baz/Baz.o.
```
```
Create source files Bar.cpp and Foo.cpp and enter your application code.
To build the project, simply type
$ make
```
```
This will recompile all changed source files, and relink all executables. Vrui's build system automatically
takes care of additional dependencies such as included header files. There is no need to explicitly enter
source dependencies into the makefile.
```

```
In projects with multiple executables, make will build all executables by default. To build only a single one,
list its path name on make's command line:
$ make bin/Foo
```
```
Just running make Foo will not do the right thing. To fix that, add the following lines after the build rule for
$(EXEDIR)/Foo in the makefile:
.PHONY: Foo
Foo: $(EXEDIR)/Foo
```
```
After that addition, make Foo will work as expected.
```
For all this work, you'll get these major benefits: you will never have to worry about source file dependencies,
you will probably never have to use make clean, and make will never waste time rebuilding files that don't need
rebuilding.

### But that's too complicated! I just want to compile one source file!

Oh, OK. In that case, go into the directory containing your source file, say Foo.cpp. Copy Vrui's template makefile
into that same directory:

$ cp <Vrui share dir>/make/makefile.

where <Vrui share dir> is the share directory underneath Vrui's installation directory, such as ~/Vrui-4.2/share or
/usr/share/Vrui-4.2 or /usr/local/share/Vrui-4.2.

Then compile Foo.cpp into ./bin/Foo and execute it:

$ make PACKAGES=MYVRUI Foo
$ ./bin/Foo

## 15. How do I use third-party libraries with Vrui's build system?

Vrui's internal build system uses the notion of "packages" to set up and use internal or external software
libraries. A package defines compiler and linker flags that are needed to compile source files and link
executables against a given library. The list of external packages known to Vrui can be found in the
make/Packages.System file in Vrui's share directory, and Vrui's own component libraries can be found in
make/Packages.Vrui in the same directory.

Creating a new package to use some external library in a project based on Vrui's build system is straightforward.
As an example, we will use a locally-built version of the ImageMagick library, installed underneath
/usr/local/Magick6. First, we need to pick some unique name for the resulting package, say IMAGEMAGICK_LOCAL. We will
then add a section for that package somewhere in the makefile of the project using it:

IMAGEMAGICK_LOCAL_BASEDIR =
IMAGEMAGICK_LOCAL_DEPENDS =
IMAGEMAGICK_LOCAL_INCLUDE =
IMAGEMAGICK_LOCAL_CFLAGS =
IMAGEMAGICK_LOCAL_LIBDIR =
IMAGEMAGICK_LOCAL_LIBS =
IMAGEMAGICK_LOCAL_LINKLIBFLAGS =

```
_BASEDIR is an optional redirect for the root directory containing the package's include and library files. This
is usually /usr or /usr/local.
_DEPENDS is a list of other packages on which the package depends. For example, the GLU (OpenGL Utility)
package listed in Vrui's Packages.System file depends on the GL (OpenGL) package.
_INCLUDE is the list of directories containing the package's header files, including the -I prefix before each
directory name. What directory names to use depends on common usage. For example, OpenGL's header
files are in /usr/include/GL, but most source code includes them via #include <GL/gl.h> etc. As a result, GL_INCLUDE
would be set to -I/usr/include.
_CFLAGS is a list of additional compiler flags needed to compile source files against the package.
_LIBDIR is the list of directories containing the package's library files, including the -L prefix before each
directory name.
_LIBS is the list of the package's library files, including the -l prefix before each abbreviated library name.
For example, the OpenGL library would be listed as -lGL.
_LINKLIBFLAGS is a list of additional linker flags needed to link against the package's libraries. Common flags
are runtime search paths to find a library file in a non-standard location without having to use the
```

```
LD_LIBRARY_PATH environment variable or other workarounds.
```
This is the resulting package section for our example non-standard ImageMagick package:

IMAGEMAGICK_LOCAL_BASEDIR = /usr/local/Magick
IMAGEMAGICK_LOCAL_DEPENDS =
IMAGEMAGICK_LOCAL_INCLUDE = -I$(IMAGEMAGICK_LOCAL_BASEDIR)/include/ImageMagick-
IMAGEMAGICK_LOCAL_CFLAGS = -fopenmp -DMAGICKCORE_HDRI_ENABLE=0 -DMAGICKCORE_QUANTUM_DEPTH=
IMAGEMAGICK_LOCAL_LIBDIR = -L$(IMAGEMAGICK_LOCAL_BASEDIR)/lib
IMAGEMAGICK_LOCAL_LIBS = -lMagick++-6.Q
IMAGEMAGICK_LOCAL_LINKLIBFLAGS = -Wl,-rpath $(IMAGEMAGICK_LOCAL_BASEDIR)/lib

The _LINKLIBFLAGS variable is only necessary because the ImageMagick library is in a non-standard place where the
dynamic linker would otherwise not find it.

By convergent evolution, Vrui's package structure is very similar to that of the pkg-config utility. Another way to
populate the package section for packages supported by pkg-config is the following:

IMAGEMAGICK_LOCAL_BASEDIR =
IMAGEMAGICK_LOCAL_DEPENDS =
IMAGEMAGICK_LOCAL_INCLUDE = $(pkg-config --cflags-only-I)
IMAGEMAGICK_LOCAL_CFLAGS = $(pkg-config --cflags-only-other)
IMAGEMAGICK_LOCAL_LIBDIR = $(pkg-config --libs-only-L)
IMAGEMAGICK_LOCAL_LIBS = $(pkg-config --libs-only-l)
IMAGEMAGICK_LOCAL_LINKLIBFLAGS = $(pkg-config --libs-only-other)

To use a package in a project, simply append its name to the PACKAGE variable in the project's makefile. If the
package is only required by some executables in a project, its name can be listed in a specific executable's build
section. For example:

$(EXEDIR)/Foo: PACKAGES += IMAGEMAGICK_LOCAL
$(EXEDIR)/Foo: $(OBJDIR)/Foo.o

## 16. How do I receive input from the keyboard?

You don't. No, really. Unlike glut et al., Vrui aims to support environment-independent applications, and non-
desktop environments do not have keyboards. As a result, Vrui applications receive input in a very different
manner than, say, glut applications. Instead of directly receiving events from the keyboard (or the mouse) via
dedicated callbacks, Vrui applications receive events from so-called tools, which in turn receive events from
actual input devices.

To use a concrete example, assume an application that wants to do something when the user presses a specific
key. In glut et al., this would be handled by registering a keyboard event callback, which will in turn check the
identity of the just-pressed key when called, and invoke the proper application behavior if the desired key is
pressed. In Vrui, application behaviors are implemented as tools. If an application has some behavior X that is to
be invoked when some key is pressed, the developer would create a corresponding tool class X and hand it to
Vrui's tool manager during start-up. After that, a user can dynamically bind a tool object of class X to any button
she desires; after that, if she presses that button, the bound tool object will be called, and can in turn invoke
behavior X.

While this approach sounds more complicated, it has important benefits. For one, it works in non-desktop
environments. Even if there is no keyboard, there will still be some input device that has some ways to signal
that a user wants to initiate an event (which could be an actual button, or a gesture, or a voice command, etc.),
and Vrui's dynamic tool binding mechanism allows the user to bind a tool of class X to any such event source,
without the application developer having to code any support for that. Even if constrained to the desktop, this
allows users to map application behaviors to any keys they desire – in other words, the keyboard remapping
functionality common in PC games comes for free in Vrui.

Additionally, in actual code, setting up a tool is no more complicated than writing a callback. For simple cases
like the one above, where a key press invokes some behavior, Vrui offers a convenience shortcut that creates a
tool class, passes it to Vrui's tool manager, handles dynamic binding, and invokes an arbitrary application
callback when an event happens, in a single line of code (see VruiEventToolDemo.cpp in the ExamplePrograms subdirectory
for several concrete examples). Only in more complex cases, such as when behaviors require their own internal
states, will a developer have to implement an actual tool class, which would be essentially the same amount of
work as in other toolkits.

### But I don't want events, I want the user to enter some text, such as a file name or label!


In that case, still don't query the keyboard. Create a GLMotif dialog box with an editable text field and let Vrui
worry about where that text comes from (from a real keyboard, from a virtual keyboard, from a gesture-based
text entry method, from speech recognition, ...).

## 17. How do I query the display resolution in points per inch etc.?

You don't. Due to Vrui's portability, an application might not have a display, might have multiple displays with
different resolutions, or might have an immersive display, where display resolution is not even an applicable
concept. In Vrui, only very specific applications, such as calibration utilities, would need to know anything about
display resolution. In almost all cases, the actual functionality for which a developer would like to know the
display resolution can be achieved in a more direct way. One common example is scaling: a developer wants to
be able to display 3D data at fixed scales (1:1, 1:100) such that images on the display can be measured. This is
supported directly in Vrui: if an application tells Vrui which unit of measurement is used by an application, it will
provide an interactive scale bar that shows the exact scale factor from application to real world, and a user
interface to adjust the scale factor to common fixed values, such as 1:1.

Another common example is an application needing to know how big to make a display, like a text label, or how
big to make the influence zone around an interaction event, based on display resolution. In Vrui, these
parameters are configured by the system integrator/administrator appropriately for a concrete environment,
and can be queried directly via the Vrui kernel API.

In other words, Vrui applications should always render in 3D application space in any unit of measurement they
want, advertise that unit to Vrui's coordinate manager, and let Vrui take care of the rest. Any questions about
appropriate display sizes, interaction fuzz values, optimal font sizes, etc. should be answered by querying the
Vrui kernel API whenever needed.

## 18. How do I manage server-side OpenGL state, such as texture

## objects?

In most other 3D or VR toolkits, application-side state, such as a 3D mesh representation, and server-side
OpenGL state, such as texture or buffer objects, are mixed freely. In an application object, a pointer to a mesh
structure might directly be followed by a handle to a vertex buffer object holding the mesh's vertices. This,
however, prevents portability to environments where one application has to deal with multiple OpenGL
contexts, where a single mesh structure might be represented by several vertex buffers, each for a different
OpenGL context, with a different handle. Vrui solves the problem by strictly separating application-side state
from server-side OpenGL state, and associating multiple copies of an application's server-side state with a single
copy of application-side state. Context and state management, and necessary state replication, are completely
hidden by the Vrui API. Applications always only see a single copy of server-side state, and only at the time this
state is needed for rendering. This process is described in detail in the GLContextData document.

## 19. Can I use modern OpenGL in Vrui applications?

At first glance, it might seem as if Vrui is a ca. 1998 old-school OpenGL 1.0 toolkit. And while Vrui itself does still
contain enough legacy code to require running under an OpenGL compatibility profile (removing that is
ongoing work in progress), Vrui applications are free to use modern features (and even many components of
Vrui do so), as long as their functionality is either directly provided by the underlying OpenGL library or available
via OpenGL extensions. There are two main approaches to use modern features:

```
1. Simply call a modern OpenGL entry point and hope for the best.
2. Use Vrui's internal OpenGL extension wrangling mechanism.
```
The first approach might work on a developer's local machine, where the OpenGL driver automatically exports
all OpenGL entry points, but it won't be portable. The second approach requires a small amount of additional
work, but it much safer and more flexible. Here is how it is used:

Let us assume that some module of your project, say a single class, wants to use some OpenGL extension, say
GL_ARB_vertex_buffer_object to store some geometry in the GPU's own memory. In Vrui, each supported OpenGL
extension is represented as an individual class, with header files in the GL/Extensions subdirectory.

To use an extension, the source file defining the class first has to include the appropriate header file:


#include <GL/Extensions/GLARBVertexBufferObject.h>

(note the conversion from separate_words to CamelCase). Including this file makes all #define constants and
function prototypes provided by the extension available to the source file's code. All declarations will use the
extension's name space suffix (ARB, EXT, etc.), such as GL_ARRAY_BUFFER_ARB or glGenBuffersARB.

After that, the code has to initialize the extension before any of its functions can be used by calling the
extension class's static initExtension method:

GLARBVertexBufferObject::initExtension();

This is typically done once per lifetime of each created object, typically in the class's OpenGL context
initialization method (see initContext in GLContextData). Vrui's extension manager is designed such that multiple
redundant calls to initExtension are safe and fast; developers do not have to worry about managing extension
initialization and can leave it completely decentralized, improving code modularity.

The initExtension method throws a run-time error exception if an extension is not supported by the current
OpenGL context. For more flexibility, especially when using new and rarely-supported extensions, application
code can query the availability of an extension via the static isSupported method:

if(GLARBVertexBufferObject::isSupported())
GLARBVertexBufferObject::initExtension();
else
{
/* Set up a fallback rendering path etc. */
}

This pattern only makes sense if there is a reasonable alternative to using the requested extension, such as a
fallback rendering path. If a module cannot meaningfully function without a certain extension, it is probably best
to let initExtension throw an exception and let the module's client code decide what to do.

An important fact about OpenGL extensions is that they are context-dependent state. Due to Vrui's multi-
machine, multi-pipe rendering architecture, it is entirely possible (albeit rare) that a certain extension is
supported in one OpenGL context used by the application, and not in another. This is normally handled
transparently by Vrui's GLContextData mechanism (see GLContextData), but an important effect is that isSupported,
initExtension, and all extension functions can only be called when there is a current OpenGL context, i.e., during a
class's initContext or rendering method, or when a class's method is called from inside another class's initContext
method etc. The GLContextData document contains a more detailed treatment of application/OpenGL state
separation and multi-context rendering.

## 20. What if Vrui's extension manager does not support an OpenGL

## extension that I need?

Vrui strives to support all OpenGL extensions in the long term, but as of right now, due to limited developer
time, only those OpenGL extensions that are required by Vrui itself or by "official" Vrui applications, and those
that were requested by external Vrui developers, are available. Find the full list of extensions supported by your
version of Vrui inside the include/GL/Extensions directory. If an extension you require is not there, you have two
options:

```
1. Write an extension class using an existing extension class as a template, and ideally submit it back to the
Vrui repository.
2. Ask someone to do it for you (yes, please do!).
```
## 21. Can I use GLEW in Vrui applications?

Yes, but it is tricky, finicky, and unsupported. GLEW (the OpenGL Extension Wrangler) is an external library with
the same goals as Vrui's built-in extension manager (see Can I use modern OpenGL in Vrui applications?).
Unfortunately, GLEW's design clashes with Vrui's multi-machine/multi-context rendering architecture. While
there is a multi-context capable version of GLEW, its use in an application must be centralized, and requires
inclusion of GLEW's header file(s) in every source file of a Vrui application using GLEW, before any OpenGL
headers are included. While some external Vrui developers have been successful in using GLEW in their projects,
it requires careful management and breaks easily. Most importantly, GLEW cannot be used in a library, unless
users of the library add GLEW handling throughout their applications. Vrui's built-in extension manager is much
more compatible, but see What if Vrui's extension manager does not support an OpenGL extension that I need?.


