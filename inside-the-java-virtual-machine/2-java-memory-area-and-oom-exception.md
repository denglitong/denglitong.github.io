## 第2章 Java 内存区域与内存溢出异常

- [深入理解Java虚拟机 目录](./index.md)

<em>Java 与 C++ 之间有一堵由内存分配和垃圾回收技术所围成的高墙，墙外面的人想进去，墙里面的人想出来。</em>

### 1. 运行时数据区

**运行时数据区**

```
线程隔离的数据区：
    - 程序计数器 Program Counter Register
    - 虚拟机栈 VM Stack
    - 本地方法栈 Native Method Stack
所有线程共享的数据区：
    - 堆 Heap
    - 方法区 Method Area
```

执行引擎 --> 本地库接口 --> 本地方法库

**Java 虚拟机栈**

每个方法被执行的时候，Java 虚拟机都会同步创建一个栈帧（Stack Frame），用于存储局部变量表、操作数栈、动态连接、方法出口等信息。

通常所说的 Java 内存区的堆和栈，堆就是指运行时数据区的堆 Heap，栈就是指虚拟机栈，或者狭义的指虚拟机栈中的局部变量表部分。

栈帧的**局部变量表**可存放：基本数据类型、对象引用类型（reference 类型，可能是一个指向对象起始地址的引用指针，也可能是指向一个代表对象的句柄）和 returnAddress 类型（指向了一条字节码指令的地址）。这些类型在局部变量表的存储空间中以局部**变量槽**（Slot）来表示。

局部变量表所需的内存空间在编译期间完成分配，一个方法需要在栈帧中需要分配多少个变量槽是完全确定的，在方法运行期间不会改变量槽的数量。其中 64 位长度的 long 和 double 类型的数据会占用两个变量槽，其余类型只占用一个。

如果线程请求的栈深度大于虚拟机栈所允许的深度，将抛出 StackOverflowError 异常如果虚拟机栈可以动态扩展，当栈扩展时无法申请到足够的内存会抛出 OutOfMemoryError异常。

**本地方法栈**

本地方法栈和 Java 虚拟机栈类似，区别在于后者为虚拟机执行 Java 方法（也就是字节码）服务，而前者是为虚拟机使用到的本地（Native）方法服务。

**Java 堆**

Java 堆是虚拟机所管理内存中最大的一块，被所有线程共享，在虚拟机启动时创建，唯一的目的就是存放对象实例。

Java 堆是垃圾收集器管理的内存区域，也称作 GC 堆。

Java 堆可以处在物理上不连续的内存空间，但在逻辑上是视为连续的。在主流的 JVM 上 Java 堆的大小都是可扩展。

**方法区**

方法区用于存储已被虚拟机加载的类型信息、常量、静态变量、即时编译器编译后的代码缓存等数据。

方法区的 GC 很少出现，主要是针对常量池的回收和对类型的卸载。

在 JDK8 以前，方法区被人称为“永久代”，但这只是 HotSpot 虚拟机使用永久代来实现方法区而已，容易遇到内存溢出的问题。到了 JDK8 以后，则完全废弃了永久代的概念，改用了在本地内存（Native Memory）中实现的元空间，来实现方法区。

**本地内存**是供 JVM 自身进程使用的，也称 C-Heap。本地内存的上限由系统的可用空间来控制。当 Java Heap 空间不足时会触发 GC，但本地内存空间不够不会触发 GC。

**方法区 之 运行时常量池**

Class 文件中除了有类的版本、字段、方法、接口等描述信息外，还有一项信息是常量池表（Constant Pool Table），用于存放编译器生成的各种字面量与符号引用，这部分内容将在类加载后存放到方法区的运行时常量池中。

运行时常量池具有动态性，运行期间也可以将新的常量放入常量池中，比如 String 类的 intern() 方法。

**直接内存**（Direct Memory）

直接内存并不是虚拟机运行时数据区的一部分，但这部分内存也被频繁地使用。用于在 NIO（New Input/Output） 类中通过 通道（Channel）与缓冲区（Buffer）的 I/O 方式， 使用 Native 函数库直接分配堆外内存，然后在 Java 堆中存放 DirectByteBuffer 对象作为堆外内存的引用进行操作。

### 2. HotSpot 虚拟机堆对象

