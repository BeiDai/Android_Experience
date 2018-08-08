# 利用JDBC调用SQLServer、Oracle、MySQL、SQLite

## JAVA中利用JDBC操作数据库

- 理解JDBC原理
- 掌握Connection接口的使用
- 掌握Statement接口的使用
- 掌握ResultSet接口的使用
- 掌握PreparedStatem接口的使用

JDBC：是Java数据可连接技术的简称，提供连接各种常用数据库的能力

Java应用程序 - JDBC API - JDBC Driver Manager - JDBC驱动 - Sql Server、Oracle

JDBC API：与数据库建立连接、执行SQL语句、处理结果

1、 Driver Manager：依据数据库的不同管理JDBC驱动

2、 Connection：负责连接数据库

3、 Statement: 由Connection产生、负责执行SQL语句

4、 ResultSet：负责保存Statement执行后所产生的查询结果

#### JDBC工作模板

    try{
      Class.forName(JDBC);
    }catch(ClassNotFoundException e){
      System.out.println("无法找到驱动类");
    }

    try{
      Connection con = DriverManager.getConnection(
        JDBC URL,数据库用户名,密码);

      Statement stmt = con.createStatement();
      ResultSet rs = stmt.executeQuery("SELECT a,b,c FROM Table1");

      while(rs.next()){
        int x = rs.getlnt("a");
        String s = rs.getString("b");
        float f = rs.getFloat("c");
      }
      con.close();
    }catch (SQLException e){
      e.printStackTrace();
    }

#### 加载驱动、建立连接、数据处理、断开连接

#### 驱动方式1：JDBC-ODBC桥 - ODBC - 数据库

- 将对JDBC API调用，装换为对另一组数据库连接API的调用
- 优点：可以访问所有ODBC可以访问的数据库
- 缺点：执行效率低，功能不够强大，每一台机子配置ODBC
---
    Connection conn = null;
    try{
      Class.forName("sun.jdbc.odbc.JdbcDriver");
    }catch(ClassNotFoundException e){
      logger.error(e);
    }

    try{
      conn = DriverManager.getConnection(
        "jdb:odbc:ConnSQLServer","jdit",bdqn);
      System.out.println("建立连接成功！");
    }catch(SQLException e){
      logger.error(e);
    }finally{
      try{
        conn.close();
      }catch(SQLException e){
        logger.error(e);
      }
    }

- 配置数据库软件： SQLServer 2005
- 确认TCP/IP启动：IP地址可以连接 (ipconfig查看IP地址) 1433 端口
- SQL Server 和 Windows 身份登录(安全性)
- 允许远程连接到此服务器(属性)
- 设置SQL Server账号密码
- 右键新建数据库，新建表，打开表，添加数据

- 控制面板：管理工具：控制源ODBC：(环境变量：ISH、DSH):创建数据源

#### 驱动方式2：纯Java驱动 - 数据库

- 由JDBC驱动直接访问数据库
- 优点：100%Java,快且跨平台
- 缺点: 访问不同的数据库需要下载专用的JDBC驱动
---
    Connection conn = null;
    try{
      Class.forName("com.microsoft.jdbc.sqlserver.SQLServerDriver");
    }catch(ClassNotFoundException e){
      logger.error(e);
    }
    try{
      conn = DriverManager.getConnection(
       "jdbc:sqlserver://localhost:1433;DatabaseName=epet","jdit","bdqn");
      System.out.println("建立连接成功！");
      }catch(SQLException e){
        logger.error(e);
      }catch(SQLException e){
        logger.error(e);
    }finally{
      try{
        conn.close();
      }catch(SQLException e){
        logger.error(e);
      }
    }

- 下载javasql驱动包(msbase.jar,mssqlserver.jar,msutil.jar)
- 添加方式一：项目属性 ·· Java Build Path ·· Libraries ·· Add External jars 添加第三方jar包
- 添加方式二：拷贝到 WEB-INF/lib 目录下

## JDBC应用

- 对宠物和主人信息进行修改
  - 宠物和主人信息储存在SQL Server 2008中
  - 通过JDBC对宠物和主人进行增、删、改、查

---
|宠物信息表格|
|:----------:|

| 字段名 | 字段说明 | 字段类型 | 其他 |
|:------|:-------:|:---------:|---------:|
|   id  |   序号  |     int   |  主键、自增|
| name  | 名字 | varchar(12) |  |
| health | 健康值  | int   | |
| love  | 亲密度 | int | |
| strain | 品种 | varchar(12)|||

---
|主人信息表格|
|:----------:|

| 字段名 | 字段说明 | 字段类型 | 其他 |
|:------|:-------:|:---------:|---------:|
|   id  |   序号  |     int   |  主键、自增|
| name  | 名字 | varchar(12) |  |
| passwor | 密码 | varchar(12) |  ||

---
#### 示例

    ...
    Connection coon = null;
    Statement stmt = null;

    ...
    // 建立连接
    coon = DriverManager.getConnection(
      "jdbc.sqlserver://lacalhost:1433;DatabaseName=epet","jbit","bdqn");
    // 插入狗狗信息到数据库
    stmt = conn.createStatement();
    StringBuffer sbSql = new StringBuffer(
      "insert into dog(name,health,love,strain)values("")"
      sbSql.append(name+",");
      sbSql.append(health+",");
      sbSql.append(love+",");
      sbSql.append(strain+")")
      stmt.execute(sbSql.toString()); // 添加删除指令
      )
      ...
      stmt.close();
      conn.close();

    // 更新指令
    stmt.executeUpdate("Update dog set health=80,love=15 where id=1");

---

    // 使用Statement和ResultSet查询宠物
    Connection coon = null;
    Statement stmt = null;
    ResultSet rs = null;
    ...
    conn = DriverManager.getConnection(
      "jdbc:sqlserver://localhost:1433;DatabaseName=epet","jbit","bdqn"
      )
    stmt = conn.createStatement();
    rs = stmt.executeQuery("select*from dog");
    System.out.println("\t\t狗狗信息列表\n编号\t姓名\t健康值\t亲密度\t品种");
    while(rs.next()){
      System.out.println(rs.getlnt(1)+"\t");
      System.out.println(rs.getString(2)+"\t");
      System.out.println(rs.getlnt("health")+"\t");
      System.out.println(rs.getlnt("love")+"\t");
      System.out.println(rs.getString("strain"));
    }
    ...
    rs.close();
    stmt.close();
    conn.close();

#### Statment常用方法

- ResultSetexecuteQuery：执行SQL查询并获取ResultSet对象
- int executeUpdate（String sql)：可以执行插入、删除、更新等操作，返回值是执行该操作所影响的行数
- boolean execute(String sql): 可以执行SQL语句，然后获得一个布尔值，表示是否返回ResultSet

#### Result常用方法

| 方法名 | 说明 |
|:----------|-----------:|
| boolean next() | 将光标从当前位置向下移动一行 |
| boolean previous | 游标当前位置向上移动一行 |
| void close() | 关闭ResulSet对象 |
| int getlnt(int collndex) | 以int形式获取结果集当前行指定列号值 |
| int getlnt(String colLabel) | 以int形式获取结果集当前行指定列名值 |
| float getFloat(int collndex) | 以float形式获取结果集当前行指定列号值 |
| float getFloat(String colLabel) | 以float形式获取结果集当前行指定列名值 |
| String getString(int collndex) | 以String形式获取结果集当前行指定列号值 |
| String getString(String colLabel) |以String形式获取结果集当前行指定列名值|
