#java enum的用法详解
##Java Enum原理 
```
public enum Size{ SMALL, MEDIUM, LARGE, EXTRA_LARGE };
```
实际上，这个声明定义的类型是一个类，它刚好有四个实例，在此尽量不要构造新对象。

因此，在比较两个枚举类型的值时，永远不需要调用equals方法，而直接使用"=="就可以了。(equals()方法也是直接使用==,  两者是一样的效果)

Java Enum类型的语法结构尽管和java类的语法不一样，应该说差别比较大。但是经过编译器编译之后产生的是一个class文件。该class文件经过反编译可以看到实际上是生成了一个类，该类继承了java.lang.Enum<E>。

  例如：


```
public enum WeekDay { 
     Mon("Monday"), Tue("Tuesday"), Wed("Wednesday"), Thu("Thursday"), Fri( "Friday"), Sat("Saturday"), Sun("Sunday"); 
     private final String day; 
     private WeekDay(String day) { 
            this.day = day; 
     } 
    public static void printDay(int i){ 
       switch(i){ 
           case 1: System.out.println(WeekDay.Mon); break; 
           case 2: System.out.println(WeekDay.Tue);break; 
           case 3: System.out.println(WeekDay.Wed);break; 
            case 4: System.out.println(WeekDay.Thu);break; 
           case 5: System.out.println(WeekDay.Fri);break; 
           case 6: System.out.println(WeekDay.Sat);break; 
            case 7: System.out.println(WeekDay.Sun);break; 
           default:System.out.println("wrong number!"); 
         } 
     } 
    public String getDay() { 
        return day; 
     } 
}

```

WeekDay经过反编译(javap WeekDay命令)之后得到的内容如下(去掉了汇编代码)：

```
	public final class WeekDay extends java.lang.Enum{ 
	    public static final WeekDay Mon; 
	    public static final WeekDay Tue; 
	    public static final WeekDay Wed; 
	    public static final WeekDay Thu; 
	    public static final WeekDay Fri; 
	    public static final WeekDay Sat; 
	    public static final WeekDay Sun; 
	    static {}; 
	    public static void printDay(int); 
	    public java.lang.String getDay(); 
	    public static WeekDay[] values(); 
	    public static WeekDay valueOf(java.lang.String); 
	}
```

用法一：常量

在JDK1.5 之前，我们定义常量都是： public static fianl。现在好了，有了枚举，可以把相关的常量分组到一个枚举类型里，而且枚举提供了比常量更多的方法。

```
		public enum Color {  
		  RED, GREEN, BLANK, YELLOW  
		}	
```



用法二：switch

JDK1.6之前的switch语句只支持int,char,enum类型，使用枚举，能让我们的代码可读性更强。

```
enum Signal {
        GREEN, YELLOW, RED
    }

    public class TrafficLight {
        Signal color = Signal.RED;

        public void change() {
            switch (color) {
            case RED:
                color = Signal.GREEN;
                break;
            case YELLOW:
                color = Signal.RED;
                break;
            case GREEN:
                color = Signal.YELLOW;
                break;
            }
        }
    }
```
用法三：向枚举中添加新方法

如果打算自定义自己的方法，那么必须在enum实例序列的最后添加一个分号。而且 Java 要求必须先定义 enum 实例。

```
public enum Color {
    RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4);
    // 成员变量
    private String name;
    private int index;

    // 构造方法
    private Color(String name, int index) {
        this.name = name;
        this.index = index;
    }

    // 普通方法
    public static String getName(int index) {
        for (Color c : Color.values()) {
        if (c.getIndex() == index) {
            return c.name;
        }
        }
        return null;
    }

    // get set 方法
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getIndex() {
        return index;
    }

    public void setIndex(int index) {
        this.index = index;
    }
    }
```
用法四：覆盖枚举的方法

下面给出一个toString()方法覆盖的例子。

```
public class Test {
    public enum Color {
        RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4);
        // 成员变量
        private String name;
        private int index;

        // 构造方法
        private Color(String name, int index) {
            this.name = name;
            this.index = index;
        }

        // 覆盖方法
        @Override
        public String toString() {
            return this.index + "_" + this.name;
        }
    }

    public static void main(String[] args) {
        System.out.println(Color.RED.toString());
    }
}
```
用法五：实现接口

所有的枚举都继承自java.lang.Enum类。由于Java 不支持多继承，所以枚举对象不能再继承其他类。

```
public interface Behaviour {
    void print();

    String getInfo();
    }

    public enum Color implements Behaviour {
    RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4);
    // 成员变量
    private String name;
    private int index;

    // 构造方法
    private Color(String name, int index) {
        this.name = name;
        this.index = index;
    }

    // 接口方法

    @Override
    public String getInfo() {
        return this.name;
    }

    // 接口方法
    @Override
    public void print() {
        System.out.println(this.index + ":" + this.name);
    }
    }
```
用法六：使用接口组织枚举 

```
public interface Food {
        enum Coffee implements Food {
            BLACK_COFFEE, DECAF_COFFEE, LATTE, CAPPUCCINO
        }

        enum Dessert implements Food {
            FRUIT, CAKE, GELATO
        }
    }
```
用法七：关于枚举集合的使用

java.util.EnumSet和java.util.EnumMap是两个枚举集合。EnumSet保证集合中的元素不重复;EnumMap中的 key是enum类型，而value则可以是任意类型。关于这个两个集合的使用就不在这里赘述，

