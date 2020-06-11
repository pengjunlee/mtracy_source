---
title: Java核心基础之--Zip压缩工具类（二）
author: 李朋军
avatar: https://cdn.jsdelivr.net/gh/pengjunlee/picture-warehouse/img/logo.jpg
authorLink: pengjunlee
authorAbout: 为他人带来阳光的人，自己也一定会沐浴在阳光下。
authorDesc: 时间是最好的检查者，而耐心才是最高明的指导者。
categories: 编程
comments: true
date: 2020-06-06 17:03:10
tags: Java,Zip
keywords: Java,Zip
description: Zip压缩工具类（二）
photos: https://cdn.jsdelivr.net/gh/pengjunlee/picture-warehouse/java-core/jf.png
---
在**Java**项目中需要对文件夹的内容进行**Zip**压缩，参考了网上的代码并修复了里面的一些问题，例如：中文乱码、目录不存在异常。

所使用的**Jar**包：`ant-*.*.*.jar` 和 `log4j-*.*.*.jar`。

# ZipUtil工具类源码

	import java.io.File;
	import java.io.FileInputStream;
	import java.io.FileOutputStream;
	import java.io.IOException;
	import java.io.InputStream;
	import java.util.Enumeration;
	 
	import org.apache.log4j.Logger;
	import org.apache.tools.zip.ZipEntry;
	import org.apache.tools.zip.ZipFile;
	import org.apache.tools.zip.ZipOutputStream;
	 
	public class ZipUtil {
	 
		private static final Logger logger = Logger.getLogger(ZipUtil.class);
	 
		/**
		 * 压缩文件或目录
		 * 
		 * @param src
		 *            要压缩的源文件或目录
		 * @param dest
		 *            生成的压缩文件全名，这里是绝对路径
		 * @param encoding
		 *            压缩时采用的编码格式
		 * @throws IOException
		 */
		public static void zip(String src, String dest, String encoding) {
			ZipOutputStream out = null;
			if (isEmptyStr(src) || isEmptyStr(dest)) {
				logger.error("invalid compress parameters...");
				return;
			}
			if (isEmptyStr(encoding)) {
				encoding = "UTF-8";
			}
			try {
				File outFile = new File(dest);
	 
				File parentFile = outFile.getParentFile();
				if (!parentFile.exists()) {
					parentFile.mkdirs();
				}
				out = new ZipOutputStream(outFile);
				out.setEncoding(encoding);
				File fileOrDirectory = new File(src);
	 
				if (fileOrDirectory.isFile()) {
					zipFileOrDirectory(out, fileOrDirectory, "");
				} else {
					File[] entries = fileOrDirectory.listFiles();
					for (int i = 0; i < entries.length; i++) {
						// 递归压缩，更新curPaths
						zipFileOrDirectory(out, entries[i], "");
					}
				}
	 
			} catch (IOException e) {
				logger.error(e.getMessage());
			} finally {
				if (null != out) {
					try {
						out.close();
					} catch (IOException e) {
						logger.error(e.getMessage());
					}
				}
			}
		}
	 
		private static boolean isEmptyStr(String str) {
			return str == null || str.length() == 0;
		}
	 
		/**
		 * 递归压缩文件或目录
		 * 
		 * @param out
		 *            压缩输出流对象
		 * @param file
		 *            要压缩的文件或目录对象
		 * @param curPath
		 *            当前压缩条目的路径，用于指定条目名称的前缀
		 * @throws IOException
		 */
		private static void zipFileOrDirectory(ZipOutputStream out, File file,
				String curPath) {
			FileInputStream in = null;
			try {
				if (!file.isDirectory()) {
					// 压缩文件
					byte[] buffer = new byte[1024 * 1024];
					int len;
					in = new FileInputStream(file);
	 
					ZipEntry entry = new ZipEntry(curPath + file.getName());
					out.putNextEntry(entry);
	 
					while ((len = in.read(buffer)) != -1) {
						out.write(buffer, 0, len);
					}
					out.closeEntry();
				} else {
					// 压缩目录
					File[] entries = file.listFiles();
					for (int i = 0; i < entries.length; i++) {
						// 递归压缩，更新curPath
						zipFileOrDirectory(out, entries[i],
								curPath + file.getName() + "/");
					}
				}
			} catch (IOException e) {
				logger.error(e.getMessage());
			} finally {
				if (null != in) {
					try {
						in.close();
					} catch (IOException e) {
						logger.error(e.getMessage());
					}
				}
			}
		}
	 
		/**
		 * 解压缩
		 * 
		 * @param zipFileName
		 *            源文件
		 * @param outputDirectory
		 *            解压缩后文件存放的目录
		 * @throws IOException
		 */
		@SuppressWarnings({ "rawtypes" })
		public static void unzip(String zipFileName, String outputDirectory,
				String encoding) {
	 
			ZipFile zipFile = null;
			if (isEmptyStr(encoding)) {
				encoding = "UTF-8";
			}
			try {
				zipFile = new ZipFile(zipFileName, encoding);
				Enumeration zipEntries = zipFile.getEntries();
				ZipEntry zipEntry = null;
	 
				File dest = new File(outputDirectory);
				if (!dest.exists()) {
					dest.mkdirs();
				}
	 
				while (zipEntries.hasMoreElements()) {
					zipEntry = (ZipEntry) zipEntries.nextElement();
	 
					String entryName = new String(zipEntry.getName().getBytes(
							encoding), encoding);
	 
					InputStream in = null;
					FileOutputStream out = null;
	 
					try {
						if (zipEntry.isDirectory()) {
							String name = zipEntry.getName();
							name = name.substring(0, name.length() - 1);
	 
							File f = new File(outputDirectory + File.separator
									+ name);
							f.mkdirs();
						} else {
							int index = entryName.lastIndexOf("\\");
							if (index != -1) {
								File df = new File(outputDirectory + File.separator
										+ entryName.substring(0, index));
								df.mkdirs();
							}
							index = entryName.lastIndexOf("/");
							if (index != -1) {
								File df = new File(outputDirectory + File.separator
										+ entryName.substring(0, index));
								df.mkdirs();
							}
	 
							File f = new File(outputDirectory + File.separator
									+ zipEntry.getName());
							// f.createNewFile();
							in = zipFile.getInputStream(zipEntry);
							out = new FileOutputStream(f);
	 
							int len;
							byte[] buffer = new byte[1024 * 1024];
	 
							while ((len = in.read(buffer)) != -1) {
								out.write(buffer, 0, len);
							}
							out.flush();
						}
					} catch (IOException e) {
						logger.error("解压失败...");
						logger.error(e.getMessage());
					} finally {
						if (null != in) {
							try {
								in.close();
							} catch (IOException e) {
								logger.error(e.getMessage());
							}
						}
						if (null != out) {
							try {
								out.close();
							} catch (IOException e) {
								logger.error(e.getMessage());
							}
						}
					}
				}
	 
			} catch (IOException e) {
				logger.error("解压失败...");
				logger.error(e.getMessage());
			} finally {
				if (null != zipFile) {
					try {
						zipFile.close();
					} catch (IOException e) {
						logger.error(e.getMessage());
					}
				}
			}
	 
		}
	 
		public static void main(String[] args) {
			ZipUtil.zip("D:\\sourceFolder", "D:\\targetFolder\\test.zip", "GBK");
			ZipUtil.unzip("D:\\targetFolder\\test.zip", "D:\\test", "GBK");
	 
		}
	 
	}