<em>HotSpot 虚拟机在 Java 堆中进行对象分配、布局和访问的全过程。</em>

**对象的创建**

Java 虚拟机遇到 new 指令时：

1. 首先检查这个指令的参数能否在常量池中定位到一个类的符号引用
2. 接着检查这个符号引用的类是否已被加载、解析和初始化过
3. 如果没有则先执行相应的类加载过程
4. 在类检查通过后，将会为新生对象分配内存，对象所需内存的大小在类加载完成后便可完全确定

为对象分配内存空间其实就在在堆上划一块区域出来，如果 Java 堆中的内存是绝对规整的，已分配内存在一边，未分配内存在另一边，中间放着一个指针作为分界点的指示器，那分配内存就仅仅是把分界点指针往空闲方向挪动一段距离，这种分配方式称为“指针碰撞”（Bump The Pointer）。

如果 Java 堆并不是规整的，已被使用的内存和空闲的内存交错在一起，那么虚拟机就必须维护一个列表，记录哪些内存卡是空闲的，在分配的时候从列表中查找到一块足够大的空间分配给对象实例，这种分配方式称为“空闲列表”（Free List）。

选择哪种分配方式由 Java 堆是否规整决定，而 Java 堆是否规整由所采用的垃圾收集器是否带有空间压缩整理（Compact）的能力决定。

**内存分配的线程安全问题**

虚拟机中的内存分配是非常频繁的，需要保证操作在并发情况下是线程安全的。一种方法是对分配内存空间的操作进行同步处理——虚拟机采用 CAS（Compare and Swap） 失败重试的方式保证更新操作的原子性；另一种方法是把内存分配的动作按照线程划分在不同的空间中执行，先在 Java 堆中预先分配一小块内存，即本地线程分配缓冲（Thread Local Allocation Buffer, TLAB），只有本地线程的缓冲区用完了，分配新的缓冲区时才需要同步锁定。JVM 可以使用 -XXX: +/-UseTLAB 参数来指定是否开启 TLAB。

内存分配完成后，虚拟机必须将对应的内存空间（除对象头外）都**初始化为零值**，这样程序就能访问到对应数据类型的零值。

对象头设置：对象实例属于哪个类、如果找到类的元数据信息、对象的 GC 分代年龄、对象的哈希码（实际计算延迟到 Object::hashcode() 执行时）、是否加偏向锁等。

Java 虚拟机遇到 invokespecial 指令时：

1. 对构造出来的对象实例进行初始化，执行 Class 文件中的 \<init>() 方法，比如字段都设置为默认的零值，对象需要的其他资源和状态信息按预定设置
2. 执行完成后程序里面一个真正可用的对象才算构造完成

**对象的内存布局**

在 HotSpot 虚拟机中堆内存上的对象的存储布局可划分为：对象头（Header）、实例数据（Instance Data）和对齐填充（Padding）。

**对象头**（Header）包括两类信息:

第一类是用于**存储对象自身的运行时数据**：<em>哈希码、GC 分代年龄、锁状态标志、线程持有的锁、偏向线程 ID、偏向时间戳</em>等，这部分数据的长度在32/64个比特（视虚拟机位数为32/64而定），官方称为“Mark Word”。对象头里的信息是与对象自身定义的数据无关的额外存储成本，Mard Word 被设计成一个有着动态定义的数据结构，以便在极小的空间内存储尽量多的数据。

对象头的另一部分是类型指针，即对象指向它的类型元数据的指针，JVM 通过这个指针来确定该对象是哪个类的实例。不过，并不是所有的虚拟机实现都必须在对象数据上保留类型指针，或者说查找对象的元数据信息并不一定要经过对象自身。此外，如果对象是一个数组，那在对象头中海必须有一块用于记录数组长度的数据。

**实例数据**（Instance Data）:

实例数据部分是对象真正存储的有效信息，即我们在程序代码里所定义的各种类型的字段内容。这部分的存储顺序会受到虚拟机分配策略参数（-XX: FieldsAllocationStyle）和字段在 Java 源码中定义顺序的影响。HotSpot 的默认分配策略总是将相同宽度的字段分配到一起。

**对齐填充**（Padding）:

对象填充仅仅起着占位符的作用，并不是必然存在的。Hotspot 虚拟机的自动内存管理系统要求对象起始地址必须是 8 字节的整数倍，因此如果对象实例数据部分没有补齐的话，就需要通过对齐填充来补全。

