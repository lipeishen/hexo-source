title: Spring 基础
date: 2014-04-13 23:31:44
tags: Spring
language: ch
categories: Java
---

<!--more-->
##Spring 基本知识

![1](/img/20.jpg)

  从简单性、可测试性和松耦合的角度而言，任何Java应用都可以从Spring中受益。简单来说，Spring就是一个轻量级的控制反转（IoC）和面向切面（AOP）的容器框架。
  
  下面从整体上认识一下Spring的主要特征：
*轻量：从大小与开销两方面而言Spring都是轻量的。此外，Spring是非侵入式的：使用Spring,我们的类还是pojo类，完全不用继承和实现Spring的类和接口等。
也就是说，使用Spring的应用中的对象不依赖于Spring的特定类。*

*IoC:Spring通过控制反转技术促进了松耦合。当应用了IoC,一个对象依赖的其它对象会通过被动的方式传递进来，而不是这个对象自己创建或者查找依赖对象。可以认为IoC与JNDI相反--不是我们自己控制对象从容器中查找依赖，而是容器在对象初始化时不等对象请求就主动将依赖传递给它，这就是DI--依赖注入。依赖注入有三种方式：构造函数注入，设值方法注入，借口注入，前两种最常用。
基本上就是对象不用自己动手管理和创建。完全由容器管理，我们只管用就行。*

*AOP:Spring提供了面向切面的编程支持，AOP将与程序业务无关的内容分离提取，应用对象只实现它们应该做的--完成业务逻辑--仅此而已。它们并不负责其它的系统级关注点，例如日志或事务支持。AOP只是一种思路而不算是具体的实现技术。Spring的Aop是建立在Java的代理机制之上的
AOP将与业务无关的逻辑横切进真正的逻辑中。*

###《Spring参考手册》中定义了以下几个AOP的重要概念，结合以上代码分析如下：

 • 切面（Aspect） ：官方的抽象定义为“一个关注点的模块化，这个关注点可能会横切多个对象”，在本例中，“切面”就是类TestAspect所关注的具体行为，例如，AServiceImpl.barA()的调用就是切面TestAspect所关注的行为之一。“切面”在ApplicationContext中<aop:aspect>来配置。
  
•	连接点（Joinpoint） ：程序执行过程中的某一行为，例如，AServiceImpl.barA()的调用或者BServiceImpl.barB(String _msg, int _type)抛出异常等行为。
•	通知（Advice） ：“切面”对于某个“连接点”所产生的动作，例如，TestAspect中对com.spring.service包下所有类的方法进行日志记录的动作就是一个Advice。其中，一个“切面”可以包含多个“Advice”，例如TestAspect

•	切入点（Pointcut） ：匹配连接点的断言，在AOP中通知和一个切入点表达式关联。例如，TestAspect中的所有通知所关注的连接点，都由切入点表达式execution(* com.spring.service.*.*(..))来决定

•	目标对象（Target Object） ：被一个或者多个切面所通知的对象。例如，AServcieImpl和BServiceImpl，当然在实际运行时，Spring AOP采用代理实现，实际AOP操作的是TargetObject的代理对象。

•	AOP代理（AOP Proxy） 在Spring AOP中有两种代理方式，JDK动态代理和CGLIB代理。默认情况下，TargetObject实现了接口时，则采用JDK动态代理，例如，AServiceImpl；反之，采用CGLIB代理，例如，BServiceImpl。强制使用CGLIB代理需要将 <aop:config> 的 proxy-target-class 属性设为true

###  通知（Advice）类型
•	前置通知（Before advice） ：在某连接点（JoinPoint）之前执行的通知，但这个通知不能阻止连接点前的执行。ApplicationContext中在<aop:aspect>里面使用<aop:before>元素进行声明。例如，TestAspect中的doBefore方法

