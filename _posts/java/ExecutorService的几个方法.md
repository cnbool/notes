title: ExecutorService的几个方法
id: 1710
comment: false
categories:
  - Java
date: 2015-07-06 18:54:49
tags:
---

## void shutdown();

启动一次顺序关闭，执行以前提交的任务，但不接受新任务。

* * *

## List<Runnable> shutdownNow();

试图停止所有正在执行的活动任务，暂停处理正在队列中的任务，并返回队列中等待执行的任务列表。

* * *

## boolean awaitTermination(long timeout, TimeUnit unit) throws InterruptedException;

阻塞，直到shutdown调用后全部任务执行完毕或发生超时或当前线程被interrupted

* * *

## boolean isTerminated();

shutdown之后所有任务都已结束时返回true

如果没有先调用shutdown或shutdownNow，此方法永远不会返回true

* * *

下列方法分两个阶段关闭 ExecutorService。第一阶段调用 shutdown 拒绝传入任务，然后调用 shutdownNow（如有必要）取消所有遗留的任务：

    void shutdownAndAwaitTermination(ExecutorService pool) {
    pool.shutdown(); // Disable new tasks from being submitted
      try {
        // Wait a while for existing tasks to terminate
        if (!pool.awaitTermination(60, TimeUnit.SECONDS)) {
          pool.shutdownNow(); // Cancel currently executing tasks
          // Wait a while for tasks to respond to being cancelled
          if (!pool.awaitTermination(60, TimeUnit.SECONDS))
              System.err.println(\"Pool did not terminate\");
        }
      } catch (InterruptedException ie) {
        // (Re-)Cancel if current thread also interrupted
        pool.shutdownNow();
        // Preserve interrupt status
        Thread.currentThread().interrupt();
      }
    }
    
