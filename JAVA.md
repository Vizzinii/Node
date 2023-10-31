## 顺序语句

方法调用：同cpp
赋值:同cpp
**JAVA中不存在“表达式语句”**

## 分支语句

if：同cpp
switch：同cpp

## 循环语句

for：同cpp 可以用 for(int age : ages) , 指**只读**地遍历 ages 中的元素
while：同cpp

## 特殊流程控制语句

break语句：同cpp，单一个break可以跳出当前语句块，也可以通过标签指明要终止的是哪一层语句，示例如下：
label1:  {
label2:     {
label3:        {
                   break label2;
               }
            }
         }  

continue语句：同cpp，其余同上

## 数组

**声明方式**： int[] arr; 方括号可以写到变量的前面或者后面
int[] a, b, c; 指a,b,c都是int的数组
int a[] , b , c; 指只有a是int数组，b,c是int类型的变量
*JAVA中数组与cpp有三点不同，如下*
**壹：声明时不能指定长度，因为只是引用，必须用 new 来生成**
**贰：数组一经分配空间就会被初始化，如int []a = new int[5]; 数值类型是0，引用类型是 null**
**叁：每个元素都有一个属性 length 来指明它的长度（元素个数）**
System.arraycopy 提供了数组元素复制的功能
**JAVA的多维数组要从高维向低维赋值，先声明高维数组有多少个元素，然后每个元素再是一个数组。像cpp那样从低维到高维的声明初始化顺序在JAVA是违法的。

## 类
分为字段（属性）和方法（功能操作）。
构造函数：同cpp
定义类对象： new
调用：同cpp
重载：同cpp

## this指针
是 this.name ，不是cpp的 this->name
可以在构造函数中，用 this 来调用另一个构造函数
在子类中可以访问父类的域和方法。

## 继承
**JAVA只支持单继承，即一个类只能有一个直接父类**
关键词是 extends ，如 class Student extends Person , 若没有 extends 语句，则默认为 java.lang.Object 的子类。
同cpp，子类可以继承、覆盖、重载、添加属性和方法。
*覆盖要求参数类型列表相同，而重载不需要。重载实际上是重写一个函数。*
同cpp，父类指针可指向子类对象，反之不行。除非强转，但是语句能编译不能执行。

## 构造函数
一切同cpp

## super
使用 super 可以访问被子类所隐藏了的同名变量，用法是 age = super.age 。
**即在覆盖父类方法的同时，又利用已定义好的父类** *批判性继承* *酷炫难耐*

## 包
目的：解决名字空间、名字冲突。同名的类一旦在不同的包下，全称就不重名了。
同包里面的类没有严格关系，域继承毫不相干。一个子类及其父类可以位于不同的包中。但是同一个包内的类默认下可以互相访问。
import 语句可以导入存在于某些包内的所需要的类。
同sql，*表示包内当前层的所有类。

## 接口
方法自动是 public 。面向接口编程。
接口、类、数组都是引用类型。
通过接口可以实现不相关类的相同行为，而不用考虑这些类之间的层次关系。
通过接口可以指明多个类需要实现的方法。
完整接口声明如下： 
[public] interface interfaceName [extends listOfSuperInterface]
{

}
其中 public 指任意类都可以访问这个接口，若缺少 public 则只有同个包内的类才能访问。
子接口可以有多个父接口，比起“子类只能有一个父类”的特性来有所不同。
**接口中只提供方法的声明，而不进行方法的实现。因此方法定义没有方法体。**
若子接口有与父接口同名的常量或方法，则父接口中常量被隐藏，方法被重载。
**在接口中定义的常量可以被实现该接口的多个类共享，相当于C中的 #define 。**
例子如下：
```java
// 定义一个接口
interface MyInterface {
  public void method1();
  public void method2();
}

// 实现接口
class MyClass implements MyInterface {
  public void method1() {
    System.out.println("Implementation of method1");
  }
  public void method2() {
    System.out.println("Implementation of method2");
  }
}

// 创建对象并调用方法
class Main {
  public static void main(String[] args) {
    MyClass obj = new MyClass();
    obj.method1();
    obj.method2();
  }
}

```
## 韩顺平接口
实现接口的类相当于U盘、相机；接口本身相当于USB接口；调用接口的类相当于电脑。关系就是：电脑可以调用接口，从而对适配（实现）接口的东西进行操作。
 **接口的作用：工程中统一调用函数，使得能够根据形参自动选择调用的函数，且方便管理。**
