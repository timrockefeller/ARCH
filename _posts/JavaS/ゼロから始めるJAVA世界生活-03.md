---
title: ゼロから始めるJAVA世界生活 03
date: 2018-05-13 14:30:00
tags: [oracle,java,coding]
---
关于数组、函数的碎片知识。
<!--more-->

## 数组定义
数组这个东西十分有趣，它的定义是**相同类型**的一组数据集变量。
在Java中，它能够这样被定义：
```java
int n = 5;
int a[] = new int[n];
int []b = new int[n];
```
c++中了解到，数组的本质是**指针**，而Java里面并没有指针的**强类型概念**，但分配空间的规则仍旧是一样的。即当定义一个数组时需要：
	* 数据类型（int）
	* 数组名称（a）
	* 数组 ~长度~ （n=5）
我们在使用数组时，用下标来获取、赋予内容，如`a[0]=a[1]===a[n-4]?a[4]:a[0];`
## 一些算法
算法问题是个 ~玄学~ ，希望哪天也能写出一段连自己都不知道怎么回事的代码并标上`//神奇代码，勿动`。
查找
### 二分查找（binary search）
需要**已排序**的数组array。
```java
public static int biSearch(int [] array, int a){
    int lo = 0;
    int hi = array.length - 1;
    int mid;
    while(lo <= hi){
        mid = (lo + hi) / 2;
        if(array[mid] == a){
            return mid + 1;
        }else if(array[mid] < a){
            lo=mid + 1;
        }else{
            hi=mid - 1;
        }
    }
    return -1;
}
```
### 冒泡排序（bubble sorting）
```java
public static void BubbleSorting(int[] array){
    for(int i=0;i<array.length-1;i++){
    for(int j=0;j<array.length-1-i;j++){
        if(array[j]>array[j+1]){
            int temp=array[j];
            array[j]=arr[j+1];
            array[j+1]=temp;
        }
　　}}
}
```
## API有话说
JDK API几乎把所有的Java自带特性讲了个遍，这里需要了解一个名词：**重载**。函数重载可使函数的 ~参数~ 变得更多样。
上文提及的算法大部分都能由Java Utility Package中Arrays类的 ~静态函数~ 更加快速、低占用地实现。
```java
Arrays.binarySearch(T[] origin, T key); //return the key number
Arrays.sort(T[] origin);
```
以及Array类型的数据自带一些属性，著名的是`·length`。
## 函数
参阅[作用域](https://www.cnblogs.com/AlanLee/p/6627949.html)。
一个函数会构建一个AO作用域，为其分配 ~独立~ 空间。
```
[public|private] <static> T arg(..T args){
    //do something 
}
```
上面这就是一个挂在类里面的函数，可以闭包、重用。
最近好像**函数式编程**挺火的，可以了解一下。