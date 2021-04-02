---
title: Java-Note22
categories:
  - 后端
  - Java
tags:
  - 网络编程
  - IP
  - UDP
  - TCP
author:
  - Jungle
date: 2021-02-06 12:25:30

---
# 网络编程 #

## 基本概念 ##
- 计算机网络：指将地理位置不同的具有独立功能的多台计算机及其外部设备，通过通信线路连接起来，在网络操作系统、网络管理软件及网络通信协议的管理和协调下，实现资源共享和信息传递的计算机系统。
- 网络编程：在网络通信协议下，实现网络互联的不同计算机上运行的程序间可以进行数据交换。

## 网络编程三要素 ##
1. IP地址：为了让网络中的计算机能够互相通信而为每台计算机指定的一个标识号，通过这个标志号来指定要接收数据的计算机和识别发送的计算机，这个标志号就是IP地址。
2. 端口：网络的通信，本质上是两个应用程序的通信，端口号就是可以**唯一标识设备中的应用程序**的。
	- 用两个字节表示的整数，它的取值范围是0~65535。
	- 其中，0~1023之间的端口号用于一些知名的网络服务和应用，普通的应用程序需要使用1024以上的端口号。
	- 如果端口号被另外一个服务或应用所占用，会导致当前程序启动失败。
3. 协议：通过计算机网络可以使多台计算机实现连接，位于同一个网络中的计算机在进行连接和通信时需要遵守一定的规则，这些**连接和通信的规则被称为网络通信协议**，它对数据的传输格式、传输速率、传输步骤等做了统一规定，通信双方必须同时遵守才能完成数据交换。常见的有UDP协议和TCP协议。
	1. UDP协议
		- 用户数据报协议(User Datagram Protocol)
		- UDP是**无连接**通信协议，即在数据传输时，数据的发送端和接收端不建立逻辑连接。
		- 简单来说，当一台计算机向另外一台计算机发送数据时，发送端不会确认接收端是否存在，就会发出数据，同样接收端在收到数据时，也不会向发送端反馈是否收到数据。
		- 由于使用UDP协议消耗资源小，通信效率高，所以通常都会用于音频、视频和普通数据的传输
		- 例如：视频会议通常采用UDP协议，因为这种情况即使偶尔丢失一两个数据包，也不会对接收结果产生太大影响。
		- 但是在使用UDP协议传送数据时，由于UDP的面向无连接性，不能保证数据的完整性，因此在传输重要数据时不建议使用UDP协议
	2. TCP协议
		- 传输控制协议 (Transmission Control Protocol)
		- TCP协议是**面向连接**的通信协议，即传输数据之前，在发送端和接收端建立逻辑连接，然后再传输数据，它提供了两台计算机之间**可靠无差错**的数据传输。
		- 在TCP连接中必须要明确客户端与服务器端，由客户端向服务端发出连接请求，每次连接的创建都需要经过“三次握手”
		- **三次握手**：TCP协议中，在发送数据的准备阶段，客户端与服务器之间的三次交互，以保证连接的可靠性：
			1. 第一次握手，客户端向服务器端发出连接请求，等待服务器确认
			2. 第二次握手，服务器端向客户端回送一个响应，通知客户端收到了连接请求
			3. 第三次握手，客户端再次向服务器端发送确认信息，确认连接
		- 完成三次握手，连接建立后，客户端和服务器就可以开始进行数据传输了。
		- 由于这种面向连接的特性，TCP协议可以保证传输数据的安全，所以应用十分广泛。
		- 例如上传文件、下载文件、浏览网页等

## IP地址 ##
- IPV4：是给每个连接在网络上的主机分配一个**32位地址**。按照TCP/IP规定，IP地址用二进制来表示，每个IP地址长32bit，也就是**4个字节**。为了方便使用，IP地址经常被写成十进制的形式，中间使用符号"."分隔不同的字节。例如："11000000 10101000 00000001 01000010" --> "192.168.1.66"，这种表示法加做 “点分十进制表示法”。
- IPV6：由于互联网的蓬勃发展，IP地址的需求量愈来愈大，但是网络地址资源有限，使得IP的分配越发紧张。为了扩大地址空间，通过IPV6重新定义地址空间，采用**128位地址长度**，也就是**16个字节**，每**16位为一组**，分成8组十六进制数，这样就解决了网络地址资源数量不够的问题。
- 常用命令：
	- ipconfig：查看本机IP地址
	- ping IP地址：检查网络是否连通
