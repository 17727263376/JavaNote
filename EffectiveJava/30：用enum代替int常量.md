> **JAVA1.5以后增加了两个新的引用类型：枚举类型、注解类型**

[toc]

<font size = "3">

# int常量的不足
> int枚举模式

```
public static final int APPLE_FUJI = 0;
public static final int APPLE_PIPPIN = 1;
public static final int APPLE_GRANNY_SMITH = 2;

public static final int ORANGE_NAVEL = 0;
public static final int ORANGE_TEMPLE = 1;
public static final int ORANGE_BLOOD = 2;
```
- 类型安全性和使用方便性方面没有帮助。
- 如果常量的值发生了变化，就需要重新编译。
- int枚举常量仅仅只是一个值，在打印成字符串的时候没有很大的方便。
- 遍历一个组中所有的int枚举常量，获得int枚举组的大小，没有可靠的方法，如想知道APPLE的常量有多少个，除了查看int枚举常量所在位置的代码外，别无他法，而且靠的是观察APPLE_前缀有多少个，不可靠

# 枚举的优势
**枚举的本质依旧是int值**

- 枚举是真正的final
- 枚举是安全的，试图传递类型错误的值时，会导致编译时错误。
- 包含同名常量的多个枚举类型可以共存，因为每个类型有自己的命名空间，增加或重新排列枚举类型的常量，无需重新编译客户端代码。通过调用toString方法，可以将枚举转换成可打印的字符串。
- 枚举类型还允许添加任意的方法和域，并实现任意的接口
```
public enum Planet {
    MERCURY(3.302e+23, 2.439e6),
    VENUS(4.869e+24, 6.052e6),
    EARTH(5.975e+24, 6.378e6),
    MARS(6.419e+23, 3.393e6),
    JUPITER(1.899e+27, 7.149e7),
    SATURN(5.685e+26, 6.027e7),
    URANUS(8.683e+25, 2.556e7),
    NEPTUNE(1.024e+26, 2.477e7);
    private final double mass;
    private final double radius;
    private final double surfaceGravity;
    //枚举天生就是不可变的，因此所有的域都应该是final的
    private static final double G = 6.67300e-11;

    Planet(double mass, double radius) {
        this.mass = mass;
        this.radius = radius;
        surfaceGravity = G * mass / (radius * radius);
    }
    public double mass() {
        return mass;
    }
    public double radius() {
        return radius;
    }
    public double surfaceGravity() {
        return surfaceGravity;
    }
    public double surfaceWeight(double mass) {//F=ma
        return mass * surfaceGravity;
    }
}
```
main函数的使用
```
public static void main(String[] args) {
    double earthWeight = 175;
    double mass = earthWeight / Planet.EARTH.surfaceGravity();
    for(Planet p : Planet.values()) {
        System.out.printf("Weight on %s is %f%n", p, p.surfaceWeight(mass));
    }
    //每一个枚举类型都有一个values()方法，通过这个方法可以按照声明顺序返回它的值数组
    
    //对于打印，可以通过覆盖toString()方法来实现。
}
```
**如果枚举具有普遍适用性，它应该成为一个顶层类，如果只被用在一个特定的顶层类中，应该成为顶层类的一个成员类。**

# 枚举抽象方法
有的时候不同的枚举需要完成不同的功能，使用switch是不安全的，当增加一个新的枚举的时候，如果忘记增加相应的case，编译器不会检查出来，造成错误

**可以通过设置枚举抽象放法，让每一个枚举都实现它**
```
public enum Operation {
    PLUS("+") {
        double apply(double x, double y) {return x + y;}
    },
    MINUS("-") {
        double apply(double x, double y) {return x - y;}
    },
    TIMES("*") {
        double apply(double x, double y) {return x - y;}
    },
    DIVIDE("/") {
        double apply(double x, double y) {return x - y;}
    };
    private final String symbol;//抽象方法
    Operation(String symbol) {
        this.symbol = symbol;
    }
    public String toString() {
        return symbol;
    }
    abstract double apply(double x, double y);
}
```
# 枚举策略
枚举唯一不足的地方就是不能够共享域中的数据，也就是同一个枚举中的不同的实例，这样是很难共享数据的

- **枚举策略解决多个枚举常量同时共享相同的行为。**

枚举策略就是添加一个内部枚举类，并将抽象方法移至内部枚举
```
enum PayrollDay {
    MONDAY(PayType.WEEKDAY),
    THESDAY(PayType.WEEKDAY),
    WEDNESDAY(PayType.WEEKDAY),
    THURSDAY(PayType.WEEKDAY),
    FRIDAY(PayType.WEEKDAY),
    SATURDAY(PayType.WEEKEND),
    SUNDAY(PayType.WEEKEND);
    
    private final PayType p;
    PayrollDay(PayType p) {
        this.p = p;
    }
    double pay(double hoursWorked, double payRate) {
        return payType.pay(hoursWorked, payRate);
    }

    private enum PayType {
        WEEKDAY {
            double overtimePay(double hours, double payRate) {
                return hours <= HOURS_PER_SHIFT ? 0 :
                    (hours - HOURS_PER_SHIFT) * payRate * 2;
            }
        },
        WEEKEND {
            double overtimePay(double hours, double payRate) {
                return hours * payRate / 2;
            }
        };
        private final static int HOURS_PER_SHIFT = 8;
        abstract double overtimePay(double hours, double Rate);
        double pay(double hoursWorked, double payRate) {
            double basePay = hoursWorked * payRate;
            return basePay + overtimePay(hoursWorked, payRate);
        }
    }
}
```
</font>