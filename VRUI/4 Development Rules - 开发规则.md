# Development Rules

Vrui application developers have to follow certain rules to create correct, portable, and
usable applications. Many of these rules are enforced by the Vrui API itself, but others have
to be willingly obeyed by developers. This document attempts to spell out all those
additional rules, and explain the reasoning behind them.

```
Correctness Rules
Do not use srand()
Do not use clock(), clock_gettime(), etc.
Do not change application state asynchronously
Do not change application or toolkit state during rendering
Do not store OpenGL or OpenAL per-context state in application state
Do check whether third-party libraries mix application and per-context state
Do check whether third-party libraries clean up after themselves
Portability Rules
Do not perform OpenAL or OpenGL context setup
Do not assume context state persistence
Do aim to always use the highest possible abstraction level offered by the toolkit
Do not change Vrui display environment layout programmatically
Do understand the relationship between physical and navigational space
Do not make assumptions about physical space
Do not directly reference screens, viewers, windows, or input devices
Usability Rules
Do not block Vrui's foreground thread
Do mind your application's frame rate
Do not mess with the navigation transformation
Do not force a particular scale factor
Do express all user interactions in physical space
Do not hardcode interaction parameters
Do not hardcode tool bindings
Do not implement custom tool classes that require configuration
```
## Correctness Rules

As every developer should strive to create correct applications, the rules in this category
must be followed to ensure that applications function in a predictable and replicatable
manner.

One important, and Vrui-specific, component of correctness is cluster consistency. As Vrui
applications can be run in single-system or cluster mode depending on environment setup,
applications that fail to work correctly in a cluster environment must be considered
incorrect. In cluster mode, Vrui employs a split-first paradigm, where an application is
executed independently on each cluster node, and application instances are synchronized
by broadcasting input device data. As a result, applications must be implemented such that
their internal state deterministically follows from the input data stream; if individual
application instances run "out of synch," the application ceases to function as a whole.


Cluster consistency can pose problems for applications using pseudo-randomness, wall
clock-based processing, or multithreading. These features need to be implemented
carefully, and Vrui contains support for each of these areas. Specifically, while Vrui itself
uses a split-first paradigm, applications are free to implement a split-middle approach as
they see fit, by using the explicit cluster communication interfaces provided by Vrui and its
underlying libraries. For applications using time-based computing or parallelism, explicit
communication is often required to ensure cluster consistency. For example, an application
that does background computations might employ cluster communication to only do
computations on the cluster's head node – which often has more processing power
anyway – and then broadcast the results to all other nodes.

### Do not use srand()

Vrui calls srand() itself, with the current wall-clock time, at the beginning of its own
initialization. It ensures that all nodes in a cluster use the same random number seed, so
that it is safe to use random numbers in the main thread of a Vrui application without
breaking cluster consistency. Unfortunately, it is not possible to use random numbers from
background threads, or multiple threads in parallel. If an application requires this, it is the
developer's responsibility to explicitly communicate any generated random numbers from
a cluster's head node to all slave nodes using per-thread cluster pipes.

### Do not use clock(), clock_gettime(), etc.

The clocks on individual cluster nodes might not be synchronized, and due to random
delays, using values returned by any time functions will break cluster consistency. Vrui
provides several methods of querying process or wall-clock times in a cluster-transparent
manner. Developers wanting to use native time functions must take care of cluster
synchronization explicitly.

### Do not change application state asynchronously

Vrui is designed to support application-level concurrency. It does not enforce a static split
into exactly one "simulation thread" and one "rendering thread" like some other toolkits,
but instead makes extensive use of threading internally to the toolkit, and allows
applications to do the same. However, concurrency brings its own set of rules.

Most importantly, background threads must not directly change application state. In a
cluster environment, asynchronous changes will break cluster consistency due to slight
timing or scheduling differences between nodes. In multi-display or multi-view
environments, asynchronous updates will break view consistency, which can lead to severe
ill effects for the user, such as eye strain, headaches, or nausea.

