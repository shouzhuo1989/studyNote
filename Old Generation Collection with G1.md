### Old Generation Collection with G1

```
The G1 collector performs the following phases on the old generation of the heap. Note that some phases are part of a young generation collection.
```

## Initial Mark*(Stop the World Event)*

```
This is a stop the world event. With G1, it is piggybacked on a normal young GC. Mark survivor regions (root regions) which may have references to objects in old generation. 
```

```
工作在年轻代;
```

## Root Region Scanning

```
Scan survivor regions for references into the old generation. This happens while the application continues to run. The phase must be completed before a young GC can occur. 
```

```
 这个阶段必须在年轻的 GC 出现之前完成。 》》这是啥意思？
```

## Concurrent Marking

```
Find live objects over the entire heap. This happens while the application is running. This phase can be interrupted by young generation garbage collections. 
```

```
在整个堆上查找活动对象。 这是在应用程序运行时发生的。 这个阶段可能会被年轻一代的垃圾收集所打断。
```

## Remark*(Stop the World Event)*

```
Completes the marking of live object in the heap. Uses an algorithm called snapshot-at-the-beginning (SATB) which is much faster than what was used in the CMS collector. 
```

```
完成堆中活动对象的标记。 使用一种称为起始快照(snapshot-at-the-beginning，SATB)的算法，该算法比 CMS 收集器中使用的算法快得多。
```

## Cleanup  *(Stop the World Event and Concurrent)*

```
    Performs accounting on live objects and completely free regions. (Stop the world)
    Scrubs the Remembered Sets. (Stop the world)
    Reset the empty regions and return them to the free list. (Concurrent)
```

```
记录活着的对象和空闲区域（Stop the world）
擦洗Rsets。(Stop the world)
重置空白区域并将它们返回到空闲列表（Concurrent）
```

## Copying*(Stop the World Event)*

```
These are the stop the world pauses to evacuate or copy live objects to new unused regions. This can be done with young generation regions which are logged as [GC pause (young)]. Or both young and old generation regions which are logged as [GC Pause (mixed)].
```

```
停止应用程序，移动或者复制活着的对象到新的没有使用的region。 
```

#### Summary of Old Generation GC

```
In summary, there are a few key points we can make about the G1 garbage collection on the old generation.

    Concurrent Marking Phase
        Liveness information is calculated concurrently while the application is running.
        This liveness information identifies which regions will be best to reclaim during an evacuation pause.
        There is no sweeping phase like in CMS.
    Remark Phase
        Uses the Snapshot-at-the-Beginning (SATB) algorithm which is much faster then what was used with CMS.
        Completely empty regions are reclaimed.
    Copying/Cleanup Phase
        Young generation and old generation are reclaimed at the same time.
        Old generation regions are selected based on their liveness.

```

