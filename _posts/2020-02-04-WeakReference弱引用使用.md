---
layout:     post
title:      WeakReference弱引用使用
subtitle:   WeakReference弱引用使用
date:       2020-02-04
author:     BenjaminLiu
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - java

---

>WeakReference弱引用使用



## WeakReference弱引用使用


```java
 static WeakReference<User> user = new WeakReference<>(new User(1, new byte[1]));

    public static void main(String[] args) {
        int i = 0;
        while (true) {
            //将注释打开，用一个强引用来引用thread local 对象
            /*
            输出：是正常循环中，说明弱对象没有被回收，所以以上概念不够严谨，应该是没有强引用引用的弱对象会被gc回收
             */
//            User u=user.get();
            if (user.get() != null) {
                i++;
                System.out.println("Object is alive for " + i + " loops ");
            } else {
                System.out.println("Object has been collected.");
                break;
            }
            //由概念可知无论内存是否足够，只要gc弱引用就会被释放。
            System.gc();
        }

```



```java
// output:

Object is alive for 1 loops - 
Object has been collected.

```