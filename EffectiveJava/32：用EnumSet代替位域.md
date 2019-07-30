[toc]

<font size = "3">

# 什么是位域
通过一个int字段，通过位运算来管理和添加多个标志或者状态。

如果没有位域的话，当我们遇到了多个状态以及多个状态可以合成更多状态的情况的时候。只能通过设置bool值来设置，需要编写大量的代码

> 它的核心思想就是将, int 数值看做是 二进制数位表示.如果有四个状态就可以像这样 0000,用四位二进制表示,每一个二进制位都可以表示一种状态. 然后通过 位运算,来提取或添加标记位.四位对应的组合状态有16个. 而我们,只需要通过一个int变量就能够管理这些状态.

```
public class Gravity {
    // 二进制表示 0001
    public static final int LEFT              = 1;
    // 二进制表示 0010
    public static final int RIGHT             = LEFT << 1;
    // 二进制表示 0100
    public static final int TOP               = LEFT << 2;
    // 二进制表示 1000
    public static final int BOTTOM            = LEFT << 3;
    // 水平居中, 二进制表示 0011
    public static final int HORIZONTAL_CENTER = LEFT | RIGHT;
    // 垂直居中, 二进制表示 1100
    public static final int VERTICAL_CENTER   = TOP | BOTTOM;
    // 居中, 二进制表示 1111
    public static final int CENTER            = HORIZONTAL_CENTER | VERTICAL_CENTER;
    // 默认左上角, 二进制表示 0101
    public static final int DEFAULT           = LEFT | TOP;

    // 存放标志位
    private int mFlags = DEFAULT;

    // 设置标记位,会清除原来的标记
    public void setFlags(int flags) {
        mFlags = flags;
    }

    // 添加标记位,在原来的基础上添加
    public void addFlags(int flags) {
        mFlags |= flags;
    }

    // 清除指定的标记
    public void clearFlags(int flags) {
        mFlags &= ~flags;
    }

    // 清除所有标记,设为默认
    public void clears() {
        mFlags = DEFAULT;
    }

    // 判断是否存在指定的标记
    public boolean hasFlags(int flags) {
        return (mFlags & flags) == flags;
    }

    // 判断是否 只有指定的标记
    public boolean onlyFlags(int flags) {
        return mFlags == flags;
    }

    public void apply() {
        String des = "左上角";
        if (hasFlags(CENTER)) {
            des = "整体居中";
        } else if (hasFlags(HORIZONTAL_CENTER)) {
            if (hasFlags(BOTTOM)) des = "水平居中,竖直向下";
            else des = "水平居中,竖直向下";
        } else if (hasFlags(VERTICAL_CENTER)) {
            if (hasFlags(RIGHT)) des = "竖直居中,水平向右";
            else des = "竖直居中,水平向左";
        } else if (hasFlags(LEFT | BOTTOM)) {
            des = "左下角";
        } else if (hasFlags(RIGHT | TOP)) {
            des = "右上角";
        } else if (hasFlags(RIGHT | BOTTOM)) {
            des = "右下角";
        }
        System.out.println("你选择的布局是 : " + des);
    }
}
```

# 使用int表示位域的缺点
- 用int表示位域有着int枚举常量的所有缺点
- 当位域以数字形式打印时，翻译位域更加困难

# 使用EnumSet
java.util包提供了EnumSet类来有效的表示从单个枚举类型中提取的多个值得多个集合。这个类实现Set接口，提供了丰富的功能、类型安全性，以及可以从任何其他Set实现中得到的互用性。但是在内部具体的实现上，每个EnumSet就是用单个long来表示，因此它的性能比得上位域的性能。

# EnumSet的用法
EnumSet 是一个与枚举类型一起使用的专用 Set 实现。枚举set中所有元素都必须来自单个枚举类型（即必须是同类型，且该类型是Enum的子类）。
- 使用noneOf创建一个空的EnumSet：
```JAVA
Set<Season> emptyEnumSet = EnumSet.noneOf(Season.class);
```
- 使用allOf创建一个拥有所有枚举元素的EnumSet：
```JAVA
Set<Season> enumSet = EnumSet.allOf(Season.class);
```
- 使用Of创建一个拥有部分枚举元素的EnumSet：
```JAVA
Set<Month> summer = EnumSet.of(Month.APRIL, Month.MAY, Month.JUNE);
```
- 通过toArray转化为枚举数组
</font>