# GLContextData --- GL 上下文数据

Tags: Vrui

[TOC]

One of the main goals of the Vrui toolkit is to support development of truly portable VR software. This means a VR application is written once, and then runs on any VR environment without change including desktop environments, single-computer VR environments, and even multi-pipe or cluster-based VR environments. The GLContextData class (and its sibling, the GLObject class) provide a framework to hide differences between single-pipe and multi-pipe or cluster-based OpenGL rendering from a developer.
> Vrui工具包的主要目标之一是支持开发真正便携式VR软件。这意味着VR应用程序被写入一次，然后在任何VR环境中运行，包括桌面环境，单机VR环境，甚至是多管道或基于群集的VR环境。GLContextData类（及其同级，GLObject类）提供了一个框架来隐藏来自开发人员的单管和多管或基于群集的OpenGL渲染之间的差异。

The exact problem that GLContextData/GLObject try to hide is how to store per-context OpenGL data in an application that might run in a single- or multi-pipe environment depending on from where it is started. Take, as an example, an application that renders a surface as an indexed triangle set with a texture mapped onto it. If the state related to this task is encapsulated in a single class, this class might look like the following:
> GLContextData/GLObject尝试隐藏的确切问题是如何将每个上下文的OpenGL数据存储在可能在单管道或多管道环境中运行的应用程序中，具体取决于从哪里开始。作为一个例子，将一个表面渲染为一个带有纹理映射到其上的索引三角形集合的应用程序。如果与此任务相关的状态封装在单个类中，则此类可能如下所示：

```
class IndexedTriangleSet
{
...
Vertex* vertices; // Array of vertices
GLuint* indices; // Array of triangle vertex indices
GLuint textureObjectId; // ID of the texture object holding the surface texture
...
void render(void); // Renders the triangle set
};
```

The problem is that this class (and the application using it) would only work in a single-pipe environment. Depending on the architecture of the underlying system, it is not guaranteed that OpenGL objects (such as texture objects) will have the same IDs across different OpenGL contexts. In other words, if the above class is supposed to work in a multi-pipe environment, there are two approaches: (1) replicate entire IndexedTriangleSet objects for each context, or (2) store more than one texture object ID in each IndexedTriangleSet object. The first approach wastes resources because the vertex and triangle data live in the application's address space and can be shared between OpenGL contexts; the second approach is annoying because the programmer has to take care to allocate the proper number of object IDs, and ensure that the texture is uploaded into each OpenGL context separately. For a programmer who does not really anticipate ever using a multi-pipe system, both approaches are wasted effort, leading to many VR applications that will not run in multi-pipe VR environments.
> 问题是这个类（和使用它的应用程序）只能在单管环境中工作。根据底层系统的体系结构，不能保证OpenGL对象（如纹理对象）在不同OpenGL上下文中具有相同的ID。换句话说，如果上述类应该在多管道环境中工作，则有两种方法：（1）为每个上下文复制整个IndexedTriangleSet对象，或（2）在每个IndexedTriangleSet对象中存储多个纹理对象ID 。第一种方法浪费资源，因为顶点和三角形数据存在于应用程序的地址空间中，可以在OpenGL上下文之间共享;第二种方法是令人厌烦的，因为程序员必须注意分配适当数量的对象ID，并确保将纹理分别上传到每个OpenGL上下文中。对于没有真正期待使用多管道系统的程序员，这两种方法都是浪费的努力，导致许多VR应用程序不能在多管道VR环境中运行。

## Separation of Per-Application and Per-Context State --- 每个应用程序和每个上下文状态的分离