示例如下：
```java
//自定义一个接口
public interface Usbinterface
{
  //是抽象方法，相当于接口的尺寸。
  //所有要适配（实现）这个接口的类，都必须实现interfece里面的属性和方法。
  public void start();
  public void stop();
}

//定义一个实现接口的类
public class phone implements Usbinterface
{
  //这个类必须实现接口内属性和方法
  public void start()
  {
    System.out.println("手机开始工作")；
  }
  public void stop() 
  {
    System.out.println("手机停止工作")；
  }
}

//定义一个能调用接口的类
public class computer
{
  //类似多态的调用方法，会根据对象来确定调用谁的实现，完全就是动态绑定
 public void  work(Usbinterface usbin)//形参是：接口类型，传进去一个实现了接口的类对象
 {
  //依据传进的实现类，来判断实际的 start() 方法和 stop() 方法的具体实现
  usbin.start();
  usbin.stop();
 }
}
```
 jdk8之后，在接口中可以有抽象方法的默认实现，但是要使用default。或者是static方法。

**接口不能被实例化。**而且接口中的所有方法都是 public 方法，默认是 public 、abstract 。

一个类可以实现多个接口。
接口中的属性： public static final 修饰
接口不能继承类，但是可以继承其他的接口。**用 extends 而不是 implements**
接口中属性的访问形式： 接口名.属性名

**由于Java中只支持单继承，因此不能做到类似C++的“一个子类来自于多个父类”的操作。因此在Java中，要做到类似多继承的操作，同时维持项目的模块化，就要使用接口。这样由于是通过“类a调用接口b来引用实现接口的类c”，因此在修改实现的时候，不用担心会对其他部分产生影响。**
#### 为什么要写接口，而不在类里面直接实现呢？
*比如你要做一个画板程序，其中里面有一个面板类，主要负责绘画功能，然后你就这样定义了这个类。可是在不久将来，你突然发现现有的类已经不能够满足需要，然后你又要重新设计这个类，更糟糕是你可能要放弃这个类，那么其他地方可能有引用他，这样修改起来很麻烦。如果你一开始定义一个接口，把绘制功能放在接口里，然后定义类a时实现这个接口，然后你只要通过类b调用这个接口去引用实现它的类a就行了，以后要换的话只不过是引用另一个类（类a->类c）而已，这样就达到维护、拓展的方便性。*
*写接口并不是为了扩展，而是为了扩展以后的模块仍然跟项目模块保持高度一致。*
**自己理解：**
①每一种职业就像一个接口，每个单独的人都是一个 Person类的子类，人又可以有多种职业。这就是一个类同时实现多个接口。
②人在拥有一种职业的前提，是必须掌握了这个职业的所有技能。这就是实现类必须要实现接口的所有方法。
③一个人或团体在完成某件事的时候，需要雇佣一些特定职业的人来完成。这个雇佣就是接口的调用。
④团体在雇佣某职业的一些人的时候，会交给他们一个抽象的任务，而这些人要做的就是根据自己的掌握来完成任务。每个人掌握的都不尽相同，因此不同的人在完成这个任务的时候会有不同的表现。这就是类通过接口调用实现类时候的动态绑定。

创建接口类型的数组，可存放的元素是 实现了该接口的类 。而且存在能判断元素具体类型的方法，因此可以做到“遍历接口数组，并调用特定实现类的某个独有函数”。

接口类型的变量可以指向实现该接口的类的实例。

接口是可以继承的，子接口可以指向的实例，父接口也可以。

## 异常
当程序抛出异常 ArithmeticExcepyion 后，程序就终止（崩溃）了，之后的代码就不再执行。然而一个健壮的程序不能一遇到问题就崩溃，因此 Java 提供了**异常处理机制**来解决问题。

如果程序员认为某一段代码可能出现异常/问题，可以使用 try-catch 异常处理机制来解决。从而保证程序的健壮。

