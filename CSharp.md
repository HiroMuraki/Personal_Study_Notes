# 通用

代码大纲

```c#
#region Name
#endregion
```



# 变量

## 范围

数值类型

```c#
sbyte
```
## 枚举

###  声明

~~~c#
enum ColorBase : int{
    R,
    G,
    B,//最后一个逗号可选
}
//:后面指明枚举类型，默认为int
~~~

枚举一定是值类型

 ### 使用

```c#
//取值
ColorBase.R;//输出为字符串"R"，这点和C++有点不一样
//转换
(int)ColorBase.R;//强制转换，值为0
(ColorBase)0;//强制转换，值为Color.R
Convert.ToString(ColorBase.R);//强制转换，值为R得字符串值
ColorBase.R.ToString();//同上
(Color)Enum.Parse(typeof(Color), "R");//强制转换，字符串值转为枚举
```

## 类型转换

checkded用于检查数值转换是否溢出边界，若溢出，则会在运行时报错。对性能有一定影响

```c#
long r = 100000000000000000;
int a;
checked{
    a = (int)r;
}
WriteLine(a);
```

## 数组

```c#
//多维数组
int[,] matrix = new int[3,4];//3行4列得二维数组
int[,] martix = new int[3,4]{{1,2,3},{1,2,3,4}};//初始化
//交错数组
//赋值
int[][] nums = new int[2][];
nums[0] = new int[2];
nums[1] = new int[3];
//初始化
int[][] nums = {
    new int[2],
    new int[3]
};//第一行为2列，第二行为3列的数组
```

## 元组

```c#
//默认访问
int nums = (1,2,3,4,5);
nums.Item1;//第一个元素
nums.Item2;//第二个元素
//指定成员名
(int one, int two ,int three) nums = (1,2,3);
nums.one;//one的值
```



## 结构

```c#
struct TEST{
    int a;//默认为private
    public int b;//指定访问修饰符为public
    public int total() => a+b;
}
```



## 可空值类型

```c#
Type num = 0;
Type? numNullable = null;//可空值类型
Type? numNullalbe = num;//√，普通类型可以直接赋值可空类型
num = (Type)numNullable;//可空类型转化为普通类型需要强制转换
num = numNullable.value;//作用同上，取值
```

# 运算符与流程控制

## 布尔运算符

||与&&具有短路性质，即若面前的条件可以判定结果，则不再判定后面的条件

|与&则是全判断

按位运算符

```C#
var1 &= var2 //var1 = var1 & var2
var1 |= var2 //var1 = var1 | var2
var1 ^= var2 //var1 = var1 ^ var2
```

其结果为位运算结果

## switch跳转语句

实现连续执行case语句

```c#
switch(n){
    case 0:
    case 1:
        //Do something
        goto case 2;
    case 2:
        //DO something
        goto default;
    default:
        //Do something
        break;
}
```

模式匹配

```c#
object n;
switch(n){
    case int val when val <0://如果n是int且n<0则激活
        //Do something
        break;
    case int val when val >10://如果n是int且n>10则激活
        //Do something
        break;
    case string str when str.Length>10://如果n为string且n长度大于10则激活
        //Do something
        break;
    case null://如果n为null引用则激活。
        //Do something
        break;
}
```

# 函数

默认/可选与命名参数

```C#
//使用默认值
int Add(int val1, int val2 = 0){
	//
}
//使用[Optional]特性
int Add(int val1, [Optional] val2){
    //
}
//命名参数
int Add(val2 : 2, val1 : 3){
    //
}
```

```C#
public void Funciton(ref int a, out b, int c = 1){
	//Do something
}
Funciton(R, out int r, c: 10);
```

out: 输出

ref: 引用

参数名: 参数 : 指定参数名传参

表达式方法

```c#
static double Multiply(double va11, double val2){
    return val1 * val2;
}
//转换为表达式方法
static double Multiply(double val1, double val2) => val1 * val2;
```

参数数组

```c#
static double Add(double multiplier, params double[] vals){
    double sum = 0;
    foreach(double val in vals){
        sum += val;
    }
    return multiplier * sum;
}
```

引用参数

```c#
static ref int Multiplier(ref int n){
    n *= 2;
    return ref n;
}
//若返回类型引用，则返回类型，返回语句，形参都需要ref修饰，且不可返回表达式
```

输出参数

```c#
static int multiplier(out int n){
    n = 100;
    reutrn 0;
}
//调用函数后实参值变为100，同时必须在函数中为out修饰的参数赋值，且实参的值在函数中无效
```

局部函数

```c#
static int FuntionA(int a){
    a *= 5;
    int FuncitonB(int b){
        a * = 10;
        return a;
    }
    a = FunctionB(a);
    return a;
}
```

作用域

```c#
string str = "";
for(int i = 0; i < 10; ++i){
    str = $"{i}";
}
str2 = str;//必须在循环外为str初始化分配内存空间，才能再循环外访问
```

Main参数

