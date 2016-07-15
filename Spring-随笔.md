---
title: Spring 随笔
date: 2016-07-15 20:31:06
tags: 服务器
---
# IOC 和 AOP
控制反转模式（也称作依赖性介入）的基本概念是：不创建对象，但是描述创建它们的方式。在代码中不直接与对象和服务连接，但在配置文件中描述哪一个组件需要哪一项服务。容器 （在 Spring 框架中是 IOC 容器） 负责将这些联系在一起。在典型的 IOC 场景中，容器创建了所有对象，并设置必要的属性将它们连接在一起，决定什么时间调用方法。
***
# IOC/DI
首先想说说IoC（Inversion of Control，控制倒转）。这是spring的核心，贯穿始终。所谓IoC，对于spring框架来说，就是由spring来负责控制对象的生命周期和对象间的关系。这是什么意思呢，举个简单的例子，我们是如何找女朋友的？常见的情况是，我们到处去看哪里有长得漂亮身材又好的mm，然后打听她们的兴趣爱好、qq号、电话号、ip号、iq号………，想办法认识她们，投其所好送其所要，然后嘿嘿……这个过程是复杂深奥的，我们必须自己设计和面对每个环节。传统的程序开发也是如此，在一个对象中，如果要使用另外的对象，就必须得到它（自己new一个，或者从JNDI中查询一个），使用完之后还要将对象销毁（比如Connection等），对象始终会和其他的接口或类藕合起来。

那么IoC是如何做的呢？有点像通过婚介找女朋友，在我和女朋友之间引入了一个第三者：婚姻介绍所。婚介管理了很多男男女女的资料，我可以向婚介提出一个列表，告诉它我想找个什么样的女朋友，比如长得像李嘉欣，身材像林熙雷，唱歌像周杰伦，速度像卡洛斯，技术像齐达内之类的，然后婚介就会按照我们的要求，提供一个mm，我们只需要去和她谈恋爱、结婚就行了。简单明了，如果婚介给我们的人选不符合要求，我们就会抛出异常。整个过程不再由我自己控制，而是有婚介这样一个类似容器的机构来控制。Spring所倡导的开发方式就是如此，所有的类都会在spring容器中登记，告诉spring你是个什么东西，你需要什么东西，然后spring会在系统运行到适当的时候，把你要的东西主动给你，同时也把你交给其他需要你的东西。所有的类的创建、销毁都由 spring来控制，也就是说控制对象生存周期的不再是引用它的对象，而是spring。对于某个具体的对象而言，以前是它控制其他对象，现在是所有对象都被spring控制，所以这叫控制反转。如果你还不明白的话，我决定放弃。

IoC的一个重点是在系统运行中，动态的向某个对象提供它所需要的其他对象。这一点是通过DI（Dependency Injection，依赖注入）来实现的。比如对象A需要操作数据库，以前我们总是要在A中自己编写代码来获得一个Connection对象，有了 spring我们就只需要告诉spring，A中需要一个Connection，至于这个Connection怎么构造，何时构造，A不需要知道。在系统运行时，spring会在适当的时候制造一个Connection，然后像打针一样，注射到A当中，这样就完成了对各个对象之间关系的控制。A需要依赖 Connection才能正常运行，而这个Connection是由spring注入到A中的，依赖注入的名字就这么来的。那么DI是如何实现的呢？ Java 1.3之后一个重要特征是反射（reflection），它允许程序在运行的时候动态的生成对象、执行对象的方法、改变对象的属性，spring就是通过反射来实现注入的。

