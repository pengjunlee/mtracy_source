---
title: Java核心基础之--Stack
author: 李朋军
avatar: https://cdn.jsdelivr.net/gh/pengjunlee/picture-warehouse/img/logo.jpg
authorLink: pengjunlee
authorAbout: 为他人带来阳光的人，自己也一定会沐浴在阳光下。
authorDesc: 时间是最好的检查者，而耐心才是最高明的指导者。
categories: 编程
comments: true
date: 2020-06-06 17:00:10
tags: Java,Stack
keywords: Java,Stack
description: Stack
photos: https://cdn.jsdelivr.net/gh/pengjunlee/picture-warehouse/java-core/jc.png
---
在Java中Stack类表示后进先出（LIFO）的对象堆栈。栈是一种非常常见的数据结构，它采用典型的先进后出的操作方式完成的。每一个栈都包含一个栈顶，每次出栈是将栈顶的数据取出，如下：

<div align=center>![Stack示意图](https://cdn.jsdelivr.net/gh/pengjunlee/picture-warehouse/java-core/32.png "Stack示意图")
<div align=left>

Stack通过五个操作对Vector进行扩展，允许将向量视为堆栈。这个五个操作如下：

<div align=center>![Stack示意图](https://cdn.jsdelivr.net/gh/pengjunlee/picture-warehouse/java-core/33.png "Stack示意图")
<div align=left>

Stack继承Vector，他对Vector进行了简单的扩展：

	public class Stack<E> extends Vector<E>

Stack的实现非常简单，仅有一个构造方法，五个实现方法（从Vector继承而来的方法不算与其中），同时其实现的源码非常简单。

	/**
     * 构造函数
     */
    public Stack() {
    }
 
    /**
     *  push函数：将元素存入栈顶
     */
    public E push(E item) {
        // 将元素存入栈顶。
        // addElement()的实现在Vector.java中
        addElement(item);
 
        return item;
    }
 
    /**
     * pop函数：返回栈顶元素，并将其从栈中删除
     */
    public synchronized E pop() {
        E    obj;
        int    len = size();
 
        obj = peek();
        // 删除栈顶元素，removeElementAt()的实现在Vector.java中
        removeElementAt(len - 1);
 
        return obj;
    }
 
    /**
     * peek函数：返回栈顶元素，不执行删除操作
     */
    public synchronized E peek() {
        int    len = size();
 
        if (len == 0)
            throw new EmptyStackException();
        // 返回栈顶元素，elementAt()具体实现在Vector.java中
        return elementAt(len - 1);
    }
 
    /**
     * 栈是否为空
     */
    public boolean empty() {
        return size() == 0;
    }
 
    /**
     *  查找“元素o”在栈中的位置：由栈底向栈顶方向数
     */
    public synchronized int search(Object o) {
        // 获取元素索引，elementAt()具体实现在Vector.java中
        int i = lastIndexOf(o);
 
        if (i >= 0) {
            return size() - i;
        }
        return -1;
    }
 