```c#
string[] args;
args[0];//第一个参数，而非程序名
```

## 委托

```c#
delegate double Process(double val1, double val2);
static double Add(double val1, double val2){
    return val1 + val2;
}
Process R = new Process(Add);//或者Process R= Add
R(2,3);//调用Add(2,3)
```

## Lambda表达式

```c#
delegate NumbersOperator(int paramA, int paramB);
void Operation(NumberOperator del);
Operation( (paramA, paramB) => paramA + paramB );//注意参数是未类型化的，需要编译器推断
Operation( (int paramA,int paramB) => paramA + paramB );//可以显式化参数类型
() => Math.PI;//无参数
param => param + 5;//一个参数，可以省略括号,省略括号后不能显式化参数类型
Operation( delegate(int paramA, int paramB){return paramA + paramB;} );//作用同上

(paramA, paramB) => {
    //...
    return ...;
}//多行lambda，如果返回类型不为void，则需要返回值
```



# 异常

基本用法

```c#
try{
    str = Console.Readline();
    if(str != "Hello World!"){
        throw new Exception();
    }
}
catch(Exception){
    //如果str不为"Hello World!"则被捕获
}
finally{
    //无论str是什么，都执行此块
}
```

异常过滤器

```c#
List<string> words = new List<string>(){
    "AAA", "BBB", "CCCCC",
};
foreach(string word in words){
    try{
        if(word.Length == 3){
            throw new Exception();
        }
    }
    catch(Exception e) when (word = "AAA"){
        //当word长度为3，且内容为AAA的时候在这里捕获
    }
    catch(Exception e) when (word = "BBB"){
        //当word长度为3，且内容为BBB的时候被这里捕获
    }
    catch(Exception e){
        //当word长度为3，且不满足上述所有过滤条件时，被这里捕获
    }
    finally{
        //finally块
    }
}
```



# 类

## OOP

原则：继承，抽象，封装，多态

 ## 修饰符

```c#
//访问修饰符
public
protected
internal
protected internal
private//默认修饰符
private protected//同一程序集中可访问，默认修饰符
//类修饰符
partial//部分类，可以将类定义放置于不同位置
sealed//不可继承类
abstract//不可实例化类，即抽象类，只有抽象类可以包含抽象类方法

//方法修饰符
partial//部分方法，部分方法必须为private，且不能有返回值
static//静态
new//显式声明隐藏
abstract//必须在派生类中重写（除非派生类也是抽象类）
virtual//可重写
sealed//不可在派生类中重写
override//覆盖virtual修饰的方法
extern//方法于程序集外部定义
```

抽象类

由abstract访问符修饰，不可进行实例化

```c#
abstract class AbstractClass{
    public void Get(){};//访问修饰符不可为private
    public abstract void Get2();//声明为抽象方法后，在其继承类中必须实现其抽象方法，且仅有抽象类可以有抽象成员
}
```

实例继承

```c#
class ClassName:AbstractClass{
    public void Get2(){
        //Do something
    }
}
```

抽象类依然可以引用继承类

密封类

密封类无法继承，但是可通过以下方式扩展方法

```c#
sealed class PlainBot{
    private string name;
    public void Move(){
        WriteLine("Is Moving!");
    }
}
static class AdvancedBot{
    public static void Jump(this PlainBot pb){
        WriteLine("Is Rotating!");
    }
}
//这样，就为PlainBot扩展了一个Jump()方法
class MAIN{
    public static void Main(){
        PlainBot pb = new PlainBot();
        pb.Move();
        pb.Jump();
	}
}
```

定义扩展方法的类必须为static类

仅可扩展顶级类，即无法扩展密封的嵌套类；同时，扩展方法对普通类也适用

某种程度上，类似于“友元方法”

## 委托构造函数

```c#
class BaseClass{
    public string strA;
    public BaseClass(string a){
        this.strA = a;
    }
}
class DerivedClass : BaseClass{
    public string strB;
    public DerivedClass(string a, string b):base(a){//调用基类的构造函数
        this.strB = b;
    }
    public DerivedClass():this("Hello", "World"){//调用该类的参数匹配的构造函数
        //
    }
}
DerviedClass test = new DerviedClass();
//调用堆栈
//object
BaseClass(string a);
DerviedClass(string a, string b);
DerviedClass();
```

## 继承

基类方法调用

```c#
void DoSomething(){
    base.DoSomething();
}
```





# 属性

声明一个属性Test

```c#
public int Test{
    public get; //取值方法
    public set; //存值方法
}
```

属性本质上只是字段和方法的集合而已



# 接口

## 声明

~~~C#
interface ITestInterface{
    int Arguments{get;set};
    void TestProcess();
}
~~~

接口声明，接口中的属性与方法必须为public

## 使用

```C#
//隐式实现
class TEST:ITestInterface{
    int Arguments{get;set}
    public void TestProcess(){
        //do something
    }
}
//显式实现
class TEST:ITestInterface{
    void ITestInterface.TestProcess(){
        //do something
    }
}
```