概念：IoC (Inverse of Control，控制反转)，是指类A中有一个类B的对象，本来需要开发者在类A中初始化这个对象的，现在经过配置，spring可以自动地完成类A中的类B对象的初始化。这个过程也可以被叫作DI (Depend Injection，依赖注入)，因为B类对象依赖于A类，通过spring 类B变量被注入到了A类的对象中。
作用（好处）：配置灵活。 IOC模式，系统中通过引入实现了IOC模式的IOC容器，即可由IOC容器来管理对象的生命周期、依赖关系等，从而使得应用程序的配置和依赖性规范与实 际的应用程序代码分开。其中一个特点就是通过文本的配件文件进行应用程序组件间相互关系的配置，而不用重新修改并编译具体的代码。 因为把对象生成放在了XML里定义，所以当我们需要换一个实现子类将会变成很简单（一般这样的对象都是现实于某种接口的），只要修改XML就可以了。虽然说修改XML后不需要重新编译java代码，但XML经常与java源代码一起打包。所有想要修改XML的话，还是需要重新打包，还是免不了重新发布。就算修改java代码，重新编译这些修改后的代码，也不是太麻烦。那使用IOC模式最本质的好处是什么呢？有一种说法是，写大型程序的时候，会用到很多其他人开发的java类。当程序员甲用到程序员乙开发的java类A时，如果使用IOC，在程序员乙写好XML配置文件后，程序员甲就不需要关心类A应如果初始化的问题，直接使用即可。IOC有利于在多人开发大型程序中提高开发效率。
实现原理：java的反射机制。
***
# AOP
AOP (Aspect Oriented Programming，面向切面编程)，是指通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术，是对面向对象思维方式的有力补充。我的理解中，可以做个形象的类比：甲要去办一件事情1，需要经过ABCD...流程。但这时，在B步聚前，被叫去先办了另一件事情2，完成之后又回来继续完成事情1的剩余步骤。被打断先去完成另一件事情，这个可以类比为AOP。AOP通过被管理记录日志、记录函数执行时间、事务管理等与进行数据处理主干无关的功能上。
作用（好处）：AOP应用到项目中的好处，能够将与业务逻辑不相关的代码（如：日志、权限等）分离出来，减小相关业务类负担，并能让一些通用需求（如：事务）得到更广泛的复用。可以动态的添加和删除在切面上的逻辑而不影响原来的执行代码。
实现原理：通过Java的动态代理来创建AOP代理。Spring 中 AOP 代理由 Spring 的 IoC 容器负责生成、管理，其依赖关系也由 IoC 容器负责管理。因此，AOP 代理可以直接使用容器中的其他 Bean 实例作为目标。
***
# componentscan与annotation-driven
Spring中的ApplicationContexts可以被限制在不同的作用域。在web框架中，每个DispatcherServlet有它自己的WebApplicationContext，它包含了DispatcherServlet配置所需要的bean。DispatcherServlet 使用的缺省BeanFactory是XmlBeanFactory，并且DispatcherServlet在初始化时会在你的web应用的WEB-INF目录下寻找[servlet-name]-servlet.xml文件。DispatcherServlet使用的缺省值可以使用servlet初始化参数修改，
WebApplicationContext仅仅是一个拥有web应用必要功能的普通ApplicationContext。它和一个标准的ApplicationContext的不同之处在于它能够解析主题，并且它知道和那个servlet关联（通过到ServletContext的连接）。WebApplicationContext被绑定在ServletContext上，当你需要的时候，可以使用RequestContextUtils找到WebApplicationContext。
Spring的DispatcherServlet有一组特殊的bean，用来处理请求和显示相应的视图。这些bean包含在Spring的框架里，（可选择）可以在WebApplicationContext中配置，配置方式就象配置其它bean的方式一样。这些bean中的每一个都在下面被详细描述。待一会儿，我们就会提到它们，但这里仅仅是让你知道它们的存在以便我们继续讨论DispatcherServlet。对大多数bean，都提供了缺省值，所有你不必要担心它们的值。 
`<annotaion-driven/>` 
这个标签对应的实现类是org.springframework.web.servlet.config.AnnotationDrivenBeanDefinitionParser
仔细阅读它的注释文档可以很明显的看到这个类的作用。解析这个文档：
这个类主要注册8个类的实例：
1.RequestMappingHandlerMapping  
2.BeanNameUrlHandlerMapping  
3.RequestMappingHandlerAdapter  
4.HttpRequestHandlerAdapter  
5.SimpleControllerHandlerAdapter  
6.ExceptionHandlerExceptionResolver  
7.ResponseStatusExceptionResolver  
8.DefaultHandlerExceptionResolver  
1是处理@RequestMapping注解的。  
2.将controller类的名字映射为请求url。1和2都实现了HandlerMapping接口，用来处理请求映射。  
3是处理@Controller注解的控制器类  
4是处理继承HttpRequestHandlerAdapter类的控制器类  
5.处理继承SimpleControllerHandlerAdapter类的控制器。所以这三个是用来处理请求的。具体点说就是确定调用哪个controller的哪个方法来处理当前请求。  
6,7,8全部继承AbstractHandlerExceptionResolver，这个类实现HandlerExceptionResolver，该接口定义：接口实现的对象可以解决处理器映射、执行期间抛出的异常，还有错误的视图。  
所以<annotaion-driven/>标签主要是用来帮助我们处理请求映射，决定是哪个controller的哪个方法来处理当前请求，异常处理。
`<context:component-scan/>`  
它的实现类是org.springframework.context.annotation.ComponentScanBeanDefinitionParser.
把鼠标放在context:component-scan上就可以知道有什么作用的，用来扫描该包内被@Repository @Service @Controller的注解类，然后注册到工厂中。并且context:component-scan激活@ required。@ resource,@ autowired、@PostConstruct @PreDestroy @PersistenceContext @PersistenceUnit。使得在适用该bean的时候用@Autowired就行了。

