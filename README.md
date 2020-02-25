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
