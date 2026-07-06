---
title: Java 文件复制：字符缓冲流与字节缓冲流
cover: /img/covers/java-file-copy.jpg
date: 2026-06-24 14:45:00
top_img: https://picsum.photos/seed/filecopy/1920/600
tags:
  - Java
  - IO
  - 文件操作
  - 缓冲流
categories:
  - Java课后作业
---

## 一、Java IO 流概述

在 Java 中，IO（输入/输出）流是处理文件和网络数据传输的核心机制。按照数据单位的不同，IO 流可以分为两大类：

- **字节流**：以字节（byte）为单位读写数据，适用于任何类型的文件，如图片、音频、视频、可执行文件等。
- **字符流**：以字符（char）为单位读写数据，适用于文本文件，能够自动处理字符编码。

为了提升读写效率，Java 还提供了**缓冲流**（Buffered Stream），它在内存中开辟一个缓冲区，减少与磁盘交互的次数，从而显著提高文件复制速度。

## 二、文本文件复制：字符缓冲流

对于文本文件，最常用的是**字符缓冲流**：`BufferedReader` 和 `BufferedWriter`。它们不仅可以提高读写效率，还提供了按行读写的方法，非常方便。

```java
import java.io.*;

public class TextFileCopy {
    public static void main(String[] args) {
        String src = "source.txt";
        String dest = "copy.txt";

        try (BufferedReader br = new BufferedReader(new FileReader(src));
             BufferedWriter bw = new BufferedWriter(new FileWriter(dest))) {

            String line;
            while ((line = br.readLine()) != null) {
                bw.write(line);
                bw.newLine(); // 写入换行符
            }

            System.out.println("文本文件复制完成！");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 字符缓冲流的优点

1. **自带缓冲区**，读写效率高。
2. 提供 `readLine()` 和 `newLine()` 方法，适合按行处理文本。
3. 自动处理字符编码，避免中文乱码问题。

## 三、任意文件复制：字节缓冲流

对于非文本文件（如图片、视频、压缩包等），或者不确定文件类型时，应该使用**字节缓冲流**：`BufferedInputStream` 和 `BufferedOutputStream`。它可以复制任意类型的文件，因此被称为“万能复制”。

```java
import java.io.*;

public class AnyFileCopy {
    public static void main(String[] args) {
        String src = "photo.jpg";
        String dest = "photo_copy.jpg";

        try (BufferedInputStream bis = new BufferedInputStream(new FileInputStream(src));
             BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(dest))) {

            byte[] buffer = new byte[1024];
            int len;
            while ((len = bis.read(buffer)) != -1) {
                bos.write(buffer, 0, len);
            }

            System.out.println("任意文件复制完成！");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 字节缓冲流的优点

1. **适用范围广**，可以复制任何类型文件。
2. **自带缓冲区**，大幅减少系统调用次数。
3. 使用 1024 字节（或更大）的数组批量读写，效率更高。

## 四、缓冲流与普通流的性能对比

普通流每次读写都直接与磁盘交互，而缓冲流会先将数据读入内存缓冲区，等缓冲区满或流关闭时再一次性写入磁盘。这样可以显著减少 IO 次数，提升复制效率。

| 流类型 | 特点 | 适用场景 |
|--------|------|----------|
| 普通字节流 | 无缓冲，效率低 | 小文件或简单读写 |
| 字节缓冲流 | 有缓冲，效率高 | 任意文件复制 |
| 普通字符流 | 无缓冲，效率低 | 简单文本读写 |
| 字符缓冲流 | 有缓冲，可逐行读写 | 文本文件复制 |

## 五、完整对比示例

下面是一个同时展示两种复制方式的示例：

```java
import java.io.*;

public class FileCopyDemo {

    // 复制文本文件
    public static void copyTextFile(String src, String dest) {
        try (BufferedReader br = new BufferedReader(new FileReader(src));
             BufferedWriter bw = new BufferedWriter(new FileWriter(dest))) {
            String line;
            while ((line = br.readLine()) != null) {
                bw.write(line);
                bw.newLine();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    // 复制任意文件
    public static void copyAnyFile(String src, String dest) {
        try (BufferedInputStream bis = new BufferedInputStream(new FileInputStream(src));
             BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(dest))) {
            byte[] buffer = new byte[1024];
            int len;
            while ((len = bis.read(buffer)) != -1) {
                bos.write(buffer, 0, len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) {
        copyTextFile("source.txt", "copy.txt");
        copyAnyFile("photo.jpg", "photo_copy.jpg");
        System.out.println("全部复制完成！");
    }
}
```

## 六、注意事项

1. **文本文件用字符流，其他文件用字节流**。用字符流复制二进制文件可能导致文件损坏。
2. **用完流要及时关闭**。推荐使用 try-with-resources 语句，自动关闭资源。
3. **缓冲区大小要适当**。一般 1024 字节或 8192 字节即可，过大可能浪费内存。
4. **处理异常**。IO 操作容易出现异常，应进行适当的异常处理。

## 七、总结

本次学习总结了 Java 中两种常用的文件复制方式：

- **字符缓冲流**：最适合复制文本文件，支持按行读写。
- **字节缓冲流**：可以复制任意类型文件，是“万能复制”方案。

掌握这两种流的使用，是 Java IO 编程的基础，也是处理文件上传、下载、备份等功能的必备技能。
