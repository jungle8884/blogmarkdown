---
title: Java-Note20
categories:
  - Java
tags:
  - File
  - BufferStream
author:
  - Jungle
date: 2020-12-14 15:19:37

---

# File：文件和目录路径名的抽象表示 #

1. 文件和目录是可以通过File封装成对象的
2. 对于File而言，其封装的并不是一个真正存在的文件，仅仅是一个路径名而已。
	 - 它可以是存在的，也可以是不存在的。
     - 将来是要通过具体的操作把这个路径的内容转换为具体存在的

**构造方法：**

1. File(String pathname)：通过将给定的路径名字符串转换为抽象路径名来创建新的 File实例。
2. File(String parent, String child)：从父路径名字符串和子路径名字符串创建新的 File实例。
3. File(File parent, String child)：从父抽象路径名和子路径名字符串创建新的 File实例。


## File类创建功能 ##


        public boolean createNewFile()：当具有该名称的文件不存在时，创建一个由该抽象路径名命名的新空文件
            如果文件不存在，就创建文件，并返回true
            如果文件存在，就不创建文件，并返回false

        public boolean mkdir()：创建由此抽象路径名命名的目录
            如果目录不存在，就创建目录，并返回true
            如果目录存在，就不创建目录，并返回false

        public boolean mkdirs()：创建由此抽象路径名命名的目录，包括任何必需但不存在的父目录
            如果目录不存在，就创建目录，并返回true
            如果目录存在，就不创建目录，并返回false

## File类的判断和获取功能 ##
        public boolean isDirectory()：测试此抽象路径名表示的File是否为目录
        public boolean isFile()：测试此抽象路径名表示的File是否为文件
        public boolean exists()：测试此抽象路径名表示的File是否存在

        public String getAbsolutePath()：返回此抽象路径名的绝对路径名字符串
        public String getPath()：将此抽象路径名转换为路径名字符串
        public String getName()：返回由此抽象路径名表示的文件或目录的名称

        public String[] list()：返回此抽象路径名表示的目录中的文件和目录的名称字符串数组
        public File[] listFiles()：返回此抽象路径名表示的目录中的文件和目录的File对象数组

## File类删除功能 ##
public boolean delete()：删除由此抽象路径名表示的文件或目录

			public class FileDemo03 {
			    public static void main(String[] args) throws IOException {
			        //需求1：在当前模块目录下创建java.txt文件
			        File f1 = new File("myFile\\java.txt");
			        System.out.println(f1.createNewFile());
			
			        //需求2：删除当前模块目录下的java.txt文件
			        System.out.println(f1.delete());
			        System.out.println("--------");
			
			        //需求3：在当前模块目录下创建itcast目录
			        File f2 = new File("myFile\\itcast");
			        System.out.println(f2.mkdir());
			
			        //需求4：删除当前模块目录下的itcast目录
			        System.out.println(f2.delete());
			        System.out.println("--------");
			
			        //需求5：在当前模块下创建一个目录itcast,然后在该目录下创建一个文件java.txt
			        File f3 = new File("myFile\\itcast");
			        System.out.println(f3.mkdir());
			        File f4 = new File("myFile\\itcast\\java.txt");
			        System.out.println(f4.createNewFile());
			
			        //需求6：删除当前模块下的目录itcast
			        System.out.println(f4.delete()); //先删除内容
			        System.out.println(f3.delete()); //后删除目录
			    }
			}

## 递归遍历目录 ##

- 需求：
	- 给定一个路径(E:\\itcast)，请通过递归完成遍历该目录下的所有内容，
	- 并把所有文件的绝对路径输出在控制台
	
