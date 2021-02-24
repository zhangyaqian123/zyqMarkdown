# SpringBoot属性注入

## 前言——Springboot常用注入方式

* 构造器注入

* setter注入

* 注解注入

  **区别：**

  ***构造器注入*** 是自己重写了带参构造函数，此时Spring构建这个类的时候就只能用自己写的构造器，但是构造器里面需要其他参数，Spring会帮忙将参数初始化，然后注入进去。

  *优点* ：此时Spring不允许参数为null，和存在循环依赖的。

  *缺点*： 当需要注入的对象较多时，代码特别冗余，维护困难。-----但是这意味着这个类的责任太多，需要被重构。

  ***setter* 注入**：

  *优点*：只有当对象需要使用时才会被注入。

  ***注解注入*** ： 

  *优点：* 精短，可读性高，方便维护。

  *缺点：* 

  - 会有空指针风险
  - 不能将对象设为final

常用注解含义大全

https://blog.csdn.net/weixin_40753536/article/details/81285046

## 1. @SpringBootApplication

​	包含了@ComponentScan、@Configuration和@EnableAutoConfiguration注解。其中@ComponentScan让spring Boot扫描到Configuration类并把它加入到程序上下文。

​	申请让SpringBoot自动给出程序进行必要的配置，这个配置等同于：@Configuration, @EnableAutoConfiguration, @ComponentScan这三个配置。springboot是通过注解`@EnableAutoConfiguration`的方式，去查找，过滤，加载所需的`configuration`

**@ComponentScan** : 用来自动扫描被注解标识的类，最终生成ioc容器里的bean。

## 2. @Configuration

相当于传统的.xml配置文件，其配合@bean可以将方法注册为bean，并交给Spring管理

## 3. @Component

泛指组件，当组件不好归类的时候，我们可以使用这个注解进行标注。

  ## 4. 

- @Controller

- @Service

- @Reopsitory

- @Component

  这几个注解并没有功能上的区别，相当于一个分类标签。

  https://stackoverflow.com/questions/6827752/whats-the-difference-between-component-repository-service-annotations-in

## 5.@RestController

：注解是@Controller和@ResponseBody的合集,表示这是个控制器bean,并且是将函数的返回值直 接填入HTTP响应体中,是REST风格的控制器









