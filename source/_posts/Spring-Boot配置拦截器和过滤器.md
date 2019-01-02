title: Spring Boot配置拦截器和过滤器
author: Asia Cui
tags:
  - Spring Boot
categories:
  - 配置
date: 2019-01-16 13:43:00
---
## 一、拦截器与过滤器

　　在讲 Spring boot 之前，我们先了解一下过滤器和拦截器。这两者在功能方面很类似，但是在具体技术实现方面，差距还是比较大的。在分析两者的区别之前，我们先理解一下 AOP 的概念，AOP 不是一种具体的技术，而是一种编程思想。在面向对象编程的过程中，我们很容易通过继承、多态来解决纵向扩展。 但是对于横向的功能，比如，在所有的 service 方法中开启事务，或者统一记录日志等功能，面向对象的是无法解决的。所以 AOP——面向切面编程其实是面向对象编程思想的一个补充。而我们今天讲的过滤器和拦截器都属于面向切面编程的具体实现。而两者的主要区别包括以下几个方面：

　　1、Filter 是依赖于 Servlet 容器，属于 Servlet 规范的一部分，而拦截器则是独立存在的，可以在任何情况下使用。

　　2、Filter 的执行由 Servlet 容器回调完成，而拦截器通常通过动态代理的方式来执行。

　　3、Filter 的生命周期由 Servlet 容器管理，而拦截器则可以通过 IoC 容器来管理，因此可以通过注入等方式来获取其他 Bean 的实例，因此使用会更方便。

## 二、过滤器的配置

　　现在我们通过过滤器来实现记录请求执行时间的功能，其实现如下：

```java
public class LogCostFilter implements Filter {
    @Override
   public void init(FilterConfig filterConfig) throws ServletException {

   }
 
	@Override
	public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, 				FilterChain filterChain) throws IOException, ServletException {
	
    		long start = System.currentTimeMillis();
    		filterChain.doFilter(servletRequest,servletResponse);
    		System.out.println("Execute cost="+(System.currentTimeMillis()-start));
	}
 
	@Override
	public void destroy() {
 
	}
}
```

　　这段代码的逻辑比较简单，就是在方法执行前先记录时间戳，然后通过过滤器链完成请求的执行，在返回结果之间计算执行的时间。这里需要主要，这个类必须继承 Filter 类，这个是 Servlet 的规范，这个跟以前的 Web 项目没区别。但是，有了过滤器类以后，以前的 web 项目可以在 web.xml 中进行配置，但是 spring boot 项目并没有 web.xml 这个文件，那怎么配置？在 Spring boot 中，我们需要 FilterRegistrationBean 来完成配置。其实现过程如下：

```java
@Configuration
public class FilterConfig {
 
    @Bean
    public FilterRegistrationBean registFilter() {
        FilterRegistrationBean registration = new FilterRegistrationBean();
        registration.setFilter(new LogCostFilter());
        registration.addUrlPatterns("/*");
        registration.setName("LogCostFilter");
        registration.setOrder(1);
        return registration;
    }
 
}
```



　　这样配置就完成了，需要配置的选项主要包括实例化 Filter 类，然后指定 url 的匹配模式，设置过滤器名称和执行顺序，这个过程和在 web.xml 中配置其实没什么区别，只是形式不同而已。现在我们可以启动服务器访问任意 URL：