#### 操作步骤
选中某一断代码的代码块，使用快捷键 ctrl + alt + t ，选中 try-catch 。


## 泛型
在使用 ArrayList 的时候，ArrayList 中的数据类型是由创建 ArrayList 对象时指定的类型决定的。例如，如果创建一个 ArrayList<String> 对象，那么它只能存储 String 类型的元素。如果没有指定类型，那么默认的数据类型是 Object，也就是说可以存储任何类型的元素。
这样的好处是可以存放各种类型的数据，缺点是在遍历的时候，代码是类似如下的：**（存的时候不会有错，取出并类型转换的时候会运行错误）**
```java
ArrayList list = new ArrayList();
list.add(new Dog("傻狗",10));
list.add(new Cat("傻猫",8));
for(Object a : list)
{
  Dog dd = (Dog) a;
  System.out.println(dd.getName() + "-" + dd.getAge());
}
```
**其中 ArrayList 内元素的数据类型变化是：在放入到 ArrayList 的时候由 Dog 转换为 Object ，在取出的时候还需要转换为 Dog 类型。**
缺点是
①加入存放的是 Cat 类的对象，但是在遍历的时候需要把 Object 类型转换为 Dog 类型，这样编译时检测不出来，但是运行的时候会出现异常。
②在 ArrayList 内元素较多的时候，多次使用类型转换会使运行时间大大增多。

#### **泛型的示例**
```java
ArrayList<Dog> list = new ArrayList<Dog>();//代表集合内只能存放 Dog 类型的对象
list.add(new Dog("傻狗",10));
list.add(new Cat("傻猫",8));
for(Object a : list)
{
  Dog dd = (Dog) a;
  System.out.println(dd.getName() + "-" + dd.getAge());
}
```
**全程元素的数据类型变化是：在放入到 ArrayList 的时候 Dog 还是 Dog ，在取出的时候不需要转换为 Dog 类型。**
①以上代码在```list.add(new Cat("傻猫",8));```的时候就会报错，即从“运行时报错”==>“编译时报错”。
②遍历时不用再进行一步“类型转换”，方便且省时。

#### 泛型的作用
可以在类声明时通过一个标识来表示类中某个属性的类型，或者是某个方法返回值的类型，或者是参数类型。
如：
```java
class Person<T>
{
  T aaa;
  //在定义对象的时候，如果赋予 T 是 String ，那么 aaa 就是 String 类的；如果赋的是 int ，那么 aaa 就是 int 类型的。
  public T function()
  {
    return s;//返回值待定，在定义 Person 对象的时候指定的，是在编译期间确定的。
  }
}
```
保证了：若代码在编译时正确，则在运行时不会出现类型转换上的错误。
**泛型的主要目标是提高 Java 程序的类型安全 ； 泛型的一个附带好处是，消除源代码中的许多强制类型转换。**

#### 泛型的本质
是一种可以表示数据类型的数据类型，可以作为它的对象的是 Integer 、String、自己定义的类 等等。

#### 泛型的使用范围
在接口、类的声明处，都可以使用泛型。它的作用是：指明该 接口/类 内未确定类型的数值，它们的数据类型应该是什么。

#### 注意事项
①在指定泛型的具体类型后，可以传入该类型及其子类类型。示例如下：
```java
class A{};
class B extends A{};
 class P<T>
 {
  T t;
  public P(T t)
  {
    this.t=t;
  }
 }
 P<A> aP = new P<A>(new A());
 P<A> bP = new P<B>(new B());
```
②给泛型指向的数据类型，要求是引用类型，不能是基本数据类型。
③**如果没有指定类型，那么会默认泛型对象是 Object 。**

#### 使用形式
传统形式：
```List<Integer> list1 = new ArrayList<Integer>();```
```ArrayList<Integer> list2 = new ArrayList<Integer>();```
推荐形式：（编译器会自动进行类型推断，它会知道没写的地方应该是什么）
```ArrayList<Integer> list3 = new ArrayList<>();```

## IO流
#### 流的分类
**按照操作数据单位不同**分为：字节流（二进制文件）、字符流（文本文件）
**按照数据流的流向不同**分为：输入流（输入到程序/内存中）、输出流
**按照流的角色不同**分为：节点流、处理流/包装流

