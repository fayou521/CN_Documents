# Vrui VR Toolkit
> # Vrui 虚拟现实工具集

The task of a virtual reality (VR) development toolkit is to shield an application developer from the particular
configuration of a VR environment, such that applications can be developed quickly and in a portable and
scalable fashion. Three important parts of this overarching goal are encapsulation of the display environment,
encapsulation of the distribution environment, and encapsulation of the input device environment. In more
detail, these three partial goals are:

> 虚拟现实（VR）开发工具包的任务是保护应用程序开发人员不受VR环境的特殊配置的影响，从而使应用程序能够快速、灵活、可扩展地开发。

Display abstraction
A toolkit should provide OpenGL rendering contexts that are set up in such a fashion that rendering a
model in user-specific coordinates will display that model on all rendering surfaces (monitors, screens,
head-mounted displays) in correct head-tracked stereographic mode.
Distribution abstraction
As larger VR environments require more than one computer to operate, the detail aspects of
distribution (number of computers, connection topology, etc.) should be hidden by the toolkit. In
principle, there are at least three ways to distribute a rendering environment: "split first," where an
application is replicated on all computers and synchronized by distributing input device and ancillary
data; "split middle," where a shared data structure, e.g., a scene graph, is used to transmit data from one
application computer to all rendering computers; and "split last," where the OpenGL API call stream is
broadcast from one application computer to all rendering computers, e.g., using Chromium. A toolkit
should also hide the difference between a distributed rendering environment running on a cluster of
individual computers, and one running on a shared-memory multi-CPU computer with several
independent graphics pipes.
Input abstraction
There are a wide variety of different vendors, models and protocols to connect VR input devices such as
space balls, space mice, 6-DOF trackers, wands, data gloves, etc., to the computers comprising a VR
environment. A toolkit must hide these differences in hardware, and provide a uniform view of the set of
connected input devices. Furthermore, a toolkit should provide mechanisms not only to hide the
hardware details of the input device environment, but also the number and configuration of input
devices. An application should be written without aiming for a particular input environment (such as
"CAVE wand and head tracker" or "stylus, two pinch gloves, and head tracker"). Instead, the toolkit
should provide a layer that allows an application to specify its input requirements at a higher level, and
allows a user to map input devices to these requirements.

Most existing VR toolkits cover the first aspect well; some VR toolkits cover the second aspect by using any
one of the listed distribution schemes, but no toolkit we found covers the third aspect. Although all toolkits
have some kind of built-in input device driver to hide hardware details, none provide a higher-level semantic
interface that allows to write an application once, and run it in VR environments with widely differing input
environments. This implies that no toolkits provide an adequate way to run desktop applications using a
regular mouse and keyboard; although many of them have simulators, these are merely awkward low-level
debugging tools and are not useful for running and actually using a VR application on a desktop.

The Vrui VR toolkit aims to support fully scalable and portable applications that run on a range of VR
environments starting from a laptop with a touchpad, over desktop environments with special input devices
such as space balls, to full-blown immersive VR environments ranging from a single-screen workbench to a
multi-screen tiled display wall or CAVE. Applications using the Vrui VR toolkit are written without a particular
input environment in mind, and Vrui-enabled VR environments are configured to map the available input
devices to application functions such that the application appears to be written natively for the environment
it runs on. For example, a Vrui application running on the desktop should be as usable and intuitive as a 3D
application written specifically for the desktop.


```
Figure 1: The same Vrui application, run in a 4-sided CAVE (left) and on a laptop (right).
```
## Project Goals

The main project goals were to design and implement a VR development toolkit for scalable and portable
applications providing the following abstractions and additional features:

