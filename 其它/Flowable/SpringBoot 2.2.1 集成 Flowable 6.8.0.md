1、在pom.xml中引入flowable包

```xml
<properties>  
    <flowable.version>6.8.0</flowable.version>  
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
flowable:  
  async-executor-activate: false #关闭定时任务JOB  
  #  将databaseSchemaUpdate设置为true。当Flowable发现库与数据库表结构不一致时，会自动将数据库表结构升级至新版本。  
  database-schema-update: true
```

3、在pom.xml中添加flowable ui 包

```xml
<!--flowable UI设计器-->  
<dependency>  
    <groupId>org.flowable</groupId>  
    <artifactId>flowable-spring-boot-starter-ui-modeler</artifactId>  
    <version>${flowable.version}</version>  
</dependency>  
<dependency>  
    <groupId>org.flowable</groupId>  
    <artifactId>flowable-spring-boot-starter-ui-admin</artifactId>  
    <version>${flowable.version}</version>  
</dependency>  
<dependency>  
    <groupId>org.flowable</groupId>  
    <artifactId>flowable-spring-boot-starter-ui-idm</artifactId>  
    <version>${flowable.version}</version>  
</dependency>  
<dependency>  
    <groupId>org.flowable</groupId>  
    <artifactId>flowable-spring-boot-starter-ui-task</artifactId>  
    <version>${flowable.version}</version>  
</dependency>
```









可能遇到的问题：
1、ORA-00955: 名称已由现有对象使用
这是因为执行建表的Sql时出错了，我的解决办法是手动删除已建立的部分ACT_和FLW_表，再去把war包中的Sql语句拿到数据库中执行。

2、完成上面1、2 后，启动系统无法登录进入系统


3、运行项目时，command line is too long。Shorten the command line via JAR manifest or via a classpath file and rerun。