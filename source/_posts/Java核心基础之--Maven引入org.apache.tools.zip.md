---
title: Java核心基础之--Maven引入org.apache.tools.zip
author: 李朋军
avatar: https://cdn.jsdelivr.net/gh/pengjunlee/picture-warehouse/img/logo.jpg
authorLink: pengjunlee
authorAbout: 为他人带来阳光的人，自己也一定会沐浴在阳光下。
authorDesc: 时间是最好的检查者，而耐心才是最高明的指导者。
categories: 编程
comments: true
date: 2020-06-06 16:58:10
tags: Java,maven
keywords: Java,maven
description: Maven引入org.apache.tools.zip
photos: https://cdn.jsdelivr.net/gh/pengjunlee/picture-warehouse/java-core/j9.png
---
可以看出 `org.apache.tools.zip` 是 `ant-**.jar` 里面的。

<div align=center>![zip示意图](https://cdn.jsdelivr.net/gh/pengjunlee/picture-warehouse/java-core/35.png "zip示意图")
<div align=left>

所以要引入`org.apache.tools.zip`，直接maven引入`ant`即可。

	<!-- https://mvnrepository.com/artifact/org.apache.ant/ant -->
	<dependency>
		<groupId>org.apache.ant</groupId>
		<artifactId>ant</artifactId>
		<version>1.10.5</version>
	</dependency>