```
Display abstraction by setting up and maintaining OpenGL rendering contexts that can display head-
tracked stereoscopic images on monitors, fixed single- and multi-screen projections systems, movable
monitors or screens, head-mounted displays, or any combinations thereof.
Distribution abstraction using a split-first distribution of the toolkit itself, with support for transparent
multicast communication between an application computer and the rendering computers to allow
applications to internally use a split-middle scheme, for example using an external scene graph API or
application-specific communication protocols. On a shared-memory multi-CPU/multi-pipe visualization
system, applications should automatically make use of parallelism and high-speed communications.
Input abstraction by providing so-called "semantic events" which notify an application of user
interaction, and a set of run-time selectable so-called "tools" that map the input devices of a VR
environment to the semantic events of an application. Tools not only map from devices to events, but
also encapsulate the user interface to evoke certain events. For example, different styles of navigation
are implemented as different navigation tools, selectable by users at run-time, while an application only
understands a generic "navigate" event.
Powerful operating system-independent foundation libraries for functionality commonly used in VR
application development, such as distributed file I/O, 3D geometry abstractions, spatial data structures,
OpenGL extension management, GLSL shader support, access to video and sound recording hardware,
simple scene graphs, etc.
A 3D user interface component library that allows application programmers to assemble 3D user
interfaces using similar interactions as traditional 2D GUIs.
A scalable design paradigm, where applications only need to reference features that they require, such
that simple applications have simple (and short) source codes. A complete Vrui "Hello World"
application, rendering a cube and a sphere with full viewpoint navigation, can be written in 34 lines of
code (see VruiDemoSmall.cpp).
```
A more complete list of goals and the architecture / design features implementing them can be found in
the Vrui Manifesto.

## Project Status

At this point, all building blocks of the Vrui VR toolkit are in place and functional, and specific supported and
tested environments are:

```
Laptop or desktop computers with mouse and keyboard, and optional additional devices such as
joysticks, space balls, game pads, etc.
Responsive Workbench environment with head tracking, stylus, and two pinch gloves.
Immersadesk environment with head tracking and CAVE wand.
```

```
Tiled 2 x 2 display wall with two pinch gloves run by 4-node cluster (no head tracking, anaglyphic
stereo).
Upright display wall with optical tracking system and multiple custom-designed input devices.
Tiled 3 x 2 display wall with head tracking and updated CAVE wand run by 7-node cluster.
Four-sided CAVE with head tracking, updated CAVE wand, and pinch gloves run by 5-node cluster.
Four-sided CAVE with head tracking and updated CAVE wand run by SGI PRISM shared-memory multi-
CPU/multi-pipe vizualization system.
Low-cost fully immersive VR environment consisting of a 3D TV and an optical tracking system,
see Low-Cost VR.
Low-cost VR environment consisting of a 3D TV and a Razer Hydra desktop 6-DOF input device,
see Low-Cost VR.
Head-mounted displays (eMagin Z800 3DVisor, Sony HMZ-T1) tracked by Intersense tracking system,
with updated CAVE wand.
Oculus Rift DK1 and DK2, using built-in head trackers, with a wide variety of input devices
(mouse/keyboard, joysticks, spaceballs, Wiimote, desktop 6-DOF trackers such as Razer Hydra).
HTC Vive
```
Application-level streaming to layer shared graphics data structures in a split-middle architecture over Vrui's
internal split-first distribution is finally working reliably. The first application using split-middle is ProtoShop.
Instead of computing the inverse kinematics to manipulate proteins on all nodes of a cluster, the
computation is only done on the cluster's head node, and the results are broadcast to the render nodes. This
ensures cluster-wide synchronization in all cases, and improves responsiveness on clusters where the head
node has multiple CPUs (or CPU cores), and the render nodes only have single CPUs.

As of 04/08/2009, the Vrui VR toolkit has been released publicly under the GNU General Public License
version 2, and the most recent and several older versions are available for download from the download
page.

As of 11/25/2010, the Vrui VR toolkit has moved to version 2.0.

As of 06/07/2011, the Vrui VR toolkit has moved to version 2.1.

As of 12/16/2012, the Vrui VR toolkit has moved to version 2.6.

As of 08/12/2013, the Vrui VR toolkit has moved to version 3.0.

As of 12/14/2013, the Vrui VR toolkit has moved to version 3.1.

As of 10/13/2016, the Vrui VR toolkit has moved to version 4.2.


