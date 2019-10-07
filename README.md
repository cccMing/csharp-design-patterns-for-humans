<h3 align="center">
Adaptation of <a href="https://github.com/kamranahmedse/design-patterns-for-humans">Design Patterns for Humans</a>  to C#
<br>适配 <a href="https://github.com/kamranahmedse/design-patterns-for-humans">Design Patterns for Humans</a>  的C#版本
</h3>
<p align="center"><sub>All the explanation for design patterns stays the same, with minor changes.<br>所有设计模式的解释和原文保持一致，仅有细微改动</sub>
</p>

****

<p align="center">
🎉 Ultra-simplified explanation to design patterns! 🎉
<br>
🎉 超简单的设计模式! 🎉
</p>
<p align="center">
A topic that can easily make anyone's mind wobble. Here I try to make them stick in to your mind (and maybe mine) by explaining them in the <i>simplest</i> way possible.
<br>这个话题很容易使人望而生畏。在这里我尝试用 <i>最简单的</i> 方法去阐述，或许能够使你牢记。
</p>
<p align="center">
You can find full length examples for code snippets used in this article <a href="https://github.com/anupavanm/csharp-design-patterns-for-humans-examples">here.</a>
<br>你可以在 <a href="https://github.com/anupavanm/csharp-design-patterns-for-humans-examples">这里</a> 找到所有相关的代码片段。
</p>

****
🚀 Introduction
=================

Design patterns are solutions to recurring problems; **guidelines on how to tackle certain problems**. They are not classes, packages or libraries that you can plug into your application and wait for the magic to happen. These are, rather, guidelines on how to tackle certain problems in certain situations.
设计模式是针对一些常见问题的解决方案；**指导如何去解决某些问题**。它们不是可以直接引入应用程序并等待神奇发生的类库。相反，设计模式可以在特定情况下去解决特定的问题。

> Design patterns are solutions to recurring problems; guidelines on how to tackle certain problems
> 设计模式是针对常见问题的解决方案；解决某些问题的指导方针

Wikipedia describes them as

> In software engineering, a software design pattern is a general reusable solution to a commonly occurring problem within a given context in software design. It is not a finished design that can be transformed directly into source or machine code. It is a description or template for how to solve a problem that can be used in many different situations.
> 在软件工程中，设计模式是对软件设计中普遍存在（反复出现）的各种问题，所提出的解决方案。它不是可以直接转换为源代码或机器代码的已完成的设计。设计模式用来在许多不同情况下如何去解决问题，是一种方案的描述或者是模板。

⚠️ Be Careful
-----------------
- Design patterns are not a silver bullet to all your problems.
  设计模式并不是解决所有问题的灵丹妙药。
- Do not try to force them; bad things are supposed to happen, if done so. Keep in mind that design patterns are solutions **to** problems, not solutions **finding** problems; so don't overthink.
  不要试图强制套用设计模式，这样做的话只会适得其反。请记住，设计模式是去**解决**问题，而不是去**查找**问题；所以不要为了使用而使用。
- If used in a correct place in a correct manner, they can prove to be a savior; or else they can result in a horrible mess of a code.
  如果能以恰当的方式使用设计模式，那设计模式简直就是是救世主；否则它会导致代码一团糟。

> Also note that the code samples below are in C#-7, however this shouldn't stop you because the concepts are same anyways. Plus the **support for other languages is underway**.
  另请注意，下面的代码示例是 C#，即使代码语言不同，这也不会阻碍你学习设计模式，因为设计模式的理念都是相同的。

Types of Design Patterns 设计模式的类型
-----------------

