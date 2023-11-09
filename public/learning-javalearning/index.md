# Java Learning

Java learning notes.

<!--more-->

## JetBrains JavaDeveloper

--20220511

### Paradigms

Different approaches to creating programs are called paradigms.

#### Imperative 

- how to do
- focuses on achieving a result using step-by-step instructions that change the data sequentially.
- includes Procedural programming paradigm, Object-oriented programming, and Parallel processing approach.

#### Declarative

- what to do
- focuses on the task and tries to get an expected result.
- includes Logic, Functional, and Database paradigms.

## 类与对象

### 方法
```java
public class 三_1 {
int m=10,n;//正确，成员变量的操作只能放在方法中，声明时可以赋初值
n=100;//报错，赋值只能放在方法中，不能在类体
}
```
### 构造方法
默认有一个无参数构造函数，但是如果指定了有参数的构造函数，**就需要自己写一个无参数的**。

java中的构造函数无指定默认参数功能，但可以通过方法重载实现：

```java
class  Lader{
    private double up, down, height, size;
        double laderSize(){
        //返回面积
        size=(up+down)*height/2;
        return size;
    }
//Java支持方法重载，间接实现构造函数默认参数，
// 因此没有如C++中默认参数的功能
    Lader(double u, double d){
        up=u;
        down=d;
    }

    Lader(double u, double d, double h){
        up=u;
        down=d;
        height=h;
    }
}
```
### this
表示某个对象，可以出现在实例方法和构造方法中
### 局部变量
- 局部变量没有默认值，必须初始化。
- 成员变量有默认值，可以不初始化。
### 实例成员与类成员
- 用static 修饰的称作类变量、类方法（static 方法、静态方法）。可以通过类名调用

- 类方法
    >不能操作实例变量。在类创建对象之前实例成员还没有分配内存。
    
    >也不能调用类中的实例方法

- 其他则成为实例变量、实例方法。

- 实例方法可以操作实例变量、类变量，调用实例方法、类方法（不包括构造方法）
- static 变量在实例创建之前就已经被初始化

### 可变参数
```java
class E_4{

    public static void main(String args[]){
        f(1,2);
        f(-2,-2,-3,-4);
    }
    public static void f(int ... x){//x是可变参数的代表，代表若干个int型参数
        for(int i=0;i<x.length;i++)
            System.out.println(x[i]);//类似数组，代表第i个参数
    }
}
```

## Java Library
### ArrayList
### format
- &d decimal:
>`format("%d",42);`
> 
>42
- %f floating point：
>`format("%.3f",42.00000000);`
>
>42.000
- %x hexadecimal十六进制：
>`format("%.3f",42.00000000);`
>
>42.000
- %c character
>`format("%c",42);
>//42表示 char "*"`
>*
>
- 多个argument
- `format("The rank is %,d out of &,.2f",one,two);`
  
    > The rank is 20,456,654 out of 100,567,890.25

## 子类与继承

子类is a 父类
```java
class Sum{
    //...
}
class Average extends Sum{
    //...Sum的子类
}
```

- **Single Parent**
- 子类不继承父类的构造方法（默认调用父类不带参数的构造方法），可以使用super()调用父类的其他构造方法

- super()必须是构造方法的第一个语句

- 当创建一个子类，总是先调用父类的构造方法再调用子类的

- 默认所有的类继承自Object类
- instanceof:左边的操作元素是右面的类或其子类所创建的对象时为true
### 子类的继承性
在同一包：除了private

不同包：除了private 和 default，即继承protected 和public

权限由高到低：public protected default private 
### 成员变量的隐藏与方法重写
#### 成员变量的隐藏
子类声明的成员变量名字与从父类继承来的成员变量名字相同，类型可以不同

子类继承的方法只能操作子类继承和隐藏的成员变量

子类重写或新增的方法能操作子类继承和新声明的成员变量，不能操作隐藏的成员的变量
#### 重写 override
- 子类方法必须与父类保持类型一致，
- 与重载 overload不同
- 不能降低方法的访问权限，只能提升
- 不可以把父类的实例方法重写为类（static）方法，也不可以把父类的类方法重写为实例方法。
### super
super必须是子类构造方法的第一句

