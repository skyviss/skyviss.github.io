```
多线程并发编程
```
* 线程、进程、纤程
 * 进程： 
   操作系统os资源分配的基本单元，比如内存、打开文件、网络io，
   分配了独立的内存空间 
 * 线程： 
   操作系统os资源调度的基本单元，cpu分配的基本单元
 * 纤程：
   用户态线程，是线程中的线程，切换和调度不需要经过os
   轻量级的线程
   占用资源少，轻量级，切换简单，可以同时被启动多个（10万个都没问题）
   
纤程应用场景
   纤程VS线程池：很短的计算任务，不需要和内核打交道，并发量高
   目前支持纤程的有Kotlin,Scala,go,可惜java没有官方支持纤程
   java需要依赖Quasar库
```
    <dependency>
        <groupId>co.paralleluniverse</groupId>
        <artifactId>quasar-core</artifactId>
        <version>0.7.6</version>
    </dependency>
```
