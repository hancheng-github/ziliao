## 原理初探

![image-20200422182218812](C:\Users\韩宝宝\AppData\Roaming\Typora\typora-user-images\image-20200422182218812.png)

**pom.xml**

​     spring-boot-dependencies：核心依赖在父工程
​     我们再写或者引入spring依赖的时候，不需要版本，因为有这些版本仓库

**启动器：**     

```xml
	<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
	</dependency>
```

​     启动器：就是SpringBoot的启动场景
​          比如spring-boot-starter-web，它就导入web环境的所有依赖

​     springboot会将所有的功能场景，都变成一个个的启动器
​         要使用上面功能，只需要找到对应的启动器就可以了starter

### 自动装配：

![image-20200423184400850](C:\Users\韩宝宝\AppData\Roaming\Typora\typora-user-images\image-20200423184400850.png)

- @ConfigurationProperties

  `将配置文件中配置的每一个值，映射到这个组件中，告诉springboot将本类中所有属性和配置文件中相关的配置进行绑定`

  ![image-20200423190317787](C:\Users\韩宝宝\AppData\Roaming\Typora\typora-user-images\image-20200423190317787.png)

  ![image-20200423190356307](C:\Users\韩宝宝\AppData\Roaming\Typora\typora-user-images\image-20200423190356307.png)

  第二个图片会为Dog里的属性赋值

#### 自动装配的精髓：

1. SpringBoot启动会加载大量的自动配置类

2. 我们看我们需要的功能在不在SpringBoot默认写好的自动配置类当中

3. 我们在看这个自动配置类中到底配置了哪些注解；（子要我们要用的组件存在其中，就不需要手动配置了）

4. 给容器中自动配置类添加组件时，会从properties类中获取某些属性，我们只需要在配置文件中指定这些属性的值即可；

   xxxAutoConfiguration：自动配置类；给容器中添加组件

   xxxProperties：封装配置文件中相关属性；**可以使用application.yaml文件配置**

### 主程序：

- 注解：

- ```java
  @SpringBootApplication：标记这个类是一个springboot的应用
  	@SpringBootConfiguration：springboot的配置
  		@Configuration：表示是一个配置类
  			@Component：表示是一个组件
  	@EnableAutoConfiguration：自动导入配置
      	@AutoConfigurationPackage：自动配置包
          	@Import(AutoConfigurationPackage.Register.class)：导入自动配置包注册
      	@Import(AutoConfigurationImportSelector.class)：导入自动配置选择器
  			List<String> configurations=getCandidateConfigurations(annotationMetadata,attributes);  获取所有的配置
  ```

  获取候选的配置

  ```Java
  protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
     List<String> configurations = SpringFactoriesLoader.loadFactoryNames(getSpringFactoriesLoaderFactoryClass(),
           getBeanClassLoader());
     Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you "
           + "are using a custom packaging, make sure that file is correct.");
     return configurations;
  }
  ```

  META-INF/spring.factories：自动配置的核心文件

  ![image-20200422184122704](C:\Users\韩宝宝\AppData\Roaming\Typora\typora-user-images\image-20200422184122704.png)  

  ```java
  Properties properties = PropertiesLoaderUtils.loadProperties(resource);
  所有资源加载到配置类中！
  ```

  结论：springboot所有自动装配都是在启动的时候扫描并加载：spring.factories所有的自动配置类都在这里面，但不一定生效，要判断条件是否成立，只要导入对应的start，就有对应的启动器，有了启动器，自动装配就会生效，然后就配置成功。

  

  1. springboot在启动的时候，从类路径下/META-INF/spring.factories获取指定的值；
  
  2. 将这些自动配置的类导入容器，自动配置机会生效，帮我们进行自动配置
  
  3. 整个JavaEE，解决方案和自动配置的东西都在spring-boot-autoconfigure-2.2.6.RELEAE.jar这个报包下
  
  4. 它会把所有需要导入的组件，一类名的方式返回，着些组件就会被添加到容器
  
  5. 容器中会存在很多的XxxAutoConfiguration的文件（@Bean），就是这些类给容器中导入了这个场景需要的所有组件
  
     

### 启动方法：

```Java
SpringApplication.run(Springboot01Application.class, args);
```

- SpringApplication：

  这个类主要做了一下四件事情

  1. 推断应用的类型是普通的项目还是web项目
  2. 查找并加载所有可用初始化器
  3. 找出所有应用程序监听器，设置到listeners属性中
  4. 推断并设置main方法的定义类，找到运行的主类



