---
title: Java核心基础之--清空整个文件夹（目录）
author: 李朋军
avatar: https://cdn.jsdelivr.net/gh/pengjunlee/picture-warehouse/img/logo.jpg
authorLink: pengjunlee
authorAbout: 为他人带来阳光的人，自己也一定会沐浴在阳光下。
authorDesc: 时间是最好的检查者，而耐心才是最高明的指导者。
categories: 编程
comments: true
date: 2020-06-06 17:13:10
tags: Java,清空,文件夹
keywords: Java,清空,文件夹
description: 清空整个文件夹（目录）
photos: https://cdn.jsdelivr.net/gh/pengjunlee/picture-warehouse/java-core/jn.png
---
**目标**：清空整个文件夹

**条件**：file类、递归方法

**备注**：在Java中，如果想要删除一个文件夹，那么必须确保该文件下已被清空代码

	package May.Eighth.File;

	import java.io.File;
	/**
	 * 删除文件和目录（文件夹）
	 * @author Jia
	 *
	 */
	public class DeleteFileAndDirectory {
		public static void main(String[] args) {
			//调用delete方法
			new DeleteFileAndDirectory().delete("E:/Javatest/test");
			System.out.println("该文件夹已清空完毕");
		}
		// 创建目录（文件夹）删除的方法
		public void delete(String path) {
			// 为传进来的路径参数创建一个文件对象
			File file = new File(path);
			// 如果目标路径是一个文件，那么直接调用delete方法删除即可
			// file.delete();
			// 如果是一个目录，那么必须把该目录下的所有文件和子目录全部删除，才能删除该目标目录，这里要用到递归函数
			// 创建一个files数组，用来存放目标目录下所有的文件和目录的file对象
			File[] files = new File[50];
			// 将目标目录下所有的file对象存入files数组中
			files = file.listFiles();
			// 循环遍历files数组
			for(File temp : files){.
				// 判断该temp对象是否为文件对象
				if (temp.isFile()) {
					temp.delete();
				}
				// 判断该temp对象是否为目录对象
				if (temp.isDirectory()) {
					// 将该temp目录的路径给delete方法（自己），达到递归的目的
					delete(temp.getAbsolutePath());
					// 确保该temp目录下已被清空后，删除该temp目录
					temp.delete();
				}
			}
		}
	}
 