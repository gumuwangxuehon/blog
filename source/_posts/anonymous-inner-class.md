---
title: 匿名内部类的使用场景
cover: /img/covers/anonymous-inner-class.jpg
date: 2026-06-24 14:35:00
top_img: https://picsum.photos/seed/innerclass/1920/600
tags:
  - Java
  - 匿名内部类
  - 回调
  - 多线程
categories:
  - Java课后作业
---

## 一、什么是匿名内部类

匿名内部类（Anonymous Inner Class）是 Java 中的一种特殊内部类，它没有显式的类名，通常用于在创建对象的同时实现某个接口或继承某个类。它的最大特点是**简洁**——可以在需要的地方直接定义并实例化，而不需要单独写一个类文件。

基本语法：

```java
new 父类或接口() {
    // 重写方法或实现方法
};
```

## 二、典型使用场景

### 场景 1：作为回调函数使用

在事件驱动编程中，我们经常需要为某个事件注册一个回调。使用匿名内部类可以避免为每个回调都创建一个独立的类。

```java
import javax.swing.JButton;
import javax.swing.JFrame;

public class CallbackDemo {
    public static void main(String[] args) {
        JFrame frame = new JFrame("匿名内部类示例");
        JButton button = new JButton("点击我");

        // 使用匿名内部类为按钮添加点击事件监听器
        button.addActionListener(new java.awt.event.ActionListener() {
            @Override
            public void actionPerformed(java.awt.event.ActionEvent e) {
                System.out.println("按钮被点击了！");
            }
        });

        frame.add(button);
        frame.setSize(300, 200);
        frame.setVisible(true);
    }
}
```

在这个例子中，`ActionListener` 是一个接口，我们直接用匿名内部类实现了它的 `actionPerformed` 方法，代码紧凑且直观。

### 场景 2：简化线程创建

在 Java 多线程编程中，可以使用匿名内部类快速创建 `Thread` 或实现 `Runnable`。

```java
public class ThreadDemo {
    public static void main(String[] args) {
        // 使用匿名内部类创建 Runnable
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 5; i++) {
                    System.out.println("子线程输出: " + i);
                }
            }
        };

        Thread thread = new Thread(runnable);
        thread.start();
    }
}
```

更简洁的写法：

```java
new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("这是一个匿名内部类线程");
    }
}).start();
```

### 场景 3：实现简单的接口或抽象类

当我们只需要某个接口或抽象类的一个简单实现时，匿名内部类非常方便。

例如，计算一个方法执行了多少秒，可以使用匿名内部类实现一个计时器：

```java
public class TimerDemo {
    public static void main(String[] args) {
        // 使用匿名内部类实现一个计时任务
        Task task = new Task() {
            @Override
            public void execute() {
                // 模拟一个耗时操作
                try {
                    Thread.sleep(1234);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        };

        long start = System.currentTimeMillis();
        task.execute();
        long end = System.currentTimeMillis();

        System.out.println("方法执行了 " + (end - start) / 1000.0 + " 秒");
    }
}

abstract class Task {
    public abstract void execute();
}
```

输出：

```
方法执行了 1.234 秒
```

这种方式将具体任务逻辑与计时逻辑分离，同时通过匿名内部类避免了创建大量临时类文件。

## 三、匿名内部类的优缺点

| 优点 | 缺点 |
|------|------|
| 代码简洁，减少类文件数量 | 代码可读性较差， especially 当逻辑复杂时 |
| 可以直接访问外部类的成员 | 不能定义构造方法 |
| 适合一次性、简单的实现 | 不利于复用和单元测试 |

## 四、与 Lambda 表达式的关系

从 Java 8 开始，函数式接口（只有一个抽象方法的接口）可以用 Lambda 表达式进一步简化匿名内部类的写法。

例如前面的 `Runnable` 例子可以写成：

```java
new Thread(() -> System.out.println("这是一个 Lambda 线程")).start();
```

但需要注意的是，Lambda 只能用于函数式接口，而匿名内部类可以作用于任何接口、抽象类或具体类，适用范围更广。

## 五、总结

匿名内部类是 Java 中非常实用的语法糖，特别适合以下场景：

1. **回调函数**：如事件监听器、按钮点击等。
2. **快速创建线程**：实现 `Runnable` 接口。
3. **一次性实现接口或抽象类**：如计时任务、比较器等。

虽然 Lambda 表达式在很多情况下可以替代匿名内部类，但理解匿名内部类仍然是掌握 Java 面向对象编程的重要基础。
