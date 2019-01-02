title: JNI 加载本地dll类库
author: Asia Cui
tags:
  - Java工具类
categories:
  - JNI 加载本地dll类库
date: 2018-12-31 12:22:00
---
```java
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.text.SimpleDateFormat;
import java.util.Date;

/**
 * 加载本地dll类库，调用实例<code>Library.load("IESNlp.dll");</code>
 * try {
 * 		JniLibrary.loadLibrary("IESNlp.dll","2018-03-13");
 * } finally {
 *      System.out.println("Loading " + "IESNlp.dll");
 * }
 */
public class JniLibrary {

	static final String SEPARATOR;

	static {
		SEPARATOR = System.getProperty("file.separator");
	}

	/**
	 * <p>
	 * 重jar文件中抽取出dll文件
	 * </p>
	 * 
	 * @param fileName
	 *            ：抽取文件名，包含路径
	 * @param mappedName
	 *            ： dll文件名
	 * @return
	 */
	private static boolean extract(String fileName, String mappedName) {
		FileOutputStream os = null;
		InputStream is = null;
		File file = new File(fileName);
		try {
			// 如果文件存在则先进行删除操作
			if (file.exists()) {
				file.delete();
			}
			// 从jar文件中读取dll文件
			is = JniLibrary.class.getResourceAsStream("/resources/" + mappedName);
			if (is != null) {
				int read;
				byte[] buffer = new byte[4096];
				os = new FileOutputStream(fileName);
				while ((read = is.read(buffer)) != -1) {
					os.write(buffer, 0, read);
				}
				os.close();
				is.close();
				return true;
			}
		} catch (Throwable e) {
			try {
				if (os != null)
					os.close();
				if (is != null)
					is.close();
			} catch (IOException e1) {
			}
		}
		return false;
	}

	private static boolean load(String libName) {
		try {
			if (libName.indexOf(SEPARATOR) != -1) {
				System.load(libName);
				return true;
			}
		} catch (UnsatisfiedLinkError e) {
			e.printStackTrace();
		}
		return false;
	}

	public static void extract(String name) {
		String libName = name;
		// 尝试从系统临时目录加载该文件
		String path = System.getProperty("java.io.tmpdir"); //$NON-NLS-1$
		if (path != null) {
			// 创建jni文件夹
			path = path + "jni";
			File file = new File(path);
			path = file.getAbsolutePath();
			// 文件夹是否存在
			if (!file.exists()) {
				if (!file.mkdirs()) {
					throw new UnsatisfiedLinkError("创建文件夹" + path + "失败");
				}
			}
			String fileName = path + SEPARATOR + libName;
			file = new File(fileName);
			if (!file.exists()) {
				extract(fileName, libName);
			}
		}
	}

	/**
	 * 加载类库
	 * 
	 * @param name: dll名称
	 * @param expdate: dll最后修改日期.格式为:yyyy-MM-dd.如:2009-06-11
	 */
	public static void loadLibrary(String name, String expdate) {
		String libName = name;
		// 尝试从系统临时目录加载该文件
		String path = System.getProperty("java.io.tmpdir"); //$NON-NLS-1$
		if (path != null) {
			// 创建jni文件夹
			path = path + "jni";
			File file = new File(path);
			path = file.getAbsolutePath();
			// 文件夹是否存在
			if (!file.exists()) {
				if (!file.mkdirs()) {
					throw new UnsatisfiedLinkError("创建文件夹" + path + "失败");
				}
			}
			String fileName = path + SEPARATOR + libName;
			file = new File(fileName);
			// 判断文件是否存在，不存在进行重新生成
			Date date = new Date(file.lastModified());
			SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
			String modifyDate = sdf.format(date);
			if (!file.exists() || !modifyDate.equals(expdate)) {
				if (extract(fileName, libName)) {
					load(fileName);
					return;
				}
			}
			if (load(fileName))
				return;
		}
		// 加载失败，从临时目录和jar文件都都没有找到该文件
		throw new UnsatisfiedLinkError("no " + libName + " in java.io.tmpdir or the jar file");
	}

}

```