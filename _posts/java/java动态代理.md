title: java动态代理
categories:
  - java
date: 2016-07-30 11:07:52
tags:
---

## JDK 动态代理 ##

特点：仅能代理继承接口的类

被代理类：
```
public class MyJob implements Runnable {
	
	@Override
	public void run() {
		System.out.println("coding...");
	}
}
```

代理需继承InvocationHandler：
```
public class MyJobProxy implements InvocationHandler {
	private Object obj;

    public MyJobProxy(Object obj) {
        this.obj = obj;
    }
    
    @SuppressWarnings("unchecked")
	public <T> T getProxy() {
    	return (T) Proxy.newProxyInstance(obj.getClass().getClassLoader(), obj.getClass().getInterfaces(),
				new MyInvocationHandler(obj));
    }

	@Override
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
		System.out.println("before invoke");
		Object rst = method.invoke(obj, args);
		System.out.println("after invoke");
		return rst;
	}

}
```

调用代理类：
```
public static void main(String[] args) throws Exception {
	MyJob obj = new MyJob();
	Runnable proxy = new MyJobProxy(obj).getProxy();
	proxy.run();
}
```

