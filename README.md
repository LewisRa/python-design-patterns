# Design patterns in Python

## Design Pattern Classification 
1. Creational - deals with object creation 
2. Structural- Object relationship
3. Behaviorial - Object interaction and responsibility

## SOLID
S- Single responsibility <br>
O - Open/Closed principle <br>
L - Liskov substitution principle <br>
I - Interface segregation principle ---> Abstract base classes <br><br>
D - Dependency Inversion Principle <br>

---
This repository contains python implementations of common design patterns based on
[Design Patterns with Python](https://app.pluralsight.com/library/courses/python-design-patterns) course.

## Requirements

* Python 3.6+
* pip3

## Installation

```bash
$ git clone https://github.com/kaduev13/python-design-patterns.git
$ cd python-design-patterns
$ pip3 install -r requirements.txt
```

## Testing

```bash
$ pip3 install -r requirements-test.txt
$ pytest
```

## Patterns

### ABC Usage

Simple ABC usage scenario.
* `Runnable` – is an interface with methods `run`, `stop` and property `is_running`.
* `BadClass` – the class that inherited from Runnable, but does not implement it's methods/property.
* `GoodClass` – the class that fully implements Runnable interface.

### Strategy

Strategy pattern implementation allows you to encapsulate algorithms into the absolutely separated objects.

#### Before the Strategy: Problems Discovered
- Violates single responsibility
An order should not be concerned with how the products will eventually be shipped. 

- Violates Open/Closed principle
Shipping Cost method computes the cost according to the shippers stored in order. If the shipper is not one of thoese, it raise an expectation. The method then calls a provate

- Violates Dependency Inversion principle 
Since we are programming to a concrete class and method, the shipping cost class and its shipping cost method is not abstract

- Long list of if/ elif clauses is a bit fagile anf whenever you see if/elif, it's good to ask if the code should be written better


In this example we need to calculate Order shipping cost, that depends on shipping type: postal, FedEx, UPS.
* `Order` – just an empty class, that represents real order.
* `ShippingCost` – context, that contains selected shipping strategy.
* `Strategy` – abstract strategy with the only method `calculate(order: Order)`.
* `FedExStrategy`, `PostalStrategy`, `UPSStrategy` – implementations of concrete shipping strategies.

### Observer

Classic observer pattern implementation located in `observer.lib` package.
* `observer.lib.Observer` – abstract observer class
* `observer.lib.Subject` – abstract subject class with implemented default behavior 

In the example we have some "source" – `KPIs`, that is a `Subject` descendant. Also
we have two observers: `ClosedTickets` and `KPIsDisplay`. When the `KPIs` changes, it
notifies observers instances and they do their job.
* `KPIs` – event source, subject
* `ClosedTickets` – observer, calculates (and prints) the sum of total closed tickets
* `KPIsDisplay` – observer, just displays the current KPIs every time `update` is fired.

### Command (Action or Transaction Pattern)(Behavioral)

Command Pattern provides a way to encapsulate a request as an object. The encapusulation lets you parametrize various objetcs with different requests.The command pattern also provides a simple way to support quenues and logs i.e. updating a databse ot creating an audit trail of requests. Command pattern with commands auto-discovery and fallback to NoCommand if there is no available command.
- separate command logic
- add potential capabilites
  - validation
  - undo


* `command.commands.*` – contains custom available commands.
* `command.executor.CommandExecutor` – the class, that is able to discovery and execute commands.
* `command.command.Command` – abstract console command.
* `command.no_command.NoCommand` – fallback command, that will be executed if no other command found.

### Singleton

Two implementations of Singleton pattern.

* `singleton.singleton_base.SingletonBase` – singleton, implemented as a base class.
* `singleton.singleton_meta.SingletonMeta` – singleton, implemented as a meta class.

### Builder
The Builder Pattern
Builder helps by separating the construction of a complex object, say an object representing a custom computer from its representation, what the specifications of the finished computer actually look like. Builder does this by encapsulating the construction of the object. Thereby obeying the principle, encapsulate what varies, which is a corollary of the single responsibility principle, the S in SOLID. It also allows for a multi-step construction process. 

The Computer Builder: Too Many Parameters!
For a motivating example, let's implement the custom computer builder. Now a custom computer can have many components, and this sort of thing can lead to some bad design choices, as we'll see. For this demo, I'll be using the free Visual Studio Community edition, a lean cousin of the full Microsoft Visual Studio.  Well here we have the main program, in our first attempt at writing a custom computer builder. It imports one class, Computer, which looks like this. Note that the constructor uses lots of parameters, one for each component. This class also includes a display method to show the results of building the custom computer. So back in the main program, we can see that it instantiates the computer object, specifying all the required parameters. Long parameter lists like this can be a sign that there may be a better way to approach this sort of problem. Now let's run this program to see if it works. Opening up the Immediate Window, we can see the results of execution. The program works. Now, I'm still bothered by that long parameter list, and you may also encounter constructors like this in your own work. They can be hard to understand and maintain since it's not immediately obvious which parameters are mandatory and which are optional. Sometimes constructors seem to grow, like this one, as a project adds new features. The Builder pattern is good at handling just this sort of issue, but before we get there let's see another way to tackle this problem.

The Computer Builder: Exposing the Attributes
For the second attempt let's get rid of those constructor parameters. In fact, I won't need a custom constructor at all, so all we have in the Computer class is the display method. Instead of constructor parameters, I'm setting the attributes in the main program. This is actually one common technique to solving the parameter proliferation problem, though not a very good one. Running this version, we'll see that it produces the same results as before, so this one also works, and we've solved one problem, too many constructor parameters. But now we've got a new one, directly setting attributes in the client program. Not only is this error prone and maintenance unfriendly, it also goes directly against the encapsulate what varies principle. Let's try again.

The Computer Builder: Encapsulation
For the third attempt, I've encapsulated the computer components in a separate class called, MyComputer. It defines two methods, get_computer and build_computer. The second one instantiates a new computer object and encapsulates the setting of its attributes. In the main program then, all I have to do is instantiate my new class, build the computer, get the finished product, and display the results. Let's run this one. As we can see, it still works. So far we've fixed two problems. We've fixed the problem of too many parameters. Now there are none, that long parameter list is gone, and we've fixed the problem of setting the attributes in the client program. Now we've encapsulated those in the MyComputer class, but there's another problem, ordering. A computer cannot be assembled in any old way. There are definite steps to be followed in sequence. For example, you can't install the video card before you install the mainboard, and if you install the hard drive first it may obstruct other work you need to do. On to attempt number four.

The Computer Builder: Ordering
This time let's try to get the ordering correct. To help with that I've created a new class, MyComputerBuilder, which enhances the previous MyComputer class by adding the steps to build a computer in the correct order. The build_computer method does this by calling methods for each of the steps. Note that there's a separate step for the mainboard assembly, which is then installed into the case. The main program looks pretty much the same as before, so let's run it. The results are the same as last time, so this method works too. Imagine though that I wanted the option of building another, cheaper computer. Well I suppose I could do something like this. I could find mycomputer_builder, make a copy of that, I could paste it back in to the same area, and I could give it a new name. We'll just call it, well let's rename it to cheapone, because that's what we're going to try to do. I'll open up the CheapOne class, rename it, and then start making modifications. So I could simply make cheaper components. For example, maybe I could say that I'll use the onboard graphics instead of a separate graphics card, and maybe I'll use a smaller hard drive, instead of 2 TB, well let's say for the cheap one, 500 GB is enough, and so on, up the way. When I get to the top though, I look at this and I say, wait a minute, I've got these two methods, a get_computer method and a build_computer method, they look suspiciously like the same ones in MyComputerBuilder, and in fact they are exactly the same. Copied code like this is often a real maintenance headache. If that code changes, you'll have to find all those copies and change them all. There must be a better way.

Applying the Builder Pattern
Though we've solved the problems of too many parameters, encapsulating the attributes, and defining the order, we're left with a duplicate code problem. The Builder pattern to the rescue. This is the structure of the Builder pattern in a UML diagram. We can use this to solve the custom computer problem. Top-right is the AbsBuilder. It defines at least one BuildPart method that must be implemented. Below it is a ConcreteBuilder. There can be many of these. They implement the AbsBuilder methods and GetResult method. Top-left is a Director class. It knows how to assemble the product and uses the ConcreteBuilder methods to do the work. It simply calls each builder part method in the correct sequence to get that work done. Finally, the Director calls the GetResult method, which returns the finished product. For the final demo in this module, let's implement the Builder pattern and use it to solve our problem of building a custom computer object. Let's start off by looking at the AbsBuilder metaclass here. I've gone ahead and implemented the get_computer and new_computer methods in the abstract base class since these would otherwise be repeated in each concrete builder in this example. For more complex objects, you should consider making these abstract as well, and implementing them in the concrete builder instead. Each step of building the computer is represented here by an abstract method. The concrete builders must implement each of these. Looking at the revised MyComputerBuilder class, it now inherits from AbsBuilder, and implements the required methods. That's it. The duplicate code is gone, but where? It's in the new Director class. The client, the main program in our example, will pass in the builder it wants to use for the computer to be built. The Director then uses the methods it knows are in the AbsBuilder metaclass, and calls them in the right order to get the computer built. The Director class also implements a get_computer method to retrieve the finished computer object from the builder. There is actually another concrete builder here, the BudgetBoxBuilder. It looks just like the MyComputerBuilder class except for the components. We've encapsulated what varies and nothing else. The main program builds two computers using the two different builders and shows the results. Let's run it. This time we get two computers built, each with its own set of custom components. The Builder pattern has done its job.

Summary
To recap then, what are the problems we've hit and fixed on the way to the Builder pattern? First off, we had an issue with too many parameters. This can happen as a project matures and new features are added without refactoring the code. We fixed that one by exposing the attributes to the client, but this was actually worse than the original problem and would only make maintenance harder. To fix that we created a new class to encapsulate the attributes and their values, but then that just exposed the next problem. There was no order to the assembly. We enforced the assembly order by separating the attributes into methods, and that looked great until we wanted a different product, then we needed to copy a bunch of code. Finally, we applied the Builder pattern to separate the assembly operation from the product's components. So then, what does the Builder pattern do for us? It separates the how from the what. That is, the assembly of a product, or some object, is separated from the components that go into it. This approach encapsulates what varies, the components, while permitting different representations. In the custom computer example, we saw it was easy to create new concrete builders for different product configurations. The client, the main program in the example, creates a Director object. The Director uses a concrete builder and builds the product in the order required. The builder adds parts to the product, and when the Director is finished building the product, the client receives the finished product. The Builder pattern is a great one to have at hand when you hit problems similar to the ones solved in this module. Be sure to have it in your toolkit.
In this example, we need to build computers with different configurations.

* `builder.computer.Computer` – computer that we build.
* `builder.computer_builder.ComputerBuilder` – abstract computer builder.
* `builder.default_computer_builder.DefaultComputerBuilder` – default computer configuration.
* `builder.cheap_computer_builder.CheapComputerBuilder` – cheap computer configuration.
* `builder.director.Director` – director, the class, that knows how to build a computer.

### Factory

Classic factory pattern implementation with factory-loader.

* `factory.loader.load_factory` – loads factory based on factory's name.
* `factory.factories.factory.Factory` – abstract car factory.
* `factory.factories.*` – concrete cars factories implementations.
* `factory.cars.car.Car` – abstract car.
* `factory.cars.*` – concrete cars.

### Abstract factory

There are two car manufacturers: GM and Ford. Each of them can create sport, luxury and economy cars.

 * `abstract_factory.factories.factory.Factory` – abstract factory that can create sport, luxury and sport cars.
 * `abstract_factory.factories.*` – manufacturer-specific factories (derived from base Factory).
 * `abstract_factory.cars.car.*` – abstract `SportCar`, `EconomyCar` and `LuxuryCar`
 * `abstract_factory.cars.ford.*` and `abstract_factory.cars.gm.*` – manufacturer-specific cars.