The correct approach for background processing is multi-buffering of any state affected by
background threads (or explicit message passing, for the same effect), and only updating
application state during the synchronous frame method, or its equivalent methods for
tools or vislets. The implicit synchronization points provided by a sequential main loop are
the main reason why Vrui uses one. It is a common misunderstanding that Vrui is single-
threaded due to its sequential frame loop; on the contrary, that loop is intended to be
merely a synchronization point for massively concurrent processing happening in any
number of background threads.


### Do not change application or toolkit state during rendering

Any rendering code, both for graphics using OpenGL and sound using OpenAL, must not
have any side effects on application state, and this includes initialization code executed in
the initContext methods declared by classes GLObject and ALObject (obviously, rendering
code is allowed to change per-context state). Due to differing window and multi-view
setups, an application's display and context initialization methods will be called an
unspecified number of times per application frame, including not at all, and potentially
concurrently from multiple threads. It is therefore imperative that rendering code does not
change application state or toolkit state. The Vrui::Application class enforces part of this by
declaring the display method as const, and GLObject/ALObject do the same with the
initContext method. However, this does not apply to toolkit state, and to devious
developers who use const casts to change object members from within const methods. If
particular functionality absolutely requires changing application state during rendering, the
developer is responsible for using proper mutual exclusion, and being extremely careful.

Vrui and its supporting libraries are carefully designed to indicate whether any class
methods change state using the const modifier. The Vrui kernel API in Vrui/Vrui.h, for
legacy reasons, is not implemented as a class, and can therefore not do that. However, it
should be fairly obvious from its name and description whether any given kernel function
call changes toolkit state or not.

### Do not store OpenGL or OpenAL per-context state in application state

Due to differing window setups, there is no one-to-one relationship between applications
and OpenGL and OpenAL contexts. An application might have to render to any number of
contexts at the same time; the number of contexts might even change dynamically as
windows are created and destroyed at runtime. It might not be possible for these contexts
to share per-context state such as display lists, texture objects, vertex or frame buffers, etc.
An application must therefore be capable of handling any number of independent copies
of its per-context states. To simplify managing multiple copies of per-context data, Vrui
provides the GLObject/GLContextData mechanism for OpenGL state, and the analogous
ALObject/ALContextData mechanism for OpenAL state.

### Do check whether third-party libraries mix application and per-context

### state

Many third-party graphics or sound libraries work under the implicit assumption of running
in standard desktop environments, with only a single OpenGL/OpenAL context per
application, or the assumed ability to share per-context state between multiple contexts.
These libraries will freely intermix application and per-context states, and sometimes this
might be completely hidden underneath the API. Using such libraries without special care
will either not work at all, or lead to unpredictable results. For example, many libraries will
assume that there is a current OpenGL context when a library data structure is created or
initialized. However, Vrui applications will typically initialize their data structures in the
application constructor, before any windows or OpenGL contexts are created. Libraries
querying or setting OpenGL state at that time will crash, or retain bogus state.

Any data structures that intermix application and per-context states must either be
completely treated as per-context state in Vrui, leading to some potential redundancy and


consistency problems as their application states are replicated, or must be split by the
developer into per-application and per-context pieces, if that is possible.

### Do check whether third-party libraries clean up after themselves

Similarly to the previous rule, some third-party graphics or sound libraries -- typically
higher-level libraries such as scene graph toolkits or full game engines -- will assume that
they are the only parties rendering to windows or OpenGL or OpenAL contexts. These
libraries might clear the display buffer before they start, mess with the projection and/or
modelview matrices, assume that their state changes will be retained from one frame to
the next, and so forth. It is the developer's responsibility to ensure that these libraries play
nice with Vrui by following other, previously stated rules about handling rendering
contexts.

## Portability Rules

Vrui aims to support portable applications, i.e., applications that function in any kind of VR
environments from standard PCs to fully-immersive cluster-based multi-screen
visualization facilities. For example, since most software development will happen on the
desktop, and almost all developers learned their trade in the desktop paradigm, it is easy
to let desktop-dependent assumptions or behavior, such as the assumption that an
application uses a single OpenGL window, or has access to a keyboard and a mouse, slip
into an application and limit its portability to other types of environments.

### Do not perform OpenAL or OpenGL context setup

