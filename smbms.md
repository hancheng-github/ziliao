# 项目搭建

1. 搭建maven web项目

2. 配置tomcat

3. 测试项目是否能够跑起来

4. 导入需要的jar包

   jsp、Servlet、mysql驱动、jstl、stand

5. 编写实体类

6. 编写基础公共类

   1. 数据库配置文件

      ```properties
      driver=com.mysql.jdbc.Driver;
      url=jdbc:mysql://localhost:3306/jdbc2?useUnicode=true&characterEncoding=utf-8
      user=root
      password=123456
      ```

   2. 编写数据库的公共类

      ```java
      public class BaseDao {
          private static String driver;
          private static String url;
          private static String user;
          private static String password;
      
          static {
              Properties properties = new Properties();
              InputStream inputStream = BaseDao.class.getClassLoader().getResourceAsStream("/db.properties");
              try{
                  properties.load(inputStream);
              }catch (IOException e){
                  e.printStackTrace();
              }
      
              driver = properties.getProperty("driver");
              url = properties.getProperty("url");
              user = properties.getProperty("user");
              password = properties.getProperty("password");
          }
      
          //获取数据库连接
          public static Connection getConnection(){
              Connection connection = null;
              try {
                  Class.forName(driver);
                  connection = DriverManager.getConnection(url, user, password);
              } catch (ClassNotFoundException e) {
                  e.printStackTrace();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
              return connection;
          }
      
          //编写查询公共类
          public static ResultSet execute(Connection connection,String sql,Object[] params, ResultSet resultSet, PreparedStatement preparedStatement){
              try {
                  preparedStatement = connection.prepareStatement(sql);
                  for(int i = 1; i < params.length; i++){
                      preparedStatement.setObject(i+1,params[i]);
                      resultSet = preparedStatement.executeQuery();
                  }
              } catch (SQLException e) {
                  e.printStackTrace();
              }
      
              return resultSet;
          }
      
          //编写增删改方法
          public static int update(Connection connection,String sql,Object[] params, PreparedStatement preparedStatement){
              int updateRows = 0;
              try {
                  preparedStatement = connection.prepareStatement(sql);
                  for(int i = 1; i < params.length; i++){
                      preparedStatement.setObject(i+1,params[i]);
                       updateRows = preparedStatement.executeUpdate();
                  }
              } catch (SQLException e) {
                  e.printStackTrace();
              }
      
              return updateRows;
          }
      
          public static boolean close(Connection connection,ResultSet resultSet, PreparedStatement preparedStatement){
              boolean flag = true;
      
              if(resultSet!=null){
                  try {
                      resultSet.close();
                      resultSet = null;
                  } catch (SQLException e) {
                      e.printStackTrace();
                      flag = false;
                  }
              }
      
              if(preparedStatement!=null){
                  try {
                      preparedStatement.close();
                      preparedStatement = null;
                  } catch (SQLException e) {
                      e.printStackTrace();
                      flag = false;
                  }
              }
      
              if(connection!=null){
                  try {
                      connection.close();
                      connection = null;
                  } catch (SQLException e) {
                      e.printStackTrace();
                      flag = false;
                  }
              }
      
              return flag;
          }
      }
      ```

   3. 编写字符编码过滤器

7. 导入静态资源



# 登录功能实现

1. 编写前端页面

2. 设置首页

   ```xml
   <welcome-file-list>
       <welcome-file>login.jsp</welcome-file>
   </welcome-file-list>
   ```

3. 编写dao层用户登录的接口

   ```java
   public interface UserDao {
       //得到要登录的用户
       public User getLoginUser(Connection connection,String userCode);
   }
   ```

4. 实现

   ```java
   public class UserDaoImpl implements UserDao {
       @Override
       public User getLoginUser(Connection connection,String userCode) {
   
           PreparedStatement pstm = null;
           ResultSet rs = null;
           User user = null;
   
           if(connection!=null){
               String sql = "select * from smbms_user where userCode = ?";
               Object[] params = {userCode};
               try{
                   rs = BaseDao.execute(connection, sql, params, rs, pstm);
   
                   while(rs.next()){
                       user = new User();
                       user.setId(rs.getInt("id"));
                       user.setUserCode(rs.getString("userCode"));
                       user.setUserName(rs.getString("userName"));
                       user.setUserPassword(rs.getString("userPassword"));
                       user.setGender(rs.getInt("gender"));
                       user.setBirthday(rs.getDate("birthday"));
                       user.setPhone(rs.getString("phone"));
                       user.setAddress(rs.getString("address"));
                       user.setUserRole(rs.getInt("userRole"));
                       user.setCreatedBy(rs.getInt("createdBy"));
                       user.setCreationDate(rs.getTimestamp("creationDate"));
                       user.setModifyBy(rs.getInt("modifyBy"));
                       user.setModifyDate(rs.getTimestamp("modifyDate"));
                   }
                   BaseDao.close(null,pstm,rs);
               }catch (Exception e){
   
               }
           }
   
           return user;
       }
   }
   ```

