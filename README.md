## Redis
redis是一款开源的Key-Value数据库，运行在内存中，由ANSI C编写。企业开发通常采用Redis来实现缓存。同类的产品还有memcache 、memcached 、MongoDB等。
## Jedis
Jedis是Redis官方推出的一款面向Java的客户端，提供了很多接口供Java语言调用。可以在Redis官网下载，当然还有一些开源爱好者提供的客户端，如Jredis、SRP等等，推荐使用Jedis。
## Spring Data Redis
Spring-data-redis是spring大家族的一部分，提供了在srping应用中通过简单的配置访问redis服务，对reids底层开发包(Jedis,  JRedis, and RJC)进行了高度封装，RedisTemplate提供了redis各种操作、异常处理及序列化，支持发布订阅，并对spring 3.1 cache进行了实现。
spring-data-redis针对jedis提供了如下功能：<br>
* 连接池自动管理，提供了一个高度封装的“RedisTemplate”类
* 针对jedis客户端中大量api进行了归类封装,将同一类型操作封装为operation接口
   * ValueOperations：简单K-V操作
   * SetOperations：set类型数据操作
   * ZSetOperations：zset类型数据操作
   * HashOperations：针对map类型的数据操作
   * ListOperations：针对list类型的数据操作
##Spring Data Redis入门小Demo
4.5.1准备工作
* 依赖 <br>
```xml
<dependency> 
		  <groupId>redis.clients</groupId> 
		  <artifactId>jedis</artifactId> 
		  <version>2.8.1</version> 
</dependency> 
<dependency> 
		  <groupId>org.springframework.data</groupId> 
		  <artifactId>spring-data-redis</artifactId> 
		  <version>1.7.2.RELEASE</version> 
</dependency>	
```
* 在src/main/resources下创建properties文件夹，建立redis-config.properties <br>
```
redis.host=127.0.0.1 
redis.port=6379 
redis.pass= 
redis.database=0 
redis.maxIdle=300 
redis.maxWait=3000 
redis.testOnBorrow=true 
```
* 在src/main/resources下创建spring文件夹 ，创建applicationContext-redis.xml <br>
```xml
   <context:property-placeholder location="classpath*:properties/*.properties" />   
   <!-- redis 相关配置 --> 
   <bean id="poolConfig" class="redis.clients.jedis.JedisPoolConfig">  
     <property name="maxIdle" value="${redis.maxIdle}" />   
     <property name="maxWaitMillis" value="${redis.maxWait}" />  
     <property name="testOnBorrow" value="${redis.testOnBorrow}" />  
   </bean>  
   <bean id="JedisConnectionFactory" class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory" 
       p:host-name="${redis.host}" p:port="${redis.port}" p:password="${redis.pass}" p:pool-config-ref="poolConfig"/>  
   
   <bean id="redisTemplate" class="org.springframework.data.redis.core.RedisTemplate">  
     <property name="connectionFactory" ref="JedisConnectionFactory" />  
   </bean>  
```
   
maxIdle ：最大空闲数 <br>
maxWaitMillis:连接时的最大等待毫秒数<br>
testOnBorrow：在提取一个jedis实例时，是否提前进行验证操作；如果为true，则得到的jedis实例均是可用的；<br>

