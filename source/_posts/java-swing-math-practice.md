---
title: 面向对象程序设计实验报告
date: 2026-07-01 00:00:00
cover: /img/covers/java-swing-math-practice.jpg
categories:
  - Java实验报告
tags:
  - Java
  - Swing
  - GUI
  - 实验报告
---

# 面向对象程序设计实验报告
**实验日期**：2026年 7 月1 日
## 一、实验目的：
1.掌握 Java Swing 图形界面核心组件（窗口、标签、输入框、按钮、下拉选择框）的创建、属性设置与布局管理方法，理解 GUI 程序的界面分层设计思路。
2.理解 Java 事件处理完整机制，分清事件源、事件对象、事件监听器、事件处理接口四大核心要素，熟练使用ActionListener完成按钮、输入框、下拉框的动作监听。
3.结合随机数、分支运算逻辑，实现分层难度的四则运算出题功能，完成整数、小数、分数多类型算术表达式计算，巩固 Java 基础语法与数值运算。
## 二、实验环境：
java

## 三、实验内容：
相关知识点
GUI相关的类，组件的属性及功能，事件源、监视器、处理事件的接口
实验目的
学习掌握事件处理
实验要求
按以下需求（可扩充），设计并完成一个能运行的且界面美观的小软件。提交可运行软件
程序主要针对小学生的算术计算。
可以自定义计算的难度（此项可根据功能进行扩展）
随机获取不一样的题目，能通过按键触发确定填写输入的答案是否正确。
计算满足+ - *  /(可扩展）
操作数可以是整数、小数、分数等等（可扩展）
## 实验代码

```java
package org.example;
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.Random;
public class MathPractice extends JFrame {
// 界面组件
private JComboBox<String> difficultyBox;
private JLabel questionLabel;
private JTextField answerInput;
private JButton submitBtn, nextBtn;
private JLabel resultTip, scoreLabel;
// 运算数据
private final Random random = new Random();
private int score = 0;
private int num1, num2;
private double d1, d2;
private String op;
private int type; // 0整数 1小数 2分数
private final String[] ops = {"+", "-", "*", "/"};
public MathPractice() {
initUI();
generateQuestion();
}
// 初始化界面
private void initUI() {
setTitle("小学生算术练习系统");
setSize(450, 320);
setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
setLocationRelativeTo(null);
setLayout(new BorderLayout(10, 10));
setResizable(false);
// 顶部难度选择面板
JPanel topPanel = new JPanel();
topPanel.add(new JLabel("选择难度："));
String[] levels = {"简单-整数", "中等-小数", "进阶-分数"};
difficultyBox = new JComboBox<>(levels);
topPanel.add(difficultyBox);
scoreLabel = new JLabel("当前得分：0");
topPanel.add(scoreLabel);
add(topPanel, BorderLayout.NORTH);
// 中间题目展示面板
JPanel centerPanel = new JPanel();
centerPanel.setLayout(new GridLayout(3, 1, 5, 10));
centerPanel.setBorder(BorderFactory.createEmptyBorder(20, 30, 20, 30));
questionLabel = new JLabel("题目加载中", SwingConstants.CENTER);
questionLabel.setFont(new Font("黑体", Font.BOLD, 24));
JPanel inputPanel = new JPanel();
inputPanel.add(new JLabel("你的答案："));
answerInput = new JTextField(12);
answerInput.setFont(new Font("宋体", Font.PLAIN, 18));
inputPanel.add(answerInput);
resultTip = new JLabel(" ", SwingConstants.CENTER);
resultTip.setFont(new Font("宋体", Font.BOLD, 18));
centerPanel.add(questionLabel);
centerPanel.add(inputPanel);
centerPanel.add(resultTip);
add(centerPanel, BorderLayout.CENTER);
// 底部按钮面板
JPanel btnPanel = new JPanel();
submitBtn = new JButton("提交答案");
nextBtn = new JButton("下一题");
btnPanel.add(submitBtn);
btnPanel.add(nextBtn);
add(btnPanel, BorderLayout.SOUTH);
bindEvent();
}
private void bindEvent() {
submitBtn.addActionListener(this::checkAnswer);
answerInput.addActionListener(this::checkAnswer);
// Java8不支持下划线参数，改用(e) ->
nextBtn.addActionListener(e -> {
generateQuestion();
answerInput.setText("");
resultTip.setText("");
});
difficultyBox.addActionListener(e -> {
generateQuestion();
answerInput.setText("");
resultTip.setText("");
});
}
private void generateQuestion() {
int level = difficultyBox.getSelectedIndex();
op = ops[random.nextInt(4)];
switch (level) {
case 0:
type = 0;
num1 = random.nextInt(20) + 1;
num2 = random.nextInt(20) + 1;
if (op.equals("-") && num1 < num2) {
int t = num1; num1 = num2; num2 = t;
}
if (op.equals("/")) {
num2 = random.nextInt(10) + 1;
num1 = num2 * (random.nextInt(10) + 1);
}
questionLabel.setText(num1 + " " + op + " " + num2 + " = ?");
break;
case 1:
type = 1;
d1 = Math.round((random.nextDouble() * 20 + 1) * 10.0) / 10.0;
d2 = Math.round((random.nextDouble() * 10 + 1) * 10.0) / 10.0;
if (op.equals("-") && d1 < d2) {
double t = d1; d1 = d2; d2 = t;
}
if (op.equals("/")) {
d2 = Math.round((random.nextDouble() * 5 + 1) * 10.0) / 10.0;
d1 = Math.round(d2 * (random.nextInt(8) + 1) * 10.0) / 10.0;
}
questionLabel.setText(d1 + " " + op + " " + d2 + " = ?");
break;
case 2:
type = 2;
num1 = random.nextInt(9) + 1;
num2 = random.nextInt(9) + 1;
int n3 = random.nextInt(9) + 1;
int n4 = random.nextInt(9) + 1;
if (op.equals("-") && (double)num1/num2 < (double)n3/n4) {
int t1 = num1, t2 = num2;
num1 = n3; num2 = n4;
n3 = t1; n4 = t2;
}
questionLabel.setText(num1 + "/" + num2 + " " + op + " " + n3 + "/" + n4 + " = ?（小数作答）");
break;
}
}
// 修复点：把 _ 改为合法命名参数 e，兼容Java8
private void checkAnswer(ActionEvent e) {
String inputStr = answerInput.getText().trim();
if (inputStr.isEmpty()) {
resultTip.setText("请输入答案！");
resultTip.setForeground(Color.ORANGE);
return;
}
double userAns;
try {
userAns = Double.parseDouble(inputStr);
} catch (Exception ex) {
resultTip.setText("输入格式错误，请输入数字");
resultTip.setForeground(Color.RED);
return;
}
double realAns = 0;
switch (type) {
case 0:
realAns = calcInt(num1, num2, op);
break;
case 1:
realAns = calcDouble(d1, d2, op);
break;
case 2:
double f1 = (double) num1 / num2;
int n3 = random.nextInt(9)+1;
int n4 = random.nextInt(9)+1;
double f2 = (double)n3 / n4;
realAns = calcDouble(f1, f2, op);
break;
}
if (Math.abs(userAns - realAns) < 0.01) {
score += 10;
scoreLabel.setText("当前得分：" + score);
resultTip.setText("回答正确！+10分");
resultTip.setForeground(new Color(0, 150, 0));
} else {
resultTip.setText("回答错误，正确答案：" + String.format("%.2f", realAns));
resultTip.setForeground(Color.RED);
}
}
private int calcInt(int a, int b, String op) {
return switch (op) {
case "+" -> a + b;
case "-" -> a - b;
case "*" -> a * b;
case "/" -> a / b;
default -> 0;
};
}
private double calcDouble(double a, double b, String op) {
return switch (op) {
case "+" -> a + b;
case "-" -> a - b;
case "*" -> a * b;
case "/" -> a / b;
default -> 0;
};
}
public static void main(String[] args) {
SwingUtilities.invokeLater(() -> new MathPractice().setVisible(true));
}
}
```

## 四、心得体会：
本次实验将 Java 基础语法、面向对象、Swing 图形、事件处理多块知识融合，不再是孤立学习知识点。独立完成一套完整可运行的桌面软件后，我熟悉了小型桌面工具的开发思路，也认识到自己在逻辑简化、代码复用方面仍有提升空间。后续我会尝试扩展功能，比如增加错题保存、计时答题模块，进一步巩固 Swing 与 Java 综合开发能力。