 __Design-pattern `C#`__
===

## What are Design Patterns?

> Design patterns are solutions to software design problems you find again and again in real-world application development. Patterns are about reusable designs and interactions of objects. Each pattern is like a blueprint
that you can customize to solve a particular design problem in your code.


A design pattern is a proven design solution to a common problem faced by software developers. Design patterns became popular with the rise of Object Oriented Analysis and Design (OOAD). Design patterns are designed to help developers deliver higher quality, more easily maintained software products in less time and at lower cost.
Design patterns are:

- **encapsulated** - They embody design knowledge regarding collaboration of classes and objects, distribution of
- **responsibility**, and other design issues.
- **object-oriented** - They incorporate OOAD principles—e.g., low coupling, high cohesion.
- **reusable** - They are adaptable, flexible, general solutions to classes of problems with broad applicability. Design patterns simplify the task facing the developer.

## How did design patterns originate?
The history of design patterns is quite short. They originated as an architectural concept proposed by Christopher Alexander in the late nineteen seventies (ca. 1977). Kent Beck and Ward Cunningham experimented with the notion of applying patterns to computer programming in the late nineteen eighties (ca. 1987). Design patterns did not become widely used in computer science until after Design Patterns: Elements of Reusable Object-Oriented Software by the so-called Gang of Four was published in 1994.

