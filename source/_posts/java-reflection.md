---
title: 了解 Java 的反射机制
cover: /img/covers/java-reflection.jpg
date: 2026-06-24 14:40:00
top_img: https://picsum.photos/seed/reflection/1920/600
tags:
  - Java
  - 反射
  - 进阶
categories:
  - Java课后作业
---

## 一、什么是反射

反射（Reflection）是 Java 的一种强大机制，它允许程序在**运行时**动态地获取类的信息，并操作类的属性和方法。通过反射，我们可以在编译期不知道某个类的情况下，依然能够创建对象、调用方法、访问字段等。

反射的核心类都位于 `java.lang.reflect` 包下，主要包括：

- `Class`：代表一个类或接口
- `Field`：代表类的字段
- `Method`：代表类的方法
- `Constructor`：代表类的构造方法
- `Modifier`：用于判断字段、方法等的修饰符

## 二、获取 Class 对象的三种方式

在 Java 中，每个类被加载后都会在 JVM 中生成一个唯一的 `Class` 对象。获取 `Class` 对象是使用反射的第一步。

```java
// 方式 1：通过类名.class
Class<?> clazz1 = Student.class;

// 方式 2：通过对象的 getClass() 方法
Student stu = new Student();
Class<?> clazz2 = stu.getClass();

// 方式 3：通过 Class.forName() 全限定类名
Class<?> clazz3 = Class.forName("com.example.Student");
```

## 三、反射的常见操作

### 1. 创建对象

```java
Class<?> clazz = Class.forName("com.example.Student");

// 使用无参构造方法创建对象
Object obj = clazz.getDeclaredConstructor().newInstance();
```

### 2. 获取并调用方法

```java
Class<?> clazz = Class.forName("com.example.Student");
Object obj = clazz.getDeclaredConstructor().newInstance();

// 获取 public 方法
Method method = clazz.getMethod("study", String.class);

// 调用方法
method.invoke(obj, "Java");
```

### 3. 获取并访问字段

```java
Class<?> clazz = Class.forName("com.example.Student");
Object obj = clazz.getDeclaredConstructor().newInstance();

// 获取私有字段
Field field = clazz.getDeclaredField("name");

// 设置可访问（暴力访问私有成员）
field.setAccessible(true);

// 设置字段值
field.set(obj, "张三");

// 获取字段值
String name = (String) field.get(obj);
System.out.println(name);
```

## 四、反射的应用场景

### 1. 框架开发

许多主流框架都大量使用了反射，例如：

- **Spring**：通过反射创建 Bean、注入依赖、调用 `@RequestMapping` 标注的方法。
- **MyBatis**：通过反射将数据库表字段映射到 Java 对象属性。
- **JUnit**：通过反射找到并执行测试方法。

### 2. 动态代理

反射是实现动态代理的基础。Java 的 `java.lang.reflect.Proxy` 可以在运行时动态生成代理类，常用于 AOP（面向切面编程）。

```java
interface Hello {
    void sayHello();
}

Hello proxy = (Hello) Proxy.newProxyInstance(
    Hello.class.getClassLoader(),
    new Class[]{Hello.class},
    (proxyObj, method, args) -> {
        System.out.println("前置增强");
        return null;
    }
);

proxy.sayHello();
```

### 3. 配置文件与类的解耦

通过反射，程序可以从配置文件中读取类名，然后动态加载并实例化，而不需要在代码中硬编码。

```java
String className = props.getProperty("driver.class");
Class<?> clazz = Class.forName(className);
Object driver = clazz.getDeclaredConstructor().newInstance();
```

## 五、反射的优缺点

| 优点 | 缺点 |
|------|------|
| 运行时可以动态操作类 | 性能开销较大 |
| 增加程序的灵活性 | 破坏封装性，可访问私有成员 |
| 是许多框架的底层基础 | 编译期无法检查类型安全 |
| 实现解耦和动态扩展 | 代码可读性和可维护性下降 |

## 六、简单示例

```java
import java.lang.reflect.Field;
import java.lang.reflect.Method;

public class ReflectionDemo {
    public static void main(String[] args) throws Exception {
        Class<?> clazz = Class.forName("java.util.ArrayList");

        System.out.println("类名: " + clazz.getName());
        System.out.println("简单类名: " + clazz.getSimpleName());

        // 获取所有 public 方法
        Method[] methods = clazz.getMethods();
        System.out.println("public 方法数量: " + methods.length);

        // 获取所有字段
        Field[] fields = clazz.getDeclaredFields();
        for (Field field : fields) {
            System.out.println("字段: " + field.getName());
        }
    }
}
```

## 七、总结

反射是 Java 中一项非常强大的技术，它为框架开发、动态代理、配置驱动编程等提供了基础能力。虽然反射会牺牲一定的性能和安全性，但在需要动态性和灵活性的场景下，它是不可或缺的工具。

通过本次学习，我理解了反射的基本概念、核心 API 以及常见应用场景。在后续的学习中，我会继续结合 Spring 等框架，深入理解反射在实际项目中的运用。