调用被隐藏的成员变量与方法

默认调用父类不带参数的构造方法

### final
- final类不能被继承

- final方法不能被重写

- final变量为常量，声明时必须指定值。
- 不可以用final修饰构造方法
- final static 变量：
    ```java
    //初始化final static 变量的两种方式
    class Foo{
        final static int X=25;
        //final: value doesn't change
        //static: don't need an instance of object
    }
    //or
    class Bar{
        static final double X;
        static{
        //static initializer 在类加载前就执行的代码块
            X=1.0;
        }
    }
    ```
## 抽象
### 抽象类
如Animals类。不应该被实例化，因此定义为抽象类，让其无法被new

抽象类可以有抽象方法和非抽象方法
### 抽象方法
必须被重写的方法，which has no body!
没有大括号

`public abstract void eat();`

- 抽象方法必须放在抽象类里。
- 必须重写所有的抽象方法

- 为什么要有抽象方法？为了多态性，所有的子类都有这个方法
### 上转型对象
### 多重继承
- java不支持多重继承
- 但是可以用`interface`实现多重继承的功能

## 接口 interface
```java
public interface Pet{
    void beFriendly();
    public abstract void play();
    //接口所有方法默认为public abstract,可以省略
}
public class Dog extends Canine implements Pet{
    //...
}
```

- 允许除了抽象方法和常量以外的：
> default 实例方法（必须是public）（不可以省略default,可以省略public）
> （不可以定义default的static方法）//jdk8
> 
> static 方法//jdk8
> 
> private 方法//jdk9
> 
1. 一个class可以implements 多个interface
>`class Dog extends Animal implements Eatable,Sleepable{}`
2. 重写接口中的方法
- 重写default方法需要去掉default
- 类不拥有接口的static方法和private方法
- 除了private以外，其他方法默认为public，重写时不可省略，否则降低了访问权限
- abstract类可以选择性重写，非abstract类必须全部重写
3. 可以用接口名访问接口的常量、调用接口中的static方法
4. 接口的细节
- public接口可以被任意类实现（implements）
- 友好接口（不加public）只能被同包中的类实现
- 父类实现接口即子类也实现了，不必再implements

### 接口的继承
- 接口可以继承
- 一个接口可以有多个父接口

```java
interface Com{
  int M=200;
  int f();
}
class ImpCom implements Com{
  //错! 返回类型改变，参数类型未改变
  public double f(){
  return 2.6;
  }
  
  //正确
  public int f(){
  return M+100;
  }
}
```
### 接口回调
把接口实现的类创建的对象引用赋值给接口声明的变量，那么该接口变量可以调用被类实现的方法以及接口提供的default方法。


## 多态性
- 静态多态性
>方法重载
- 动态多态性
>   将子类对象的引用放进父类：
>   上转型对象a: 不能访问子类新增的方法和变量
>   `A a = new B()//B为子类`







## 内部类和异常类

### 内部类

指一个类中定义的另一个类

- 内部类中不可以声明类变量和类方法。

- static内部类不能操作外嵌类中的实例变量成员

### 匿名类
- 匿名类一定是内部类，是在某个类中直接用匿名类创建对象。
- 编译器会给匿名类一个名字
- 在匿名类中不可以声明static成员变量和static方法
```java
 abstract class Bank{
    int money;
    public Bank(){
        money =100;
    }
    public Bank(int money){
        this.money=money;
    }
    public abstract void output();
}
 class ShowBank{
    void showMess(Bank bank){
        bank.output();
    }
}
class Example{
    public static void main(String[] args) {
        ShowBank showBank = new ShowBank();
        showBank.showMess(new Bank() {//匿名类的类体
            @Override
            public void output() {
                money += 100;
                System.out.println(money);
            }
        });
        showBank.showMess(new Bank(500) {//匿名类的类体
            @Override
            public void output() {
                    money+=100;
                System.out.println(money);
            }
        });
    }
}
```
### 异常类

try catch finally