A Vrui application is not the only party rendering to its graphics or sound contexts in the
Vrui runtime environment. Other parties include the tool graph, the GUI widget manager,
any loaded vislets, etc. It is the responsibility of the Vrui toolkit, not the developer, to set
up and initialize OpenGL or OpenAL contexts at the beginning of the rendering cycle.
Application rendering code is called when all rendering contexts are already set up
properly, and other 3D geometry or sound might already have been rendered into them. It
is the developer's responsibility not to change context state permanently, and not to
destroy what has already been rendered, or will be rendered. Concretely, applications must
never change the projection matrix, clear the color or depth buffers, change the viewport
or depth range, leave texture objects or buffers bound, etc. If some functionality requires
to do any of these, it is the developer's responsibility to ensure that the larger Vrui runtime
enviroment is not affected, and that all state changes are properly restored before an
application's rendering functions return.

### Do not assume context state persistence

For the same reason as the previous rule, an application must not assume that changes to
a rendering context will be retained between frames, or even between multiple rendering
passes into the same context. If an application needs a rendering context in a certain state,
it has to set it up accordingly every time its rendering functions are called.

### Do aim to always use the highest possible abstraction level offered by the

### toolkit


Vrui is intentionally designed to allow low-level and bare-metal programming to offer
maximum performance and interoperability between it and second- or third-party code; it
does not force developers to use any toolkit APIs besides the (small) Vrui kernel API.
However, Vrui offers a comprehensive set of lower-level abstractions, which are designed
to integrate well with the rest of the toolkit. Developers are encouraged to use these
abstractions whenever possible. The main benefits of using Vrui-provided abstractions are
increased possibility of code reuse between multiple applications or multiple developers;
additional features such as built-in cluster transparency; often improved performance; and
hopefully fewer bugs because toolkit-provided functionality is used and checked by more
people.

### Do not change Vrui display environment layout programmatically

While it is possible to change certain aspects of the layout of a Vrui display environment,
such as viewer or screen posititions, this must never be done by applications. Environments
are configured by the user or a dedicated environment administrator such that Vrui's
notion of an environment's layout exactly matches reality. Any changes performed by an
application will lead to discrepancies between virtual and real layout, which can cause
severe ill effects for the user, including eye strain, headaches, and nausea. As an important
example, this means that it is impossible for an application to change a display's field of
view programmatically. In VR, field of view is fully determined by physical layout; to change
the field of view, the user has to move towards or away from the display.

The only exception to this rule are, obviously, dedicated calibration utilities whose sole
purpose it is to guide a user in configuring an environment in the first place.

### Do understand the relationship between physical and navigational space

Vrui works in two coordinate spaces: physical space and navigational space. Navigational
space plays the same role as model space in other 3D toolkits; it is the space in which
application-specific data lives. Navigation space can have arbitrary, application-defined
orientations and measurement units. Physical space, on the other hand, is bound to the
display hardware of a particular VR environment; it is the space in which a user lives. It can
still have arbitrary orientations and measurement units, but it expresses the layout of a VR
environment in the real world.

The mapping between physical space and navigational space is expressed by
the navigation transformation, which has a similar purpose as the modelview matrix in
typical OpenGL applications. The navigation transformation is also the means how Vrui
changes viewpoints, or how users navigatethrough their 3D data (hence the name). Any
changes to the viewpoint in Vrui applications are expressed by changes to the navigation
transformation. This is typically done by dedicated navigation tools, which are
generalizations of the viewpoint manipulation metaphors typically used in desktop 3D
applications, such as virtual trackballs.

The introduction of a third rendering matrix, on top of OpenGL's projection and modelview
matrices, was necessary because the projection and modelview matrices are based on a
camera model, where the position and the orientation of the camera are programmatically
controlled by applications. In VR, on the other hand, there is no camera. It is the user
himself who observes the 3D scene, and the user can change his position and orientation


outside of any application's control, by physically moving through the VR display
environment. While this additional freedom can be expressed through the canonical two-
matrix setup (after all, the product of two matrices is a matrix), and many other VR toolkits
do just that, it is much more straightforward and intuitive, for both users and developers,
to use the concept of navigational space. It is not the camera that moves, but it is the
entire physical display environment that moves through navigational space, and the virtual
3D model defined therein.

