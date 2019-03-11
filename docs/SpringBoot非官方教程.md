# SpringBoot非官方教程 知识点汇总

## 启动SpringBoot 方式
1. cd到项目主目录
2.
    ```
    mvn clean  
    mvn package  编译项目的jar
    ```
3. 方式1：mvn spring-boot: run 启动  
方式2：cd 到target目录，java -jar 项目.jar

## 注解
### @Bean

## 配置文件
@ConfigurationProperties 注解支持两种方式.

* 方式一：
加在类上,需要和@Component注解,结合使用.代码如下:
    ```Java
    @EnableConfigurationProperties({ConfigBean.class})
    ```

    ```Java
    @ConfigurationProperties(prefix = "my")
    @Component
        public class People {

        private String name;

        private Integer age;

        private List<String> address;

        private Phone phone;
    }   

    ```
* 方式二：
通过@Bean的方式进行声明,这里我们加在启动类即可,代码如下:
    @Bean
    @ConfigurationProperties(prefix = "my")
    public ConfigBean configBean(){
        return new ConfigBean();
    }

## 自定义配置文件
有时我们不愿意把配置都写到application配置文件中，这时需要我们自定义配置文件，比如test.properties
``` test.properties
com.wangxu.name=wangxu2
com.wangxu.age=222
```
``` CustomConfigBean.java
@Configuration
//@PropertySource("classpath:test.properties")无法加载yaml文件
@PropertySource("classpath:test.properties")
@ConfigurationProperties(prefix = "myprops")

public class CustomConfigBean {

    private String name;
    private int age;
    private int number;
    private String uuid;
    private int max;
    private String value;
    private String greeting;

    ……Getter Setter
}

```

## 单元测试
通过@RunWith() @SpringBootTest开启注解：
```Java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = SpringBootLearnApplication.class, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class HelloControllerTest {

    @LocalServerPort
    private int port;

    private URL base;

    @Autowired
    private TestRestTemplate template;

@Before
    public void setUp() throws MalformedURLException {
        this.base = new URL("http://localhost:" + port + "/");
    }

    @Test
    public void getHello(){
        ResponseEntity<String> response = template.getForEntity(base.toString(),String.class);

        System.out.println(response.getBody());
        Assert.assertEquals(response.getBody(),"Greetings from Spring Boot!");
    }
}
```
其中，classes属性指定启动类，SpringBootTest.WebEnvironment.RANDOM_PORT经常和测试类中@LocalServerPort一起在注入属性时使用。会随机生成一个端口号。

### Junit基本注解介绍

@BeforeClass 在所有测试方法前执行一次，一般在其中写上整体初始化的代码

@AfterClass 在所有测试方法后执行一次，一般在其中写上销毁和释放资源的代码

@Before 在每个测试方法前执行，一般用来初始化方法（比如我们在测试别的方法时，类中与其他测试方法共享的值已经被改变，为了保证测试结果的有效性，我们会在@Before注解的方法中重置数据）

@After 在每个测试方法后执行，在方法执行完成后要做的事情

@Test(timeout = 1000) 测试方法执行超过1000毫秒后算超时，测试将失败

@Test(expected = Exception.class) 测试方法期望得到的异常类，如果方法执行没有抛出指定的异常，则测试失败

@Ignore(“not ready yet”) 执行测试时将忽略掉此方法，如果用于修饰类，则忽略整个类

@Test 编写一般测试用例

@RunWith 在JUnit中有很多个Runner，他们负责调用你的测试代码，每一个Runner都有各自的特殊功能，你要根据需要选择不同的Runner来运行你的测试代码。

## JDBC
1. 引入依赖
    ``` pom.xml
    <!--JDBC-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jdbc</artifactId>
    </dependency>
    <!--MySQL-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>runtime</scope>
    </dependency>
    <!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.1.14</version>
    </dependency>
    ```
2. 在application.yml文件配置mysql的驱动类，数据库地址，数据库账号、密码信息。
    ```application.yml
    spring:
    datasource:
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://localhost:3306/test
        username: root
        password: root
    ```
3. 写代码
    * 实体类            -entity
    * DAO层及实现       -dao dao.impl
    * Service层及实现   -service service.impl
    * controller层      -controller

## SpringDataJPA
1. 引入依赖
2. 在application.yml文件配置mysql的驱动类，数据库地址，数据库账号、密码信息。
    ```application.yml
    spring:
        datasource:
            driver-class-name: com.mysql.cj.jdbc.Driver
            url: jdbc:mysql://localhost:3306/test
            username: root
            password: root
          jpa:
            hibernate:
                ddl-auto: update  # 第一次简表create  后面用update
            show-sql: true
    ```
3. 写代码
    * 实体类            -entity @Entity
    * Repository层      -repository @Repository
    * Service层         -service @Service
    * controller层      -controller @Controller