类在声明一个接口后，必须实现它，实现来源可以来自其基类(*可以通过VS自动实现接口，默认为throw*)

接口无法实例化，但是可以引用实现了接口的对象(包括由初始化而来的对象)，当接口被类显式实现的时候，则只能通过接口来访问接口成员

```c#
TEST test = new TeST();
ITestInterface R = test;
ITestInterface R = new TEST();
```

将接口名作为参数名，任何继承了该接口的类都可以作为参数传入

如果要确定传入的参数类类型，可以**is**来查看确定其类型

```c#
R is TEST;//检查接口印用R指向的是否为TEST，返回Bool值
```
同时，通过**is**可以查看某个类是否实现了某个接口

~~~c#
TEST S = new TEST();
S is ITestInterface;//返回Bool值
~~~
如果要以以按特定类的方法使用接口印用的对方，则可以用**as**来转换(向下强制类型转换，转换失败则指向null)

```C#
TEST T = R as TEST;//T储存了R指向的TEST对象
```

派生类会继承基类的接口 

## 继承

```c#
interface IAdvancedTestInterface:ItestInterface{
    int Arguments2{get;set}
    void AdvancedTEstProcess();
}
```

如果一个类实现了一个继承接口，则该类也要实现继承接口的基接口

另外需要注意的

```c#
interface IPlay(){
    void Play();
}
class YOYO : IPlay{
    public void Play(){
        Console.WriteLine("Is playing yoyo!");
    }
}
class MetalYOYO : YOYO{
    public new void Play(){
        Console.WriteLine("Is playing Metalyoyo!");
    }
}

IPlay play  = new MetalYOYO();
YOYO yoyo = new MetalYOYO();
play.Play();//输出Is playing yoyo!"
yoyo.Play();//输出Is playing yoyo!"
```

在这里由于基类的YOYO里没有把Play声明为虚方法，所以如果使用基类或者接口引用派生类MetalYOYO，调用Play方法依然是调用基类的方法

## 限制

接口不能包含字段，构造函数，析构函数，静态成员或常量



# 集合，比较和转换

自定义集合类

通常需要继承于CollectionBase类，该类含有接口IEnumerable, ICollection, IList

此外还有DictionaryBase类

## 集合

```c#
class YOYO{
    public string Name{get;set}
    public int width{get;set}
}
```

继承**CollectionBase**的简单集合

~~~c#
class YOYOS : CollectionBase {
    public void Add(YOYO yoyo){
        List.Add(YOYO);
    }
    public void Remove(YOYO yoyo){
        List.Remove(yoyo);
    }
    public void RemoveAt(int index){
        list.RemoveAt(index);
    }
    public YOYO this[int index]{
        get{return (List[index] as YOYO);}
        set{List[index] = value;}
    }
}
~~~

继承自**DictionaryBase**的简单集合

```c#
class YOYOS : DictionaryBase {
    public void Add(YOYO yoyo){
        Dictionary.Add(yoyo.Name, yoyo);
    }
    public void Remove(YOYO yoyo){
        Dictionary.Remove(yoyo.Name);
    }
    public YOYO this[string name]{
        get{return (Dicitonary[name] as YOYO);}
        set{Dicitonary[name] = value;}
    }
    public void Display(){
        foreach(DictionaryEntry item in Dictionary){
            YOYO yoyo = item as YOYO;
            WriteLine(yoyo);
        }
    }
}
```



## 索引符

定义

```c#
public class YOYOCollection : CollectionBase{
    public YOYO this[int index]{
        get{
            return (YOYO)List[index];
        }
        set{
            List[index] = value
        }
    }
}
```

## 迭代器

GetEnumerator()：用于迭代类，返回类型是IEnumerator

```c#
//定义
public IEnumerator GetEnumerator(){
    yield return "string 1";
    yield return "string 2";
    yield return "string 3";
}
//使用
foreach(string item in GetEnumberator()){
    WriteLine(item);
}
//定义在类中
class SayHelloWorld : IEnumerable{//IEnumerable可以省略
    public int sayTimes;
    public SayHelloWorld(int i){
        this.sayTimes = i;
    }
    public IEnumerator GetEnumerator(){
        for (int i = 0; i < sayTimes; ++i) {
            yield return "Hello World!".substring(0,i);
        }
    }
}
//使用
SayHelloWorld shw = new SayHelloWorld(10);
foreach (string hw in shw) {
    WriteLine(hw);
}
```

IEnumerable：用于迭代类成员

```c#
class YOYO{
    public string Name{get;set;}
    public int Width{get;set;}
}
class YOYOS : DictionaryBase, IEnumerable{
    public IEnumerable Names{
        get{
            foreach(DictionaryEntry item in Dictionary){
                YOYO yoyo = item.value as YOYO;
                yield return yoyo.name;
            }
        }
    }
    public void Add(YOYO yoyo) => Dictionary.Add(yoyo.Name, yoyo);
    public void Remove(YOYO yoyo) => Dictionary.Remove(yoyo);
    public YOYO this[string name]{get{return (Dictionary[name] as YOYO);}}
}
```



