---
title: Java 创建线程的两种方式及其区别
cover: /img/covers/java-thread-creation.jpg
date: 2026-06-24 14:50:00
top_img: https://picsum.photos/seed/threadcreation/1920/600
tags:
  - Java
  - 多线程
  - Thread
  - Runnable
categories:
  - Java课后作业
---

## 一、前言

在 Java 中，创建线程主要有两种方式：

1. **继承 `Thread` 类**
2. **实现 `Runnable` 接口**

这两种方式都可以创建并启动一个新线程，但它们在语法、设计思想和使用场景上有明显的区别。理解它们的差异，是掌握 Java 多线程编程的基础。

## 二、方式一：继承 Thread 类

通过继承 `Thread` 类，重写 `run()` 方法来定义线程执行的任务。

```java
public class MyThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + " 输出: " + i);
        }
    }
}

public class ThreadDemo {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        MyThread t2 = new MyThread();
        t1.start();
        t2.start();
    }
}
```

### 特点

- 写法简单，直接定义线程类。
- 线程任务与线程对象耦合在一起。
- 由于 Java 只支持单继承，继承了 `Thread` 类后就不能再继承其他类，扩展性受限。

## 三、方式二：实现 Runnable 接口

通过实现 `Runnable` 接口，将线程任务与线程对象分离，然后把 `Runnable` 实现类作为参数传递给 `Thread` 构造方法。

```java
public class MyRunnable implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + " 输出: " + i);
        }
    }
}

public class RunnableDemo {
    public static void main(String[] args) {
        MyRunnable task = new MyRunnable();

        Thread t1 = new Thread(task, "线程A");
        Thread t2 = new Thread(task, "线程B");
        t1.start();
        t2.start();
    }
}
```

### 特点

- 任务与线程分离，代码结构更清晰。
- 实现 `Runnable` 接口后，仍然可以继承其他类，扩展性更好。
- 同一个 `Runnable` 任务可以被多个线程共享，适合处理共享资源的场景。

## 四、核心区别对比

| 对比项 | 继承 Thread 类 | 实现 Runnable 接口 |
|--------|---------------|-------------------|
| 实现方式 | 继承 `Thread`，重写 `run()` | 实现 `Runnable`，重写 `run()` |
| 任务与线程关系 | 任务内嵌在线程类中 | 任务与线程分离 |
| 继承限制 | 受单继承限制 | 不受限制，可同时继承其他类 |
| 资源共享 | 多个线程对象不共享任务 | 多个线程可共享同一个任务对象 |
| 代码耦合 | 较高 | 较低 |
| 推荐程度 | 不推荐 | 推荐使用 |

## 五、资源共享示例

实现 `Runnable` 接口的方式更适合多个线程共享同一份数据。

```java
public class TicketTask implements Runnable {
    private int ticket = 10;

    @Override
    public void run() {
        while (ticket > 0) {
            System.out.println(Thread.currentThread().getName() + " 卖出第 " + ticket-- + " 张票");
        }
    }
}

public class TicketDemo {
    public static void main(String[] args) {
        TicketTask task = new TicketTask();

        new Thread(task, "窗口A").start();
        new Thread(task, "窗口B").start();
        new Thread(task, "窗口C").start();
    }
}
```

在这个例子中，三个线程共享同一个 `TicketTask` 对象中的 `ticket` 变量，共同售卖 10 张票。如果使用继承 `Thread` 的方式，每个线程对象都会拥有一份独立的 `ticket` 变量，无法达到资源共享的效果。

## 六、推荐使用 Runnable 接口

虽然继承 `Thread` 类的方式写起来更直接，但在实际开发中，更推荐使用 `Runnable` 接口，原因如下：

1. **解耦**：将线程的执行逻辑（任务）与线程本身分离开。
2. **避免单继承限制**：Java 类只能继承一个父类，但可以实现多个接口。
3. **便于资源共享**：多个线程可以共享同一个 `Runnable` 实现对象。
4. **更符合面向对象设计**：任务本身不是线程，任务只是线程要执行的内容。

## 七、总结

Java 创建线程的两种方式各有特点：

- **继承 `Thread` 类**：简单直接，但扩展性差，任务与线程耦合。
- **实现 `Runnable` 接口**：结构清晰、扩展性好、支持资源共享，是实际开发中的首选方式。

通过本次学习，我更加理解了两种线程创建方式的本质区别。在后续的多线程学习中，我会优先使用 `Runnable` 接口来编写线程任务。
