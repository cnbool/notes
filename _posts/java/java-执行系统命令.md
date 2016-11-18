title: Java-执行系统命令
categories:
  - Java
date: 2015-08-20 14:14:00
tags:
---

<!-- more --> 

```java
try {
	final Process process = Runtime.getRuntime().exec("E:\\startup.bat");
	new Thread(new Runnable() {

		@Override
		public void run() {
			BufferedReader bufReader = new BufferedReader(new InputStreamReader(process.getInputStream()));
			String line = null;
			try {
				while ((line = bufReader.readLine()) != null) {
					System.out.println(line);
				}
			} catch (IOException e) {
				e.printStackTrace();
			} finally {
				try {
					bufReader.close();
				} catch (IOException ignore) {
				}
			}
		}
	}).start();
	System.out.println(process.waitFor());
} catch (IOException e) {
	e.printStackTrace();
} catch (InterruptedException e) {
	e.printStackTrace();
}
```