### 注解：

| @ConfigurationProperties(prefix="唯一标识")   | `将配置文件中配置的每一个值，映射到这个组件中，告诉springboot将本类中所有属性和配置文件中相关的配置进行绑定` |
| :-------------------------------------------- | ------------------------------------------------------------ |
| **@PropertySource(value="classpath:文件名")** | `加载指定的配置文件`                                         |
| @Validated                                    | `（开启JSR303）数据校验`                                     |
| **@ConfigurationProperties**                  | `表示这个类是一个配置类`                                     |
| @EnableConfigurationProperties                | `自动配置属性`                                               |
| @ConditionalOnWebApplication                  | `spring的底层注解，根据不同的条件，来判断当前配置或者类是否生效` |
| @xxxAutoConfiguration                         | `自动配置类；给容器中添加组件`                               |
| xxxProperties                                 | `封装配置文件中相关属性；可以使用application.yaml文件配置`   |
|                                               |                                                              |
|                                               |                                                              |

### JSR303校验：

- cp只需要写一次即可，value则需要每个字段都添加

- 松散绑定：比如last-name=lastName，-后面跟着的字母默认是大写的，这就是松散绑定

- JSR303数据校验：在字段上增加一层过滤器验证，可以保证数据的合法性

- 复杂类型封装，yaml中可以封装对象，使用@Value就不支持

  ![image-20200423175210749](C:\Users\韩宝宝\AppData\Roaming\Typora\typora-user-images\image-20200423175210749.png)



### 多环境配置：

- 两个---之间可以理解为一个配置文件

  ```yaml
  #springboot的多环境配置：可以选择激活哪一个配置文件
  spring:
    profiles:
      active: test
  
  ---
  server:
    port: 8080
  ---
  server:
    port: 8081
  spring:
    profiles: dev
  ---
  server:
    port: 8082
  spring:
    profiles: test
  ```



## Web开发

要解决的问题

### 导入静态资源

```java
public void addResourceHandlers(ResourceHandlerRegistry registry) {
    //判断resourceProperties是否实现了自定义
   if (!this.resourceProperties.isAddMappings()) {
      logger.debug("Default resource handling disabled");
      return;
   }
   Duration cachePeriod = this.resourceProperties.getCache().getPeriod();
   CacheControl cacheControl = this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl();
    //是否有webjars文件
   if (!registry.hasMappingForPattern("/webjars/**")) {
       //寻找webjars下的所有文件
      customizeResourceHandlerRegistration(registry.addResourceHandler("/webjars/**")
            .addResourceLocations("classpath:/META-INF/resources/webjars/")
            .setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
   }
    //获得静态资源的路径
   String staticPathPattern = this.mvcProperties.getStaticPathPattern();
   if (!registry.hasMappingForPattern(staticPathPattern)) {
      customizeResourceHandlerRegistration(registry.addResourceHandler(staticPathPattern)
            .addResourceLocations(getResourceLocations(this.resourceProperties.getStaticLocations()))
            .setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
   }
}
```

#### 默认可以存放静态资源的地址：

| "classpath:/META-INF/resources/" | 优先级依次降低 |
| -------------------------------- | -------------- |
| "classpath:/resources/"          |                |
| "classpath:/static/"             |                |
| "classpath:/public/"             |                |

#### 自定义静态资源地址：

```yaml
spring:
  mvc:
    static-path-pattern: /xxx/xxx
```

### 首页

- 在静态资源目录中添加index.html文件，springboot会自动把它变成首页

  ![image-20200425160323179](C:\Users\韩宝宝\AppData\Roaming\Typora\typora-user-images\image-20200425160323179.png)

- 在templates目录下的所有页面，只能通过controller来跳转

### jsp，模板引擎Thymeleaf

- 引入模板引擎

  ```xml
  <dependency>
      <groupId>org.thymeleaf</groupId>
      <artifactId>thymeleaf-spring5</artifactId>
  </dependency>
  <dependency>
      <groupId>org.thymeleaf</groupId>
      <artifactId>thymeleaf-extras-java8time</artifactId>
  </dependency>
  ```

  **导入完成后，只需要把html文件放在templates下面即可**

- Thymeleaf表达式

  ```html
  <div th:text=""></div>
  ```

  所有的html标签都可以被Thymeleaf替换接管

