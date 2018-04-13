---
layout: post
title: "HashMap和HashTable的那点事"
description: "HashMap和HashTable的那点事"
categories: [java基础]
tags: [HashMap,HashTable]
redirect_from:
  - /2013/07/09/
---
> 简介：HashMap是基于哈希表的Map接口的非同步实现。此实现提供所有可选的映射操作，并允许使用null值和null键。此类不保证映射的顺序，特别是它不保证该顺序恒久不变,通过hashCode()和equals方法保证key的唯一性。

## HashMap的数据结构
> 在Java编程语言中，最基本的结构就是两种，一个是数组，另外一个是模拟指针（引用），所有的数据结构都可以用这两个基本结构来构造的，HashMap也不例外。HashMap实际上是一个“链表散列”的数据结构，即数组和链表的结合体。
> 
> HashMap底层就是一个数组结构，数组中的每一项又是一个链表。当新建一个HashMap的时候，就会初始化一个数组。
> 
> Entry就是数组中的元素，每个 Map.Entry 其实就是一个key-value对，它持有一个指向下一个元素的引用，这就构成了链表。

## HashMap和HashTable的区别
1. 继承不同  
    public class HashMap extends AbstractMap implements Map  
    public class HashTable extends Dictionary implements Map

2. HahsMap的方法在缺省情况下是非同步的，HashTable的方法是同步的。具体的可看两者相关源码，HashTable的方法，都有加syschronized关键字同步操作

3. HashMap允许使用null键和null值，null键只有一个，null值可以有一个或多个。故当get(k)  返回null值时，不能代表没有该键,也可以表示该键的值为null。其他方法有containKey(obejct key)、 containVale(Object value)  
   HashTable不允许null键和null值。其它方法有contain(Object value)、containKey(obejct key)、containVale(Object value)

4. HashMap和HashTable  
   HashMap中的数组默认大小是16，而且一定是2的倍数  
   HashTable的数组默认大小是11，增加方式是old*2+1
 
5. 哈希值使用不同，HashMap直接重新计算hash值，HashTable直接使用对象的HashCode

6. HashMap和HashTable遍历方式都使用了iterator,但HashTable还使用了Enumeration的方式。

## HashMap的两种遍历方式
第一种(效率高)：
>      Map map = new HashMap();
    　　Iterator iter = map.entrySet().iterator();
    　　while (iter.hasNext()) {
    　　Map.Entry entry = (Map.Entry) iter.next();
    　　Object key = entry.getKey();
    　　Object val = entry.getValue();
    　　}
    
第二种(效率低)：
>       Map map = new HashMap();
    　　Iterator iter = map.keySet().iterator();
    　　while (iter.hasNext()) {
    　　Object key = iter.next();
    　　Object val = map.get(key);
    　　}