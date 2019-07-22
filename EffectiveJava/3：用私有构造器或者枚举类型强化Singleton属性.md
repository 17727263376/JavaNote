[toc]
<font size = "3">
# 什么是Singleton
仅仅被实例化一次的类。**详细参考[设计模式-单例模式]**

# 用私有构造器来强化
很简单，就是将构造器声明为private类型的，但是需要注意一点，享有特权的客户端可以利用反射机制来调用到私有的构造器，为了进一步确保单例的唯一性，我们可以在私有的构造方法中判断唯一实例是否存在，存在的话抛出异常，就像这样：
```

public class Singleton {
        private static final Singleton INSTANCE = new Singleton();
 
        private Singleton() {
            if (INSTANCE != null) {
                throw new UnsupportedOperationException("Instance already exist");
            }
        }
 
        public static Singleton getInstance() {
            return INSTANCE;
        }
}

```
但是这样的方法不仅在多线程中会存在错误，在序列化这个对象之后，在进行反序列化时也会出现多个对象的情况。

针对这个只需要添加以下函数即可
```

private Object readResolve() {
            return INSTANCE;
}
```
# 利用枚举来强化Singleton(最佳方案)
利用单元素的枚举来实现单例（Singleton），**绝对防止多次实例化**，上面的问题在这里都不会出现，实现代码如下：
```
public enum SingletonEnum {
        INSTANCE;       //枚举变量：一定只能有一个！！！
 
        private Book filed01;   //需要实现单例的类
        
        public SingletonEnum(Book b){
            filed01 = b         
            
        }
 
        public Book getFiled01() {
            return filed01;
        }
}
```
# 序列化和反序列化
- **序列化：** 就是将内存中的对象转换为==字节序列==，方便持久化到磁盘或者网络传输

过程：
1. 将对象转换为字节数组
2. 将字节数组存储到磁盘
- **反序列化：** 就是将字节序列转换为内存中的对象

> 在什么情况下需要进行序列化和反序列化呢？
> 1. 当需要把数据从内存中保存在本地物理硬盘中的时候，需要将数据序列化后存入本地
> 2. 在网络应用中，数据都是以字节序列的形式在网络中传输，所以在需要网路请求的情况也会进行序列化和反序列化

**如何实现序列化和反序列化：**

在JAVA中一个类只有实现了 ==Serializabl== 和==Externalizable==接口才能被序列化

> 实现Externalizable接口的类**完全由自身**来控制序列化的行为，

> 而仅实现Serializable接口的类可以采用**默认**的序列化方式。

# readResolve()的作用
按道理通过单例设计出来的类他是只有一个实例，但是在序列化和反序列化的过程中，会出现问题，就是==通过反序列化,一个新的对象克隆了出来==。这不符合单例的要求。所以我们通过添加readResolve方法可以==避免反序列化的时候再一次创建==一个新的类
</font>