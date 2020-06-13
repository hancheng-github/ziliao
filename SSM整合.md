







![image-20200530220809967](../../AppData/Roaming/Typora/typora-user-images/image-20200530220809967.png)

# web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://java.sun.com/xml/ns/javaee"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         id="WebApp_ID" version="3.0">
  <display-name>Archetype Created Web Application</display-name>

  <!--读取Spring的Xml文件，整合Spring-->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:spring.xml</param-value>
  </context-param>
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>

  <!--前端控制器-->
  <servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!--在服务器启动时加载器springmvc.xml文件-->
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:springmvc.xml</param-value>
    </init-param>
  </servlet>
  <servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>

  <!--字符串过滤器-->
  <filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>

  <!--4.使用Rest风格的URI,将页面普通的post请求转为delete或put请求-->
  <filter>
    <filter-name>hiddenHttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
  </filter>
  <filter-mapping>
    <filter-name>hiddenHttpMethodFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
</web-app>
```

# springmvc.xml

- ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:mvc="http://www.springframework.org/schema/mvc"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd
         http://www.springframework.org/schema/context
         http://www.springframework.org/schema/context/spring-context.xsd
         http://www.springframework.org/schema/mvc
         http://www.springframework.org/schema/mvc/spring-mvc.xsd">
  
      <!--设置扫描包-->
      <context:component-scan base-package="com" use-default-filters="false">
          <!--只扫描Controller注解-->
          <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"></context:include-filter>
      </context:component-scan>
  
      <!--注解驱动-->
      <mvc:annotation-driven></mvc:annotation-driven>
  
      <!--视图解析器-->
      <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
          <property name="prefix" value="/WEB-INF/jsp/"></property>
          <property name="suffix" value=".jsp"></property>
      </bean>
  </beans>
  ```



# spring.xml

- ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:tx="http://www.springframework.org/schema/tx"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd
         http://www.springframework.org/schema/context
         http://www.springframework.org/schema/context/spring-context.xsd
         http://www.springframework.org/schema/tx
         http://www.springframework.org/schema/tx/spring-tx.xsd">
  
      <!--设置扫描包-->
      <context:component-scan base-package="com">
          <!--不扫描Controller注解-->
          <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"></context:exclude-filter>
      </context:component-scan>
  
      <context:property-placeholder location="classpath:dbconfig.properties"></context:property-placeholder>
  
      <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
          <property name="driverClass" value="${jdbc.driver}"></property>
          <property name="jdbcUrl" value="${jdbc.url}"></property>
          <property name="user" value="${jdbc.username}"></property>
          <property name="password" value="${jdbc.password}"></property>
      </bean>
  
      <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
          <property name="configLocation" value="classpath:mybatis-config.xml"></property>
          <property name="dataSource" ref="dataSource"></property>
          <!--mapper映射文件的位置-->
          <property name="mapperLocations" value="classpath*:com/mapper/*.xml"></property>
      </bean>
  
      <!--配置Mapper接口-->
      <!--<bean id="mapperFactory" class="org.mybatis.spring.mapper.MapperFactoryBean">-->
          <!--&lt;!&ndash;关联Mapper接口&ndash;&gt;-->
          <!--<property name="mapperInterface" value="com.dao.CustomerDao"></property>-->
          <!--&lt;!&ndash;关联SqlSessionFactory&ndash;&gt;-->
          <!--<property name="sqlSessionFactory" ref="sqlSessionFactory"></property>-->
      <!--</bean>-->
  
      <!--配置一个Mapper接口的扫描
          注意：如果使用Mapper接口包扫描，那么每一个Mapper接口在Spring容器中的id的名称为类名的首字母小写
      -->
      <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
          <property name="basePackage" value="com.dao"></property>
      </bean>
  
      <!--开启Spring的事务-->
      <!--事务管理器-->
      <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
          <property name="dataSource" ref="dataSource"></property>
      </bean>
  
      <!--启用Spring的事务注解-->
      <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
  </beans>
  ```