- 思路：
	1. 根据给定的路径创建一个File对象
	2. 定义一个方法，用于获取给定目录下的所有内容，参数为第1步创建的File对象
	3. 获取给定的File目录下所有的文件或者目录的File数组
	4. 遍历该File数组，得到每一个File对象
	5. 判断该File对象是否是目录
		- 是：递归调用
		- 不是：获取绝对路径输出在控制台
	6. 调用方法
	    

				public static void main(String[] args) {
			        //根据给定的路径创建一个File对象
			        File srcFile = new File("E:\\itheima"); 
			
			        //调用方法
			        getAllFilePath(srcFile);
			    }
			
			    //定义一个方法，用于获取给定目录下的所有内容，参数为第1步创建的File对象
			    public static void getAllFilePath(File srcFile) {
			        //获取给定的File目录下所有的文件或者目录的File数组
			        File[] fileArray = srcFile.listFiles();
			        //遍历该File数组，得到每一个File对象
			        if(fileArray != null) {
			            for(File file : fileArray) {
			                //判断该File对象是否是目录
			                if(file.isDirectory()) {
			                    //是：递归调用
			                    getAllFilePath(file);
			                } else {
			                    //不是：获取绝对路径输出在控制台
			                    System.out.println(file.getAbsolutePath());
			                }
			            }
			        }
			    }

----------
# 字节缓冲流 #
- **BufferOutputStream**
- **BufferedInputStream**

## 构造方法 ##
	    字节缓冲输出流：BufferedOutputStream​(OutputStream out)
	    字节缓冲输入流：BufferedInputStream​(InputStream in)

## 示例代码 ##
		public class BufferStreamDemo {
		    public static void main(String[] args) throws IOException {
		        //字节缓冲输出流：BufferedOutputStream​(OutputStream out)
		//        FileOutputStream fos = new FileOutputStream("myByteStream\\bos.txt");
		//        BufferedOutputStream bos = new BufferedOutputStream(fos);

        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("myByteStream\\bos.txt"));
        //写数据
        bos.write("hello\r\n".getBytes());
        bos.write("world\r\n".getBytes());
        //释放资源
        bos.close();



        //字节缓冲输入流：BufferedInputStream​(InputStream in)
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream("myByteStream\\bos.txt"));

        //一次读取一个字节数据
        int by;
        while ((by=bis.read())!=-1) {
            System.out.print((char)by);
        }
    
        //一次读取一个字节数组数据
        byte[] bys = new byte[1024];
        int len;
        while ((len=bis.read(bys))!=-1) {
            System.out.print(new String(bys,0,len));
        }

    
        //释放资源
        bis.close();
    }
}

## 复制视频文件 ##
- 需求：

        把E:\\itcast\\字节流复制图片.avi 复制到模块目录下的 字节流复制图片.avi

- 思路：

	        1:根据数据源创建字节输入流对象
	        2:根据目的地创建字节输出流对象
	        3:读写数据，复制图片(一次读取一个字节数组，一次写入一个字节数组)
	        4:释放资源

- 四种方式实现复制视频，并记录每种方式复制视频的时间

	        1:基本字节流一次读写一个字节             共耗时：64565毫秒
	        2:基本字节流一次读写一个字节数组          共耗时：107毫秒
	        3:字节缓冲流一次读写一个字节             共耗时：405毫秒
	        4:字节缓冲流一次读写一个字节数组          共耗时：60毫秒 

- 实现代码：

		public class CopyAviDemo {
		    public static void main(String[] args) throws IOException {
		        //记录开始时间
		        long startTime = System.currentTimeMillis();
		
		        //复制视频
		//        method1();
		//        method2();
		//        method3();
		        method4();
		
		        //记录结束时间
		        long endTime = System.currentTimeMillis();
		        System.out.println("共耗时：" + (endTime - startTime) + "毫秒");
		    }
		
		    //字节缓冲流一次读写一个字节数组
		    public static void method4() throws IOException {
		        BufferedInputStream bis = new BufferedInputStream(new FileInputStream("E:\\itcast\\字节流复制图片.avi"));
		        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("myByteStream\\字节流复制图片.avi"));
		
		        byte[] bys = new byte[1024];
		        int len;
		        while ((len=bis.read(bys))!=-1) {
		            bos.write(bys,0,len);
		        }
		
		        bos.close();
		        bis.close();
		    }
		
		    //字节缓冲流一次读写一个字节
		    public static void method3() throws IOException {
		        BufferedInputStream bis = new BufferedInputStream(new FileInputStream("E:\\itcast\\字节流复制图片.avi"));
		        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("myByteStream\\字节流复制图片.avi"));
		
		        int by;
		        while ((by=bis.read())!=-1) {
		            bos.write(by);
		        }
		
		        bos.close();
		        bis.close();
		    }
		
		
		    //基本字节流一次读写一个字节数组
		    public static void method2() throws IOException {
		        //E:\\itcast\\字节流复制图片.avi
		        //模块目录下的 字节流复制图片.avi
		        FileInputStream fis = new FileInputStream("E:\\itcast\\字节流复制图片.avi");
		        FileOutputStream fos = new FileOutputStream("myByteStream\\字节流复制图片.avi");
		
		        byte[] bys = new byte[1024];
		        int len;
		        while ((len=fis.read(bys))!=-1) {
		            fos.write(bys,0,len);
		        }
		
		        fos.close();
		        fis.close();
		    }
		
		    //基本字节流一次读写一个字节
		    public static void method1() throws IOException {
		        //E:\\itcast\\字节流复制图片.avi
		        //模块目录下的 字节流复制图片.avi
		        FileInputStream fis = new FileInputStream("E:\\itcast\\字节流复制图片.avi");
		        FileOutputStream fos = new FileOutputStream("myByteStream\\字节流复制图片.avi");
		
		        int by;
		        while ((by=fis.read())!=-1) {
		            fos.write(by);
		        }
		
		        fos.close();
		        fis.close();
		    }
		}