The approach embodied by GLContextData/GLObject is to separate per-application state from per-context state, and to provide a mechanism to associate per-context state with an application when that state is needed for rendering. Fundamentally, any class using per-context state, such as display list indices, is derived from the GLObject base class, and stores per-context state in an embedded helper class, derived from GLObject::DataItem. The IndexedTriangleSet class, reformulated to work with GLContextData, would look like the following:
> GLContextData/GLObject体现的方法是将每个应用程序状态与每个上下文状态分开，并提供一种机制，以便在需要该状态进行呈现时将每个上下文状态与应用程序相关联。从根本上说，使用每个上下文状态的任何类，如显示列表索引，都是从GLObject基类派生的，并且在GLObject::DataItem派生的嵌入式助手类中存储每个上下文的状态。IndexedTriangleSet类，重新配置为使用GLContextData，将如下所示：

```
class IndexedTriangleSet:public GLObject
{
...
struct DataItem:public GLObject::DataItem // Structure containing per-context data
{
GLuint textureObjectId;
DataItem(void); // Creates any per-context state
virtual ~DataItem(void); // Destroys all per-context state
};
...
Vertex* vertices; // Array of vertices
GLuint* indices; // Array of triangle vertex indices
...
virtual void initContext(GLContextData& contextData) const; // Creates per-context data
void render(GLContextData& contextData) const; // Renders the triangle set
...
};
```

After a class' state has been separated into per-application and per-context data, a developer does not have to know how many OpenGL contexts are used for rendering. The application will create one IndexedTriangleSet object, and initialize its per-application state. During rendering, that object's render() method will be called for every OpenGL context used by the application, each time using a different GLContextData object. The IndexedTriangleSet object will query its per-context state related to the current OpenGL context from the GLContextData object. In other words, the same IndexedTriangleSet object will see different per-context data, depending on which OpenGL context it is currently rendered in. This is also the reason why the render() method is declared const -- since the render() method will be called an unkown number of times, and possibly concurrently, it is not allowed to change per-application state from inside that method. Per-context state, however, can be changed -- that is why the GLContextData object passed into the render() method is not declared const.
> 在一个类的状态已经被分解为每个应用程序和每个上下文数据之后，开发人员不必知道用于渲染的OpenGL上下文的数量。应用程序将创建一个IndexedTriangleSet对象，并初始化其每个应用程序的状态。在渲染过程中，每次使用不同的GLContextData对象时，应用程序使用的每个OpenGL上下文都会调用该对象的render（）方法。IndexedTriangleSet对象将从GLContextData对象中查询与当前OpenGL上下文相关的每个上下文状态。换句话说，相同的IndexedTriangleSet对象将看到不同的每个上下文数据，这取决于当前渲染的OpenGL上下文。这也是为什么render（）方法被声明为const的原因 - 因为render（）方法将被称为unkown次数，并且可能并发，不允许从该方法内部更改每应用程序状态。然而，每上下文状态可以被更改-这就是为什么传递给render（）方法的GLContextData对象不被声明为const。

One related problem with multi-pipe rendering is when to initialize and release per-context state. Since it is not allowed to change application state from inside a render() method, an application can only create new objects from some other method, for example an event callback. That means that per-context state must be initialized right before an object is rendered first in each context it is rendered in. Releasing per-context resources is an even bigger problem: Once an object has been deleted from somewhere outside the render() method, it is not available anymore to clean up after itself. The GLContextData method solves both these problems elegantly. Any class derived from GLObject contains a virtual method initContext(). This method is called right before the first time a new object is rendered in each OpenGL context. Inside of it, the application will typically create a new DataItem object, and store it in the passed GLContextData object (to later be retrieved in the render() method). If an object derived from GLObject is destroyed, the destructor of GLObject will ensure that any DataItem object belonging to it in any OpenGL context will be destroyed the next time that OpenGL context is made current for rendering.
> 多管渲染的一个相关问题是什么时候初始化和释放每个上下文的状态。由于不允许从render（）方法内部更改应用程序状态，应用程序只能从其他方法（例如事件回调）创建新对象。这意味着每个上下文状态必须在对象在渲染的每个上下文中被首先呈现之前被初始化。释放每个上下文资源是一个更大的问题：一旦对象已经从render（）方法之外的某个地方删除，它本身不能再清理。GLContextData方法优雅地解决了这两个问题。从GLObject派生的任何类都包含一个虚函数initContext（）。在第一次在每个OpenGL上下文中呈现新对象之前调用此方法。在其内部，应用程序通常会创建一个新的DataItem对象，并将其存储在传递的GLContextData对象中（以后在render（）方法中检索）。如果从GLObject导出的对象被销毁，则GLObject的析构函数将确保在下一次将OpenGL上下文当作渲染时，任何OpenGL上下文中属于它的任何DataItem对象都将被销毁。

