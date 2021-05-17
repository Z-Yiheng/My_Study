#  JAVA

### 初阶（java面向对象）

- java类及类的成员

  属性、方法、构造器；代码块、内部类

- 面向对象的三大特征

  封装性、继承性、多态性、(抽象性)

- 其他关键字

  this、super、static、final、abstract、interface、package、import

#### 面向过程与面向对象的区别

##### 面向过程

强调的是功能行为，以函数为最小单位，考虑怎么做。

##### 面向对象

强调具备了功能的对象，以类/对象为最小单位，考虑谁来做

#### 面向对象的两个要素

- 类

  类:对一类事物的描述，是抽象的、概念上的定义

- 对象

  ​	对象:是实际存在的该类事物的每个个体，因而也称为实例

##### 属性

一个英雄有姓名，血量，护甲等等状态这些**状态**就叫做一个类的**属性**

```java
public class Hero {
	String name; //姓名   
  	float hp; //血量  
  	float armor; //护甲  
  	int	moveSpeed; //移动速度
}
```

##### 方法

在LOL中，一个英雄可以做很多事情，比如超神，超鬼，坑队友能做什么在类里面就叫做**方法**

```java
public class Hero {
  String name; //姓名
   
    float hp;  //血量
   
    float armor;  //护甲
   
    int moveSpeed;  //移动速度
     //坑队友
    void keng(){
      System.out.println( "坑队友！" );
   }
}
```

 方法参数

```java
public class Hero {
  String name; //姓名
   
    float hp;  //血量
   
    float armor;  //护甲
   
    int moveSpeed;  //移动速度
     //坑队友
    void keng(){
      System.out.println( "坑队友！" );
   }
    
    //获取护甲值
    float getArmor(){
        return armor;
    }
    
    //增加移动速度
    void addSpeed(int speed){
        moveSpeed = moveSpeed + speed;
    }
    
    public static void main(String[] args) {
        Hero garen = new Hero();
        garen.name = "盖伦";
        garen.moveSpeed = 350;
        garen.addSpeed(100);
    }
}
```

###### 方法的重载

- 重载的概念：

在同一 个类中，允许存在一 个以上的同名方法，只要它们的参数个数或者参数

- 重载的特点

与返回值类型无关，只看参数列表，且参数列表必须不同。(参数个 数或参数类型)。调用时，根据方法参数列表的不同来区别。

###### 构造方法

通过一个类创建一个对象，这个过程叫做**实例化**

实例化是通过调用**构造方法**(又叫做**构造器**)实现的

##### 构造器

创建的对象：new + 构造器 

##### JavaBean

JavaBean是一种Java语言写成的可重用组件

所谓javaBean,是指符合如下标准的Java类:

- 类是公共的
- 有一个无参的公共的构造器
- 有属性，且有对应的get、set方法

```java
public class Customer {
    
    private int id;
    private String name;
    
    public Customer(){
        
    }
    
    public void setId(int i){
        id = i;
    }
    public void getId(){
        return id;
    }
    public void setName(String n){
        name = n;
    }
    public void getName(){
        return name;
    }
}
```

------

关键字

##### this

 this可以修饰：属性、方法、构造器

在类的方法中，我们可以使用**"this.属性"**或**"this.方法"**的方式，调用当前对象属性或方法。通常情况下，我们都选择省略"this."。特殊情况下，如果方法的形参和类的属性同名时，我们必须显式的使用"this.变量"的方式，表明此变量是属性，而非形参。

```java
public class Customer {
	private int id;
	private String name;

	public Customer(){
    
	}

	public void setId(int id){
    	this.id = id;
	}
	public void getId(){
    	return id;
	}
	public void setName(String name){
    	this.name = name;
	}
	public void getName(){
    	return name;
	}
}
```
this调用构造器

- 我们在类的构造器中，可以显式的使用"this(形参列表)"方式，调用本类中指定的其他构造器
- 我们在类的构造器中，可以显式的使用"this(形参列表)"方式，调用本类中指定的其他构造器
- 如果一个类中有n个构造器，则最多有n - 1构造器中使用了 "this(形参列表)"
- 规定: "this (形参列表) "必须声明在当前构造器的首行