**一共有四个*抽象*基类**如下
分类|输入流|        输出流|
| ---- | -------| ------ 
|字节流| InputStream|   OutStream|
|字符流|   Reader   |     Writer |

Java 中的 IO 流一共涉及四十多个类，每个类都是从以上的四个抽象基类派生的。由这四个类派生出来的子类名称，都是以父类名作为子类名后缀。

在流读取完成之后，应该关闭这个流，避免造成资源浪费。

#### InputStream 常用子类
**FileInputStream** ：文件输入流（是个字节输入流，把文件-->程序）
```java
//第 616 个视频
public void readFile01()
{
  String filePath = "路径";
  //读取每一个字节
  int readData = 0 ;
  //创建目前为空的文件输入流对象
  FileInputStream fileInputStream =  null ;
  try
  {
    //创建 FileInputStream 对象，用于读取文件
    //用文件地址来调用文件输入流对象的构造器
    fileInputStream = new FileInputStream (filePath);
    //每次从该输入里读取一个字节的数据，如果返回 -1 ，代表读到最后一位的后面了
    //读的是 ASCII 码
    while((readData = fileInputStream.read()) != -1)
    {
      System.out.print((char)readData);
    }
  }
  catch(IOException e)
  {
    e.printStackTrace();
  }
  finally
  {
    try
    {
      //关闭文件流，释放资源
      fileInputStream.close();
    }
    catch(IOException e)
    {
      e.printStackTrace();
    }
  }
}
```
以下是改进后的，每次读取一个字节数组的代码。
```java
public void readFile01()
{
  String filePath = "路径";
  //一次读取八个字节
  byte[] buf = new byte[8];
  //每次实际读取的字节数
  int readLen = 0 ;
  FileInputStream fileInputStream =  null ;
  try
  {
    //创建 FileInputStream 对象，用于读取文件
    fileInputStream = new FileInputStream (filePath);
    //每次从该输入里读取 buf.length 字节的数据到字节数组中
    //如果读取正常，返回实际读取的字节数 
    //如果返回 -1 ，表示读取完毕
    while((readLen = fileInputStream.read(buf)) != -1)
    {
      System.out.print(new String(buf , 0 , readLen));
    }
  }
  catch(IOException e)
  {
    e.printStackTrace();
  }
  finally
  {
    try
    {
      //关闭文件流，释放资源
      fileInputStream.close();
    }
    catch(IOException e)
    {
      e.printStackTrace();
    }
  }
}
```
注意，这不能用于读取汉字。因为一个汉字是由三个字节来编码的（utf-8），而字节流的 read() 方法每次只读取一个字节，这样就会出现乱码。因此如果是文本文件，最好用字符流来处理。
**BufferedInputStream** ：缓冲字节输入流
**ObjectInputStream** ：对象字节输入流

#### OutputStream 常用子类
**FileOutputStream**:文件输出流。如果文件不存在，那么会自己创建文件（但前提是目录已经存在。）
```java
public void writeFile()
{
  //创建 FileOutputStream 对象
  String filePath = "路径";
  FileOutputStream fileOutputStream = null ;
  try
  {
    //得到一个 FileOutputStream 对象了,
    //此时是覆盖原文件重写，默认是重写
    fileOutputStream = new FileOutputStream(filePath);
    //此时是在原文件最后继续写
    fileOutputStream = new FileOutputStream(filePath , true);
    //写入一个字节
    // char 可以自动转为 int
    fileOutputStream.write('a');

    //写入字符串
    //str.getBytes() 可以把字符串->字节数组
    fileOutputStream.write(str.getBytes());
    fileOutputStream.write(str.getBytes(),0,3);
  }
  catch(IOException e)
  {
    e.printStackTrace();
  }
  finally
  {
    try 
    {
      fileOutputStream.close();
    }
    catch
    {
      e.printStackTrace();
    }
  }
}
```

## 线程
#### 进程
**进程**指的是运行中的程序。譬如当打开并运行一个程序时，操作系统会为该进程分配内存空间；当再打开一个程序时，操作系统会再分配新的内存空间。**进程是程序的一次执行过程or正在运行的一个程序，是动态过程，有自身的产生、存在、消亡过程。**
程序一旦运行起来就是进程，而进程一定会占据内存空间。**程序是写出来的东西，是代码；而进程是动态的东西。**

