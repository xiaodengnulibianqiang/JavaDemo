# 泛型 Lambda表达式

## 目录

-   [介绍](#介绍)
-   [泛型类声明格式](#泛型类声明格式)
-   [泛型对象](#泛型对象)
-   [泛型接口](#泛型接口)
    -   [实现接口中指定具体类型](#实现接口中指定具体类型)
    -   [子类使用模板形参声明泛型类型](#子类使用模板形参声明泛型类型)
-   [类型通配符](#类型通配符)
-   [Lambda表达式](#Lambda表达式)

## 介绍

泛型是程序设计的一种特性 ,它允许强类型程序语言设计代码定义一些可以变的部分,编程中用泛型来代替某个实际的类型,而后通过实际调用传入的类型对泛型进行替换,达到代码复用的目的,有些像函数传参

> **使用泛型,操作的数据类型被指定为一个参数,这种参数在 类 接口 和方法中称为泛型类,泛型接口 泛型方法
> 相对于传统上的传参,泛型还可以使参数具有更多类型上的变化,使代码达到复用**

下面的代码是是没有经过泛型修改的, 现在我们需要一个`Integer`类型数据类,这个时候只能重新创建一个类,把`value`修改为`Integer`,泛型可以解决这个问题

```java
 class box{
    private String value;
    
    public void setValue(String value) {
        this.value = value;
    }
    public String getValue() {
        return value;
    }
}

```

box在定义时使用了`<T>` 类型,表示此类型是从外部调用本类时指定的,这样实例化对象时可以传入除基础

数据以外的任意数据类型,使类具有良好的通用性

```java
 public class box<T>{
    // T为数据的类型  t为参数也就是Value,意味着我们可以自定传入
    private T t;

    public void setValue(T t) {
        this.t = t;
    }
    public T get() {
        return t;
    }
}

```

泛型类就是在类声明时通过一个标识表示类中某个属性的类型或者是某个方法的返回值和参数类型,只有用户数使用该类的时候 该类所属的类型才能明确

## **泛型类声明格式**

```java
[访问权限] class 类名称<泛型类标识1,泛型类标识2,泛型类标识n,>
[访问权限] 泛型类型标识 变量名称;
[访问权限] 泛型类型标识 方法名称;
[访问权限] 返回值类型声明 方法名称(泛型类型标识,变量名称);
```

## 泛型对象

定义好泛型类后就可以创建泛型对象,创建泛型对象的语法格式如下

```java
类名称<参数化类型> 对象名称 = new 类名称<参数化类型> ();
```

下面这种强制转换写法会报错,因为存取3个元素,1个是`String`类型2个是`int`类型,虽然强制转换了

但是`int`类型的 `Integer` 对象无法转换为字符串所以报错,需要泛型来进行修改

```java
public class op {
    public static void main(String[] args) {
        ArrayList list = new ArrayList();//创建ArrayList集合
        list.add("String");
        list.add(2);
        list.add(3);
        for (Object obj:list){ // for循环遍历集合
            String str = (String) obj; //强制转换为String类型
        }
   }
}

```

使用泛型可以很好的结果类型转换的问题 下面是修改过后的代码,下面代码修改后\*\*只能存储`Inteage`类型的值,这样存入的元素类型只能为Inteage而不是​`Object`避免了强制类型转换 ,\*\*但是这样的意义是什么呢,，还是只限定了类型 我以为是那种使用泛型了什么都可以存进去,那还不如ArrayList好用

```java
import java.util.ArrayList;

public class c {
    public static void main(String[] args) {
    // 泛型类传入的值就是限定传入传入的是对象那么像传入 整数或者字符串都将会报错
        ArrayList<Integer> list = new ArrayList<Integer>();//创建ArrayList集合
//        list.add("String");
        list.add(2);
        list.add(3);
        for (Integer str:list){ // for循环遍历集合
            System.out.println(str);
        }
    }
}
```

泛型方法

泛型方法的定义与其在的类是否是泛型类没有任何关系,泛型方法所在的类可以是泛型类,也可以不是泛型类&#x20;

**泛型类格式如下**

```java
[访问权限] <泛型标识> 返回值类型 方法名称 (泛型标识 参数名称) 
```

下面代码是泛型类同泛型方法的使用,将方法的参数类型和返回值类型都定义为泛型,和形参一样,传入的数据是什么类型返回就会是什么,特别的灵活,如果定义为其他类型,那么就只能传入那种类型,也就是限定死了,和上上方的泛型类一样,

` ArrayList<Integer> list = new ArrayList<Integer>`

限定了只能传入`Integer`类型别的`String`类型不可以，泛型类则是把方法和参数都变成形参

**泛型类的如果不是形参的格式那么就是传入的数据类型就是被限定死的,泛型方法而很灵活者才是区别**

` public <T> void show(T t){`
&#x20;       `System.out.println(t); //参数``    }`

```java
class  Dog{
    String eat;
    Integer age;
    public <T> void show(T t){
        System.out.println(t); //参数
    }
}
public class d {
    public static void main(String[] args) {
    // 创建对象
    Dog dog = new Dog();
    // 调用方法 参数的参数是什么类型返回值就是什么类型 者就是泛型方法,和泛型类结合形成复用
        dog.show("hello");
        dog.show(2);
        dog.show(3);
        }
}


----------------------------------------

输出:

hello
2
3

```

## 泛型接口

声明泛型接口和声明泛型类语言类似,也是在接口名称后面加上`<T>`&#x20;

**泛型接口声明格式**

```java
[访问权限] interface [接口名称] <泛型标识>
```

```java
// 理由声明格式定义泛型接口

interface Info<T>{
  public T getVar();
}
```

接口定义完成后,就要定义此泛型接口的子类  有两种方式

-   接口中指定具体类型
-   子类定义上声明泛型类型

### 实现接口中指定具体类型

子类明确泛型类的类型参数变量时,**外界使用子类 需要传递类型变量进来** 在实现类中需要定义类型参数变量

第`7` 实现泛型接口并且是传入了具体的泛型类型String, `Inter<String>` ，这样第 `8` 代码重写`sab()`

**方法直接指明类型即可, 具体的泛型类型是什么,传入的参数类型就是什么，除非是模板形参**

```java
 // 泛型接口
interface Inter<T>{
   
    public abstract void sab(T t); // 抽象类
}

class interImp implements  Inter<String>{
    public void sab(String s){ // 继承实现接口
        System.out.println(s);
    }
}
public class f {
    public static void main(String[] args) {
    Inter<String> inter = new interImp();
    inter.sab("hello");
    }
}


----------------------------------------------------

输出:


hello
```

### 子类使用模板形参声明泛型类型

**子类不确定泛型类的类型变量时间,外界使用也需要传递类型参数进来,实现类中也需要定义参数,这时候就可以用到一开始模板语法,和泛型方法一样**

子类不确定泛型类的类型参数时,定义对象时泛型可以为任意类型

```java
interface Inter<T>{
    // 泛型方法
    public abstract void sab(T t); // 抽象类
}
class interImp<T> implements  Inter<T>{
    public void sab(T t){ // 继承实现接口
        System.out.println(T);
    }
}
public class f {
    public static void main(String[] args) {
    Inter<String> inter = new interImp();
    Inter <Integer> li = new InterImpl<>();
    inter.sab("hello");
    li.sab(12);
    }
}

```

## 类型通配符[^注释1]

Java中数组是可以协变,代表着父类和子类可以保持相同形式的变化 **,比如有A类和B类,A继承了B,那么A和B是可以兼容的**,而集合是不能协变的,集合是不能协变的,**List\<Animal>不是List\<dog>父类,为了解决协变就出现了 类型通配符**

` public static void test(List<?> list)` 定义`test()` 方法使用`List<?>` 表示`test()` 方法的参数可以是存储任意数据类型的List对象

```java
import java.util.ArrayList;
import java.util.List;

public class g {
    public static void main(String[] args) {
       // 集合转载的是Integer
        List<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(3);
        list.add(2);
        test(list); // 将集合传递进去
    }
    public static void test(List<?> list){
        for (int i=0;i<list.size();i++){ //使用集合的个数代替索引length
            System.out.println(list.get(i)); // 取出数据
        }
    }
}


------------------------------------------

输出:

1
3
2

```

但是**使用了通配符 ****`?`**** 接收泛型对象,那么  ****`?`**** 修饰的对象只能接收,不能修改也就是不能设置**

一下将字符串设置给泛型声明的属性, 但是使用的是通配符 `?` 所以无法将内容添加到集合中但可以设置为`null`

```java
class Test{
  public static void main(String[] args){
  List<?> list = new ArrayList<String>()
    list.add("张三")
  }

}
```

## Lambda表达式

它可以取代大部分的匿名类,写出更好的代码,特别是在集合遍历和集中操作中

**表达式由 参数列表、箭头符号——> 函数体组成,其中函数体可以是一个表达式可以是一个语句块,**

**表达式会被执行 返回返回执行结果,语句块中的语句会被依次执行**

```java
import java.util.Arrays;

public class IDEA84 {
    public static void main(String[] args) {
      String[] arr = {"program","crreek","is","java","java"};
        Arrays.sort(arr,(m,n)->Integer.compare(m.length(),n.length() ));
        System.out.println("Lamba语句体只有一条语句,参数类型推断为"+Arrays.toString(arr));
        Arrays.sort(arr,(String m,String n)->{
            if (m.length() >n.length())
                return -1;
             else
                 return 0;

        });
        System.out.println("多条语句是"+Arrays.toString(arr));
    }
}



---------------------------------------


输出:

Lamba语句体只有一条语句,参数类型推断为[is, java, java, crreek, program]
多条语句是[program, crreek, java, java, is]


```

[^注释1]: 使用了通配符 `?` 接收泛型对象,那么  `?` 修饰的对象只能接收,不能修改也就是不能设置
