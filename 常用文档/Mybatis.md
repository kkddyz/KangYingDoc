## Mybaits框架

学习内容：

![image-20200711184452284](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20200711184452.png)



### Mybatis概述

1. mybatis是一个持久层Java框架，对JDBC进行了封装；

2. 目的是让开发者关注sql语句本身，忽略jdbc本来繁琐的步骤；

3. 使用ORM思想：建立了实体和数据库的映射。我们只需要关系实体做什么，而不用关心真正操作的实现。


> ORM ：Object Relational Mapping 


4. mybatis通过xml配置sql语句。	

   jbdc操作数据库的主体是statement，statement对象封装了要执行的SQL语句，带来的问题就是违反开闭原则。

(也就是必须修改源代码才能实现需求)

---

### Mybatis入门


#### mybatis环境搭建 

- https://mybatis.org/mybatis-3/zh/index.html -- 官方参考手册 

##### 创建mave工程导入依赖坐标

```xml
    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.5</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.19</version>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.12</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.10</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

##### 创建实体类(POJO)

```
package com.itheima.domain;

import java.io.Serializable;
import java.util.Date;

/**
 * @author kkddyz
 */
public class User implements Serializable {

    private Integer id;
    private String username;
    private Date birthday;
    private String sex;
    private String address;


    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }


    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", birthday=" + birthday +
                ", sex='" + sex + '\'' +
                ", address='" + address + '\'' +
                '}';
    }

}

```

##### 持久层接口(Dao)

```
package com.itheima.dao;

import com.itheima.domain.User;

import java.util.List;

/**
 * The interface User dao.
 *
 * @author kkddyz
 */
public interface IUserDao {
    /**
     * 查询所有
     *
     * @return the list
     */
    List<User> findAll();
}

```



##### 	mybatis主配置文件 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<!-- mybatis主配置文件-->
<configuration>
    <!-- 配置环境 可以配置多个环境 环境是指连接数据库的配置 默认采用id为mysql的环境-->
    <environments default="mysql">
        <!-- 配置mysql环境 -->
        <environment id="mysql">
            <!-- 配置事务类型 -->
            <transactionManager type="jdbc"></transactionManager>
            <!-- 数据源(连接池) -->
            <dataSource type="POOLED">
                <!-- 配置连接数据库的4个基本信息 -->
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/learn_mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>

    <!-- 指定映射配置文件的位置，映射配置文件指的是每个dao独立的配置文件    -->
    <mappers>
        <mapper resource="com/itheima/dao/IUserDao.xml"></mapper>
    </mappers>
</configuration>


```

> <transactionManager type="jdbc"> 
>
> **1、什么是事务**
>
> 事务是一个不可分割的工作单位，事务中包括的诸操作要么都做，要么都不做。事务可大可小，在关系数据库中，一个事务可以是一条SQL语句，一组SQL语句或整个程序。
>
> **2、MyBatis事务管理策略**
>
> MyBatis的事务管理分为两种形式：
>
> （1）使用JDBC的事务管理机制。
>
> 这种机制就是利用java.sql.Connection对象完成对事务的提交
>
> （2）使用MANAGED的事务管理机制。
>
> 这种机制mybatis自身不会去实现事务管理，而是让程序的Web容器或者Spring容器来实现对事务的管理。






##### 	接口配置文件 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.itheima.dao.IUserDao">
    <select id="findAll" resultType="com.itheima.domain.User">
        select * from user;
    </select>
</mapper>
```



---

##### 注意事项 

![image-20210811201147151](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210811201147.png)	

---

#### 入门案例

##### MybatisTest源码

```
/**
 * mybatis 入门案例
 *
 * @author kkddyz
 */
public class MybatisTest {

    public static void main(String[] args) throws IOException {


        // 1. 读取配置文件
        InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");

        //2. 创建SqlSessionFactory工厂
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory factory = builder.build(in);

        // 3. 使用工厂创建SqlSession
        SqlSession sqlSession = factory.openSession();

        // 4. 使用SqlSession创建代理对象(在web filter屏蔽敏感词第一次学到动态创建代理对象)
        IUserDao mapper = sqlSession.getMapper(IUserDao.class);

        // 5. 使用代理对象执行方法
        List<User> users = mapper.findAll();

        for (User user : users) {
            System.out.println(user);

        }

        // 6. 释放资源
        sqlSession.close();
        in.close();
    }
}

```



##### 入门案例分析 

![image-20210812200418338](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210812200418.png)

1. 读取配置文件

- `InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");`

- 路径一般不适用相对路径或者绝对路径
  - 相对路径在，web项目经过会发生变化(源文件在src，编译后在另外的文件夹里)
- 常用方式
  - ClassLoder ：只能读取类路径的配置文件
  - ServletContext.getRealPath()

> [Resources.getResourceAsStream（）](https://blog.csdn.net/dreamzuora/article/details/80354601)
>
> Resources 类为从类路径中加载资源，提供了易于使用的方法。

2. builder 

   [Builder模式笔记](https://github.com/kkddyz/DesignPattern-workspace/blob/master/src/builder/readme.md)



---



##### 注解配置

![mybatis配置](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210812212824.png)

##### Dao实现类

```java
/**
 * 接口实现类
 */
public class UserDaoImpl implements IUserDao {

    private SqlSessionFactory factory;

    /**
        为啥传递factory,而不是直接传递SqlSession
     */
    public UserDaoImpl(SqlSessionFactory factory) {
        this.factory = factory;
    }

    @Override
    public List<User> findAll() {
        // 1. 使用工厂创建SqlSession
        SqlSession sqlSession = factory.openSession();

        // 2. 通过SqlSession 执行查询 通过全类名.方法名指定配置的SQL语句
        List<User> users = sqlSession.selectList("com.itheima.dao.IUserDao.findAll");

        // 3. 关闭资源
        sqlSession.close();

        // 4. 返回查询结果
        return users;
    }
}
```

```java
/*
	测试类
*/

public class MybatisTest {

    public static void main(String[] args) throws IOException {


        // 1. 读取配置文件
        InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");

        //2. 创建SqlSessionFactory工厂
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory factory = builder.build(in);

        // 3. 通过工厂创建Dao实现类
        IUserDao userDao = new UserDaoImpl(factory);

        // 4. 使用代理对象执行方法
        List<User> users = userDao.findAll();

        for (User user : users) {
            System.out.println(user);

        }

        // 5. 释放资源
        in.close();
    }
}
```



### 自定义Mybatis