- 特殊IP地址：
	- 127.0.0.1：是回送地址，可以代表本机地址，一般用来测试。

## InetAddress ##
	为了方便我们对IP地址的获取和操作，Java提供了一个类InetAddress供我们使用。
**此类表示Internet协议（IP）地址**。

- public **static** InetAddress getByName​(String host)：确定主机名称的IP地址。主机名称可以是机器名称，也可以是IP地址
- public String getHostName​()：获取此IP地址的主机名
- public String getHostAddress​()：返回文本显示中的IP地址字符串

		InetAddress address = InetAddress.getByName("192.168.31.170");
  	  String name = address.getHostName();
  	  String ip = address.getHostAddress();

## UDP通信程序 ##
	UDP协议是一种不可靠的网络协议，它在通信的两端各建立一个Socket对象，但是这两个Socket只是发
	送，接收数据的对象，因此对于基于UDP协议的通信双方而言，没有所谓的客户端和服务器的概念
**Java提供了DatagramSocket类作为基于UDP协议的Socket**	

- 构造方法:
	- DatagramSocket()：创建数据报套接字并将其绑定到本机地址上的任何可用端口
	- DatagramPacket(byte[] buf,int len,InetAddressadd,int port)：创建数据包,发送长度为len的数据包到指定主机的指定端口
- 相关方法：
	- void send(DatagramPacket p)：发送数据报包
	- void close()：关闭数据报套接字
	- void receive(DatagramPacket p)：从此套接字接受数据报包
- UDP发送数据的步骤
	1. 创建发送端的Socket对象(DatagramSocket)
    2. 创建数据，并把数据打包
    3. 调用DatagramSocket对象的方法发送数据
    4. 关闭发送端

			//创建发送端的Socket对象:构造数据报套接字并将其绑定到本地主机上的任何可用端口
			        DatagramSocket ds = new DatagramSocket();
			
			        //构造一个数据包，发送长度为 length的数据包到指定主机上的指定端口号。
			        byte[] bys = "hello, udp".getBytes();
			//        int length = bys.length;
			//        InetAddress address = InetAddress.getByName("192.168.31.170");
			//        int port = 10086;
			        //创建数据,并把数据打包
			        //DatagramPacket dp = new DatagramPacket(bys, length, address, port);
			        DatagramPacket dp = new DatagramPacket(bys, bys.length, InetAddress.getByName("192.168.31.170"), 10000);
			
			        //发送数据
			        ds.send(dp);
			
			        //关闭发送端
			        ds.close();


- UDP接收数据的步骤
	1. 创建接收端的Socket对象(DatagramSocket)
    2. 创建一个数据包，用于接收数据
    3. 调用DatagramSocket对象的方法接收数据
    4. 解析数据包，并把数据在控制台显示
    5. 关闭接收端

			//创建接收端的Socket对象(DatagramSocket)
			        //DatagramSocket​(int port) 构造数据报套接字并将其绑定到本地主机上的指定端口
			        DatagramSocket ds = new DatagramSocket(10000);
			
			        //创建一个数据包，用于接收数据
			        //DatagramPacket​(byte[] buf, int length) 构造一个 DatagramPacket用于接收长度为 length数据包
			        byte[] bys = new byte[1024];
			        DatagramPacket dp = new DatagramPacket(bys, bys.length);
			
			        //调用DatagramSocket对象的方法接收数据
			        ds.receive(dp);
			
			        //解析数据包，并把数据在控制台显示
			        //byte[] getData() 返回数据缓冲区
			        byte[] datas = dp.getData();
			        //int getLength​() 返回要发送的数据的长度或接收到的数据的长度
			        int len = dp.getLength();
			        String dataString = new String(datas,0,len);
			        System.out.println("数据是：" + dataString);
			
			        //关闭接收端
			        ds.close();


## UDP通信小程序练习 ##