In a nutshell, applications use their own, arbitrary navigational space to position their 3D
models, and let Vrui and the user worry about how to map that space to Vrui's physical
space. The rendering, modelling, simulation, etc. aspects of an application will typically
never reference anything but navigational space. User interaction, on the other hand, is
often expressed at least partially in physical space, because that is the space in which the
display environment is defined, and through which the user moves.

### Do not make assumptions about physical space

In Vrui, the layout of physical space is not defined by the toolkit, but by whoever
configured a particular environment. Applications should therefore never make any
assumptions about physical space. In particular, developers must not assume that...

```
... the origin of the physical coordinate system is the center of the screen/display. Use
Vrui::getDisplayCenter() to query the physical-space position of the current display
system's center point.
... the measurement unit of the physical coordinate system is inches, or meters, or
anything. Use Vrui::getInchFactor() or Vrui::getMeterFactor() to query the length of an
inch or a meter, respectively, in physical coordinate units.
... the y axis (or z axis) points "up." Use Vrui::getUpDirection() to query the "up"
direction as a vector in physical coordinates.
... the z axis (or y axis) points "forward." Use Vrui::getForwardDirection() to query a
vector in physical coordinates that points "into" a display environment.
... physical space is constant over the lifetime of an application. For example, in the
default desktop setup, the center point and size of the display environment change as
the application's main window is moved or resized. Always query information about
physical space directly from Vrui when it is needed.
```
There is only one safe assumption about physical space: just like navigational space,
physical space is always right-handed.

### Do not directly reference screens, viewers, windows, or input devices

Unlike in the desktop world, there is no canonical keyboard/video/mouse input/output
model in VR. VR environments have multiple screens, multiple views per screen, moving
screens, potentially multiple head tracked viewers, and multiple input devices of widely
varying capabilities and layouts. Vrui is carefully designed to shield application developers
from this extremely diverse design space, but developers have to cooperate and use the
abstraction mechanisms Vrui provides.

Display abstraction is achieved relatively easily; Vrui applications simply render into
graphics or sound rendering contexts that have already been properly set up by the toolkit,


including their projection and modelview matrices. Only very specific applications will ever
need to know anything about a VR environment's display layout.

Input abstraction is harder to get used to for developers, because it is the area where VR
differs most from the desktop world, and unlike 3D rendering it is still an active research
area. It is also the area in which Vrui differs most from other VR toolkits. Vrui's general
approach is to completely decouple applications from input devices. Other VR toolkits offer
"virtualized input devices," which effectively hide differences in hardware implementations
between different models of the same type of device (such as different brands of data
gloves), but that do not hide differences between completely different device types, such
as a mouse and a position-tracked data glove. A VR device driver might successfully
manage to cast those two into the same data structure, but that does not mean that they
should be treated the same by an application. Boldly speaking, an application that does not
distinguish between a mouse and a data glove will be unusable on both.

While Vrui uses the same virtualized device model to hide minor implementation
differences, this is just a starting point for the real abstraction, which is the tool model. In
Vrui, tools are smallish objects that map from physical (albeit virtualized) input devices to
application functionality. Tools increase functionality and portability by being an
independent intermediate layer between input devices and applications. For example, Vrui
contains about two dozen navigation tools, which implement different metaphors how
users can use devices to move through an application's 3D space. Why does it need so
many, when desktop 3D applications can typically make do with one or two? The answer is
that the tools have to account for both the capabilities of available input devices, and also
for personal preferences of the user, which can change between and even within
applications. For example, there are multiple navigation tools for mice, implementing
different functionality. One allows free 3D navigation, while another constrains navigation
to an application-defined surface. Typical users will use both in turn, depending on what
they want to do at any given moment. On the other hand, there are multiple navigation
tools which implement basically the same functionality, but for different devices.

The bottom line for application developers is that they don't have to worry about this, as it
is taken care of by the toolkit. They can just develop their application to react to higher-
level events, such as a change in the navigation transformation effected by any navigation
tool, and leave the details to a given VR environment's users or administrator. Of course,
this requires that developers don't punch through this useful abstraction layer by talking
directly to the devices themselves. This will forbid users to adapt the application to their
particular environment, and to their liking. In Vrui, tools are bound to input devices not
programmatically, but dynamically and interactively by the users.

