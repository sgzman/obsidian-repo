
这两个方法都是获取类的全部字段

只是
- getDeclaredFields()只获取自己类中的字段
- getField()还可以获取父类中的字段

**注意：**
如果字段是private的话，那只能先设置setAccessible(true)，再去获取