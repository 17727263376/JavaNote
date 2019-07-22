[toc]

<font size = "3">

# Object的equals方法

**特点：** 类的每个实例只与它自己相等
```
public boolean equals(Object obj) {
  return (this == obj);
}
```
注意：==和equals的区别
> ==符号是JAVA比较两个变量的地址，而equals是由用户自己定义或者调用Object的

# 覆盖equals应该遵循的规则
-  **自反性：** 对于任何非null的引用值x，x.equals(x)必须返回true。
-  **对称性：** 对于任何非null的引用值x和y，当且仅当y.equals(x)返回true时，x.equals(z)必须返回true。
-  **传递性：** 对于任何非null的引用值x、y和z，如果x.equals(y)返回true，并且y.equals(z)也返回true，那么x.equals(z)也必须返回true。
-  **一致性：** 对于任何非null的引用值x和y，只要equals的比较操作在对象中所用的信息没有被修改，多次调用x.equals(y)就会一致地返回true，或者一致地返回false。
-  对于任何非null的引用值x，x.equals(null)必须返回false。

违背了这些规则会带来非常大的错误，具体的错误**在书中**有一一举例。

# 实现高质量equals的步骤
1. **使用==操作符检查“参数是否为这个对象的引用”。** 如果是，则返回true。这只不过是一种性能优化，以尽可能避免后面的比较。
2. **使用instanceof操作符检查“参数是否为正确的类型”。** 如果不是，则返回false。
3. **把参数转换成正确的类型**
4. **对于该类中的每个“关键域”，检查参数中的域是否与该对象中的对应域相匹配。** 应该优先比较那些最有可能不一样的域，或者是开销比较小的域。这样能够及时判断出false，减小内存消耗
```
public class Point {
  private final int x;
  private final int y;
  public Point(int x, int y) {
    this.x = x;
    this.y = y;
  }

  @Override
  public boolean equals(Object o) {
    if (o == this) return true;     //第一条
    if (!(o instanceof Point)) return false;    //第二条
    Point p = (Point) o;    //第三条
    return p.x == x && p.y == y;    //第四条
  }
}
```

# 注意
- 覆盖equals时总要覆盖hashCode。
- 不要企图让equals方法过于智能。
- 不要将equals声明中的的Object对象替换为其它类型。（继承而来的equals的参数都是Object类型）
</font>