#### 什么是线程
线程是由进程创建的，是进程的一个实体。一个进程可以有多个线程。
单线程：同一时刻，只允许执行一个线程。
多线程：同一个时刻，可以执行多个线程：例如一个QQ进程可以同时打开多个聊天窗口，可以同时下载多个文件。
**并发：**同一个时刻，多个任务来回切换、交替执行，造成一种“看起来同时执行”的错觉。单核 CPU 实现的多任务就是并发。
**并行：**同一个时刻，多个任务真正地同时执行。多核 CPU 可以实现并行。
也可能并发与并行同时发生。
```java
public class CpuNum
{
  public static void main(String[] args)
  {
    Runtime runtime = Runtime.getRuntime();
    //获取当前电脑的 CPU 数量
    int cpunums = runtime.availablaProcessors();
    System.out.pringln("当前电脑CPU个数为" + cpunums);
  }
}
```

#### 怎么创建线程
如果要创建一个线程，有两个方法。
第一个方法是 用一个类继承 Thread 类。
第二个是 用一个类直接实现 Thread 类的父接口———— Runnable 接口，同样可以达到把一个类做成自定义的线程类。
```java
//第一种
//通过继承 Thread 类来创建线程
public class Thread01
{
  public static void main(String[] args)
  {
    //创建一个 Cat 对象，可以当作线程使用
    Cat mimon = new Cat();
    //线程，启动！
    mimon.start();
  }
}

//当一个类继承了 Thread 类，该类就可以当作线程使用。
class Cat extends Thread
{
  @Override
  public void run()
  {
    //会自动出现以下的方法，这个方法在 Thread 类中是实现 Runnable 接口中 run 方法。
    // super.run();
    //一般要重写 run() 方法，写上自己的实现逻辑
    while(true)
    {
    System.out.println("鲜参战力无双");
    //得到当前线程名
    System.out.println(Thread.currentThread().getName());
    //让该线程休眠十分之一秒
    try
    {
      Thread.sleep(100);
    }
    catch(InterruptedException e)
    {
      e.printStackTrace();
    }
    }
  }
}
```
#### 主线程与子线程
启动一个进程之后，马上会进入主方法（ main 函数），其实就是进程开启了一个线程，叫主线程。
刚刚 mimon.start() 的时候又启动了一个子线程。
当主线程启动一个子线程之后，主线程不会阻塞：代码会继续往下执行，而不会等到子线程执行完毕再往下。
不是说主方法结束了==》进程结束了，因为主线程可能会开启子线程，当主线程消亡的时候子线程仍在运行，此时整个进程不会结束。私以为主线程同子线程其实并无区别，只是主线程是进程直接开的线程而已。而且子线程同样可以再开子线程，同样的情况同样适用。
只有这个进程的所有线程都结束了，这个进程才会消亡。
如果在主线程中，在原本“启动子线程”这一步不是写 mimon.start() ，而是写 mimon.run() ，那么代表是由主线程直接找到 run() 方法并且在主线程中执行，而没有再开启一个子线程。此时主线程会阻塞，在执行完 mimon.run() 之后，才会执行下面的代码。
mimon.start() 方法中，有一个本地（native）方法叫做 start0() ，这个 start0() 方法才是真正实现多线程的方法。这个 start0() 方法是由 JVM 来调用的，不能被程序员直接调用，底层是 c/c++ 。不同的操作系统有不同的算法来对 start0() 方法进行调用。意思就是：在 start() 方法的 start0() 方法里面，用多线程的方式来调用了 run() 方法。
start() 方法调用 start0() 方法之后，该线程并不一定会立马执行，只是将线程变成了可运行状态。具体什么时候执行完全取决于 CPU 的统一调度。