----------
# 字符流 #
- ASCII 
- GB2312
- **GBK**

		最常用的中文码表。是在GB2312标准基础上的扩展规范，使用了双字节编码方案，共收录了
		21003个汉字，完全兼容GB2312标准，同时支持繁体汉字以及日韩汉字等

- GB18030
- Unicode： 统一码
	- **UTF-8**

		- UTF-8编码：
		 
				可以用来表示Unicode标准中任意字符，它是电子邮件、网页及其他存储或传送文字的应用中，优先采用的编码。
				互联网工程工作小组（IETF）要求所有互联网协议都必须支持UTF-8编码。
				它使用一至四个字节为每个字符编码。

		- 编码规则：	
			- 128个US-ASCII字符，只需一个字节编码
			- 拉丁文等字符，需要二个字节编码
			- 大部分常用字（含中文），使用三个字节编码
			- 其他极少使用的Unicode辅助字符，使用四字节编码
	- UTF-16
	- UTF-32
- 采用何种规则编码，就要采用对应规则解码，否则就会出现乱码。

## 编码 ##
- byte[] getBytes()：使用平台的默认字符集将该String编码为一系列字节
- byte[] getBytes(String charsetName)：使用**指定的字符集**将该String编码为一系列字节


## 解码 ##
- String(byte[] bytes)：使用平台的默认字符集解码指定的字节数组来创建字符串
- String(byte[] bytes, String charsetName)：通过**指定的字符集**解码指定的字节数组来创建字符串

## 演示代码 ##

		public class StringDemo {
		    public static void main(String[] args) throws UnsupportedEncodingException {
		        //定义一个字符串
		        String s = "中国";
		
		        //byte[] getBytes()：使用平台的默认字符集将该 String编码为一系列字节，将结果存储到新的字节数组中
		        //byte[] bys = s.getBytes(); //[-28, -72, -83, -27, -101, -67]
		        //byte[] getBytes(String charsetName)：使用指定的字符集将该 String编码为一系列字节，将结果存储到新的字节数组中
		//        byte[] bys = s.getBytes("UTF-8"); //[-28, -72, -83, -27, -101, -67]
		        byte[] bys = s.getBytes("GBK"); //[-42, -48, -71, -6]
		        System.out.println(Arrays.toString(bys));
		
		        //String(byte[] bytes)：通过使用平台的默认字符集解码指定的字节数组来构造新的 String
		//        String ss = new String(bys);
		        //String(byte[] bytes, String charsetName)：通过指定的字符集解码指定的字节数组来构造新的 String
		//        String ss = new String(bys,"UTF-8");
		        String ss = new String(bys,"GBK");
		        System.out.println(ss);
		    }
		}


## InputStreamReader：是从字节流到字符流的桥梁 ##
        它读取字节，并使用指定的编码将其解码为字符
        它使用的字符集可以由名称指定，也可以被明确指定，或者可以接受平台的默认字符集

## OutputStreamWriter：是从字符流到字节流的桥梁 ##
        是从字符流到字节流的桥梁，使用指定的编码将写入的字符编码为字节
        它使用的字符集可以由名称指定，也可以被明确指定，或者可以接受平台的默认字符集

