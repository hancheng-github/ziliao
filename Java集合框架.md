## 1、集合的理解和好处

理解：就是一种容器，用于保存元素



### 集合和数组的对比：

#### 数组的不足:

1. 数组的长度必须提前指定，并且一旦指定不能修改
2. 数组只能保存相同类型的元素

#### 集合的好处：

1. 集合在使用时，长度不用指定，而且可以实现自动扩容或截断

2. 集合可以保存不同类型（Object）元素（在没用泛型的情况下）



> 数组：比较适合保存基本类型的元素
>
> 集合：比较适合保存应用类型的元素

## 2、集合的框架体系图

### Collection接口继承树

![image-20200522175526405](../../AppData/Roaming/Typora/typora-user-images/image-20200522175526405.png)

### Map接口继承树

![image-20200522175757582](../../AppData/Roaming/Typora/typora-user-images/image-20200522175757582.png)

## 3、Collection接口的特点和使用

### 使用

#### 添加元素：

1. add(Object o)
2. addAll(Collection c)

#### 删除元素:

1. remove(Object o)
2. removeAll(Collection c)

#### 查找元素:

1. contains(Object o)
2. contains(Collection c)

#### 清空集合：

1. clear()

### 如何遍历：

1. 使用迭代器

   ```java
   Iterator iterator = col.iterator();
   while(iterator.hasNext()){
       Object next = iterator.next();
       System.out.println(next);
   }
   ```

2. ```java
   for(Object o : col){
       System.out.println(o);
   }
   ```

#### 判断是否为空：

1. isEmpty()



### 注意：

> 当添加的元素为引用类型元素时，需要给引用类重写**hashCode()**方法
>
> 使用迭代器过程中，不适合做增删，会报ConCurrentModificationException，**如果想修改，需要用迭代器本身的remove()方法**
>
> **使用迭代器过程中，可以做修改，但如果修改地址，不会报错，但没有效果**（包括String类型）
>
> 使用增强版的for循环，也不能给应用类型修改地址



## 4、List接口和Set接口的特点和使用

### List：

#### list接口特有的方法：

1. add(int index,Object o)

2. remove(int index)

3. indexOf(Object o)

4. get(int index)

5. 截取数组

   subList(int fromIndex,int toIndex)

#### 特点

1. 有序，可以重复

### Set特点：

1. 无需，不可重复



## 5、List接口的实现类学习

### ArrayList

底层结构：可变数组



jdk8：ArrayList中维护了Object[] elementData，初始容量为0，第一次添加时，容量扩大为10，再次添加时，如果容量足够，就不会扩容，如果容量不足，则扩容为原来的1.5倍



### Vector

底层结构：可变数组



初始容量为0，第一次添加时，容量扩大为10，再次添加时，如果容量足够，就不会扩容，如果容量不足，则扩容为原来的2倍

线程安全



### LinkedList

底层结构：双向列表



LinkedList中维护了两个重要的属性first和last，分别指向首节点和尾节点。

每个节点（Node类型）里面维护了三个重要的属性item、next、prev，分别指向当前元素，下一个和上一个



### 对比：

#### ArrayList和Vector的对比：

|           | 底层结构 | 版本 | 线程安全（同步） | 效率 | 扩容的倍数 |
| --------- | -------- | ---- | ---------------- | ---- | ---------- |
| ArrayList | 可变数组 | 新   | 不安全（不同步） | 较高 | 1.5        |
| Vector    | 可变数组 | 老   | 安全（同步）     | 较低 | 2          |

#### ArrayList和LinkedList的对比

|            | 底层结构 | 增删的效率                 | 改查的效率 |
| ---------- | -------- | -------------------------- | ---------- |
| ArrayList  | 可变数组 | 前面和中间的增删，效率较低 | 较高       |
| LinkedList | 双向列表 | 前面和中间的增删，效率较高 | 较低       |



### 总结

1. 考虑线程安全问题，用Vector
2. 不考虑线程安全问题
   - 增删较多，用LinkedList
   - 查较多，用ArrayList



## 6、Set接口的实现类学习

#### 特点

1. 不允许重复
2. 无序（插入和取出顺序不同），没有索引



### HashSet

#### 原理

1. 先获取需要添加的元素的hash值，通过一些运算获取一个整数索引index（要存放在数组的位置）

2. 如果改索引处没有元素，则直接添加

   如果索引处有其他元素，则需要进行equals判断，如果不相等，则以链表的形式追加到已有元素的后面；如果相等，则直接覆盖，返回false

#### 实现类

##### 底层结构

维护了一个HashMap对象，也就是和HashMap的底层一样，基于哈希表结构的

##### 如何实现去重

底层通过调用hashCode()方法和equals()方法实现去重

先调用hashCode，如果不相等，则直接可以添加