* [Creational](#creational-design-patterns) 创建型
* [Structural](#structural-design-patterns) 结构型
* [Behavioral](#behavioral-design-patterns) 行为型

Creational Design Patterns 创建设计模式
==========================

In plain words
> Creational patterns are focused towards how to instantiate an object or group of related objects.
> 创建型模式专注于如何实例化对象或一组关联的对象。

Wikipedia says
> In software engineering, creational design patterns are design patterns that deal with object creation mechanisms, trying to create objects in a manner suitable to the situation. The basic form of object creation could result in design problems or added complexity to the design. Creational design patterns solve this problem by somehow controlling this object creation.
在软件工程中，创建型模式是处理对象创建机制的设计模式，试图根据实际情况使用合适的方式去创建对象。对象创建的基本方式可能导致设计上问题、或会增加设计的复杂性。创建型模式通过某种方式控制此对象创建来解决此问题。

 * [Simple Factory](#-simple-factory) 简单工厂
 * [Factory Method](#-factory-method) 工厂方法
 * [Abstract Factory](#-abstract-factory) 抽象工厂
 * [Builder](#-builder) 建造者
 * [Prototype](#-prototype) 原型模式
 * [Singleton](#-singleton) 单例模式

🏠 Simple Factory 简单工厂
--------------
Real world example
> Consider, you are building a house and you need doors. It would be a mess if every time you need a door, you put on your carpenter clothes and start making a door in your house. Instead you get it made from a factory.
想象一下，你正在建一座房子，因此你需要很多门。每当你需要一扇门，你可以穿上你的木匠衣服，带上一些木头，胶水，钉子和建造门所需的所有工具，然后开始在你的房间里建造它，但这可能会一团糟。取而代之的是你可以叫工厂造一些门给你。

In plain words
> Simple factory simply generates an instance for client without exposing any instantiation logic to the client
简单工厂只是为客户端生成一个实例，而不会向客户端暴露具体的实例化过程

Wikipedia says
> In object-oriented programming (OOP), a factory is an object for creating other objects – formally a factory is a function or method that returns objects of a varying prototype or class from some method call, which is assumed to be "new".
在面向对象编程（OOP）中，工厂是用于创建其他对象的对象 - 准确来说工厂是一个函数或方法，通过函数的调用它可以返回不同的原型或者是对象，这些对象我们当它是"new"出来的。

**Programmatic Example**

First of all we have a door interface and the implementation
首先，我们有一个Door的接口和它的实现类
```C#
public interface IDoor
{
    int GetHeight();
    int GetWidth();
}

public class WoodenDoor : IDoor
{
    private int Height { get; set; }
    private int Width { get; set; }

    public WoodenDoor(int height, int width)
    {
        this.Height = height;
        this.Width = width;
    }

    public int GetHeight()
    {
        return this.Height;
    }
    public int GetWidth()
    {
        return this.Width;
    }
}
```
Then we have our door factory that makes the door and returns it
然后，我们的DoorFactory可以构建木门这个对象并返回它
```C#
public static class DoorFactory
{
    public static IDoor MakeDoor(int height, int width)
    {
        return new WoodenDoor(height, width);
    }
}
```
And then it can be used as
```C#
var door = DoorFactory.MakeDoor(80, 30);
Console.WriteLine($"Height of Door : {door.GetHeight()}");
Console.WriteLine($"Width of Door : {door.GetWidth()}");
```

**When to Use?**

When creating an object is not just a few assignments and involves some logic, it makes sense to put it in a dedicated factory instead of repeating the same code everywhere.
当创建一个对象不仅仅是一些参数赋值而且涉及到一些逻辑时，将它放在专用工厂中而不是在要用到的时候去写重复的代码逻辑来构建对象，这样做才更好。

🏭 Factory Method 工厂方法
--------------

Real world example
> Consider the case of a hiring manager. It is impossible for one person to interview for each of the positions. Based on the job opening, she has to decide and delegate the interview steps to different people.
考虑这样一种情形-招聘经理。招聘经理不可能对应聘不同职位的求职者亲自面试。基于空缺的职位，她必须决定并将面试流程委托给不同的人。

In plain words
> It provides a way to delegate the instantiation logic to child classes.
它提供了一种将实例化逻辑委托给子类的方法。

Wikipedia says
> In class-based programming, the factory method pattern is a creational pattern that uses factory methods to deal with the problem of creating objects without having to specify the exact class of the object that will be created. This is done by creating objects by calling a factory method—either specified in an interface and implemented by child classes, or implemented in a base class and optionally overridden by derived classes—rather than by calling a constructor.
在面向对象程序设计中，工厂方法是一种创建模式，它使用工厂方法来处理创建对象的问题，而无需指定将要创建的对象的确切类。这是通过调用工厂方法来创建对象来完成的 - 在接口中指定并由子类实现，或者在基类中实现并可选地由派生类重写 - 而不是通过调用构造函数。

 **Programmatic Example**

Taking our hiring manager example above. First of all we have an interviewer interface and some implementations for it
以上面的招聘经理为例。首先，我们要有一个面试官接口和一些实现类

```C#
interface IInterviewer
{
    void AskQuestions();
}

class Developer : IInterviewer
{
    public void AskQuestions()
    {
        Console.WriteLine("Asking about design patterns!");
    }
}

class CommunityExecutive : IInterviewer
{
    public void AskQuestions()
    {
        Console.WriteLine("Asking about community building!");
    }
}
```

Now let us create our `HiringManager`(招聘经理)

```C#
abstract class HiringManager
{
    // Factory method
    abstract protected IInterviewer MakeInterviewer();
    public void TakeInterview()
    {
        var interviewer = this.MakeInterviewer();
        interviewer.AskQuestions();
    }
}

```
Now any child can extend it and provide the required interviewer
现在任何子类都可以继承它并提供相应的面试官对象
```C#
class DevelopmentManager : HiringManager
{
    protected override IInterviewer MakeInterviewer()
    {
        return new Developer();
    }
}

class MarketingManager : HiringManager
{
    protected override IInterviewer MakeInterviewer()
    {
        return new CommunityExecutive();
    }
}

```
and then it can be used as

```C#
var devManager = new DevelopmentManager();
devManager.TakeInterview(); //Output : Asking about design patterns!

var marketingManager = new MarketingManager();
marketingManager.TakeInterview();//Output : Asking about community building!

```

**When to use?**

Useful when there is some generic processing in a class but the required sub-class is dynamically decided at runtime. Or putting it in other words, when the client doesn't know what exact sub-class it might need.
在类中有一些通用处理逻辑（方法），但需要子类动态的决定构造的对象时会很有用。换句话说，客户端并不知道到底需要怎样的子类。

🔨 Abstract Factory 抽象工厂
----------------

Real world example
> Extending our door example from Simple Factory. Based on your needs you might get a wooden door from a wooden door shop, iron door from an iron shop or a PVC door from the relevant shop. Plus you might need a guy with different kind of specialities to fit the door, for example a carpenter for wooden door, welder for iron door etc. As you can see there is a dependency between the doors now, wooden door needs carpenter, iron door needs a welder etc.
继续刚才我们 Simple Factory 门的例子。根据你的需要，你可以从木门商店获取木门，铁门商店获取铁门，或其他相关的店铺购买PVC材质的门。另外，你可能需要一个有不同技能的师傅来装配门，例如木门需要木匠，铁门需要焊接工等。现在你发现门和门之间存在依赖关系，木门需要木匠，铁门需要焊工等

In plain words
> A factory of factories; a factory that groups the individual but related/dependent factories together without specifying their concrete classes.
抽象工厂是工厂的工厂；是一个将独立但是有关联/相互依赖的工厂集合在一起，但并没有制定具体的类。

Wikipedia says
> The abstract factory pattern provides a way to encapsulate a group of individual factories that have a common theme without specifying their concrete classes
抽象工厂模式提供了一种方式，可以将一组具有同一主题但没有指定其具体类的单独工厂封装起来

**Programmatic Example**

Translating the door example above. First of all we have our `Door` interface and some implementation for it
下面实现上文中门的例子。首先，我们有我们的Door接口和一些实现类

```C#
interface IDoor
{
  void GetDescription();
}

class WoodenDoor : IDoor
{
  public void GetDescription()
  {
    Console.WriteLine("I am a wooden door");
  }
}

class IronDoor : IDoor
{
  public void GetDescription()
  {
    Console.WriteLine("I am a iron door");
  }
}
```
Then we have some fitting experts for each door type
然后我们为每种门的材质都配备了装配师傅

```C#
interface IDoorFittingExpert
{
  void GetDescription();
}

class Welder : IDoorFittingExpert
{
  public void GetDescription()
  {
    Console.WriteLine("I can only fit iron doors");
  }
}

class Carpenter : IDoorFittingExpert
{
  public void GetDescription()
  {
    Console.WriteLine("I can only fit wooden doors");
  }
}
```

Now we have our abstract factory that would let us make family of related objects i.e. wooden door factory would create a wooden door and wooden door fitting expert and iron door factory would create an iron door and iron door fitting expert
现在我们有了抽象工厂，可以让我们创建一组联对象，即木门工厂可以制造一个木门和所需的装配师傅，铁门工厂可以制造一个铁门和铁门装配师傅
```C#
interface IDoorFactory {
  IDoor MakeDoor();
  IDoorFittingExpert MakeFittingExpert();
}

// Wooden factory to return carpenter and wooden door
class WoodenDoorFactory : IDoorFactory
{
  public IDoor MakeDoor()
  {
    return new WoodenDoor();
  }

  public IDoorFittingExpert MakeFittingExpert()
  {
    return new Carpenter();
  }
}

// Iron door factory to get iron door and the relevant fitting expert
class IronDoorFactory : IDoorFactory
{
  public IDoor MakeDoor()
  {
    return new IronDoor();
  }

  public IDoorFittingExpert MakeFittingExpert()
  {
    return new Welder();
  }
}
```
And then it can be used as
```C#
var woodenDoorFactory = new WoodenDoorFactory();

var woodenDoor = woodenDoorFactory.MakeDoor();
var woodenDoorFittingExpert = woodenDoorFactory.MakeFittingExpert();

woodenDoor.GetDescription(); //Output : I am a wooden door
woodenDoorFittingExpert.GetDescription();//Output : I can only fit woooden doors

var ironDoorFactory = new IronDoorFactory();

var ironDoor = ironDoorFactory.MakeDoor();
var ironDoorFittingExpert = ironDoorFactory.MakeFittingExpert();

ironDoor.GetDescription();//Output : I am a iron door
ironDoorFittingExpert.GetDescription();//Output : I can only fit iron doors
```

As you can see the wooden door factory has encapsulated the `carpenter` and the `wooden door` also iron door factory has encapsulated the `iron door` and `welder`. And thus it had helped us make sure that for each of the created door, we do not get a wrong fitting expert.
正如你所看到，木门工厂已经封装了`木匠`和`木门`，铁门工厂已封装的`铁门`和`焊接工`。因此，它可以帮助我们确保每个门创建的正确性，我们不会得到错误的装配师傅。

**When to use?**

When there are interrelated dependencies with not-that-simple creation logic involved
当存在相互关联的依赖关系时，而并非仅涉及到简简单单的构建逻辑

👷 Builder 建造模式
--------------------------------------------
Real world example
> Imagine you are at Hardee's and you order a specific deal, lets say, "Big Hardee" and they hand it over to you without *any questions*; this is the example of simple factory. But there are cases when the creation logic might involve more steps. For example you want a customized Subway deal, you have several options in how your burger is made e.g what bread do you want? what types of sauces would you like? What cheese would you want? etc. In such cases builder pattern comes to the rescue.
想象一下，你在汉堡王，点了一个标准餐，我们说：“皇堡”，他们*毫无疑问地*把汉堡给了你，这就是简单工厂的例子。但有些情况下，创建逻辑可能涉及更多步骤。例如，你想要一个定制的七层皇堡，而且你有多种选择如何制作你的汉堡，例如你想要什么样的面包？你想要什么类型的酱汁？你想要什么样的奶酪？在这种情况下，建造模式就发挥作用了。

In plain words
> Allows you to create different flavors of an object while avoiding constructor pollution. Useful when there could be several flavors of an object. Or when there are a lot of steps involved in creation of an object.
允许你创建不同风格的对象，同时避免构造函数污染。当一个东西有几种不同口味，或者是在创建对象时涉及到很多步骤时很有用。

Wikipedia says
> The builder pattern is an object creation software design pattern with the intentions of finding a solution to the telescoping constructor anti-pattern.
建造模式是对象创建时的自定义设计模式，其目的是找到伸缩构造器反模式的解决方案。

Having said that let me add a bit about what telescoping constructor anti-pattern is. At one point or the other we have all seen a constructor like below:
话虽如此，让我补充说一下伸缩构造函数反模式是什么。在某一些地方或这其他地方，我们遇到过如下的构造函数：

```C#
public Burger(int size, bool cheese, bool pepperoni, bool lettuce, bool tomato)
{
}
```

As you can see; the number of constructor parameters can quickly get out of hand and it might become difficult to understand the arrangement of parameters. Plus this parameter list could keep on growing if you would want to add more options in future. This is called telescoping constructor anti-pattern.
如你所见；构造函数参数的数量很快就会失控，并且可能难以理解参数的排列。此外，如果你希望将来添加更多选项，此参数列表可能会继续增长。这被称为伸缩构造器反模式。

**Programmatic Example**

The sane alternative is to use the builder pattern. First of all we have our burger that we want to make
明智的替代方案是使用构建器模式。首先，我们要有我们想制造的汉堡

```C#
class Burger
{
  private int mSize;
  private bool mCheese;
  private bool mPepperoni;
  private bool mLettuce;
  private bool mTomato;

  public Burger(BurgerBuilder builder)
  {
    this.mSize = builder.Size;
    this.mCheese = builder.Cheese;
    this.mPepperoni = builder.Pepperoni;
    this.mLettuce = builder.Lettuce;
    this.mTomato = builder.Tomato;
  }

  public string GetDescription()
  {
    var sb = new StringBuilder();
    sb.Append($"This is {this.mSize} inch Burger. ");
    return sb.ToString();
  }
}
```

And then we have the builder
然后我们有了建造者

```C#
class BurgerBuilder {
  public int Size;
  public bool Cheese;
  public bool Pepperoni;
  public bool Lettuce;
  public bool Tomato;

  public BurgerBuilder(int size)
  {
    this.Size = size;
  }

  public BurgerBuilder AddCheese()
  {
    this.Cheese = true;
    return this;
  }

  public BurgerBuilder AddPepperoni()
  {
    this.Pepperoni = true;
    return this;
  }

  public BurgerBuilder AddLettuce()
  {
    this.Lettuce = true;
    return this;
  }

  public BurgerBuilder AddTomato()
  {
    this.Tomato = true;
    return this;
  }

  public Burger Build()
  {
    return new Burger(this);
  }
}
```
And then it can be used as:

```C#
var burger = new BurgerBuilder(4).AddCheese()
                                .AddPepperoni()
                                .AddLettuce()
                                .AddTomato()
                                .Build();
Console.WriteLine(burger.GetDescription());
```

**When to use?**

When there could be several flavors of an object and to avoid the constructor telescoping. The key difference from the factory pattern is that; factory pattern is to be used when the creation is a one step process while builder pattern is to be used when the creation is a multi step process.
当可能存在几种类型的对象并且要避免构造函数伸缩。与工厂模式的主要区别在于：当创建只需一个步骤时，将使用工厂模式，而当创建是多步骤时，应该使用建造模式。

🐑 Prototype 原型模式
------------
Real world example
> Remember dolly? The sheep that was cloned! Lets not get into the details but the key point here is that it is all about cloning
记得多莉？被克隆的羊！我们不详细介绍它，因为我们的关注点在于对象克隆

In plain words
> Create object based on an existing object through cloning.
通过克隆创建基于现有对象的对象。

Wikipedia says
> The prototype pattern is a creational design pattern in software development. It is used when the type of objects to create is determined by a prototypical instance, which is cloned to produce new objects.
原型模式是软件开发中的创建型设计模式。当要创建的对象类型由原型实例确定时使用它，该实例被克隆以生成新对象。

In short, it allows you to create a copy of an existing object and modify it to your needs, instead of going through the trouble of creating an object from scratch and setting it up.
简而言之，它允许你创建现有对象的副本并根据需要进行修改，而不是从头开始创建对象并进行设置。

**Programmatic Example**

In C#, it can be easily done using `MemberwiseClone()`（注意这是个浅拷贝）

```C#
class Sheep
{
  public string Name { get; set; }

  public string Category { get; set; }

  public Sheep(string name, string category)
  {
    Name = name;
    Category = category;
  }

  public Sheep Clone()
  {
    return MemberwiseClone() as Sheep;
  }
}
```
Then it can be cloned like below
```C#
var original = new Sheep("Jolly", "Mountain Sheep");
Console.WriteLine(original.Name); // Jolly
Console.WriteLine(original.Category); // Mountain Sheep

var cloned = original.Clone();
cloned.Name = "Dolly";
Console.WriteLine(cloned.Name); // Dolly
Console.WriteLine(cloned.Category); // Mountain Sheep
Console.WriteLine(original.Name); // Jolly
```

**When to use?**

When an object is required that is similar to existing object or when the creation would be expensive as compared to cloning.
当需要一个与现有对象类似的对象时，或者创建一个对象与克隆相比成本会很高时。

💍 Singleton 单例模式
------------
Real world example
> There can only be one president of a country at a time. The same president has to be brought to action, whenever duty calls. President here is singleton.
一个国家在同一时刻只能有一个总统。每当有使命行动时，总是一个总统采取行动。这里的总统是单例。

In plain words
> Ensures that only one object of a particular class is ever created.
确保只创建特定类的单个对象。

Wikipedia says
> In software engineering, the singleton pattern is a software design pattern that restricts the instantiation of a class to one object. This is useful when exactly one object is needed to coordinate actions across the system.
在软件工程中，单例模式是一种软件设计模式，它将类的实例化限制为一个对象。当需要一个对象来协调整个系统的操作时，这非常有用。

Singleton pattern is actually considered an anti-pattern and overuse of it should be avoided. It is not necessarily bad and could have some valid use-cases but should be used with caution because it introduces a global state in your application and change to it in one place could affect in the other areas and it could become pretty difficult to debug. The other bad thing about them is it makes your code tightly coupled plus mocking the singleton could be difficult.
单例模式实际上被认为是反模式，应该避免过度使用它。单例不一定是糟糕的，它也有一些有用的地方，但是应该谨慎使用，因为它在你的应用程序中引入了一个全局状态，并且在一个地方改变它的状态可能会影响其他调用的地方，而且代码会变得难以调试。关于它们的另一个坏处是它使你的代码紧密耦合，再加上模拟单例可能很困难。

**Programmatic Example**

To create a singleton, make the constructor private, disable cloning, disable extension and create a static variable to house the instance
要创建单例，请将构造函数设为私有，禁用克隆，禁用扩展并创建静态变量来构建实例
（实际使用要double check，或者静态调用，或者Lazy&lt;Object&gt;）
```C#
public class President
{
  static President instance;
  // Private constructor
  private President()
  {
    //Hiding the Constructor
  }

  // Public constructor
  public static President GetInstance()
  {
    if (instance == null) {
      instance = new President();
    }
    return instance;
  }
}
```
Then in order to use
```C#
President a = President.GetInstance();
President b = President.GetInstance();

Console.WriteLine(a == b); //Output : true
```

Structural Design Patterns 结构型设计模式
==========================
In plain words
> Structural patterns are mostly concerned with object composition or in other words how the entities can use each other. Or yet another explanation would be, they help in answering "How to build a software component?"
结构型模式主要涉及对象组成，或者换句话说，实体之间如何相互使用。或者另一种解释是，它们能够用来回答“如何构建软件的组件？”

Wikipedia says
> In software engineering, structural design patterns are design patterns that ease the design by identifying a simple way to realize relationships between entities.
在软件工程中，结构设计模式是通过识别实现实体之间关系的简单方法来简化设计的设计模式。

 * [Adapter](#-adapter) 适配器模式
 * [Bridge](#-bridge) 桥接模式
 * [Composite](#-composite) 组合模式
 * [Decorator](#-decorator) 装饰模式
 * [Facade](#-facade) 外观模式
 * [Flyweight](#-flyweight) 享元模式
 * [Proxy](#-proxy) 代理模式

🔌 Adapter 适配器模式
-------
Real world example
> Consider that you have some pictures in your memory card and you need to transfer them to your computer. In order to transfer them you need some kind of adapter that is compatible with your computer ports so that you can attach memory card to your computer. In this case card reader is an adapter.
有这样一种情况，你的存储卡中有一些照片，然后需要将它们传输到你的电脑上。为了传输数据，你需要某种与电脑端口兼容的适配器，以便可以将存储卡连接到你的电脑。在这种情况下，读卡器就是个适配器。
> Another example would be the famous power adapter; a three legged plug can't be connected to a two pronged outlet, it needs to use a power adapter that makes it compatible with the two pronged outlet.
另一个例子是常见电源适配器；一个三脚插头不能插到双脚插座上，它需要使用转接头使之与双脚插座兼容。
> Yet another example would be a translator translating words spoken by one person to another
还有一个例子是翻译人员将一个人所说的话翻译成另一个人听得懂的语言

In plain words
> Adapter pattern lets you wrap an otherwise incompatible object in an adapter to make it compatible with another class.
适配器模式允许你在适配器中包装其他不兼容的对象，以使其与另一个类兼容。

Wikipedia says
> In software engineering, the adapter pattern is a software design pattern that allows the interface of an existing class to be used as another interface. It is often used to make existing classes work with others without modifying their source code.
在软件工程中，适配器模式是一种软件设计模式，它允许将现有类的接口用作另一个接口。它通常用于使现有类与其他类一起工作而无需修改其源代码。

**Programmatic Example**

Consider a game where there is a hunter and he hunts lions.
有这样一个游戏，一个猎人猎杀狮子。

First we have an interface `Lion` that all types of lions have to implement
首先，我们有一个Lion接口，所有类型的狮子都必须实现这个接口

```C#
interface ILion
{
  void Roar();
}

class AfricanLion : ILion
{
  public void Roar()
  {

  }
}

class AsiaLion : ILion
{
  public void Roar()
  {

  }
}
```
And hunter expects any implementation of `Lion` interface to hunt.
猎人期望任何Lion接口的实现类都可以进行捕猎操作。
```C#
class Hunter
{
  public void Hunt(ILion lion)
  {

  }
}
```

Now let's say we have to add a `WildDog` in our game so that hunter can hunt that also. But we can't do that directly because dog has a different interface. To make it compatible for our hunter, we will have to create an adapter that is compatible
现在我们再游戏中加入一个WildDog（野狗），以便猎人也可以追捕它。但我们不能直接这样做，因为狗有不同的接口。为了使它与猎人适配，我们将不得不创建一个兼容的适配器

```C#
// This needs to be added to the game
class WildDog
{
  public void bark()
  {
  }
}

// Adapter around wild dog to make it compatible with our game
class WildDogAdapter : ILion
{
  private WildDog mDog;
  public WildDogAdapter(WildDog dog)
  {
    this.mDog = dog;
  }
  public void Roar()
  {
    mDog.bark();
  }
}
```
And now the `WildDog` can be used in our game using `WildDogAdapter`.

```C#
var wildDog = new WildDog();
var wildDogAdapter = new WildDogAdapter(wildDog);

var hunter = new Hunter();
hunter.Hunt(wildDogAdapter);
```

🚡 Bridge 桥接模式
------
Real world example
> Consider you have a website with different pages and you are supposed to allow the user to change the theme. What would you do? Create multiple copies of each of the pages for each of the themes or would you just create separate theme and load them based on the user's preferences? Bridge pattern allows you to do the second i.e.
思考下，你有一个包含不同页面的网站，你支持用户更改主题。你会怎么做？为每个主题创建每个页面的多个副本，或者只是创建单独的主题并根据用户的首选项加载它们？桥模式能帮你完成后者

![With and without the bridge pattern](https://cloud.githubusercontent.com/assets/11269635/23065293/33b7aea0-f515-11e6-983f-98823c9845ee.png)

In Plain Words
> Bridge pattern is about preferring composition over inheritance. Implementation details are pushed from a hierarchy to another object with a separate hierarchy.
桥模式是组合优先于继承。实现细节从层次结构推送到具有单独层次结构的另一个对象。

Wikipedia says
> The bridge pattern is a design pattern used in software engineering that is meant to "decouple an abstraction from its implementation so that the two can vary independently"
桥接模式是软件工程中使用的设计模式，旨在“将抽象与其实现分离开来，以便两者可以独立变化”

**Programmatic Example**

Translating our WebPage example from above. Here we have the `WebPage` hierarchy
实现我们上面的 WebPage 示例。这里我们有WebPage层次结构

```C#
interface IWebPage
{
  string GetContent();
}

class About : IWebPage
{
  protected ITheme theme;

  public About(ITheme theme)
  {
    this.theme = theme;
  }

  public string GetContent()
  {
    return $"About page in {theme.GetColor()}";
  }
}

class Careers : IWebPage
{
  protected ITheme theme;

  public Careers(ITheme theme)
  {
    this.theme = theme;
  }

  public string GetContent()
  {
    return $"Careers page in {theme.GetColor()}";
  }
}
```
And the separate theme hierarchy 然后分离主题关系
```C#
interface ITheme
{
  string GetColor();
}

class DarkTheme : ITheme
{
  public string GetColor()
  {
    return "Dark Black";
  }
}

class LightTheme : ITheme
{
  public string GetColor()
  {
    return "Off White";
  }
}

class AquaTheme : ITheme
{
  public string GetColor()
  {
    return "Light blue";
  }
}
```
And both the hierarchies
```C#
var darkTheme = new DarkTheme();
var lightTheme = new LightTheme();

var about= new About(darkTheme);
var careers = new Careers(lightTheme);

Console.WriteLine(about.GetContent()); //Output: About page in Dark Black
Console.WriteLine(careers.GetContent()); //Output: Careers page in Off White
```

🌿 Composite 组合模式
-----------------

Real world example
> Every organization is composed of employees. Each of the employees has the same features i.e. has a salary, has some responsibilities, may or may not report to someone, may or may not have some subordinates etc.
每个组织都由很多员工组成。每个员工都有相同的特征，例如有工资，还有职责，有些职工需要向领导汇报，有些不需要，有些可能还有下属，有些没有等等。

In plain words
> Composite pattern lets clients treat the individual objects in a uniform manner.
组合模式允许客户端以统一的方式处理单个对象。

Wikipedia says
> In software engineering, the composite pattern is a partitioning design pattern. The composite pattern describes that a group of objects is to be treated in the same way as a single instance of an object. The intent of a composite is to "compose" objects into tree structures to represent part-whole hierarchies. Implementing the composite pattern lets clients treat individual objects and compositions uniformly.
在软件工程中，组合模式是分区设计模式。组合模式描述了一组对象的处理方式与对象的单个实例相同。组合的意图是将对象“组合”成树结构以表示部分整体层次结构。通过实现组合模式，客户可以统一处理单个对象和组合。

**Programmatic Example**

Taking our employees example from above. Here we have different employee types
以上面的员工为例。这里我们有不同的员工类型

```C#
interface IEmployee
{
  float GetSalary();
  string GetRole();
  string GetName();
}


class Developer : IEmployee
{
  private string mName;
  private float mSalary;

  public Developer(string name, float salary)
  {
    this.mName = name;
    this.mSalary = salary;
  }

  public float GetSalary()
  {
    return this.mSalary;
  }

  public string GetRole()
  {
    return "Developer";
  }

  public string GetName()
  {
    return this.mName;
  }
}

class Designer : IEmployee
{
  private string mName;
  private float mSalary;

  public Designer(string name, float salary)
  {
    this.mName = name;
    this.mSalary = salary;
  }

  public float GetSalary()
  {
    return this.mSalary;
  }

  public string GetRole()
  {
    return "Designer";
  }

  public string GetName()
  {
    return this.mName;
  }
}
```

Then we have an organization which consists of several different types of employees
然后我们有一个由几种不同类型的员工组成的组织

```C#
class Organization
{
  protected List<IEmployee> employees;

  public Organization()
  {
    employees = new List<IEmployee>();
  }

  public void AddEmployee(IEmployee employee)
  {
    employees.Add(employee);
  }

  public float GetNetSalaries()
  {
    float netSalary = 0;

    foreach (var e in employees) {
      netSalary += e.GetSalary();
    }
    return netSalary;
  }
}
```

And then it can be used as

```C#
//Arrange Employees, Organization and add employees
var developer = new Developer("John", 5000);
var designer = new Designer("Arya", 5000);

var organization = new Organization();
organization.AddEmployee(developer);
organization.AddEmployee(designer);

Console.WriteLine($"Net Salary of Employees in Organization is {organization.GetNetSalaries():c}");
//Ouptut: Net Salary of Employees in Organization is $10000.00
```

☕ Decorator 装饰模式
-------------

Real world example

> Imagine you run a car service shop offering multiple services. Now how do you calculate the bill to be charged? You pick one service and dynamically keep adding to it the prices for the provided services till you get the final cost. Here each type of service is a decorator.
想象一下，你经营一家提供多种服务的汽车服务店。现在你如何计算收益？你选择一项服务并动态地向其添加所提供服务的价格，直到获得最终成本。这里的每种服务都是装饰者。

In plain words
> Decorator pattern lets you dynamically change the behavior of an object at run time by wrapping them in an object of a decorator class.
Decorator pattern允许你通过再一个装饰器类将对一个对象进行包装，且可以动态地改变运行时的行为。

Wikipedia says
> In object-oriented programming, the decorator pattern is a design pattern that allows behavior to be added to an individual object, either statically or dynamically, without affecting the behavior of other objects from the same class. The decorator pattern is often useful for adhering to the Single Responsibility Principle, as it allows functionality to be divided between classes with unique areas of concern.
在面向对象的编程中，装饰器模式是一种设计模式，它允许将行为静态或动态地添加到单个对象，而不会影响同一类中其他对象的行为。装饰器模式通常用于遵守单一责任原则，因为它允许在具有独特关注区域的类之间划分功能。

**Programmatic Example**

Lets take coffee for example. First of all we have a simple coffee implementing the coffee interface
让我们以咖啡为例。首先，通过咖啡接口，我们有一个简单的咖啡实现

```C#
interface ICoffee
{
  int GetCost();
  string GetDescription();
}

class SimpleCoffee : ICoffee
{
  public int GetCost()
  {
    return 5;
  }

  public string GetDescription()
  {
    return "Simple Coffee";
  }
}
```
We want to make the code extensible to allow options to modify it if required. Lets make some add-ons (decorators)
我们希望使代码可扩展，以允许选项在需要时修改它。让我们做一些附加组件（装饰器）
```C#
class MilkCoffee : ICoffee
{
  private readonly ICoffee mCoffee;

  public MilkCoffee(ICoffee coffee)
  {
    mCoffee = coffee ?? throw new ArgumentNullException("coffee", "coffee should not be null");
  }
  public int GetCost()
  {
    return mCoffee.GetCost() + 1;
  }

  public string GetDescription()
  {
    return String.Concat(mCoffee.GetDescription(), ", milk");
  }
}

class WhipCoffee : ICoffee
{
  private readonly ICoffee mCoffee;

  public WhipCoffee(ICoffee coffee)
  {
    mCoffee = coffee ?? throw new ArgumentNullException("coffee", "coffee should not be null");
  }
  public int GetCost()
  {
    return mCoffee.GetCost() + 1;
  }

  public string GetDescription()
  {
    return String.Concat(mCoffee.GetDescription(), ", whip");
  }
}

class VanillaCoffee : ICoffee
{
  private readonly ICoffee mCoffee;

  public VanillaCoffee(ICoffee coffee)
  {
    mCoffee = coffee ?? throw new ArgumentNullException("coffee", "coffee should not be null");
  }
  public int GetCost()
  {
    return mCoffee.GetCost() + 1;
  }

  public string GetDescription()
  {
    return String.Concat(mCoffee.GetDescription(), ", vanilla");
  }
}

```

Lets make a coffee now

```C#
var myCoffee = new SimpleCoffee();
Console.WriteLine($"{myCoffee.GetCost():c}"); // $ 5.00
Console.WriteLine(myCoffee.GetDescription()); // Simple Coffee

var milkCoffee = new MilkCoffee(myCoffee);
Console.WriteLine($"{milkCoffee.GetCost():c}"); // $ 6.00
Console.WriteLine(milkCoffee.GetDescription()); // Simple Coffee, milk

var whipCoffee = new WhipCoffee(milkCoffee);
Console.WriteLine($"{whipCoffee.GetCost():c}"); // $ 7.00
Console.WriteLine(whipCoffee.GetDescription()); // Simple Coffee, milk, whip

var vanillaCoffee = new VanillaCoffee(whipCoffee);
Console.WriteLine($"{vanillaCoffee.GetCost():c}"); // $ 8.00
Console.WriteLine(vanillaCoffee.GetDescription()); // Simple Coffee, milk, whip, vanilla
```

📦 Facade 外观模式
----------------

Real world example
> How do you turn on the computer? "Hit the power button" you say! That is what you believe because you are using a simple interface that computer provides on the outside, internally it has to do a lot of stuff to make it happen. This simple interface to the complex subsystem is a facade.
你怎么打开电脑？你说：“按下电源按钮”。这是肯定的，因为你正在使用计算机给外部提供的简单接口，在内部它必须做很多事情来实现开机。这个复杂子系统的简单接口是一个facade。

In plain words
> Facade pattern provides a simplified interface to a complex subsystem.
外观模式为复杂的子系统提供了简单的接口。

Wikipedia says
> A facade is an object that provides a simplified interface to a larger body of code, such as a class library.
外观是一个对象，它为庞大的代码提供了简单的接口，例如类库。

**Programmatic Example**

Taking our computer example from above. Here we have the computer class
继续我们上面的计算机示例。这里我们有 computer 类

```C#
class Computer
{
  public void GetElectricShock()
  {
    Console.Write("Ouch!");
  }

  public void MakeSound()
  {
    Console.Write("Beep beep!");
  }

  public void ShowLoadingScreen()
  {
    Console.Write("Loading..");
  }

  public void Bam()
  {
    Console.Write("Ready to be used!");
  }

  public void CloseEverything()
  {
    Console.Write("Bup bup bup buzzzz!");
  }

  public void Sooth()
  {
    Console.Write("Zzzzz");
  }

  public void PullCurrent()
  {
    Console.Write("Haaah!");
  }
}
```
Here we have the facade
```C#
class ComputerFacade
{
  private readonly Computer mComputer;

  public ComputerFacade(Computer computer)
  {
    this.mComputer = computer ?? throw new ArgumentNullException("computer", "computer cannot be null");
  }

  public void TurnOn()
  {
    mComputer.GetElectricShock();
    mComputer.MakeSound();
    mComputer.ShowLoadingScreen();
    mComputer.Bam();
  }

  public void TurnOff()
  {
    mComputer.CloseEverything();
    mComputer.PullCurrent();
    mComputer.Sooth();
  }
}
```
Now to use the facade
```C#
var computer = new ComputerFacade(new Computer());
computer.TurnOn(); // Ouch! Beep beep! Loading.. Ready to be used!
Console.WriteLine();
computer.TurnOff();  // Bup bup buzzz! Haah! Zzzzz
Console.ReadLine();
```

🍃 Flyweight 享元模式
---------

Real world example
> Did you ever have fresh tea from some stall? They often make more than one cup that you demanded and save the rest for any other customer so to save the resources e.g. gas etc. Flyweight pattern is all about that i.e. sharing.
你有没有买过现做茶叶喝？他们经常制作比一杯更多的量，并为别的客户进行预留，以便节省资源，比如节省天然气等。享元模式就是个关于共享的模式。

In plain words
> It is used to minimize memory usage or computational expenses by sharing as much as possible with similar objects.
它用于通过尽可能多地与类似对象共享来降低内存使用或计算开销。

Wikipedia says
> In computer programming, flyweight is a software design pattern. A flyweight is an object that minimizes memory use by sharing as much data as possible with other similar objects; it is a way to use objects in large numbers when a simple repeated representation would use an unacceptable amount of memory.
在计算机编程中，享元模式是一种软件设计模式。flyweight 是一个通过与其他类似对象共享尽可能多的数据来最小化内存使用的对象；当简单的重复表示将使用不可接受的内存量时，它是一种大量使用对象的方法。

**Programmatic example**

Translating our tea example from above. First of all we have tea types and tea maker
翻译我们上面的关于茶的例子。首先，我们有茶的种类和茶制作人

```C#
// Anything that will be cached is flyweight.
// Types of tea here will be flyweights.
class KarakTea
{
}

// Acts as a factory and saves the tea
class TeaMaker
{
  private Dictionary<string,KarakTea> mAvailableTea = new Dictionary<string,KarakTea>();

  public KarakTea Make(string preference)
  {
    if (!mAvailableTea.ContainsKey(preference))
    {
      mAvailableTea[preference] = new KarakTea();
    }

    return mAvailableTea[preference];
  }
}
```

Then we have the `TeaShop` which takes orders and serves them
然后我们有TeaShop来接单并为客户服务

```C#
class TeaShop
{
  private Dictionary<int,KarakTea> mOrders = new Dictionary<int,KarakTea>();
  private readonly TeaMaker mTeaMaker;

  public TeaShop(TeaMaker teaMaker)
  {
    mTeaMaker = teaMaker ?? throw new ArgumentNullException("teaMaker", "teaMaker cannot be null");
  }

  public void TakeOrder(string teaType, int table)
  {
    mOrders[table] = mTeaMaker.Make(teaType);
  }

  public void Serve()
  {
    foreach(var table  in mOrders.Keys){
      Console.WriteLine($"Serving Tea to table # {table}");
    }
  }
}
```
And it can be used as below

```C#
var teaMaker = new TeaMaker();
var teaShop = new TeaShop(teaMaker);

teaShop.TakeOrder("less sugar", 1);
teaShop.TakeOrder("more milk", 2);
teaShop.TakeOrder("without sugar", 5);

teaShop.Serve();
// Serving tea to table# 1
// Serving tea to table# 2
// Serving tea to table# 5
```

🎱 Proxy 代理模式
-------------------
Real world example
> Have you ever used an access card to go through a door? There are multiple options to open that door i.e. it can be opened either using access card or by pressing a button that bypasses the security. The door's main functionality is to open but there is a proxy added on top of it to add some functionality. Let me better explain it using the code example below.
你有没有用过门禁卡进门？这儿有很多种打开们的方式，即可以使用门禁卡或按下绕过安检的按钮打开。门的主要功能是开门，但在它上面添加了一个代理来增加一些功能。让我通过下面的代码示例来更好地解释它。

In plain words
> Using the proxy pattern, a class represents the functionality of another class.
使用代理模式，类代替了另一个类的功能。

Wikipedia says
> A proxy, in its most general form, is a class functioning as an interface to something else. A proxy is a wrapper or agent object that is being called by the client to access the real serving object behind the scenes. Use of the proxy can simply be forwarding to the real object, or can provide additional logic. In the proxy extra functionality can be provided, for example caching when operations on the real object are resource intensive, or checking preconditions before operations on the real object are invoked.
代理以其最一般的形式，是一个充当其他东西的接口的类。代理是一个包装器或代理对象，客户端正在调用它来访问幕后的真实服务对象。使用代理可以简单地转发到真实对象，或者可以提供额外的逻辑。在代理中，可以提供额外的功能，例如当对真实对象的操作是资源密集时的高速缓存，或者在调用对象的操作之前检查先决条件。

**Programmatic Example**

Taking our security door example from above. Firstly we have the door interface and an implementation of door
继续我们上面防盗门的示例。首先我们有门的接口和门的实现

```C#
interface IDoor
{
  void Open();
  void Close();
}

class LabDoor : IDoor
{
  public void Close()
  {
    Console.WriteLine("Closing lab door");
  }

  public void Open()
  {
    Console.WriteLine("Opening lab door");
  }
}
```
Then we have a proxy to secure any doors that we want
然后我们有一个代理来保障门
```C#
class SecuredDoor
{
  private IDoor mDoor;

  public SecuredDoor(IDoor door)
  {
    mDoor = door ?? throw new ArgumentNullException("door", "door can not be null");
  }

  public void Open(string password)
  {
    if (Authenticate(password))
    {
      mDoor.Open();
    }
    else
    {
      Console.WriteLine("Big no! It ain't possible.");
    }
  }

  private bool Authenticate(string password)
  {
    return password == "$ecr@t";
  }

  public void Close()
  {
    mDoor.Close();
  }
}
```
And here is how it can be used
```C#
var door = new SecuredDoor(new LabDoor());
door.Open("invalid"); // Big no! It ain't possible.

door.Open("$ecr@t"); // Opening lab door
door.Close(); // Closing lab door
```
Yet another example would be some sort of data-mapper implementation. For example, I recently made an ODM (Object Data Mapper) for MongoDB using this pattern where I wrote a proxy around mongo classes while utilizing the magic method `__call()`. All the method calls were proxied to the original mongo class and result retrieved was returned as it is but in case of `find` or `findOne` data was mapped to the required class objects and the object was returned instead of `Cursor`.
另一个例子就是一些数据映射器实现。例如，我最近使用这种模式为 MongoDB 制作了一个 ODM（对象数据映射器），我在使用魔术方法的同时围绕 mongo 类编写了一个代理__call()。所有方法调用被代理到原始mongo类和结果检索到的返回，因为它是但在的情况下，find或findOne数据被映射到所需的类对象和对象返回代替Cursor。

Behavioral Design Patterns 行为型设计模式
==========================

In plain words
> It is concerned with assignment of responsibilities between the objects. What makes them different from structural patterns is they don't just specify the structure but also outline the patterns for message passing/communication between them. Or in other words, they assist in answering "How to run a behavior in software component?"
它关注对象之间的职责分配。它们与结构模式的不同之处在于它们不仅指定了结构，还概述了它们之间的消息传递/通信模式。或者换句话说，他们有助于回答“如何在软件组件中运行行为？”

Wikipedia says
> In software engineering, behavioral design patterns are design patterns that identify common communication patterns between objects and realize these patterns. By doing so, these patterns increase flexibility in carrying out this communication.
在软件工程中，行为型模式为设计模式的一种类型，用来识别对象之间的常用交流模式并加以实现。如此，可在进行这些交流活动时增强弹性。

* [Chain of Responsibility](#-chain-of-responsibility) 责任链模式
* [Command](#-command) 命令行模式
* [Iterator](#-iterator) 迭代器模式
* [Mediator](#-mediator) 中介者模式
* [Memento](#-memento) 备忘录模式
* [Observer](#-observer) 观察者模式
* [Visitor](#-visitor) 访问者模式
* [Strategy](#-strategy) 策略模式
* [State](#-state) 状态模式
* [Template Method](#-template-method) 模板方法模式

🔗 Chain of Responsibility 责任链模式
-----------------------

Real world example
> For example, you have three payment methods (`A`, `B` and `C`) setup in your account; each having a different amount in it. `A` has 100 USD, `B` has 300 USD and `C` having 1000 USD and the preference for payments is chosen as `A` then `B` then `C`. You try to purchase something that is worth 210 USD. Using Chain of Responsibility, first of all account `A` will be checked if it can make the purchase, if yes purchase will be made and the chain will be broken. If not, request will move forward to account `B` checking for amount if yes chain will be broken otherwise the request will keep forwarding till it finds the suitable handler. Here `A`, `B` and `C` are links of the chain and the whole phenomenon is Chain of Responsibility.
例如，你的账户有三种付款方式（A，B和C）；每个账户里面都有不同的金额。A有 100 美元，B有 300 美元，C有 1000 美元，然后按照支付偏好去选择A、B、C的顺序进行支付。你试着购买价值 210 美元的东西。使用责任链，首先检查账户A里的钱是否足够支付，如果足够，则进行购买并且责任链不会继续。如果不够，请求将继续对帐户B进行金额检查，如果足够，责任链将终止，否则请求将继续转发，直到找到合适的处理者。在这里A，B和C 是链的链接，整个现象是责任链。

In plain words
> It helps building a chain of objects. Request enters from one end and keeps going from object to object till it finds the suitable handler.
它有助于构建一系列对象。请求从一端进入并继续从一个对象到另一个对象，直到找到合适的处理程序。

Wikipedia says
> In object-oriented design, the chain-of-responsibility pattern is a design pattern consisting of a source of command objects and a series of processing objects. Each processing object contains logic that defines the types of command objects that it can handle; the rest are passed to the next processing object in the chain.
责任链模式在面向对象程式设计里是一种软件设计模式，它包含了一些命令对象和一系列的处理对象。每一个处理对象决定它能处理哪些命令对象，它也知道如何将它不能处理的命令对象传递给该链中的下一个处理对象。该模式还描述了往该处理链的末尾添加新的处理对象的方法。

**Programmatic Example**

Translating our account example above. First of all we have a base account having the logic for chaining the accounts together and some accounts
翻译上面的帐户示例。首先，我们有一个基本帐户类型，它能够用逻辑将账户链接在一起

```C#
abstract class Account
{
  private Account mSuccessor;
  protected decimal mBalance;

  public void SetNext(Account account)
  {
    mSuccessor = account;
  }

  public void Pay(decimal amountTopay)
  {
    if (CanPay(amountTopay))
    {
      Console.WriteLine($"Paid {amountTopay:c} using {this.GetType().Name}.");
    }
    else if (this.mSuccessor != null)
    {
      Console.WriteLine($"Cannot pay using {this.GetType().Name}. Proceeding..");
      mSuccessor.Pay(amountTopay);
    }
    else
    {
      throw new Exception("None of the accounts have enough balance");
    }
  }
  private bool CanPay(decimal amount)
  {
    return mBalance >= amount;
  }
}

class Bank : Account
{
  public Bank(decimal balance)
  {
    this.mBalance = balance;
  }
}

class Paypal : Account
{
  public Paypal(decimal balance)
  {
    this.mBalance = balance;
  }
}

class Bitcoin : Account
{
  public Bitcoin(decimal balance)
  {
    this.mBalance = balance;
  }
}
```

Now let's prepare the chain using the links defined above (i.e. Bank, Paypal, Bitcoin)
现在，我们准备通过上面的关系来链接（即 Bank，Paypal，Bitcoin）

```C#
// Let's prepare a chain like below
//      $bank->$paypal->$bitcoin
//
// First priority bank
//      If bank can't pay then paypal
//      If paypal can't pay then bit coin
var bank = new Bank(100);          // Bank with balance 100
var paypal = new Paypal(200);      // Paypal with balance 200
var bitcoin = new Bitcoin(300);    // Bitcoin with balance 300

bank.SetNext(paypal);
paypal.SetNext(bitcoin);

// Let's try to pay using the first priority i.e. bank
bank.Pay(259);
// Output will be
// ==============
// Cannot pay using bank. Proceeding ..
// Cannot pay using paypal. Proceeding ..:
// Paid 259 using Bitcoin!
```

👮 Command 命令模式
-------

Real world example
> A generic example would be you ordering food at a restaurant. You (i.e. `Client`) ask the waiter (i.e. `Invoker`) to bring some food (i.e. `Command`) and waiter simply forwards the request to Chef (i.e. `Receiver`) who has the knowledge of what and how to cook.
> Another example would be you (i.e. `Client`) switching on (i.e. `Command`) the television (i.e. `Receiver`) using a remote control (`Invoker`).
一个通用的例子是你在餐厅点餐。你（即Client）要求服务员（即Invoker）上菜（即Command），服务员只是将请求转发给主厨（即Receiver），该主厨知道用什么食材以及如何去烹饪。
另一个例子是你（即client）使用遥控器（即Invoker）打开（即Command）电视（即Receiver）。

In plain words
> Allows you to encapsulate actions in objects. The key idea behind this pattern is to provide the means to decouple client from receiver.
允许你将操作封装在对象中。这种模式背后的关键思想是提供将客户端与接收器解耦的方法。

Wikipedia says
> In object-oriented programming, the command pattern is a behavioral design pattern in which an object is used to encapsulate all information needed to perform an action or trigger an event at a later time. This information includes the method name, the object that owns the method and values for the method parameters.
在面向对象的编程中，命令模式是行为设计模式，其中对象用于封装执行动作或稍后触发事件所需的所有信息。此信息包括方法名称，拥有该方法的对象以及方法参数的值。

**Programmatic Example**

First of all we have the receiver that has the implementation of every action that could be performed
首先，我们有接收器，它实现了每个可以执行的操作
```C#
// Receiver
class Bulb
{
  public void TurnOn()
  {
    Console.WriteLine("Bulb has been lit");
  }

  public void TurnOff()
  {
    Console.WriteLine("Darkness!");
  }
}
```
then we have an interface that each of the commands are going to implement and then we have a set of commands
然后我们有一个接口，将每个命令实现，然后我们有一组命令
```C#
interface ICommand
{
  void Execute();
  void Undo();
  void Redo();
}

// Command
class TurnOn : ICommand
{
  private Bulb mBulb;

  public TurnOn(Bulb bulb)
  {
    mBulb = bulb ?? throw new ArgumentNullException("Bulb", "Bulb cannot be null");
  }

  public void Execute()
  {
    mBulb.TurnOn();
  }

  public void Undo()
  {
    mBulb.TurnOff();
  }

  public void Redo()
  {
    Execute();
  }
}

class TurnOff : ICommand
{
  private Bulb mBulb;

  public TurnOff(Bulb bulb)
  {
    mBulb = bulb ?? throw new ArgumentNullException("Bulb", "Bulb cannot be null");
  }

  public void Execute()
  {
    mBulb.TurnOff();
  }

  public void Undo()
  {
    mBulb.TurnOn();
  }

  public void Redo()
  {
    Execute();
  }
}
```
Then we have an `Invoker` with whom the client will interact to process any commands
然后我们有一个Invoker与客户端进行交互以处理任何命令
```C#
// Invoker
class RemoteControl
{
  public void Submit(ICommand command)
  {
    command.Execute();
  }
}
```
Finally let's see how we can use it in our client
```C#
  var bulb = new Bulb();

  var turnOn = new TurnOn(bulb);
  var turnOff = new TurnOff(bulb);

  var remote = new RemoteControl();
  remote.Submit(turnOn); // Bulb has been lit!
  remote.Submit(turnOff); // Darkness!

  Console.ReadLine();
```

Command pattern can also be used to implement a transaction based system. Where you keep maintaining the history of commands as soon as you execute them. If the final command is successfully executed, all good otherwise just iterate through the history and keep executing the `undo` on all the executed commands.
命令模式还可用于实现基于事务的系统。在执行命令时，一直保持命令历史记录的位置。如果成功执行了最后一个命令，那么所有好处都只是遍历历史记录并继续执行undo所有已执行的命令。

➿ Iterator 迭代器模式
--------

Real world example
> An old radio set will be a good example of iterator, where user could start at some channel and then use next or previous buttons to go through the respective channels. Or take an example of MP3 player or a TV set where you could press the next and previous buttons to go through the consecutive channels or in other words they all provide an interface to iterate through the respective channels, songs or radio stations.
老式无线电设备将是迭代器的一个很好的例子，用户可以从某个频道开始，然后使用下一个或上一个按钮来浏览相应的频道。或者以 MP3 播放器或电视机为例，你可以按下下一个和上一个按钮来浏览连续的频道，换句话说，它们都提供了一个界面来迭代各自的频道，歌曲或电台。

In plain words
> It presents a way to access the elements of an object without exposing the underlying presentation.
它提供了一种在不暴露底层表示的情况下访问对象元素方法。

Wikipedia says
> In object-oriented programming, the iterator pattern is a design pattern in which an iterator is used to traverse a container and access the container's elements. The iterator pattern decouples algorithms from containers; in some cases, algorithms are necessarily container-specific and thus cannot be decoupled.
在面向对象的编程中，迭代器模式是一种设计模式，其中迭代器用于遍历容器并访问容器的元素。迭代器模式将算法与容器分离; 在某些情况下，算法必然是特定于容器的，因此不能解耦。

**Programmatic example**

In C# it can be done by implementing IEnumerable<T>. Translating our radio statiIons example from above. First of all we have `RadioStation`
在 C# 中，使用 IEnumerable<T> 很容易实现。从上面翻译我们的广播电台示例。首先，我们有RadioStation

```C#
class RadioStation
{
  private float mFrequency;

  public RadioStation(float frequency)
  {
    mFrequency = frequency;
  }

  public float GetFrequecy()
  {
    return mFrequency;
  }

}

```
Then we have our iterator

```C#
class StationList : IEnumerable<RadioStation>
{
  List<RadioStation> mStations = new List<RadioStation>();

  public RadioStation this[int index]
  {
    get { return mStations[index]; }
    set { mStations.Insert(index, value); }
  }

  public void Add(RadioStation station)
  {
    mStations.Add(station);
  }

  public void Remove(RadioStation station)
  {
    mStations.Remove(station);
  }

  public IEnumerator<RadioStation> GetEnumerator()
  {
    return this.GetEnumerator();
  }

  IEnumerator IEnumerable.GetEnumerator()
  {
    //Use can switch to this internal collection if you do not want to transform
    //return mStations.GetEnumerator();

    //use this if you want to transform the object before rendering
    foreach (var x in mStations)
    {
      yield return x;
    }
  }
}
```
And then it can be used as
```C#
var stations = new StationList();
var station1 = new RadioStation(89);
stations.Add(station1);

var station2 = new RadioStation(101);
stations.Add(station2);

var station3 = new RadioStation(102);
stations.Add(station3);

foreach(var x in stations)
{
  Console.Write(x.GetFrequecy());
}

var q = stations.Where(x => x.GetFrequecy() == 89).FirstOrDefault();
Console.WriteLine(q.GetFrequecy());

Console.ReadLine();
```

👽 Mediator 中介者模式
========

Real world example
> A general example would be when you talk to someone on your mobile phone, there is a network provider sitting between you and them and your conversation goes through it instead of being directly sent. In this case network provider is mediator.
一个典型的例子就是当你用手机与某人交谈时，有一个运营商隐藏在你和他们之间，你的对话通过运营商去发送而不是直接发送。在这种情况下，运营商是中介。

In plain words
> Mediator pattern adds a third party object (called mediator) to control the interaction between two objects (called colleagues). It helps reduce the coupling between the classes communicating with each other. Because now they don't need to have the knowledge of each other's implementation.
中介模式添加第三方对象（称为 mediator）来控制两个对象（称为同事）之间的交互。它有助于减少彼此通信的类之间的耦合，因为现在他们不需要了解彼此的实现。

Wikipedia says
> In software engineering, the mediator pattern defines an object that encapsulates how a set of objects interact. This pattern is considered to be a behavioral pattern due to the way it can alter the program's running behavior.
在软件工程中，中介模式定义了一个对象，该对象封装了一组对象的交互方式。由于它可以改变程序的运行行为，因此这种模式被认为是一种行为模式。

**Programmatic Example**

Here is the simplest example of a chat room (i.e. mediator) with users (i.e. colleagues) sending messages to each other.
这里有一个聊天室（即中介）与用户（即同事）相互发送消息的最简单示例。

First of all, we have the mediator i.e. the chat room
首先，我们有mediator即聊天室

```C#
interface IChatRoomMediator
{
  void ShowMessage(User user, string message);
}

//Mediator
class ChatRoom : IChatRoomMediator
{
  public void ShowMessage(User user, string message)
  {
    Console.WriteLine($"{DateTime.Now.ToString("MMMM dd, H:mm")} [{user.GetName()}]:{message}");
  }
}
```

Then we have our users i.e. colleagues
然后我们有我们的用户，即colleagues
```C#
class User
{
  private string mName;
  private IChatRoomMediator mChatRoom;

  public User(string name, IChatRoomMediator chatroom)
  {
    mChatRoom = chatroom;
    mName = name;
  }

  public string GetName()
  {
    return mName;
  }

  public void Send(string message)
  {
    mChatRoom.ShowMessage(this, message);
  }
}
```
And the usage
```C#
var mediator = new ChatRoom();

var john = new User("John", mediator);
var jane = new User("Jane", mediator);

john.Send("Hi there!");
jane.Send("Hey!");

//April 14, 20:05[John]:Hi there!
//April 14, 20:05[Jane]:Hey!
```

💾 Memento 备忘录模式
-------
Real world example
> Take the example of calculator (i.e. originator), where whenever you perform some calculation the last calculation is saved in memory (i.e. memento) so that you can get back to it and maybe get it restored using some action buttons (i.e. caretaker).
以计算器（即originator）为例，无论何时执行某些计算，最后的计算都会保存在内存中（即memento），以便你可以回退它并使用某些操作按钮（即caretaker）恢复它。

In plain words
> Memento pattern is about capturing and storing the current state of an object in a manner that it can be restored later on in a smooth manner.
备忘录模式是关于之后可以以平滑的方式恢复关于捕获和存储对象的当前状态。

Wikipedia says
> The memento pattern is a software design pattern that provides the ability to restore an object to its previous state (undo via rollback).
备忘录模式是一种软件设计模式，它提供将对象恢复到其先前状态的能力（通过回滚撤消）。

Usually useful when you need to provide some sort of undo functionality.
当你需要提供某种撤消功能时通常很有用。

**Programmatic Example**

Lets take an example of text editor which keeps saving the state from time to time and that you can restore if you want.
让我们举一个文本编辑器的例子，它时不时地保存文本状态，你可以根据需要恢复历史。

First of all we have our memento object that will be able to hold the editor state
首先，我们有 memento 对象，可以保存编辑器状态

```C#
class EditorMemento
{
  private string mContent;

  public EditorMemento(string content)
  {
    mContent = content;
  }

  public string Content
  {
    get
    {
      return mContent;
    }
  }
}
```

Then we have our editor i.e. originator that is going to use memento object
然后我们有我们的编辑器即即将使用 memento 对象的创作者

```C#
class Editor {

  private string mContent = string.Empty;
  private EditorMemento memento;

  public Editor()
  {
    memento = new EditorMemento(string.Empty);
  }

  public void Type(string words)
  {
    mContent = String.Concat(mContent," ", words);
  }

  public string Content
  {
    get
    {
      return mContent;
    }
  }

  public void Save()
  {
    memento = new EditorMemento(mContent);
  }

  public void Restore()
  {
    mContent = memento.Content;
  }
}
```

And then it can be used as

```C#
var editor = new Editor();

//Type some stuff
editor.Type("This is the first sentence.");
editor.Type("This is second.");

// Save the state to restore to : This is the first sentence. This is second.
editor.Save();

//Type some more
editor.Type("This is third.");

//Output the content
Console.WriteLine(editor.Content); // This is the first sentence. This is second. This is third.

//Restoring to last saved state
editor.Restore();

Console.Write(editor.Content); // This is the first sentence. This is second

```

😎 Observer 观察者模式
--------
Real world example
> A good example would be the job seekers where they subscribe to some job posting site and they are notified whenever there is a matching job opportunity.
一个很好的例子是求职者，他们订阅了一些职位发布网站，只要有匹配的工作机会，他们就会得到通知。

In plain words
> Defines a dependency between objects so that whenever an object changes its state, all its dependents are notified.
定义对象之间的依赖关系，以便每当对象更改其状态时，都会通知其所有依赖项。

Wikipedia says
> The observer pattern is a software design pattern in which an object, called the subject, maintains a list of its dependents, called observers, and notifies them automatically of any state changes, usually by calling one of their methods.
观察者模式是一种软件设计模式，其中一个称为主体的对象维护其依赖者列表，称为观察者，并通常通过调用其中一种方法自动通知它们任何状态变化。

**Programmatic example**

Translating our example from above. First of all we have job seekers that need to be notified for a job posting
从上面翻译我们的例子。首先，我们有求职者需要通知职位发布
```C#
class JobPost
{
  public string Title { get; private set; }

  public JobPost(string title)
  {
    Title = title;
  }
}
class JobSeeker : IObserver<JobPost>
{
  public string Name { get; private set; }

  public JobSeeker(string name)
  {
    Name = name;
  }

  //Method is not being called by JobPostings class currently
  public void OnCompleted()
  {
    //No Implementation
  }

  //Method is not being called by JobPostings class currently
  public void OnError(Exception error)
  {
    //No Implementation
  }

  public void OnNext(JobPost value)
  {
    Console.WriteLine($"Hi {Name} ! New job posted: {value.Title}");
  }
}
```
Then we have our job postings to which the job seekers will subscribe
然后我们的职位发布会向那些订阅了的求职者发布招聘信息
```C#
class JobPostings : IObservable<JobPost>
{
  private List<IObserver<JobPost>> mObservers;
  private List<JobPost> mJobPostings;

  public JobPostings()
  {
    mObservers = new List<IObserver<JobPost>>();
    mJobPostings = new List<JobPost>();
  }

  public IDisposable Subscribe(IObserver<JobPost> observer)
  {
    // Check whether observer is already registered. If not, add it
    if (!mObservers.Contains(observer))
    {
      mObservers.Add(observer);
    }
    return new Unsubscriber<JobPost>(mObservers, observer);
  }

  private void Notify(JobPost jobPost)
  {
    foreach(var observer in mObservers)
    {
      observer.OnNext(jobPost);
    }
  }

  public void AddJob(JobPost jobPost)
  {
    mJobPostings.Add(jobPost);
    Notify(jobPost);
  }

}

internal class Unsubscriber<JobPost> : IDisposable
{
  private List<IObserver<JobPost>> mObservers;
  private IObserver<JobPost> mObserver;

  internal Unsubscriber(List<IObserver<JobPost>> observers, IObserver<JobPost> observer)
  {
    this.mObservers = observers;
    this.mObserver = observer;
  }

  public void Dispose()
  {
    if (mObservers.Contains(mObserver))
      mObservers.Remove(mObserver);
  }
}
```
Then it can be used as
```C#
//Create Subscribers
var johnDoe = new JobSeeker("John Doe");
var janeDoe = new JobSeeker("Jane Doe");

//Create publisher and attch subscribers
var jobPostings = new JobPostings();
jobPostings.Subscribe(johnDoe);
jobPostings.Subscribe(janeDoe);

//Add a new job and see if subscribers get notified
jobPostings.AddJob(new JobPost("Software Engineer"));

//Output
// Hi John Doe! New job posted: Software Engineer
// Hi Jane Doe! New job posted: Software Engineer

Console.ReadLine();
```

🏃 Visitor 访问者模式
-------
Real world example
> Consider someone visiting Dubai. They just need a way (i.e. visa) to enter Dubai. After arrival, they can come and visit any place in Dubai on their own without having to ask for permission or to do some leg work in order to visit any place here; just let them know of a place and they can visit it. Visitor pattern lets you do just that, it helps you add places to visit so that they can visit as much as they can without having to do any legwork.
去迪拜旅游的人。他们只需要一种方式（即签证）来进入迪拜。抵达后，他们可以去迪拜的任何地方，而无需征求许可或是为了参观某些地方跑腿办事，以便访问这里的任何地方; 让他们知道哪些地方他们可以去游玩。访客模式可以让您做到这一点，它可以帮助您添加游玩的地方，以便他们可以尽可能多地访问，而无需做任何跑腿工作。

In plain words
> Visitor pattern lets you add further operations to objects without having to modify them.
访客模式允许您向对象添加更多操作，而无需修改它们。

Wikipedia says
> In object-oriented programming and software engineering, the visitor design pattern is a way of separating an algorithm from an object structure on which it operates. A practical result of this separation is the ability to add new operations to existing object structures without modifying those structures. It is one way to follow the open/closed principle.
在面向对象的编程和软件工程中，访问者设计模式是一种将算法与其运行的对象结构分离的方法。这种分离的实际结果是能够在不修改这些结构的情况下向现有对象结构添加新操作。这是遵循开放/封闭原则的一种方式。

**Programmatic example**

Let's take an example of a zoo simulation where we have several different kinds of animals and we have to make them Sound. Let's translate this using visitor pattern
让我们举一个动物园模拟的例子，这里有几种不同的动物，我们必须让它们发出叫声。让我们用访客模式翻译这个

```C#
// Visitee
interface IAnimal
{
  void Accept(IAnimalOperation operation);
}

// Visitor
interface IAnimalOperation
{
  void VisitMonkey(Monkey monkey);
  void VisitLion(Lion lion);
  void VisitDolphin(Dolphin dolphin);
}
```
Then we have our implementations for the animals
然后我们有动物的实现
```C#
class Monkey : IAnimal
{
  public void Shout()
  {
    Console.WriteLine("Oooh o aa aa!");
  }

  public void Accept(IAnimalOperation operation)
  {
      operation.VisitMonkey(this);
  }
}

class Lion : IAnimal
{
  public void Roar()
  {
    Console.WriteLine("Roaar!");
  }

  public void Accept(IAnimalOperation operation)
  {
      operation.VisitLion(this);
  }
}

class Dolphin : IAnimal
{
  public void Speak()
  {
    Console.WriteLine("Tuut tittu tuutt!");
  }

  public void Accept(IAnimalOperation operation)
  {
      operation.VisitDolphin(this);
  }
}
```
Let's implement our visitor 让我们实现我们的访问者
```C#
class Speak : IAnimalOperation
{
  public void VisitDolphin(Dolphin dolphin)
  {
    dolphin.Speak();
  }

  public void VisitLion(Lion lion)
  {
    lion.Roar();
  }

  public void VisitMonkey(Monkey monkey)
  {
    monkey.Shout();
  }
}
```

And then it can be used as
```C#
var monkey = new Monkey();
var lion = new Lion();
var dolphin = new Dolphin();

var speak = new Speak();

monkey.Accept(speak);    // Ooh oo aa aa!
lion.Accept(speak);      // Roaaar!
dolphin.Accept(speak);   // Tuut tutt tuutt!

```
We could have done this simply by having an inheritance hierarchy for the animals but then we would have to modify the animals whenever we would have to add new actions to animals. But now we will not have to change them. For example, let's say we are asked to add the jump behavior to the animals, we can simply add that by creating a new visitor i.e.
我们可以通过为动物建立一个继承层次结构来做到这一点，但是每当我们不得不为动物添加新动作时我们就必须修改动物。但现在我们不必改变它们。例如，假设我们被要求向动物添加跳跃行为，我们可以通过创建新的访问者来添加它，即

```C#
class Jump : IAnimalOperation
{
  public void VisitDolphin(Dolphin dolphin)
  {
    Console.WriteLine("Walked on water a little and disappeared!");
  }

  public void VisitLion(Lion lion)
  {
    Console.WriteLine("Jumped 7 feet! Back on the ground!");
  }

  public void VisitMonkey(Monkey monkey)
  {
    Console.WriteLine("Jumped 20 feet high! on to the tree!");
  }
}
```
And for the usage
```C#
var jump = new Jump();

monkey.Accept(speak);   // Ooh oo aa aa!
monkey.Accept(jump);    // Jumped 20 feet high! on to the tree!

lion.Accept(speak);     // Roaaar!
lion.Accept(jump);      // Jumped 7 feet! Back on the ground!

dolphin.Accept(speak);  // Tuut tutt tuutt!
dolphin.Accept(jump);   // Walked on water a little and disappeared

```

💡 Strategy 策略模式
--------

Real world example
> Consider the example of sorting, we implemented bubble sort but the data started to grow and bubble sort started getting very slow. In order to tackle this we implemented Quick sort. But now although the quick sort algorithm was doing better for large datasets, it was very slow for smaller datasets. In order to handle this we implemented a strategy where for small datasets, bubble sort will be used and for larger, quick sort.
考虑排序的例子，我们实现了冒泡排序，但随着数据量的增长，冒泡排序开始变得非常缓慢。为了解决这个问题，我们实现了快速排序。虽然快速排序算法对大型数据集的排序速度很快，但对于较小的数据集来说速度非常慢。为了解决这个问题，我们实现了一个策略，对于小型数据集，将使用冒泡排序，大规模数据用快速排序。

In plain words
> Strategy pattern allows you to switch the algorithm or strategy based upon the situation.
策略模式允许您根据情况切换算法或策略。

Wikipedia says
> In computer programming, the strategy pattern (also known as the policy pattern) is a behavioural software design pattern that enables an algorithm's behavior to be selected at runtime.
在计算机编程中，策略模式（strategy pattern 也称为 policy pattern）是一种行为软件设计模式，可以在运行时选择算法的行为。

**Programmatic example**

Translating our example from above. First of all we have our strategy interface and different strategy implementations
翻译我们上面的例子。首先，我们有策略接口和不同的策略实现

```C#
interface ISortStrategy
{
  List<int> Sort(List<int> dataset);
}

class BubbleSortStrategy : ISortStrategy
{
  public List<int> Sort(List<int> dataset)
  {
    Console.WriteLine("Sorting using Bubble Sort !");
    return dataset;
  }
}

class QuickSortStrategy : ISortStrategy
{
  public List<int> Sort(List<int> dataset)
  {
    Console.WriteLine("Sorting using Quick Sort !");
    return dataset;
  }
}
```

And then we have our client that is going to use any strategy
然后我们的客户端可以使用任何策略
```C#
class Sorter
{
  private readonly ISortStrategy mSorter;

  public Sorter(ISortStrategy sorter)
  {
    mSorter = sorter;
  }

  public List<int> Sort(List<int> unSortedList)
  {
    return mSorter.Sort(unSortedList);
  }
}
```
And it can be used as
```C#
var unSortedList = new List<int> { 1, 10, 2, 16, 19 };

var sorter = new Sorter(new BubbleSortStrategy());
sorter.Sort(unSortedList); // // Output : Sorting using Bubble Sort !

sorter = new Sorter(new QuickSortStrategy());
sorter.Sort(unSortedList); // // Output : Sorting using Quick Sort !
```

💢 State 状态模式
-----
Real world example
> Imagine you are using some drawing application, you choose the paint brush to draw. Now the brush changes its behavior based on the selected color i.e. if you have chosen red color it will draw in red, if blue then it will be in blue etc.
想象一下，你正在使用一些绘图应用程序，你选择画笔来画画。现在，画笔根据所选颜色改变其行为，即如果你选择了红色，它会涂红色，如果是蓝色则涂蓝色等。

In plain words
> It lets you change the behavior of a class when the state changes.
它允许你在状态更改时更改类的行为。

Wikipedia says
> The state pattern is a behavioral software design pattern that implements a state machine in an object-oriented way. With the state pattern, a state machine is implemented by implementing each individual state as a derived class of the state pattern interface, and implementing state transitions by invoking methods defined by the pattern's superclass.
> The state pattern can be interpreted as a strategy pattern which is able to switch the current strategy through invocations of methods defined in the pattern's interface.
状态模式是一种行为软件设计模式，它以面向对象的方式实现状态机。使用状态模式，通过将每个单独的状态实现为状态模式接口的派生类来实现状态机，并通过调用由模式的超类定义的方法来实现状态转换。状态模式可以解释为一种策略模式，它能够通过调用模式接口中定义的方法来切换当前策略。

**Programmatic example**

Let's take an example of text editor, it lets you change the state of text that is typed i.e. if you have selected bold, it starts writing in bold, if italic then in italics etc.
让我们以文本编辑器为例，它允许您更改键入的文本的状态，即如果您选择了粗体，则开始以粗体显示，如果是斜体，则以斜体显示等。

First of all we have our state interface and some state implementations
首先，我们有状态接口和一些状态实现

```C#
interface IWritingState {
  void Write(string words);
}

class UpperCase : IWritingState
{
  public void Write(string words)
  {
    Console.WriteLine(words.ToUpper());
  }
}

class LowerCase : IWritingState
{
  public void Write(string words)
  {
    Console.WriteLine(words.ToLower());
  }
}

class DefaultText : IWritingState
{
  public void Write(string words)
  {
    Console.WriteLine(words);
  }
}
```
Then we have our editor
```C#
class TextEditor {

  private IWritingState mState;

  public TextEditor()
  {
    mState = new DefaultText();
  }

  public void SetState(IWritingState state)
  {
    mState = state;
  }

  public void Type(string words)
  {
    mState.Write(words);
  }

}
```
And then it can be used as
```C#
var editor = new TextEditor();

editor.Type("First line");

editor.SetState(new UpperCase());

editor.Type("Second Line");
editor.Type("Third Line");

editor.SetState(new LowerCase());

editor.Type("Fourth Line");
editor.Type("Fifthe Line");

// Output:
// First line
// SECOND LINE
// THIRD LINE
// fourth line
// fifth line
```

📒 Template Method 模板方法模式
---------------

Real world example
> Suppose we are getting some house built. The steps for building might look like
假设我们正在建造一些房屋。建房子的步骤可能看起来像
> - Prepare the base of house 准备房子的基地
> - Build the walls 建造墙壁
> - Add roof 添加屋顶
> - Add other floors 添加其他楼层

> The order of these steps could never be changed i.e. you can't build the roof before building the walls etc but each of the steps could be modified for example walls can be made of wood or polyester or stone.
这些步骤的顺序永远不会改变，即在建造墙壁等之前不能建造屋顶，但是每个步骤都可以修改，例如墙壁可以由木头或聚酯或石头制成。

In plain words
> Template method defines the skeleton of how a certain algorithm could be performed, but defers the implementation of those steps to the children classes.
模板方法定义了如何执行某个算法的框架，但是将这些步骤的实现由子类完成。

Wikipedia says
> In software engineering, the template method pattern is a behavioral design pattern that defines the program skeleton of an algorithm in an operation, deferring some steps to subclasses. It lets one redefine certain steps of an algorithm without changing the algorithm's structure.
在软件工程中，模板方法模式是一种行为设计模式，它定义了操作中算法的程序框架，将一些步骤推迟到子类。它允许重新定义算法的某些步骤而不改变算法的结构。

**Programmatic Example**

Imagine we have a build tool that helps us test, lint, build, generate build reports (i.e. code coverage reports, linting report etc) and deploy our app on the test server.
想象一下，我们有一个构建工具，可以帮助我们测试，lint，构建，生成构建报告（即代码覆盖率报告，linting 报告等），然后在测试服务器上部署我们的应用程序。

First of all we have our base class that specifies the skeleton for the build algorithm
首先，我们有基类，它指定构建算法的框架（流程）
```C#
abstract class Builder
{
    // Template method
    public void Build()
    {
      Test();
      Lint();
      Assemble();
      Deploy();
    }

    abstract public void Test();
    abstract public void Lint();
    abstract public void Assemble();
    abstract public void Deploy();
}
```

Then we can have our implementations

```C#
class AndroidBuilder : Builder
{
  public override void Assemble()
  {
    Console.WriteLine("Assembling the android build");
  }

  public override void Deploy()
  {
    Console.WriteLine("Deploying android build to server");
  }

  public override void Lint()
  {
    Console.WriteLine("Linting the android code");
  }

  public override void Test()
  {
    Console.WriteLine("Running android tests");
  }
}


class IosBuilder : Builder
{
  public override void Assemble()
  {
    Console.WriteLine("Assembling the ios build");
  }

  public override void Deploy()
  {
    Console.WriteLine("Deploying ios build to server");
  }

  public override void Lint()
  {
    Console.WriteLine("Linting the ios code");
  }

  public override void Test()
  {
    Console.WriteLine("Running ios tests");
  }
}

```
And then it can be used as

```C#
var androidBuilder = new AndroidBuilder();
androidBuilder.Build();

// Output:
// Running android tests
// Linting the android code
// Assembling the android build
// Deploying android build to server

var iosBuilder = new IosBuilder();
iosBuilder.Build();

// Output:
// Running ios tests
// Linting the ios code
// Assembling the ios build
// Deploying ios build to server
```

## 🚦 Wrap Up Folks

And that about wraps it up. I will continue to improve this, so you might want to watch/star this repository to revisit. Also, I have plans on writing the same about the architectural patterns, stay tuned for it.

## 👬 Contribution

- Report issues
- Open pull request with improvements
- Spread the word
- Contact me on <a href="https://twitter.com/anupavanm">Twitter</a>

## License

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