##### package（包）

1、为了更好的实现项目中类的管理，提供包的概念

2、使用package声明类或接口所属的包，声明在源文件的首行

3、包，属于标识符，遵循标识符的命名原则、规范

4、每"."一次，代表一层文件目录



补充：

- 同一个包下，不能命名同名的接口、类
- 不同的包下，可以命名同名的接口、类

###### MVC设计模式

- 模型层  model  主要处理数据

  数据对象封装  model.bean/domain

  数据库操作类  model.dao

  数据库  model.db

- 控制层  controller  处理业务逻辑

  应用界面相关  controller.activity

  存放fragment  controller.fragment

  显示列表的适配器  controller.adapter

  服务相关的  controller.service

  抽取的基类  controller.base

- 视图层  view  显示数据

  相关工具类  view.utils

  自定义view  view.ui



##### import（导入）

- 在源文件中显式的使用import结构导入指定包下的类、接口
- 声明在包的声明和类的声明之间
- 如果需要导入多个结构，则并列写出即可
- 可以使用"xxx. *"的方式，表示可以导入xxx包下的所有结构
- 如果使用的类或接口是java. lang包下定义的，则可以省略import结构
- 如果使用的类或接口是java. lang包下定义的，则可以省略import结构
- 如果在源文件中，使用了不同包下的同名的类，则必须至少有一个类需要以全类名的方式显示
- import static：导入指定类或接口中的静态结构

##### super

super理解为：父类的

super可以用来调用：属性、方法、构造器

super的使用：

当子类和父类中具有**同名属性（方法）**时，"this."调用子类属性（方法），"super."调用父类属性（方法）

##### instanceof

a  instanceof  A：判断对象a是否时类A的实例

##### static（静态的）

static可以用来修饰：属性、方法、代码块、内部类

###### 使用static修饰属性



#### 继承性

##### 继承

这一次Weapon**继承Item**
虽然Weapon自己没有设计name和price,但是通过继承Item类，也具备了name和price属性

```java
public class Item {
	String name;
	int price;
}

```

```java
public class Weapon{
	String name;
	int price;
	int damage; //攻击力

}

```

```java
public class Weapon extends Item{
	int damage; //攻击力
	
	public static void main(String[] args) {
		Weapon infinityEdge = new Weapon();
		infinityEdge.damage = 65; //damage属性在类Weapon中新设计的
		
		infinityEdge.name = "无尽之刃";//name属性，是从Item中继承来的，就不需要重复设计了
		infinityEdge.price = 3600;
		
	}
	
}
```

格式：

​	class  A  extends  B{}

​	A:子类、派生类、subclass

​	B:父类、超类、基类、superclass

体现:

一旦子类A继承父类B以后，子类A中就获取了父类B中声明的结构:属性、方法

Java中关于继承性的规定：

- 一个类可以被多个子类继承
- Java中类的单继承性: -个类只能有一个父类
- 子父类是相对的概念。
- 子类直接继承的父类，称为:直接父类。间接继承的父类称为:间接父类
- 子类继承父类以后，就获取了直接父类以及所有间接父类中声明的属性和方法

##### 重写（override)

- 重写

  子类继承父类以后，可以对父类中同名同参数的方法，进行覆盖

- 应用

  重写以后，当创建子类对象以后，通过子类对象调用子父类中的同名同参数的方法时，实际执行的时子类重写父类的方法

#### Object类中的方法

##### == 和equals的区别

##### ==（运算符）

- 可以使用在基本数据类型变量和引用数据类型变量中
- 如果比较的是引用数据类型变量:比较两个对象的地址值是否相同.即两个引用是否指向同一一个对象实体
- 如果比较的是引用数据类型变量:比较两个对象的地址值是否相同.即两个引用是否指向同一一个对象实体

##### equals()

equals()是一个方法，不是运算符

- 只能适用于引用数据类型
- 像String、Date、File、包装类等都重写了Object类中的equals( )方法。重写以后，比较的不是两个引用的地址是否相同，而是比较两个对象的"实体内容"是否相同

