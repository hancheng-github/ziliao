概念：Java database connectivity  Java数据库连接，Java语言操作数据库
JDBC：官方定义的一套操作所有关系型数据库的规则（接口）。各个数据库厂商去实现这个接口，提供数据库驱动jar包。我们可以使用这套接口（JDBC）编程，真正执行的代码是驱动jar包中的实现类

# 1、步骤：

​    1.导入驱动jar包
​        1.复制mysql-connector-java-8.0.18.jar到项目的libs目录下
​        2.右键添加库
​    2.注册驱动
​        Class.forName("com.mysql.cj.jdbc.Driver");
​    3.获取数据库连接对象 Connection
​        Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/jdbc?useSSL=false&serverTimezone=UTC","root","123456");
​    4.定义sql语句
​        String sql = "select * from class";
​    5.获取执行sql语句的对象 Statement或者PreparedStatement（安全）
​        Statement sta = con.createStatement();

```java
String sql = "update smbms_user set userPassword = ? where id = ?";
preparedStatement = connection.prepareStatement(sql);
preparedStatement.setString(1,123456);
preparedStatement.setInt(2,1);
```

​    6.执行sql，接受返回结果
​        ResultSet rs = sta.executeQuery(sql);
​    7.处理结果
​         while(rs.next()) {
​            System.out.println(rs.getString(1)+","+rs.getString(2));
​        }
​    8.释放资源
​        rs.close();
​        sta.close();
​        con.close();



# 2、JDBC各个对象

##     1.DriverManager : 驱动管理对象

​        功能：
​            1.注册驱动
​                static void registerDriver(Driver driver) : 注册驱动给定的驱动程序 DriverManager.
​                写代码使用 : Class.forName("com.mysql.cj.jdbc.Driver");
​                通过查看源码发现 : 在com.mysql.cj.jdbc.Driver 类中存在静态代码块
​                    static {
​                        try{
​                            java.sql.DriverManager.registerDriver(new Driver());
​                        }catch(SQLException e){
​                            throw new RuntimeException("Can't register driver!");
​                        }
​                    }
​            2.获取数据库连接
​                方法 : static Connection getConnection(String url, String user, String password);

##     2.Connection : 数据库连接对象

​        1.获取执行sql语句的对象
​            Statement createStatement();
​            PreparedStatement PreparedStatement(String sql)
​        2.事务管理
​            开启事务 : setAutoCommit(boolean autoCommit) : 调用该方法设置为false，即可开启事务
​            提交事务 : commit()
​            回滚事务 : rollback()

##     3.Statement : 执行sql的对象

​        1.执行sql的对象
​            1.boolean execute(String sql) : 可以执行任意的sql语句
​            2.int executeUpdate(String sql) : 执行DML（insert,update,delecte）语句,DDL（create,alter,drop）语句，返回的是执行的行数
​            3.ResultSet executeQuery(String sql) : 执行DQL（select）语句,返回的是结果集

##     4.ResultSet : 结果集对象

​        next() : 游标向下移动一行
​        getXxx(参数) : 获取数据
​            Xxx : 代表数据类型
​            参数:
​                1. int : 代表列的编号，从1开始   如：getString(1)
​                2. String : 代表列名称

##     5.PreparedStatement : 执行sql语句，比Statement更强大

​        Connection.preparedStatement(String sql)
​        预编译的sql语句：使用?作为占位符
​            select * from user where username = ? and password = ?;
​        赋值 : setXxx(参数1，参数2)
​            参数1 : ?的位置编号，从1开始
​            参数2 : ?的值

# 3、JDBC控制事务

​    1.事务：一个包含多个操作步骤的业务操作。如果这个业务操作被事务管理，则这多个步骤要么同时成功，要么同时失败
​    2.操作：
​        1.开启事务
​        2.提交事务
​        3.回滚事务
​    3.使用Connection对象来管理事务
​        开启事务 : setAutoCommit(boolean autoCommit) : 调用该方法设置为false，即可开启事务
​            在执行sql之前开启事务
​        提交事务 : commit()
​            当所有sql都执行完提交事务
​        回滚事务 : rollback()   
​            在catch中回滚事务

# 4、数据库连接池

​    1.概念 : 其实就是一个容器(集合)，存放数据库连接

##     2.实现 :

​        1.标准接口 : DataSource javax.sql包下
​            方法
​                获取连接 : getConnection()
​                归还连接 : 如果连接对象Connection是从连接池中获取的，那么调用Connection.close()方法，则不会在关闭连接了，而是归还连接
​        2.一般我们不去实现它，由数据库厂商来实现
​            1.C3P0 : 数据库连接池技术
​            2.Druid : 数据库连接池实现技术，由阿里巴巴提供

##     3.C3P0 : 数据库连接池技术

​        1.导入jar包（两个） c3p0-0.9.5.2.jar 和 mchange-commons-java-0.2.11.jar和mysql-connection-java-8.0.18.jar
​        2.定义配置文件 :
​            名词 : c3p0.properties or c3p0-config.xml
​            路径 : 直接放在src目录下
​        3.创建核心对象,数据库连接池对象 : ComboPooledDatasource
​        4.获取连接 : getConnection()

##     4.Druid : 数据库连接池实现技术，由阿里巴巴提供

​        1.导入jar包 druid-1.0.9.jar
​        2.定义配置文件:
​            是properties形式的
​            可以叫任意名称，可以放在任意目录下
​        3.加载配置文件 : properties
​        4.获取数据库连接池对象 : 通过工厂来获取 
​            DataSource ds = DruidDataSourceFactory.createDataSource()
​        5.获取连接 : getConnection()

# 5、Spring JDBC

​    Spring框架对JDBC的简单封装.提供了一个JDBCTemple
​    步骤:
​        1.导入jar包
​        2.创建JdbcTemplelate对象。依赖于数据源Datasource
​            JdbcTemplate template = new JdbcTemplate();
​        3.调用JdbcTmplelate的方法来完成CRUD的操作
​            update(String) : 执行DML语句。增，删，改语句
​            queryForMap(String) : 查询结果将结果集封装为map集合
​                注意：这个方法查询的结果集只能是1
​            queryForList(String) : 查询结果j将结果封装为list集合
​                注意：将每一条记录封装为一个Map集合，再将Map集合装载到List集合中
​            query(String，RowMapper) : 查询结果，将结果封装为JavaBean对象
​                new BeanPropertyRowMapper<Emp(类型)>(Emp.class)
​            queryForObject(String，返回类型) : 查询结果，将结果封装为对象