## Proper Procedure --- 合适程序

To ensure proper handling of per-context resources, the following procedure is required:
> 为了确保对每个上下文资源的正确处理，需要以下过程：

1. Any class that has per-context state must be derived from GLObject.
> 1 . 任何具有每个上下文状态的类必须从GLObject派生。

2. Any per-context state of the class must be separated into an embedded DataItem structure derived from GLObject::DataItem.
> 2 . 类的任何每个上下文状态必须分成从GLObject::DataItem派生的嵌入式DataItem结构。

3. The DataItem constructor allocates OpenGL resources (texture objects, vertex buffers, etc.), but does not necessarily have to initialize those resources. Example:
> 3 . DataItem构造函数分配OpenGL资源（纹理对象，顶点缓冲区等），但不一定必须初始化这些资源。例：

```
IndexedTriangleSet::DataItem::DataItem(void)
:textureObjectId(0)
{
glGenTextures(1,&textureObjectId);
}
```

4. The virtual DataItem destructor releases all allocated OpenGL resources. At the time when the destructor is called, the parent object has already been destroyed. The DataItem class must retain enough state to release all OpenGL state that was allocated in the DataItem object's constructor, and initialized in the parent object's initContext() method. Example:
> 4 . 虚拟DataItem析构函数释放所有分配的OpenGL资源。在调用析构函数时，父对象已被破坏。DataItem类必须保留足够的状态来释放在DataItem对象的构造函数中分配的所有OpenGL状态，并在父对象的initContext（）方法中初始化。例：

```
IndexedTriangleSet::DataItem::~DataItem(void)
{
glDeleteTextures(1,&textureObjectId);
}
```

5. The IndexedTriangleSet constructor creates per-application state, e.g., allocates and initializes the vertex and index arrays.
> IndexedTriangleSet构造函数创建每个应用程序的状态，例如，分配和初始化顶点和索引数组。

6. The IndexedTriangleSet destructor destroys all per-application state.
> IndexedTriangleSet析构函数会破坏所有的每个应用程序的状态。

7. The IndexedTriangleSet initContext() method creates a data item, stores it in the GLContextData, and initializes the OpenGL resources. Example:
> IndexedTriangleSet initContext（）方法创建一个数据项，将其存储在GLContextData中，并初始化OpenGL资源。例：

```
void IndexedTriangleSet::initContext(GLContextData& contextData) const
{
/* Create a new data item: */
DataItem* dataItem=new DataItem();
/* Associate object and data item in GLContextData: */
contextData.addDataItem(this,dataItem);
/* Read and upload texture image into dataItem->textureObjectId: */
glBindTexture(GL_TEXTURE_2D,dataItem->textureObjectId);
...
/* Protect texture object: */
glBindTexture(GL_TEXTURE_2D,0);
}
```

8. The IndexedTriangleSet render() method retrieves the data item from the GLContextData, and uses it to render. Example:
> IndexedTriangleSet render（）方法从GLContextData中检索数据项，并使用它来呈现。例：

```
void IndexedTriangleSet::render(GLContextData& contextData) const
{
/* Retrieve data item from GLContextData: */
DataItem* dataItem=contextData.retrieveDataItem<DataItem>(this);
/* Activate texture object: */
glBindTexture(GL_TEXTURE_2D,dataItem->textureObjectId);
/* Render triangles: */
...
/* Protect texture object: */
glBindTexture(GL_TEXTURE_2D,0);
}
```

## Guarantees --- 担保

