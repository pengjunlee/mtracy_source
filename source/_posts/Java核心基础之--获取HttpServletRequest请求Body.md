---
title: Java核心基础之--获取HttpServletRequest请求Body
author: 李朋军
avatar: https://cdn.jsdelivr.net/gh/pengjunlee/picture-warehouse/img/logo.jpg
authorLink: pengjunlee
authorAbout: 为他人带来阳光的人，自己也一定会沐浴在阳光下。
authorDesc: 时间是最好的检查者，而耐心才是最高明的指导者。
categories: 编程
comments: true
date: 2020-06-06 17:10:10
tags: Java,HttpServletRequest,Body
keywords: Java,HttpServletRequest,Body
description: 获取HttpServletRequest请求Body
photos: https://cdn.jsdelivr.net/gh/pengjunlee/picture-warehouse/java-core/jk.png
---
在实际开发过程中，经常需要从 `HttpServletRequest` 中读取**HTTP**请求的**body**内容，俗话说的好”好记性不如烂笔头“，特在此将其读取方法记录一下。 

	import java.io.BufferedReader;
	import java.io.IOException;
	import java.io.InputStream;
	 
	import javax.servlet.ServletInputStream;
	import javax.servlet.http.HttpServletRequest;
	 
	public class HttpServletRequestReader
	{
	 
	    // 字符串读取
	    // 方法一
	    public static String ReadAsChars(HttpServletRequest request)
	    {
	 
	        BufferedReader br = null;
	        StringBuilder sb = new StringBuilder("");
	        try
	        {
	            br = request.getReader();
	            String str;
	            while ((str = br.readLine()) != null)
	            {
	                sb.append(str);
	            }
	            br.close();
	        }
	        catch (IOException e)
	        {
	            e.printStackTrace();
	        }
	        finally
	        {
	            if (null != br)
	            {
	                try
	                {
	                    br.close();
	                }
	                catch (IOException e)
	                {
	                    e.printStackTrace();
	                }
	            }
	        }
	        return sb.toString();
	    }
	 
	    // 方法二
	    public static void ReadAsChars2(HttpServletRequest request)
	    {
	        InputStream is = null;
	        try
	        {
	            is = request.getInputStream();
	            StringBuilder sb = new StringBuilder();
	            byte[] b = new byte[4096];
	            for (int n; (n = is.read(b)) != -1;)
	            {
	                sb.append(new String(b, 0, n));
	            }
	        }
	        catch (IOException e)
	        {
	            e.printStackTrace();
	        }
	        finally
	        {
	            if (null != is)
	            {
	                try
	                {
	                    is.close();
	                }
	                catch (IOException e)
	                {
	                    e.printStackTrace();
	                }
	            }
	        }
	 
	    }
	 
	    // 二进制读取
	    public static byte[] readAsBytes(HttpServletRequest request)
	    {
	 
	        int len = request.getContentLength();
	        byte[] buffer = new byte[len];
	        ServletInputStream in = null;
	 
	        try
	        {
	            in = request.getInputStream();
	            in.read(buffer, 0, len);
	            in.close();
	        }
	        catch (IOException e)
	        {
	            e.printStackTrace();
	        }
	        finally
	        {
	            if (null != in)
	            {
	                try
	                {
	                    in.close();
	                }
	                catch (IOException e)
	                {
	                    e.printStackTrace();
	                }
	            }
	        }
	        return buffer;
	    }
	 
	}

<font color=red>注意：HttpServletRequest 请求中的 body 内容仅能调用 request.getInputStream()， request.getReader()和request.getParameter("key") 方法读取一次，重复读取会报 java.io.IOException: Stream closed 异常。 </font>