#### 实现 Runnanle 接口来实现多线程
原因： Java 只支持单继承，某些情况下，一个类可能已经继承了某个父类，因此此时要再继承 Thread 类来实现多线程就不现实。因此 Java 提供了另一种方法，就是让这个已经有父类的类直接实现 Thread 类的父接口 Runnable ，来实现一个线程类。
```java
//第二种
//通过实现接口 Runnable 来开发线程
public class Thread02
{
  public static void main(String[] args)
  {
    Dog vae = new Dog();
    //现在再调用 start() 方法不行了，因为接口 Runnable 里面不存在 statr() 方法。
    //vae.start();
    //因此要创建一个 Thread 对象，再把 vae 传进去，之后调用 Thread 对象的 start() 方法。
    //**Thread 有两个构造器，一个是无参的用来继承，一个是有参的传入对象。**
    Thread tttt = new Thread(vae);
    tttt.start();
  }
}

class Dog implements Runnable
{
  int count = 0 ;
  @Override
  public void run()
  {
    while(true)
    {
      System.out.println("鲜参战力无双" + Thread.currentThread().getName());
      //休眠一秒
      try
      {
        Thread.sleep(1000);
      }
      catch(InterruptedExcepyion e)
      {
        e.printStackTrace();
      }
    }
  }
}
```

在以上代码中可以看到， Runnable 接口本身并没有一个方法叫 start() ，但是如果直接用 run() 方法就只是在主线程里面调用 run() 方法而已，没做到再开一个子线程。
这里做法是把 Dog 类的对象 vae 来初始化一个 Thread 类的对象 tttt ，然后调用 tttt 的 start() 方法。
这里采用了一种叫做“静态代理”的设计模式。模拟了一个极简的 Thread 类。
Thread 类有一个构造器可以接受 实现 Runnable 接口的对象，接受到之后创建一个 Thread 对象，然后调用这个新的 Thread 对象的 start() 方法来调用 start0() 方法，从而再调用 Runnable 接口的 run() 方法。
**因为 Runnable 接口中没有 start() 方法，因此在一个调用了 Runnable 接口的类 Thread 内写了一个 start() 方法，这个 start() 方法用来调用 start0() 方法从而开启子线程，同时调用了 Runnable 接口内的 run() 方法。**
只有这个进程的主线程及其创建的所有子线程、子子线程都消亡了，这个进程才算结束了。
使用实现 Runnable 接口的方式更适合“多个线程共享一个资源”的情况，示例如下。并且避免了单继承的局限性。**比较建议使用 Runnable 接口来实现多线程。**
```java
T3 t3 = new T3 ( "hello" ) ;
Thread thread01 = new Thread ( t3 ) ;
Thread thread02 = new Thread ( t3 ) ;
```

#### 多线程小例
对于一个继承自 Thread 的类，在创建了多个该类的对象后，会开启相应数量的子线程，而且所有的子线程共享该类的 static 属性和方法，即其中一个对象改变了类中的 static 属性，那么另一个对象再访问这个属性，也会是被修改后的数值。
对于一个实现了 Runnable 接口的类，创建多个子线程的方式是：创建多个 Thread 类对象来接收同一个该类的对象，此时开启每个线程的 Thread 类都由同一个该类对象初始化。

#### 线程终止
第一种：当线程完成任务后，会自动退出。
第二种：可以通过使用变量来控制 run() 方法退出的方式来停止线程，即通知方式。
通知方式具体实现：之前我说到，再主线程中单独调用某个实现 Runnable接口或继承 Thread 类的类的 run() 方法，那么是主线程直接寻找到这个方法并在主线程中使用，是一种阻塞的方式。但是如果这个方法是一个**修改类内循环条件**的方法，那么主线程会直接调用这个方法，而当这个修改方法在主线程中实现了之后，子线程运行的类对象的循环条件就改变了，此时就会跳出循环，从而终止任务，结束子线程。

#### 线程的常用方法
```java
setName()      //设置线程名字，使其与参数 name 相同。
getName()      //返回该线程的名字。
start()        //使该线程开始执行；Java虚拟机底层调用该线程的 start0() 方法。
run()          //调用线程对象 run() 方法。
setPriority()  //更改线程的优先级。
getPriority()  //获取线程的优先级。
sleep()        //在指定的毫秒数内让当前正在执行的程序休眠。（暂停执行）
interrupt()    //中断线程。不是终止线程。
// sleep()  方法和 interrupt() 方法配套使用，interrupt() 用来中止休眠，在主线程中直接阻塞调用，相当于提前结束子线程的休眠。
```