5. 业务层接口

   ```java
   public interface UserService {
       //用户登录
       public User login(String userCode, String password);
   }
   ```

6. 业务层实现类

   ```java
   public class UserServiceImpl implements UserService {
   
        private UserDao userDao;
   
        public UserServiceImpl(){
            userDao = new UserDaoImpl();
        }
   
       @Override
       public User login(String userCode, String password) {
           Connection connection = BaseDao.getConnection();
   //        PreparedStatement preparedStatement = null;
   //        ResultSet resultSet = null;
   //        Object[] params = {userCode,password};
   //        String sql = "selct * from smbms_user where userCode = ? and userPassword = ?";
           User user = null;
   
          try{
   //           resultSet = BaseDao.execute(connection, preparedStatement, resultSet, params, sql);
   
   //           if(resultSet!=null){
                  user = userDao.getLoginUser(connection, userCode);
   //           }
          }catch (Exception e){
              e.printStackTrace();
          }finally {
              BaseDao.close(connection,null,null);
          }
   
           return user;
       }
   
       @Test
       public void test(){
           UserService service = new UserServiceImpl();
           User admin = service.login("admin","1213131");
           System.out.println(admin);
       }
   }
   ```

7. 编写Servlet

   ```java
   public class LoginServlet extends HttpServlet {
   
       private UserService service = new UserServiceImpl();
   
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           //获取用户名和密码
           String userCode = req.getParameter("userCode");
           String userPassword = req.getParameter("userPassword");
   
           //和数据库中的数据进行对比，调用业务层
           User user = service.login(userCode, userPassword);
   
           if(user!=null){//查有此人，可以登录
               //将用户的信息放到Session中
               req.getSession().setAttribute(Constants.USER_SESSION,user);
               //跳转到主页
               resp.sendRedirect("jsp/frame.jsp");
           }else{//查无此人，跳转到错误页面
               req.setAttribute("error","用户名或者密码错误");
               req.getRequestDispatcher("login.jsp").forward(req,resp);
           }
   
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doGet(req, resp);
       }
   }
   ```

8. 注册servlet

   ```xml
   <servlet>
       <servlet-name>loginServlet</servlet-name>
       <servlet-class>com.hc.servlet.user.LoginServlet</servlet-class>
   </servlet>
   <servlet-mapping>
       <servlet-name>loginServlet</servlet-name>
       <url-pattern>/login</url-pattern>
   </servlet-mapping>
   ```



# 登录功能优化

## 注销功能

```java
public class LogoutServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //移除用户的Session
        req.getSession().removeAttribute(Constants.USER_SESSION);
        //返回登录页面
        resp.sendRedirect("login.jsp");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

注册XML

```xml
<servlet>
    <servlet-name>logoutServlet</servlet-name>
    <servlet-class>com.hc.servlet.user.LogoutServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>logoutServlet</servlet-name>
    <url-pattern>/logout</url-pattern>