Proper use of the GLObject/GLContextData mechanism, as described in the previous section, guarantees the following:
> 如上一节所述，正确使用GLObject/GLContextData机制保证了以下内容：

* Each class using per-context state will have as many copies of that state as there are OpenGL contexts in the application.
* > 每个使用每个上下文状态的类将具有与应用程序中OpenGL上下文相同的副本。

* During initialization and/or rendering, a class will only see the copy of its per-context state that is valid for the current OpenGL context.
* > 在初始化和/或渲染期间，类只会看到其对于当前OpenGL上下文有效的每个上下文状态的副本。

* An object's initContext() methods will be called exactly once for each OpenGL context used by an application, before any user code could use that object for rendering in that context. However, there is no guarantee that the initContext() method will be called for all OpenGL contexts in an application, before an object could be used for rendering in any OpenGL context. Used of an object related to multiple contexts could occur in any order, or even concurrently.
* > 对于应用程序使用的每个OpenGL上下文，对象的initContext（）方法将被正确地调用一次，然后任何用户代码才能使用该对象在该上下文中呈现。但是，在使用任何OpenGL上下文渲染对象之前，不能保证对应用程序中的所有OpenGL上下文都会调用initContext（）方法。用于与多个上下文相关的对象可以以任何顺序发生，甚至可以并发。

* The destructors of an object's DataItem objects are called for each OpenGL context in an application, after the object itself has already been deleted. Each DataItem object's destructor is called when the OpenGL context that object was associated with is current.
* > 对象的DataItem对象的析构函数在应用程序中的每个OpenGL上下文被调用后，对象本身已被删除。当与该对象相关联的OpenGL上下文是当前时，每个DataItem对象的析构函数被调用。

### Initialization Sequence in Vrui --- Vrui中的初始化序列

In general, the initContext() method of a newly created object derived from GLObject is called at an unspecified time, but before an application would have a chance to use the object for rendering. However, the sequence of events during Vrui initialization guarantees that the initContext() methods for all GLObject-derived objects created in an application's constructor (or before explicitly calling Vrui's startDisplay() function) are called for all OpenGL contexts used by that application before the first time the application's frame() method is invoked (or before the explicitly invoked startDisplay() function returns). This guarantee is necessary for very specific circumstances in which results obtained during all invocations of initContext() influence the overall behavior of an application, and need to be queried at a specific time.
> 通常，从GLObject派生的新创建的对象的initContext（）方法在未指定的时间被调用，但在应用程序将有机会使用该对象进行呈现之前调用。但是，Vrui初始化期间的事件序列保证在应用程序的构造函数（或显式调用Vrui的startDisplay（）函数之前创建的所有GLObject派生对象的initContext（）方法都被该应用程序使用的所有OpenGL上下文调用首先应用程序的frame（）方法被调用（或者在显式调用startDisplay（）函数返回之前）。对于在所有调用initContext（）中获得的结果影响应用程序的整体行为并且需要在特定时间查询的特定情况，这种保证是必需的。

An example is an application that queries the availability of certain OpenGL extension in its initContext() method, and retains a "minimal set" of supported extensions in all used OpenGL contexts. It then checks for this minimal set during the first invocation of the frame() method, and changes its overall behavior accordingly, for example by preparing application-wide data structures needed to fall back to an alternative rendering method. It is important to remember that each object's initContext() method will be invoked an unknown number of times, once per OpenGL context, in no particular order and sometimes concurrently. Therefore, special care needs to be taken that any changes to per-application state from inside the initContext() method are reentrant and thread-safe (which is why the initContext method is declared const).
> 一个例子是在其initContext（）方法中查询某些OpenGL扩展的可用性的应用程序，并且在所有使用的OpenGL上下文中保留了一个支持的扩展的“最小集”。然后，它会在首次调用frame（）方法时检查该最小集合，并相应地更改其整体行为，例如通过准备应用程序范围的数据结构，以回溯到另一种渲染方法。重要的是要记住，每个对象的initContext（）方法将被调用一个未知的次数，一次每个OpenGL上下文，没有特定的顺序，有时并发。因此，需要特别注意，initContext（）方法内的每个应用程序状态的任何更改都是可重入和线程安全的（这就是为什么initContext方法被声明为const）。