![](https://images2017.cnblogs.com/blog/820406/201801/820406-20180129231150312-465693715.png)

　　大家可以看到上面的配置已经生效了。除了通过 FilterRegistrationBean 来配置以外，还有一种更直接的办法，直接通过注解就可以完成了：

```java
@WebFilter(urlPatterns = "/*", filterName = "logFilter2")
public class LogCostFilter2 implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
 
    }
 
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        long start = System.currentTimeMillis();
        filterChain.doFilter(servletRequest, servletResponse);
        System.out.println("LogFilter2 Execute cost=" + (System.currentTimeMillis() - start));
    }
 
    @Override
    public void destroy() {
 
    }
}
```

　　这里直接用 @WebFilter 就可以进行配置，同样，可以设置 url 匹配模式，过滤器名称等。这里需要注意一点的是 @WebFilter 这个注解是 Servlet3.0 的规范，并不是 Spring boot 提供的。除了这个注解以外，我们还需在配置类中加另外一个注解：@ServletComponetScan，指定扫描的包。

```java
@SpringBootApplication
@MapperScan("com.pandy.blog.dao")
@ServletComponentScan("com.pandy.blog.filters")
public class Application {
    public static void main(String[] args) throws Exception {
        SpringApplication.run(Application.class, args);
    }
}
```

　　现在，我们再来访问一下任意 URL：

![](https://images2017.cnblogs.com/blog/820406/201801/820406-20180129232130046-1905239306.png)

　　可以看到，我们配置的两个过滤器都生效了。细心的读者会发现，第二个 Filter 我们并没有指定执行的顺序，但是却在第一个 Filter 之前执行。这里需要解释一下，@WebFilter 这个注解并没有指定执行顺序的属性，其执行顺序依赖于 Filter 的名称，是根据 Filter 类名（注意不是配置的 filter 的名字）的字母顺序倒序排列，并且 @WebFilter 指定的过滤器优先级都高于 FilterRegistrationBean 配置的过滤器。有兴趣的朋友可以自己实验一下。

## 三、拦截器的配置

 　　上面我们已经介绍了过滤器的配置方法，接下来我们再来看看如何配置一个拦截器。我们使用拦截器来实现上面同样的功能，记录请求的执行时间。首先我们实现拦截器类：

```java
public class LogCostInterceptor implements HandlerInterceptor {
    long start = System.currentTimeMillis();
    @Override
    public boolean preHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o) throws Exception {
        start = System.currentTimeMillis();
        return true;
    }
 
    @Override
    public void postHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, ModelAndView modelAndView) throws Exception {
        System.out.println("Interceptor cost="+(System.currentTimeMillis()-start));
    }
 
    @Override
    public void afterCompletion(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) throws Exception {
    }
}
```

　　这里我们需要实现 HandlerInterceptor 这个接口，这个接口包括三个方法，preHandle 是请求执行前执行的，postHandler 是请求结束执行的，但只有 preHandle 方法返回 true 的时候才会执行，afterCompletion 是视图渲染完成后才执行，同样需要 preHandle 返回 true，该方法通常用于清理资源等工作。除了实现上面的接口外，我们还需对其进行配置：

```java
@Configuration
public class InterceptorConfig extends WebMvcConfigurerAdapter {
 
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LogCostInterceptor()).addPathPatterns("/**");
        super.addInterceptors(registry);
    }
}
```

　　这里我们继承了 WebMVCConfigurerAdapter，看过前面的文章的朋友应该已经见过这个类了，在进行静态资源目录配置的时候我们用到过这个类。这里我们重写了 addInterceptors 这个方法，进行拦截器的配置，主要配置项就两个，一个是指定拦截器，第二个是指定拦截的 URL。现在我们再启动系统访问任意一个 URL：

![](https://images2017.cnblogs.com/blog/820406/201801/820406-20180130003851859-2140257073.png)

 　　可以看到，我们通过拦截器实现了同样的功能。不过这里还要说明一点的是，其实这个实现是有问题的，因为 preHandle 和 postHandle 是两个方法，所以我们这里不得不设置一个共享变量 start 来存储开始值，但是这样就会存在线程安全问题。当然，我们可以通过其他方法来解决，比如通过 ThreadLocal 就可以很好的解决这个问题，有兴趣的同学可以自己实现。不过通过这一点我们其实可以看到，虽然拦截器在很多场景下优于过滤器，但是在这种场景下，过滤器比拦截器实现起来更简单。