</servlet-mapping>
```





## 登录拦截

编写一个拦截器，并注册

```java
public class SysFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest) servletRequest;
        HttpServletResponse response = (HttpServletResponse) servletResponse;

        //重Session中获取数据
        User user = (User) request.getSession().getAttribute(Constants.USER_SESSION);

        if(user==null){//已经被移除或者被注销了，或者未登录
            response.sendRedirect("/jsp/error.jsp");
        }

        filterChain.doFilter(servletRequest,servletResponse);
    }

    @Override
    public void destroy() {

    }
}
```

用户登录过滤

```xml
<!--用户登录过滤器-->
<filter>
    <filter-name>sysFilter</filter-name>
    <filter-class>com.hc.filter.SysFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>sysFilter</filter-name>
    <url-pattern>/jsp/*</url-pattern>
</filter-mapping>
```



# 密码修改

1. 导入前端素材

2. UserDao接口

   ```java
   //修改当前用户密码
   public int updatePwd(Connection connection, int id, String password);
   ```

3. UserDao实现接口

   ```java
   //修改当前用户密码
   @Override
   public int updatePwd(Connection connection, int id, String password) {
       int execute = 0;
       PreparedStatement pstm = null;
   
       if(connection!=null){
           String sql = "update smbms_user set userPassword = ? where id = ?";
           Object[] params = {id,password};
           execute = BaseDao.execute(connection, pstm, params, sql);
           BaseDao.close(connection,pstm,null);
       }
   
       return execute;
   }
   ```

4. UserService接口

   ```java
   //修改当前用户密码
   public boolean updatePwd(int id, String password);
   ```

5. UserService实现类

   ```java
   @Override
   public boolean updatePwd(int id, String password) {
       Connection connection = BaseDao.getConnection();
       int execute = userDao.updatePwd(connection, id, password);
       boolean flag = false;
   
      try{
          if(execute >= 1){
              flag = true;
          }
      }catch (Exception e){
          e.printStackTrace();
      }finally {
          BaseDao.close(connection,null,null);
      }
   
       return flag;
   }
   ```

6. Servlet实现

   ```java
   //实现Servlet复用
   public class UserSevlet extends HttpServlet {
   
       private UserService userService = new UserServiceImpl();
   
       @Override
       protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           String method = req.getParameter("method");
           if((method == "method") && method != null){
               this.updatePwd(req,resp);
           }
       }
   
       @Override
       protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           doGet(req, resp);
       }
   
       public void updatePwd(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
           //从Session里面拿ID
           Object user = req.getSession().getAttribute(Constants.USER_SESSION);
           //获取新密码
           String newpassword = req.getParameter("newpassword");
   
           boolean flag = false;
   
           if(user!=null && !StringUtils.isNullOrEmpty(newpassword)){
               flag = userService.updatePwd(((User) user).getId(), newpassword);
               if(flag){
                   req.setAttribute("message","密码修改成功,请推出，使用新密码登录");
                   req.getSession().removeAttribute(Constants.USER_SESSION);
               }else{
                   req.setAttribute("message","密码修改失败");
               }
           }else {
               req.setAttribute("message","新密码有问题");
           }
   
           req.getRequestDispatcher("/jsp/pwdmodify.jsp").forward(req,resp);
       }
   }
   ```

7. Servlet注册

   ```xml
   <servlet>
       <servlet-name>userSevlet</servlet-name>
       <servlet-class>com.hc.servlet.user.UserSevlet</servlet-class>
   </servlet>
   <servlet-mapping>
       <servlet-name>userSevlet</servlet-name>
       <url-pattern>/user/updatepwd.html</url-pattern>
   </servlet-mapping>
   ```



## 实现Servlet的复用

需要提取出方法

```java
//实现Servlet复用
public class UserSevlet extends HttpServlet {

    private UserService userService = new UserServiceImpl();

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String method = req.getParameter("method");
        if((method == "method") && method != null){
            this.updatePwd(req,resp);
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }

    public void updatePwd(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //从Session里面拿ID
        Object user = req.getSession().getAttribute(Constants.USER_SESSION);
        //获取新密码
        String newpassword = req.getParameter("newpassword");

        boolean flag = false;

        if(user!=null && !StringUtils.isNullOrEmpty(newpassword)){
            flag = userService.updatePwd(((User) user).getId(), newpassword);
            if(flag){
                req.setAttribute("message","密码修改成功,请推出，使用新密码登录");
                req.getSession().removeAttribute(Constants.USER_SESSION);
            }else{
                req.setAttribute("message","密码修改失败");
            }
        }else {
            req.setAttribute("message","新密码有问题");
        }

        req.getRequestDispatcher("/jsp/pwdmodify.jsp").forward(req,resp);
    }
}
```



# 用户管理实现

思路

![image-20200508170021983](../../AppData/Roaming/Typora/typora-user-images/image-20200508170021983.png)



1. 导入分页的工具类
2. 用户列表页面导入



## 1、获取用户数量

1. UserDao

   ```java
   //查询用户总数
   public int getUserCount(Connection connection,String username, int userRole);
   ```

2. UserDaoImpl

   ```java
   //根据用户名或者角色查询用户总数
   @Override
   public int getUserCount(Connection connection, String username, int userRole) {
       PreparedStatement preparedStatement = null;
       ResultSet resultSet = null;
       int count = 0;
   
       if(connection!=null){
           StringBuffer sql = new StringBuffer();
           sql.append("select count(1) as count from smbms_user u,smbms_role r where u.userRole = r.id");
           ArrayList<Object> list = new ArrayList<>();//存放我们的参数
   
           if(!StringUtils.isNullOrEmpty(username)){
               sql.append(" and u.userName like ?");
               list.add("%"+username+"%");
           }
           if(userRole>0){
               sql.append(" and r.id = ?");
               list.add(userRole);
           }
   
           Object[] objects = list.toArray();
   
           System.out.println("UserDaoImpl->getUserCount:"+sql.toString());//输出最后完整的SQL语句
   
           resultSet = BaseDao.execute(connection, preparedStatement, resultSet, objects, sql.toString());
   
           try {
               if (resultSet.next()) {
                   count = resultSet.getInt("count");//从结果集中获取最终的数量
               }
           } catch (SQLException e) {
               e.printStackTrace();
           }finally {
               BaseDao.close(connection,preparedStatement,resultSet);
           }
       }
   
       return count;
   }
   ```

3. UserService

   ```java
   //查询记录数
   public int getUserCount(String username,int userRole);
   ```

4. UserServiceImpl

   ```java
   //查询记录数
   @Override
   public int getUserCount(String username, int userRole) {
       Connection connection = BaseDao.getConnection();
       int userCount = 0;
       try{
           userCount = userDao.getUserCount(connection, username, userRole);
       }catch (Exception e){
           e.printStackTrace();
       }finally {
           BaseDao.close(connection,null,null);
       }
       return userCount;
   }
   ```



## 2、获取用户列表

1. UserDao

   ```java
   //根据条件查询用户列表
   public List<User> getUserList(Connection connection,String userName,int userRole,int currentPageNo,int pageSize) throws SQLException;
   ```

2. UserDaoImpl

   ```java
   /*
   	currentPageNo:第几页
   	pageSize:页面放几条数据
   	跳过前面几页的数据，并获取到本页的数据
   	limit (currentPageNo - 1) * pageSize,pageSize
   */
   @Override
   public List<User> getUserList(Connection connection, String userName, int userRole, int currentPageNo, int pageSize) throws SQLException {
       PreparedStatement preparedStatement = null;
       ResultSet resultSet = null;
       List<User> userList = new ArrayList<>();
   
       if(connection!=null){
           StringBuffer sql = new StringBuffer();
           sql.append("select u.*,r.roleName as userRoleName from smbms_user u, smbms_role r where u.userRole = r.id");
           List<Object> list = new ArrayList<>();
           if(!StringUtils.isNullOrEmpty(userName)){
               sql.append(" and u.userName like ?");
               list.add("%"+userName+"%");
           }
           if(userRole>0){
               sql.append(" and r.id = ?");
               list.add(userRole);
           }
   
           //在数据库中，分页使用Limit
           //跳过页面
           sql.append(" order by creationDate DESC limit ?,?");
           currentPageNo = (currentPageNo - 1) * pageSize;
           list.add(currentPageNo);
           list.add(pageSize);
   
           Object[] params = list.toArray();
           System.out.println("sql--->"+sql.toString());
           resultSet = BaseDao.execute(connection,preparedStatement,resultSet,params,sql.toString());
           while (resultSet.next()){
               User user = new User();
               user.setId(resultSet.getInt("id"));
               user.setUserCode(resultSet.getString("userCode"));
               user.setUserName(resultSet.getString("userName"));
               user.setGender(resultSet.getInt("gender"));
               user.setBirthday(resultSet.getDate("birthday"));
               user.setPhone(resultSet.getString("phone"));
               user.setUserRole(resultSet.getInt("userRole"));
               user.setUserRoleName(resultSet.getString("userRoleName"));
               userList.add(user);
           }
           BaseDao.close(connection,preparedStatement,resultSet);
       }
   
       return userList;
   }
   ```

3. UserService

   ```java
   //根据条件查询用户列表
   public List<User> getUserList(String userName, int userRole, int currentPageNo, int pageSize);
   ```

4. UserServiceImpl

   ```java
   @Override
   public List<User> getUserList(String userName, int userRole, int currentPageNo, int pageSize) {
       Connection connection = BaseDao.getConnection();
       List<User> userList = new ArrayList<>();
       try {
           userList = userDao.getUserList(connection, userName, userRole, currentPageNo, pageSize);
       } catch (SQLException e) {
           e.printStackTrace();
       }finally {
           BaseDao.close(connection,null,null);
       }
   
       return userList;
   }
   ```



## 3、获取角色列表

1. RoleDao

   ```java
   public interface RoleDao {
       //根据条件查询角色列表
       public List<Role> getRoleList(Connection connection) throws SQLException;
   }
   ```

2. RoleDaoImpl

   ```java
   public class RoleDaoImpl implements RoleDao {
       @Override
       public List<Role> getRoleList(Connection connection) throws SQLException {
           List<Role> roleList = new ArrayList<>();
           PreparedStatement preparedStatement = null;
           ResultSet resultSet = null;
   
           if(connection!=null){
               String sql = "select * from smbms_role";
               Object[] params = {};
   
               resultSet = BaseDao.execute(connection, preparedStatement, resultSet, params, sql);
   
               while(resultSet.next()){
                   Role role = new Role();
                   role.setId(resultSet.getInt("id"));
                   role.setRoleCode(resultSet.getString("roleCode"));
                   role.setRoleName(resultSet.getString("roleName"));
                   role.setCreatedBy(resultSet.getInt("createdBy"));
                   role.setCreationDate(resultSet.getDate("creationDate"));
                   role.setModifyBy(resultSet.getInt("modifyBy"));
                   role.setModifyDate(resultSet.getDate("modifyDate"));
               }
   
               BaseDao.close(connection,null,null);
           }
   
           return roleList;
       }
   }
   ```

3. RoleService

   ```java
   public interface RoleService {
       //根据条件查询角色列表
       public List<Role> getRoleList() throws SQLException;
   }
   ```

4. RoleServiceImpl

   ```java
   public class RoleServiceImpl implements RoleService {
   
       private RoleDao roleDao = new RoleDaoImpl();
   
       @Override
       public List<Role> getRoleList() throws SQLException {
           List<Role> roleList = new ArrayList<>();
           Connection connection = BaseDao.getConnection();
   
           roleList = roleDao.getRoleList(connection);
   
           BaseDao.close(connection,null,null);
   
           return roleList;
       }
   }
   ```



## 4、用户显示的Servlet

1. 获取用户前端的数据（查询）

2. 判断请求是否需要执行

3. 为了实现分页，需要计算出页面和总页面，页面大小...

4. 用户列表展示

5. 返回前端

   ```java
   //重点，难点
   public void query(HttpServletRequest req, HttpServletResponse resp) throws SQLException, ServletException, IOException {
       //从前端获取数据
       String queryUserName = req.getParameter("queryUserName");
       String temp = req.getParameter("queryUserRole");
       String pageIndex = req.getParameter("pageIndex");
       int queryUserRole = 0;
   
       //第一次走这个请求，一定是第一页，页面大小固定的
       int pageSize = 5;
   
       //当前页码
       int currentPageNo = 1;
   
       if(queryUserName == null){
           queryUserName = "";
       }
       if(temp != null && !temp.equals("")){
           queryUserRole = Integer.parseInt(temp);//给查询赋值
       }
       if(pageIndex != null){
           currentPageNo = Integer.parseInt(pageIndex);
       }
   
       //获取用户的总数(分页：上一页，下一页的情况)
       int userCount = userService.getUserCount(queryUserName, queryUserRole);
   
       //总页数支持
       PageSupport pageSupport = new PageSupport();
       pageSupport.setCurrentPageNo(currentPageNo);
       pageSupport.setPageSize(pageSize);
       pageSupport.setTotalCount(userCount);
   
       int totalPageCount = pageSupport.getTotalCount();
   
       //控制首页和尾页
       //如果页面小于1，就强制显示第一页的内容
       if(currentPageNo  < 1){
           currentPageNo = 1;
       }else if(currentPageNo > totalPageCount){
           currentPageNo = totalPageCount;
       }
   
       //获取用户列表
       List<User> userList = userService.getUserList(queryUserName, queryUserRole, currentPageNo, pageSize);
       req.setAttribute("userList",userList);
   
       //获取角色列表
       List<Role> roleList = roleService.getRoleList();
       req.setAttribute("roleList",roleList);
       req.setAttribute("totalCount",userCount);
       req.setAttribute("currentPageNo",currentPageNo);
       req.setAttribute("totalPageCount",totalPageCount);
   
       //返回前端
       req.getRequestDispatcher("/jsp/userlist.jsp").forward(req,resp);
   }
   ```

   

