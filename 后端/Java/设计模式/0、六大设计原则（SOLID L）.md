
我觉得这篇公众号讲的挺好的，链接在下面
[为什么你总是觉得设计模式很难？. (qq.com)](https://mp.weixin.qq.com/s/Phiab8CD80Zzqr3rCPe5vw)

1. 单一职责原则（Single Responsibility Principle）
2. 开闭原则（Open Closed Principle）
3. 里氏替换原则（Liskov Substitution Principle）
4. 接口隔离原则（Interface Segregation Principle）
5. 依赖倒置原则（Dependence Inversion Principle）
7. 迪米特原则（Law of Demeter）
### 一、单一职责原则（Single Responsibility Principle）

> 一个类只负责一个功能领域中的相应职责

- 对于类来说，即一个类应该只负责一项职责。如果A类负责两个不同的职责：职责1、职责2。当职责1发生变化而改变A时，可能会对职责2造成影响使职责2运行错误，所以需要将类A的粒度分解为A1、A2。
- 如果在类中没有满足单一职责原则，在一个类的方法中遵守单一职责原则也是可以的(交通工具)
- 标准的单一职责原则，是在类的级别上进行拆分，而不是方法级别。
- 通常情况下，我们要遵守单一职责原则，只有当逻辑足够简单，才可以在代码级别违反单一职责原则；只有类中的方法数量足够少，可以在方法级别保持单一职责原则。
- 优秀的代码中使用类来区分多个分支，而不使用 if...else if()....else(耦合度高)


### 二、开闭原则（Open Closed Principle）

> 一个软件实体如类、模块和函数应该对扩展开放，对修改关闭

也就是说，一个软件实体应该通过扩展来实现变化，而不是通过修改已有的代码实现变化。这是为软件实体的未来事件而制定的对现行开发设计进行约束的一个原则。

在我们编码的过程中，需求变化是不断的发生的，当我们需要对代码进行修改时，我们应该尽量做到能不动原来的代码就不动，通过扩展的方式来满足需求。

遵循开闭原则的最好手段就是**抽象**（接口或者抽象类），例如一个工程师类，我们说的是把方法抽离成单独的类，每个类负责单一的职责，但其实从开闭原则的角度说，更好的方式是把职责设计成接口，例如把写代码的职责方法抽离成接口的形式，同时，我们在设计之初需要考虑到未来所有可能发生变化的因素，比如未来有可能因为业务需要分成后台和前端的功能，这时设计之初就可以设计成两个接口，

```java
public interface BackCode{
	void writeCode();
}
```

```java
public interface FrontCode{
	void writeCode();
}
```

如果将来前端代码的业务发生变化，我们只需扩展前端接口的功能，或者修改前端接口的实现类即可，后台接口以及实现类就不会受到影响，这就是抽象的好处。


### 三、里氏替换原则（Liskov Substitution Principle）


里氏替换原则，英文名Liskov Substitution Principle，它的定义是

> 如果对每一个类型为T1的对象o1，都有类型为T2的对象o2，使得以T1定义的所有程序P在所有对象o1都替换成o2的时候，程序P的行为都没有发生变化，那么类型T2是类型T1的子类型。

看起来有点绕口，它还有一个简单的定义：

> 所有引用基类的地方必须能够透明地使用其子类的对象。

通俗点说，只要父类能出现的地方子类就可以出现，而且替换为子类也不会产生任何异常。 但是反过来就不行了，因为子类可以扩展父类没有的功能，同时子类还不能改变父类原有的功能。

我们都知道，面向对象的三大特征是封装、继承和多态，这三者缺一不可，但三者之间却并不 “和谐“。因为继承有很多缺点，当子类继承父类时，虽然可以复用父类的代码，但是父类的属性和方法对子类都是透明的，子类可以随意修改父类的成员。如果需求变更，子类对父类的方法进行了一些复写的时候，其他的子类可能就需要随之改变，这在一定程度上就违反了封装的原则，解决的方案就是引入里氏替换原则。

里氏替换原则为良好的继承定义了一个规范，它包含了4层含义：

1、子类可以实现父类的抽象方法，但是不能覆盖父类的非抽象方法。

2、子类可以有自己的个性，可以有自己的属性和方法。

3、子类覆盖或重载父类的方法时输入参数可以被放大。

比如父类有一个方法，参数是**HashMap**

```java
public class Father {
    public void test(HashMap map){
        System.out.println("父类被执行。。。。。");
    }
}
```

那么子类的同名方法输入参数的类型可以扩大，例如我们输入参数为**Map**，

```java
public class Son extends Father{
    public void test(Map map){
        System.out.println("子类被执行。。。。");
    }
}
```

我们写一个场景类测试一下父类的方法执行效果，

```java
public class Client {
    public static void main(String[] args) {
        Father father = new Father();
        HashMap map = new HashMap();
        father.test(map);
    }
}
```

结果输出：父类被执行。。。。。

因为里氏替换原则，只要父类能出现的地方子类就可以出现，而且替换为子类也不会产生任何异常。我们改下代码，调用子类的方法，

```java
public class Client {
    public static void main(String[] args) {
        Son son = new Son();
        HashMap map = new HashMap();
        father.test(map);
    }
}
```

运行结果是一样的，因为子类方法的输入参数类型范围扩大了，子类代替父类传递到调用者中，子类的方法永远不会被执行，这样的结果其实是正确的，如果想让子类方法执行，可以重写方法体。

反之，如果子类的输入参数类型范围比父类还小，比如父类中的参数是Map，而子类是HashMap，那么执行上述代码的结果就会是子类的方法体，有人说，这难道不对吗？子类显示自己的内容啊。其实这是不对的，因为子类没有复写父类的同名方法，方法就被执行了，这会引起逻辑的混乱，如果父类是抽象类，子类是实现类，你传递一个这样的实现类就违背了父类的意图了，容易引起逻辑混乱，所以子类覆盖或重载父类的方法时输入参数必定是相同或者放大的。

4、子类覆盖或重载父类的方法时输出结果可以被缩小，也就是说返回值要小于或等于父类的方法返回值。

确保程序遵循里氏替换原则可以要求我们的程序建立抽象，通过抽象去建立规范，然后用实现去扩展细节，所以，它跟开闭原则往往是相互依存的


### 四、依赖倒置原则（Dependence Inversion Principle）


依赖倒置原则，Dependence Inversion Principle，简称DIP，它的定义是：

> 高层模块不应该依赖底层模块，两者都应该依赖其抽象；
> 
> 抽象不应该依赖细节；
> 
> 细节应该依赖抽象；

什么是高层模块和底层模块呢？不可分割的原子逻辑就是底层模块，原子逻辑的再组装就是高层模块。

在Java语言中，抽象就是指接口或抽象类，两者都不能被实例化；而细节就是实现接口或继承抽象类产生的类，也就是可以被实例化的实现类。依赖倒置原则是指模块间的依赖是通过抽象来发生的，实现类之间不发生直接的依赖关系，其依赖关系是通过接口是来实现的，这就是俗称的面向接口编程。

我们用歌手唱歌来举例，比如一个歌手唱国语歌，用代码表示就是：

```Java
public class ChineseSong {
    public String language() {
        return "国语歌";
    }
}
public class Singer {
    //唱歌的方法
    public void sing(ChineseSong song) {
        System.out.println("歌手" + song.language());
    }
}
public class Client {
    public static void main(String[] args) {
        Singer singer = new Singer();
        ChineseSong song = new ChineseSong();
        singer.sing(song);
    }
}
```

运行main方法，结果就会输出：歌手唱国语歌

现在，我们需要给歌手加一点难度，比如说唱英文歌，在这个类中，我们发现是很难做的。因为我们Singer类依赖于一个具体的实现类ChineseSong，也许有人会说可以在加一个方法啊，但这样一来我们就修改了Singer类了，如果以后需要增加更多的歌种，那歌手类不是一直要被修改？也就是说，依赖类已经不稳定了，这显然不是我们想看到的。

所以我们需要用面向接口编程的思想来优化我们的方案，改成如下的代码：

```Java
public interface Song {
    public String language();
}
public class ChineseSong implements Song{
    public String language() {
        return "唱国语歌";
    }
}
public class EnglishSong implements Song {
    public String language() {
        return "唱英语歌";
    }
}
public class Singer {
    //唱歌的方法
    public void sing(Song song) {
        System.out.println("歌手" + song.language());
    }
}
public class Client {
    public static void main(String[] args) {
        Singer singer = new Singer();
        EnglishSong englishSong = new EnglishSong();
        // 唱英文歌
        singer.sing(englishSong);
    }
}
```

我们把歌单独抽成一个接口`Song`，每个歌种都实现该接口并重写方法，这样一来，歌手的代码不必改动，如果需要添加歌的种类，只需写多一个实现类继承`Song`即可。

通过这样的面向接口编程，我们的代码就有了更好的扩展性，同时也降低了耦合，提高了系统的稳定性。

### 五、接口隔离原则（Interface Segregation Principle）

> 客户端不应该依赖它不需要的接口

意思就是客户端需要什么接口就提供什么接口，把不需要的接口剔除掉，这就需要对接口进行细化，保证接口的纯洁性。换成另一种说法就是，类间的依赖关系应该建立在最小的接口上，也就是建立单一的接口。

你可能会疑惑，建立单一接口，这不是单一职责原则吗？其实不是，单一职责原则要求的是类和接口职责单一，注重的是职责，一个职责的接口是可以有多个方法的，而接口隔离原则要求的是接口的方法尽量少，模块尽量单一，如果需要提供给客户端很多的模块，那么就要相应的定义多个接口，不要把所有的模块功能都定义在一个接口中，那样会显得很臃肿。

举个例子，现在的智能手机非常的发达，几乎是人手一部的社会状态，在我们年轻人的观念里，好的智能手机应该是价格便宜，外观好看，功能丰富的，由此我们可以定义一个智能手机的抽象接口 **ISmartPhone**，代码如下所示：

```java
public interface ISmartPhone {
    public void cheapPrice();
    public void goodLooking();
    public void richFunction();
}
```

接着，我们定义一个手机接口的实现类，实现这三个抽象方法，

```java
public class SmartPhone implements ISmartPhone{
    public void cheapPrice() {
        System.out.println("这手机便宜~~~~~");
    }

    public void goodLooking() {
        System.out.println("这手机外观好看~~~~~");
    }

    public void richFunction() {
        System.out.println("这手机功能真多~~~~~");
    }
}
```

然后，定义一个用户的实体类 **User**，并定义一个构造方法，以**ISmartPhone** 作为参数传入，同时，我们也定义一个使用的方法**usePhone** 来调用接口的方法，

```java
public class User {

    private ISmartPhone phone;
    public User(ISmartPhone phone){
        this.phone = phone;
    }
    public void usePhone(){
        phone.cheapPrice();
        phone.goodLooking();
        phone.richFunction();
    }
}
```

可以看出，当我们实例化`User`类并调用其方法`usePhone`后，控制台上就会显示手机接口三个方法的方法体信息，这种设计看上去没什么大毛病，但是我们可以仔细想下，**ISmartPhone**这个接口的设计是否已经达到最优了呢？很遗憾，答案是没有，接口其实还可以再优化。

因为除了年轻人之外，中年商务人士也在用智能手机，在他们的观念里，智能手机并不需要丰富的功能，甚至不用考虑是否便宜 (有钱就是任性)，因为成功人士都比较忙，对智能手机的要求大多是外观大气，功能简单即可，这才是他们心中好的智能手机的特征，这样一来，我们定义的 **ISmartPhone** 接口就无法适用了，因为我们的接口定义了智能手机必须满足三个特性，如果实现该接口就必须三个方法都实现，而对商务人员的标准来说，我们定义的方法只有外观符合且可以重用而已。你可能会说，我可以重写一个实现类啊，只实现外观的方法，另外两个方法置空，什么都不写，这不就行了吗？但是这也不行，因为 **User** 引用的是**ISmartPhone** 接口，它调用三个方法，你只实现了两个，那么打印信息就少了两条了，只靠外观的特性，使用者怎么知道智能手机是否符合自己的预期？

分析到这里，我们大概就明白了，其实**ISmartPhone**的设计是有缺陷的，过于臃肿了，按照接口隔离原则，我们可以根据不同的特性把智能手机的接口进行拆分，这样一来，每个接口的功能就会变得单一，保证了接口的纯洁性，也进一步提高了代码的灵活性和稳定性。


### 六、迪米特原则（Law of Demeter）

迪米特原则，Law of Demeter，简称LoD，也被称为最少知识原则，它描述的规则是：

> 一个对象应该对其他对象有最少的了解

也就是说，一个类应该对自己需要耦合或调用的类知道的最少，类与类之间的关系越密切，耦合度越大，那么类的变化对其耦合的类的影响也会越大，这也是我们面向设计的核心原则：低耦合，高内聚。

迪米特法则还有一个解释：只与直接的朋友通信。

什么是直接的朋友呢？每个对象都必然与其他对象有耦合关系，两个对象的耦合就成为朋友关系，这种关系的类型很多，例如组合、聚合、依赖等。其中，我们称出现成员变量、方法参数、方法返回值中的类为直接的朋友，而出现在局部变量中的类则不是直接的朋友。也就是说，陌生的类最好不要作为局部变量的形式出现在类的内部。

举个例子，上体育课之前，老师让班长先去体务室拿20个篮球，等下上课的时候要用。根据这一场景，我们可以设计出三个类 **Teacher**(老师)，**Monitor** (班长) 和 **BasketBall** (篮球)，以及发布命令的方法`command` 和 拿篮球的方法`takeBall`，

```java
public class Teacher {
    // 命令班长去拿球
    public void command(Monitor monitor) {
        List<BasketBall> ballList = new ArrayList<BasketBall>();
        // 初始化篮球数目
        for (int i = 0;i<20;i++){
            ballList.add(new BasketBall());
        }
        // 通知班长开始去拿球
        monitor.takeBall(ballList);
    }
}
public class BasketBall {
}
public class Monitor {
    // 拿球
    public void takeBall(List<BasketBall> balls) {
        System.out.println("篮球数目：" + balls.size());
    }
}
```

然后，我们写一个情景类进行测试：

```java
public class Client {
    public static void main(String[] args) {
        Teacher teacher = new Teacher();
        teacher.command(new Monitor());
    }
}
```

结果显示如下：

```java
篮球数目：20
```

虽然结果是正确的，但我们的程序其实还是存在问题，因为从场景来说，老师只需命令班长拿篮球即可，Teacher只需要一个朋友----Monitor，但在程序里，Teacher的方法体中却依赖了BasketBall类，也就是说，Teacher类与一个陌生的类有了交流，这样Teacher的健壮性就被破坏了，因为一旦BasketBall类做了修改，那么Teacher也需要做修改，这很明显违背了迪米特法则。

因此，我们需要对程序做些修改，在Teacher的方法中去掉对BasketBall类的依赖，只让Teacher类与朋友类Monitor产生依赖，修改后的代码如下：

```java
public class Teacher {
    // 命令班长去拿球
    public void command(Monitor monitor) {
        // 通知班长开始去拿球
        monitor.takeBall();
    }
}
public class Monitor {
    // 拿球
    public void takeBall() {
        List<BasketBall> ballList = new ArrayList<BasketBall>();
        // 初始化篮球数目
        for (int i = 0;i<20;i++){
            ballList.add(new BasketBall());
        }
        System.out.println("篮球数目：" + ballList.size());
    }
}
```

这样一来，Teacher类就不会与BasketBall类产生依赖了，即时日后因为业务需要修改BasketBall也不会影响Teacher类。