### TreeSet

#### 底层结构

红黑树

#### 两种实现方式

1. 自然排序

   要求：必须让添加元素类型实现Comparable接口，实现里面的compareTo方法

   ```java
   public class Employee implements Comparable {
   
       private Integer id;
       private String name;
       private Integer age;
   
       public int compareTo(Object o) {
           Employee e = (Employee)o;
           return Double.compare(this.age,e.age);
       }
   }
   ```

2. 定制排序

   要求：创建TreeSet对象时，传入一个Comparator，并实现里面的compare方法

   ```java
   TreeSet treeSet = new TreeSet(new Comparator() {
       public int compare(Object o1, Object o2) {
           Employee e1 = (Employee)o1;
           Employee e2 = (Employee)o2;
           return Double.compare(e1.getAge(),e2.getAge());
       }
   });
   ```

#### 如何实现去重

通过比较方法的返回值是否为0来判断是否重复

## 7、Map接口的特点和使用

### 特点

1. 用于保存一组键值对映射关系的
2. 键不能重复，值可以重复，一个键只能对应一个值

### 常见方法

1. put：添加

2. remove：删除
3. containsKey：查找键
4. containsValue：查找值
5. get：根据key获取value
6. size：获取键值对的数量
7. isEmpty：判断map是否为空
8. **entrySet：获取所有的关系**
9. **keySet：获取所有的键**
10. **values：获取所有的值**

1. 



## 8、Map接口的实现类学习

### hashMap

#### 底层结构

jdk7：数组+链表

jdk8：数组+链表+红黑树

#### 源码分析

jdk8：hashMap中维护了Node类型的数组table，当hashMap创建对象时，只是对loadFactory初始化为0.75；table还是保持默认值null



第一次添加时，将初始化table的容量为16，临界值为12

每次添加调用时putVal方法：

1. 先获取key的二次hash值并进行取与运算，得出存放的位置

2. 判断该存放位置上是否有元素，如果没有直接放

   如果改存放位置上已有元素，则进行继续判断：

   - 如果和当前元素直接相等，则覆盖
   - 如果不相等，则继续判断是否是链表结构还是树状结构，按照对应结构的判断方式判断相等

3. 将size++，判断是否超过了临界值，如果超过了，则需要进行2被扩容，并且重新排列

4. 当一个桶中的链表的节点数>=8 && 桶的总个数（table的容量）>=64时，会将链表结构变成红黑树结构



#### jdk7和jdk8的区别

|      |                                                              | table类型       | 数组结构         |
| ---- | ------------------------------------------------------------ | --------------- | ---------------- |
| jdk7 | 创建hashMap对象时，初始值table容量为16                       | table类型为Node | 数组+链表        |
| jdk8 | 创建hashMap对象时，没有初始table，仅仅只是初始加载因子，只有当第一个元素添加时才会初始化table为16 | table类型为Node | 数组+链表+红黑树 |

1. jdk7：创建hashMap对象时，初始值table容量为16

   jdk8：创建hashMap对象时，没有初始table，仅仅只是初始加载因子，只有当第一个元素添加时才会初始化table为16  

2. jdk7：table类型为Node

   jdk8：时才会初始化table为16  table类型为Node

3. jdk7：数组+链表，不管链表的总结点是多少都不会变成树结构

   jdk8：数组+链表+红黑树，当链表的节点树>=8 && 数组长度>=64时，会键链表变为红黑树结构

### hashtable

#### 底层结构

hash表，同hashMap

不能添加null



### TreeMap

#### 底层结构

红黑树，可以实现对添加元素的key进行排序

#### 应用：

- 自然排序：要求key的元素类型实现Comparable，并实现里面的compareTo方法
- 定制排序：要求创建TreeMap对象时，传入Comparator

## 9、Collection工具类的使用

## 10、泛型的使用

### 理解

参数化的类型，可以将某个类型当作参数	传递给类、接口或方法

#### 好处

1. 检查待添加的元素的类型，提高了类型的安全性
2. 减少了类型转换的次数，提高了效率

### 自定义泛型类

```java
Class MyClass<T>{
	T name;
	public void setName(T t){}
}
```

### 自定义泛型接口

```java
interface MyGenericInter<T,U,M>{
	void method(T t,U u,M m);
	M method2();
	U method3(T t1,T t2);
	M method4(T t,U u);
}
```

**实现或者继承的时候需要指定泛型，如果不指定，则默认为Object类型的**

**如果想让继承类也是泛型，需要给继承类加泛型**

```java
class MyGenericImpl<T,U,M> implements MygenericInter<T,U,M>{
	public void method(T t,U u,M m){}
	public M method2(){}
	public U method3(T t1,T t2){}
	public M method4(T t,U u){}
}
```