#### Thymeleaf基础语法

| <div th:text="${msg}"></div>                        | 取值但是不转义 |
| --------------------------------------------------- | -------------- |
| <div th:utext="${msg}"></div>                       | 转义           |
| <h3 th:each="list:${lists}" th:text="${list}"></h3> | 取出一个数组   |
| @{...}                                              | 链接时使用     |
| #{...}                                              | 国际化         |
| ${...}                                              | 参数           |

- 使用Thymeleaf表达式取出Controller中存如的元素

  ![image-20200425164625445](C:\Users\韩宝宝\AppData\Roaming\Typora\typora-user-images\image-20200425164625445.png)

  ![image-20200425164606391](C:\Users\韩宝宝\AppData\Roaming\Typora\typora-user-images\image-20200425164606391.png)

### 装配扩展SpringMVC

1. 创建一个类添加注解@Configuration（不可以添加@EnableWebMvc注解）
2. 实现WebMvcConfigurer接口
3. 重写方法

自定义日期格式：

```yaml
spring:
  mvc:
    date-format: 
```

- 在springboot中，有非常多的xxxConfiguration帮助我们进行扩展配置

### 增删改查

#### 1.提取公共页面

1. ```html
   th:fragment="sidebar"
   ```

2. ```html
   <div th:replace="~{commons/commons::sidebar}"></div>
   ```

3. 如果要传递参数，可以直接使用()传参

   ```html
   <div th:insert="~{commons/commons::sidebar(active='main.html')}"></div>
   ```

#### 2.列表循环展示

1. 把用户列表放到Model种

   ```java
   @RequestMapping("/allEmp")
   public String getAllEmployee(Model model){
       Collection<Employee> employees = employeeDao.getEmployees();
       model.addAttribute("emps",employees);
       return "/emp/list";
   }
   ```

2. 使用Thymeleaf提取数据

   ```html
   <table class="table table-striped table-sm">
      <thead>
         <tr>
            <th>id</th>
            <th>lastName</th>
            <th>email</th>
            <th>gender</th>
            <th>department</th>
            <th>birth</th>
            <th>操作</th>
         </tr>
      </thead>
      <tbody>
         <tr th:each="emp:${emps}">
            <td th:text="${emp.getId()}"></td>
            <td th:text="${emp.getLastName()}"></td>
            <td th:text="${emp.getEmail()}"></td>
            <td th:text="${emp.getGender()==0 ? '女' : '男'}"></td>
            <td th:text="${emp.getDepartment().getDepartmentName()}"></td>
            <td th:text="${#dates.format(emp.getBirth(),'yyyy-mm-dd hh:mm:ss')}"></td>
            <td>
               <a class="btn btn-sm btn-primary" th:href="@{/emp/update/}+${emp.getId()}">编辑</a>
               <a class="btn btn-sm btn-danger" th:href="@{/emp/delemp/}+${emp.getId()}">删除</a>
            </td>
         </tr>
      </tbody>
   </table>
   ```

#### 3.修改用户数据

1. 调用Controller方法，并把需要修改的用户的id传过去

   ```html
   <a class="btn btn-sm btn-primary" th:href="@{/emp/update/}+${emp.getId()}">编辑</a>
   ```

2. 执行Controller方法

   ```java
   //员工修改页面
   @GetMapping("/update/{id}")
   public String toUpdateEmp(@PathVariable("id") Integer id, Model model){
       //根据id查出来员工
       Employee employee = employeeDao.getEmployee(id);
       //将员工信息返回页面
       model.addAttribute("emp",employee);
       model.addAttribute("dept",employeeDao.getEmployee(id).getDepartment());
       //查出所有的部门，提供修改选择
       Collection<Department> departments = departmentDao.getDeparments();
       model.addAttribute("departments",departments);
   
       return "/emp/update";
   }
   ```

