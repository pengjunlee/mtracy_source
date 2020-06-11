---
title: Java核心基础之--数组初始化填充
author: 李朋军
avatar: https://cdn.jsdelivr.net/gh/pengjunlee/picture-warehouse/img/logo.jpg
authorLink: pengjunlee
authorAbout: 为他人带来阳光的人，自己也一定会沐浴在阳光下。
authorDesc: 时间是最好的检查者，而耐心才是最高明的指导者。
categories: 编程
comments: true
date: 2020-06-06 17:08:10
tags: Java,数组,初始化
keywords: Java,数组,初始化
description: 数组初始化填充
photos: https://cdn.jsdelivr.net/gh/pengjunlee/picture-warehouse/java-core/ji.png
---
# 对数组进行初始化填充

	import java.util.Arrays;
	 
	public class ArrayFilling {
	 
		public static void main(String[] args) {
	 
			int[] scoreArr = new int[8]; // 创建一个大小为8的数组
	 
			Arrays.fill(scoreArr, 0); // 将数组使用数字 0 进行填充
	 
			for (int i = 0; i < scoreArr.length; i++) {
				System.out.print(scoreArr[i] + " ");
			}
	                System.out.print("");
			Arrays.fill(scoreArr, 2, 6, 1);// 将索引从 2 到 6 使用数字 1 进行填充
			for (int i = 0; i < scoreArr.length; i++) {
				System.out.print(scoreArr[i] + " ");
			}
		}
	}

 打印结果：

`0 0 0 0 0 0 0 0`  
`0 0 1 1 1 1 0 0`  