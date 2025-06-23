# **四、Java应用篇详解**

## **1. 企业级开发**

### **1.1 Spring框架核心**

#### **1.1.1 IoC容器深度解析**
**XML配置方式**：
```xml
<!-- applicationContext.xml -->
<bean id="userService" class="com.example.UserServiceImpl">
    <property name="userDao" ref="userDao"/>
</bean>

<bean id="userDao" class="com.example.UserDaoImpl"/>
```

**注解方式**：
```java
@Service
public class UserServiceImpl {
    @Autowired  // 自动装配
    private UserDao userDao;
}

@Repository
public class UserDaoImpl implements UserDao {}
```

**生命周期回调**：
```java
public class BeanLifecycle implements InitializingBean, DisposableBean {
    @PostConstruct
    public void init() { /* 初始化逻辑 */ }

    @PreDestroy
    public void cleanup() { /* 销毁逻辑 */ }
}
```

**高级特性**：
- `@Conditional`：条件化Bean注册
- `@Profile`：环境特定配置
- `BeanPostProcessor`：Bean初始化前后处理

---

#### **1.1.2 AOP实战**
**切面定义**：
```java
@Aspect
@Component
public class LoggingAspect {
    @Pointcut("execution(* com.example.service.*.*(..))")
    public void serviceLayer() {}

    @Around("serviceLayer()")
    public Object logExecutionTime(ProceedingJoinPoint pjp) throws Throwable {
        long start = System.currentTimeMillis();
        Object result = pjp.proceed();
        System.out.println("方法执行耗时: " + (System.currentTimeMillis() - start) + "ms");
        return result;
    }
}
```

**通知类型对比**：
| 注解 | 执行时机 | 典型应用 |
|------|----------|----------|
| `@Before` | 方法执行前 | 权限校验 |
| `@AfterReturning` | 方法成功返回后 | 日志记录 |
| `@AfterThrowing` | 方法抛出异常时 | 异常监控 |
| `@Around` | 包裹目标方法 | 性能监控 |

**代理机制**：
- JDK动态代理（接口代理）
- CGLIB（类代理，`proxyTargetClass=true`）

---

### **1.2 Spring Boot进阶**

#### **1.2.1 自动配置原理**
**启动流程**：
1. `@SpringBootApplication`组合了：
   - `@SpringBootConfiguration`（配置类标识）
   - `@EnableAutoConfiguration`（自动配置入口）
   - `@ComponentScan`（组件扫描）

2. `spring.factories`定义自动配置类：
```properties
# META-INF/spring.factories
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
  com.example.MyAutoConfiguration
```

**自定义Starter**：
1. 创建`autoconfigure`模块
2. 编写`@Configuration`类
3. 添加`spring.factories`配置

---

#### **1.2.2 微服务实践**
**Spring Cloud核心组件**：
```java
// 服务注册与发现
@EnableEurekaClient
@SpringBootApplication
public class Application { /*...*/ }

// 声明式REST客户端
@FeignClient(name = "user-service")
public interface UserClient {
    @GetMapping("/users/{id}")
    User getUser(@PathVariable Long id);
}

// 熔断降级
@HystrixCommand(fallbackMethod = "defaultUser")
public User getUser(Long id) { /*...*/ }
```

**配置中心示例**：
```yaml
# bootstrap.yml
spring:
  cloud:
    config:
      uri: http://config-server:8888
      name: user-service
```

---

## **2. 项目实战**

### **2.1 学生管理系统（控制台版）**
**核心设计**：
```java
public class Student {
    private String id;
    private String name;
    private int score;
    // getters/setters
}

public interface StudentService {
    void addStudent(Student s);
    List<Student> getAllStudents();
}

@Service
public class StudentServiceImpl implements StudentService {
    private final Map<String, Student> store = new ConcurrentHashMap<>();
    
    @Override
    public void addStudent(Student s) {
        store.put(s.getId(), s);
    }
}
```

**扩展功能**：
- 数据持久化（JSON文件存储）
- 成绩统计分析（Stream API）
- 导出Excel（Apache POI）

---

### **2.2 Web项目实战**