**对象的访问定位**

Java 程序会通过栈上的 reference 数据来操作堆上的具体对象。栈上的引用应该通过什么方式去定位、访问到堆中对象的具体位置，是由虚拟机实现而定的。主流方式有使用句柄（Handler）和直接指针（Direct Pointer）两种：

**句柄**

使用句柄访问，Java 堆中可能划分会一块内存来作为句柄池，栈上的引用中存储的就是对象的句柄地址，而句柄中包含了对象实例数据与类型数据各自具体的地址信息。

使用直接指针访问，Java 堆中对象的内存布局就必须考虑如何放置访问类型数据的相关信息，栈上的引用中存储的就是对象的地址，如果只是访问对象本身的话，不需要多一次间接访问的开销。

两种方式各有优势，使用 Handler 的最大好处就是 reference 中存储的是稳定句柄地址，在对象被移动（GC 期间）时之后改变 Handler 中的实例数据指针，而 reference 本身不需要修改。

> 加了一层抽象，问题就容易解决了:-)。

使用 Direct Pointer 来访问最大的好处就是速度更快，节省了一次指针定位的时间开销。由于对象访问在 Java 中非常频繁，因此也是一项极为可观的执行时间优化。就 HotSpot 虚拟机而言，它主要使用 Direct Pointer 方式来访问对象。

**Java 堆溢出**

java.lang.OutOfMemoryError: Java heap space.

当出现 OOM 的时候，第一步应确认到底是出现了内存泄露（Memory Leak）还是内存溢出（Memory Overflow）。

如果是内存泄露，可进一步通过工具查看泄露对象到 GC Roots 的引用链，找到泄露对象是通过怎样的引用路径、与哪些 GC Roots 项关联，才导致垃圾收集器无法回收它们。根据泄露对象的类型信息以及它到 GC Roots 引用链的信息，一般可以比较准确地定位到对象创建的位置，进而找到代码泄露的具体位置。

如果不是内存泄露，即内存中的对象确实都是必须存活的，即内存溢出了，应当检查 JVM 的堆参数设置（-Xmx与-Xms），与机器的内存对比，看看是否还有向上调整的空间。然后再从代码上检查是否存在某些对象声明周期过长、持有状态时间过长、存储结构设计不合理等情况，尽量减少程序运行期的内存消耗。

**虚拟机栈和本地方法栈溢出**

HotSpot虚拟机并不区分虚拟机栈和本地方法栈，并且 HotSpot 虚拟机不支持栈内存的动态扩展。将线程请求的栈深度大于虚拟机所允许的最大深度时，将抛出 StackOverflowErro 异常，当虚拟机允许栈内存动态扩展但取法申请到足够的内存时，将抛出 OutOfMemoryError 异常。

操作系统分配给每个进程的内存是有限制的，比如 32 位 Windows 的单个进程最大内存限制为 2GB，64 位 Linux 单进程内存可超过 32GB（可映射到虚拟内存）。

HotSpot 虚拟机中单个线程可分配的最大内存减去最大堆容量、再减去最大方法容量，再去掉直接内存和虚拟机本身耗费的内存，剩下的内存就由虚拟机和本地方法栈来分配。因此为每个线程分配到的栈内存越大，可建立的线程数量就越少，建立线程时就越容易把剩下的内存耗尽。

当在 32 位操作系统中出现上述因单线程建立过多而导致的容量不足的问题时，如果不能减少线程数量或者更换 64 位虚拟机的情况下，就只能通过减少最大堆和减少栈容量来换取更多的线程。

**方法区和运行时常量池溢出**

String::intern()是一个本地方法，可将 String 字符串放到运行时的常量池中，并返回对常量池对象的一个引用。

方法区的主要职责是用于存放类型的相关信息，如类名、访问修饰符、常量池、字段描述、方法描述等。

**直接内存溢出**

直接内存（Direct Memory）的容量大小可通过 -XX: MaxDirectMemorySize 来指定，如果不去指定，则默认与 Java 堆最大值（由 -Xmx 指定）一致。

使用 DirectByteBuffer 分配内存时并没有真正像操作系统申请分配内存，而是通过计算得知内存无法分配就会在代码里手动抛异常。真正向操作系统申请分配内存的方法是 Unsafe::allocateMemory()。