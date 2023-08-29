
jdk1.8中Predicate源码如下：


```java
/*  
 * Copyright (c) 2010, 2013, Oracle and/or its affiliates. All rights reserved.  
 * ORACLE PROPRIETARY/CONFIDENTIAL. Use is subject to license terms.  
 *  
 */  
package java.util.function;  
  
import java.util.Objects;  
  
/**  
 * Represents a predicate (boolean-valued function) of one argument.  
 *  
 * <p>This is a <a href="package-summary.html">functional interface</a>  
 * whose functional method is {@link #test(Object)}.  
 *  
 * @param <T> the type of the input to the predicate  
 *  
 * @since 1.8  
 */  
@FunctionalInterface  
public interface Predicate<T> {  
  
    /**  
     * Evaluates this predicate on the given argument.  
     *  
     * @param t the input argument  
     * @return {@code true} if the input argument matches the predicate,  
     * otherwise {@code false}  
     */  
    boolean test(T t);  
  
    /**  
     * Returns a composed predicate that represents a short-circuiting logical  
     * AND of this predicate and another.  When evaluating the composed  
     * predicate, if this predicate is {@code false}, then the {@code other}  
     * predicate is not evaluated.  
     *  
     * <p>Any exceptions thrown during evaluation of either predicate are relayed  
     * to the caller; if evaluation of this predicate throws an exception, the  
     * {@code other} predicate will not be evaluated.  
     *  
     * @param other a predicate that will be logically-ANDed with this  
     *              predicate  
     * @return a composed predicate that represents the short-circuiting logical  
     * AND of this predicate and the {@code other} predicate  
     * @throws NullPointerException if other is null  
     */  
    default Predicate<T> and(Predicate<? super T> other) {  
        Objects.requireNonNull(other);  
        return (t) -> test(t) && other.test(t);  
    }  
  
    /**  
     * Returns a predicate that represents the logical negation of this  
     * predicate.  
     *  
     * @return a predicate that represents the logical negation of this  
     * predicate  
     */  
    default Predicate<T> negate() {  
        return (t) -> !test(t);  
    }  
  
    /**  
     * Returns a composed predicate that represents a short-circuiting logical  
     * OR of this predicate and another.  When evaluating the composed  
     * predicate, if this predicate is {@code true}, then the {@code other}  
     * predicate is not evaluated.  
     *  
     * <p>Any exceptions thrown during evaluation of either predicate are relayed  
     * to the caller; if evaluation of this predicate throws an exception, the  
     * {@code other} predicate will not be evaluated.  
     *  
     * @param other a predicate that will be logically-ORed with this  
     *              predicate  
     * @return a composed predicate that represents the short-circuiting logical  
     * OR of this predicate and the {@code other} predicate  
     * @throws NullPointerException if other is null  
     */  
    default Predicate<T> or(Predicate<? super T> other) {  
        Objects.requireNonNull(other);  
        return (t) -> test(t) || other.test(t);  
    }  
  
    /**  
     * Returns a predicate that tests if two arguments are equal according  
     * to {@link Objects#equals(Object, Object)}.  
     *  
     * @param <T> the type of arguments to the predicate  
     * @param targetRef the object reference with which to compare for equality,  
     *               which may be {@code null}  
     * @return a predicate that tests if two arguments are equal according  
     * to {@link Objects#equals(Object, Object)}  
     */  
    static <T> Predicate<T> isEqual(Object targetRef) {  
        return (null == targetRef)  
                ? Objects::isNull  
                : object -> targetRef.equals(object);  
    }  
}
```

## 1. Predicate test

`Predicate` 函数接口可以用于判断一个参数是否符合某个条件。

**示例**：判断某个字符串是否为空。

```java
import java.util.function.Predicate;

public class Java8PredicateTest {
    public static void main(String[] args) {
        Predicate<String> isEmpty = String::isEmpty;
        System.out.println(isEmpty.test(""));
        System.out.println(isEmpty.test("www.wdbyte.com"));
    }
}
```

输出结果：

```java
true
false
```

## 2. Predicate Stream filter

`Stream` 中的 `filter()` 方法是通过接收一个 `Predicate` 函数接口实现的。

**示例**：过滤出集合中，字符串长度为 4 的字符串。

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class Java8PredicateFilter {

    public static void main(String[] args) {
        List<String> list = Arrays.asList("java", "node", "www.wdbyte.com");
        list = list.stream().filter(str -> str.length() == 4).collect(Collectors.toList());
        System.out.println(list);
    }
}
```

输出结果：

```java
[java, node]
```

## 3. Predicate and

使用 `and()` 方法，可以让前后两个 `Predicate` 判断条件一起生效。

**示例 1**：过滤数字集合中，数字大小在 5 至 9 之间的数字。

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;

public class Java8PredicateAnd {

    public static void main(String[] args) {
        List<Integer> numberList = Arrays.asList(3, 4, 5, 6, 7, 8, 9, 10);

        Predicate<Integer> greaterThan5 = number -> number > 5;
        Predicate<Integer> lessThan9 = number -> number < 9;
        Predicate<Integer> filter = greaterThan5.and(lessThan9);

        numberList = numberList.stream().filter(filter).collect(Collectors.toList());
        System.out.println(numberList);
    }
}
```