- 接收端：

		package UDPTest;/*
		  @author: Jungle
		  @DESCRIPTION: What to do...
		  @creteTime: 2021/2/7  
		*/
		
		/*
		    UDP接收数据：
		        因为接收端不知道发送端什么时候停止发送，故采用死循环接收
		 */
		
		import java.io.IOException;
		import java.net.DatagramPacket;
		import java.net.DatagramSocket;
		import java.net.SocketException;
		
		public class ReceiveDemo {
		    public static void main(String[] args) throws IOException {
		        //创建接收端的Socket对象(DatagramSocket)
		        DatagramSocket ds = new DatagramSocket(12345);
		
		        while (true) {
		            //创建一个数据包，用于接收数据
		            byte[] bys = new byte[1024];
		            DatagramPacket dp = new DatagramPacket(bys, bys.length);
		
		            //调用DatagramSocket对象的方法接收数据
		            ds.receive(dp);
		
		            //解析数据包，并把数据在控制台显示
		            System.out.println("数据是：" + new String(dp.getData(), 0, dp.getLength()));
		        }
		
		        //关闭接收端---一直截数据就不用关了
		//        ds.close();
		    }
		}


- 发送端：

		package UDPTest;/*
		  @author: Jungle
		  @DESCRIPTION: What to do...
		  @creteTime: 2021/2/7  
		*/
		
		/*
		    UDP发送数据：
		        数据来自于键盘录入，直到输入的数据是886，发送数据结束
		 */
		
		import java.io.BufferedReader;
		import java.io.IOException;
		import java.io.InputStreamReader;
		import java.net.DatagramPacket;
		import java.net.DatagramSocket;
		import java.net.InetAddress;
		
		public class SendDemo {
		    public static void main(String[] args) throws IOException {
		        //创建发送端的Socket对象(DatagramSocket)
		        DatagramSocket ds = new DatagramSocket();
		
		        //自己封装键盘录入数据
		        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		        String line;
		        while ((line = br.readLine()) != null) {
		            //输入的数据是886，发送数据结束
		            if ("886".equals(line)) {
		                break;
		            }
		
		            //创建数据，并把数据打包
		            byte[] bys = line.getBytes();
		            DatagramPacket dp = new DatagramPacket(bys, bys.length, InetAddress.getByName("192.168.31.170"), 12345);
		
		            //调用DatagramSocket对象的方法发送数据
		            ds.send(dp);
		        }
		
		        //关闭发送端
		        ds.close();
		    }
		}

## TCP通信程序 ##
	TCP通信协议是一种可靠的网络协议，它在通信的两端各建立一个Socket对象，从而在通信的两端形成网络虚拟链路，
	一旦建立了虚拟的网络链路，两端的程序就可以通过虚拟链路进行通信；
	Java对基于TCP协议的的网络提供了良好的封装，使用Socket对象来代表两端的通信端口，并通过Socket产生IO流来进行网络通信；
	Java为客户端提供了Socket类，为服务器端提供了ServerSocket类。

- TCP发送数据的步骤 
	1. 创建客户端的Socket对象(Socket) --> Socket​(String host, int port)
    2. 获取输出流，写数据 --> OutputStream getOutputStream​()
    3. 释放资源 --> void close​()

- 构造方法：
	- Socket(InetAddress address,int port)： 创建流套接字并将其连接到指定IP指定端口号
	- Socket(String host, int port)： 创建流套接字并将其连接到指定主机上的指定端口号

- 相关方法：
	- OutputStream getOutputStream()： 返回此套接字的输出流
	

			        //创建客户端的Socket对象(Socket)
			        //Socket​(InetAddress address, int port) 创建流套接字并将其连接到指定IP地址的指定端口号
			//        Socket s = new Socket(InetAddress.getByName("192.168.31.170"),10000);
			        //Socket​(String host, int port) 创建流套接字并将其连接到指定主机上的指定端口号
			        Socket s = new Socket("192.168.31.170",10000);
			
			        //获取输出流，写数据
			        //OutputStream getOutputStream​() 返回此套接字的输出流
			        OutputStream os = s.getOutputStream();
			        os.write("hello,tcp,我来了".getBytes());
			
			        //释放资源
			        s.close();


- TCP接收数据的步骤
	1. 创建服务器端的Socket对象(ServerSocket) --> ServerSocket​(int port)
	2. 监听客户端连接，返回一个Socket对象 --> Socket accept​()
	3. 获取输入流，读数据，并把数据显示在控制台 --> InputStream getInputStream​() 
	4. 释放资源 --> void close​()

