<font size = "3">

**float和double是为了科学计算和工程计算而设计的。他们执行二进制浮点运算**

**但是！！！他们是不精确的**

```
System.out.println(1.03-0.42);
```
然而结果为0.6100000000000001

那么应该如何解决呢？

**使用BigDecimal、int、long来代替**
- BigDecimal：
> BigDecimal是JAVA封装的一个高精度的类，可以直接进行浮点运算，并且保证正确性。BigDecimal有8种舍入模式。但是它有两个缺点，**与基本运算相比它很麻烦、速度慢**
```JAVA
BigDecimal a = new BigDecimal("1.03");
BigDecimal b = new BigDecimal("0.42");
//注意：如果BigDecimal的构造函数使用了double构造，那么计算结果也是不精确的。
System.out.println(a.subtract(b));
```
- int、long
> 通过将原本的小数转换为整数，例如1.03-0.42就换成103-42计算。最后在处理结果。

**总结：**

对于需要精确答案的计算，不能使用float或者double，BigDecimal允许完全控制舍入，如果**业务要求涉及多种舍入方式**，使用BigDecimal很方便，如果性能很关键，涉及的数值不大，就可以使用int或者float，**如果数值范围没有超过9位十进制数字，可以使用int，如果不超过18位数字，使用long，如果数值可能超过18位，就必须用BigDecimal**。
</font>