3. 前端修改用户信息

   ```html
   <h2>修改员工信息</h2>
   <form th:action="@{/emp/updateEmp}" method="post" >
      <input name="id" type="hidden" class="form-control" th:value="${emp.getId()}">
      <div class="form-group">
         <label>姓名</label>
         <input name="lastName" type="text" class="form-control " th:value="${emp.getLastName()}" >
      </div>
      <div class="form-group">
         <label>邮箱</label>
         <input name="email" type="email" class="form-control" th:value="${emp.getEmail()}">
      </div>
      <div class="form-group">
         <label>性别</label><br/>
         <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" name="gender" value="1"
                  th:checked="${emp.getGender()==1}">
            <label class="form-check-label">男</label>
         </div>
         <div class="form-check form-check-inline">
            <input class="form-check-input" type="radio" name="gender" value="0"
                  th:checked="${emp.getGender()==0}">
            <label class="form-check-label">女</label>
         </div>
      </div>
      <div class="form-group">
         <label>部门</label>
         <!--提交的是部门的ID-->
         <select class="form-control" name="department.id">
            <option th:selected="${dept.id == emp.department.id}" th:each="dept:${departments}"
                  th:text="${dept.departmentName}" th:value="${dept.id}">1
            </option>
         </select>
      </div>
      <div class="form-group">
         <label>时间</label>
         <input name="date" type="text"  class="form-control" th:value="${#dates.format(emp.getBirth(),'yyyy-MM-dd')}" id="dateFormat" autocomplete="off">
      </div>
      <button type="submit" class="btn btn-primary">修改</button>
   </form>
   ```

4. 后端执行修改操作

   ```java
   @PostMapping("/updateEmp")
   public String updateEmp(Employee employee){
       employeeDao.addEmployee(employee);
       //回到员工列表页面
       return "redirect:/emp/allEmp";
   }
   ```

#### 4.删除用户

1. 调用Controller方法，并把需要删除的用户的id传过去

   ```html
   <a class="btn btn-sm btn-danger" th:href="@{/emp/delemp/}+${emp.getId()}">删除</a>
   ```

2. 执行删除操作

   ```java
   @GetMapping("/delemp/{id}")
   public String deleteEmp(@PathVariable("id")int id){
       employeeDao.delete(id);
       return "redirect:/emp/allEmp";
   }
   ```

### 拦截器

1. 创建一个继承了`HandlerInterceptor`接口的类，并实现`preHandle()`方法

   ```java
   public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
       //登录成功之后，应该会有用户的session
       Object loginUser = request.getSession().getAttribute("loginUser");
   
       if(loginUser==null){
           request.setAttribute("error","没有权限，请先登录");
           request.getRequestDispatcher("/index.html").forward(request,response);
           return false;
       }else{
           return true;
       }
   }
   ```

2. 在springMVC中重写`addInterceptors()`方法

   ```java
   @Override
   public void addInterceptors(InterceptorRegistry registry) {
       registry.addInterceptor(new LoginHandlerInterceptor())
               //拦截所有的请求                 不需要拦截的请求
               .addPathPatterns("/**").excludePathPatterns("/index.html","/","/user/login","/css/**","/img/**","/js/**");
   }
   ```

### 国际化

1. 在resource下配置i18n文件

   ![image-20200428175620910](C:\Users\韩宝宝\AppData\Roaming\Typora\typora-user-images\image-20200428175620910.png)

   ![image-20200428215559318](C:\Users\韩宝宝\AppData\Roaming\Typora\typora-user-images\image-20200428215559318.png)

2. 在application配置文件添加国际化配置的地址`spring.messages.basename="i18n.login"`

3. 我们如果需要在项目中进行按钮自动切换，我们需要继承一个接口`LocaleResolver`，并重写它的`resolveLocale()`方法

   ```java
   //解析请求
   @Override
   public Locale resolveLocale(HttpServletRequest request) {
       //获取请求中的语言参数
       String language = request.getParameter("l");
       Locale locale = Locale.getDefault();//如果没有就使用默认的
   
       //如果请求的连接携带了国际化的参数
       if(!StringUtils.isEmpty(language)){
           //zh_CN
           String[] split = language.split("_");
           //国家，地区
           locale = new Locale(split[0], split[1]);
   
       }
       return locale;
   }
   ```

4. 在html页面显示

   ![image-20200428220015591](C:\Users\韩宝宝\AppData\Roaming\Typora\typora-user-images\image-20200428220015591.png)

5. 记得将自己写的组件配置到spring容器中（在springMVC的扩展类中）

   ![image-20200428215730375](C:\Users\韩宝宝\AppData\Roaming\Typora\typora-user-images\image-20200428215730375.png)





## 整合JDBC



## 整合Druid数据源

1. 导入jar包

   ```xml
   <dependency>
       <groupId>com.alibaba</groupId>
       <artifactId>druid</artifactId>
       <version>1.1.22</version>
   </dependency>
   ```

2. 

