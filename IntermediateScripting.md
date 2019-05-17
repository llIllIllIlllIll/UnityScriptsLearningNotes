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

//可以在子类构造器里用base()通过特定的参数列表显示调用某一
//个父类构造器（默认情况下自动调用父类的构造器

//也可以在子类构造器里通过base调用某些父类函数

/*Member Hiding子类在继承父类的时候对子类属性的type前都加上new（或者不加也一样）
这样子的话关于多态方面Java和CSharp就会有一些显著的不同：Java中如
果你持有子类的父类引用，在用父类引用调用某一个子类父类都完成的方
法时调用的是子类的方法；而CSharp调用的是父类的方法。*/
class Fruit 
{
    public void SayHello()
    {
        Debug.Log("Hello, I am a fruit.");
    }
}
class Apple : Fruit 
{   
    public new void SayHello()
    {
        Debug.Log("Hello, I am an apple.");
    }
}
public class FruitSalad : MonoBehaviour
{
    void Start () 
    {
        Fruit myFruit = new Apple();
        //这种情况下 输出的是fruit函数调用的结果    
        myFruit.SayHello();
    }
}
/*Overriding: 想要实现Java中的Overriding效果的方法是在父类方法
的返回值前加上virtual并在复写的子类函数返回值前写上override*/
class Fruit 
{
    public virtual void SayHello()
    {
        Debug.Log("Hello, I am a fruit.");
    }
}
class Apple : Fruit 
{   
    public override void SayHello()
    {
        Debug.Log("Hello, I am an apple.");
    }
}
public class FruitSalad : MonoBehaviour
{
    void Start () 
    {
        Fruit myFruit = new Apple();
        //这种情况下 输出的是Apple函数调用的结果    
        myFruit.SayHello();
    }
}
// Interface: CSharp不支持多重继承，但支持同时实现多个接口

// Extension: 这个功能感觉很屌，可以给现有的类添加方法。
//重要的是对方法的第一个参数添加this关键字，this指向的
//类就是我们要拓展的类
//并且这个方法必须放在一个非泛型的static类里

public static class ExtensionMethods
{
    //Even though they are used like normal methods, extension
    //methods must be declared static. Notice that the first
    //parameter has the 'this' keyword followed by a Transform
    //variable. This variable denotes which class the extension
    //method becomes a part of.
    public static void ResetTransformation(this Transform trans)
    {
        trans.position = Vector3.zero;
        trans.localRotation = Quaternion.identity;
        trans.localScale = new Vector3(1, 1, 1);
    }
}
```