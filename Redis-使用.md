---
title: Redis-使用
date: 2016-07-15 21:25:14
tags: Redis
---
# Spring-data
spring-xxx.xml  
```xml  
<bean id="redisTemplate" class="org.springframework.data.redis.core.RedisTemplate">
        <property name="connectionFactory" ref="jedisConnectionFactory" />
        <property name="keySerializer">
            <bean
                    class="org.springframework.data.redis.serializer.StringRedisSerializer" />
        </property>
        <property name="valueSerializer">
            <bean
                    class="org.springframework.data.redis.serializer.JdkSerializationRedisSerializer" />
        </property>
    </bean>
```  
Service.java

```java  
@Service
public class DemoService {

    @Resource
    SyslogMapper syslogMapper;
	//  列表操作
	@Resource(name="redisTemplate")
	private ListOperations<String, String> listOps;  
	
	//  哈希操作
	@Resource(name="redisTemplate")
	private HashOperations hashOperations;

	//设置操作
    @Resource(name="redisTemplate")
    private SetOperations<String,Syslog> setOperations;
    public void insertDemo(Syslog syslog) {

        syslog.setSysLogId(new Random().nextInt()+"");

        //写入redis
        setOperations.add("asdasd",syslog);

        //从redis读出
        Syslog tmp = setOperations.pop("asdasd");

        //写入db
        syslogMapper.insertSyslog(tmp);
    }  
}  

```  


