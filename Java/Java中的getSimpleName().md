
在Java中，`getSimpleName()` 方法是从 `java.lang.Class` 类继承而来的一个方法，用于获取类的简单名称（不包括包名）的字符串表示。以下是一个示例：

```java
public class ExampleClass {
    // 类的成员和方法...
}

public class Main {
    public static void main(String[] args) {
        ExampleClass example = new ExampleClass();
        Class<?> clazz = example.getClass();

        String simpleName = clazz.getSimpleName();
        System.out.println("Simple Name of the Class: " + simpleName);
    }
}

```

在这个示例中，`clazz.getSimpleName()` 会返回字符串 `"ExampleClass"`，因为 `ExampleClass` 是类的简单名称。请确保在实际代码中替换为你想要获取简单名称的类。