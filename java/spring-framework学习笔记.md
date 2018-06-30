> 最新进度 

**7.8 Container Extension Points之前**

![spring](http://39.108.180.232/group1/M00/00/00/rBL721q2EwaAASVoAABw2WPb4DM063.png)

> 2017-10-23 21:39

**spring framework半月学习计划** 
**[官方文档](https://docs.spring.io/spring/docs/4.3.12.RELEASE/spring-framework-reference/htmlsingle)&nbsp;**(版本:4.3.12.RELEASE)

<!--more-->

> 2017-10-24 17:15 *(7.3.2 Instantiating beans之前)*

- **Ⅰ. *OverView of Spring Framework (1-2)***
- **Ⅱ. *What's New in Spring Framework 4.x (3-6)***
- **Ⅲ. *Core Technologies (7-12)***
- **Ⅳ. *Testing (13-16)***
- **V. *Data Access (17-21)***
- **Ⅵ. *The Web (22-27)***
- **Ⅶ. *Integration (28-36)***
- **Ⅷ. *Appendices (37-44)***

> 2017-10-25 18:43 *(7.4.2 Dependencies and configuration in detail之前)*

**DI(Dependency injection) exists in two major variants, `Constructor-based` dependency injection and `Setter-based` dependency injection.**

**主要是讲两种依赖注入方式,通过构造方法传递依赖和通过setter方法传递依赖.官方推荐前者也说明其代码美观度不友好且不太灵活.实际项目比如嫦娥是用的后者,我习惯于使用后者.**

**文档主要使用xml配置来讲述,很清晰,实际项目还是使用`@Annotation`多一点.**

> 2017-10-28 11:23 *(7.4.5 Autowiring collaborators之前)*

**ref:一个bean直接依赖另一个bean,即被依赖bean是依赖bean的属性,被依赖bean先加载再赋值给依赖bean.**

**depends-on:一个bean间接依赖另一个bean,比如被依赖的bean是静态操作,如数据库配置相关,这时,使用depends-on强制实例化被依赖bean.**

> 2017-10-30 18:53 *(7.5.5 Custom scopes之前)*

**As a rule, use the `prototype scope` for all stateful beans and the `singleton scope` for stateless beans.**

**In the case of prototypes, configured destruction lifecycle callbacks are not called.**

**In some respects, the Spring container’s role in regard to a prototype-scoped bean is a replacement for the Java `new` operator.**

> 2017-11-1 19:33 *(7.6.2 ApplicationContextAware and BeanNameAware之前)*

**The Spring container guarantees that a configured initialization callback is called immediately after a bean is supplied with all dependencies.**

**Thus the initialization callback is called on the raw bean reference, which means that AOP interceptors and so forth are not yet applied to the bean. A target bean is fully created first, then an AOP proxy (for example) with its interceptor chain is applied.**

**意思是说原生bean被创建出来后(包括`init`方法(如果有的话)),AOP切面才会被应用到该bean.**

`If a "depends-on" relationship exists between any two objects, the dependent side will start after its dependency, and it will stop before its dependency.`

**就是说如果A依赖B,当然要B先存在,A才能存在,否则会报错.那么B先start(),然后A再start().最后A执行stop(),B再stop().**

> 2017-11-4 11:15 *(7.8 Container Extension Points之前)*

**bean的继承**

**Similarly, the container’s internal `preInstantiateSingletons()` method ignores bean definitions that are defined as abstract.**