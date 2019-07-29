[toc]
<font size = "3">

# 引例
- 有限制通配符可以增加一个泛型的灵活性
```
public void pushAll(Iterable<E> src) {  
    for (E e: src)  
        push(e);  
}
```
首先我们分析上面这个函数。这个函数的问题是：**只有当Iterable<E>中的E和外部类的E完全一致的时候才能正确使用**，也就是只有泛型一致才能用。但是这在平常生活中是不合理的。因为这个类完全可以push泛型E的子类的。

举个例子：如果泛型定义为Number，那么它也可以放入Integer，因为Integer是Number的子类。

但是
```
Stack<Number> numberStack = new Stack<Number>();  
Iterable<Integer> integers = ...;  
numberStack.pushAll(integers); 
```
这是不允许的，因为泛型是==不可变的==。

**解决办法：**

使用有限制的通配符
```
public void pushAll(Iterable<? extends E> src) {  
    for (E e: src)  
        push(e);  
}
```
其中的<? extends E>就表示Iterable的泛型可以是E及其子类，这样就可以满足以上的需求。

# PESC
producter-extends, consumer-super.

简单的来说就是：

**生产者就用extends**，因为生产者为原有泛型提供实例，这个实例是可以为子类的

**消费则就用super**，因为消费者为外部提供实例，这个实例是可以向上转型为父类的

==不要用通配符类型作为返回类型==

# 如果类型参数只在方法声明中出现一次，就可以用通配符取代它。
```
public static <E> void swap(List<E> list, int i, int j);  
public static void swap(List<?> list, int i, int j); 
```
通常第二种方法更好一些，因为他更简单

但是他有一个问题
```
public static void swap(List<?> list, int i, int j) {  
    list.set(i, list.set(j, list.get(i)));  
}
```
这样是操作是实现不了的
因为list是无限通配符类型List<?>，以前说过，除了null以外的任何对象都无法放入其中。

我们可以通过设置一个私有的辅助函数来铺抓通配符
```
public static void swap(List<?> list, int i, int j) {  
    swapHelper(list, i, j);  
}  
  
// Private helper method for wildcard capture  
private static <E> void swapHelper(List<E> list, int i, int j) {  
    list.set(i, list.set(j, list.get(i)));  
}  
```
</font>