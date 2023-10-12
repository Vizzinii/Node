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