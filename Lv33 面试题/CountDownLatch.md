# CountDownLatch

让一些线程阻塞直到另外一些完成后才被唤醒，[点击这里](https://blog.csdn.net/zhangchao_cn/article/details/85163718)

## CountDownLatch主要有两个方法

- 当一个或多个线程调用await方法时,调用线程会被阻塞
- 其他线程调用countDown方法计数器减1(调用countDown方法时线程不会阻塞),当计数器的值变为0,因调用await方法被阻塞的线程会被唤醒,继续执行

