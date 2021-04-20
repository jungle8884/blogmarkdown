---
title: Java_NowCoder
categories:
  - 后端
  - NowCoder
tags:
  - Java
  - NowCoder
author:
  - Jungle
date: 2021-04-13
---

# 牛客 Java 高级工程师

## 搭建开发环境

### 搭建开发环境--Maven

<img src="Java-NowCoder-Note/image-20210413155327508.png" alt="image-20210413155327508" style="zoom:67%;" />

- [下载链接](http://maven.apache.org/)

- [阿里云镜像仓库](https://maven.aliyun.com/mvn/view)

  - 复制镜像链接：[central](https://maven.aliyun.com/repository/central)
  - 配置镜像：安装目录\apache-maven-3.8.1\conf\settings.xml

  ```
  <mirror>
        <id>maven-aliyun</id>
        <mirrorOf>central</mirrorOf>
        <name>Maven aliyun</name>
        <url>https://maven.aliyun.com/repository/central</url>
  </mirror>
  ```

  - 配置环境变量：Path中加上`D:\apache-maven-3.8.1\bin`

  ```
  C:\Users\LeBro>mvn --version
  Apache Maven 3.8.1 (05c21c65bdfed0f71a2f2ada8b84da59348c4c5d)
  Maven home: D:\apache-maven-3.8.1-bin\apache-maven-3.8.1\bin\..
  Java version: 11.0.10, vendor: AdoptOpenJDK, runtime: C:\Program Files\AdoptOpenJDK\jdk-11.0.10.9-hotspot
  Default locale: zh_CN, platform encoding: GBK
  OS name: "windows 10", version: "10.0", arch: "amd64", family: "windows"
  ```

#### 使用CMD命令创建项目

- maven命令：[快速上手](http://maven.apache.org/guides/getting-started/maven-in-five-minutes.html)

> 先切换到你要创建项目的目录

- 创建项目

```
D:\Codes>cd Work
# archetype:generate 		以模板原型方式生成文件
# DgroupId					组织ID: 通常写公司域名倒序.项目名
# DartifactId				项目ID
# DarchetypeArtifactId		项目模板ID
# DinteractiveMode=false	是否启用交互模式
D:\Codes\Work>mvn archetype:generate -DgroupId=com.jungle8884.mavendemo1 -DartifactId=mavendemo1 -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false
# 创建成功
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------< org.apache.maven:standalone-pom >-------------------
[INFO] Building Maven Stub Project (No POM) 1
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] >>> maven-archetype-plugin:3.2.0:generate (default-cli) > generate-sources @ standalone-pom >>>
[INFO]
[INFO] <<< maven-archetype-plugin:3.2.0:generate (default-cli) < generate-sources @ standalone-pom <<<
[INFO]
[INFO]
[INFO] --- maven-archetype-plugin:3.2.0:generate (default-cli) @ standalone-pom ---
[INFO] Generating project in Batch mode
[WARNING] No archetype found in remote catalog. Defaulting to internal catalog
[INFO] ----------------------------------------------------------------------------
[INFO] Using following parameters for creating project from Archetype: maven-archetype-quickstart:1.4
[INFO] ----------------------------------------------------------------------------
[INFO] Parameter: groupId, Value: com.jungle8884.mavendemo1
[INFO] Parameter: artifactId, Value: mavendemo1
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] Parameter: package, Value: com.jungle8884.mavendemo1
[INFO] Parameter: packageInPathFormat, Value: com/jungle8884/mavendemo1
[INFO] Parameter: package, Value: com.jungle8884.mavendemo1
[INFO] Parameter: groupId, Value: com.jungle8884.mavendemo1
[INFO] Parameter: artifactId, Value: mavendemo1
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] Project created from Archetype in dir: D:\Codes\Work\mavendemo1
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  2.746 s
[INFO] Finished at: 2021-04-13T16:46:21+08:00
[INFO] ------------------------------------------------------------------------
```

- 查看：D:\Codes\Work\mavendemo1\src

  - main: 开发代码
  - test: 测试代码

- 查看仓库：C:\Users\LeBro\.m2\repository

  - 远程仓库下载的资源都放在这里

- 在该目录下编译：D:\Codes\Work\mavendemo1

  - 会生成taget文件夹
  - 如下图：

  <img src="Java-NowCoder-Note/image-20210413172106645.png" alt="image-20210413172106645" style="zoom: 80%;" />

  ```
  D:\Codes\Work\mavendemo1>mvn compile
  [INFO] Scanning for projects...
  [INFO]
  [INFO] ----------------< com.jungle8884.mavendemo1:mavendemo1 >----------------
  [INFO] Building mavendemo1 1.0-SNAPSHOT
  [INFO] --------------------------------[ jar ]---------------------------------
  [INFO] Changes detected - recompiling the module!
  [INFO] Compiling 1 source file to D:\Codes\Work\mavendemo1\target\classes
  [INFO] ------------------------------------------------------------------------
  [INFO] BUILD SUCCESS
  [INFO] ------------------------------------------------------------------------
  [INFO] Total time:  27.798 s
  [INFO] Finished at: 2021-04-13T16:54:41+08:00
  [INFO] ------------------------------------------------------------------------
  ```

- 在该目录下清除：D:\Codes\Work\mavendemo1

  ```
  D:\Codes\Work\mavendemo1>mvn clean
  [INFO] Scanning for projects...
  [INFO]
  [INFO] ----------------< com.jungle8884.mavendemo1:mavendemo1 >----------------
  [INFO] Building mavendemo1 1.0-SNAPSHOT
  [INFO] --------------------------------[ jar ]---------------------------------
  [INFO]
  [INFO] --- maven-clean-plugin:3.1.0:clean (default-clean) @ mavendemo1 ---
  [INFO] Deleting D:\Codes\Work\mavendemo1\target
  [INFO] ------------------------------------------------------------------------
  [INFO] BUILD SUCCESS
  [INFO] ------------------------------------------------------------------------
  [INFO] Total time:  2.505 s
  [INFO] Finished at: 2021-04-13T17:15:18+08:00
  [INFO] ------------------------------------------------------------------------
  ```

- 也可以组合使用：编译前先清除

  ```
  D:\Codes\Work\mavendemo1>mvn clean compile
  [INFO] Scanning for projects...
  [INFO]
  [INFO] ----------------< com.jungle8884.mavendemo1:mavendemo1 >----------------
  [INFO] Building mavendemo1 1.0-SNAPSHOT
  [INFO] --------------------------------[ jar ]---------------------------------
  [INFO]
  [INFO] --- maven-clean-plugin:3.1.0:clean (default-clean) @ mavendemo1 ---
  [INFO]
  [INFO] --- maven-resources-plugin:3.0.2:resources (default-resources) @ mavendemo1 ---
  [INFO] Using 'UTF-8' encoding to copy filtered resources.
  [INFO] skip non existing resourceDirectory D:\Codes\Work\mavendemo1\src\main\resources
  [INFO]
  [INFO] --- maven-compiler-plugin:3.8.0:compile (default-compile) @ mavendemo1 ---
  [INFO] Changes detected - recompiling the module!
  [INFO] Compiling 1 source file to D:\Codes\Work\mavendemo1\target\classes
  [INFO] ------------------------------------------------------------------------
  [INFO] BUILD SUCCESS
  [INFO] ------------------------------------------------------------------------
  [INFO] Total time:  1.422 s
  [INFO] Finished at: 2021-04-13T17:19:36+08:00
  [INFO] ------------------------------------------------------------------------
  ```

- 测试使用

  ```
  D:\Codes\Work\mavendemo1>mvn clean test
  [INFO] Scanning for projects...
  [INFO]
  [INFO] ----------------< com.jungle8884.mavendemo1:mavendemo1 >----------------
  [INFO] Building mavendemo1 1.0-SNAPSHOT
  [INFO] --------------------------------[ jar ]---------------------------------
  [INFO]
  [INFO] --- maven-clean-plugin:3.1.0:clean (default-clean) @ mavendemo1 ---
  [INFO] Deleting D:\Codes\Work\mavendemo1\target
  [INFO]
  [INFO] --- maven-resources-plugin:3.0.2:resources (default-resources) @ mavendemo1 ---
  [INFO] Using 'UTF-8' encoding to copy filtered resources.
  [INFO] skip non existing resourceDirectory D:\Codes\Work\mavendemo1\src\main\resources
  [INFO]
  [INFO] --- maven-compiler-plugin:3.8.0:compile (default-compile) @ mavendemo1 ---
  [INFO] Changes detected - recompiling the module!
  [INFO] Compiling 1 source file to D:\Codes\Work\mavendemo1\target\classes
  [INFO]
  [INFO] --- maven-resources-plugin:3.0.2:testResources (default-testResources) @ mavendemo1 ---
  [INFO] Using 'UTF-8' encoding to copy filtered resources.
  [INFO] skip non existing resourceDirectory D:\Codes\Work\mavendemo1\src\test\resources
  [INFO]
  [INFO] --- maven-compiler-plugin:3.8.0:testCompile (default-testCompile) @ mavendemo1 ---
  [INFO] Changes detected - recompiling the module!
  [INFO] Compiling 1 source file to D:\Codes\Work\mavendemo1\target\test-classes
  [INFO]
  [INFO] --- maven-surefire-plugin:2.22.1:test (default-test) @ mavendemo1 ---
  [INFO]
  [INFO] -------------------------------------------------------
  [INFO]  T E S T S
  [INFO] -------------------------------------------------------
  [INFO] Running com.jungle8884.mavendemo1.AppTest
  [INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.062 s - in com.jungle8884.mavendemo1.AppTest
  [INFO]
  [INFO] Results:
  [INFO]
  [INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
  [INFO]
  [INFO] ------------------------------------------------------------------------
  [INFO] BUILD SUCCESS
  [INFO] ------------------------------------------------------------------------
  [INFO] Total time:  15.081 s
  [INFO] Finished at: 2021-04-13T17:22:53+08:00
  [INFO] ------------------------------------------------------------------------
  ```

  - 生成对应目录：D:\Codes\Work\mavendemo1\target\test-classes\com\jungle8884\mavendemo1
  - 生成对应文件：AppTest.class
  - 如下图：

![image-20210413172343661](Java-NowCoder-Note/image-20210413172343661.png)

#### 使用IntelliJ IDEA创建项目

> 在`IntelliJ IDEA`里配置`Maven`，主要是以下两个安装目录和配置文件

- `D:\apache-maven-3.8.1`

- `D:\apache-maven-3.8.1\conf\settings.xml`

  ![image-20210413204056208](Java-NowCoder-Note/image-20210413204056208.png)

- 新建Maven项目：

  ![image-20210413204355924](Java-NowCoder-Note/image-20210413204355924.png)

  ![image-20210413204657056](Java-NowCoder-Note/image-20210413204657056.png)

  ![image-20210413204744364](Java-NowCoder-Note/image-20210413204744364.png)

  ![image-20210413204921218](Java-NowCoder-Note/image-20210413204921218.png)



- 发现与CMD命令创建一致！

  ![image-20210413205427608](Java-NowCoder-Note/image-20210413205427608.png)



- 发现运行时出错：不能运行App.java文件

  - 不断尝试后，发现换了个最新的IntelliJ IDEA 2021版本
  - 成功运行！

  ![image-20210413215305063](Java-NowCoder-Note/image-20210413215305063.png)

------

### 搭建开发环境--Spring

#### 搭建开发环境--Spring Initializer

[链接](https://start.spring.io)

![image-20210413175933733](Java-NowCoder-Note/image-20210413175933733.png)

- generate project

![image-20210413181551924](Java-NowCoder-Note/image-20210413181551924.png)

- 自己在操作时发现没有AOP，于是只能自己添加！

  ```xml
  <!--使用Spring Initializr时视频中能搜到AOP，我搜不到，下面是手动加的-->
  		<dependency>
  			<groupId>org.springframework.boot</groupId>
  			<artifactId>spring-boot-starter-aop</artifactId>
  		</dependency>
  		<!--使用Spring Initializr时视频中能搜到AOP，我搜不到，上面是手动加的-->
  ```

- 完整的pom.xml文件：

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  	<modelVersion>4.0.0</modelVersion>
  	<parent>
  		<groupId>org.springframework.boot</groupId>
  		<artifactId>spring-boot-starter-parent</artifactId>
  		<version>2.4.4</version>
  		<relativePath/> <!-- lookup parent from repository -->
  	</parent>
  	<groupId>com.nowcoder.community</groupId>
  	<artifactId>community</artifactId>
  	<version>0.0.1-SNAPSHOT</version>
  	<name>community</name>
  	<description>nowcoder community</description>
  	<properties>
  		<java.version>11</java.version>
  	</properties>
  	<dependencies>
  		<!--使用Spring Initializr时视频中能搜到AOP，我搜不到，下面是手动加的-->
  		<dependency>
  			<groupId>org.springframework.boot</groupId>
  			<artifactId>spring-boot-starter-aop</artifactId>
  		</dependency>
  		<!--使用Spring Initializr时视频中能搜到AOP，我搜不到，上面是手动加的-->
  		<dependency>
  			<groupId>org.springframework.boot</groupId>
  			<artifactId>spring-boot-starter-thymeleaf</artifactId>
  		</dependency>
  		<dependency>
  			<groupId>org.springframework.boot</groupId>
  			<artifactId>spring-boot-starter-web</artifactId>
  		</dependency>
  
  		<dependency>
  			<groupId>org.springframework.boot</groupId>
  			<artifactId>spring-boot-devtools</artifactId>
  			<scope>runtime</scope>
  			<optional>true</optional>
  		</dependency>
  		<dependency>
  			<groupId>org.springframework.boot</groupId>
  			<artifactId>spring-boot-starter-test</artifactId>
  			<scope>test</scope>
  		</dependency>
  	</dependencies>
  
  	<build>
  		<plugins>
  			<plugin>
  				<groupId>org.springframework.boot</groupId>
  				<artifactId>spring-boot-maven-plugin</artifactId>
  			</plugin>
  		</plugins>
  	</build>
  
  </project>
  
  ```

- 测试：CommunityApplicationTests

  ![image-20210414104128988](Java-NowCoder-Note/image-20210414104128988.png)

- 运行：CommunityApplication

  ![image-20210414105050597](Java-NowCoder-Note/image-20210414105050597.png)

  Spring boot 会启动 tomcat：http://localhost:8080/

   ![image-20210414105005806](Java-NowCoder-Note/image-20210414105005806.png)

  ------

#### 搭建开发环境 Spring Boot 入门示例



![image-20210414105652894](Java-NowCoder-Note/image-20210414105652894.png)

> 入门示例

1. 新建controller包，再新建AlphaController示例类

![image-20210414115959020](Java-NowCoder-Note/image-20210414115959020.png)

2. 自己写一个sayHello方法，再添上对应的注解

   ![image-20210414120116904](Java-NowCoder-Note/image-20210414120116904.png)

3. 示例代码：

   ```java
   package com.nowcoder.community.controller;
   
   import org.springframework.stereotype.Controller;
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.ResponseBody;
   
   @Controller
   @RequestMapping("/alpha")
   public class AlphaController {
       @RequestMapping("/hello")
       @ResponseBody
       public String sayHello() {
           return "Hello Spring Boot.";
       }
   }
   ```

4. 打开本地服务器页面：http://localhost:8080/alpha/hello

   ![image-20210414120252093](Java-NowCoder-Note/image-20210414120252093.png)

5. 可能会遇到的问题：8080端口被占用了

   - 可以自己在resources文件夹下的application.properties中设置端口

   ```
   server.port=8090
   server.servlet.context-path=/community
   ```

   - 输入：http://localhost:8090/community/alpha/hello

   ![image-20210414121522176](Java-NowCoder-Note/image-20210414121522176.png)

------

## Spring入门

![image-20210414131449300](Java-NowCoder-Note/image-20210414131449300.png)

[Spring 链接](https://spring.io/)

> 学习 [spring-framework](https://spring.io/projects/spring-framework#learn)

![image-20210414131754965](Java-NowCoder-Note/image-20210414131754965.png)

### Spring IOC

> IOC：Inversion of Control
>
> 控制反转

![image-20210414132340909](Java-NowCoder-Note/image-20210414132340909.png)

- 本节课程：[视频链接](https://www.bilibili.com/video/BV1TV411m788?t=1024&p=3)
  - `JavaBean`的介绍：[JavaBean](https://www.liaoxuefeng.com/wiki/1252599548343744/1260474416351680)
  - `Annotation`的介绍：注释会被编译器直接忽略，而注解则可以被编译器打包进入class文件，因此，注解是一种用作标注的“元数据”。
    - [使用注解](https://www.liaoxuefeng.com/wiki/1252599548343744/1265102413966176)
    - [定义注解](https://www.liaoxuefeng.com/wiki/1252599548343744/1265102803921888)
    - [处理注解](https://www.liaoxuefeng.com/wiki/1252599548343744/1265102026065728)
  
  - [IoC容器](https://www.liaoxuefeng.com/wiki/1252599548343744/1266265100383840)
  - 演示依赖注入：
  
  > AlphaDao 接口及其实现类
  
  ```
  // AlphaDao
  package com.nowcoder.community.dao;
  
  public interface AlphaDao {
      String select();
  }
  
  // AlphaDaoMybatisImpl
  package com.nowcoder.community.dao;
  
  import org.springframework.context.annotation.Primary;
  import org.springframework.stereotype.Repository;
  
  @Repository
  @Primary
  public class AlphaDaoMybatisImpl implements AlphaDao{
      @Override
      public String select() {
          return "Mybatis";
      }
  }
  
  // AlphaDaoHibernateImpl
  package com.nowcoder.community.dao;
  
  import org.springframework.stereotype.Repository;
  
  @Repository("alphaHibernate")
  public class AlphaDaoHibernateImpl implements AlphaDao{
      @Override
      public String select() {
          return "Hibernate";
      }
  }
  ```
  
  > AlphaService 
  >
  > - 注入AlphaDao
  >   - @Autowired //给AlphaService注入AlphaDao 
  >   - private AlphaDao alphaDao;
  > - find()：依赖于AlphaDao的方式
  
  ```
  package com.nowcoder.community.service;
  
  import com.nowcoder.community.dao.AlphaDao;
  import org.springframework.beans.factory.annotation.Autowired;
  //import org.springframework.context.annotation.Scope;
  import org.springframework.stereotype.Service;
  
  import javax.annotation.PostConstruct;
  import javax.annotation.PreDestroy;
  
  @Service
  //@Scope("prototype") //每次创建不同的实例
  public class AlphaService {
      @Autowired //给AlphaService注入AlphaDao
      private AlphaDao alphaDao;
  
  	/*	默认使用AlphaDaoMybatisImpl；
  		若要使用AlphaDaoHibernateImpl，
  		修改为：
              @Autowired
              @Qualifier("alphaHibernate")
              private AlphaDao alphaDao;
  	*/
      public AlphaService() {
          System.out.println("实例化AlphaService");
      }
  
      @PostConstruct //在构造后调用
      public void init() {
          System.out.println("初始化AlphaService");
      }
  
      @PreDestroy //在销毁之前调用
      public void destroy() {
          System.out.println("销毁AlphaService");
      }
  
      public String find() { //AlphaService依赖于AlphaDao的方式
          return alphaDao.select();
      }
  }
  ```
  
  > AlphaController
  >
  > - 注入AlphaService
  >   -  @Autowired //给AlphaController注入AlphaService
  >   - private AlphaService alphaService;
  > - getData()：依赖于AlphaService的方式
  
  ```
  package com.nowcoder.community.controller;
  
  import com.nowcoder.community.service.AlphaService;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.stereotype.Controller;
  import org.springframework.web.bind.annotation.RequestMapping;
  import org.springframework.web.bind.annotation.ResponseBody;
  
  @Controller
  @RequestMapping("/alpha")
  public class AlphaController {
  
      @Autowired //给AlphaController注入AlphaService
      private AlphaService alphaService;
  
      @RequestMapping("/hello")
      @ResponseBody
      public String sayHello() {
          return "Hello Spring Boot.";
      }
  
      @RequestMapping("/data")
      @ResponseBody
      public String getData() {
          return alphaService.find();
      }
  }
  ```
  
  > 运行CommunityApplication.main()方法，进行测试
  >
  > - 右键CommunityApplication类
  > - 运行CommunityApplication.main()
  > - 打开：http://localhost:8080/community/alpha/data
  
  ![image-20210414165924161](Java-NowCoder-Note/image-20210414165924161.png)
  
  ------

### Spring MVC

![image-20210418144929314](Java-NowCoder-Note/image-20210418144929314.png)

> 先学习一下 [HTTP协议](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Overview)
>

![image-20210416200140144](Java-NowCoder-Note/image-20210416200140144.png)

> MVC是解决三层架构中表现层的问题：
>
> - 三层架构：浏览器访问网页，首先访问表现层，表现层调用业务层处理业务，处理过程中会访问数据库（数据层），最终得到业务层返回的数据，展示在表现层。
> - 浏览器发送请求访问服务器，访问的是control控制器，它接收请求后调用业务层处理数据，处理完成后返回数据封装到Model层传给View层，生成html给浏览器。

官方链接：[SpringMVC](https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#spring-web)

![image-20210418151143063](Java-NowCoder-Note/image-20210418151143063.png)

![image-20210418151619559](Java-NowCoder-Note/image-20210418151619559.png)

> Thymeleaf
>

![image-20210418151912024](Java-NowCoder-Note/image-20210418151912024.png)

### MyBatis入门

![image-20210418201823518](Java-NowCoder-Note/image-20210418201823518.png)

- 下载链接：[mysql server](https://dev.mysql.com/downloads/mysql/)

  - 新建 my.ini 配置，在（安装）根目录下：

  ```
  [mysqld]
  # 设置3306端口
  port=3306
  # 设置mysql的安装目录
  basedir=D:\MySQL\mysql-8.0.16-winx64
  # 设置mysql数据库的数据的存放目录
  datadir=D:\MySQL\Database
  # 允许最大连接数
  max_connections=200
  # 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
  max_connect_errors=10
  # 服务端使用的字符集默认为UTF8
  character-set-server=utf8
  # 创建新表时将使用的默认存储引擎
  default-storage-engine=INNODB
  # 默认使用“mysql_native_password”插件认证
  default_authentication_plugin=mysql_native_password
  [mysql]
  # 设置mysql客户端默认字符集
  default-character-set=utf8
  [client]
  # 设置mysql客户端连接服务端时默认使用的端口
  port=3306
  default-character-set=utf8
  ```

  - 成功则输出如下内容：

  ```shell
  # 第一步：
  D:\mysql-8.0.16-winx64\bin>mysqld --initialize --console
  2021-04-18T13:26:03.855416Z 0 [System] [MY-013169] [Server] D:\mysql-8.0.16-winx64\bin\mysqld.exe (mysqld 8.0.16) initializing of server in progress as process 17944
  2021-04-18T13:26:03.856934Z 0 [Warning] [MY-013242] [Server] --character-set-server: 'utf8' is currently an alias for the character set UTF8MB3, but will be an alias for UTF8MB4 in a future release. Please consider using UTF8MB4 in order to be unambiguous.
  2021-04-18T13:26:07.939343Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: L/Ad.M?d6>sX
  2021-04-18T13:26:09.432261Z 0 [System] [MY-013170] [Server] 
  D:\mysql-8.0.16-winx64\bin\mysqld.exe (mysqld 8.0.16) initializing of server has completed
  ```

  - 若之前安装过，解决办法：

    ```shell
    # 第二步：
    D:\mysql-8.0.16-winx64\bin>mysqld install
    The service already exists!
    The current server installed: "D:\MySQL\bin\mysqld" --defaults-file="D:\MySQL\my.ini" MySQL
    
    # 关闭窗口重新打开（以管理员权限）
    C:\Windows\system32>sc query mysql
    SERVICE_NAME: mysql
            TYPE               : 10  WIN32_OWN_PROCESS
            STATE              : 1  STOPPED
            WIN32_EXIT_CODE    : 0  (0x0)
            SERVICE_EXIT_CODE  : 0  (0x0)
            CHECKPOINT         : 0x0
            WAIT_HINT          : 0x7d0
            
    C:\Windows\system32>sc delete mysql
    [SC] DeleteService SUCCESS
    ```

  - 重新安装：

    ```shell
    # 第一步：
    D:\MySQL\mysql-8.0.16-winx64\bin>mysqld --initialize --console
    2021-04-18T13:42:46.751081Z 0 [System] [MY-013169] [Server] D:\MySQL\mysql-8.0.16-winx64\bin\mysqld.exe (mysqld 8.0.16) initializing of server in progress as process 19852
    2021-04-18T13:42:46.752336Z 0 [Warning] [MY-013242] [Server] --character-set-server: 'utf8' is currently an alias for the character set UTF8MB3, but will be an alias for UTF8MB4 in a future release. Please consider using UTF8MB4 in order to be unambiguous.
    2021-04-18T13:42:50.354065Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: :rfm;Mr:2TjG
    2021-04-18T13:42:51.666406Z 0 [System] [MY-013170] [Server] D:\MySQL\mysql-8.0.16-winx64\bin\mysqld.exe (mysqld 8.0.16) initializing of server has completed
    
    # 第二步：
    D:\MySQL\mysql-8.0.16-winx64\bin>mysqld install
    Service successfully installed.
    
    # 第三步：
    D:\MySQL\mysql-8.0.16-winx64\bin>net start mysql
    The MySQL service is starting.
    The MySQL service was started successfully.
    ```

  - 第一次登陆修改密码：

    ```mysql
    C:\Users\LeBro>mysql -uroot -p
    Enter password: ************
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 9
    Server version: 8.0.16
    
    Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.
    
    Oracle is a registered trademark of Oracle Corporation and/or its
    affiliates. Other names may be trademarks of their respective
    owners.
    
    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
    
    mysql> alter user root@localhost identified by 'jungle8884';
    Query OK, 0 rows affected (0.01 sec)
    ```

  - 导入数据：

    ```
    use community;
    
    source D:/FastData/community/sql/init_schema.sql；
    
    source D:/FastData/community/sql/init_data.sql；
    ```

    

- 下载链接：[workbench](https://dev.mysql.com/downloads/workbench/)

![image-20210419103300163](Java-NowCoder-Note/image-20210419103300163.png)

- [MyBatis手册](https://mybatis.org/mybatis-3/zh/index.html)
- [spring整合MyBatis](http://mybatis.org/spring/zh/index.html)
- [mvnrepository-mysql包](https://mvnrepository.com/artifact/mysql/mysql-connector-java/8.0.16)
- [mvnrepository-mybatis包](https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter/2.0.1)

```xml
# pom.xml:
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.16</version>
</dependency>
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.0.1</version>
</dependency>

# application.properties:
# DataSourceProperties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/community?characterEncoding=utf-8&useSSL=false&serverTimezone=Hongkong
# spring.datasource.url=jdbc:mysql://localhost:3306/community?characterEncoding=utf-8&useSSL=false&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=jungle8884
spring.datasource.type=com.zaxxer.hikari.HikariDataSource
spring.datasource.hikari.maximum-pool-size=15
spring.datasource.hikari.minimum-idle=5
spring.datasource.hikari.idle-timeout=30000

# MybatisProperties
mybatis.mapper-locations=classpath:mapper/*.xml
mybatis.type-aliases-package=com.nowcoder.community.entity
mybatis.configuration.useGeneratedKeys=true
mybatis.configuration.mapUnderscoreToCamelCase=true
```









### 遇到的问题：

> 问题一：打开项目后，没有提示，无法编译了
>
> 报错：Error: Could not find or load main class com.nowcoder.community.CommunityApplication Caused by: java

解决办法：

![image-20210418160506795](Java-NowCoder-Note/image-20210418160506795.png)

## 开发社区首页

![image-20210420135951481](Java-NowCoder-Note/image-20210420135951481.png)

> 项目代码：https://gitee.com/jungle8884/NowCoder

[百度云链接：提取码：8884 ](https://pan.baidu.com/s/1ebRtHVvth2Rx4rCCR23AYg)















