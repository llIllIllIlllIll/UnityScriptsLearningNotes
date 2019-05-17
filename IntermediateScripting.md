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

//CSharp中采用了C++ namespace的概念
namespace SomeNameSpace{
    //class code
}

//使用的时候 第一种方式和C++一样
using namespace SomeNameSpace;

//第二种方式比较像是JAVA
SomeNameSpace.SomaClass.function()

//List中的对象如果想要实现List的Sort函数该对象必须实现
//Icomparable<type>接口

//CSharp中的Map被Dictionary取代了
Dictionary<Tkey,Tvalue>
Dictionary<String,SomaClass> dic = new Dictionary<String,SomaClass>();
dic.Add("abc",new SomeClass);

//获取Dictionary中的值有两种方法 第一种可能会出现异常
//当"abc"不存在于dic中时异常
dic["abc"]
//对于不能确定存在的key用下面这个
if(dic.TryGetValue("abc",out temp)){
    //成功的话
}
else{
    //失败的话
}
```
#### 2019.5.17
```CSharp
//Csharp特性：Coroutine
//Coroutine在进行类似 物体移动的效果时 比起在Update函数
//里更新物体的坐标是更加有效率的一种方法
//Coroutine用法是 通过多次调用返回来完成整段代码
public class CoroutinesExample : MonoBehaviour
{
    public float smoothing = 1f;
    public Transform target;  
    void Start ()
    {
        StartCoroutine(MyCoroutine(target));
    }
    //好像所有的coroutine function返回值都是IEnumberator类型  
    IEnumerator MyCoroutine (Transform target)
    {
        while(Vector3.Distance(transform.position, target.position) > 0.05f)
        {
            transform.position = Vector3.Lerp(transform.position, target.position, smoothing * Time.deltaTime);
            //如果前次在这里return 那么下次就在while语句开始
            yield return null;
        }      
        print("Reached the target.");      
        yield return new WaitForSeconds(3f);
        print("MyCoroutine is now finished.");
    }
}

//Quaternion:控制物体旋转相关的属性
//不要直接修改XYWZ值
//upwards:vector that defines where up is
//返回一个相应方向的Quaternion对象
Quaternion Quaternion.LookRotation(Vector3 forward,Vector3 upwards)
//Spherically interpolates between a and b by t. 
//转圈用这个很合适
Quaternion Slerp(Quaternion a, Quaternion b, float t);
//等价于欧拉的(0,0,0)
Quaternion.identity

//delegate是一个很神奇的东西
//每一个delegate变量在申明时要独特底对应于一个函数类型
//然后可以把一个或多个相应类型的函数复制给它进行调用
//调用前确认该变量不为null可以避免异常
//感觉类似于 角色技能这些方面 这个特性会非常有用

class{
    delegate void MyDelegate(int num);
    void Start () 
    {
        //用=进行赋值
        myDelegate = PrintNum;
        myDelegate(50);
        //用+进行赋值
        myDelegate+=DoubleNum;
        if(myDelegate!=null){
            //使用前进行确认
            myDelegate(50);
        }
    }
    void PrintNum(int num)
    {
        print ("Print Num: " + num);
    }
    void DoubleNum(int num)
    {
        print ("Double Num: " + num * 2);
    }
}

//attribute：又一个很独特的特性
//语法是[attribute] 后面跟的是该属性附加的对象：某个值
//或者是某个class
//Range
[Range(-100, 100)] public int speed = 0;
//ExecuteInEditMode
[ExecuteInEditMode]
public class ColorScript : MonoBehaviour 
{
    void Start()
    {
        renderer.sharedMaterial.color = Color.red;
    }
}

//Events：软工课上观察者模式的完美案例，一种特殊的delegate
public class EventManager : MonoBehaviour 
{
    public delegate void ClickAction();
    //event的定义基于一个delegate
    public static event ClickAction OnClicked;
    void OnGUI()
    {
        if(GUI.Button(new Rect(Screen.width / 2 - 50, 5, 100, 30), "Click"))
        {
            //使用前先检查是个好习惯
            if(OnClicked != null)
                OnClicked();
        }
    }
}
public class TeleportScript : MonoBehaviour 
{
    void OnEnable()
    {
        //订阅
        EventManager.OnClicked += Teleport;
    }   
    void OnDisable()
    {
        //OnDisable取消订阅 重要 避免以外的bug
        EventManager.OnClicked -= Teleport;
    }
    void Teleport()
    {
    }
}

```