##### toString()

- 当我们输出一个对象的引用时，实际上就是调用当前对象的toString( )
- 像String、Date、File、包装类等都重写了object类中的toString( )方法。使得在调用对象的toString()时，返回"实体内容"信息
- I自定义类也可以重写toString( )方法， 当调用此方法时，返回对象的"实体内容

#### 单例设计模式

所谓类的单例设计模式，就是采取一定的方法保证在整个的软件系统中，对某个类***只能存在一个对象实例***，并且该类只提供一个取得其对象实例的方法

##### 饿汉式

1. 私有化类的构造器
2. 内部创建类的对象（对象需要声明为静态static）
3. 提供公共的方法，返回类的对象

```java
class Bank{
    
    //私有化类的构造器
    private Bank(){
        
    }
    //内部创建类的对象（对象声明为静态static）
    private static Bank instance = new Bank();
    
    //提供公共的静态的方法，返回类的对象
    public static Bank getInstance(){
        return instance;
    }
}
```



##### 懒汉式

1. 私有化类的构造器
2. 声明当前类对象，没有初始化（对象声明为静态的）
3. 声明public、static的返回当前类对象的方法

```java
class Order{
    
    //私有化类的构造器
    private Order(){
        
    }
    //声明当前类对象，没有初始化（对象声明为静态的）
    private static Order instance = null;
    //声明public、static的返回当前类对象的方法
    public static Order getInstance(){
        if(instance == null){
            instance = new Order();
        }
        return instance;
    }
}
```

- **饿汉式**是立即加载的方式，无论是否会用到这个对象，都会加载,线程安全
- **懒汉式**，是延迟加载的方式，只有使用的时候才会加载，线程不安全（可改）

#### 接口（interface）

**接口就像是一种约定**，我们约定某些英雄是物理系英雄，那么他们就一定能够进行物理攻击



设计一类英雄，能够使用物理攻击，这类英雄在LOL中被叫做AD
类：ADHero
继承了Hero 类，所以继承了name,hp,armor等属性

**实现某个接口，就相当于承诺了某种约定**

所以，实现了AD这个接口，就必须提供AD接口中声明的方法physicAttack() ,实现在语法上使用关键字 implements

```java
public class ADHero extends Hero implements AD{

	@Override
	public void physicAttack() {
		System.out.println("进行物理攻击");
	}

}
```



### 中阶

#### 异常处理

- 异常定义：

  导致程序的正常流程被中断的事件，叫做异常

- 异常处理常见手段： 

  try  catch  finally  throws

##### try catch

1. 将可能抛出FileNotFoundException **文件不存在异常**的代码放在try里
2. 如果文件存在，就会顺序往下执行，并且不执行catch块中的代码
3. 如果文件不存在，try 里的代码会立即终止，程序流程会运行到对应的catch块中
4. e.printStackTrace(); 会打印出方法的调用痕迹，如此例，会打印出异常开始于TestException的第16行，这样就便于定位和分析到底哪里出了异常

```java
public class TestException {

	public static void main(String[] args) {
		
		File f= new File("d:/LOL.exe");
		
		try{
			System.out.println("试图打开 d:/LOL.exe");
			new FileInputStream(f);
			System.out.println("成功打开");
		}
		catch(FileNotFoundException e){
			System.out.println("d:/LOL.exe不存在");
			e.printStackTrace();
		}	
	}
}

```



##### 多异常捕捉办法

###### 方法一

- 有的时候一段代码会抛出多种异常，比如

```java
new FileInputStream(f);
Date d = sdf.parse("2016-06-03");
```

- 这段代码，会抛出 文件不存在异常 FileNotFoundException 和 解析异常ParseException
  解决办法之一是分别进行catch

```java
catch (FileNotFoundException e) {
    System.out.println("d:/LOL.exe不存在");
    e.printStackTrace();
} catch (ParseException e) {
    System.out.println("日期格式解析错误");
    e.printStackTrace();
}
```

###### 方法二

另一个种办法是把多个异常，放在一个catch里统一捕捉