## 深度复制与浅度复制

```c#
class Content{
    private int val;
}
class Cloner : ICloneable{
    public Content TheContent;
    public Cloner(int val){
        this.TheContent.val = val;
    }
    //浅度复制, 使用MemberwiseClone()方法
    public object GetCopy() => this.MemberwiseClone();
    //深度复制,该方法继承自Icloneable接口
    public object Clone(){
        Cloner clonedCloner = new Cloner(this.TheContent.val);
        return clonedCloner;
    }
}
```

简单来说，如果一个对象中只有值类型，那么MemberwiseClone()即可达到完全的复制效果，但如果有引用类型，则应该处理每一个引用类型的复制

## 封箱与拆箱

封箱：把值类型转化为Object类型或接口类型

拆箱：将Object或者接口类型转化为相应的值类型

```c#
//对结构(值类型)
struct TheStruct : TheInterface{
    public int val;
}
TheStruct ts = new TheStruct();
ts.val = 5;
object refTs = ts;//这里的refTs是ts变量副本的引用
ts.val = 6;
WriteLine(((TheStruct)refTs).val);//输出依然是5

//对类(引用类型)
class TheClass{
    public int val;
}
TheClass tc = new TheClass();
tc.val = 5;
object refTc = tc;//这里的refTc即为tc
tc.val = 6;
WriteLine(((TheClass)refTc).val);//输出是6

//对接口
interface ITheInterface{
    
}
struct TheStruct : ITheInterace{
    public int val;
}
TheStruct ts = new TheStruct();
ts.val = 5;
ITheInterface iTs = ts;//这里iTs指向的内容由ts的类型所决定,相当于object
ts.val = 6;
WrlteLine(((TheStruct)iTs).val);//输出是
```

封箱将值类型转化为object类型，允许其使用object的方法

## 转换

is运算符：用于检查某种类型能否转换为特定类型

<operand>  is <type> [<variable>]

<variable>是可选的临时变量名

规则

1. 如果<type>是一个类类型，且<operand>也是该类型或者继承自该类型，或可以封箱为该类型，则返回true

2. 如果<type>是一个接口类型，且<operand>也是该类型或者实现该接口的类型，则返回true

3. 如果<type>是一个值类型，而<operand>也是该类型，或者可以拆箱到该类型中，则结果为true



typeof()与GetType()

前者返回参数的类型，后者返回调用的对象的类型



as运算符：将某种类型转化为指定的引用类型（最大的意义在于如果无法转换则返回null，而非抛出异常）

<operand> as <type>

规则

1. <operand>是<type>
2. <operand>可以隐式转换为<type>
3. <operand>可以封箱到<type>
4. 如果不符合以上条件，则返回null



转换运算符重载

用implicit与explicit声明转换为隐式或显式

~~~c#
class INT{
    public int val;
    public static implicit operator DOUBLE(INT i){
        DOUBLE returnVal = new DOUBLE();
        returnVal.val = i.val;
        return returnVal;
    }
}
class DOUBLE{
    public double val;
    public static explicit operator INT(DOUBLE i){
        INT returnVal = new INT();
        returnVal.val = (int)i.val;
        return returnVal;
    }
}
~~~



## 比较

对于类类型，==运算符默认比较引用是否相同

可通过运算符重载

```c#
static public operator ==(T a, T b){
    //
}
```

IComparable：在要比较的对象的类中实现，可以比较该对象与另一对象

IComparer：在一个单独的类中实现，可以任意两个对象

Comparer有一个接口IComparer的默认实现代码，位于Comparer.Default.Compare()

规则：

1：检查是否支持IComparable，如果支持，则使用该实现代码

2：null值将被解释为小于其他任何对象

3：若字符串为其他语言，则Comparer类需使用构造函数

使用（以ArrayList的sort()方法为例)

```c#
class YOYO : IComparable{
    public string Name;
    public int width;
    public YOYO(){}//省略构造函数了
    public int CompareTo(object obj){
        if (obj is YOYO) {
            YOYO yoyo2 = obj as YOYO;
            return this.width = yoyo2.width;
        } else {
            throw new ArgumentException("...");
        }
    }
}
class YOYOComparerName : IComparer{
    public static IComparer Default = new YOYOComparerName;
    public int Compare(object x, object y){
        if (x is YOYO && y is YOYO) {
            return Comparer.Default.Compare(
            (x as YOYO).Name, (y as YOYO).Name);
        }else{
            throw new ArgumentException("...");
        }
    }
}
ArrayList yoyos = new ArrayList();
yoyos.Add(new YOYO());
yoyos.Add(new YOYO());
...
yoyos.sort();//默认排序，将使用YOYO类的CompareTo作为比较方式
yoyos.sort(YOYOComparerName.Default);//使用YOYOComparerName类的排序方法排序
```



