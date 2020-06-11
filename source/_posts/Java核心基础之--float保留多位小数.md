---
title: Java核心基础之--float保留多位小数
author: 李朋军
avatar: https://cdn.jsdelivr.net/gh/pengjunlee/picture-warehouse/img/logo.jpg
authorLink: pengjunlee
authorAbout: 为他人带来阳光的人，自己也一定会沐浴在阳光下。
authorDesc: 时间是最好的检查者，而耐心才是最高明的指导者。
categories: 编程
comments: true
date: 2020-06-06 16:55:10
tags: Java,float,保留小数
keywords: Java,float,保留小数
description: float保留多位小数
photos: https://cdn.jsdelivr.net/gh/pengjunlee/picture-warehouse/java-core/j6.png
---
# 方法1
用`Math.round`计算,这里返回的数字格式的。

	float price=89.89;
	int itemNum=3;
	float totalPrice=price*itemNum;
	float num=(float)(Math.round(totalPrice*100)/100);//如果要求精确4位就*10000然后/10000

# 方法2
用`DecimalFormat` 返回的是String格式的.该类对十进制进行全面的封装。像%号,千分位，小数精度，科学计算。

	float price=1.2;
	DecimalFormat decimalFormat=new DecimalFormat(".00");//构造方法的字符格式这里如果小数不足2位,会以0补足.
	String p=decimalFomat.format(price);//format 返回的是字符串

个人觉得在前台显示金额方面的还是用第二种方式。理由很简单是字符串格式的。