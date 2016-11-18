title: apache-commons-io之File-Monitor
categories:
  - apache-commons
date: 2015-07-10 00:30:55
tags:
---

** File Monitor组件使用了观察者模式，实现了监控目录和文件的创建/更新/删除事件 **

** FileAlterationListener **
FileAlterationListener是File Monitor组件唯一的接口,包含8个方法:

```java
    void    onDirectoryChange(File directory) 
    void    onDirectoryCreate(File directory) 
    void    onDirectoryDelete(File directory) 
    void    onFileChange(File file) 
    void    onFileCreate(File file) 
    void    onFileDelete(File file) 
    void    onStart(FileAlterationObserver observer) 
    void    onStop(FileAlterationObserver observer) 
```

目录的创建，更新和删除3个方法，文件的创建，更新和删除3个方法
onStart：当Observer开始检查事件的时候调用
onStop：当Observer结束检查事件的时候调用
Observer检查事件又是怎么回事呢，下文会提到

** FileAlterationListenerAdaptor **
这个类继承了FileAlterationListener接口，没有任何实现。
继承此类比直接继承FileAlterationListener接口方便的地方在于不需覆盖一些没有用到的方法，用到哪个方法就重写哪个方法。

** FileAlterationMonitor **
此类继承了Runnable接口，产生一个线程，以指定的时间间隔触发观察者的checkAndNotify方法

```java
    public final class FileAlterationMonitor implements Runnable {

        private final long interval;
        private final List<FileAlterationObserver> observers = new CopyOnWriteArrayList<FileAlterationObserver>();
        private Thread thread = null;
        private ThreadFactory threadFactory;
        private volatile boolean running = false;

        /**
         * Construct a monitor with a default interval of 10 seconds.
         */
        public FileAlterationMonitor() {
            this(10000);
        }

        /**
         * Construct a monitor with the specified interval.
         *
         * @param interval The amount of time in miliseconds to wait between
         * checks of the file system
         */
        public FileAlterationMonitor(long interval) {
            this.interval = interval;
        }

        /**
         * Construct a monitor with the specified interval and set of observers.
         *
         * @param interval The amount of time in miliseconds to wait between
         * checks of the file system
         * @param observers The set of observers to add to the monitor.
         */
        public FileAlterationMonitor(long interval, FileAlterationObserver... observers) {
            this(interval);
            if (observers != null) {
                for (FileAlterationObserver observer : observers) {
                    addObserver(observer);
                }
            }
        }

        //. . . 
    }
```

如果实例化时使用默认的构造函数，则触发间隔为10000毫秒，即10秒
此类维护了一个观察者列表observers

FileAlterationMonitor的启动

```java
    /**
     * Start monitoring.
     *
     * @throws Exception if an error occurs initializing the observer
     */
    public synchronized void start() throws Exception {
        if (running) {
            throw new IllegalStateException("Monitor is already running");
        }
        for (FileAlterationObserver observer : observers) {
            observer.initialize();
        }
        running = true;
        if (threadFactory != null) {
            thread = threadFactory.newThread(this);
        } else {
            thread = new Thread(this);
        }
        thread.start();
    }
```

启动时首先检查是否已在启动状态，即start方法是否已经掉用过
然后调用所有观察者的初始化方法，之后将启动标志running设置为true
创建新线程并启动,因本类实现了Runnable接口，所以创建线程时参数为 this
新线程执行的方法：

```java
    /**
     * Run.
     */
    public void run() {
        while (running) {
            for (FileAlterationObserver observer : observers) {
                observer.checkAndNotify();
            }
            if (!running) {
                break;
            }
            try {
                Thread.sleep(interval);
            } catch (final InterruptedException ignored) {
            }
        }
    }
```

此线程每隔指定时间调用Observer的checkAndNotify方法