# 泛型

## 可空值类型

允许使用null的值类型

```c#
System.Nullable<int> num = null;//一个可空的int类型
int? num = null;//同上，简写
num.HasValue();//用于检查是否为null，仅能用于可空值类型
//用于计算
int? op1 = 5;
int res = op1 * 5;//错误
int? res = op1 * 5;//正确
int res = op1.Value * 5;//正确
int res = (int)op1 * 5;//正确
```

??运算符（空接合运算符）

```c#
op1 ?? op2;//如果op1不为null，则返回op1，否则返回op2，并且会进行隐式转换为非空值类型
op1 == null ? op2 : op1;
```

?.运算符（空条件运算符）

```c#
op?.GetValue();//如果op为null，则返回null，而非抛出异常
```

## 泛型集合

IComparable<T>与IComparer<T>

用于对List<T>排序

Comparison<T>：用于泛型集合排序方法

Predicate<T>：用于泛型集合搜索方法

```c#
List<YOYO> yoyos = new List<YOYO>();
...
Comparison<YOYO> sorter = new Comparison<YOYO>(methodName);//method为方法名
yoyos.sort(sorter);//使用sorter进行排序
yoyos.sort(methodName);//同上，但是会隐式生成Comparison<T>委托
```

Dictionary<K, V>

如果要将类用作键，且该类不支持IComparable或IComparable<T>接口，则需要把IComparer<T>接口传递给其构造函数

```c#
Dictionary<string, int> things = 
    new Dictionary<string, int>(StringComparer.CurrentCultureIgnoreCa);
```

索引初始化器

```c#
Dictionary<string, int> things = new Dictionary<string, int>(){
    ["AAA"] = 1,
    ["BBB"] = 2,
    ["CCC"] = 3
};
```

## 泛型类

### 无绑定类型

定义

```c#
class GenericClass<T>{
    private T innerObject;
    public GenericClass(){
        innerObject = default(T);//default关键之，若T为值类型，赋值为0，若为引用类型，赋值为null
    }
    public GenericClass(T t){
        innerObject = t;
    }
    public T InnerObject{
        get{return innerObject;}
    }
}
```

限制在于，不能假设泛型T的类型，因此对于构造函数，比较运算符的使用存在限制

关键字

### 约束类型

定义

```c#
class HES{}
class YOYO : HES{}
class Cube : HES{}
class Spin : HES{}
class GenericClass<T> where T : HES， IComparable{
    //T的类型只能为HES或者其子类，且必须实现IComparable接口
}
class GenericClass<T1, T2> 
    where T1 : YOYO
    where T2 : HES{
    //多泛型类型约束
}
```

使用where关键字限定泛型的允许类型，where关键字必须位于继承说明符之后

where后的约束类型遵循继承原则

类类型约束必须位于首位（潜在限制即类类型约束只能有一个参数）

约束类型说明

|            |                                                            |
| ---------- | ---------------------------------------------------------- |
| class      | 类型为引用类型                                             |
| struct     | 类型为值类型                                               |
| base-class | 类型必须是积累或者继承自基类，可以根据这个约束提供任意类名 |
| interface  | 类型必须是接口或者实现了该接口                             |
| new()      | 类型必须有一个公共的无参构造函数                           |

### 继承

限制

1：如果基类型受到约束，则继承类无法解除约束

2：继承时必须提供基类的必要信息

若给泛型类型提供了参数，则称该类型为“关闭的泛型”，否则称为“打开的泛型”

### 泛型运算符

```c#
public static implicit operator List<HES>(HES<T> hes){
    //将HES<T>转换为List<YOYO>
}
public static HES<T> operator +(HES<T> hes1, List<HES> hes2){
    //HES<T>与List<HES>相加运算
}
```

## 泛型接口&泛型接口

基本规则同泛型类

## 泛型方法

非泛型类下的泛型方法

```c#
public class Defaulter {
    public T GetDefault<T>() => default(T);
}
```

泛型类下的泛型方法

```c#
public class Defaulter<T1> {
	public T2 GetDefault<T2>()
        where T2 : T1{
            return default<T2>;
    }
}
```

注意泛型方法的泛型名不可与泛型类所用的相同

此外，泛型参数数量会改变泛型方法的签名

## 泛型委托

```c#
delegate T1 Delegate<T1, T2>(T1 op1, T2 op2) where T2 : T2{}//基本同类
```

## 变体

### 协变

只有泛型接口和泛型委托支持变体

问题

```c#
interface IPLay<T>{
    public void Play();
}
class HES {
    public virtual void Play(){
        WriteLine("Is Playing hes");
    }
}
class YOYO : HES, IPLay<YOYO>{
    public override void Play(){
        WriteLine("Is Playing YOYO");
    }
}
YOYO yoyo = new YOYO():
IPlay<YOYO> yoyoPlay = yoyo;
IPlay<HES> hesPlay = yoyoPlay;//这一句无法通过编译，尽管YOYO是HES的子类
IPlay<HES> hesPlay2 = yoyo;//同上，无法通过编译
```

