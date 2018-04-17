---
title: ゼロから始めるJAVA世界生活 01
date: 2018-04-17 20:20:06
tags: oracle
---

## 环境搭建

接下来将介绍如何建立环境并支持**VSCode**。

1. 从[官网](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)下载jdk

2. 等待一段时间安装完成

3. 在系统的环境变量设置中，全局变量里添加`JAVA_HOME`并将值设置为`C:\Program Files\Java\jdk-*\`，即jdk根目录。

4. 变量中的`PATH`添加值`%JAVA_HOME%\jre\`，`%JAVA_HOME%\lib\`和`%JAVA_HOME%\bin\`，此时测试以下命令
    ```
$ javac -version
$ java -version
    ```
    若能成功显示版本号则安装成功。

<!--more-->

### VSCode

安装 VSCode 后，添加插件`Java Extension Pack`。

在首选项中覆盖以下条目：
```
"files.autoGuessEncoding" : true,
"java.home" : "%JAVA_HOME%"
```
环境变量中添加`JDK_HOME`，值与`JAVA_HOME`相同。

### Eclipse

该软件能自动寻找jdk环境。

## 概念描述

### 编译过程

```sequence
Title: Processing
*.jar-->RunTimeEnv: classPath
*.java->*.class: $ javac
*.class->RunTimeEnv: $ javaw
*.java-->RunTimeEnv: classname
```
### package

package是java的**逻辑**文件夹，在文件级别上，也会依据分类。

package能够用`.`来连接
```java
    import package1.package2.package3.class1 //意味着将三层逻辑文件夹里的class1类声明使用
```

### javadoc

#### usage
```
$ javadoc [options] [packagename] [sourcefiles] [@files]
$ javadoc -d doc *.java
```
根据文档中关键词生成文档，诸如`@param`，`@returns`，`@throw`可用于方法注释。

#### format
```java
/**
 * your note here
 * @auther Kitekii
 * @return a happy number
 */
int static foo();
```

## 语法

### 元件

初始化变量：

```java
int a = 0;
```
由五个部分组成

    **数据类型** *名称* 运算符 **初始化值** 分号
    
### operator

包括`+`,`-`,`*`,`/`,`=`,`==`,`!=`,`>`,`<`,`>>`,`<<`,`<=`,`>=`,`?*:`,`~`,`()`,`[]`,`{}`,`&&`,`||`,`^`在内的二元、三元运算符。

## 面向对象设计

java是纯面向对象语言，风格和C#类似

类 -> 实例1，实例2，...

`实例1.fun();`，`类.(static)foo();`。
