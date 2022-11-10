---
layout: post
title:  "Spring @Autowired 问题记录"
date:   2022-11-10 21:53:34 +0800
categories: 源码学习
---
* 目录
{:toc #markdown-toc}


# Spring @Autowired 问题记录

## 问题复现

1. 问题背景

   一个service需要通过某个rpc中间件进行服务暴露，于是在xml文件中向容器注册了该Service,但是问题是，这个service的实现类上还使用了Spring的@Service注解，这就导致一个bean被注册到容器中两次。

   于是，应用启动的时候，报了(以下为复现代码报错)

   * 报错

     ```shell
     Field sayHelloService in demo.spring.bugreview.muchBean.HelloController required a single bean, but 2 were found:
     	- helloServiceImpl: defined in file [D:\work\demo\target\classes\demo\spring\bugreview\muchBean\HelloServiceImpl.class]
     	- helloService: defined in class path resource [applicationContext.xml]
     ```

2. 问题复现

   1. 服务接口

      ```java
      public interface HelloService {
          void sayHello();
      }
      ```

   2. 服务实现

      ```java
      @Service
      public class HelloServiceImpl implements HelloService {
      	@Override
      	public void sayHello() {
      		System.out.println("hello world!");
      	}
      }
      ```

   3. xml 注册bean

      ```xml
      <bean id="helloService" class="demo.spring.bugreview.muchBean.HelloServiceImpl"/>
      ```

   4. 被注入方

      ```java
      @Controller
      public class HelloController {
          @Autowired
          private HelloService sayHelloService;
          @PostMapping("/dog/hello")
          public void  sayDogHello(){
              helloService.sayHello();
          }
      }
      ```

## 为什么

1. 问题解析

   首先，bean在容器中注册了两次，一次通过xml,一次通过注解@Service.并且两次注册名都不相同，xml中未显示指定name,于是默认name=id,即helloService;注解注册则名字为类名HelloServiceImpl的一点小变形即helloServiceImpl

   所以当HelloController注入HelloService的时候，@Autowired是按照类型注入的，有两个相同的类型需要注入一个依赖，所以报错。

2. 怎么解决

   1.  问题bean的定义与依赖注入

      ```java
      @Autowired
      private HelloService sayHelloService;
      <bean id="helloService" class="demo.spring.bugreview.muchBean.HelloServiceImpl"/>
      @Autowired
      private HelloService helloService;
      ```

   2. 通过@Qualifier 进行再限定

      通过查看Spring 文档，其中与此问题相关的另一个注解为@Qualifier 他是通过名字来限定过滤匹配到的容器中的多个bean.可以显示增加@Qualifier("helloService")两个bean，选择xml中那个进行解决.解决方案如下

      ```java
      public class HelloController {
      	@Autowired
          @Qualifier("helloService")
      	private HelloService sayHelloService;
      	@PostMapping("/dog/hello")
      	public void  sayDogHello(){
      		sayHelloService.sayHello();
      	}
      }
      
      ```

   3. 通过设置合适的依赖注入的变量名

      一般来说，引用名称和xml中的id,或name名称一般是相同的习惯，即接口名的首字母小写，即helloService,但是当时需求初步实现比较随意写成了sayHelloService。但是如果我正常命名即helloService那么这个依赖注入是不会报错的。

      因为通过查阅spring文档，@Autowaired如果遇到容器中存在相同类型的Bean,那么会对引用名称进行进一步的过滤，即和@Qualifier("注入的参数名称")具有相同的作用

      ```English
      However, although you can use this convention to refer to specific beans by name,
      @Autowired is fundamentally about type-driven injection with optional semantic qualifiers. This
      means that qualifier values, even with the bean name fallback, always have narrowing semantics
      within the set of type matches.
      ```

      此前，我们定义的sayHelloService没有匹配任何一个注册在容器中的对象，所以过滤后还是两个所以报错。当对引用名称进行修改后就没有报错了

      ```java
      @Controller
      public class HelloController {
          @Autowired
          private HelloService helloService;
          @PostMapping("/dog/hello")
          public void  sayDogHello(){
              helloService.sayHello();
          }
      }
      ```

   4. 使用唯一的注册方式

      只使用xml进行注册，或只使用注解进行bean容器注册

## 其他问题

1. 当使用两种方式，xml和注解分别对容器注册相同name的bean时，会产生什么问题？

   * 注解方式

     ```java
     @Service(value = "helloService")
     public class HelloServiceImpl implements HelloService {
     	@Override
     	public void sayHello() {
     		System.out.println("hello world!");
     	}
     }
     
     ```

   * xml方式

     ```xml
     <bean id="helloService" class="demo.spring.bugreview.muchBean.HelloServiceImpl"/>
     ```

     结果：报错，推荐解决办法：允许bean-definition覆盖。配置后问题解决;此时容器中HelloService这个类只要一个实例，故引用名可以随意

     ```shell
     The bean 'helloService', defined in class path resource [applicationContext.xml], could not be registered. A bean with that name has already been defined in file [D:\work\demo\target\classes\demo\spring\bugreview\muchBean\HelloServiceImpl.class] and overriding is disabled.
     
     Action:
     
     Consider renaming one of the beans or enabling overriding by setting spring.main.allow-bean-definition-overriding=true
     ```

     

