上述问题可以通过协变解决：协变，允许在泛型之间建立继承关系

```c#
public interface IPlay<out T>{}//将参数T定义为协变
```

使用以上语句后，开头无法通过编译的代码便可以通过编译

注意：对于接口定义，协变类型参数只能用作方法的返回值或属性get访问器

### 抗变

和协变类似，不同的是抗变用于允许基类传递给派生类，如下

```c#
public interface IPLay<in T>{
    public void Play();
}
class HES : IPLay<HES>{
    public void Play(){
        WriteLine("Is Playing HES");
    }
}
class YOYO : HES{
    public void Play(){
        WriteLine("Is Playing YOYO");
    }
}
HES hes = new HES();
IPlay<HES> hesPlayer = hes;
IPlay<YOYO> yoyoPlayer = hes;//虽然很奇怪，但这一段代码是可以通过编译的
```

注意：对于接口类型，抗变只能用作方法参数，而不能用作返回类型

# 其他

::运算符：强制编译器使用using定义的别名

```c#
using C = A.B;
namespace A{
	namespace B{
        public class CLASS{}
    }
    namespace C{
        public class CLASS{}
    }
}
C.CLASS;//这里调用的是A.C下的CLASS
C::CLASS;//这里调用的是A.B下的CLASS
```

global关键字：顶级名称空间的别名

## 事件

~~~c#
using System;
using System.Timers;

namespace TEST {
    public delegate void MessageHander(string messageText);
    public class Connection {
        private static Random random = new Random();
        public string Name { get; set; }
        public event EventHandler<MessageArrivedEventArgs> MessageArrived;
        private Timer pollTimer;
        public Connection() {
            this.pollTimer = new Timer(100);
            this.pollTimer.Elapsed +=
                delegate (object source, ElapsedEventArgs e) {
                    Console.WriteLine("Check for new message");
                    if (random.Next(9) == 0 && MessageArrived != null) {
                        MessageArrived(this, new MessageArrivedEventArgs("Hello World"));
                    }
                };
        }
        public void Connnect() => this.pollTimer.Start();
        public void Dis() => this.pollTimer.Stop();
    }

    public class Display {
        public void DisplayMessage(object source, MessageArrivedEventArgs e) {
            Console.WriteLine($"Message arrived from: {((Connection)source).Name}");
            Console.WriteLine("Message arrived :{0}", e.Message);
        }
    }

    public class MessageArrivedEventArgs : EventArgs {
        private string message;
        public string Message {
            get {
                return this.message;
            }
        }
        public MessageArrivedEventArgs() => this.message = "no message sent";
        public MessageArrivedEventArgs(string newMessage) => this.message = newMessage;
    }

    class Program {
        static void Main(string[] args) {
            Connection connection1 = new Connection();
            connection1.Name = "(1) connection";
            Connection connection2 = new Connection();
            connection2.Name = "(2) connection";

            Display display = new Display();

            connection1.MessageArrived += display.DisplayMessage;
            connection2.MessageArrived += display.DisplayMessage;
            connection1.Connnect();
            connection2.Connnect();

            System.Threading.Thread.Sleep(200);
            Console.ReadKey();
        }
    }
}

~~~

匿名方法

```c#
Button.Click +=
    delegate(object sender, RoutedEventArgs e){
    Console.WriteLine("Test");
};
```

## 特性

自定义特性：使用**System.Attribute**派生，允许提供带参构造函数，同时使用AttributeUsageAttribute特性与AttributeTarget设置特性**可应用的目标**和**是否允许多次应用**，

```c#
[AttributeUsage(AttributeTargets.Class|AttributeTargets.Method,AllowMultiple = false)]
class DoesInterestingThingsAttribute : Attribute{
    public DoesInterestingThingsAttribute(int howManyTimes){
        this.HowManyTimes = howManyTimes;
    }
    public string WhatDoesItDo{get;set;}
    public int HowManyTimes{get;private set;}
}
```

读取特性：使用反射

```c#
[DoesInterestingThings(1000, WhatDoesItDo = "Hello World!")]
class DecoratedClass{ }

class Program{
    static void Main(string[] args){
        Type classType = typeof(DecoratedClass);
        object[] customAttributes = classType.GetCustomAttributes(true);
        foreach(object attribute in customAttributes){
            DoesInterestingThingsAttribute A = attribute as DoesInterestingThingsAttribute;
            if (A != null){
                Console.WriteLine($"This attribute does {A.WhatDoesItDo} x {A.HowManyTimes}");
            }
        }
    }
}
```

## 初始化器

对象初始化器

