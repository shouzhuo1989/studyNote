# 一、CMS Collector operations step by step

## 1、**Heap Structure for CMS Collector**

```
The heap is split into three spaces.Young generation is split into Eden and two survivor spaces. Old generation is one contiguous space. Object collection is done in place. No compaction is done unless there is a full GC.
```

```
Old generation是一个连续的空间。垃圾收集就地进行。没有compaction，除非是Full GC。
```

## 2、**How Young GC works in CMS**

```
The young generation is colored light green and the old generation in blue. This is what the CMS might look like if your application has been running for a while. Objects are scattered around the old generation area.
With CMS, old generation objects are deallocated in place. They are not moved around. The space is not compacted unless there is a full GC.
```

```
年轻一代是浅绿色，老一代是蓝色。 如果您的应用程序已经运行了一段时间，那么这就是 CMS 的样子。 对象在老年代散乱放置。

使用 CMS，旧的一代对象将被释放到适当的位置。 它们不会四处移动。 除非有一个Full GC，否则空间不会compact。
```

<img src="https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/G1GettingStarted/images/slide2.png" alt="/webfolder/technetwork/tutorials/obe/java/G1GettingStarted/images/slide2.png" style="zoom:80%;" />

## 3、**Young Generation Collection**

```
Live objects are copied from the Eden space and survivor space to the other survivor space. Any older objects that have reached their aging threshold are promoted to old generation.
```

```
达到aging threshold就晋升到老年代
```

![/webfolder/technetwork/tutorials/obe/java/G1GettingStarted/images/slide3.png](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/G1GettingStarted/images/slide3.png)

## 4、**After Young GC**

```
After a young GC, the Eden space is cleared and one of the survivor spaces is cleared.
Newly promoted objects are shown in dark blue on the diagram. The green objects are surviving young generation objects that have not yet been promoted to old generation.
```

![/webfolder/technetwork/tutorials/obe/java/G1GettingStarted/images/slide4.png](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/G1GettingStarted/images/slide4.png)

## 5、**Old Generation Collection with CMS**

```
Two stop the world events take place: initial mark and remark. When the old generation reaches a certain occupancy rate, the CMS is kicked off. 
(1) Initial mark is a short pause phase where live (reachable) objects are marked. (2) 从 finds live objects while the application continues to execute. Finally, in the (3) remark phase, objects are found that were missed during (2) concurrent marking in the previous phase.
```

```
stop the world发生两次: initial mark和remark。 当老一代达到一定的占用率时，CMS 就被启动了。

initial mark是一个短暂的暂停阶段，其中live (reachable) objects被标记。 Concurrent marking查找活动对象和应用程序同时进行。 最后，在remark阶段，发现在前一阶段并发标记期间遗漏的对象。
```

![/webfolder/technetwork/tutorials/obe/java/G1GettingStarted/images/slide5.png](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/G1GettingStarted/images/slide5.png)

## 6、**Old Generation Collection - Concurrent Sweep**

```
Objects that were not marked in the previous phase are deallocated in place. There is no compaction.
Note: Unmarked objects == Dead Objects
```

![/webfolder/technetwork/tutorials/obe/java/G1GettingStarted/images/slide6.png](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/G1GettingStarted/images/slide6.png)

## 7、**Old Generation Collection - After Sweeping**

```
After the (4) Sweeping phase, you can see that a lot of memory has been freed up. You will also notice that no compaction has been done. 
Finally, the CMS collector will move through the (5) resetting phase and wait for the next time the GC threshold is reached.
```

```
没有compact执行;
```

![/webfolder/technetwork/tutorials/obe/java/G1GettingStarted/images/slide7.png](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/G1GettingStarted/images/slide7.png)

# 二、CMS Collection Phases

## 1、Initial Mark*(Stop the World Event)*

```
Objects in old generation are “marked” as reachable including those objects which may be reachable from young generation. Pause times are typically short in duration relative to minor collection pause times. 
```

```
老一代中的对象被”标记为”可达” ，包括那些可能从年轻一代可达的对象。 相对于minor collection pause times，暂停时间通常持续时间较短。
```

## 2、Concurrent Marking

```
Traverse the tenured generation object graph for reachable objects concurrently while Java application threads are executing. Starts scanning from marked objects and transitively marks all objects reachable from the roots. The mutators are executing during the concurrent phases 2, 3, and 5 and any objects allocated in the CMS generation during these phases (including promoted objects) are immediately marked as live. 
```

```
在 Java 应用程序线程执行时，并发地遍历the tenured generation object graph。 开始从标记的对象扫描，并且传递性地标记从根可到达的所有对象。 每个对象都有一个mutators，在2、4、5并发阶段，假如这个mutators在执行，就把这个对象marked as live？（后半段是这样理解吗？）
```

## 3、Remark*(Stop the World Event)*

```
Finds objects that were missed by the concurrent mark phase due to updates by Java application threads to objects after the concurrent collector had finished tracing that object. 
```

```
查找并发标记阶段遗漏的对象，这些对象是在并发收集器完成对该对象的跟踪后，由 Java 应用程序线程更新而遗漏的。
```

## 4、Concurrent Sweep

```
Collects the objects identified as unreachable during marking phases. The collection of a dead object adds the space for the object to a free list for later allocation. Coalescing of dead objects may occur at this point. Note that live objects are not moved.
```

```
收集在标记阶段中被识别为不可到达的对象。 死对象的收集将该对象的空间添加到空闲列表中，以便以后分配。 在这一点上，dead objects可能会合并。 请注意，活动对象不会被移动。
```

## 5、Resetting

```
Prepare for next concurrent collection by clearing data structures. .;斯蒂芬
```

```
通过清除数据结构为下一次并发收集做准备。
```

