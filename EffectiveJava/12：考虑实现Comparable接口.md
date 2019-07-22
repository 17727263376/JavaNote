[toc]

<font size = "3">

# 什么是Comparable接口
如果自己写的一个类实现了Comparable接口，那么它就具有排序、集合、等运算的能力

并且一旦实现了这个接口，它就可以跟许多泛型算法以及依赖与该接口的集合实现进行协作
```
public class CompObj implements Comparable<CompObj> {
  private int age;
  private String name;
  public CompObj(int age, String name) {
    this.age = age;
    this.name = name;
  }
  public int getAge() {
    return age;
  }
  public String getName() {
    return name;
  }

  //重点在这，这是接口中唯一的一个方法
  @Override
  public int compareTo(@NonNull CompObj another) {
    return this.age - another.age;
  }
}
```
排序操作
```
List<CompObj> list = ...;//初始化一个list
Collections.sort(list); //集合排序，就是这么简单
CompObj array[] = ...;//初始化一个数组
Arrays.sort(list)； //数组排序，就是这么简单
```
# CompareTo与equals的关系
他们都需要满足自反性、传递性、对称性

当CompareTo判定为相等的时候，equals可以判定为相等也可以判定为不相等。它任然可以工作，但是在一些集合API中会出现错误。

**强烈建议**CompareTo判定为相等时equals也可以判定为相等

# 什么时候要实现Comparable接口
- 你写的类是一个值类，像Integer这样的。
- 类中有很明显的内在排序关系，如字母排序、按数值顺序或是时间等。
# 如何写好一个Comparable接口
1. **满足对称性。**
即对象A.comparaTo(B)大于0的话，则B.comparaTo(A)必须小于0;
2. **满足传递性。**
即对象A.comparaTo(B)大于0，对象B.comparaTo(Z)大于0，则对象A.comparaTo(Z)一定要大于0；
3. **建议comparaTo方法和equals()方法保持一致。** 即对象A.comparaTo(B)等于0，则建议A.equals(B)等于true。
4. **对于实现了Comparable接口的类，尽量不要继承它，而是采取复合的方式**。


</font>