```c#
class YOYO{
    public YOYOType TypeOfYOYO{get;set;}
    public string Name{get;set;}
    public double Width{get;set;}
    public double Weight{get;set;}
}
class YOYOType{
    public string TypeName {get;set;}
    public int ID{get;set;}
}

YOYO yoyo = new YOYO{//可以使用括号调用特定构造函数，若省略括号则采用默认构造函数
    Name = "AAA",
    Width = 56,
    Weight = 64,
    TypeOfYOYO = new YOYOType{//允许嵌套
        Typename = "NormalYOYO",
        ID = 1
    }
};
```

集合初始化器

```c#
List<YOYO> yoyos = new List<YOYO>{
    new YOYO{
        Name = "A", Width = 56, Weight = 64
    },
    new YOYO{
        Name = "B", Width = 56, Weight = 64
    },
    new YOYO{
        Name = "C", Width = 56, Weight = 64
    }
};//注意会调用yoyos的Add方法，因此若yoyos集合没有Add方法，需要考虑嵌套访问
```

## 匿名类型

匿名类

```c#
class YOYO{
    public string Name {get;set;}
    public double Width {get;set;}
    public double Weight {get;set;}
}
YOYO yoyo = new YOYO{
	Name = "AAA", Width = 65.5, Weight = 64
};

//对应的匿名类型
var yoyo = new {
    Name = "AAA", Width = 65.5, Weight = 64
};
```

匿名集合

```c#
var yoyos = new[]{
    new {Name = "AAA", Width = 65.5, Weight = 64},
    new {Name = "BBB", Width = 65.6, Weight = 64}
};
//注意由于两个元素参数相同，只会创建一个匿名类
```

## 动态类型

```c#
dynamic Obj;
Obj = 1;
Obj = "Hello World!";
Obj = new{
    Name = "AAA", Width = 65, Weight = 64
};
//上述代码是可通过编译的

dynamic Obj;
Obj = MethodThatGetAMethod();
Obj.Add(2,3);//尝试访问Obj的Add方法，如果该方法不存在，则抛出RuntimeBinderException异常
```

动态类型一般用于处理未知类型的C#对象，例如处理其他动态语言

# 文件IO

```c#
StreamReader sr = new StreamReader("Filename");//读
sr.ReadLine()//读取一行
StreamWriter sw = new StreamWriter("Filename");//写
wr.Write()//写入
```

使用@表示路径，或者不要使用反斜杠\

注意使用Close()关闭文件流，或者使用using语句(代码块完成后自动调用Dispose()方法)

```c#
using (StreamWriter sw = new StreamWriter("filename")) //注意没有;
using (StreamReader sr = new StreamReader("filename")){
    //Do something
}
```

流再传输的过程中可以修改，例如，可以在写入的时候加密，称为0串流

## WinForm下的SaveFileDialog与OpenFileDialog

```c#
control.Name;//获取读取的文件名
control.Title = "AAA";//设置标题
control.InitialDirectory = @"C:\";//设置初始文件夹（必须使用反斜杠)
control.Filter = "txtfile|(*.txt)";//设置文件筛选器
```

## WPF中的文件对话框

~~~c#
OpenFileDialog file = new OpenFileDialog();
file.Title = "打开文件"; //设置打开窗口的标题 
file.InitialDirectory = 
    Environment.GetFolderPath(Enviroment.SpecialFolder.MyDocuments);//设置起始路径为用户文档
file.Filter = "PNG|*.PNG|JPG|*.JPG|BMP|*.BMP"; //文件过滤器
if(file.ShowDialog() == true){ //ShowDialog()返回一个bool值，表示是否选择了文件
    MessageBox.Show(file.FileName); //FileName返回完整路径
}
~~~



## (反)序列化

```c#
[Serializable] //添加可序列化的attribute
class ClassName{
    //Class Definition
}

using System.Runtime.Serialization.Formatters.Binary;
BinaryForamtter formatter = new Bianary BinaryForamtter();
//序列化
ClassName test = new ClassName();
using (Stream output = File.Create("filename")){
    formatter.Serialize(output, test);
}
//反序列化
using (Stream input = File.OpenRead("filename	")){
    ClassName test = (ClassName)formatter.Deserizlie(input);//注意类型转换
}
```

## 二进制文件读写

~~~c#
BinaryWriter writer = new BinaryWriter(stream);//一个二进制写入对象，绑定stream
byte[] bytes = File.ReadAllBytes(filename);//读取filename里的所有字节并储存到bytes

BinaryReader reader = new BinaryReader(stream);//一个二进制读取对象，绑定stream
int varable = reader.ReadInt32();//reader对象以int读取文件内容
byte[] varable = reader.ReadBytes(5);//读取5个byte
File.Read();//直接从流中读取字节
~~~

# 数据绑定

## Xaml + C#

假设有如下类定义

```c#
class YOYO{
    private double width;
    private double weight;
    public string yoyodata{
        get{
            return $"{{width}mm, {height}g}";
		}
    }
    public YOYO(double width, double weight){
        this.width = width;
        this.wight = weight;
    }
}
```

如下Xaml标记

```xaml
<TextBlock x:Name="text" Text="{Binding yoyodata}"/>
```

### TextBlock分段