这种方式从 JDK7开始支持，好处是捕捉的代码**更紧凑**，不足之处是，一旦发生异常，**不能确定到底是哪种异常**，需要通过instanceof 进行判断具体的异常类型

```java
public class TestException {

	public static void main(String[] args) {

		File f = new File("d:/LOL.exe");

		try {
			System.out.println("试图打开 d:/LOL.exe");
			new FileInputStream(f);
			System.out.println("成功打开");
			SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
			Date d = sdf.parse("2016-06-03");
		} catch (FileNotFoundException | ParseException e) {
			if (e instanceof FileNotFoundException)
				System.out.println("d:/LOL.exe不存在");
			if (e instanceof ParseException)
				System.out.println("日期格式解析错误");

			e.printStackTrace();
		}
	}
}

```

无论是否出现异常，finally中的代码都会被执行

##### throws

考虑如下情况：
			主方法调用method1
			method1调用method2
			method2中打开文件

method2中需要进行异常处理
				但是method2**不打算处理**，而是把这个异常通过**throws**抛出去。那么method1就会**接到该异常**。 处理办法也是两种，要么是try catch处理掉，要么也是**抛出去**。method1选择本地try catch住 一旦try catch住了，就相当于把这个异常消化掉了，主方法在调用method1的时候，就不需要进行异常处理了。

```java
public class TestException {

	public static void main(String[] args) {
		method1();

	}

	private static void method1() {
		try {
			method2();
		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}

	}

	private static void method2() throws FileNotFoundException {

		File f = new File("d:/LOL.exe");

		System.out.println("试图打开 d:/LOL.exe");
		new FileInputStream(f);
		System.out.println("成功打开");

	}
}

```

#### 异常分类

异常分类： 可查异常，运行时异常和错误3种

其中，运行时异常和错误又叫非可查异常

##### 可查异常

可查异常： CheckedException
可查异常即**必须进行处理的异常**，要么try catch住,要么往外抛，谁调用，谁处理，比如 FileNotFoundException
如果不处理，编译器，就不让你通过

##### 运行时异常

运行时异常RuntimeException指： **不是必须进行try catch的异常**

常见运行时异常

- 除数不能为0异常:ArithmeticException
- 下标越界异常:ArrayIndexOutOfBoundsException
- 空指针异常:NullPointerException

##### 错误

错误Error，指的是**系统级别的异常**，通常是内存用光了

#### 多线程

##### 概念

- 程序

  是为了完成特定任务，用某种语言编写的一组指令的集合。即指一段静态的代码

- 进程

  程序的一次执行过程，或是正在运行的一个程序

- 线程

  进程可进一步细化为线程，是一个程序内部的一条执行路径



多线程即在同一时间，可以做多件事情



创建多线程有3种方式：

- 继承线程类（Thread类）
- 实现Runnable接口
- 匿名类



Thread类的有关方法

- void start()

  启动线程，并执行对象的run()方法

- run()

  线程在被调用时执行的操作

- String getName()

  返回线程的名字

- void setName(String name)

  设置该线程的名称

- static Thread currentThread()

  返回当前线程。在Thread子类中就是this，通常用于主线程和RUnnable实现类
  
  

##### 方法一：继承线程类（Thread类）

###### 方法

1. 创建一个继承于Thread类的子类
2. 重写Thread类的run()-->将此线程执行的操作声明在run()中
3. 创建Thread类的子类的对象
4. 通过此对象调用start()：1、启动当前线程  2、调用当前线程的run()

###### 例子

例：遍历100以内的所有的偶数

```java
//创建一个继承于Thread类的子类
class MyThreads extends Thread {
    //重写Thread类的run()
    @Override
    public void run() {
            for (int i = 0; i < 100; i++) {
                if(i % 2 == 0) {
                    System.out.println(i);
                }
            }
        }
    }
public class ThreadTest {
    public static void main(String[] args) {
        //创建Thread类的子类
        MyThreads t1 = new MyThreads();

        //通过此对象调用start()
        t1.start();

        for (int i = 0; i < 100; i++) {
            if(i % 2 == 0) {
                System.out.println(i + "*******");
            }
    }
}
```