- 构造方法：
	
- ServletSocket(int port) 创建绑定到指定端口的服务器套接字
	
- 相关方法：
	- Socket accept() 监听要连接到此的套接字并接受它
	- InputStream getInputStream()： 返回此套接字的输入流

			//创建服务器端的Socket对象(ServerSocket)
	  	  //ServerSocket​(int port) 创建绑定到指定端口的服务器套接字
	  	  ServerSocket ss = new ServerSocket(10000);
			
	  	  //Socket accept​() 侦听要连接到此套接字并接受它
	  	  Socket s = ss.accept();
			
	  	  //获取输入流，读数据，并把数据显示在控制台
	  	  InputStream is = s.getInputStream();
	  	  byte[] bys = new byte[1024];
	  	  int len = is.read(bys); //数据不多，读一次就好了
	  	  String data = new String(bys,0,len);
	  	  System.out.println("数据是：" + data);
			
	  	  //释放资源
	  	  s.close();
	  	  ss.close();

## TCP小程序练习 ##

- 服务器端：

		package TCPTest;/*
		  @author: Jungle
		  @DESCRIPTION: What to do...
		  @creteTime: 2021/2/7  
		*/
		
		import java.io.*;
		import java.net.ServerSocket;
		import java.net.Socket;
		
		public class ServerDemo {
		    public static void main(String[] args) throws IOException {
		        //创建服务器端的Socket对象
		        ServerSocket ss = new ServerSocket(10000);
		
		        //监听客户端连接，返回一个Socket对象
		        Socket s = ss.accept();
		
		        //获取输入流，读数据，并把数据显示在控制台
		//        InputStream is = s.getInputStream();
		//        byte[] bys = new byte[1024];
		//        int len = is.read(bys);
		//        String data = new String(bys, 0, len);
		//        System.out.println("服务器：" + data);
		        //接收数据
		        BufferedReader br = new BufferedReader(new InputStreamReader(s.getInputStream()));
		        //把数据写入文本文件
		        //BufferedWriter bw = new BufferedWriter(new FileWriter("s.txt"));
		        BufferedWriter bw = new BufferedWriter(new FileWriter("copy.java"));
		
		        String line;
		        while ((line=br.readLine()) != null) { //等待读取数据
		//            if ("886".equals(line)) {
		//                break;
		//            }
		
		            bw.write(line);
		            bw.newLine();
		            bw.flush();
		            System.out.println("服务器：" + line);
		        }
		
		        //给出反馈
		//        OutputStream os = s.getOutputStream();
		//        os.write("数据已经收到".getBytes());
		        //替换上面代码
		        BufferedWriter bwServer = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
		        bwServer.write("文件上传成功");
		        bwServer.newLine();
		        bwServer.flush();
		
		        //释放资源
		        bw.close();
		        ss.close();
		    }
		}


- 客户端：

		package TCPTest;/*
		  @author: Jungle
		  @DESCRIPTION: What to do...
		  @creteTime: 2021/2/7  
		*/
		
		import java.io.*;
		import java.net.Socket;
		
		public class ClientDemo {
		    public static void main(String[] args) throws IOException {
		        //创建客户端的Socket对象
		        Socket s = new Socket("192.168.31.170", 10000);
		
		//        //数据来自于键盘录入，直到输入的数据是886，发送数据结束
		//        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		        //封装文本文件的数据
		        BufferedReader br = new BufferedReader(new FileReader("D:\\Codes\\JavaCode\\Example\\src\\TestDate.java"));
		
		        //封装输出流对象
		        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
		
		        String line;
		        while ((line = br.readLine()) != null){
		//            if ("886".equals(line)){
		//                break;
		//            }
		            //获取输出流，写数据 --> 被封装输出流对象替代了
		//            OutputStream os = s.getOutputStream();
		//            os.write(line.getBytes());
		            bw.write(line);
		            bw.newLine(); //读一行，加上换行符
		            bw.flush();
		        }
		
		        //自定义结束标记
		//        bw.write("886");
		//        bw.newLine();
		//        bw.flush();
		        //shutdownOutput代替自定义结束标记
		        s.shutdownOutput();
		
		        //接收服务器反馈
		//        InputStream is = s.getInputStream();
		//        byte[] bys = new byte[1024];
		//        int len = is.read(bys);
		//        String data = new String(bys, 0, len);
		//        System.out.println("客户端：" + data);
		        //替换上面代码
		        BufferedReader brClient = new BufferedReader(new InputStreamReader(s.getInputStream()));
		         String data = brClient.readLine();
		        System.out.println("服务器的反馈：" + data);
		
		        //释放资源
		        br.close();
		        s.close();
		    }
		}