## 示例代码 ##
		public class ConversionStreamDemo {
		    public static void main(String[] args) throws IOException {
		//        OutputStreamWriter​(OutputStream out) 创建一个使用默认字符编码的OutputStreamWriter。
		//        OutputStreamWriter​(OutputStream out, String charsetName) 创建一个使用命名字符集的OutputStreamWriter。
		//        FileOutputStream fos = new FileOutputStream("myCharStream\\osw.txt");
		//        OutputStreamWriter osw = new OutputStreamWriter(fos);
		//        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("myCharStream\\osw.txt"));
		//        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("myCharStream\\osw.txt"),"UTF-8");
		        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("myCharStream\\osw.txt"),"GBK");
		        osw.write("中国");
		        osw.close();
		
		//        InputStreamReader​(InputStream in) 创建一个使用默认字符集的InputStreamReader。
		//        InputStreamReader​(InputStream in, String charsetName) 创建一个使用命名字符集的InputStreamReader。
		//        InputStreamReader isr = new InputStreamReader(new FileInputStream("myCharStream\\osw.txt"));
		        InputStreamReader isr = new InputStreamReader(new FileInputStream("myCharStream\\osw.txt"),"GBK");
		        //一次读取一个字符数据
		        int ch;
		        while ((ch=isr.read())!=-1) {
		            System.out.print((char)ch);
		        }
		
		        isr.close();
		
		    }
		}

## OutputStreamWriter---字符流写数据的5种方式 ##

- 构造方法：	 
	- OutputStreamWriter​(OutputStream out)：创建一个使用默认字符编码的OutputStreamWriter
- 写数据的5种方式：
	- void write​(int c)：写一个字符
	- void write​(char[] cbuf)：写入一个字符数组
	- void write​(char[] cbuf, int off, int len)：写入字符数组的一部分
	- void write​(String str)：写一个字符串
	- void write​(String str, int off, int len)：写一个字符串的一部分


			public class OutputStreamWriterDemo {
			    public static void main(String[] args) throws IOException {
			        //OutputStreamWriter​(OutputStream out)：创建一个使用默认字符编码的OutputStreamWriter
			        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("myCharStream\\osw.txt"));
			
			        //void write​(int c)：写一个字符
			//        osw.write(97);
			//        //void flush()：刷新流
			//        osw.flush();
			//        osw.write(98);
			//        osw.flush();
			//        osw.write(99);
			
			        //void write​(char[] cbuf)：写入一个字符数组
			        char[] chs = {'a', 'b', 'c', 'd', 'e'};
			//        osw.write(chs);
			
			        //void write​(char[] cbuf, int off, int len)：写入字符数组的一部分
			//        osw.write(chs, 0, chs.length);
			//        osw.write(chs, 1, 3);
			
			        //void write​(String str)：写一个字符串
			//        osw.write("abcde");
			
			        //void write​(String str, int off, int len)：写一个字符串的一部分
			//        osw.write("abcde", 0, "abcde".length());
			        osw.write("abcde", 1, 3);
			
			        //释放资源
			        osw.close();
			        //Exception in thread "main" java.io.IOException: Stream closed
			//        osw.write(100);
			    }
			}


## InputStreamReader---字符流读数据的2种方式 ##
- 构造方法：
	- InputStreamReader​(InputStream in)：创建一个使用默认字符集的InputStreamReader

- 读数据的2种方式：
       - int read​()：一次读一个字符数据
       - int read​(char[] cbuf)：一次读一个字符数组数据


			public class InputStreamReaderDemo {
			    public static void main(String[] args) throws IOException {
			        //InputStreamReader​(InputStream in)：创建一个使用默认字符集的InputStreamReader
			//        InputStreamReader isr = new InputStreamReader(new FileInputStream("myCharStream\\osw.txt"));
			        InputStreamReader isr = new InputStreamReader(new FileInputStream("myCharStream\\ConversionStreamDemo.java"));
			
			        //int read​()：一次读一个字符数据
			//        int ch;
			//        while ((ch=isr.read())!=-1) {
			//            System.out.print((char)ch);
			//        }
			
			        //int read​(char[] cbuf)：一次读一个字符数组数据
			        char[] chs = new char[1024];
			        int len;
			        while ((len = isr.read(chs)) != -1) {
			            System.out.print(new String(chs, 0, len)); //将字符数组转换为字符串
			        }
			
			
			        //释放资源
			        isr.close();
			    }
			}

