title: Java NIO
id: 1315
categories:
  - Java
date: 2014-04-22 20:10:38
tags:
---

**缓冲区类型**
最常用的缓冲区类型是 ByteBuffer,其他缓冲区类型：
ByteBuffer,CharBuffer,ShortBuffer,IntBuffer,LongBuffer,FloatBuffer,DoubleBuffer

**缓冲区的分配**
分配一个具有指定大小的底层数组，并将它包装到一个缓冲区对象中:
```java
ByteBuffer buffer = ByteBuffer.allocate( 1024 );
//现有的数组转换为缓冲区:
byte array[] = new byte[1024];
ByteBuffer buffer = ByteBuffer.wrap( array );
```

**通道**
Channel是一个对象，可以通过它读取和写入数据

例子:
```java
FileInputStream fin = new FileInputStream( "in.txt" );
FileChannel fcin = fin.getChannel();
FileOutputStream fout = new FileOutputStream( "out.txt" );
FileChannel fcout = fout.getChannel();
ByteBuffer buffer = ByteBuffer.allocate(1024);

while(fc.read(buffer) != -1) { // 读取完毕时返回-1
  buffer.flip(); // 切换到读取模式(可以从缓冲区中读取数据)
  fc.write(buffer);
  buffer.clear(); // 重设缓冲区到写入模式(可以接受读入的数据)
}
```
<!--more-->

**缓冲区内部**
状态变量:
**position**
缓冲区指针
向缓冲区读入时它指向下一个写入的位置,从缓冲区取出时它指向下一个取出的位置
position <= limit

**limit**
向缓冲区读入时最多可放入的数量,此时limit == capacity
从缓冲区取出时最多可取出的数量

**capacity**
缓冲区的容量

**Buffer常用方法:**
**flip()**
缓冲区切换到读取模式:
limit = position;
position = 0;
makes a buffer ready for a new sequence of channel-write or relative get operations: It sets the limit to the current position and then sets the position to zero. 

**clear()**
重设缓冲区到写入模式:
limit = capacity;
position = 0;
makes a buffer ready for a new sequence of channel-read or relative put operations: It sets the limit to the capacity and the position to zero. 

**rewind()**
倒带
makes a buffer ready for re-reading the data that it already contains: It leaves the limit unchanged and sets the position to zero. 

**boolean hasRemaining()**
Tells whether there are any elements between the current position and the limit.

**int remaining()**
Returns the number of elements between the current position and the limit.

**Buffer mark()**
Sets this buffer's mark at its position.

**Buffer reset()**
Resets this buffer's position to the previously-marked position.

**ByteBuffer常用方法**
**byte get()**
Relative get method.
Reads the byte at this buffer's current position, and then increments the position. 

**ByteBuffer get(byte[] dst)**
Relative bulk get method.
Transfers bytes from this buffer into the given destination array

**ByteBuffer get(byte[] dst, int offset, int length)**
Relative bulk get method.

**byte get(int index)**
Absolute get method.
Reads the byte at the given index. 

相对方法:position在get之后会增加
绝对方法:get之后position不变

**ByteBuffer 类中有五个 put() 方法：**
1\. ByteBuffer put( byte b );
2\. ByteBuffer put( byte src[] );
3\. ByteBuffer put( byte src[], int offset, int length );
4\. ByteBuffer put( ByteBuffer src );
5\. ByteBuffer put( int index, byte b );
前四个方法是相对的，而第五个方法是绝对的。

**slice()**
根据现有的缓冲区创建一种 子缓冲区。也就是说它创建一个新的缓冲区，新缓冲区与原来的缓冲区的一部分共享数据
```java
ByteBuffer buffer = ByteBuffer.allocate(10);
for (int i = 0; i < buffer.capacity(); ++i) {
  buffer.put( (byte)i );
}

//现在我们对这个缓冲区分片，以创建一个包含槽3到槽6的子缓冲区
//窗口的起始和结束位置通过设置 position 和 limit 值来指定，然后调用 Buffer 的 slice() 方法：
buffer.position( 3 );
buffer.limit( 7 );
ByteBuffer slice = buffer.slice();

//我们遍历子缓冲区，将每一个元素乘以 11 来改变它
for (int i = 0; i < slice.capacity(); ++i) {
    byte b = slice.get(i);
    b *= 11;
    slice.put(i, b);
}

//原缓冲区中的内容
buffer.position(0);
buffer.limit(buffer.capacity());
while (buffer.remaining()>0) {
    System.out.println(buffer.get());
}

输出:
0
1
2
33
44
55
66
7
8
9
```

**asReadOnlyBuffer()**
只读缓冲区
返回一个与原缓冲区完全相同的缓冲区(并与其共享数据)，只不过它是只读的。
Creates a new, read-only byte buffer that shares this buffer's content.
The content of the new buffer will be that of this buffer. Changes to this buffer's content will be visible in the new buffer; the new buffer itself, however, will be read-only and will not allow the shared content to be modified. The two buffers' position, limit, and mark values will be independent.
The new buffer's capacity, limit, position, and mark values will be identical to those of this buffer. 

**duplicate()**
复制缓冲区(共享数据)
Creates a new byte buffer that shares this buffer's content.
The content of the new buffer will be that of this buffer. Changes to this buffer's content will be visible in the new buffer, and vice versa; the two buffers' position, limit, and mark values will be independent.
The new buffer's capacity, limit, position, and mark values will be identical to those of this buffer. The new buffer will be direct if, and only if, this buffer is direct, and it will be read-only if, and only if, this buffer is read-only.

**ByteBuffer compact()**
压缩缓冲区,丢弃已经读取过的数据,将未读取的数据移动到缓冲区头部,position指向压缩后缓冲区末位数据的下一个位置,limit与capacity相等
 The bytes between the buffer's current position and its limit, if any, are copied to the beginning of the buffer. That is, the byte at index p = position() is copied to index zero, the byte at index p + 1 is copied to index one, and so forth until the byte at index limit() - 1 is copied to index n = limit() - 1 - p. The buffer's position is then set to n+1 and its limit is set to its capacity. The mark, if defined, is discarded.

The buffer's position is set to the number of bytes copied, rather than to zero, so that an invocation of this method can be followed immediately by an invocation of another relative put method.

Invoke this method after writing data from a buffer in case the write was incomplete. The following loop, for example, copies bytes from one channel to another via the buffer buf:

     buf.clear();          // Prepare buffer for use
     while (in.read(buf) >= 0 || buf.position != 0) {
         buf.flip();
         out.write(buf);
         buf.compact();    // In case of partial write
     }