```java
try{
    turnOvenOn();
    x.bake();
}catch (BakingException ex){
    ex.printStackTrace();//recover code
}finally{
    turnOvenOff();
}

```

throw抛出异常

throws声明异常

```java
public class BankException extends Exception{
    String message;
    public BankExceltion(int m ,int n){
        message="入账资金"+ m +"是负数或支出"+ n +"是正数，不符合系统要求";
    }
    public String warnMess(){
        return message;
    }
}
public class Bank{
    //...
    public void income(int in,int out)throws BankException{
        if(in<=0||out>=0||int+out<=0){
            throw new BankException(in,out);
        }
    }
    //...
}
```



### 异常分类

可控异常(检测型异常)：预期可能发生的，必须 处理

- I/O输入输出

执行时异常：可以不处理


### 断言
`assert booleanExpression;`
若表达式booleanExpression的值为true，程序继续执行，否则程序立刻结束执行。

`assert booleanExpression;`
若表达式booleanExpression的值为true，程序继续执行，否则程序立刻结束执行，并输出messageException的值。
## 常用使用类
### 正则表达式
e.g.

```java
''\\dcat';
\d //元字符,表示0-9中任意数
```

```
public boolean matches(String regex){}//regex表示正则表达式
//调用：
str.matches(regex);//判断字符串是否与正则表达式匹配
```

```
replaceAll();
public String split(String regex);//使用参数指定的正则表达式regex作为分隔标记
```

```
StringTokenlizer对象是一个分析器
```





## GUI

Three ways to put things on GUI:
1. put widgets on a frame
2. draw 2D graphics on a widget
3. put a JPEG on a widget

- Swing包
- AWT包


## Saving objects 
### *Serializable* 可串行化的
Use an ObjectOutputStream(from the java.io package) to serialize an object.
{{< admonition note "Note" >}}

a interface known as a marker or tag interface:
doesn't have methods to implement

*Most classes will and should, implements Serializable*

{{< /admonition >}}

announce an class is *serinalizable* -> objects of the type *saveable*

superclass *serializable* -> subclasses *serializable* automatically 

#### transient 
an instance variable as transient if it **can’t (or shouldn’t) be saved**.



{{< admonition warning "Exception" >}}

否则，在一个*Srializable* 的类中有非*Srializable* 的 instance variable会引起错误。

```java
import java.net.*; 
class Chat implements Serializable { 
    transient String currentID;
	String userName;
	// more code
}
```

{{< /admonition >}}
### Questions
{{< admonition question "Question"  true >}}

How is a non-serializable instance saved?

> - If you serialize an object, a transient reference instance variable will be brought back as null, regardless of the value it had at the time it was saved. 
>
> - To solve this:
>
> 1. Reinitialize the null null instance variable back to some default state.
> 2.  Save the key values and use them to essentially re-create a brand new variable that’s identical to the original.

What happens if two objects in the object graph are the same object?

> Serialization knows when two objects in the graph are the same.
>
>  In that case, only one of the objects is saved, and during deserialization, any references to that single object are
> restored.

{{< /admonition >}}

### Writing a String to a Text file
```java
import java.io.*;

class WriteAFile {
  public static void main(String[] args) {
    try{
     FileWriter writer = new FileWriter("Foo.txt");//若文件不存在，会自动创建
     writer.write("hello foo!");//必须是str
     writer.close();
    }catch (IOException ex){
       ex.printStackTrace();
    }
  }
}


```

## 常用实用类

### String



- final类

- 常量对象

- String对象

- +并置运算

  ```java
  String testOne="你"+"好";//两个常量并置运算，所得到的仍然是常量
  String you="你";
  String testTwo=you+"好"//只要有一个是变量，结果就是变量
  ```

- 常用方法

  - `public int length()`
  
  - `public boolean equals(String s)`
  
    **字符串不能用==直接比较**，这是比较地址！要用equals.
  
  ```java
  String tom=new String("天道酬勤")
  String jerry=new String("天道酬勤")
  ```
        tom.equals(jerry)的值是true
        tom==jerry的值是false:因为String对象tom、jerry中存放的是引用
  
  - `public boolean startsWith(String s)`
	- `public boolean endWith(String s)`
	- `public String sunstring(int startpoint)`
  `String tom = "我喜欢篮球";
  `String str = tom.substring(1,3);
  - `public String trim()`：去掉前后空格
  - 文件目录符：\\
  