## 多线程实现文件上传 ##

- 客户端：数据来自于文本文件，接收服务器反馈

		package TCPTest;/*
		  @author: Jungle
		  @DESCRIPTION: What to do...
		  @creteTime: 2021/2/7  
		*/
		
		import java.io.*;
		import java.net.Socket;
		
		// 客户端：数据来自于文本文件，接收服务器反馈
		public class ClientDemo {
		    public static void main(String[] args) throws IOException {
		        //创建客户端的Socket对象
		        Socket s = new Socket("192.168.31.170", 10000);
		
		        //封装文本文件的数据
		        BufferedReader br = new BufferedReader(new FileReader("D:\\Codes\\JavaCode\\Example\\src\\TestDate.java"));
		
		        //封装输出流对象
		        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
		
		        String line;
		        while ((line = br.readLine()) != null){
		            bw.write(line);
		            bw.newLine();
		            bw.flush();
		        }
		
		        s.shutdownOutput();
		
		        //接收服务器反馈
		        BufferedReader brClient = new BufferedReader(new InputStreamReader(s.getInputStream()));
		        String data = brClient.readLine(); //等待读取数据
		        System.out.println("服务器的反馈：" + data);
		
		        //释放资源
		        br.close();
		        s.close();
		    }
		}


- 服务器：接收到的数据写入文本文件，给出反馈，代码用线程进行封装，为每一个客户端开启一个线程

		package TCPTest;/*
		  @author: Jungle
		  @DESCRIPTION: What to do...
		  @creteTime: 2021/2/7  
		*/
		
		import java.io.*;
		import java.net.ServerSocket;
		import java.net.Socket;
		
		//服务器：接收到的数据写入文本文件，给出反馈，代码用线程进行封装，为每一个客户端开启一个线程
		public class ServerDemo {
		    public static void main(String[] args) throws IOException {
		        //创建服务器端的Socket对象
		        ServerSocket ss = new ServerSocket(10000);
		
		        while (true) {
		            //监听客户端连接，返回一个对应的Socket对象
		            Socket s = ss.accept();
		            //为每一个客户端开启一个线程
		            new Thread(new ServerThread(s)).start();
		        }
		
		//        ss.close(); //服务器一般不关闭
		    }
		}

- 线程封装：


		package TCPTest;/*
		  @author: Jungle
		  @DESCRIPTION: What to do...
		  @creteTime: 2021/2/7  
		*/
		
		import java.io.*;
		import java.net.Socket;
		
		public class ServerThread implements Runnable{
		    private Socket s;
		
		    public ServerThread(Socket s) {
		        this.s = s;
		    }
		
		    @Override
		    public void run() {
		        try {
		            //接收数据写到文本文件
		            BufferedReader br = new BufferedReader(new InputStreamReader(s.getInputStream()));
		//            BufferedWriter bw = new BufferedWriter(new FileWriter("Copy.java"));
		            //解决名称冲突问题
		            int count = 0;
		            File file = new File("Copy["+count+"].java");
		            while (file.exists()) {
		                count++;
		                file = new File("Copy["+count+"].java");
		            }
		            BufferedWriter bw = new BufferedWriter(new FileWriter(file));
		
		            String line;
		            while ((line=br.readLine())!=null) {
		                bw.write(line);
		                bw.newLine();
		                bw.flush();
		            }
		
		            //给出反馈
		            BufferedWriter bwServer = new BufferedWriter(new OutputStreamWriter(s.getOutputStream()));
		            bwServer.write("文件上传成功");
		            bwServer.newLine();
		            bwServer.flush();
		
		            //释放资源
		            s.close();
		        } catch (IOException e) {
		            e.printStackTrace();
		        }
		    }
		}


----------
