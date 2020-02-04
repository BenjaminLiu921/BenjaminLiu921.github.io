---
layout:     post
title:      @Transactional如何实现的？
subtitle:   @Transactional如何实现的？
date:       2020-02-04
author:     BenjaminLiu
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - java

---

>为何在service的方法上添加 @Transactional 就能实现事务的回滚和提交等功能?



## 为何在service的方法上添加 @Transactional 就能实现事务的回滚和提交等功能？


```java
spring-aop-5.0.4.RELEASE-sources.jar  JdkDynamicAopProxy.java:197
// Get the interception chain for this method.
List<Object> chain = this.advised.getInterceptorsAndDynamicInterceptionAdvice(method, targetClass);

```

拦截器链 ：此时会将方法上注解的拦截器实现类加入到拦截器链中

例如在一个service的方法上添加 @Transactional
```java
@Autowired  
UserDao userDao;

@Override
@Transactional
public void addUser() {
    System.out.println("add user test");
    // add to db
    this.userDao.insert(new User(1, "xiaoA"));
}
```

spring 动态代理

**spring会将当前service生成一个代理类**，用于代理该service,
在 **JdkDynamicAopProxy#invoke()** 中，会根据方法名和该方法注解的拦截器，放入到一个 **Map<MethodCacheKey, List<Object>> methodCache**  （map 拦截器链）中，
如：org.springframework.transaction.interceptor.TransactionInterceptor
返回该map中的拦截器的列表，
然后调用该list中的拦截器的invoke()，从而实现@Transactional事务的提交和回滚等的功能。