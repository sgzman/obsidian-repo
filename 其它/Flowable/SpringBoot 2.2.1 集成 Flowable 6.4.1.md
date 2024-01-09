[springboot和flowable对应说明](https://blog.csdn.net/linfen1520/article/details/103213059)

1、在pom.xml中引入flowable包

```xml
<properties>  
    <flowable.version>6.4.1</flowable.version>  
</properties>

<!-- https://mvnrepository.com/artifact/org.flowable/flowable-spring-boot-starter -->  
<dependency>  
    <groupId>org.flowable</groupId>  
    <artifactId>flowable-spring-boot-starter</artifactId>  
    <version>${flowable.version}</version>  
</dependency>
```

2、yml配置

```yaml
# flowable 配置
flowable:
  # 关闭异步，不关闭历史数据的插入就是异步的，会在同一个事物里面，无法回滚
  # 开发可开启会提高些效率，上线需要关闭
  async-executor-activate: false
```

3、在pom.xml中添加flowable ui 包

```xml 
<!-- flowable 集成依赖 rest，logic，conf -->  
<dependency>  
    <groupId>org.flowable</groupId>  
    <artifactId>flowable-ui-modeler-rest</artifactId>  
    <version>${flowable.version}</version>  
</dependency>  
<dependency>  
    <groupId>org.flowable</groupId>  
    <artifactId>flowable-ui-modeler-logic</artifactId>  
    <version>${flowable.version}</version>  
</dependency>  
<dependency>  
    <groupId>org.flowable</groupId>  
    <artifactId>flowable-ui-modeler-conf</artifactId>  
    <version>${flowable.version}</version>  
</dependency>
```


4、添加一个配置类

```java
package com.etc.eip.config;  
  
import lombok.Data;  
import org.flowable.engine.ProcessEngine;  
import org.flowable.engine.ProcessEngineConfiguration;  
import org.flowable.engine.impl.cfg.StandaloneProcessEngineConfiguration;  
import org.slf4j.Logger;  
import org.slf4j.LoggerFactory;  
import org.springframework.beans.factory.annotation.Value;  
import org.springframework.boot.context.properties.ConfigurationProperties;  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  
import org.springframework.context.annotation.Primary;  
  
/**  
 * 流程引擎配置文件  
 * @author: linjinp  
 * @create: 2019-10-21 16:49  
 **/@Configuration  
@ConfigurationProperties(prefix = "spring.datasource.druid")  
@Data  
public class ProcessEngineConfig {  
  
    private Logger logger = LoggerFactory.getLogger(ProcessEngineConfig.class);  
  
    @Value("${spring.datasource.druid.url}")  
    private String url;  
  
    @Value("${spring.datasource.druid.driver-class-name}")  
    private String driverClassName;  
  
    @Value("${spring.datasource.druid.username}")  
    private String username;  
  
    @Value("${spring.datasource.druid.password}")  
    private String password;  
  
    @Value("${spring.jpa.properties.hibernate.default_schema:}")  
    private String defaultSchema;  
    /**  
     * 初始化流程引擎  
     * @return  
     */  
    @Primary  
    @Bean(name = "processEngine")  
    public ProcessEngine initProcessEngine() {  
        logger.info("=============================ProcessEngineBegin=============================");  
  
        // 流程引擎配置  
        ProcessEngineConfiguration cfg = null;  
  
        try {  
            cfg = new StandaloneProcessEngineConfiguration()  
                    .setJdbcUrl(url)  
                    .setJdbcUsername(username)  
                    .setJdbcPassword(password)  
                    .setJdbcDriver(driverClassName)  
                    // 初始化基础表，不需要的可以改为 DB_SCHEMA_UPDATE_FALSE                    .setDatabaseSchemaUpdate(ProcessEngineConfiguration.DB_SCHEMA_UPDATE_TRUE)  
                    // 默认邮箱配置  
                    // 发邮件的主机地址，先用 QQ 邮箱  
                    .setMailServerHost("smtp.qq.com")  
                    // POP3/SMTP服务的授权码  
                    .setMailServerPassword("hergynqfusnsbbdf")  
                    // 默认发件人  
                    .setMailServerDefaultFrom("836369078@qq.com")  
                    // 设置发件人用户名  
                    .setMailServerUsername("管理员")  
                    // 解决流程图乱码  
                    .setActivityFontName("宋体")  
                    .setLabelFontName("宋体")  
                    .setAnnotationFontName("宋体");  
            //设置一下setDatabaseSchema  
            if (!(defaultSchema.equals("null")||defaultSchema.isEmpty())){  
                cfg.setDatabaseSchema(defaultSchema);  
            }  
        } catch (Exception e) {  
            e.printStackTrace();  
        }  
        // 初始化流程引擎对象  
        ProcessEngine processEngine = cfg.buildProcessEngine();  
        logger.info("=============================ProcessEngineEnd=============================");  
        return processEngine;  
    }  
}
```


5、








可能遇到的问题：
1、ORA-00955: 名称已由现有对象使用
这是因为执行建表的Sql时出错了，我的解决办法是手动删除已建立的部分ACT_和FLW_表，再去把war包中的Sql语句拿到数据库中执行。

2、完成上面1、2 后，启动系统无法登录进入系统


3、运行项目时，command line is too long。Shorten the command line via JAR manifest or via a classpath file and rerun。
根据搜索引擎得知在.idea文件夹中的workspace.xml中的PropertiesComponent下面添加"dynamic.classpath": "true"

4、forcing the use of CGLib-based proxies by setting proxyTargetClass=true on @EnableAsync and/or @EnableCaching.
在对应的配置类上面添加下面两个注解
@EnableCaching(proxyTargetClass = true) //开启支持缓存的注解 并基于类进行代理
@EnableAsync(proxyTargetClass = true) //开启对异步任务的支持 

5、SELECT LOCKED FROM EIPUSER.ACT_CO_DATABASECHANGELOGLOCK WHERE ID=1 FOR UPDATE
在数据库中执行

```sql
select o.owner,  
       o.object_name,  
       s.username,  
       l.object_id,  
       l.session_id,  
       s.serial#,  
       s.lockwait,  
       s.status,  
       s.machine,  
       s.program  
  from v$session s, v$locked_object l, dba_objects o  
 where s.sid = l.session_id  
   and l.object_id = o.object_id;
```

显示为1
执行下列Sql进行解锁

```sql
alter system kill session 'sid,serial';  
  
alter system kill session '74,25391' immediate;
```

5、控制台报出下列错误
~~~java
Error creating bean with name 'idmSecurityConfiguration': Unsatisfied dependency expressed through field 'identityService'; nested exception is org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'idmIdentityService' defined in class path resource [org/flowable/spring/boot/idm/IdmEngineServicesAutoConfiguration.class]: Unsatisfied dependency expressed through method 'idmIdentityService' parameter 0; nested exception is org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'idmEngine' defined in class path resource [org/flowable/spring/boot/idm/IdmEngineServicesAutoConfiguration$AlreadyInitializedAppEngineConfiguration.class]: Unsatisfied dependency expressed through method 'idmEngine' parameter 0; nested exception is org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'flowableAppEngine': FactoryBean threw exception on object creation; nested exception is org.flowable.common.engine.api.FlowableException: Error initialising content data model
~~~

在