•	后通知（After advice） ：当某连接点退出的时候执行的通知（不论是正常返回还是异常退出）。ApplicationContext中在<aop:aspect>里面使用<aop:after>元素进行声明。例如，TestAspect中的doAfter方法，所以AOPTest中调用BServiceImpl.barB抛出异常时，doAfter方法仍然执行

•	返回后通知（After return advice） ：在某连接点正常完成后执行的通知，不包括抛出异常的情况。ApplicationContext中在<aop:aspect>里面使用<after-returning>元素进行声明。

•	环绕通知（Around advice） ：包围一个连接点的通知，类似Web中Servlet规范中的Filter的doFilter方法。可以在方法的调用前后完成自定义的行为，也可以选择不执行。ApplicationContext中在<aop:aspect>里面使用<aop:around>元素进行声明。例如，TestAspect中的doAround方法。

•	抛出异常后通知（After throwing advice） ： 在方法抛出异常退出时执行的通知。 ApplicationContext中在<aop:aspect>里面使用<aop:after-throwing>元素进行声明。
例如，TestAspect中的doThrowing方法。

###下面的代码是Spring通知的用法：

1,前置性通知
//SayHello.java

    package sunyang.BeforeAdvice;

    public class SayHello {
	public void sayHello() {
		System.out.println("Hello......");
	 }
    }

//Smile.java

     package sunyang.BeforeAdvice;

     import java.lang.reflect.Method;

     import  org.springframework.aop.MethodBeforeAdvice;

    public class Smile implements MethodBeforeAdvice {

	public void before(Method arg0, Object[] arg1, Object arg2)
			throws Throwable {
		System.out.println("Smile......");
	 }

    }
   
// TestBeforeAdivce.java

    package sunyang.BeforeAdvice;

    import org.springframework.aop.BeforeAdvice;
    import org.springframework.aop.framework.ProxyFactory;

    public class TestBeforeAdvice {
	public static void main(String[] args) {
		SayHello sayHello = new SayHello();
		BeforeAdvice ba = new Smile();
		ProxyFactory pf = new ProxyFactory();
		pf.setTarget(sayHello);
		pf.addAdvice(ba);
		SayHello sh = (SayHello) pf.getProxy();
		sh.sayHello();
	}
     }
     
2 环绕型通知只需要更改第三个文件为如下：

//TestMethodInterceptor.java

    package sunyang.MethodInterceptor;

    import org.aopalliance.intercept.MethodInterceptor;
    import org.springframework.aop.framework.ProxyFactory;

    public class TestMethodInterceptor {
	public static void main(String[] args) {
		SayHello sayHello = new SayHello();
		MethodInterceptor ba = new Smile();
		ProxyFactory pf = new ProxyFactory();
		pf.setTarget(sayHello);
		pf.addAdvice(ba);
		SayHello sh = (SayHello) pf.getProxy();
		sh.sayHello();
	  }
    }


*框架：Spring可以将简单的组件配置、组合成为复杂的应用。在Spring中，应用对象被声明式地组合，典型地是在一个XML文件里。Spring也提供了很多基础功能（事务管理、持久化框架集成等等），而用户就有更多的时间和精力去开发应用逻辑

所有Spring的这些特征都能帮助我们够编写更干净、更可管理、并且更易于测试的代码。它们也为Spring中的各种模块提供了基础支持。

*借助Spring,荣国依赖注入，AOP应用，面向接口编程，来降低业务组件之间的耦合度，增强系统的扩展性。

*
虽然Spring可以一站式解决整个项目问题，但是Spring并不想取代那些已有的框架，而是与它们无缝地整合。Spring可以降低各种框架的使用难度，他提供了对各种优秀框架（如Struts、Hibernate、Hessian、Quartz等）的直接支持。

使用Spring的主要目的是使J2EE易用和促进好的编程习惯，Spring的目标就是让已有的技术更加易用。
所以Spring的一个重要思想就是整合和兼容。
Spring 中所有组件都已Bean的形式存在，要想使用容器管理Bean ,就必须在配置文件中规定好各个bean 的属性和依赖关系。


