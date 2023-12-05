之前一直没用过，也没详细的了解过这个super方法，但是现在也不得不去了解了，那就开始来看一下是怎么用的吧。

super翻译过来是超级的，比如说超人-superman

但是用在代码里我感觉还是有点蒙的，感觉用top、upper和dad都比它好，至少从字面意思还能大概看出来是什么意思

上面是吐槽，下面就言归正传了

看到一个前辈说的是super和this进行了一番对比
this指代的是当前类的实例，即**用于在子类中指代父类对象**，而super则指代的是当前类的父类。这么一说还是挺容易记住的。
使用super加点（.）来调用父类中的变量或者方法啥的，而且在调用的时候就感觉调用的是当前类中的方法一样。而且调用行为不必发生在父类中，它可以自动向上层追溯。

举个例子，就好比用自己父母的车一样自然~

**super 关键字的功能：**
- 访问父类的**方法**。
- 调用父类**构造方法**。
- 访问父类中的**隐藏成员变量**。

下面将使用代码实例来认识这个super

> 子类调用父类中的变量

```Java
public class Father {  
    public String SAYING="这是一个好的示例";  
}
```
``
```Java
public class Son extends Father{  
    public Son(){  
        System.out.println(super.SAYING);  
    }  
}
```

```Java
@Test  
void contextLoads() {  
    System.out.println("我在测试");  
    Son son=new Son();  
}
```

```Java
//输出
我在测试
这是一个好的示例
```

> 子类调用父类中的构造函数（无参/有参）


```Java
public class Father {  
    Father(){  
        System.out.println("这是父类中的构造函数");  
    }  
    Father(String name){  
        System.out.println("你好，我是"+name);  
    }  
}
```

```Java
public class Son extends Father{  
    public Son(){  
        super();  
    }  
  
    public Son(String name){  
        super(name);  
    }  
}
```

```Java
@Test  
void contextLoads() {  
    System.out.println("我在测试");  
    Son son=new Son();  
    Son sonName=new Son("MEAL");  
}
```

```Java
//输出
我在测试
这是父类中的构造函数
你好，我是MEAL
```

> 子类访问父类中的方法

```java
public void doSome(){  
    System.out.println("父类中的方法");  
}
```

```java
public void doSomeInSon(){  
    super.doSome();  
}
```

```java
@Test  
void contextLoads() {  
    System.out.println("我在测试");  
    Son son=new Son();  
    son.doSomeInSon();  
}
```

```java
//输出
我在测试
父类中的方法
```

还可以重写父类中的方法等