```xaml
<TextBlock x:Name="text">
	<Run Text="First Line" x:Name="FirstLine"/>
    <LineBreak/>
    <Run Text="Second Line" x:Name="SecondLine"/>
</TextBlock>
```

如下代码隐藏

```c#
YOYO yoyo = new YOYO(55, 64);
this.text.DataContext = yoyo;
```

那么在窗口中，名为text的TextBlock的Text属性就会变为yoyo.yoyodata的内容

注意，Binding的数据必须为属性，如果是字段将不会显示

## 仅C#

```C#
//创建一个数据绑定对象
Binding dataBinding = new Binding();
dataBinding.Path = new PropertyPath("yoyodata");
dataBinding.Source = yoyo;
this.data.SetBinding(Text.TextProperty, dataBinding);
```

数据绑定具有传递性

# 异步调用

 如果方法中有await的方法，则方法必须有asycn修饰

#  结构

与类的主要不同点

1. 无法继承
2. 是值类型

# 数据处理

## LINQ

```C#
//查询
int[] values = new int[]{0,10,20,30,40,50};
IEnumerable<int> result = 
    		 from v in values //从value中查询
    		 where v < 25 //选择小于25的数
    		 orderby v ascending//排序选择的v(按升序)
    		 //orderby v descending //降序排列
      		 select v; //返回结果(小于25的数)
//将result转成列表
List<int> intList = result.ToList();
//取strList前2个元素
IEnumberable<int> firstTwo = intList.Take(2);

//修改
string[] strs = new string[]{"AAA","BBB","CCC"};
IEnumerable<string> result = 
    		 from str in strs
    		 select str + "[]";

//为实现了IEnumerable<T>的类提供的扩展方法
IEnumberable<T> test = ...;
test.Max();
test.Min();
...
    
//分组
class YOYO{
    public string Color;
    public int width;
}
YOYO[] yoyos = new YOYO[]{...};
IOrderedEnumerable<<IGrouping<int, YOYO>> groupByWidth = 
    from yoyo in yoyos
    group yoyo by yoyo.width //按widh分组
    into groups //把分组结果放于groupByWidth中
    orderby groups.Key //以分组的键排序(这里是width)
    select groups; //返回结果
//分组访问
foreach(<IGrouping<int, YOYO> yg in groupByWidth){
    WriteLine($"{yg.key} contains {yg.count()}: ");
    foreach(YOYO yoyo in yg){
        WriteLine($"{yoyo.Color}, {yoyo.Width}");
    }
}

//合并查询
YOYO[] yoyosA = new YOYO[]{...};
YOYO[] yoyosB = new YOYO[]{...};
var yoyos = 
    from yoyoa in yoyosA
    join yoyob in yoyosB //加入比对列表
    on yoyoa.Color //1号列表要比较的数据
    equals yoyob.color //2号列表要比较数据
    orderby yoyoa.width
    select new {yoyoa.color, yoyob,width}//由于返回的是匿名类型，故需要将类型声明为var
//访问
foreach(var items in yoyos){
	WriteLine($"{item.color}, {item.Width}");
}

```

访问LINQ的查询结果之前，LINQ不会进行查询（延迟计算）

# 事件与委托

特点：对象关注点分离，即对象只需要考虑自己的行为

由对象发起事件，任何订购了该事件的对象都会回应

一个事件有一个发布者，但可以由多个订购者，即：即一个对象产生事件，其他对象监听

产生事件后会有参数，这些参数作为一个EventArgs对象实例关联到事件

事件处理方法可以串联

```c#
private void SayMove(object sender, EventArgs e){
    MessageBox.Show("Moving now!");
}
private void SayRotate(object sender, EventArgs e){
    MessageBox.Show("Rotating now!");
}
this.RobotCommand.Click += SayMove;
this.RobotCommand.Click += SayRotate;
//当点击RobotCommand时，就会弹出两个对话框
```

# 杂项

## 一个有问题的封装

~~~c#
namespace TEST{
    //一个简单的类
    class PrivateValue{
        private int value = 10;
        public void Reset(){
            this.value = 20;
        }
        public override string ToString(){
            return value.ToString();
        }
    }
    //一个使用了上面简单类的简单类
    class Modifier{
        private PrivateValue pv = new PrivateValue();
        public PrivateValue Pv{
            get{
                return pv;
            }
        }
        public override string ToString(){
            return pv.ToString();
        }
    }
    //使用Modifier
    class MAIN{
        public static void Main(string[] args){
            Modifier temp = new Modifier();
            PrivateValue pv = temp.Pv;
            //
            WriteLine($"Before Reset(): {temp}");
            //
            pv.Reset();
            WriteLine($"After Reset(): {temp}");
            return;
        }
    }
}
//输出如下
//Before Reset(): 10
//After Rest(): 20
~~~

很奇怪的，明明在Modifier里把PrivateValue声明为私有字段，依然会导致其被外部引用改变。原因在于其属性Pv返回的是pv的引用。这点需要注意