可以参考JDK文档

三、 完整示例代码

枚举类型的完整演示代码如下：

```
public class LightTest {

    // 1.定义枚举类型

    public enum Light {

    // 利用构造函数传参

    RED(1), GREEN(3), YELLOW(2);

    // 定义私有变量

    private int nCode;

    // 构造函数，枚举类型只能为私有

    private Light(int _nCode) {

        this.nCode = _nCode;

    }

    @Override
    public String toString() {

        return String.valueOf(this.nCode);

    }

    }

    /**
     * 
     * @param args
     */

    public static void main(String[] args) {

    // 1.遍历枚举类型

    System.out.println("演示枚举类型的遍历 ......");

    testTraversalEnum();

    // 2.演示EnumMap对象的使用

    System.out.println("演示EnmuMap对象的使用和遍历.....");

    testEnumMap();

    // 3.演示EnmuSet的使用

    System.out.println("演示EnmuSet对象的使用和遍历.....");

    testEnumSet();

    }

    /**
     * 
     * 演示枚举类型的遍历
     */

    private static void testTraversalEnum() {

    Light[] allLight = Light.values();

    for (Light aLight : allLight) {

        System.out.println("当前灯name：" + aLight.name());

        System.out.println("当前灯ordinal：" + aLight.ordinal());

        System.out.println("当前灯：" + aLight);

    }

    }

    /**
     * 
     * 演示EnumMap的使用，EnumMap跟HashMap的使用差不多，只不过key要是枚举类型
     */

    private static void testEnumMap() {

    // 1.演示定义EnumMap对象，EnumMap对象的构造函数需要参数传入,默认是key的类的类型

    EnumMap<Light, String> currEnumMap = new EnumMap<Light, String>(

    Light.class);

    currEnumMap.put(Light.RED, "红灯");

    currEnumMap.put(Light.GREEN, "绿灯");

    currEnumMap.put(Light.YELLOW, "黄灯");

    // 2.遍历对象

    for (Light aLight : Light.values()) {

        System.out.println("[key=" + aLight.name() + ",value="

        + currEnumMap.get(aLight) + "]");

    }

    }

    /**
     * 
     * 演示EnumSet如何使用，EnumSet是一个抽象类，获取一个类型的枚举类型内容<BR/>
     * 
     * 可以使用allOf方法
     */

    private static void testEnumSet() {

    EnumSet<Light> currEnumSet = EnumSet.allOf(Light.class);

    for (Light aLightSetElement : currEnumSet) {

        System.out.println("当前EnumSet中数据为：" + aLightSetElement);

    }

    }

}
```
执行结果如下：


演示枚举类型的遍历 ......

当前灯name：RED

当前灯ordinal：0

当前灯：1

当前灯name：GREEN

当前灯ordinal：1

当前灯：3

当前灯name：YELLOW

当前灯ordinal：2

当前灯：2

演示EnmuMap对象的使用和遍历.....

[key=RED,value=红灯]

[key=GREEN,value=绿灯]

[key=YELLOW,value=黄灯]

演示EnmuSet对象的使用和遍历.....

当前EnumSet中数据为：1

当前EnumSet中数据为：3

当前EnumSet中数据为：2

------------------------------------------------------------
 

1. 所有的枚举类型都是Enum类的子类。 它们继承了这个类的许多方法。其中最有用的一个方法是toString()，这个方法能够返回枚举常量名。   toString()方法的逆方法是静态方法valueOf(Class, String). 例如 Light lt = (Light) Enum.valueOf(Light.class, "RED"); 将lt设置为 Light.RED。 每个枚举类型都有一个静态的values()方法，它将返回一个包含全部枚举值的数组。   ordinal()方法返回enum声明中枚举常量的位置，位置从0开始计数。例如  Light.GREEN.ordinal()返回1。   Enum类实现了Comparable接口，  int  compareTo( E other)  如果枚举常量在other之前，则返回一个负值； 如果this==other，则返回0；否则，返回正值。 枚举常量的出现次序在enum 声明中给出。（所以不能直接用<,>符号比较两个枚举值）

2. 可以创建一个enum类，把它看做一个普通的类。除了它不能继承其他类了。(java是单继承，它已经继承了Enum),

可以添加其他方法，覆盖它本身的方法

3. switch()参数可以使用enum了

4. values()方法是编译器插入到enum定义中的static方法，所以，当你将enum实例向上转型为父类Enum是，values()就不可访问了。解决办法：在Class中有一个getEnumConstants()方法，所以即便Enum接口中没有values()方法，我们仍然可以通过Class对象取得所有的enum实例

5. 无法从enum继承子类，如果需要扩展enum中的元素，在一个接口的内部，创建实现该接口的枚举，以此将元素进行分组。达到将枚举元素进行分组。

6. 使用EnumSet代替标志。enum要求其成员都是唯一的，但是enum中不能删除添加元素。

7. EnumMap的key是enum，value是任何其他Object对象。

8. enum允许程序员为eunm实例编写方法。所以可以为每个enum实例赋予各自不同的行为。

9. 使用enum的职责链(Chain of Responsibility) .这个关系到设计模式的职责链模式。以多种不同的方法来解决一个问题。然后将他们链接在一起。当一个请求到来时，遍历这个链，直到链中的某个解决方案能够处理该请求。

10. 使用enum的状态机

11. 使用enum多路分发