#### **2.2.1 Servlet/JSP项目**
**MVC架构**：
```java
@WebServlet("/user")
public class UserServlet extends HttpServlet {
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) {
        List<User> users = userService.getAllUsers();
        req.setAttribute("users", users);
        req.getRequestDispatcher("/WEB-INF/views/users.jsp").forward(req, resp);
    }
}
```

**JSP页面**：
```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<table>
  <c:forEach items="${users}" var="user">
    <tr><td>${user.name}</td></tr>
  </c:forEach>
</table>
```

---

#### **2.2.2 RESTful API设计**
**Spring Boot实现**：
```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    @GetMapping
    public ResponseEntity<List<User>> listUsers(
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "10") int size
    ) {
        Page<User> users = userService.getUsers(page, size);
        return ResponseEntity.ok()
            .header("X-Total-Count", String.valueOf(users.getTotalElements()))
            .body(users.getContent());
    }
}
```

**API文档生成**：
```java
@Bean
public Docket api() {
    return new Docket(DocumentationType.SWAGGER_2)
        .select()
        .apis(RequestHandlerSelectors.basePackage("com.example"))
        .paths(PathSelectors.any())
        .build();
}
```

---

### **2.3 分布式系统实战**

#### **2.3.1 Spring Cloud Alibaba**
```java
// 分布式配置
@RefreshScope
@RestController
public class ConfigController {
    @Value("${config.info}")
    private String configInfo;
}

// 分布式事务
@GlobalTransactional
public void placeOrder(Order order) {
    orderDao.create(order);
    accountService.debit(order.getUserId(), order.getMoney());
}
```

#### **2.3.2 Docker部署**
```dockerfile
FROM openjdk:11-jre
COPY target/app.jar /app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
```

**编排部署**：
```yaml
# docker-compose.yml
services:
  app:
    image: my-app:latest
    ports:
      - "8080:8080"
    depends_on:
      - mysql
  mysql:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: root
```

---

## **3. 工具与扩展**

### **3.1 构建工具对比**

| 特性       | Maven                          | Gradle                     |
|------------|--------------------------------|----------------------------|
| 构建脚本   | XML（pom.xml）                 | Groovy/Kotlin（build.gradle） |
| 依赖管理   | `<dependencies>`               | `dependencies {}`          |
| 构建速度   | 较慢（线性执行）               | 快（增量构建）             |
| 多模块支持 | `<modules>`                    | `include()`                |

**Gradle示例**：
```groovy
plugins {
    id 'java'
    id 'org.springframework.boot' version '2.5.4'
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.7.0'
}
```

---

### **3.2 Git高级用法**
**分支策略**：
```bash
# 功能开发
git checkout -b feature/login
git commit -m "实现登录功能"
git push origin feature/login

# 代码审查后合并
git checkout develop
git merge --no-ff feature/login
```

**撤销操作**：
```bash
git reset --soft HEAD~1  # 撤销commit但保留更改
git checkout -- file.txt # 丢弃工作区修改
```

---

### **3.3 设计模式实践**

#### **3.3.1 Spring中的设计模式**
- **单例模式**：`@Bean`默认单例
- **工厂模式**：`BeanFactory`
- **代理模式**：AOP动态代理
- **模板方法**：`JdbcTemplate`

#### **3.3.2 观察者模式示例**
```java
// 事件定义
public class OrderEvent extends ApplicationEvent {
    public OrderEvent(Order source) {
        super(source);
    }
}

// 监听器
@Component
public class OrderListener implements ApplicationListener<OrderEvent> {
    @Override
    public void onApplicationEvent(OrderEvent event) {
        // 处理订单事件
    }
}

// 发布事件
applicationContext.publishEvent(new OrderEvent(order));
```

---

## **总结对比表**
| 领域         | 技术栈                          | 企业级应用场景         |
|--------------|---------------------------------|-----------------------|
| Web开发      | Spring MVC + Thymeleaf          | 后台管理系统         |
| 微服务       | Spring Cloud + Docker           | 电商平台             |
| 持续集成     | Jenkins + GitLab CI             | 自动化测试部署       |

**推荐学习路径**：
1. 完成Spring Boot + MyBatis商品管理系统
2. 基于Spring Cloud Alibaba搭建微服务架构
3. 通过Kubernetes实现生产级部署