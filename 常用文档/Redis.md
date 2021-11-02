> [redis教程](https://www.redis.net.cn/tutorial/3501.html)



## Redis入门

概念：高性能的NOSQL系列的非关系型数据库



出现的原因：满足大量数据库访问的需求.

![image-20210704182335677](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210705191950.png)

 



用途：NOSQL常常作为MYSQL数据库的缓存使用。

---





安装 ： 在redis中文网下载免安装版本 



### 重要文件 





- redis.windows.config 核心配置文件

- redis-cli.exe  : redis 客户端 

- redis-server.exe : 服务端 



### redis数据结构 

- 使用键值对
  - K ：String 
  -  value的数据结构 
    1. 字符串 String 
    2. HashMap
    3. LinkedList
    4. Set
    5. SortedSet有序集合：不允许重复元素+自动排序

<img src="https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210705191951.png" alt="image-20210704202049614" style="zoom:67%;" align="left"/>

---

## 命令操作





### 简单使用



#### String

<img src="https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210705191952.png" alt="image-20210704202844372" style="zoom:80%;" align="left"/>





#### hash

![image-20210704205041232](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210705191953.png)

> 注意：在redis中HashMap的属性只能是String-String，不可以嵌套另一个hash实现多级表。
>
> HashMap一般用于存储对象

---

#### LinkedList

操作分左右(头尾)进行。

![image-20210704210431474](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210705191954.png)

----





```
redis 127.0.0.1:6379> sadd mySet banana
(integer) 1
redis 127.0.0.1:6379> sadd mySet apple
(integer) 1
redis 127.0.0.1:6379> sadd mySet orange
(integer) 1
redis 127.0.0.1:6379> smembers mySet
1) "orange"
2) "banana"
3) "apple"
redis 127.0.0.1:6379> sadd mySet aaa
(integer) 1
redis 127.0.0.1:6379> smembers mySet
1) "aaa"
2) "orange"
3) "banana"
4) "apple"
redis 127.0.0.1:6379> sadd mySet aa
(integer) 1
redis 127.0.0.1:6379> smembers mySet
1) "aaa"
2) "orange"
3) "banana"
4) "apple"
5) "aa"
redis 127.0.0.1:6379> srem mySet aa
(integer) 1
redis 127.0.0.1:6379> srem mySet aaa
(integer) 1
redis 127.0.0.1:6379> smembers mySet
1) "banana"
2) "apple"
3) "orange"
redis 127.0.0.1:6379> sadd mySet apple
(integer) 0
redis 127.0.0.1:6379> sadd mySet apple
(integer) 0
redis 127.0.0.1:6379> smembers mySet
1) "orange"
2) "banana"
3) "apple"
```

> 重复添加不会被执行 无序



---

#### sorted set



*Redis 有序集合和集合一样也是string类型元素的集合,且不允许重复的成员。*

*不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。*

*有序集合的成员是唯一的,但分数(score)却可以重复。*

*集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。*



<img src="https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210705191955.png" alt="image-20210704212312919" style="zoom:80%;" align="left"/>



按照score升序排列

---

### 通用命令

keys 列举所有key 

type 获取key类型

del 删除key

<img src="https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210705191956.png" alt="image-20210704212724647" style="zoom:80%;" align="left"/>

> keys可以匹配正则表达式



---



## Redis持久化

1. 为了redis服务器重启时还能获取之前的数据，需要将数据保存到磁盘，即持久化。
2. 持久化机制
   1. ROB:默认方式
      - 在一定的时间间隔中，检测key的变化情况，然后持久化数据

      - 需要编辑redis.conf

        ```
        save 900 1
        save 300 10
        save 60 10000
        
        In the example below the behaviour will be to save:
        #   after 900 sec (15 min) if at least 1 key changed
        #   after 300 sec (5 min) if at least 10 keys changed
        #   after 60 sec if at least 10000 keys changed
        ```
      
      - 指定配置文件启动服务器 `redis-server.exe redis.conf`
      - 数据库文件![image-20210704215519890](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210705191957.png)
      
   2. AOF：日志记录，记录每一条命令的操作，每一次操作后持久化数据。

      1. 需要编辑redis.conf	
      2. ![image-20210704215912221](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210705191958.png)
      3. ![image-20210704220232369](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210705191959.png)





---



## jedis

- 通过java代码访问redis数据库，类似于jdbc；



### **maven依赖**

```XML
    <dependency>	   
        <groupId>redis.clients</groupId>
        <artifactId>jedis</artifactId>
        <version>2.9.0</version>
    </dependency>

    <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-pool2</artifactId>
        <version>2.3</version>
    </dependency>

```



### 快速入门

```java
public class Demo1_QuickStart {

    public static void main(String[] args) {
        // 1.获取连接
        Jedis jedis = new Jedis("localhost", 6379);

        // 2. 访问数据库
        jedis.set("username","jedis");

        // 3. 关闭连接(close client)
        jedis.close();
    }
}
```

#### String 

```JAVA
public class Demo2_String {
    public static void main(String[] args) {

        // 1.获取连接 空参创建的jedis默认为 localhost:6379
        Jedis jedis = new Jedis();

        // 2. 操作数据库

        // a.存数据
        jedis.set("username","jedis");

        // b.读数据 (数据不存在，返回null)
        System.out.println(jedis.get("username"));

        // c.设置过期时间 -- 激活码的有效时间
        jedis.setex("activeCode",20,"1234");

        // 3. 关闭连接(close client)
        jedis.close();
    }
}
```

#### Hash

```JAVA
public class Demo3_Hash {
    public static void main(String[] args) {
        // 1.获取连接
        Jedis jedis = new Jedis("localhost", 6379);

        // 2. 访问数据库

        // a.存储对象
        jedis.hset("person","name","张三");
        jedis.hset("person","age","23");

        // b.读取对象  -- getAll 返回一个Map
        Map<String, String> person = jedis.hgetAll("person");
        for (String field : person.keySet()) {
            String value = jedis.hget("person",field);
            System.out.println(field+" : "+value);
        }
        // 3. 关闭连接(close client)
        jedis.del("myHash");
        jedis.close();
    }
}

```

#### List

```JAVA
public class Demo4_List {

    public static void main(String[] args) {

        // 1.获取连接
        Jedis jedis = new Jedis("localhost", 6379);

        // 2. 访问数据库

        // 向myList中插入数据
        jedis.lpush("myList","a","b","c"); // 头插法
        jedis.rpush("myList","a","b","c"); // 尾插法

        // 查询myList数据
        List<String> myList = jedis.lrange("myList", 0, -1);
        System.out.println(myList);

        // 弹出数据
        System.out.println(jedis.rpop("myList"));
        System.out.println(jedis.lrange("myList", 0, -1));


        // 3. 关闭连接(close client)
        jedis.del("myList");
        jedis.close();
    }
}
```

#### Set

```JAVA
public class Demo5_Set {

    public static void main(String[] args) {
        // 1.获取连接
        Jedis jedis = new Jedis("localhost", 6379);

        // 2. 访问数据库

        // 存储集合
        jedis.sadd("mySet","java","python","c++");

        // 获取集合
        Set<String> mySet = jedis.smembers("mySet");
        System.out.println(mySet);

        // 3. 关闭连接(close client)
        jedis.del("mySet");
        jedis.close();

    }
}

```



#### SortedSet

```java
public class Demo6_sortedSet {

    public static void main(String[] args) {
        // 1.获取连接
        Jedis jedis = new Jedis("localhost", 6379);

        // 2. 访问数据库

        jedis.zadd("mySortedSet",1,"apple");
        jedis.zadd("mySortedSet",3,"orange");
        jedis.zadd("mySortedSet",2,"peach");

         // 相当于一个特殊的List : 元素不重复
        Set<String> mySortedSet = jedis.zrange("mySortedSet", 0, -1);
        System.out.println(mySortedSet);

        jedis.del("mySortedSet");
        jedis.close();

    }
}
```

---

### 连接池

#### 简单入门

```JAVA
public class Demo1_quickStart {

    public static void main(String[] args) {

        // 配置连接池
        JedisPoolConfig config = new JedisPoolConfig();
        config.setMaxTotal(50); // 最大连接数
        config.setMaxIdle(10); // 最大空闲数



        // 创建连接池(传入配置对象) 获取 Jedis对象
        JedisPool jedisPool = new JedisPool(config,"localhost",6379);
        Jedis jedis = jedisPool.getResource();

        // 操作数据库
        jedis.set("testKey","hehe");
        System.out.println(jedis.get("testKey"));

        // 关闭
        jedis.del("testKey");
        jedis.close();
    }
}

```

#### 使用Utils 

- 读取配置文件

```JAVA
public class JedisPoolUtils {

    private static JedisPool jedisPool;

    // 读取配置 创建连接池
    static {
        // 创建流
        InputStream is = JedisPool.class.getClassLoader().getResourceAsStream("jedis.properties");

        // 导入properties对象
        Properties pro = new Properties();
        try {
            pro.load(is);
        } catch (IOException e) {
            e.printStackTrace();
        }

        // 读取数据到config对象
        JedisPoolConfig config = new JedisPoolConfig();
        config.setMaxTotal(Integer.parseInt(pro.getProperty("maxTotal")));
        config.setMaxIdle(Integer.parseInt(pro.getProperty("maxIdle")));

        // pro.getProperty()返回String类型
        jedisPool = new JedisPool(config,pro.getProperty("hostname"),Integer.parseInt(pro.getProperty("port")) );
    }



    public static Jedis getJedis(){
        return jedisPool.getResource();
    }


}
```



---

## 案例

> 案例需求：
> 	1. 提供index.html页面，页面中有一个省份 下拉列表
> 	2. 当 页面加载完成后 发送ajax请求，加载所有省份



流程分析：

![image-20210708132159048](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210708132243.png)







首先在mysql数据库中创建province表并插入数据项。

```
create database day23; -- 创建数据库
use day23; -- 使用数据库
create table province( -- 创建表
	id int primary key auto_increment,
	name varchar(20) not null
);

-- 插入数据
insert into province values(null,'北京');
insert into province values(null,'上海');
insert into province values(null,'广州');
insert into province values(null,'陕西');
```



项目在 `F:\javaweb_workspace\_11_redis` 

前端 -> servlet ->service ->dao



缓存优化

![image-20210708193731391](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210708193731.png)





案例省份信息

![案例省份信息](https://kkddyz-oss-image-hosting-service.oss-cn-hangzhou.aliyuncs.com/image/20210708203428.png)

---









---

## 使用总结

在使用redis作为缓存时，常常是将对象格式化为JsonStr，jedis本身并不提供String -> Object的转换



在redis中数据都是string，并不存在类型。 





使用redis缓存导航栏分类时必须使用SortedSet,因为分类的顺序是固定的。











