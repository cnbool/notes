title: Java Queue
id: 1380
categories:
  - Java
date: 2014-05-02 00:54:45
tags:
---

## **Java接口 Queue<E>**

public interface Queue
extends Collection

除了基本的 Collection 操作外，队列还提供其他的插入、提取和检查操作。每个方法都存在两种形式：一种抛出异常（操作失败时），另一种返回一个特殊值（null 或 false，具体取决于操作）。插入操作的后一种形式是用于专门为有容量限制的 Queue 实现设计的.
<table border="" cellspacing="1" cellpadding="3">
<tbody>
<tr>
<td></td>
<td align="CENTER">_抛出异常_</td>
<td align="CENTER">_返回特殊值_</td>
</tr>
<tr>
<td>**插入**</td>
<td>[`add(e)`](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Queue.html#add(E))</td>
<td>[`offer(e)`](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Queue.html#offer(E))</td>
</tr>
<tr>
<td>**移除**</td>
<td>[`remove()`](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Queue.html#remove())</td>
<td>[`poll()`](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Queue.html#poll())</td>
</tr>
<tr>
<td>**检查**</td>
<td>[`element()`](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Queue.html#element())</td>
<td>`[peek()](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Queue.html#peek())`

 </td>
</tr>
</tbody>
</table>
<table summary="" width="100%" border="1" cellspacing="0" cellpadding="3">
<tbody>
<tr bgcolor="#CCCCFF">
<th colspan="2" align="left"><span style="font-size: small;">方法摘要</span></th>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` boolean`</span></td>
<td>`** [add](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Queue.html#add(E))**([E](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Queue.html "Queue 中的类型参数") e)`
将指定的元素插入此队列（如果立即可行且不会违反容量限制），在成功时返回 <tt>true</tt>，如果当前没有可用的空间，则抛出<tt>IllegalStateException</tt>。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` [E](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Queue.html "Queue 中的类型参数")`</span></td>
<td>`** [element](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Queue.html#element())**()`
获取，但是不移除此队列的头。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` boolean`</span></td>
<td>`** [offer](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Queue.html#offer(E))**([E](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Queue.html "Queue 中的类型参数") e)`
将指定的元素插入此队列（如果立即可行且不会违反容量限制），当使用有容量限制的队列时，此方法通常要优于 [`add(E)`](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Queue.html#add(E))，后者可能无法插入元素，而只是抛   出一个异常。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` [E](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Queue.html "Queue 中的类型参数")`</span></td>
<td>`** [peek](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Queue.html#peek())**()`
获取但不移除此队列的头；如果此队列为空，则返回 <tt>null</tt>。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` [E](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Queue.html "Queue 中的类型参数")`</span></td>
<td>`** [poll](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Queue.html#poll())**()`
获取并移除此队列的头，如果此队列为空，则返回 <tt>null</tt>。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` [E](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Queue.html "Queue 中的类型参数")`</span></td>
<td>`** [remove](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Queue.html#remove())**()`
获取并移除此队列的头。</td>
</tr>
</tbody>
</table>

## <span style="color: #000000;">**Java接口 BlockingQueue**</span>

支持两个附加操作的 [`Queue`](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Queue.html "java.util 中的接口")，这两个操作是：获取元素时如果队列为空等待队列非空，以及存储元素时如果队列已满等待空间变得可用。

<tt>BlockingQueue</tt> 方法以四种形式出现，对于不能立即满足但可能在将来某一时刻可以满足的操作，这四种形式的处理方式不同：第一种是抛出一个异常，第二种是返回一个特殊值（<tt>null</tt> 或 <tt>false</tt>，具体取决于操作），第三种是在操作可以成功前，无限期地阻塞当前线程，第四种是在放弃前只在给定的最大时间限制内阻塞。下表中总结了这些方法：
<!--more-->
<table border="" cellspacing="1" cellpadding="3">
<tbody>
<tr>
<td></td>
<td align="CENTER">_抛出异常_</td>
<td align="CENTER">_特殊值_</td>
<td align="CENTER">_阻塞_</td>
<td align="CENTER">_超时_</td>
</tr>
<tr>
<td>**插入**</td>
<td>[`add(e)`](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html#add(E))</td>
<td>[`offer(e)`](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html#offer(E))</td>
<td>[`put(e)`](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html#put(E))</td>
<td>[`offer(e, time, unit)`](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html#offer(E, long, java.util.concurrent.TimeUnit))</td>
</tr>
<tr>
<td>**移除**</td>
<td>[`remove()`](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html#remove(java.lang.Object))</td>
<td>[`poll()`](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html#poll(long, java.util.concurrent.TimeUnit))</td>
<td>[`take()`](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html#take())</td>
<td>[`poll(time, unit)`](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html#poll(long, java.util.concurrent.TimeUnit))</td>
</tr>
<tr>
<td>**检查**</td>
<td>[`element()`](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Queue.html#element())</td>
<td>[`peek()`](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Queue.html#peek())</td>
<td>_不可用_</td>
<td>_不可用_

 </td>
</tr>
</tbody>
</table>
<tt>BlockingQueue</tt> 不接受 <tt>null</tt> 元素。
<tt>BlockingQueue</tt> 可以是限定容量的。
<tt>BlockingQueue</tt> 实现是线程安全的。
<table summary="" width="100%" border="1" cellspacing="0" cellpadding="3">
<tbody>
<tr bgcolor="#CCCCFF">
<th colspan="2" align="left"><span style="font-size: small;">方法摘要</span></th>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` boolean`</span></td>
<td>`** [add](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html#add(E))**([E](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html "BlockingQueue 中的类型参数") e)`
将指定元素插入此队列中（如果立即可行且不会违反容量限制），成功时返回 <tt>true</tt>，如果当前没有可用的空间，则抛出<tt>IllegalStateException</tt>。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` boolean`</span></td>
<td>`** [contains](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html#contains(java.lang.Object))**([Object](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/lang/Object.html "java.lang 中的类") o)`
如果此队列包含指定元素，则返回 <tt>true</tt>。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` int`</span></td>
<td>`** [drainTo](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html#drainTo(java.util.Collection))**([Collection](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html "java.util 中的接口")<? super [E](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html "BlockingQueue 中的类型参数")> c)`
移除此队列中所有可用的元素，并将它们添加到给定 collection 中。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` int`</span></td>
<td>`** [drainTo](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html#drainTo(java.util.Collection, int))**([Collection](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html "java.util 中的接口")<? super [E](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html "BlockingQueue 中的类型参数")> c, int maxElements)`
最多从此队列中移除给定数量的可用元素，并将这些元素添加到给定 collection 中。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` boolean`</span></td>
<td>`** [offer](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html#offer(E))**([E](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html "BlockingQueue 中的类型参数") e)`
将指定元素插入此队列中（如果立即可行且不会违反容量限制），成功时返回 <tt>true</tt>，如果当前没有可用的空间，则返回 <tt>false</tt>。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` boolean`</span></td>
<td>`** [offer](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html#offer(E, long, java.util.concurrent.TimeUnit))**([E](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html "BlockingQueue 中的类型参数") e, long timeout, [TimeUnit](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/TimeUnit.html "java.util.concurrent 中的枚举") unit)`
将指定元素插入此队列中，在到达指定的等待时间前等待可用的空间（如果有必要）。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` [E](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html "BlockingQueue 中的类型参数")`</span></td>
<td>`** [poll](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html#poll(long, java.util.concurrent.TimeUnit))**(long timeout, [TimeUnit](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/TimeUnit.html "java.util.concurrent 中的枚举") unit)`
获取并移除此队列的头部，在指定的等待时间前等待可用的元素（如果有必要）。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` void`</span></td>
<td>`** [put](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html#put(E))**([E](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html "BlockingQueue 中的类型参数") e)`
将指定元素插入此队列中，将等待可用的空间（如果有必要）。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` int`</span></td>
<td>`** [remainingCapacity](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html#remainingCapacity())**()`
返回在无阻塞的理想情况下（不存在内存或资源约束）此队列能接受的附加元素数量；如果没有内部限制，则返回<tt>Integer.MAX_VALUE</tt>。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` boolean`</span></td>
<td>`** [remove](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html#remove(java.lang.Object))**([Object](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/lang/Object.html "java.lang 中的类") o)`
从此队列中移除指定元素的单个实例（如果存在）。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` [E](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html "BlockingQueue 中的类型参数")`</span></td>
<td>`** [take](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/concurrent/BlockingQueue.html#take())**()`
获取并移除此队列的头部，在元素变得可用之前一直等待（如果有必要）。</td>
</tr>
</tbody>
</table>
