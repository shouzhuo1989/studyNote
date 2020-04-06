# Garbage Collector

## Ergonomics

```
1、The Parallel Collector有两个垃圾收集调优参数：maximum pause time goal and application throughput goal;（其他收集器没有）
```

```
2、使用命令行选项-XX: MaxGCPauseMillis=nnn 指定maximum pause time goal。 这被解释为对垃圾回收器的提示，希望暂停时间为 nnn 毫秒或更少。 垃圾收集器将调整 Java 堆大小和其他与垃圾收集相关的参数，以使垃圾收集暂停时间小于 nnn 毫秒。 默认情况下没有最大暂停时间目标。 这些调整可能会导致垃圾收集器更频繁地发生，从而降低应用程序的总吞吐量。 垃圾收集器试图满足吞吐量目标之前的任何暂停时间目标。 但是，在某些情况下，暂停时间目标无法达到。
```

```
3、application throughput goal是根据收集垃圾所花费的时间和垃圾收集之外所花费的时间(称为应用程序时间)来度量的。 目标由命令行选项 -XX: GCTimeRatio=nnn 指定。 垃圾收集时间与应用程序时间的比值为1 / (1 + nnn)。 例如,-XX:GCTimeRatio=19设置了垃圾收集总时间的1 / 20或5% 的目标。

用于垃圾收集的时间是年轻一代和老一代收集合计的总时间。 如果没有达到吞吐量目标，则会增加代的大小，以增加应用程序在collections之间运行的时间。
```

```
4、当垃圾收集器试图满足相互竞争的目标时，堆的大小通常会发生变化。 即使应用程序已经达到稳定状态，这也是正确的。 实现吞吐量目标(可能需要更大的堆)的压力与最大暂停时间和最小占用空间(这两者都可能需要较小的堆)的目标相竞争。
```

```
5、除非您知道需要一个大于默认最大堆大小的堆，否则不要为堆选择最大值。 选择一个对您的应用程序足够的吞吐量目标。
```

```
6、堆将增长或收缩到支持所选吞吐量目标的大小。 应用程序行为的更改可能导致堆的增长或收缩。 例如，如果应用程序开始以更高的速度分配，则堆将增大以维持相同的吞吐量。
```

```
7、如果堆增长到其最大大小，而吞吐量目标没有达到，则最大堆大小对于吞吐量目标来说太小了。 将最大堆大小设置为一个值，该值接近平台上的总物理内存，但不会导致应用程序的swapping（啥意思？） 。 再次执行应用程序。 如果吞吐量目标仍然没有达到，那么应用程序时间的目标对于平台上的可用内存来说太高了。
```

```
8、如果吞吐量目标可以达到，但是暂停时间过长，则选择最大暂停时间目标。 选择一个最大暂停时间目标可能意味着您的吞吐量目标将无法实现，因此选择对于应用程序来说可以接受的折衷值。
```

## Generations

```
1、如果垃圾收集成为瓶颈，您很可能必须自定义总的堆大小以及各代的大小。 检查详细的垃圾收集器输出，然后研究单个性能指标对垃圾收集器参数的敏感性。
```

```
2、没有一个正确的方法来选择the size of a generation。 最佳选择取决于应用程序the way the application uses memory as well as user requirements。 因此，虚拟机对垃圾回收器的选择并不总是最佳的，可能会被命令行选项所覆盖，这些选项在“调整代的大小”一节中进行了描述。
```

```
3、-verbose:gc   -XX:+PrintGCDetails   -XX:+PrintGCTimeStamps   打印GC日志
```

```
4、响应时间？ 吞吐量？
```

```
5、 the size of the heap, the amount of live data maintained by the application, and the number and speed of available processors  调优时需要考虑的因素
```

```
6、统计信息(例如收集器保存的平均停顿时间)在每次收集结束时更新。 然后进行测试，以确定目标是否已经达到，并对the size of a generation 作出任何必要的调整。 例外情况是，在保存统计信息和调整代的大小方面，忽略显式的垃圾收集(例如对 System.gc ()的调用)。
```

```
7、java -XX:+PrintFlagsFinal -version 》查看虚拟机参数
```

```
8、If more than 98% of the total time is spent in garbage collection and less than 2% of the heap is recovered, then an OutOfMemoryError is thrown.
```

