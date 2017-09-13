# Library Overview --- 各个库概况

Tags: Vrui

[TOC]

## Misc -- Miscellaneous Library --- 杂项

Contains assorted abstraction classes such as error handling, file I/O, containers, configuration file management, etc. Used extensively throughout the higher-level libraries.
> 包含各种抽象类，如错误处理，文件I / O，容器，配置文件管理等。广泛应用于更高级的库。

### SizedTypes.h

SizedTypes.h contains namespace-global type declarations for integer and floating-point types of well-defined word sizes, to guarantee matching types across binary files and network connections.
> SizedTypes.h包含用于定义明确的字大小的整数和浮点类型的全局类型命名空间声明，以保证跨二进制文件和网络连接的匹配类型。

### Utility.h

Utility.h contains namespace-global functions implementing common tasks, such as swapping variables.
> Utility.h包含实现常见任务的命名空间全局函数，如交换变量。

### PrintIntegers.h

PrintIntegers.h contains low-level namespace-global functions to print numbers of all integer types.
> PrintIntegers.h包含低级全局命名空间函数，用于打印所有整数类型的数字。

### StringPrintf.h

StringPrintf.h contains a namespace-global function to print into a C++ string using a printf-style interface.
> StringPrintf.h包含一个命名空间全局函数，使用printf风格的界面打印到C字符串中。

### SelfDestructPointer.h

A simple wrapper class around regular pointers that destroys a new()-created object when the pointer object goes out of scope. Light-weight alternative to Misc::Autopointer to temporarily hold dynamically-allocated objects in contexts when an exception or other error handling might exit the current scope unexpectedly.
> 围绕常规指针的简单包装器类，当指针对象超出范围时，会破坏一个新创建的对象。Misc::Autopointer的轻量级替代方案，当异常或其他错误处理可能意外退出当前范围时，临时保存上下文中动态分配的对象。

### SelfDestructArray.h

Companion to Misc::SelfDestructPointer for new[]-allocated arrays of objects.
> 伴随Misc::SelfDestructPointer为新的[]  - 分配的对象数组。

### RefCounted.h

RefCounted is a base class for reference-counted objects to implement simple automatic garbage collection. It works closely with the Autopointer class.
> RefCounted是引用计数对象的基类，用于实现简单的自动垃圾收集。它与Autopointer类紧密合作。

### Autopointer.h

Autopointer is a class implementing reference-counting pointers for automatic garbage collection. It works closely with the RefCounted class.
> Autopointer是一个实现引用计数指针的类，用于自动垃圾收集。它与RefCounted类紧密合作。

### RefCountedArray

RefCountedArray is a class to represent shared C-style fixed-size arrays using a copy-on-write mechanism. This version of the class does not support sharing arrays between multiple threads.
> RefCountedArray是使用写时复制机制来表示共享C风格的固定大小数组的类。该版本的类不支持在多个线程之间共享数组。

### FixedArray.h

FixedArray is a low-overhead wrapper class to represent 1-dimensional arrays of compile-time fixed sizes.
> FixedArray是一个低开销的封装类，用于表示编译时固定大小的1维数组。

### ArrayIndex.h

ArrayIndex is a class for n-dimensional multi-indices, templatized by dimension. ArrayIndex is primarily used to index array elements in the Array class. It provides convenient constructors, index comparison, and index arithmetics.
> ArrayIndex是由维度模板化的n维多索引的类。ArrayIndex主要用于对Array类中的数组元素进行索引。它提供了方便的构造函数，索引比较和索引算术。

### Array.h

Array is a class to represent n-dimensional arrays, templatized by the type of array element and the array dimension. It uses the ArrayIndex class to index array elements.
> Array是一个表示n维数组的类，由数组元素的类型和数组维度进行模板化。它使用ArrayIndex类来索引数组元素。

### ChunkedArray.h

