---
title: web踩坑
date: 2021-06-24
categories: Notes
tags: FrontEnd
---

## Tomcat

1. Eclipse导不进去包，网上的各种导servlet-api.jar以及改buildpath的方法无效。用了tomcat里提供的servlet样例，将javax改名为jakarta之后成功。

   ![](image-20210512141630161.png)

2. Jetbrain学生认证失败，github学生认证失败，遂下了2020.3的破解版IDEA，破解失败，改2020.1，终于成功。
   
   ![](image-20210512140528028.png)

3. IDEA仍然找不到javax包，配置Tomcat提示Application server libraries not found，这次stackoverflow有了正确答案：不支持Tomcat10 : )

   ![](image-20210512140240714.png)

   查tomcat官网可知：

- 在使用Tomcat10时，javax包不可用，javax包被重命名为jakarta，如：若想使用javax.servlet.\*，只需更改为jakarta.servlet.*。
  
- 目前即便是最新的spring(3.5.1)也还尚不支持Tomcat10，原因是spring中的包仍然使用javax包下的对象/接口(也有可能是现在的Tomcat10还并非正式版本)。
  
- JDK8.0之后的JDK版本中已经不耦合JRE。
  
- J2EE更名为JakartaEE
  
   ![](image-20210512140040483.png)

这个故事告诉我们，最新版不一定是什么好东西，尤其对于非正式版本和破解版，下软件看看官网提示，即使它是一点也不想看的英文。

## 用不到的知识

- java 7, 8, 9 代表 product version
  
- 1.7, 1.8, 1.9 代表 developer version

实际上是同一个版本

## IDEA

1. 调用方法[manageApp]时发生异常java.lang.IllegalStateException: 启动子级时出错

在此 IntelliJ IDEA 工程中使用的是 Maven 来构建项目的，而 Maven 中已经有了设置依赖的方法——通过文件pom.xml来设置，但在 Web 应用中是通过在 Web 应用下的目录WEB-INF\lib来设置的。如果同时设置了这两者，这就导致冲突，从而引发上面的报错。同理，如果在 IntelliJ IDEA 中设置了 Sources Root（源代码根目录），然后又在文件夹WEB-INF下投入了目录classes，这同样会导致冲突。因此，解决报错的方法是：

- 选择 Web 应用下的目录WEB-INF\classes作为 Web 应用所需要的已编译 Java 源文件目录。
  
- 选择 Web 应用下的目录WEB-INF\lib作为依赖的配置。
  
或

- 在 IntelliJ IDEA 中标记某一个文件夹作为 Java 源文件目录。
  
- Maven：在文件pom.xml中设置依赖

另外，在产生了报错之后，可能会破坏此 IntelliJ IDEA 工程的环境，导致以后即使运行配置正确的 Web 应用，也将一直发生相同的报错，即上面的错误。此时，可以在解决报错的问题之后，删除此 IntelliJ IDEA 工程下，上一次编译生成的文件夹（如 Maven 编译产生的文件夹 target），然后重新编译即可。