Here is an example of a Vrui application checking for the availability of a particular OpenGL extension in all used contexts:
> 以下是Vrui应用程序的示例，用于检查所有使用的上下文中特定OpenGL扩展的可用性：

```
class Test:public Vrui::Application,public GLObject
{
/* Embedded classes: */
private:
struct DataItem:public GLObject::DataItem
{
...
};
/* Elements: */
mutable bool hasVertexBufferObject; // Flag whether all used OpenGL contexts support GL_ARB_vertex_buffer_object
mutable Threads::Mutex counterMutex; // Mutex protecting the counter element
mutable int counter; // Element counting how many contexts support the extension (example of non-trivial change)
bool firstFrame; // Flag true on the first time frame() is invoked
/* Constructors and destructors: */
public:
Test(int& argc,char**& argv,char**& appDefaults);
/* Methods: */
virtual void initContext(GLContextData& contextData) const;
virtual void frame(void);
virtual void display(GLContextData& contextData) const;
};
Test::Test(int& argc,char**& argv,char**& appDefaults)
:Vrui::Application(argc,argv,appDefaults),
hasVertexBufferObject(true), // Is global "all" operation; has to be initialized to true
counter(0),
firstFrame(true)
{
/* Initialize application: */
...
}
void Test::initContext(GLContextData& contextData) const
{
/* Create and register data item: */
...
/* Check for extension: */
if(GLARBVertexBufferObject::isSupported())
{
/* Increase counter; requires locking due to possible race condition: */
{
Threads::Mutex::Lock counterLock(counterMutex);
++counter;
}
}
else
{
/* Set per-application variable to false; no locking required due to write-only access: */
hasVertexBufferObject=false;
}
/* Initialize per-context state: */
...
}
void Test::frame(void)
{
/* Check if first frame: */
if(firstFrame)
{
/* Check if extension is supported: */
if(hasVertexBufferObject)
{
/* Do something: */
...
}
else
{
/* Do something else: */
...
}
/* Don't do this again: */
firstFrame=false;
}
/* Do per-frame operations: */
...
}
void Test::display(GLContextData& contextData) const
{
/* Retrieve data item: */
...
/* Check for rendering path: */
if(hasVertexBufferObject)
{
/* Render one way: */
...
}
else
{
/* Render another way: */
...
}
}
```

This scenario should be considered a special case. Normally, it is much more appropriate to handle decisions depending on capabilities of an OpenGL context, such as whether vertex buffer objects should be used, on a per-context basis from within the initContext() method and any rendering code. All state and initialization related to alternative rendering paths should be stored in the DataItem object, and paths should be selected on a per-context basis during display. The benefit is that applications running in a heterogeneous rendering environment (such as multiple graphics cards of different brands/models) could use the optimal rendering path for each card. In the IndexedTriangleSet example class introduced above, the embedded DataItem class could contain a flag whether its associated OpenGL context supports vertex buffer objects, and could use them whereever available. As a fallback, the class could render straight from the per-application vertex and index arrays.
> 这种情况应该被视为一种特殊情况。通常，根据OpenContext（）方法和任何呈现代码的每个上下文的方式处理决定，这取决于OpenGL上下文的能力，例如是否应该使用顶点缓冲区对象。与替代渲染路径相关的所有状态和初始化应存储在DataItem对象中，并且在显示期间应该根据每个上下文选择路径。好处是在异构渲染环境中运行的应用程序（例如不同品牌/型号的多个显卡）可以为每张卡片使用最佳渲染路径。在上面介绍的IndexedTriangleSet示例类中，嵌入式DataItem类可以包含一个标志，无论其关联的OpenGL上下文是否支持顶点缓冲对象，并且可以使用它们。作为一个回退，类可以直接从每个应用程序的顶点和索引数组。


