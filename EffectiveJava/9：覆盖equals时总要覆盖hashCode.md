[toc]

<font size = "3">

# 什么是hashCode
这个函数是继承Object的函数，它可以返回一个类的哈希值。哈希值在HashMap、HashSet、HashTable中会经常使用。

如果一个类在重写了equals后没有重写hashCode的话，在使用以上Hash接口时就会出现错误。

例如：

现在有一个类Person，我们按照第8条的约定在重写了equals函数，我们将它放入一个HashMap中，我们通过
```
map.get(new Person());
```
是取不到正确答案的，因为这里new出来的两个Person的哈希值是不一样的。

# hashCode的通用约定
- 在应用程序的执行期间，只要对象的equals方法的比较操作所用到的信息没有改变，那么对于这同一个对象调用多次，hashcode方法都必须返回同一个整数。
- 如果两个对象相同，那么两个对象的hashcode也必须相同
- 如果两个对象的hashcode相同，并不一定表示两个对象就相同，也就是不一定适合equals方法，只能够说明两个对象在散列表存储结构中，“存放在同一个篮子里”。

# 如何重写一个hashCode
I：为对象计算int类型的散列码c:
- 对于boolean类型，计算(f?1:0)
- 对于byte,char,short,int类型，则计算(int)f
- 对于long类型，计算(int)(f^(f>>>32))
- 对于float类型，计算Float.floatToIntBits(f)
- 对于Double类型，计算Double.doubleToLongBits(f)，然后再按照long类型处理
- 对于对象引用，并且该类的equals方法通过递归地调用equals的方式来比较这个域，则同样为这个域递归地调用hashcode
- 对于数组，则把每一个元素当作单独的域来处理。

II：将获取到的c合并：result = 31 * reuslt + c;

III：返回result

> 注意">>>"运算符：表示无符号右移，在这里即取long的高32位转化为int类型

> Float.floatToIntBits(f)：根据IEEE 754浮点“单一格式”的二进制转化为十进制并返回int。简单的说也就是float类型的二进制转为十进制int并返回

```
 @Override
public int hashCode() {
    int result = 17;
    result = 31 * result + areaCode;
    result = 31 * result + prefix;
    result = 31 * result + lineNumber;
    return result;
}
```
> 为什么是用31来乘result？因为它是一个奇素数，他可以通过移位和减法来代替乘法，即：
> ```math
> 31*i == (i<<5)-i
> ```
> **在现代的JVM中已经可以自动优化这一步的运算了。**

**如果一个类是不可变类**，并且计算散列码的开销也比较大，就应该考虑吧散列码缓存在对象内部，而不是每次请求的时候都重新计算散列码。
```
private volatile static int hashcode;
    
@Override
public int hashCode() {
    int result = hashcode;
    if (result == 0){
        result = 31 * result + areaCode;
        result = 31 * result + prefix;
        result = 31 * result + lineNumber;
        hashcode = result;
    }
    return result;
}
```

**不要试图从散列码计算中排除一个对象的关键部分来提高性能**
</font>