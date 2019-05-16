# Intermediate Gameplay Scripting
[Scripting Tutorials: 我看的教程在这里](https://unity3d.com/learn/tutorials/s/scripting?_ga=2.265077702.898767388.1557840587-1730382583.1555070403)
[Script API Version 2018.3: 权威的API解释在这里](https://docs.unity3d.com/2018.3/Documentation/ScriptReference/)
#### 2019.5.16
```CSharp
//这一段有点特殊 是CSharp独有的properties属性 
public class Player
{
    private int experience;   
    //Experience这里是一个property
    public int Experience
    {
        //有get set两个方法 对应于这个property的赋值与取值
        //可以通过去除某个方法让这个property变成只读或只写
        get
        {
            //Some other code
            return experience;
        }
        set
        {
            //Some other code
            experience = value;
        }
    }
    //每个property不一定要有一个对应的private值
    //其实可以当作方法来看就是了
    public int Level
    {
        get
        {
            return experience / 1000;
        }
        set
        {
            experience = value * 1000;
        }
    }
    
    //默认
    public int Health{ get; set;}
}
//static class的作用是 使得某个class不可以被实例化

//可以在子类构造器里用base()通过特定的参数列表显示调用某一个父类构造器（默认情况下自动调用父类的构造器

//也可以在子类构造器里通过base调用某些父类函数

//值得一提的是 关于多态方面Java和CSharp有一些显著的不同：Java中如果你持有子类的父类引用，在用父类引用调用某一个子类父类都完成的方法时调用的是子类的方法；而CSharp调用的是父类的方法，除非你显示向下转型。
```