# Beginner Gameplay Scripting
[Scripting Tutorials: 我看的教程在这里](https://unity3d.com/learn/tutorials/s/scripting?_ga=2.265077702.898767388.1557840587-1730382583.1555070403)
[Script API Version 2018.3: 权威的API解释在这里](https://docs.unity3d.com/2018.3/Documentation/ScriptReference/)
#### 2019.5.15
```CSharp
//Awake函数调用在Start之前
MonoBehaviour.Awake()
MonoBehaviour.Start()
//Update每帧更新一次
MonoBehaviour.Update()
//FixedUpdate每隔相同时间更新一次：亲测0.02s
MonoBehaviour.FixedUpdate()
//enabled属性可以用来控制诸如灯光开关之类的事情
Light.enabled
GameObject.SetActive(bool)
//activeSelf和activeInHierarchy的概念是不一样的
GameObject.activeSelf
GameObject.activeInHierarchy
//移动
Transform.Translate(Vector3)
//旋转
Transform.Rotate(Vector3,float)
//Camera跟踪
Transform.LookAt(Transform)
//线性插值函数
Mathf.Lerp(float,float,float)
Vector3.Lerp(Vector3,Vector3,float)
Color.Lerp(Color,Color,float)
```
#### 2019.5.16
```CSharp
//Destroy可以用于GameObject或者仅仅是Component，同时也可以接受第二个可选参数：延迟时间
MonoBehaviour.Destroy(GameObejct,float)
//第一次按下去
Input.GetKeyDown(KeyCode)
//按下去+持续
Input.GetKey(KeyCode)
//起来
Input.GetKeyUp(KeyCode)
//同理
Input.GetButtonDown(string)
Input.GetButton(string)
Input.GetButtonUp(string)
//不是很清楚具体怎么用
Input.GetAxis(string)
//返回整数
Input.GetAxisRaw(string)
//点击
MonoBehaviour.OnMouseDown()
//GetComponent对性能损耗很高
//可以用来获取其他GameObject中的Component 比如BoxCollider
MonoBehaviour.GetComponent<Type>()
GameObjectReference.GetComponent<Type>()
//两次Update或者FixedUpdate之间的时间间隔
Time.deltaTime
//用来创造例如prefab的clone
MonoBehaviour.Instantiate(Object,Vector3,Quarternion)
//射子弹可以用的函数
Regidbody.AddForce(Vector3)
//找到有相应tag的GameObjects
GameObject.FindGameObjectsWithTag(string)
//Invoke延后调用某个方法 string是相应的方法名字 这个方法类型必须是void x()
MonoBehaviour.Invoke(string,float)
//基本和上一个一样但这个重复调用 每次间隔gap秒
MonoBehaviour.InvokeRepeating(String,float,float gap)
MonoBehaviour.CancelInvoke(String)
```