### 正则表达式
- `[abc]`:  abc中任意一个

- `[^abc]`:除abc外任意一个

- `[a-zA-Z]`:英文字母中的任意一个

- `[a-d]`:a~d任意一个

- `[a-d[m-p]]`:a~d或m-p **并**

- `[a-z&&[def]]`:def中任意一个 **交**

- `[a-f&&[^bc]]`:adef **差**

- 限定修饰符：
  `String regex = "hello[2468]?";
  
  1. X?表示X出现1次或0次
  
  2. X*表示X出现0词或多次
  
  3. X+表示X出现1次或多次
  
- 匹配整数的正则表达式
  `String regex = "-?[1-9]\\d*";`
  
  `\\d`表示0-9中任意一个数
  
- 匹配浮点数
  `String regex = "-?[0-9][0-9]*[.][0-9]+";`
  [.]或\\.表示'.'，.表示任意字符。

- 匹配email
  
  `String regex = "\\w+@\\w+\\.[a-z]+(\\.[a-z]+)?";`

### 字符序列的替换
`String str = "12hello567bird".replaceAll("[a-zA-Z]+","你好";`
将 "12hello567bird"中的所有英文字符替换为"你好"
### 字符序列的分解
`split(String regex)`用正则表达式作为分割标记
### StringTokenlizer类
- 无需正则表达式作标记
- 使用默认的分隔标记：空格，换行，回车，制表，进纸符。
  指定分割标记：`StringTokenlizer fenxi = new StringTokenlizer(s,"(),");//(),的任意排列都是一个分隔符`
  

### Scanner类

1. Scanner类对象
   - 可以调用方法useDelimiter(正则表达式)，以正则表达式作为分割标记。如果不指定分割标记，则以空白字符作为分割标记。（空格，制表符，换行符）
   - next()方法依次返回解析的单词
   - nextInt()或nextDouble()代替next方法：将数字型单词转化为int或double
   - 若单词不是数字，使用nextInt()或nextDouble()将出现异常。 

1. Scanner与StringTokenlizer



### 输入和输出流

运行可执行文件:runtime

#### 文件字节



- 输入流FileInputStream

  InputStream子类

  1. 构造方法
  2. 读取字节
  3. 关闭流

- 输出流FileOutputStream

  OutputStream子类

#### 文件字符

更好地支持Unicode

- 输入流FileReader

  Reader的子类

- 输出流FileWriter

  Writer的子类

#### 缓冲流

BufferedReader和BufferedWriter

FileReader无法每次读取一行。而缓冲流可以。

- 构造方法:

  `BufferedReader(Reader in);`

  `BufferedWriter(Writer out);`

#### 随机流RandomAccessFile

构造方法：

1. RandomAccessFile(String name, String mode)
2. RandomAccessFile(File file, String mode)

mode:r(只读),rw(读写)

seek(long a)方法：

定位流的读写位置，a确定距离文件开头的字节个数



## Java多线程机制

### 12.2 Java中的线程

- main方法：主线程

- 线程状态与生命周期 Thread类

  1. 新建状态NEW状态

  2. 可运行状态：调用start方法

     **只有处于NEW状态的线程可以调用start方法**

  3. 中断状态

  4. 死亡状态

### 12.5 线程同步

synchronized(同步)修饰方法。

> 当一个线程A使用synchronized方法时，其他线程想使用这个方法就必须等待，直到A使用完。

### 12.6

wait()方法可以暂时中断线程的执行。

如果其他线程使用同步方法不需要等待，则使用完后应用notifyAll()通知其他处于等待的线程。

notify则只通知某一个线程。

- 不能在非同步方法中使用：wait(),notify(),和notifyAll();

- 它们都是Object类的final方法,被所有类继承且不允许重写。