##### 方法二：实现Runnable接口

###### 方法

1. 创建一个实现了Runnable接口的类
2. 实现类去实现Runnable中的抽象方法run()
3. 创建实现类的对象
4. 将此对象作为参数传递到Thread类的构造器中，创建Thread类的对象
5. 通过Thread类的对象调用start()

###### 例子

```java
//创建一个实现了Runnable接口的类
class MThread implements Runnable{

    //实现类去实现Runnable中的抽象方法run()
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if(i % 2 == 0) {
                System.out.println(i);
            }
        }
    }
}

public class ThreadTest2 {
    public static void main(String[] args) {
        //创建实现类的对象
        MThread mThread = new MThread();
        //将此对象作为参数传递到Thread类的构造器中，创建Thread类的对象
        Thread t1 = new Thread(mThread);
        //通过Thread类的对象调用start()
        t1.start();
    }
}
```

##### 比较创建线程的两种方式

- 开发中：优先选择：实现Runnable接口的方式

  原因：1、实现的方式没有类的单继承性的限制

  ​			2、实现的方式更适合来处理多线程有共享数据的情况

- 联系

  相同点：两种方式都需要重写run()。将线程要执行的逻辑声明在run()中

##### 生命周期

<img src="C:\Users\张以恒\AppData\Roaming\Typora\typora-user-images\image-20210327112207599.png" alt="image-20210327112207599" style="zoom:80%;" />

##### 解决线程安全问题

###### 方式一：同步代码块

synchronized(同步监视器){

//需要被同步的代码

}

说明：操作共享数据的代码，即为需要被同步的代码

​			共享数据：多个线程共同操作的变量

​			同步监视器，俗称：锁。任何一个类的对象，都可以充当锁（要求：多个线程必须要共用同一把锁

```java
class window1 implements Runnable{

    private int ticket = 100;
    Object obj = new Object();
    @Override
    public void run() {
        while (true){
            synchronized (obj){
                if(ticket > 0){

                    try {
                        Thread.sleep(100);
                    }catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                        System.out.println(Thread.currentThread().getName() + ":卖票，票号：" + ticket);
                        ticket--;
                }else{
                    break;
                }
            }
        }
    }
}

public class WindowTest2 {

    public static void main(String[] args) {
        window1 w = new window1();

        Thread t1 = new Thread(w);
        Thread t2 = new Thread(w);

        t1.setName("窗口1");
        t2.setName("窗口2");

        t1.start();
        t2.start();
    }
}
```

缺点：速度

###### 方式二：同步方法

```java
class Window2 implements Runnable{
    private int ticket = 100;

    @Override
    public void run() {
        while (true){
            show();
        }
    }

    private synchronized void show(){
        if (ticket > 0) {
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + ":卖票，票号：" + ticket);
            ticket--;
        }
    }
}


public class WindowTest3 {
    public static void main(String[] args) {
        Window2 w = new Window2();

        Thread t1 = new Thread(w);
        Thread t2 = new Thread(w);

        t1.setName("窗口1");
        t2.setName("窗口2");

        t1.start();
        t2.start();
    }

}
```



##### 方法三：lock锁 

-----JDK5.0新增

```java
import java.util.concurrent.locks.ReentrantLock;
```



1. 实例化ReentranLock
2. 调用锁定lock方法
3. 调用解锁unlock方法

```java
class window3 implements Runnable{
    private int ticket = 100;

    private ReentrantLock lock = new ReentrantLock();
    @Override
    public void run() {
        while (true)

            try {
                //调用锁定lock方法
                lock.lock();

                if(ticket > 0){

                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }

                    System.out.println(Thread.currentThread().getName() + ":售票，票号：" + ticket);
                    ticket--;
                } else {
                    break;
                }
            } finally {
                //调用解锁unlock方法
                lock.unlock();
            }

        }
    }


public class LockTest {
    public static void main(String[] args) {

        window3 w = new window3();

        Thread t1 = new Thread(w);
        Thread t2 = new Thread(w);
        Thread t3 = new Thread(w);

        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");

        t1.start();
        t2.start();
        t3.start();
    }
}
```