## How are design patterns classified?
Design patterns are categorized to facilitate learning and extending them. They are classified in terms of the underlying problems they address, i.e. according to usage.
The three main design pattern categories are:

  [**Creational Patterns**](#creational-patterns)
  ---
  Address the object creation process. Creational design patterns encapsulate knowledge about how, when, and by whom an instance is created.
  - [**Factory Method**](#factory-method) Creates an instance of several derived classes 
  - [**Abstract Factory**](#abstract-factory) Creates an instance of several families of classes
  - [**Builder**](#builder) Separates object construction from its representation
  - [**Prototype**](#prototype) A fully initialized instance to be copied or cloned
  - [**Singleton**](#singleton) A class of which only a single instance can exist
    
  [**Structural patterns**](#structural-patterns)
  ---
  Address the composition of classes and objects. Design patterns vary both in granularity and level of abstraction.
  - [**Adapter**](#adapter) Match interfaces of different classes
  - [**Bridge**](#bridge) Separates an object’s interface from its implementation
  - [**Composite**](#composite) A tree structure of simple and composite objects
  - [**Decorator**](#decorator) Add responsibilities to objects dynamically
  - [**Facade**](#facade) A single class that represents an entire subsystem
  - [**Flyweight**](#flyweight) A fine-grained instance used for efficient sharing
  - [**Proxy**](#proxy) An object representing another object

  [**Behavioral patterns**](#behavioral-patterns)
  ---
  Characterize the manner of class and object interaction and how responsibilities are distributed among them.
  - [**Chain of Responsibility**](#chain-of-responsibility) A way of passing a request between a chain of objects
  - [**Command**](#command) Encapsulate a command request as an object
  - [**Iterator**](#iterator) Sequentially access the elements of a collection
  - [**Mediator**](#mediator) Defines simplified communication between classes
  - [**Memento**](#memento) Capture and restore an object's internal state
  - [**Observer**](#observer) A way of notifying change to a number of classes
  - [**State**](#state) Alter an object's behavior when its state changes
  - [**Strategy**](#strategy) Encapsulates an algorithm inside a class
  - [**Template Method**](#template-method) Method	Defer the exact steps of an algorithm to a subclass
  - [**Visitor**](#visitor) Defines a new operation to a class without change
  - **Interpreter**	A way to include language elements in a program
---

# **Creational patterns**
> Creational design patterns provide various object creation mechanisms, which increase flexibility and reuse of existing code.


---
## **Factory Method**
> Factory Method is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.

**Usage examples:** The Factory Method pattern is widely used in C# code. It’s very useful when you need to provide a high level of flexibility for your code.

**Identification:** Factory methods can be recognized by creation methods that construct objects from concrete classes. While concrete classes are used during the object creation, the return type of the factory methods is usually declared as either an abstract class or an interface.


The Factory Method defines a method, which should be used for creating objects instead of using a direct constructor call (new operator). Subclasses can override this method to change the class of objects that will be created.

```cs
// The Creator class declares the factory method that is supposed to return
// an object of a Product class. The Creator's subclasses usually provide
// the implementation of this method.
abstract class Creator
{
    // Note that the Creator may also provide some default implementation of
    // the factory method.
    public abstract IProduct FactoryMethod();

    // Also note that, despite its name, the Creator's primary
    // responsibility is not creating products. Usually, it contains some
    // core business logic that relies on Product objects, returned by the
    // factory method. Subclasses can indirectly change that business logic
    // by overriding the factory method and returning a different type of
    // product from it.
    public string SomeOperation()
    {
        // Call the factory method to create a Product object.
        var product = FactoryMethod();
        // Now, use the product.
        var result = "Creator: The same creator's code has just worked with "
            + product.Operation();

        return result;
    }
}

// Concrete Creators override the factory method in order to change the
// resulting product's type.
class ConcreteCreator1 : Creator
{
    // Note that the signature of the method still uses the abstract product
    // type, even though the concrete product is actually returned from the
    // method. This way the Creator can stay independent of concrete product
    // classes.
    public override IProduct FactoryMethod()
    {
        return new ConcreteProduct1();
    }
}

class ConcreteCreator2 : Creator
{
    public override IProduct FactoryMethod()
    {
        return new ConcreteProduct2();
    }
}

// The Product interface declares the operations that all concrete products
// must implement.
public interface IProduct
{
    string Operation();
}

// Concrete Products provide various implementations of the Product
// interface.
class ConcreteProduct1 : IProduct
{
    public string Operation()
    {
        return "{Result of ConcreteProduct1}";
    }
}

class ConcreteProduct2 : IProduct
{
    public string Operation()
    {
        return "{Result of ConcreteProduct2}";
    }
}

class Client
{
    public void Main()
    {
        Console.WriteLine("App: Launched with the ConcreteCreator1.");
        ClientCode(new ConcreteCreator1());
        
        Console.WriteLine("");

        Console.WriteLine("App: Launched with the ConcreteCreator2.");
        ClientCode(new ConcreteCreator2());
    }

    // The client code works with an instance of a concrete creator, albeit
    // through its base interface. As long as the client keeps working with
    // the creator via the base interface, you can pass it any creator's
    // subclass.
    public void ClientCode(Creator creator)
    {
        // ...
        Console.WriteLine("Client: I'm not aware of the creator's class," +
            "but it still works.\n" + creator.SomeOperation());
        // ...
    }
}

class Program
{
    static void Main(string[] args)
    {
        new Client().Main();
    }
}
```
---
## **Abstract Factory**
> Abstract Factory is a creational design pattern that lets you produce families of related objects without specifying their concrete classes.

**Usage examples:** The Adapter pattern is pretty common in C# code. It’s very often used in systems based on some legacy code. In such cases, Adapters make legacy code work with modern classes.

**Identification:** Adapter is recognizable by a constructor which takes an instance of a different abstract/interface type. When the adapter receives a call to any of its methods, it translates parameters to the appropriate format and then directs the call to one or several methods of the wrapped object.

The Abstract Factory interface declares a set of methods that return
different abstract products. These products are called a family and are
related by a high-level theme or concept. Products of one family are
usually able to collaborate among themselves. A family of products may
have several variants, but the products of one variant are incompatible
with products of another.

```cs
public interface IAbstractFactory
{
    IAbstractProductA CreateProductA();
    IAbstractProductB CreateProductB();
}

// Concrete Factories produce a family of products that belong to a single
// variant. The factory guarantees that resulting products are compatible.
// Note that signatures of the Concrete Factory's methods return an abstract
// product, while inside the method a concrete product is instantiated.
class ConcreteFactory1 : IAbstractFactory
{
    public IAbstractProductA CreateProductA()
    {
        return new ConcreteProductA1();
    }

    public IAbstractProductB CreateProductB()
    {
        return new ConcreteProductB1();
    }
}

// Each Concrete Factory has a corresponding product variant.
class ConcreteFactory2 : IAbstractFactory
{
    public IAbstractProductA CreateProductA()
    {
        return new ConcreteProductA2();
    }

    public IAbstractProductB CreateProductB()
    {
        return new ConcreteProductB2();
    }
}

// Each distinct product of a product family should have a base interface.
// All variants of the product must implement this interface.
public interface IAbstractProductA
{
    string UsefulFunctionA();
}

// Concrete Products are created by corresponding Concrete Factories.
class ConcreteProductA1 : IAbstractProductA
{
    public string UsefulFunctionA()
    {
        return "The result of the product A1.";
    }
}

class ConcreteProductA2 : IAbstractProductA
{
    public string UsefulFunctionA()
    {
        return "The result of the product A2.";
    }
}

// Here's the the base interface of another product. All products can
// interact with each other, but proper interaction is possible only between
// products of the same concrete variant.
public interface IAbstractProductB
{
    // Product B is able to do its own thing...
    string UsefulFunctionB();

    // ...but it also can collaborate with the ProductA.
    //
    // The Abstract Factory makes sure that all products it creates are of
    // the same variant and thus, compatible.
    string AnotherUsefulFunctionB(IAbstractProductA collaborator);
}

// Concrete Products are created by corresponding Concrete Factories.
class ConcreteProductB1 : IAbstractProductB
{
    public string UsefulFunctionB()
    {
        return "The result of the product B1.";
    }

    // The variant, Product B1, is only able to work correctly with the
    // variant, Product A1. Nevertheless, it accepts any instance of
    // AbstractProductA as an argument.
    public string AnotherUsefulFunctionB(IAbstractProductA collaborator)
    {
        var result = collaborator.UsefulFunctionA();

        return $"The result of the B1 collaborating with the ({result})";
    }
}

class ConcreteProductB2 : IAbstractProductB
{
    public string UsefulFunctionB()
    {
        return "The result of the product B2.";
    }

    // The variant, Product B2, is only able to work correctly with the
    // variant, Product A2. Nevertheless, it accepts any instance of
    // AbstractProductA as an argument.
    public string AnotherUsefulFunctionB(IAbstractProductA collaborator)
    {
        var result = collaborator.UsefulFunctionA();

        return $"The result of the B2 collaborating with the ({result})";
    }
}

// The client code works with factories and products only through abstract
// types: AbstractFactory and AbstractProduct. This lets you pass any
// factory or product subclass to the client code without breaking it.
class Client
{
    public void Main()
    {
        // The client code can work with any concrete factory class.
        Console.WriteLine("Client: Testing client code with the first factory type...");
        ClientMethod(new ConcreteFactory1());
        Console.WriteLine();

        Console.WriteLine("Client: Testing the same client code with the second factory type...");
        ClientMethod(new ConcreteFactory2());
    }

    public void ClientMethod(IAbstractFactory factory)
    {
        var productA = factory.CreateProductA();
        var productB = factory.CreateProductB();

        Console.WriteLine(productB.UsefulFunctionB());
        Console.WriteLine(productB.AnotherUsefulFunctionB(productA));
    }
}

class Program
{
    static void Main(string[] args)
    {
        new Client().Main();
    }
}
```
---
## **Builder**

> Builder is a creational design pattern that lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.

**Usage examples:** The Builder pattern is a well-known pattern in C# world. It’s especially useful when you need to create an object with lots of possible configuration options.

**Identification:** The Builder pattern can be recognized in a class, which has a single creation method and several methods to configure the resulting object. Builder methods often support chaining (for example, someBuilder.setValueA(1).setValueB(2).create()).

The Builder interface specifies methods for creating the different parts of the Product objects.
```cs

public interface IBuilder
{
    void BuildPartA();    
    void BuildPartB();    
    void BuildPartC();
}

// The Concrete Builder classes follow the Builder interface and provide
// specific implementations of the building steps. Your program may have
// several variations of Builders, implemented differently.
public class ConcreteBuilder : IBuilder
{
    private Product _product = new Product();
    
    // A fresh builder instance should contain a blank product object, which is used in further assembly.
    public ConcreteBuilder()
    {
        this.Reset();
    }
    
    public void Reset()
    {
        this._product = new Product();
    }
    
    // All production steps work with the same product instance.
    public void BuildPartA()
    {
        this._product.Add("PartA1");
    }
    
    public void BuildPartB()
    {
        this._product.Add("PartB1");
    }
    
    public void BuildPartC()
    {
        this._product.Add("PartC1");
    }
    
    // Concrete Builders are supposed to provide their own methods for
    // retrieving results. That's because various types of builders may
    // create entirely different products that don't follow the same
    // interface. Therefore, such methods cannot be declared in the base
    // Builder interface (at least in a statically typed programming
    // language).
    //
    // Usually, after returning the end result to the client, a builder
    // instance is expected to be ready to start producing another product.
    // That's why it's a usual practice to call the reset method at the end
    // of the `GetProduct` method body. However, this behavior is not
    // mandatory, and you can make your builders wait for an explicit reset
    // call from the client code before disposing of the previous result.
    public Product GetProduct()
    {
        Product result = this._product;

        this.Reset();

        return result;
    }
}

// It makes sense to use the Builder pattern only when your products are quite complex and require extensive configuration.

// Unlike in other creational patterns, different concrete builders can
// produce unrelated products. In other words, results of various builders
// may not always follow the same interface.
public class Product
{
    private List<object> _parts = new List<object>();
    
    public void Add(string part)
    {
        this._parts.Add(part);
    }
    
    public string ListParts()
    {
        string str = string.Empty;

        for (int i = 0; i < this._parts.Count; i++)
        {
            str += this._parts[i] + ", ";
        }

        str = str.Remove(str.Length - 2); // removing last ",c"

        return "Product parts: " + str + "\n";
    }
}

// The Director is only responsible for executing the building steps in a
// particular sequence. It is helpful when producing products according to a
// specific order or configuration. Strictly speaking, the Director class is
// optional, since the client can control builders directly.
public class Director
{
    private IBuilder _builder;
    
    public IBuilder Builder
    {
        set { _builder = value; } 
    }
    
    // The Director can construct several product variations using the same
    // building steps.
    public void BuildMinimalViableProduct()
    {
        this._builder.BuildPartA();
    }
    
    public void BuildFullFeaturedProduct()
    {
        this._builder.BuildPartA();
        this._builder.BuildPartB();
        this._builder.BuildPartC();
    }
}

class Program
{
    static void Main(string[] args)
    {
        // The client code creates a builder object, passes it to the
        // director and then initiates the construction process. The end
        // result is retrieved from the builder object.
        var director = new Director();
        var builder = new ConcreteBuilder();
        director.Builder = builder;
        
        Console.WriteLine("Standard basic product:");
        director.BuildMinimalViableProduct();
        Console.WriteLine(builder.GetProduct().ListParts());

        Console.WriteLine("Standard full featured product:");
        director.BuildFullFeaturedProduct();
        Console.WriteLine(builder.GetProduct().ListParts());

        // Remember, the Builder pattern can be used without a Director
        // class.
        Console.WriteLine("Custom product:");
        builder.BuildPartA();
        builder.BuildPartC();
        Console.Write(builder.GetProduct().ListParts());
    }
}
```

---

## **Prototype** 
also known as `Clone`

> Prototype is a creational design pattern that lets you copy existing objects without making your code dependent on their classes.

**Usage examples:** The Prototype pattern is available in C# out of the box with a ICloneable interface.

**Identification:** The prototype can be easily recognized by a clone or copy methods, etc.

All prototype classes should have a common interface that makes it possible to copy objects even if their concrete classes are unknown. Prototype objects can produce full copies since objects of the same class can access each other’s private fields.
```cs
public class Person
{
    public int Age;
    public DateTime BirthDate;
    public string Name;
    public IdInfo IdInfo;

    public Person ShallowCopy()
    {
        return (Person) this.MemberwiseClone();
    }

    public Person DeepCopy()
    {
        Person clone = (Person) this.MemberwiseClone();
        clone.IdInfo = new IdInfo(IdInfo.IdNumber);
        clone.Name = String.Copy(Name);
        return clone;
    }
}

public class IdInfo
{
    public int IdNumber;

    public IdInfo(int idNumber)
    {
        this.IdNumber = idNumber;
    }
}

class Program
{
    static void Main(string[] args)
    {
        Person p1 = new Person();
        p1.Age = 42;
        p1.BirthDate = Convert.ToDateTime("1977-01-01");
        p1.Name = "Jack Daniels";
        p1.IdInfo = new IdInfo(666);

        // Perform a shallow copy of p1 and assign it to p2.
        Person p2 = p1.ShallowCopy();
        // Make a deep copy of p1 and assign it to p3.
        Person p3 = p1.DeepCopy();

        // Display values of p1, p2 and p3.
        Console.WriteLine("Original values of p1, p2, p3:");
        Console.WriteLine("   p1 instance values: ");
        DisplayValues(p1);
        Console.WriteLine("   p2 instance values:");
        DisplayValues(p2);
        Console.WriteLine("   p3 instance values:");
        DisplayValues(p3);

        // Change the value of p1 properties and display the values of p1,
        // p2 and p3.
        p1.Age = 32;
        p1.BirthDate = Convert.ToDateTime("1900-01-01");
        p1.Name = "Frank";
        p1.IdInfo.IdNumber = 7878;
        Console.WriteLine("\nValues of p1, p2 and p3 after changes to p1:");
        Console.WriteLine("   p1 instance values: ");
        DisplayValues(p1);
        Console.WriteLine("   p2 instance values (reference values have changed):");
        DisplayValues(p2);
        Console.WriteLine("   p3 instance values (everything was kept the same):");
        DisplayValues(p3);
    }

    public static void DisplayValues(Person p)
    {
        Console.WriteLine("      Name: {0:s}, Age: {1:d}, BirthDate: {2:MM/dd/yy}",
            p.Name, p.Age, p.BirthDate);
        Console.WriteLine("      ID#: {0:d}", p.IdInfo.IdNumber);
    }
}
```
---
## **Singleton**

> Singleton is a creational design pattern that lets you ensure that a class has only one instance, while providing a global access point to this instance.

Singleton has almost the same pros and cons as global variables. Although they’re super-handy, they break the modularity of your code.

**Usage examples:** A lot of developers consider the Singleton pattern an antipattern. That’s why its usage is on the decline in C# code.

**Identification:** Singleton can be recognized by a static creation method, which returns the same cached object.

This Singleton implementation is called "double check lock". It is safe
in multithreaded environment and provides lazy initialization for the
Singleton object.
```cs
class Singleton
{
    private Singleton() { }

    private static Singleton _instance;

    // We now have a lock object that will be used to synchronize threads
    // during first access to the Singleton.
    private static readonly object _lock = new object();

    public static Singleton GetInstance(string value)
    {
        // This conditional is needed to prevent threads stumbling over the
        // lock once the instance is ready.
        if (_instance == null)
        {
            // Now, imagine that the program has just been launched. Since
            // there's no Singleton instance yet, multiple threads can
            // simultaneously pass the previous conditional and reach this
            // point almost at the same time. The first of them will acquire
            // lock and will proceed further, while the rest will wait here.
            lock (_lock)
            {
                // The first thread to acquire the lock, reaches this
                // conditional, goes inside and creates the Singleton
                // instance. Once it leaves the lock block, a thread that
                // might have been waiting for the lock release may then
                // enter this section. But since the Singleton field is
                // already initialized, the thread won't create a new
                // object.
                if (_instance == null)
                {
                    _instance = new Singleton();
                    _instance.Value = value;
                }
            }
        }
        return _instance;
    }

    // We'll use this property to prove that our Singleton really works.
    public string Value { get; set; }
}

class Program
{
    static void Main(string[] args)
    {
        // The client code.
        
        Console.WriteLine(
            "{0}\n{1}\n\n{2}\n",
            "If you see the same value, then singleton was reused (yay!)",
            "If you see different values, then 2 singletons were created (booo!!)",
            "RESULT:"
        );
        
        Thread process1 = new Thread(() =>
        {
            TestSingleton("FOO");
        });
        Thread process2 = new Thread(() =>
        {
            TestSingleton("BAR");
        });
        
        process1.Start();
        process2.Start();
        
        process1.Join();
        process2.Join();
    }
    
    public static void TestSingleton(string value)
    {
        Singleton singleton = Singleton.GetInstance(value);
        Console.WriteLine(singleton.Value);
    } 
}
```

# **Structural patterns**
**Structural patterns explain how to assemble objects and classes into larger structures, while keeping these structures flexible and efficient.**

---
## **Adapter** 
Adapter is a structural design pattern, which allows incompatible objects to collaborate.

The Adapter acts as a wrapper between two objects. It catches calls for one object and transforms them to format and interface recognizable by the second object.

**Usage examples:** The Adapter pattern is pretty common in C# code. It’s very often used in systems based on some legacy code. In such cases, Adapters make legacy code work with modern classes.

**Identification:** Adapter is recognizable by a constructor which takes an instance of a different abstract/interface type. When the adapter receives a call to any of its methods, it translates parameters to the appropriate format and then directs the call to one or several methods of the wrapped object.

```cs
// The Target defines the domain-specific interface used by the client code.
public interface ITarget
{
    string GetRequest();
}

// The Adaptee contains some useful behavior, but its interface is
// incompatible with the existing client code. The Adaptee needs some
// adaptation before the client code can use it.
class Adaptee
{
    public string GetSpecificRequest()
    {
        return "Specific request.";
    }
}

// The Adapter makes the Adaptee's interface compatible with the Target's
// interface.
class Adapter : ITarget
{
    private readonly Adaptee _adaptee;

    public Adapter(Adaptee adaptee)
    {
        this._adaptee = adaptee;
    }

    public string GetRequest()
    {
        return $"This is '{this._adaptee.GetSpecificRequest()}'";
    }
}

class Program
{
    static void Main(string[] args)
    {
        Adaptee adaptee = new Adaptee();
        ITarget target = new Adapter(adaptee);

        Console.WriteLine("Adaptee interface is incompatible with the client.");
        Console.WriteLine("But with adapter client can call it's method.");

        Console.WriteLine(target.GetRequest());
    }
}
```

## **Bridge**
Bridge is a structural design pattern that divides business logic or huge class into separate class hierarchies that can be developed independently.

One of these hierarchies (often called the Abstraction) will get a reference to an object of the second hierarchy (Implementation). The abstraction will be able to delegate some (sometimes, most) of its calls to the implementations object. Since all implementations will have a common interface, they’d be interchangeable inside the abstraction.

**Usage examples:** The Bridge pattern is especially useful when dealing with cross-platform apps, supporting multiple types of database servers or working with several API providers of a certain kind (for example, cloud platforms, social networks, etc.)

**Identification:** Bridge can be recognized by a clear distinction between some controlling entity and several different platforms that it relies on.

```cs
// The Abstraction defines the interface for the "control" part of the two
// class hierarchies. It maintains a reference to an object of the
// Implementation hierarchy and delegates all of the real work to this
// object.
class Abstraction
{
    protected IImplementation _implementation;
    
    public Abstraction(IImplementation implementation)
    {
        this._implementation = implementation;
    }
    
    public virtual string Operation()
    {
        return "Abstract: Base operation with:\n" + 
            _implementation.OperationImplementation();
    }
}

// You can extend the Abstraction without changing the Implementation
// classes.
class ExtendedAbstraction : Abstraction
{
    public ExtendedAbstraction(IImplementation implementation) : base(implementation)
    {
    }
    
    public override string Operation()
    {
        return "ExtendedAbstraction: Extended operation with:\n" +
            base._implementation.OperationImplementation();
    }
}

// The Implementation defines the interface for all implementation classes.
// It doesn't have to match the Abstraction's interface. In fact, the two
// interfaces can be entirely different. Typically the Implementation
// interface provides only primitive operations, while the Abstraction
// defines higher- level operations based on those primitives.
public interface IImplementation
{
    string OperationImplementation();
}

// Each Concrete Implementation corresponds to a specific platform and
// implements the Implementation interface using that platform's API.
class ConcreteImplementationA : IImplementation
{
    public string OperationImplementation()
    {
        return "ConcreteImplementationA: The result in platform A.\n";
    }
}

class ConcreteImplementationB : IImplementation
{
    public string OperationImplementation()
    {
        return "ConcreteImplementationA: The result in platform B.\n";
    }
}

class Client
{
    // Except for the initialization phase, where an Abstraction object gets
    // linked with a specific Implementation object, the client code should
    // only depend on the Abstraction class. This way the client code can
    // support any abstraction-implementation combination.
    public void ClientCode(Abstraction abstraction)
    {
        Console.Write(abstraction.Operation());
    }
}

class Program
{
    static void Main(string[] args)
    {
        Client client = new Client();

        Abstraction abstraction;
        // The client code should be able to work with any pre-configured
        // abstraction-implementation combination.
        abstraction = new Abstraction(new ConcreteImplementationA());
        client.ClientCode(abstraction);
        
        Console.WriteLine();
        
        abstraction = new ExtendedAbstraction(new ConcreteImplementationB());
        client.ClientCode(abstraction);
    }
}
```

---

## **Composite**
Composite is a structural design pattern that allows composing objects into a tree-like structure and work with the it as if it was a singular object.

Composite became a pretty popular solution for the most problems that require building a tree structure. Composite’s great feature is the ability to run methods recursively over the whole tree structure and sum up the results.

**Usage examples:** The Composite pattern is pretty common in C# code. It’s often used to represent hierarchies of user interface components or the code that works with graphs.

**Identification:** If you have an object tree, and each object of a tree is a part of the same class hierarchy, this is most likely a composite. If methods of these classes delegate the work to child objects of the tree and do it via the base class/interface of the hierarchy, this is definitely a composite.

```cs
// The base Component class declares common operations for both simple and
// complex objects of a composition.
abstract class Component
{
    public Component() { }

    // The base Component may implement some default behavior or leave it to
    // concrete classes (by declaring the method containing the behavior as
    // "abstract").
    public abstract string Operation();

    // In some cases, it would be beneficial to define the child-management
    // operations right in the base Component class. This way, you won't
    // need to expose any concrete component classes to the client code,
    // even during the object tree assembly. The downside is that these
    // methods will be empty for the leaf-level components.
    public virtual void Add(Component component)
    {
        throw new NotImplementedException();
    }

    public virtual void Remove(Component component)
    {
        throw new NotImplementedException();
    }

    // You can provide a method that lets the client code figure out whether
    // a component can bear children.
    public virtual bool IsComposite()
    {
        return true;
    }
}

// The Leaf class represents the end objects of a composition. A leaf can't
// have any children.
//
// Usually, it's the Leaf objects that do the actual work, whereas Composite
// objects only delegate to their sub-components.
class Leaf : Component
{
    public override string Operation()
    {
        return "Leaf";
    }

    public override bool IsComposite()
    {
        return false;
    }
}

// The Composite class represents the complex components that may have
// children. Usually, the Composite objects delegate the actual work to
// their children and then "sum-up" the result.
class Composite : Component
{
    protected List<Component> _children = new List<Component>();
    
    public override void Add(Component component)
    {
        this._children.Add(component);
    }

    public override void Remove(Component component)
    {
        this._children.Remove(component);
    }

    // The Composite executes its primary logic in a particular way. It
    // traverses recursively through all its children, collecting and
    // summing their results. Since the composite's children pass these
    // calls to their children and so forth, the whole object tree is
    // traversed as a result.
    public override string Operation()
    {
        int i = 0;
        string result = "Branch(";

        foreach (Component component in this._children)
        {
            result += component.Operation();
            if (i != this._children.Count - 1)
            {
                result += "+";
            }
            i++;
        }
        
        return result + ")";
    }
}

class Client
{
    // The client code works with all of the components via the base
    // interface.
    public void ClientCode(Component leaf)
    {
        Console.WriteLine($"RESULT: {leaf.Operation()}\n");
    }

    // Thanks to the fact that the child-management operations are declared
    // in the base Component class, the client code can work with any
    // component, simple or complex, without depending on their concrete
    // classes.
    public void ClientCode2(Component component1, Component component2)
    {
        if (component1.IsComposite())
        {
            component1.Add(component2);
        }
        
        Console.WriteLine($"RESULT: {component1.Operation()}");
    }
}

class Program
{
    static void Main(string[] args)
    {
        Client client = new Client();

        // This way the client code can support the simple leaf
        // components...
        Leaf leaf = new Leaf();
        Console.WriteLine("Client: I get a simple component:");
        client.ClientCode(leaf);

        // ...as well as the complex composites.
        Composite tree = new Composite();
        Composite branch1 = new Composite();
        branch1.Add(new Leaf());
        branch1.Add(new Leaf());
        Composite branch2 = new Composite();
        branch2.Add(new Leaf());
        tree.Add(branch1);
        tree.Add(branch2);
        Console.WriteLine("Client: Now I've got a composite tree:");
        client.ClientCode(tree);

        Console.Write("Client: I don't need to check the components classes even when managing the tree:\n");
        client.ClientCode2(tree, leaf);
    }
}

```
---

## **Decorator**
Decorator is a structural pattern that allows adding new behaviors to objects dynamically by placing them inside special wrapper objects, called decorators.

Using decorators you can wrap objects countless number of times since both target objects and decorators follow the same interface. The resulting object will get a stacking behavior of all wrappers.

**Usage examples:** The Decorator is pretty standard in C# code, especially in code related to streams.

**Identification:** Decorator can be recognized by creation methods or constructors that accept objects of the same class or interface as a current class.

```cs
// The base Component interface defines operations that can be altered by
    // decorators.
    public abstract class Component
    {
        public abstract string Operation();
    }

    // Concrete Components provide default implementations of the operations.
    // There might be several variations of these classes.
    class ConcreteComponent : Component
    {
        public override string Operation()
        {
            return "ConcreteComponent";
        }
    }

    // The base Decorator class follows the same interface as the other
    // components. The primary purpose of this class is to define the wrapping
    // interface for all concrete decorators. The default implementation of the
    // wrapping code might include a field for storing a wrapped component and
    // the means to initialize it.
    abstract class Decorator : Component
    {
        protected Component _component;

        public Decorator(Component component)
        {
            this._component = component;
        }

        public void SetComponent(Component component)
        {
            this._component = component;
        }

        // The Decorator delegates all work to the wrapped component.
        public override string Operation()
        {
            if (this._component != null)
            {
                return this._component.Operation();
            }
            else
            {
                return string.Empty;
            }
        }
    }

    // Concrete Decorators call the wrapped object and alter its result in some
    // way.
    class ConcreteDecoratorA : Decorator
    {
        public ConcreteDecoratorA(Component comp) : base(comp)
        {
        }

        // Decorators may call parent implementation of the operation, instead
        // of calling the wrapped object directly. This approach simplifies
        // extension of decorator classes.
        public override string Operation()
        {
            return $"ConcreteDecoratorA({base.Operation()})";
        }
    }

    // Decorators can execute their behavior either before or after the call to
    // a wrapped object.
    class ConcreteDecoratorB : Decorator
    {
        public ConcreteDecoratorB(Component comp) : base(comp)
        {
        }

        public override string Operation()
        {
            return $"ConcreteDecoratorB({base.Operation()})";
        }
    }
    
    public class Client
    {
        // The client code works with all objects using the Component interface.
        // This way it can stay independent of the concrete classes of
        // components it works with.
        public void ClientCode(Component component)
        {
            Console.WriteLine("RESULT: " + component.Operation());
        }
    }
    
    class Program
    {
        static void Main(string[] args)
        {
            Client client = new Client();

            var simple = new ConcreteComponent();
            Console.WriteLine("Client: I get a simple component:");
            client.ClientCode(simple);
            Console.WriteLine();

            // ...as well as decorated ones.
            //
            // Note how decorators can wrap not only simple components but the
            // other decorators as well.
            ConcreteDecoratorA decorator1 = new ConcreteDecoratorA(simple);
            ConcreteDecoratorB decorator2 = new ConcreteDecoratorB(decorator1);
            Console.WriteLine("Client: Now I've got a decorated component:");
            client.ClientCode(decorator2);
        }
    }
```
---

## **Facade** 
Facade is a structural design pattern that provides a simplified (but limited) interface to a complex system of classes, library or framework.

While Facade decreases the overall complexity of the application, it also helps to move unwanted dependencies to one place.

**Usage examples:** The Facade pattern is commonly used in apps written in C#. It’s especially handy when working with complex libraries and APIs.

**Identification:** Facade can be recognized in a class that has a simple interface, but delegates most of the work to other classes. Usually, facades manage the full life cycle of objects they use.

```cs
// The Facade class provides a simple interface to the complex logic of one
    // or several subsystems. The Facade delegates the client requests to the
    // appropriate objects within the subsystem. The Facade is also responsible
    // for managing their lifecycle. All of this shields the client from the
    // undesired complexity of the subsystem.
    public class Facade
    {
        protected Subsystem1 _subsystem1;
        
        protected Subsystem2 _subsystem2;

        public Facade(Subsystem1 subsystem1, Subsystem2 subsystem2)
        {
            this._subsystem1 = subsystem1;
            this._subsystem2 = subsystem2;
        }
        
        // The Facade's methods are convenient shortcuts to the sophisticated
        // functionality of the subsystems. However, clients get only to a
        // fraction of a subsystem's capabilities.
        public string Operation()
        {
            string result = "Facade initializes subsystems:\n";
            result += this._subsystem1.operation1();
            result += this._subsystem2.operation1();
            result += "Facade orders subsystems to perform the action:\n";
            result += this._subsystem1.operationN();
            result += this._subsystem2.operationZ();
            return result;
        }
    }
    
    // The Subsystem can accept requests either from the facade or client
    // directly. In any case, to the Subsystem, the Facade is yet another
    // client, and it's not a part of the Subsystem.
    public class Subsystem1
    {
        public string operation1()
        {
            return "Subsystem1: Ready!\n";
        }

        public string operationN()
        {
            return "Subsystem1: Go!\n";
        }
    }
    
    // Some facades can work with multiple subsystems at the same time.
    public class Subsystem2
    {
        public string operation1()
        {
            return "Subsystem2: Get ready!\n";
        }

        public string operationZ()
        {
            return "Subsystem2: Fire!\n";
        }
    }


    class Client
    {
        // The client code works with complex subsystems through a simple
        // interface provided by the Facade. When a facade manages the lifecycle
        // of the subsystem, the client might not even know about the existence
        // of the subsystem. This approach lets you keep the complexity under
        // control.
        public static void ClientCode(Facade facade)
        {
            Console.Write(facade.Operation());
        }
    }
    
    class Program
    {
        static void Main(string[] args)
        {
            // The client code may have some of the subsystem's objects already
            // created. In this case, it might be worthwhile to initialize the
            // Facade with these objects instead of letting the Facade create
            // new instances.
            Subsystem1 subsystem1 = new Subsystem1();
            Subsystem2 subsystem2 = new Subsystem2();
            Facade facade = new Facade(subsystem1, subsystem2);
            Client.ClientCode(facade);
        }
    }
```

---

## **Flyweight**
Flyweight is a structural design pattern that allows programs to support vast quantities of objects by keeping their memory consumption low.

The pattern achieves it by sharing parts of object state between multiple objects. In other words, the Flyweight saves RAM by caching the same data used by different objects.

**Usage examples:** The Flyweight pattern has a single purpose: minimizing memory intake. If your program doesn’t struggle with a shortage of RAM, then you might just ignore this pattern for a while.

**Identification:** Flyweight can be recognized by a creation method that returns cached objects instead of creating new.

```cs
// The Flyweight stores a common portion of the state (also called intrinsic
// state) that belongs to multiple real business entities. The Flyweight
// accepts the rest of the state (extrinsic state, unique for each entity)
// via its method parameters.
public class Flyweight
{
    private Car _sharedState;

    public Flyweight(Car car)
    {
        this._sharedState = car;
    }

    public void Operation(Car uniqueState)
    {
        string s = JsonConvert.SerializeObject(this._sharedState);
        string u = JsonConvert.SerializeObject(uniqueState);
        Console.WriteLine($"Flyweight: Displaying shared {s} and unique {u} state.");
    }
}

// The Flyweight Factory creates and manages the Flyweight objects. It
// ensures that flyweights are shared correctly. When the client requests a
// flyweight, the factory either returns an existing instance or creates a
// new one, if it doesn't exist yet.
public class FlyweightFactory
{
    private List<Tuple<Flyweight, string>> flyweights = new List<Tuple<Flyweight, string>>();

    public FlyweightFactory(params Car[] args)
    {
        foreach (var elem in args)
        {
            flyweights.Add(new Tuple<Flyweight, string>(new Flyweight(elem), this.getKey(elem)));
        }
    }

    // Returns a Flyweight's string hash for a given state.
    public string getKey(Car key)
    {
        List<string> elements = new List<string>();

        elements.Add(key.Model);
        elements.Add(key.Color);
        elements.Add(key.Company);

        if (key.Owner != null && key.Number != null)
        {
            elements.Add(key.Number);
            elements.Add(key.Owner);
        }

        elements.Sort();

        return string.Join("_", elements);
    }

    // Returns an existing Flyweight with a given state or creates a new
    // one.
    public Flyweight GetFlyweight(Car sharedState)
    {
        string key = this.getKey(sharedState);

        if (flyweights.Where(t => t.Item2 == key).Count() == 0)
        {
            Console.WriteLine("FlyweightFactory: Can't find a flyweight, creating new one.");
            this.flyweights.Add(new Tuple<Flyweight, string>(new Flyweight(sharedState), key));
        }
        else
        {
            Console.WriteLine("FlyweightFactory: Reusing existing flyweight.");
        }
        return this.flyweights.Where(t => t.Item2 == key).FirstOrDefault().Item1;
    }

    public void listFlyweights()
    {
        var count = flyweights.Count;
        Console.WriteLine($"\nFlyweightFactory: I have {count} flyweights:");
        foreach (var flyweight in flyweights)
        {
            Console.WriteLine(flyweight.Item2);
        }
    }
}

public class Car
{
    public string Owner { get; set; }

    public string Number { get; set; }

    public string Company { get; set; }

    public string Model { get; set; }

    public string Color { get; set; }
}

class Program
{
    static void Main(string[] args)
    {
        // The client code usually creates a bunch of pre-populated
        // flyweights in the initialization stage of the application.
        var factory = new FlyweightFactory(
            new Car { Company = "Chevrolet", Model = "Camaro2018", Color = "pink" },
            new Car { Company = "Mercedes Benz", Model = "C300", Color = "black" },
            new Car { Company = "Mercedes Benz", Model = "C500", Color = "red" },
            new Car { Company = "BMW", Model = "M5", Color = "red" },
            new Car { Company = "BMW", Model = "X6", Color = "white" }
        );
        factory.listFlyweights();

        addCarToPoliceDatabase(factory, new Car {
            Number = "CL234IR",
            Owner = "James Doe",
            Company = "BMW",
            Model = "M5",
            Color = "red"
        });

        addCarToPoliceDatabase(factory, new Car {
            Number = "CL234IR",
            Owner = "James Doe",
            Company = "BMW",
            Model = "X1",
            Color = "red"
        });

        factory.listFlyweights();
    }

    public static void addCarToPoliceDatabase(FlyweightFactory factory, Car car)
    {
        Console.WriteLine("\nClient: Adding a car to database.");

        var flyweight = factory.GetFlyweight(new Car {
            Color = car.Color,
            Model = car.Model,
            Company = car.Company
        });

        // The client code either stores or calculates extrinsic state and
        // passes it to the flyweight's methods.
        flyweight.Operation(car);
    }
}
```
---

## **Proxy**
Proxy is a structural design pattern that provides an object that acts as a substitute for a real service object used by a client. A proxy receives client requests, does some work (access control, caching, etc.) and then passes the request to a service object.

The proxy object has the same interface as a service, which makes it interchangeable with a real object when passed to a client.

**Usage examples:** While the Proxy pattern isn’t a frequent guest in most C# applications, it’s still very handy in some special cases. It’s irreplaceable when you want to add some additional behaviors to an object of some existing class without changing the client code.

**Identification:** Proxies delegate all of the real work to some other object. Each proxy method should, in the end, refer to a service object unless the proxy is a subclass of a service.

```cs
// The Subject interface declares common operations for both RealSubject and
// the Proxy. As long as the client works with RealSubject using this
// interface, you'll be able to pass it a proxy instead of a real subject.
public interface ISubject
{
    void Request();
}

// The RealSubject contains some core business logic. Usually, RealSubjects
// are capable of doing some useful work which may also be very slow or
// sensitive - e.g. correcting input data. A Proxy can solve these issues
// without any changes to the RealSubject's code.
class RealSubject : ISubject
{
    public void Request()
    {
        Console.WriteLine("RealSubject: Handling Request.");
    }
}

// The Proxy has an interface identical to the RealSubject.
class Proxy : ISubject
{
    private RealSubject _realSubject;
    
    public Proxy(RealSubject realSubject)
    {
        this._realSubject = realSubject;
    }
    
    // The most common applications of the Proxy pattern are lazy loading,
    // caching, controlling the access, logging, etc. A Proxy can perform
    // one of these things and then, depending on the result, pass the
    // execution to the same method in a linked RealSubject object.
    public void Request()
    {
        if (this.CheckAccess())
        {
            this._realSubject.Request();

            this.LogAccess();
        }
    }
    
    public bool CheckAccess()
    {
        // Some real checks should go here.
        Console.WriteLine("Proxy: Checking access prior to firing a real request.");

        return true;
    }
    
    public void LogAccess()
    {
        Console.WriteLine("Proxy: Logging the time of request.");
    }
}

public class Client
{
    // The client code is supposed to work with all objects (both subjects
    // and proxies) via the Subject interface in order to support both real
    // subjects and proxies. In real life, however, clients mostly work with
    // their real subjects directly. In this case, to implement the pattern
    // more easily, you can extend your proxy from the real subject's class.
    public void ClientCode(ISubject subject)
    {
        // ...
        
        subject.Request();
        
        // ...
    }
}

class Program
{
    static void Main(string[] args)
    {
        Client client = new Client();
        
        Console.WriteLine("Client: Executing the client code with a real subject:");
        RealSubject realSubject = new RealSubject();
        client.ClientCode(realSubject);

        Console.WriteLine();

        Console.WriteLine("Client: Executing the same client code with a proxy:");
        Proxy proxy = new Proxy(realSubject);
        client.ClientCode(proxy);
    }
}
```
---

# **Behavioral patterns** 

**Behavioral design patterns are concerned with algorithms and the assignment of responsibilities between objects.**

---
## **Chain of Responsibility**
Also known as: CoR, Chain of Command

Chain of Responsibility is a behavioral design pattern that lets you pass requests along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass it to the next handler in the chain.

**Usage examples:** The Chain of Responsibility is pretty common in C#. It’s mostly relevant when your code operates with chains of objects, such as filters, event chains, etc.

**Identification:** The pattern is recognizable by behavioral methods of one group of objects that indirectly call the same methods in other objects, while all the objects follow the common interface.

```cs
// The Handler interface declares a method for building the chain of
// handlers. It also declares a method for executing a request.
public interface IHandler
{
    IHandler SetNext(IHandler handler);
    
    object Handle(object request);
}

// The default chaining behavior can be implemented inside a base handler
// class.
abstract class AbstractHandler : IHandler
{
    private IHandler _nextHandler;

    public IHandler SetNext(IHandler handler)
    {
        this._nextHandler = handler;
        
        // Returning a handler from here will let us link handlers in a
        // convenient way like this:
        // monkey.SetNext(squirrel).SetNext(dog);
        return handler;
    }
    
    public virtual object Handle(object request)
    {
        if (this._nextHandler != null)
        {
            return this._nextHandler.Handle(request);
        }
        else
        {
            return null;
        }
    }
}

class MonkeyHandler : AbstractHandler
{
    public override object Handle(object request)
    {
        if ((request as string) == "Banana")
        {
            return $"Monkey: I'll eat the {request.ToString()}.\n";
        }
        else
        {
            return base.Handle(request);
        }
    }
}

class SquirrelHandler : AbstractHandler
{
    public override object Handle(object request)
    {
        if (request.ToString() == "Nut")
        {
            return $"Squirrel: I'll eat the {request.ToString()}.\n";
        }
        else
        {
            return base.Handle(request);
        }
    }
}

class DogHandler : AbstractHandler
{
    public override object Handle(object request)
    {
        if (request.ToString() == "MeatBall")
        {
            return $"Dog: I'll eat the {request.ToString()}.\n";
        }
        else
        {
            return base.Handle(request);
        }
    }
}

class Client
{
    // The client code is usually suited to work with a single handler. In
    // most cases, it is not even aware that the handler is part of a chain.
    public static void ClientCode(AbstractHandler handler)
    {
        foreach (var food in new List<string> { "Nut", "Banana", "Cup of coffee" })
        {
            Console.WriteLine($"Client: Who wants a {food}?");

            var result = handler.Handle(food);

            if (result != null)
            {
                Console.Write($"   {result}");
            }
            else
            {
                Console.WriteLine($"   {food} was left untouched.");
            }
        }
    }
}

class Program
{
    static void Main(string[] args)
    {
        // The other part of the client code constructs the actual chain.
        var monkey = new MonkeyHandler();
        var squirrel = new SquirrelHandler();
        var dog = new DogHandler();

        monkey.SetNext(squirrel).SetNext(dog);

        // The client should be able to send a request to any handler, not
        // just the first one in the chain.
        Console.WriteLine("Chain: Monkey > Squirrel > Dog\n");
        Client.ClientCode(monkey);
        Console.WriteLine();

        Console.WriteLine("Subchain: Squirrel > Dog\n");
        Client.ClientCode(squirrel);
    }
}
```


---

## **Command**
Also known as: Action, Transaction
 
Command is a behavioral design pattern that turns a request into a stand-alone object that contains all information about the request. This transformation lets you pass requests as a method arguments, delay or queue a request’s execution, and support undoable operations.

**Usage examples:** The Command pattern is pretty common in C# code. Most often it’s used as an alternative for callbacks to parameterizing UI elements with actions. It’s also used for queueing tasks, tracking operations history, etc.

**Identification:** The Command pattern is recognizable by behavioral methods in an abstract/interface type (sender) which invokes a method in an implementation of a different abstract/interface type (receiver) which has been encapsulated by the command implementation during its creation. Command classes are usually limited to specific actions.

```cs
// The Command interface declares a method for executing a command.
public interface ICommand
{
    void Execute();
}

// Some commands can implement simple operations on their own.
class SimpleCommand : ICommand
{
    private string _payload = string.Empty;

    public SimpleCommand(string payload)
    {
        this._payload = payload;
    }

    public void Execute()
    {
        Console.WriteLine($"SimpleCommand: See, I can do simple things like printing ({this._payload})");
    }
}

// However, some commands can delegate more complex operations to other
// objects, called "receivers."
class ComplexCommand : ICommand
{
    private Receiver _receiver;

    // Context data, required for launching the receiver's methods.
    private string _a;

    private string _b;

    // Complex commands can accept one or several receiver objects along
    // with any context data via the constructor.
    public ComplexCommand(Receiver receiver, string a, string b)
    {
        this._receiver = receiver;
        this._a = a;
        this._b = b;
    }

    // Commands can delegate to any methods of a receiver.
    public void Execute()
    {
        Console.WriteLine("ComplexCommand: Complex stuff should be done by a receiver object.");
        this._receiver.DoSomething(this._a);
        this._receiver.DoSomethingElse(this._b);
    }
}

// The Receiver classes contain some important business logic. They know how
// to perform all kinds of operations, associated with carrying out a
// request. In fact, any class may serve as a Receiver.
class Receiver
{
    public void DoSomething(string a)
    {
        Console.WriteLine($"Receiver: Working on ({a}.)");
    }

    public void DoSomethingElse(string b)
    {
        Console.WriteLine($"Receiver: Also working on ({b}.)");
    }
}

// The Invoker is associated with one or several commands. It sends a
// request to the command.
class Invoker
{
    private ICommand _onStart;

    private ICommand _onFinish;

    // Initialize commands.
    public void SetOnStart(ICommand command)
    {
        this._onStart = command;
    }

    public void SetOnFinish(ICommand command)
    {
        this._onFinish = command;
    }

    // The Invoker does not depend on concrete command or receiver classes.
    // The Invoker passes a request to a receiver indirectly, by executing a
    // command.
    public void DoSomethingImportant()
    {
        Console.WriteLine("Invoker: Does anybody want something done before I begin?");
        if (this._onStart is ICommand)
        {
            this._onStart.Execute();
        }
        
        Console.WriteLine("Invoker: ...doing something really important...");
        
        Console.WriteLine("Invoker: Does anybody want something done after I finish?");
        if (this._onFinish is ICommand)
        {
            this._onFinish.Execute();
        }
    }
}

class Program
{
    static void Main(string[] args)
    {
        // The client code can parameterize an invoker with any commands.
        Invoker invoker = new Invoker();
        invoker.SetOnStart(new SimpleCommand("Say Hi!"));
        Receiver receiver = new Receiver();
        invoker.SetOnFinish(new ComplexCommand(receiver, "Send email", "Save report"));

        invoker.DoSomethingImportant();
    }
}
```

---
## **Iterator**

Iterator is a behavioral design pattern that lets you traverse elements of a collection without exposing its underlying representation (list, stack, tree, etc.).

**Usage examples:** The pattern is very common in C# code. Many frameworks and libraries use it to provide a standard way for traversing their collections.

**Identification:** Iterator is easy to recognize by the navigation methods (such as next, previous and others). Client code that uses iterators might not have direct access to the collection being traversed.

```cs
abstract class Iterator : IEnumerator
{
    object IEnumerator.Current => Current();

    // Returns the key of the current element
    public abstract int Key();
    
    // Returns the current element
    public abstract object Current();
    
    // Move forward to next element
    public abstract bool MoveNext();
    
    // Rewinds the Iterator to the first element
    public abstract void Reset();
}

abstract class IteratorAggregate : IEnumerable
{
    // Returns an Iterator or another IteratorAggregate for the implementing
    // object.
    public abstract IEnumerator GetEnumerator();
}

// Concrete Iterators implement various traversal algorithms. These classes
// store the current traversal position at all times.
class AlphabeticalOrderIterator : Iterator
{
    private WordsCollection _collection;
    
    // Stores the current traversal position. An iterator may have a lot of
    // other fields for storing iteration state, especially when it is
    // supposed to work with a particular kind of collection.
    private int _position = -1;
    
    private bool _reverse = false;

    public AlphabeticalOrderIterator(WordsCollection collection, bool reverse = false)
    {
        this._collection = collection;
        this._reverse = reverse;

        if (reverse)
        {
            this._position = collection.getItems().Count;
        }
    }
    
    public override object Current()
    {
        return this._collection.getItems()[_position];
    }

    public override int Key()
    {
        return this._position;
    }
    
    public override bool MoveNext()
    {
        int updatedPosition = this._position + (this._reverse ? -1 : 1);

        if (updatedPosition >= 0 && updatedPosition < this._collection.getItems().Count)
        {
            this._position = updatedPosition;
            return true;
        }
        else
        {
            return false;
        }
    }
    
    public override void Reset()
    {
        this._position = this._reverse ? this._collection.getItems().Count - 1 : 0;
    }
}

// Concrete Collections provide one or several methods for retrieving fresh
// iterator instances, compatible with the collection class.
class WordsCollection : IteratorAggregate
{
    List<string> _collection = new List<string>();
    
    bool _direction = false;
    
    public void ReverseDirection()
    {
        _direction = !_direction;
    }
    
    public List<string> getItems()
    {
        return _collection;
    }
    
    public void AddItem(string item)
    {
        this._collection.Add(item);
    }
    
    public override IEnumerator GetEnumerator()
    {
        return new AlphabeticalOrderIterator(this, _direction);
    }
}

class Program
{
    static void Main(string[] args)
    {
        // The client code may or may not know about the Concrete Iterator
        // or Collection classes, depending on the level of indirection you
        // want to keep in your program.
        var collection = new WordsCollection();
        collection.AddItem("First");
        collection.AddItem("Second");
        collection.AddItem("Third");

        Console.WriteLine("Straight traversal:");

        foreach (var element in collection)
        {
            Console.WriteLine(element);
        }

        Console.WriteLine("\nReverse traversal:");

        collection.ReverseDirection();

        foreach (var element in collection)
        {
            Console.WriteLine(element);
        }
    }
}

```

---
## **Mediator**
Also known as: Intermediary, Controller

Mediator is a behavioral design pattern that lets you reduce chaotic dependencies between objects. The pattern restricts direct communications between the objects and forces them to collaborate only via a mediator object.

**Usage examples:** The most popular usage of the Mediator pattern in C# code is facilitating communications between GUI components of an app. The synonym of the Mediator is the Controller part of MVC pattern.

```cs
// The Mediator interface declares a method used by components to notify the
// mediator about various events. The Mediator may react to these events and
// pass the execution to other components.
public interface IMediator
{
    void Notify(object sender, string ev);
}

// Concrete Mediators implement cooperative behavior by coordinating several
// components.
class ConcreteMediator : IMediator
{
    private Component1 _component1;

    private Component2 _component2;

    public ConcreteMediator(Component1 component1, Component2 component2)
    {
        this._component1 = component1;
        this._component1.SetMediator(this);
        this._component2 = component2;
        this._component2.SetMediator(this);
    } 

    public void Notify(object sender, string ev)
    {
        if (ev == "A")
        {
            Console.WriteLine("Mediator reacts on A and triggers folowing operations:");
            this._component2.DoC();
        }
        if (ev == "D")
        {
            Console.WriteLine("Mediator reacts on D and triggers following operations:");
            this._component1.DoB();
            this._component2.DoC();
        }
    }
}

// The Base Component provides the basic functionality of storing a
// mediator's instance inside component objects.
class BaseComponent
{
    protected IMediator _mediator;

    public BaseComponent(IMediator mediator = null)
    {
        this._mediator = mediator;
    }

    public void SetMediator(IMediator mediator)
    {
        this._mediator = mediator;
    }
}

// Concrete Components implement various functionality. They don't depend on
// other components. They also don't depend on any concrete mediator
// classes.
class Component1 : BaseComponent
{
    public void DoA()
    {
        Console.WriteLine("Component 1 does A.");

        this._mediator.Notify(this, "A");
    }

    public void DoB()
    {
        Console.WriteLine("Component 1 does B.");

        this._mediator.Notify(this, "B");
    }
}

class Component2 : BaseComponent
{
    public void DoC()
    {
        Console.WriteLine("Component 2 does C.");

        this._mediator.Notify(this, "C");
    }

    public void DoD()
    {
        Console.WriteLine("Component 2 does D.");

        this._mediator.Notify(this, "D");
    }
}

class Program
{
    static void Main(string[] args)
    {
        // The client code.
        Component1 component1 = new Component1();
        Component2 component2 = new Component2();
        new ConcreteMediator(component1, component2);

        Console.WriteLine("Client triggers operation A.");
        component1.DoA();

        Console.WriteLine();

        Console.WriteLine("Client triggers operation D.");
        component2.DoD();
    }
}
```

---
## **Memento**
Also known as: Snapshot

Memento is a behavioral design pattern that lets you save and restore the previous state of an object without revealing the details of its implementation.

**Usage examples:** The Memento’s principle can be achieved using serialization, which is quite common in C#. While it’s not the only and the most efficient way to make snapshots of an object’s state, it still allows storing state backups while protecting the originator’s structure from other objects.

```cs
// The Originator holds some important state that may change over time. It
// also defines a method for saving the state inside a memento and another
// method for restoring the state from it.
class Originator
{
    // For the sake of simplicity, the originator's state is stored inside a
    // single variable.
    private string _state;

    public Originator(string state)
    {
        this._state = state;
        Console.WriteLine("Originator: My initial state is: " + state);
    }

    // The Originator's business logic may affect its internal state.
    // Therefore, the client should backup the state before launching
    // methods of the business logic via the save() method.
    public void DoSomething()
    {
        Console.WriteLine("Originator: I'm doing something important.");
        this._state = this.GenerateRandomString(30);
        Console.WriteLine($"Originator: and my state has changed to: {_state}");
    }

    private string GenerateRandomString(int length = 10)
    {
        string allowedSymbols = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
        string result = string.Empty;

        while (length > 0)
        {
            result += allowedSymbols[new Random().Next(0, allowedSymbols.Length)];

            Thread.Sleep(12);

            length--;
        }

        return result;
    }

    // Saves the current state inside a memento.
    public IMemento Save()
    {
        return new ConcreteMemento(this._state);
    }

    // Restores the Originator's state from a memento object.
    public void Restore(IMemento memento)
    {
        if (!(memento is ConcreteMemento))
        {
            throw new Exception("Unknown memento class " + memento.ToString());
        }

        this._state = memento.GetState();
        Console.Write($"Originator: My state has changed to: {_state}");
    }
}

// The Memento interface provides a way to retrieve the memento's metadata,
// such as creation date or name. However, it doesn't expose the
// Originator's state.
public interface IMemento
{
    string GetName();

    string GetState();

    DateTime GetDate();
}

// The Concrete Memento contains the infrastructure for storing the
// Originator's state.
class ConcreteMemento : IMemento
{
    private string _state;

    private DateTime _date;

    public ConcreteMemento(string state)
    {
        this._state = state;
        this._date = DateTime.Now;
    }

    // The Originator uses this method when restoring its state.
    public string GetState()
    {
        return this._state;
    }
    
    // The rest of the methods are used by the Caretaker to display
    // metadata.
    public string GetName()
    {
        return $"{this._date} / ({this._state.Substring(0, 9)})...";
    }

    public DateTime GetDate()
    {
        return this._date;
    }
}

// The Caretaker doesn't depend on the Concrete Memento class. Therefore, it
// doesn't have access to the originator's state, stored inside the memento.
// It works with all mementos via the base Memento interface.
class Caretaker
{
    private List<IMemento> _mementos = new List<IMemento>();

    private Originator _originator = null;

    public Caretaker(Originator originator)
    {
        this._originator = originator;
    }

    public void Backup()
    {
        Console.WriteLine("\nCaretaker: Saving Originator's state...");
        this._mementos.Add(this._originator.Save());
    }

    public void Undo()
    {
        if (this._mementos.Count == 0)
        {
            return;
        }

        var memento = this._mementos.Last();
        this._mementos.Remove(memento);

        Console.WriteLine("Caretaker: Restoring state to: " + memento.GetName());

        try
        {
            this._originator.Restore(memento);
        }
        catch (Exception)
        {
            this.Undo();
        }
    }

    public void ShowHistory()
    {
        Console.WriteLine("Caretaker: Here's the list of mementos:");

        foreach (var memento in this._mementos)
        {
            Console.WriteLine(memento.GetName());
        }
    }
}

class Program
{
    static void Main(string[] args)
    {
        // Client code.
        Originator originator = new Originator("Super-duper-super-puper-super.");
        Caretaker caretaker = new Caretaker(originator);

        caretaker.Backup();
        originator.DoSomething();

        caretaker.Backup();
        originator.DoSomething();

        caretaker.Backup();
        originator.DoSomething();

        Console.WriteLine();
        caretaker.ShowHistory();

        Console.WriteLine("\nClient: Now, let's rollback!\n");
        caretaker.Undo();

        Console.WriteLine("\n\nClient: Once more!\n");
        caretaker.Undo();

        Console.WriteLine();
    }
}
```
---

## **Observer**
Also known as: **Event-Subscriber**, **Listener**

**Usage examples:** The Observer pattern is pretty common in C# code, especially in the GUI components. It provides a way to react to events happening in other objects without coupling to their classes.

**Identification:** The pattern can be recognized by subscription methods, that store objects in a list and by calls to the update method issued to objects in that list.

Observer is a behavioral design pattern that lets you define a subscription mechanism to notify multiple objects about any events that happen to the object they’re observing.

```cs
public interface IObserver
    {
        // Receive update from subject
        void Update(ISubject subject);
    }

    public interface ISubject
    {
        // Attach an observer to the subject.
        void Attach(IObserver observer);

        // Detach an observer from the subject.
        void Detach(IObserver observer);

        // Notify all observers about an event.
        void Notify();
    }

    // The Subject owns some important state and notifies observers when the
    // state changes.
    public class Subject : ISubject
    {
        // For the sake of simplicity, the Subject's state, essential to all
        // subscribers, is stored in this variable.
        public int State { get; set; } = -0;

        // List of subscribers. In real life, the list of subscribers can be
        // stored more comprehensively (categorized by event type, etc.).
        private List<IObserver> _observers = new List<IObserver>();

        // The subscription management methods.
        public void Attach(IObserver observer)
        {
            Console.WriteLine("Subject: Attached an observer.");
            this._observers.Add(observer);
        }

        public void Detach(IObserver observer)
        {
            this._observers.Remove(observer);
            Console.WriteLine("Subject: Detached an observer.");
        }

        // Trigger an update in each subscriber.
        public void Notify()
        {
            Console.WriteLine("Subject: Notifying observers...");

            foreach (var observer in _observers)
            {
                observer.Update(this);
            }
        }

        // Usually, the subscription logic is only a fraction of what a Subject
        // can really do. Subjects commonly hold some important business logic,
        // that triggers a notification method whenever something important is
        // about to happen (or after it).
        public void SomeBusinessLogic()
        {
            Console.WriteLine("\nSubject: I'm doing something important.");
            this.State = new Random().Next(0, 10);

            Thread.Sleep(15);

            Console.WriteLine("Subject: My state has just changed to: " + this.State);
            this.Notify();
        }
    }

    // Concrete Observers react to the updates issued by the Subject they had
    // been attached to.
    class ConcreteObserverA : IObserver
    {
        public void Update(ISubject subject)
        {            
            if ((subject as Subject).State < 3)
            {
                Console.WriteLine("ConcreteObserverA: Reacted to the event.");
            }
        }
    }

    class ConcreteObserverB : IObserver
    {
        public void Update(ISubject subject)
        {
            if ((subject as Subject).State == 0 || (subject as Subject).State >= 2)
            {
                Console.WriteLine("ConcreteObserverB: Reacted to the event.");
            }
        }
    }
    
    class Program
    {
        static void Main(string[] args)
        {
            // The client code.
            var subject = new Subject();
            var observerA = new ConcreteObserverA();
            subject.Attach(observerA);

            var observerB = new ConcreteObserverB();
            subject.Attach(observerB);

            subject.SomeBusinessLogic();
            subject.SomeBusinessLogic();

            subject.Detach(observerB);

            subject.SomeBusinessLogic();
        }
    }
```

---

## **State**

State is a behavioral design pattern that lets an object alter its behavior when its internal state changes. It appears as if the object changed its class.

**Usage examples:** The State pattern is commonly used in C# to convert massive switch-base state machines into objects.

**Identification:** State pattern can be recognized by methods that change their behavior depending on the objects’ state, controlled externally.

```cs
// The Context defines the interface of interest to clients. It also
// maintains a reference to an instance of a State subclass, which
// represents the current state of the Context.
class Context
{
    // A reference to the current state of the Context.
    private State _state = null;

    public Context(State state)
    {
        this.TransitionTo(state);
    }

    // The Context allows changing the State object at runtime.
    public void TransitionTo(State state)
    {
        Console.WriteLine($"Context: Transition to {state.GetType().Name}.");
        this._state = state;
        this._state.SetContext(this);
    }

    // The Context delegates part of its behavior to the current State
    // object.
    public void Request1()
    {
        this._state.Handle1();
    }

    public void Request2()
    {
        this._state.Handle2();
    }
}

// The base State class declares methods that all Concrete State should
// implement and also provides a backreference to the Context object,
// associated with the State. This backreference can be used by States to
// transition the Context to another State.
abstract class State
{
    protected Context _context;

    public void SetContext(Context context)
    {
        this._context = context;
    }

    public abstract void Handle1();

    public abstract void Handle2();
}

// Concrete States implement various behaviors, associated with a state of
// the Context.
class ConcreteStateA : State
{
    public override void Handle1()
    {
        Console.WriteLine("ConcreteStateA handles request1.");
        Console.WriteLine("ConcreteStateA wants to change the state of the context.");
        this._context.TransitionTo(new ConcreteStateB());
    }

    public override void Handle2()
    {
        Console.WriteLine("ConcreteStateA handles request2.");
    }
}

class ConcreteStateB : State
{
    public override void Handle1()
    {
        Console.Write("ConcreteStateB handles request1.");
    }

    public override void Handle2()
    {
        Console.WriteLine("ConcreteStateB handles request2.");
        Console.WriteLine("ConcreteStateB wants to change the state of the context.");
        this._context.TransitionTo(new ConcreteStateA());
    }
}

class Program
{
    static void Main(string[] args)
    {
        // The client code.
        var context = new Context(new ConcreteStateA());
        context.Request1();
        context.Request2();
    }
}
```


---

## **Strategy**

Strategy is a behavioral design pattern that lets you define a family of algorithms, put each of them into a separate class, and make their objects interchangeable.

**Usage examples:** The Strategy pattern is very common in C# code. It’s often used in various frameworks to provide users a way to change the behavior of a class without extending it.

**Identification:** Strategy pattern can be recognized by a method that lets a nested object do the actual work, as well as a setter that allows replacing that object with a different one.

```cs
// The Context defines the interface of interest to clients.
class Context
{
    // The Context maintains a reference to one of the Strategy objects. The
    // Context does not know the concrete class of a strategy. It should
    // work with all strategies via the Strategy interface.
    private IStrategy _strategy;

    public Context()
    { }

    // Usually, the Context accepts a strategy through the constructor, but
    // also provides a setter to change it at runtime.
    public Context(IStrategy strategy)
    {
        this._strategy = strategy;
    }

    // Usually, the Context allows replacing a Strategy object at runtime.
    public void SetStrategy(IStrategy strategy)
    {
        this._strategy = strategy;
    }

    // The Context delegates some work to the Strategy object instead of
    // implementing multiple versions of the algorithm on its own.
    public void DoSomeBusinessLogic()
    {
        Console.WriteLine("Context: Sorting data using the strategy (not sure how it'll do it)");
        var result = this._strategy.DoAlgorithm(new List<string> { "a", "b", "c", "d", "e" });

        string resultStr = string.Empty;
        foreach (var element in result as List<string>)
        {
            resultStr += element + ",";
        }

        Console.WriteLine(resultStr);
    }
}

// The Strategy interface declares operations common to all supported
// versions of some algorithm.
//
// The Context uses this interface to call the algorithm defined by Concrete
// Strategies.
public interface IStrategy
{
    object DoAlgorithm(object data);
}

// Concrete Strategies implement the algorithm while following the base
// Strategy interface. The interface makes them interchangeable in the
// Context.
class ConcreteStrategyA : IStrategy
{
    public object DoAlgorithm(object data)
    {
        var list = data as List<string>;
        list.Sort();

        return list;
    }
}

class ConcreteStrategyB : IStrategy
{
    public object DoAlgorithm(object data)
    {
        var list = data as List<string>;
        list.Sort();
        list.Reverse();

        return list;
    }
}

class Program
{
    static void Main(string[] args)
    {
        // The client code picks a concrete strategy and passes it to the
        // context. The client should be aware of the differences between
        // strategies in order to make the right choice.
        var context = new Context();

        Console.WriteLine("Client: Strategy is set to normal sorting.");
        context.SetStrategy(new ConcreteStrategyA());
        context.DoSomeBusinessLogic();
        
        Console.WriteLine();
        
        Console.WriteLine("Client: Strategy is set to reverse sorting.");
        context.SetStrategy(new ConcreteStrategyB());
        context.DoSomeBusinessLogic();
    }
}
```
---

## **Template Method**

Template Method is a behavioral design pattern that defines the skeleton of an algorithm in the superclass but lets subclasses override specific steps of the algorithm without changing its structure.

**Usage examples:** The Template Method pattern is quite common in C# frameworks. Developers often use it to provide framework users with a simple means of extending standard functionality using inheritance.

**Identification:** Template Method can be recognized if you see a method in base class that calls a bunch of other methods that are either abstract or empty.

```cs
// The Abstract Class defines a template method that contains a skeleton of
// some algorithm, composed of calls to (usually) abstract primitive
// operations.
//
// Concrete subclasses should implement these operations, but leave the
// template method itself intact.
abstract class AbstractClass
{
    // The template method defines the skeleton of an algorithm.
    public void TemplateMethod()
    {
        this.BaseOperation1();
        this.RequiredOperations1();
        this.BaseOperation2();
        this.Hook1();
        this.RequiredOperation2();
        this.BaseOperation3();
        this.Hook2();
    }

    // These operations already have implementations.
    protected void BaseOperation1()
    {
        Console.WriteLine("AbstractClass says: I am doing the bulk of the work");
    }

    protected void BaseOperation2()
    {
        Console.WriteLine("AbstractClass says: But I let subclasses override some operations");
    }

    protected void BaseOperation3()
    {
        Console.WriteLine("AbstractClass says: But I am doing the bulk of the work anyway");
    }
    
    // These operations have to be implemented in subclasses.
    protected abstract void RequiredOperations1();

    protected abstract void RequiredOperation2();
    
    // These are "hooks." Subclasses may override them, but it's not
    // mandatory since the hooks already have default (but empty)
    // implementation. Hooks provide additional extension points in some
    // crucial places of the algorithm.
    protected virtual void Hook1() { }

    protected virtual void Hook2() { }
}

// Concrete classes have to implement all abstract operations of the base
// class. They can also override some operations with a default
// implementation.
class ConcreteClass1 : AbstractClass
{
    protected override void RequiredOperations1()
    {
        Console.WriteLine("ConcreteClass1 says: Implemented Operation1");
    }

    protected override void RequiredOperation2()
    {
        Console.WriteLine("ConcreteClass1 says: Implemented Operation2");
    }
}

// Usually, concrete classes override only a fraction of base class'
// operations.
class ConcreteClass2 : AbstractClass
{
    protected override void RequiredOperations1()
    {
        Console.WriteLine("ConcreteClass2 says: Implemented Operation1");
    }

    protected override void RequiredOperation2()
    {
        Console.WriteLine("ConcreteClass2 says: Implemented Operation2");
    }

    protected override void Hook1()
    {
        Console.WriteLine("ConcreteClass2 says: Overridden Hook1");
    }
}

class Client
{
    // The client code calls the template method to execute the algorithm.
    // Client code does not have to know the concrete class of an object it
    // works with, as long as it works with objects through the interface of
    // their base class.
    public static void ClientCode(AbstractClass abstractClass)
    {
        // ...
        abstractClass.TemplateMethod();
        // ...
    }
}

class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("Same client code can work with different subclasses:");

        Client.ClientCode(new ConcreteClass1());

        Console.Write("\n");
        
        Console.WriteLine("Same client code can work with different subclasses:");
        Client.ClientCode(new ConcreteClass2());
    }
}
```

## **Visitor**

> Visitor is a behavioral design pattern that lets you separate algorithms from the objects on which they operate which allows adding new behaviors to existing class hierarchy without altering any existing code.

```cs
// The Component interface declares an `accept` method that should take the
// base visitor interface as an argument.
public interface IComponent
{
    void Accept(IVisitor visitor);
}

// Each Concrete Component must implement the `Accept` method in such a way
// that it calls the visitor's method corresponding to the component's
// class.
public class ConcreteComponentA : IComponent
{
    // Note that we're calling `VisitConcreteComponentA`, which matches the
    // current class name. This way we let the visitor know the class of the
    // component it works with.
    public void Accept(IVisitor visitor)
    {
        visitor.VisitConcreteComponentA(this);
    }

    // Concrete Components may have special methods that don't exist in
    // their base class or interface. The Visitor is still able to use these
    // methods since it's aware of the component's concrete class.
    public string ExclusiveMethodOfConcreteComponentA()
    {
        return "A";
    }
}

public class ConcreteComponentB : IComponent
{
    // Same here: VisitConcreteComponentB => ConcreteComponentB
    public void Accept(IVisitor visitor)
    {
        visitor.VisitConcreteComponentB(this);
    }

    public string SpecialMethodOfConcreteComponentB()
    {
        return "B";
    }
}

// The Visitor Interface declares a set of visiting methods that correspond
// to component classes. The signature of a visiting method allows the
// visitor to identify the exact class of the component that it's dealing
// with.
public interface IVisitor
{
    void VisitConcreteComponentA(ConcreteComponentA element);
    void VisitConcreteComponentB(ConcreteComponentB element);
}

// Concrete Visitors implement several versions of the same algorithm, which
// can work with all concrete component classes.
//
// You can experience the biggest benefit of the Visitor pattern when using
// it with a complex object structure, such as a Composite tree. In this
// case, it might be helpful to store some intermediate state of the
// algorithm while executing visitor's methods over various objects of the
// structure.
class ConcreteVisitor1 : IVisitor
{
    public void VisitConcreteComponentA(ConcreteComponentA element)
    {
        Console.WriteLine(element.ExclusiveMethodOfConcreteComponentA() + " + ConcreteVisitor1");
    }

    public void VisitConcreteComponentB(ConcreteComponentB element)
    {
        Console.WriteLine(element.SpecialMethodOfConcreteComponentB() + " + ConcreteVisitor1");
    }
}

class ConcreteVisitor2 : IVisitor
{
    public void VisitConcreteComponentA(ConcreteComponentA element)
    {
        Console.WriteLine(element.ExclusiveMethodOfConcreteComponentA() + " + ConcreteVisitor2");
    }

    public void VisitConcreteComponentB(ConcreteComponentB element)
    {
        Console.WriteLine(element.SpecialMethodOfConcreteComponentB() + " + ConcreteVisitor2");
    }
}

public class Client
{
    // The client code can run visitor operations over any set of elements
    // without figuring out their concrete classes. The accept operation
    // directs a call to the appropriate operation in the visitor object.
    public static void ClientCode(List<IComponent> components, IVisitor visitor)
    {
        foreach (var component in components)
        {
            component.Accept(visitor);
        }
    }
}

class Program
{
    static void Main(string[] args)
    {
        List<IComponent> components = new List<IComponent>
        {
            new ConcreteComponentA(),
            new ConcreteComponentB()
        };

        Console.WriteLine("The client code works with all visitors via the base Visitor interface:");
        var visitor1 = new ConcreteVisitor1();
        Client.ClientCode(components,visitor1);

        Console.WriteLine();

        Console.WriteLine("It allows the same client code to work with different types of visitors:");
        var visitor2 = new ConcreteVisitor2();
        Client.ClientCode(components, visitor2);
    }
}
```
---