----------
# 字符流与字符缓冲流 #
**需求：把模块目录下的ConversionStreamDemo.java 复制到模块目录下的 Copy.java**

    数据源和目的地的分析
        数据源：myCharStream\\ConversionStreamDemo.java --- 读数据 --- Reader --- InputStreamReader --- FileReader
        目的地： myCharStream\\ Copy.java --- 写数据 --- Writer --- OutputStreamWriter --- FileWriter

    思路：
        1:根据数据源创建字符输入流对象
        2:根据目的地创建字符输出流对象
        3:读写数据，复制文件
        4:释放资源

## 字符流：FileReader&&FileWriter ##
**实现代码**

	public class CopyJavaDemo02 {
	    public static void main(String[] args) throws IOException {
	        //根据数据源创建字符输入流对象
	        FileReader fr = new FileReader("myCharStream\\ConversionStreamDemo.java");
	        //根据目的地创建字符输出流对象
	        FileWriter fw = new FileWriter("myCharStream\\Copy.java");
	
	        //读写数据，复制文件
	//        int ch;
	//        while ((ch=fr.read())!=-1) {
	//            fw.write(ch);
	//        }
	
	        char[] chs = new char[1024];
	        int len;
	        while ((len=fr.read(chs))!=-1) {
	            fw.write(chs,0,len);
	        }
	
	        //释放资源
	        fw.close();
	        fr.close();
	    }
	}

## 字符缓冲流：BufferedWriter&&BufferedReader ##
**实现代码**

	public class CopyJavaDemo01 {
	    public static void main(String[] args) throws IOException {
	        //根据数据源创建字符缓冲输入流对象
	        BufferedReader br = new BufferedReader(new FileReader("myCharStream\\ConversionStreamDemo.java"));
	        //根据目的地创建字符缓冲输出流对象
	        BufferedWriter bw = new BufferedWriter(new FileWriter("myCharStream\\Copy.java"));
	
	        //读写数据，复制文件
	        //一次读写一个字符数据
	//        int ch;
	//        while ((ch=br.read())!=-1) {
	//            bw.write(ch);
	//        }
	
	        //一次读写一个字符数组数据
	        char[] chs = new char[1024];
	        int len;
	        while ((len=br.read(chs))!=-1) {
	            bw.write(chs,0,len);
	        }
	
	        //释放资源
	        bw.close();
	        br.close();
	    }
	}

----------
# 字符缓冲流 #

- BufferedWriter：将文本写入字符输出流，缓冲字符，以提供单个字符，数组和字符串的高效写入，可以指定缓冲区大小，或者可以接受默认大小。默认值足够大，可用于大多数用途
- BufferedReader：从字符输入流读取文本，缓冲字符，以提供字符，数组和行的高效读取，可以指定缓冲区大小，或者可以使用默认大小。 默认值足够大，可用于大多数用途

- 构造方法：
	- BufferedWriter(Writer out)
	- BufferedReader(Reader in)


			public class BufferedStreamDemo01 {
			    public static void main(String[] args) throws IOException {
			        //BufferedWriter(Writer out)
			//        FileWriter fw = new FileWriter("myCharStream\\bw.txt");
			//        BufferedWriter bw = new BufferedWriter(fw);
			//        BufferedWriter bw = new BufferedWriter(new FileWriter("myCharStream\\bw.txt"));
			//        bw.write("hello\r\n");
			//        bw.write("world\r\n");
			//        bw.close();
			
			        //BufferedReader(Reader in)
			        BufferedReader br = new BufferedReader(new FileReader("myCharStream\\bw.txt"));
			
			        //一次读取一个字符数据
			//        int ch;
			//        while ((ch=br.read())!=-1) {
			//            System.out.print((char)ch);
			//        }
			
			        //一次读取一个字符数组数据
			        char[] chs = new char[1024];
			        int len;
			        while ((len=br.read(chs))!=-1) {
			            System.out.print(new String(chs,0,len));
			        }
			
			        br.close();
			
			    }
			}


----------