结果输出：

```java
[6, 7, 8]
```

**示例 2**：一个 `Predicate` 过滤数字集合中，数字大小在 5 至 9 之间的数字。

```java
List<Integer> numberList = Arrays.asList(3, 4, 5, 6, 7, 8, 9, 10);
numberList = numberList.stream().filter(x -> x > 5 && x < 9).collect(Collectors.toList());
System.out.println(numberList);
```

输出结果；

```json
[6, 7, 8]
```

## 4. Predicate negate

`predicate.negate()` 方法会返回一个与指定判断相反的 `Predicate`。

**示例**：过滤数字集合中，数字**不**大于 5 的数字。

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;

public class Java8PredicateNeagete {

    public static void main(String[] args) {
        List<Integer> numberList = Arrays.asList(3, 4, 5, 6, 7, 8, 9, 10);
        Predicate<Integer> greaterThan5 = number -> number > 5;

        numberList = numberList.stream().filter(greaterThan5.negate()).collect(Collectors.toList());
        System.out.println(numberList);
    }
}
```


输出结果：

```json
[3, 4, 5]
```

## 5. Predicate or

**示例**：过滤数字集合中，数字小于等于 5，或者大于等于 9 的数字。

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;
import java.util.stream.Collectors;

public class Java8PredicateOr {

    public static void main(String[] args) {
        List<Integer> numberList = Arrays.asList(3, 4, 5, 6, 7, 8, 9, 10);

        Predicate<Integer> lessThan5 = number -> number <= 5;
        Predicate<Integer> greaterThan8 = number -> number >= 9;

        numberList = numberList.stream().filter(lessThan5.or(greaterThan8)).collect(Collectors.toList());
        System.out.println(numberList);
    }
}
```

输出结果：

```json
[3, 4, 5, 9, 10]
```

## 6. Predicate 链式编程

`Predicate` 的 `or()` ，`and()`，`negate()` 方法可以随意组合 `Predicate`，组合后的判断逻辑是从左到右，从前到后，顺次判断。

**如**：（数字小于 5 ）**.and** (数字大于 9 ）**.negate（）**。

**解**：（数字小于 5 ）**AND** (数字大于 9 ） 对于任意数字都得 `false`，`false.negate()` 取相反 得 `true`。

所以，此判断逻辑对于任意数字都为 `true`。

**示例**：`Predicate` 的 `or()` ，`and()`，`negate()` 方法组合使用。

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;

public class Java8PredicateChain {

    public static void main(String[] args) {
        List<Integer> numberList = Arrays.asList(3, 4, 5, 6, 7, 8, 9, 10);

        Predicate<Integer> lessThan5 = number -> number <= 5;
        Predicate<Integer> greaterThan9 = number -> number >= 9;

        // 小于等于 5
        System.out.println(filter(numberList, lessThan5));
        // 大于 5
        System.out.println(filter(numberList, lessThan5.negate()));
        // 小于等于 5 或者大于等于 9
        System.out.println(filter(numberList, lessThan5.or(greaterThan9)));
        // ! (小于等于 5 AND 大于等于 9)
        System.out.println(filter(numberList, lessThan5.and(greaterThan9).negate()));
    }

    public static <T> List<T> filter(List<T> list, Predicate<T> predicate) {
        List<T> resultList = new ArrayList<>();
        for (T t : list) {
            if (predicate.test(t)) {
                resultList.add(t);
            }
        }
        return resultList;
    }
}
```


输出结果：

```json
[3, 4, 5]
[6, 7, 8, 9, 10]
[3, 4, 5, 9, 10]
[3, 4, 5, 6, 7, 8, 9, 10]
```


## 7. Predicate 与对象

**示例**：过滤符合某些特征的狗。

```java
import java.util.ArrayList;
import java.util.List;
import java.util.function.Predicate;

public class Java8PredicateObject {

    public static void main(String[] args) {
        List<Dog> dogList = new ArrayList<>();
        dogList.add(new Dog("哈士奇", 1));
        dogList.add(new Dog("牧羊犬", 2));
        dogList.add(new Dog("柯基", 3));
        dogList.add(new Dog("柴犬", 3));

        // 找到 3岁的狗
        System.out.println(filter(dogList, dog -> dog.getAge().equals(3)));
        // 找到哈士奇信息
        Predicate<Dog> predicate = dog -> ("哈士奇").equals(dog.getName());
        System.out.println(filter(dogList, predicate));
    }

    public static <T> List<T> filter(List<T> list, Predicate<T> predicate) {
        List<T> resultList = new ArrayList<>();
        for (T t : list) {
            if (predicate.test(t)) { resultList.add(t); }
        }
        return resultList;
    }
}

class Dog {
    private String name;
    private Integer age;

    public Dog(String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Dog{" +
            "name='" + name + '\'' +
            ", age=" + age +
            '}';
    }
}
```

输出结果：

```json
[Dog{name='柯基', age=3}, Dog{name='柴犬', age=3}]
[Dog{name='哈士奇', age=1}]
```

