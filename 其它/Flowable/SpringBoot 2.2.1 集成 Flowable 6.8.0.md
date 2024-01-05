1、在pom.xml中引入flowable包

```xml
<!-- https://mvnrepository.com/artifact/org.flowable/flowable-spring-boot-starter -->  
<dependency>  
    <groupId>org.flowable</groupId>  
    <artifactId>flowable-spring-boot-starter</artifactId>  
    <version>6.8.0</version>  
</dependency>
```

2、yml配置

```yaml
flowable:  
  async-executor-activate: false #关闭定时任务JOB  
  #  将databaseSchemaUpdate设置为true。当Flowable发现库与数据库表结构不一致时，会自动将数据库表结构升级至新版本。  
  database-schema-update: true
```