ChunkedArray is a class to represent dynamically growing arrays, implemented as a linear list of "chunks" containing a fixed number of elements. It is templatized by the type of array element, and the size of each chunk in bytes (meant to align chunk size to the computer's page size). It offers growing arrays without the copy passes used by std::vector, at the expense of not having constant-time random array access. It is a memory-efficient way to implement lists that only allow insertion and removal at the end.
> ChunkedArray是一个表示动态增长的数组的类，实现为包含固定数量元素的“块”的线性列表。它由数组元素的类型和每个块的大小（以字节为单位）进行模板化（意味着将块大小与计算机的页面大小对齐）。它提供了不增加std::vector使用的复制传递的增长阵列，代价是不具有常量随机数组访问。实现列表的记忆高效方法只允许在最后插入和删除。

### ChunkedQueue.h

ChunkedQueue is a class to memory-efficiently represent FIFO queues, implemented as a linear list of "chunks" containing a fixed number of elements. It is templatized by the type of array element, and the size of each chunk in bytes (meant to align chunk size to the computer's page size).
> ChunkedQueue是一个内存有效地表示FIFO队列的类，被实现为包含固定数量的元素的“块”的线性列表。它由数组元素的类型和每个块的大小（以字节为单位）进行模板化（意味着将块大小与计算机的页面大小对齐）。

### PriorityHeap.h

PriorityHeap is a class implementing a priority queue as a heap, templatized by the type of elements stored in the queue, and the type of the comparison functor. PriorityHeap.h also defines a class StdComp, templatized by value type, that uses the <= operator of the value type for comparison. PriorityHeap uses StdComp as the default comparison functor.
> PriorityHeap是一个实现优先级队列作为堆的类，通过存储在队列中的元素的类型进行模板化，以及比较函子的类型。 PriorityHeap.h还定义了一个类型StdComp，它由值类型模板化，它使用值类型的<=操作符进行比较。 PriorityHeap使用StdComp作为默认比较函子。

### PoolAllocator.h

PoolAllocator is a class to efficiently manage dynamic allocation of objects of identical type, such as list elements, by allocating memory in larger, fixed-size "chunks." It is templatized by the type of allocated objects, and the size of each chunk in bytes (meant to align allocation unit size to the computer's page size).
> PoolAllocator是通过在较大的固定大小的“块”中分配内存来有效管理同类型对象的动态分配的类，例如列表元素。它被分配的对象的类型和每个块的大小（以字节为单位）进行模板化（意味着将分配单元大小与计算机的页面大小对齐）。

### StandardHashFunction.h

StandardHashFunction is a functor class to compute hashing functions of arbitrary objects, templatized by the type of object to be hashed. It is used as the default hash function for the HashTable class. StandardHashFunction.h provides a generic implementation for arbitrary data types using a byte-wise CRC, and specialized implementations for all integral data types and for pointers.
> StandardHashFunction是一个函数类，用于计算任意对象的哈希函数，由要散列的对象的类型进行模板化。它被用作HashTable类的默认散列函数。StandardHashFunction.h提供了使用逐字节CRC的任意数据类型的通用实现，以及所有整数数据类型和指针的专用实现。

### StringHashFunctions.h

StringHashFunctions.h contains a specialized Misc::StandardHashFunction class for C++ strings, and a new Misc::StringHashFunction class for C strings.
> StringHashFunctions.h包含一个用于C字符串的专门的Misc::StandardHashFunction类，以及一个用于C字符串的新的Misc::StringHashFunction类。

### OrderedTuple.h

OrderedTuple is a class to represent ordered tuples (of integers) as hash table keys. Parametrized by element type, which must be a signed or unsigned integer type, and tuple dimension.
> OrderedTuple是一个表示有序元组（整数）作为散列表键的类。由元素类型参数化，它们必须是有符号或无符号整数类型和元组维。

### UnorderedTuple.h

UnorderedTuple is a class to represent unordered tuples (of integers) as hash table keys. Two unordered tuples are identical if one is a permutation of the other. Parametrized by element type, which must be a signed or unsigned integer type, and tuple dimension.
> UnorderedTuple是一个表示无符号元组（以整数形式）作为散列表键的类。如果一个是另一个的排列，两个无序的元组是相同的。由元素类型参数化，它们必须是有符号或无符号整数类型和元组维。

### HashTable.h

HashTable is a class implementing hash tables using linked lists for collision management. It is templatized by the hash table key type, the type of elements stored in the hash table, and the hash function functor. HashTable.h contains a specialized implementation for void as element type, essentially implementing a set. HashTable uses StandardHashFunction as the default hash function functor.
> HashTable是一个实现使用链接列表进行冲突管理的哈希表的类。它由哈希表密钥类型，存储在哈希表中的元素的类型和散列函数函子模板化。 HashTable.h包含一个专用的void作为元素类型的实现，基本上实现了一个集合。 HashTable使用StandardHashFunction作为默认散列函数函子。

### OneTimeQueue.h

OneTimeQueue is a class combining a FIFO queue and a hash table to implement queues that allow pushing of any element at most once. It is templatized by the type of element stored in the queue, and the hash function functor. OneTimeQueue uses StandardHashFunction as the default hash function functor.
> OneTimeQueue是组合FIFO队列和散列表以实现最多可以推送任何元素一次的队列的类。它是通过存储在队列中的元素的类型和散列函数函子进行模板化的。 OneTimeQueue使用StandardHashFunction作为默认散列函数函子。

### ThrowStdErr.h

ThrowStdErr contains two single namespace-global function to simplify error management. printStdErrMsg() prints an error message to a static string buffer using a printf-style interface, and throwStdErr() throws a std::runtime_error with a message generated using a printf-style interface.
> ThrowStdErr包含两个单一的命名空间全局函数，以简化错误管理。printStdErrMsg（）使用printf样式接口向静态字符串缓冲区输出错误消息，throwStdErr（）抛出一个std::runtime_error，并使用printf样式接口生成消息。

### Time.h

Time is a wrapper class around the C struct timespec data type. If offers constructors, conversion to struct timeval, and time arithmetic.
> Time是围绕C struct timespec数据类型的包装类。如果提供构造函数，转换为struct timeval和时间算术。

### Timer.h

Timer is a class providing a high-resolution wall-clock timer. It returns the amount of time passed between two timer snapshots as a floating-point number in seconds.
> 定时器是提供高分辨率挂钟计时器的课程。它返回两个定时器快照之间的传递时间，以秒为单位的浮点数。

### MessageLogger.h

MessageLogger is a base class for centralized process-wide message logging facilities with environment-appropriate behavior provided by derived classes.
> MessageLogger是集中的过程级消息记录工具的基类，具有由派生类提供的环境适当行为。

### FunctionCalls.h

FunctionCalls defines several functor classes representing function calls or method invocations as first-class objects.
> FunctionCalls将几个函子类定义为一类对象的函数调用或方法调用。

### CallbackData.h

CallbackData is the base class for all data passed to callbacks implemented as objects of class CallbackList. It contains a pointer back to the originating CallbackList object. CallbackData.h also declares the type of callback functions stored in CallbackList objects.
> CallbackData是传递给CallbackList对象实现的回调的所有数据的基类。它包含指向始发CallbackList对象的指针。CallbackData.h还声明存储在CallbackList对象中的回调函数的类型。

### CallbackList.h

CallbackList is a class managing lists of user-registered callbacks. Callbacks can be C functions or C++ methods, the latter either taking a base CallbackData object, or an object of a class derived from CallbackData.
> CallbackList是管理用户注册回调列表的类。回调可以是C函数或C方法，后者可以使用基本的CallbackData对象，也可以是从CallbackData派生的类的对象。

### TimerEventScheduler.h

TimerEventScheduler is a helper class to implement application-level timer events in GLMotif and Vrui.
> TimerEventScheduler是在GLMotif和Vrui中实现应用程序级定时器事件的辅助类。

### VarInt.h

Functions to read/write unsigned 32-bit integers from/to files using a variable number of bytes, to reduce storage for values that are predominantly very small (such as string lengths).
> 使用可变字节数从/向文件读取/写入无符号32位整数的函数，以减少主要非常小的值（如字符串长度）的存储。

### Marshaller.h

Marshaller is a generic base class to read or write data structures from or to binary data sources or sinks.
> Marshaller是从二进制数据源或汇数据读取或写入数据结构的通用基类。

### StandardMarshallers.h
StandardMarshallers.h contains specializations of Marshaller for atomic data types.
### ArrayMarshallers.h
ArrayMarshallers.h contains Marshaller classes for standard C-style arrays with implicit or explicit array sizes.
### CompoundMarshallers.h
CompoundMarshallers.h contains specializations of Marshaller for standard container data types.
### Endianness.h
Endianness contains a templatized class EndiannessSwapper that swaps the endianness of objects of arbitrary types using byte-wise exchanges and can be specialized to swap the endianness of aggregate data types. Endianness.h also contains a generic function swapEndianness(), templatized by value data type, that is used by client code. The implementation of swapEndianness() is based on EndiannessSwapper.
### GetCurrentDirectory.h
GetCurrentDirectory contains a namespace-global function to retrieve the name of the calling process' current working directory as a string.
### FileTests.h
FileTests contains namespace-global functions query information about file system objects in an operating system-independent manner.
### FileNameExtensions.h
FileNameExtensions contains namespace-global functions to manipulate the extensions of file names.
### CreateNumberedFileName.h
CreateNumberedFileName contains a single namespace-global function to generate unique numbered file names based on a given base name and number of digits. The function searches for all numbered files of the given basename, and creates a new file with a number one higher than the previous maximum. The function has undefined results if called concurrently for the same basename.
### FdSet.h
FdSet is a class representing file descriptor sets, as used by the select() and pselect() system calls. The header file contains wrappers for both functions that do not need the nfds argument.
### Pipe.h
Pipe is a small RAII wrapper around UNIX unnamed pipes.
### File.h
File is a class providing a wrapper around the C standard FILE interface that also provides automatic endianness conversion based on the swapEndianness() function declared in Endianness. It ensures correct resource management via construction/destruction, and offers a type-safe interface to read/write binary data from/to files.
### Directory.h
Directory is an exception-safe wrapper around the POSIX DIR interface.
### LargeFile.h
LargeFile is a different version of the File class, using the same interface but providing access to files larger than 2GB by using 64-bit file offset values.
### MemMappedFile.h
MemMappedFile provides a file wrapper around blocks of memory, such as memory-mapped files. If offers the same API as Misc::File.
### StringMarshaller.h
StringMarshaller contains namespace-global template functions to send/retrieve C or C++ strings via pipe objects that provide type-safe read/write routines, such as Misc::File.
### ValueCoder.h
ValueCoder is a class to encode/decode values of arbitrary types to/from strings, templatized by the type of encoded/decoded values.
### StandardValueCoders.h
StandardValueCoders contains specialized versions of the ValueCoder class for atomic data types and strings.
### ArrayValueCoders.h
ValueCoderArray is a generic class to encode/decode arrays of arbitrary element types to/from strings, using the ValueCoder class to encode/decode each array element.
### CompoundValueCoders.h
CompoundValueCoders contains specialized versions of the ValueCoder class for lists and vectors of arbitrary element types, using the ValueCoder class to encode/decode each container element.
### ConfigurationFile.h
ConfigurationFile is a class to retrieve permanent program state from hierarchical configuration files. Configuration files are human-readable text files comprised of a tree of sections, each section containing tag/value pairs. The ConfigurationFile class allows client code to retrieve tag values either as text, or directly as objects of arbitrary types using the ValueCoder mechanism.
### Optional.h
Optional is a class to represent optional configuration values in configuration files.
### FileLocator.h
FileLocator is a class to simplify searching for files in a list of search directories. Search lists can be built explicity, or by deriving standard resource locations from an application's executable name.

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
