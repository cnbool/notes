title: Java接口 Collection
id: 1376
categories:
  - Java
date: 2014-05-02 00:40:50
tags:
---

Collection 表示一组对象，这些对象也称为 collection 的元素。一些 collection 允许有重复的元素，而另一些则不允许。一些 collection 是有序的，而另一些则是无序的。JDK 不提供此接口的任何直接 实现：它提供更具体的子接口（如 Set 和 List）实现。
<table summary="" width="100%" border="1" cellspacing="0" cellpadding="3">
<tbody>
<tr bgcolor="#CCCCFF">
<th colspan="2" align="left"><span style="font-size: small;">方法摘要</span></th>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` boolean`</span></td>
<td>`**[add](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html#add(E))**([E](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html "Collection 中的类型参数") e)`
确保此 collection 包含指定的元素（可选操作）。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` boolean`</span></td>
<td>`**[addAll](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html#addAll(java.util.Collection))**([Collection](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html "java.util 中的接口")<? extends [E](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html "Collection 中的类型参数")> c)`
将指定 collection 中的所有元素都添加到此 collection 中（可选操作）。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` void`</span></td>
<td>`**[clear](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html#clear())**()`
移除此 collection 中的所有元素（可选操作）。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` boolean`</span></td>
<td>`**[contains](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html#contains(java.lang.Object))**([Object](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/lang/Object.html "java.lang 中的类") o)`
如果此 collection 包含指定的元素，则返回 <tt>true</tt>。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` boolean`</span></td>
<td>`**[containsAll](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html#containsAll(java.util.Collection))**([Collection](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html "java.util 中的接口")<?> c)`
如果此 collection 包含指定 collection 中的所有元素，则返回 <tt>true</tt>。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` boolean`</span></td>
<td>`**[equals](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html#equals(java.lang.Object))**([Object](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/lang/Object.html "java.lang 中的类") o)`
比较此 collection 与指定对象是否相等。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` int`</span></td>
<td>`**[hashCode](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html#hashCode())**()`
返回此 collection 的哈希码值。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` boolean`</span></td>
<td>`**[isEmpty](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html#isEmpty())**()`
如果此 collection 不包含元素，则返回 <tt>true</tt>。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` [Iterator](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Iterator.html "java.util 中的接口")<[E](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html "Collection 中的类型参数")>`</span></td>
<td>`**[iterator](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html#iterator())**()`
返回在此 collection 的元素上进行迭代的迭代器。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` boolean`</span></td>
<td>`**[remove](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html#remove(java.lang.Object))**([Object](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/lang/Object.html "java.lang 中的类") o)`
从此 collection 中移除指定元素的单个实例，如果存在的话（可选操作）。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` boolean`</span></td>
<td>`**[removeAll](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html#removeAll(java.util.Collection))**([Collection](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html "java.util 中的接口")<?> c)`
移除此 collection 中那些也包含在指定 collection 中的所有元素（可选操作）。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` boolean`</span></td>
<td>`**[retainAll](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html#retainAll(java.util.Collection))**([Collection](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html "java.util 中的接口")<?> c)`
仅保留此 collection 中那些也包含在指定 collection 的元素（可选操作）。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` int`</span></td>
<td>`**[size](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html#size())**()`
返回此 collection 中的元素数。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%"><span>` [Object](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/lang/Object.html "java.lang 中的类")[]`</span></td>
<td>`**[toArray](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html#toArray())**()`
返回包含此 collection 中所有元素的数组。</td>
</tr>
<tr bgcolor="white">
<td align="right" valign="top" width="1%">
<table summary="" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr align="right" valign="">
<td nowrap="nowrap"><span>`<T> T[]`</span></td>
</tr>
</tbody>
</table>
</td>
<td>`**[toArray](http://download.oracle.com/technetwork/java/javase/6/docs/zh/api/java/util/Collection.html#toArray(T[]))**(T[] a)`
返回包含此 collection 中所有元素的数组；返回数组的运行时类型与指定数组的运行时类型相同。</td>
</tr>
</tbody>
</table>
<!--more-->

## 类 AbstractCollection<E>

public abstract class AbstractCollection
extends Object
implements Collection

此类提供 <tt>Collection</tt> 接口的骨干实现，以最大限度地减少了实现此接口所需的工作。

要实现一个不可修改的 collection，编程人员只需扩展此类，并提供 <tt>iterator</tt> 和 <tt>size</tt> 方法的实现。（<tt>iterator</tt> 方法返回的迭代器必须实现 <tt>hasNext</tt>和 <tt>next</tt>。）

要实现可修改的 collection，编程人员必须另外重写此类的 <tt>add</tt> 方法（否则，会抛出 <tt>UnsupportedOperationException</tt>），<tt>iterator</tt> 方法返回的迭代器还必须另外实现其 <tt>remove</tt> 方法。
