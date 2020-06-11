---
title: 使用net.sf.json遍历Json数组
author: 李朋军
avatar: https://cdn.jsdelivr.net/gh/pengjunlee/picture-warehouse/img/logo.jpg
authorLink: pengjunlee
authorAbout: 为他人带来阳光的人，自己也一定会沐浴在阳光下。
authorDesc: 时间是最好的检查者，而耐心才是最高明的指导者。
categories: 编程
comments: true
date: 2020-06-06 17:19:10
tags: Java,json
keywords: Java,json
description: 使用net.sf.json遍历Json数组
photos: https://cdn.jsdelivr.net/gh/pengjunlee/picture-warehouse/java-core/jt.png
---
# 使用net.sf.json遍历Json数组

	import org.junit.Test;
	import java.util.Iterator;
	import net.sf.json.JSONArray;
	import net.sf.json.JSONObject;
	 
	public class JsonArrayTest {
	 
		@SuppressWarnings("unchecked")
		@Test
		public void test1() {
			String arrStr = "[{key:'a',value:'1'},{key:'b',value:'2'},{key:'c',value:'3'}]";
			JSONArray jsonArray = JSONArray.fromObject(arrStr);
			for (int i = 0; i < jsonArray.size(); i++) {
				JSONObject jsonObj = jsonArray.getJSONObject(i);
				Iterator<String> iterator = jsonObj.keys();
				while (iterator.hasNext()) {
					String key = iterator.next();
					System.out.println(key + ":" + jsonObj.getString(key) + " ");
				}
			}
		}
	 
	}