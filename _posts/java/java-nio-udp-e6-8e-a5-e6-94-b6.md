title: java NIO UDP接收
id: 1474
categories:
  - Java
date: 2014-09-11 19:47:06
tags:
---

```java
public class UDPServer {
	private static final Logger logger = LoggerFactory.getLogger(UDPServer.class);
	private int port = -1; // 本地监听端口
	private int bufferLen = 500;
	private ByteBuffer byteBuffer;
	private AtomicLong receive = new AtomicLong(0);
	private AtomicLong error = new AtomicLong(0);

	public void startup() {
		Selector selector;
		try {
			selector = Selector.open();
			DatagramChannel datagramChannel = DatagramChannel.open();
			datagramChannel.configureBlocking(false); // 非阻塞
			datagramChannel.bind(new InetSocketAddress(port));
			datagramChannel.register(selector, SelectionKey.OP_READ);
			byteBuffer = ByteBuffer.allocate(bufferLen);
			logger.info("监听端口:{}", port);

			while (true) {
				int num = 0;
				try {
					num = selector.select();
					if (num > 0) {
						Set<SelectionKey> selectedKeys = selector.selectedKeys();
						Iterator<SelectionKey> it = selectedKeys.iterator();
						while (it.hasNext()) {
							try {
								SelectionKey key = it.next(); // 获取事件
								it.remove();
								if (key.isReadable()) {
									try {
										DatagramChannel channel = (DatagramChannel) key.channel();
										channel.receive(byteBuffer);
										byteBuffer.flip();
										byte[] data = new byte[byteBuffer.remaining()];
										byteBuffer.get(data);
										byteBuffer.clear();

										// 处理收到的data
									} catch (Exception e) {
										logger.error("error", e);
										error.incrementAndGet();
									}
								}
							} catch (Exception e) {

							}
						}
					}
				} catch (Exception e) {
				}

			}
		} catch (Exception e) {
			e.printStackTrace();
		}

	}

	public int getPort() {
		return port;
	}

	public void setPort(int port) {
		this.port = port;
	}

	public int getBufferLen() {
		return bufferLen;
	}

	public void setBufferLen(int bufferLen) {
		this.bufferLen = bufferLen;
	}

	public long getReceiveCount() {
		return receive.get();
	}

	public long getErrorCount() {
		return error.get();
	}

}
```
