### Spring
#### 声明bean的注解
- @Component 组件，没有明确的角色
- @Service 在业务逻辑层使用（service层）
- @Repository 在数据访问层使用（dao层）
- @Controller 在展现层使用，控制器的声明（C）

#### 注入bean的注解
- @Autowired：由Spring提供
- @Inject：由JSR-330提供
- @Resource：由JSR-250提供

#### java配置类相关注解
- @Configuration 声明当前类为配置类，相当于xml形式的Spring配置（类上）
- @Bean 注解在方法上，声明当前方法的返回值为一个bean，替代xml中的方式（方法上）
- @Configuration 声明当前类为配置类，其中内部组合了@Component注解，表明这个类是一个bean（类上）
- @ComponentScan 用于对Component进行扫描，相当于xml中的（类上）
- @WishlyConfiguration 为@Configuration与@ComponentScan的组合注解，可以替代这两个注解

#### 切面（AOP）相关注解
- @Aspect 声明一个切面（类上）
  - 使用@After、@Before、@Around定义建言（advice），可直接将拦截规则（切点）作为参数
- @After 在方法执行之后执行（方法上）
- @Before 在方法执行之前执行（方法上）
- @Around 在方法执行之前与之后执行（方法上）
- @PointCut 声明切点
  - 在java配置类中使用@EnableAspectJAutoProxy注解开启Spring对AspectJ代理的支持（类上）
  
#### @Bean的属性支持
- @Scope 设置Spring容器如何新建Bean实例 (方法上，得有@Bean)，其设置类型包括：
  - Singleton (单例，一个Spring容器中只有一个bean实例，默认模式)
  - Protetype (每次调用新建一个bean)
  - Request (web项目中，给每个 http request 新建一个 bean)
  - Session (web项目中，给每个 http session 新建一个 bean)
  - GlobalSession (给每一个 global http session 新建一个 Bean 实例)
  - @StepScope 在Spring Batch中还有涉及
  - @PostConstruct 由JSR-250提供，在构造函数执行完之后执行，等价于xml配置文件中 bean 的 initMethod
  - @PreDestory 由JSR-250提供，在Bean销毁之前执行，等价于xml配置文件中 bean 的 destroyMethod
  
#### @Value注解
- 注入普通字符 
```java
@Value("Micheal Jackson")
String name;
```
- 注入操作系统属性
```java
@Value("#{systemProperties['os.name']}")
String osName;
```
- 注入表达式结果
```java
@Value("#{ T(java.lang.Math).random * 100 }")
String randomNumber;
```
- 注入其它bean属性
```java
@Value("#{domClass.name}")
String name;
```
- 注入文件资源
```java
@Value("classpath:com/hgs/hello/test.txt")
String name;
```
- 注入网站资源
```java
@Value("http://www.baidu.com")
String name;
```
- 注入配置文件
```java
@Value("${book.name}")
String name;
```

#### 注入配置使用方法
- 编写配置文件（test.properties）
- @PropertySource 加载配置文件(类上)
- 还需配置一个PropertySourcesPlaceholderConfigurer的bean

#### 环境切换
- @Profile 通过设定Environment的ActiveProfiles来设定当前context需要使用的配置环境
- @Conditional Spring4中可以使用此注解定义条件话的bean，通过实现Condition接口，并重写matches方法，从而决定该bean是否被实例化

#### 异步相关
- @EnableAsync 配置类中，通过此注解开启对异步任务的支持，叙事性AsyncConfigurer接口（类上）
- @Async 在实际执行的bean方法使用该注解来申明其是一个异步任务（方法上或类上所有的方法都将异步，需要@EnableAsync开启异步任务）

#### 定时任务相关
- @EnableScheduling 在配置类上使用，开启计划任务的支持（类上）
- @Scheduled 来申明这是一个任务，包括cron,fixDelay,fixRate等类型（方法上，需先开启计划任务的支持）

#### @Enable* 注解说明
- @EnableAspectJAutoProxy 开启对AspectJ自动代理的支持
- @EnableAsync 开启异步方法的支持
- @EnableScheduling 开启计划任务的支持
- @EnableWebMvc 开启Web MVC的配置支持
- @EnableConfigurationProperties 开启对@ConfigurationProperties注解配置Bean的支持
- @EnableJpaRepositories 开启对SpringData JPA Repository的支持
- @EnableTransactionManagement 开启注解式事务的支持
- @EnableTransactionManagement 开启注解式事务的支持
- @EnableCaching 开启注解式的缓存支持

#### 测试相关注解
- @RunWith 运行器，Spring中通常用于对JUnit的支持
```java
@RunWith(SpringJunit4ClassRunner.class)
```
- @ContextConfiguration 用来加载配置ApplicationContext，其中classes属性用来加载配置类
```java
@ContextConfiguration(classes={TestConfig.class})
```

#### SpringMVC部分
- @EnableWebMvc 在配置类中开启 Web MVC 的配置支持，如一些 ViewResolver 或者 MessageConverter 等，若无此句，重写 WebMvcConfigurerAdapter 方法（用于对SpringMVC的配置）
- @Controller 声明该类为 SpringMVC 中的 Controller
- @RequestMapping 用于映射 Web 请求，包括访问路径和参数（类或方法上）
- @ResponseBody 支持将返回值放在 response 内，而不是一个页面，通常用户返回json数据（返回值旁或方法上）
- @RequestBody 允许 request 的参数在 request 体中，而不是在直接连接在地址后面。（放在参数前）
- @PathVariable 用于接收路径参数，比如 @RequestMapping("/hello/{name}")申明的路径，将注解放在参数中前，即可获取该值，通常作为 Restful 的接口实现方法
- @RestController 该注解为一个组合注解，相当于 @Controller 和 @ResponseBody 的组合，注解在类上，意味着，该 Controller 的所有方法都默认加上了 @ResponseBody
- @ControllerAdvice 通过该注解，我们可以将对于控制器的全局配置放置在同一个位置，注解了 @Controller 的类的方法可使用 @ExceptionHandler、@InitBinder、@ModelAttribute 注解到方法上，这对所有注解了 @RequestMapping的控制器内的方法有效
- @ExceptionHandler 用于全局处理控制器里的异常
- @InitBinder 用来设置 WebDataBinder，WebDataBinder 用来自动绑定前台请求参数到Model中
- @ModelAttribute 本来的作用是绑定键值对到Model里，在 @ControllerAdvice 中是让全局的 @RequestMapping 都能获得在此处设置的键值对


