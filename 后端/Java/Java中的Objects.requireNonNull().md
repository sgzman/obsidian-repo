
Jdk1.8中的源码示例

~~~java
/**  
 * Checks that the specified object reference is not {@code null}. This  
 * method is designed primarily for doing parameter validation in methods * and constructors, as demonstrated below: 
 * <blockquote><pre>  
 * public Foo(Bar bar) {  
 *     this.bar = Objects.requireNonNull(bar); * } 
 * </pre></blockquote>  
 *  
 * @param obj the object reference to check for nullity  
 * @param <T> the type of the reference  
 * @return {@code obj} if not {@code null}  
 * @throws NullPointerException if {@code obj} is {@code null}  
 */  
public static <T> T requireNonNull(T obj) {  
    if (obj == null)  
        throw new NullPointerException();  
    return obj;  
}
~~~

Objects.requireNonNull()这份方法主要是为了验证方法、构造函数中的参数是否为null。如果为null则抛出空指针异常，不为null则直接返回定义的T类型


同时源码中还提供了一个预定义的提示消息方法

~~~Java
/**  
 * Checks that the specified object reference is not {@code null} and  
 * throws a customized {@link NullPointerException} if it is. This method  
 * is designed primarily for doing parameter validation in methods and * constructors with multiple parameters, as demonstrated below: 
 * <blockquote><pre>  
 * public Foo(Bar bar, Baz baz) {  
 *     this.bar = Objects.requireNonNull(bar, "bar must not be null");     
 * this.baz = Objects.requireNonNull(baz, "baz must not be null"); 
 * } 
 * </pre></blockquote>  
 *  
 * @param obj     the object reference to check for nullity  
 * @param message detail message to be used in the event that a {@code  
 *                NullPointerException} is thrown  
 * @param <T> the type of the reference  
 * @return {@code obj} if not {@code null}  
 * @throws NullPointerException if {@code obj} is {@code null}  
 */  
public static <T> T requireNonNull(T obj, String message) {  
    if (obj == null)  
        throw new NullPointerException(message);  
    return obj;  
}
~~~

注释中表明了，可以这么使用这个方法

```java
  public Foo(Bar bar, Baz baz) {  
      this.bar = Objects.requireNonNull(bar, "bar must not be null");     
  this.baz = Objects.requireNonNull(baz, "baz must not be null"); 
  } 
```