## Usability Rules

Vrui itself strives to be as usable as possible on any environment in which it is run. While
Vrui and Vrui applications are, or should be, written with no particular target environment
in mind, ideally, a Vrui application running on a desktop system should be exactly as usable
as a native desktop application, and a Vrui application running in a CAVE should be as
usable as a native CAVE application.

In Vrui, across-environment usability is addressed by the tool system, which decouples
application or toolkit functionality from input device hardware present in a given


environment. Therefore, understanding and properly using the tool infrastructure is
essential for developing effective Vrui applications.

### Do not block Vrui's foreground thread

As a corollary to the previous rule, it follows that longer-running operations must never be
performed synchronously in the frame loop. In desktop environments, it is not a big
problem if an application's display freezes for a few seconds; in VR environments, which
need to render frames rapidly in response to head-tracked users to support the illusion of
holographic 3D objects, even very short delays cause severe degradations in user
experience and potentially severe ill effects for the user.

### Do mind your application's frame rate

Rendering frame rate is of paramount importance for VR applications. Unlike in desktop
environments, VR applications have to display updated frames reflecting continuously
changing viewing conditions upwards of 30 times per second, ideally upwards of 60 times
per second, to avoid severe ill effects for users. VR applications typically require advanced
deadline-driven rendering methods utilizing view-dependent and multi-resolution
approaches to achieve the necessary frame rates independent of data sizes or hardware
performance.

Desktop-based developers need to remember that VR environments will often have
reduced rendering performance compared to desktops, because they might have more
and/or higher-resolution displays, or need to render everything multiple times per frame to
achieve stereoscopy. It is therefore imperative to test applications often, either in real VR
environments, or in simulated ones using multiple windows and/or multiple views per
window on a desktop system.

### Do not mess with the navigation transformation

Changing the navigation transformation, i.e., changing the mapping from an application's
model space into an environment's physical space, is the purview of dedicated navigation
tools, which do so under direct control by the user. An application should generally not
take navigation control away from the user. There is usually a single place in an application
where it changes the navigation transformation: it has to set up the initial navigation
transformation, which will typically map the entirety of an application's 3D data into the
display space of a VR environment. This is usually done either during the application's
initialization, or in a GUI callback as response to a main menu entry such as "reset
navigation."

That said, there are mechanisms in Vrui to allow applications to constrain the navigation
transformation, for example, keeping the user upright in a 3D model with arbitrary or
changing "up" directions, letting a user "walk" on an application-defined surface, or
implementing general collision handling. These mechanisms are implemented via hooks
between an application and so-called surface navigation tools, and do not negatively affect
user experience.

### Do not force a particular scale factor


Many applications, such as architecture walk-throughs or video games, have an inherent
scale and should typically be displayed at the correct 1:1 scale factor. For example, if an
application's navigation space is measured in meters, it is often desirable to scale
navigational space such that one meter in the model corresponds exactly to one meter in
the displaying environment's physical space. However, an application should never limit
the user to this 1:1 scale, or any other fixed scale factor. Applications are free to display
their models at any desired scale when initially started, but they should not artificially
constrain the user to that scale during interactions. The navigation tools typically used in
fixed-scale applications do not change scale; therefore, any scale changes will have been
requested by the user explicitly and should not be denied. Instead, applications should do
two things:

