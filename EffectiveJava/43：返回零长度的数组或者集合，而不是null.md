<font size = "3">

**返回零长度数组的缺点**

- 用户需要额外的判断这个null

```JAVA
// The rigth way to return an aray from a collection
private final List<Cheese> cheesesInStock = ...;
private static final Cheese[] EMPTY_CHEESE_ARRAY = new Cheese[0];
/**
 * @return an array containing all of the cheeses in the shop,
 *  or null if no cheese are available for purchase.
 */
 public Cheese[] getCheeses(){
   return cheesesInStock.toArray(EMPTY_CHEESE_ARRAY);
 }
```
上面这个代码就是一个返回空数组的实例。

首先我们要知道List.toArray这个方法，他有两种重载
- 空值：直接返回一个Object数组，需要一个一个的强制类型转换
- 数组：如果数组的大小**可以**容纳list的大小，那么就直接使用传入的数组，并返回。如果数组的大小**不能够**容纳list的大小，那么就会新建一个数组返回。原本传入的数组不变。

有了这个知识，就可以很轻松的理解这段代码

**简而言之，返回类型为数组或者集合的方法没有理由返回null，而是返回一个零长度的数组或者集合。**

</font>