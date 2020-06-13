# 容器的基本实现

```java
BeanFactory bf = new XmlBeanFactory(newClassPathResource("applicationContext.xml"));
MyTestBean bean = bf.getBean(MyTestBean.class);
System.out.println(bean.getTestStr());
```



## 核心类介绍：

### DefaultListableBeanFactory：

​		**XmlBeanFactory**继承自**DefaultListableBeanFactoty**，而**DefaultListableBeanFactoty**是整个bean加载的核心部分，是Spring注册及加载bean的默认实现

![image-20200422223819877](C:\Users\韩宝宝\AppData\Roaming\Typora\typora-user-images\image-20200422223819877.png)

​		XmlBeanFactory对DefaultListableBeanFactory类进行了扩展，主要用于从XML文档中读取BeanDefinition，对于注册及获取bean都是使用从父类DefaultListableBeanFactory继承的方法去实现。但是却**增加了XmlBeanDefinitionReader类型reader属性**，在XmlBeanFactory中主要使用reader属性对资源文件进行读取和注册。

- 各个类的功能：

| **AliasRegistry**                      | 定义对alias的简单增删改等操作                                |
| -------------------------------------- | ------------------------------------------------------------ |
| **SimpleAliasRegistry**                | 主要使用map作为alias的缓存，并对接口AliasRegistry进行实现    |
| **SingletonBeanRegistry**              | 定义对单例的注册及获取                                       |
| **BeanFactory**                        | 定义获取bean及bean的各种属性                                 |
| **DefaultSingletonBeanRegistry**       | 对接口SingletonBeanRegistry个函数的实现                      |
| **HierarchicalBeanFactory**            | 继承BeanFactory，也就是在BeanFactory定义的功能的继承上增加了对parentFactory的支持 |
| **BeanDefinitionRegistry**             | 定义对BeanDefinition的各种增删改操作                         |
| **FactoryBeanRegistrySupport**         | 在DefaultSingletonBeanRegistry基础上增加了对FactoryBean的特殊处理功能 |
| **ConfigurableBeanFactory**            | 根据各种条件获取bean的配置清单                               |
| **ListableBeanFactory**                | 根据各种条件获取bean的配置清单                               |
| **AbstractBeanFactory**                | 综合FactoryBeanRegistrySupport和ConfigurableBeanFactory的功能 |
| **AutowireCapableBeanFactory**         | 提供创建bean、自动注入、初始化以及应用bean的后处理器         |
| **AbstractAutowireCapableBeanFactory** | 综合AbstractBeanFactory并对接口AutowireCapableBeanFactory进行实现 |
| **ConfigurableListableBeanFactory**    | BeanFactory配置清单，指定忽略类型及接口等                    |
| **DefaultListableBeanFactory**         | 综合上面所有功能，主要是对bean注册后的处理                   |



### XmlBeanDefinitionReader：

​		XML配置文件的读取是Spring中重要的功能，因为Spring的大部分功能都是以配置作为切入点的

```Java
BeanFactory bf = new XmlBeanFactory(new ClassPathResource("applicationContext.xml"));
```



配置文件封装：

​		Spring的配置文件读取是通过ClassPathResource进行封装的，如new ClassPathResource("applicationContext.xml")，通过resource getResource()方法返回一个Resource文件，接下来就可以对所有资源文件进行统一处理