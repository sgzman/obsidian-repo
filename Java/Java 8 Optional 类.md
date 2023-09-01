
Optional是一个**容器对象**，而且是一个可以为<font color="#ff0000">NULL</font>的容器对象。
所以主要用来<font color="#ff0000">解决</font>的问题是臭名昭著的空指针异常（NullPointerException）

它可以保存类型**T**的值，或者仅仅保存null。
Optional提供很多有用的方法，这样我们就不用显式进行空值检测。

Optional 类的引入很好的解决空指针异常。

```java
public final class Optional<T> extends Object

```

