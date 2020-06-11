---
title: Java核心基础之--获取302响应中的Location信息
author: 李朋军
avatar: https://cdn.jsdelivr.net/gh/pengjunlee/picture-warehouse/img/logo.jpg
authorLink: pengjunlee
authorAbout: 为他人带来阳光的人，自己也一定会沐浴在阳光下。
authorDesc: 时间是最好的检查者，而耐心才是最高明的指导者。
categories: 编程
comments: true
date: 2020-06-06 16:52:10
tags: 302,Java,Location
keywords: 302,Java,Location
description: 获取302响应中的Location信息
photos: https://cdn.jsdelivr.net/gh/pengjunlee/picture-warehouse/java-core/j3.png
---
# HttpClient获取302响应中的Location头信息

	public static String getLocationUrl(String url) {
		RequestConfig config = RequestConfig.custom().setConnectTimeout(50000).setConnectionRequestTimeout(10000).setSocketTimeout(50000)
                .setRedirectsEnabled(false).build();//不允许重定向   
		CloseableHttpClient httpClient = HttpClients.custom().setDefaultRequestConfig(config).build(); 
		String location = null;
		int responseCode = 0;
 
		HttpResponse response;
		try {
			response = httpClient.execute(new HttpGet(url));
			responseCode = response.getStatusLine().getStatusCode();
			if (responseCode == 302) {
				Header locationHeader = response.getFirstHeader("Location");
				location = locationHeader.getValue();
			}
		} catch (Exception e) {
			
			e.printStackTrace();
		}
		
		return location;
	}