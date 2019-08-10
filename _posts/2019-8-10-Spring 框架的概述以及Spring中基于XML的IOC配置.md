---
title: Spring 框架的概述以及Spring中基于XML的IOC配置
date: 2019-08-10 21:09:00
tags: Spring
category: JavaEE
---

# Spring 框架的概述以及Spring中基于XML的IOC配置

## 一、简介

1. Spring的两大核心：**IOC**（DI）与**AOP**，IOC是反转控制，DI依赖注入
2. 特点：轻量级、依赖注入、面向切面编程、容器、框架、一站式
3. 优势：
   1. 方便解耦：**做到编译期不依赖，运行期才依赖**
   2. AOP的支持
   3. 声明式事务的支持
   4. 方便程序的测试
   5. 方便整合各种框架
   6. 降低JavaEE API的使用难度
   7. Spring源码很厉害



解耦：

- 耦合包括：类之间的和方法之间的

- 解决的思路：
  1. 在创建对象的时候用反射来创建，而不是new
  2. 读取配置文件来获取要创建的对象全限定类名
- Bean：在计算机英语中有可重用组件的含义
- javabean（用java语言编写的可重用组件）>实体类

## 工厂类解耦

```java
/**
 * Bean：可重用组件
 */
public class BeanFactory {
   private static Properties props;
    //静态代码块
    static{
        try {
            //1.实例化Properties对象
            props=new Properties();
            //2.获取Properties文件的流对象
            InputStream in = BeanFactory.class.getClassLoader().getResourceAsStream("bean.properties");
            props.load(in);
        }
        catch (Exception e){
            throw new ExceptionInInitializerError("初始化properties失败");
        }

    }
}
```

```java
/**
     * 根据bean的名称获取bean对象
     * @param beanName
     * @return
     */
    public static Object getBean(String beanName){
        Object bean = null;

        try {
            String beanPath = props.getProperty(beanName);
            bean = Class.forName(beanPath).newInstance();
        }catch (Exception e){
            e.printStackTrace();
        }

        return bean;
    }
```

```java
//    IAccountDao accountDao=new AccountDaoImpl();
    IAccountDao accountDao = (IAccountDao) BeanFactory.getBean("accountDao");
```

