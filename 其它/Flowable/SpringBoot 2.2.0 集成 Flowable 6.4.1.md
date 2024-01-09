[springboot和flowable对应说明](https://blog.csdn.net/linfen1520/article/details/103213059)

参考如下链接进行flowable集成
[Flowable 快速入门教程：SpringBoot 集成 Flowable + Flowable Modeler 流程配置可视化（超详细）_flowable-ui-modeler-CSDN博客](https://blog.csdn.net/qq_37143673/article/details/102851433?spm=1001.2014.3001.5501)




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