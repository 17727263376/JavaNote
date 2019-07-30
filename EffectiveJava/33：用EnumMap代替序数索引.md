[toc]

<font size = "3">

# 什么时候使用EnumMap
当我们需要建立一个枚举和其他对象的键值关系的时候，我们通常会使用一个数组来实现一个map，也就是调用枚举的ordinal方法返回序数下标

这个方法有非常多的错误
- 如果要添加新的枚举类型，那么map又要重新维护
- 如果下标返回出现错误，是很难发现的

# EnumMap的使用方法
```
public class Herb {
    public enum Type { ANNUAL, PERENNIAL, BIENNIAL }

    private final String name;
    private final Type type;
    
    Herb(String name, Type type) {
        this.name = name;
        this.type = type;
    }

    @Override
    public String toString() {
        return name;
    }
}
```
使用EnumMap对花园中的花进行分类
```
Map<Herb.Type, Set<Herb>> herbByType = new EnumMap<Herb.Type, Set<Herb>>(Herb.Type.class);//构造一个Type枚举Map
    for(Herb.Type t : Herb.Type.values())
        herbByType.put(t, new HashSet<Herb>());
        //为每一个类型创建一个set保存
    for(Herb h : garden)
        herbByType.get(h.type).add(h);
        //为set添加数据
    System.out.println(herbByType);
```


</font>