```
1. They should tell Vrui's coordinate manager what unit of measurement is used within
their navigational space. This will make Vrui's scale bar work as intended, and will
allow users to go to 1:1 scale (or any other desired scale) by interacting with the scale
bar. Additionally, this will make Vrui's measurement tools report measurements in the
correct unit.
2. They should offer a user interface mechanism to quickly move to the most
appropriate scale, typically 1:1, for example via a button in the application's main
menu. The callback hooked into this button should scale the current navigation
transformation such that the resulting scale factor is the desired one. Applications
need to calculate the proper scale factor by querying the length of some unit of
measurement in physical space, and then dividing by the length of the same unit in
navigational space. The Vrui kernel offers the functions Vrui::getInchFactor() and
Vrui::getMeterFactor() to query the length of an inch and a meter, respectively, in
physical space. Here is some example code to go to 1:1 scale while keeping the center
point of the environment fixed, assuming that the navigational space unit is 1 meter:
/* Get the current navigation transformation: */
Vrui::NavTransform nav=Vrui::getNavigationTransformation();
/* Calculate the new scale factor (assuming nav space unit is 1 meter): */
Vrui::Scalar newScale=Vrui::getMeterFactor()/Vrui::Scalar(1);
/* Change scale by scaling around the center of the environment: */
nav.leftMultiply(Vrui::NavTransform::scaleAround(Vrui::getDisplayCenter(),newScale/nav.getScaling()));
/* Apply the new navigation transformation: */
Vrui::setNavigationTransformation(nav);
```
### Do express all user interactions in physical space

As mentioned before, in Vrui environments, users live in physical space. Therefore, any
calculations affecting users or user interaction must also be performed in physical space.
For example, a picking radius for 3D object selection must not be set in navigational space,
where it would change with the scale factor between navigational and physical space as
users zoom in or out. Depending on the current zoom factor, a fixed value might either be
too small to be usable, or too large to be useful. Instead, user-related parameters such as
font or glyph sizes, interaction distances, etc. must be defined and manipulated in physical
space, and be converted to navigational space based on the current navigation
transformation's scale factor as needed.


For this reason, Vrui tools, which are meant to interact with users, are set up in physical
space. Unlike application rendering code, which is executed in navigational space, tool
code renders in physical space, and receives any input device data in physical space as well.

### Do not hardcode interaction parameters

Even if user-related parameters are expressed in physical space, in accordance with the
previous rule, they must still not be hardcoded. Depending on a particular environment's
physical layout, sizes that work well in one, don't in another. A font that looks the right size
on a 20" desktop monitor will be tiny and unreadable in a 10'x10' CAVE. More importantly,
physical space can use arbitrary units of measurement. A constant of 5 might mean 5
inches in one environment, and 5 meters in another. While the Vrui kernel offers functions
to query physical measurement units (by returning the length of one meter or one inch in
physical units), user-related physical-space sizes must still never be hardcoded, but always
be based on layout parameters provided by Vrui itself. For example, Vrui provides functions
to query base sizes for UI components, fonts, glyphs, and interaction distances (both point
distances and ray angles). Applications can use hard-coded multipliers for those toolkit-
provided variables to achieve desired object sizes.

### Do not hardcode tool bindings

The bindings from tools to input devices described in the previous rule are not only
environment-specific, but also user-specific (an assignment ideal for a right-handed user
might not work for a left-handed user). For this reason, in addition to never talking to input
devices directly, applications must also never programmatically bind tools to particular
input devices, unless this is in turn driven by user interaction. It might be tempting to
hardcode tool bindings during development do shave off the extra ten seconds it takes to
set up a tool environment when an application is run (and then crashes) repeatedly during
debugging, but it kills portability and usability. Vrui offers the ability to save and load
complete input graphs for exactly this purpose.

### Do not implement custom tool classes that require configuration

Vrui's tool system allows anyone – users, developers, or third parties – to create their own
custom tools, which are then managed by Vrui exactly like standard tools. Application-
specific custom tools are an important mechanism to expose application functionality to
the rest of the Vrui tool stack, to enable useful new interaction methods. However, there
are some common pitfalls to avoid.

Tool classes, and particular tool instances, are typically configured at creation time via
Vrui's configuration files. However, tool classes must not require a configuration file section
to be present; they must contain default configurations that make tools function in any VR
environment. This is particularly important for third-party or application-specific tools,
where users cannot be expected to know how to configure a tool that is only used for an
obscure function inside a particular application, but that makes the application crash or not
work if it is not properly set up.

If certain tool settings cannot have reasonable defaults (such as the host name of a server
to connect to, the name of a file to load, or the name of a particular hardware device on
the host computer), the tool must offer a way to select proper values interactively when a


tool object is instantiated, for example by opening a file selection dialog. If this still is not
possible, then the intended functionality should not be implemented as a tool, but as
another mechanism, probably a vislet, or the combination of a vislet and an associated
tool.


