# Structural Patterns Pt.1

<!-- INSTRUCTOR NOTES:
1) For the quiz in the Initial Exercise:
- the URL is https://docs.google.com/document/d/1kz7OpSDh_S2d7KCbUTeYEOR1M9r6-zgkxDhUGdvaDiY/edit
2) For Activity 1:
- Solution is embedded below Addtional Resources
3) for Activity 2:
- Solution is embedded below Addtional Resources
-->

## Minute-by-Minute

| **Elapsed** | **Time**  | **Activity**              |
| ----------- | --------- | ------------------------- |
| 0:00        | 0:05      | Objectives                |
| 0:05       | 0:20      | Initial Exercise             |
| 0:25       | 0:15      | Overview  I                |
| 0:40       | 0:25      | In Class Activity I       |
| 1:05       | 0:10      | BREAK                     |
| 1:15         | 0:15      | Overview  II                |
| 1:30       | 0:25      | In Class Activity II      |
|1:55       | 0:05    | Wrap Up                     |
| TOTAL       | 2:00    |                          |


## Learning Objectives (5 min)

By the end of this lesson, you should be able to...

1. Explain why **Structural design patterns** are important in software development
2. Describe:
- the **Adapter** and **Decorator** patterns
- the software construction problem each is intended to solve
- potential use cases for each (when to use them)
3. Assess:
- the suitability of a given design pattern to solve a given problem
- the trade offs (pros/cons) inherent in each
4. Implement basic examples of both patterns explored in this class


## Initial Exercise (20 min)

### Part 1 - Individually

**A Quiz** - To test and reinforce your knowledge gleaned from the previous class...

<!-- Quiz location:
https://docs.google.com/document/d/1kz7OpSDh_S2d7KCbUTeYEOR1M9r6-zgkxDhUGdvaDiY/edit
-->

<!-- Quiz answers:

Question 1: D, C, A, B

Question 2: God Object
https://en.wikipedia.org/wiki/God_object

Question 2:  Weak references
https://en.wikipedia.org/wiki/Lapsed_listener_problem

-->

### Part 2 - In Pairs

Grade each other's quizzes, sharing answers, insights, etc.

### Part 3 - As A Class

- Review progress of the media player app for which After Class work was assigned in the last 2 class sessions
- One student to volunteer to present their work (if time permits)


## Overview/TT I (15 min)

### Structural Patterns
Structural design patterns ease design by identifying a simple way to realize relationships between entities.

Examples of Structural patterns include:
- **Adapter**
- Aggregate
- Bridge
- Composite
- **Decorator**  
- Extensibility
- **Facade**
- Flyweight  
- Marker
- **Proxy**

*From: wikipedia*

*For lessons 5 and 6 in this course, we will cover the patterns highlighted in bold text above…*

**Structural Pattern Notes:**
1. MVC is a high-level example of a Structural pattern.
2. Many of the structural patterns have similar implementations but different intents. Best practice: Ensure you select the Structural pattern which best suits your intended purpose.
2. Adapter and Decorator are often each referred to as “wrapper” patterns because they wrap an object and provide a new interface around it. Here are some key differences between the two patterns:
- **Adapter** - Can be used when the wrapper must respect a particular interface and must support polymorphic behavior.
- **Decorator** makes it possible to add or alter behavior of an interface at run-time.
- **Facade** is used when an easier or simpler interface to an underlying object is desired. *(We will learn more about Facade in next lesson.)*

![example](assets/wikipedia_table.png)

### The Adapter Pattern&nbsp;&nbsp;&nbsp;&nbsp;:electric_plug:

Adapter lets classes work together that couldn’t otherwise because of incompatible interfaces.

It is used to provide a link between two elements that otherwise would not fit with each other by wrapping the "adaptee" with a class that supports the interface required by a client.

It decouples the client from the class of the targeted object.

#### Implementation Notes

The key idea in this pattern is to work through a *separate adapter* that adapts the interface of an (already existing) class *without changing it.*

Adapter is typically composed of four main *actors*:

1. **Adaptee** - Defines an existing interface that needs adapting.
2. **Adapter** - Adapts/converts the (incompatible) interface of a class (Adaptee) into another interface (Target) that clients require.
3. **Target** - Defines the domain-specific interface that the Client uses.  
4. **Client** - Collaborates with objects conforming to the Target interface.

The __*Adapter object*__ does the actual work - it "wraps" the original object and adds new requirements specified by the Target.

The pattern is implemented correctly when the adapter allows a component to be integrated into the application without requiring modification of the existing application modules or the component.

**Best Practices in Swift**
- __*Use Protocols*__ -  The *design* of iOS protocols does not perfectly match the description of the Adapter pattern, though it achieves the *goal* of the pattern: allowing classes with otherwise incompatible interfaces to work together.
- __*Use Class Extensions*__ - The most elegant way to implement the Adapter pattern is with a Swift extension. Extensions allow you to add functionality to classes that you are unable to modify. This functionality includes adding conformance to a protocol, which is perfectly suited to implementing the Adapter pattern. <sup>1</sup>


#### Simple example

**The Problem** </br>
In the following example: A Hobbit object can only `walk()`, but a Guardian object (__*code not listed*__) must `march()`. What if a Hobbit is promoted to guard the citadel?

Then the Hobbit must somehow be able to `march()` just as a Guardian object does.

**The Solution** </br>
Use the Adapter pattern to adapt the Hobbit class to conform to the behaviors required of a Guardian.

```swift
import UIKit

/// Adaptee
class Hobbit {
    func walk() {
        print("Hobbit is walking a few steps")
    }
}

/// Target
protocol GuardianOfTheCitadel {
    func march()
}

/// Adapter
extension Hobbit: GuardianOfTheCitadel {
    func march() {
        walk()
        print("Hobbit realizes he is a few steps behind the formation")
        walk()
    }
}

let hobbit = Hobbit()
hobbit.march()

/* This will print:
 Hobbit is walking a few steps
 Hobbit realizes he is a few steps behind the formation
 Hobbit is walking a few steps
 */
```
*Example above adapted from this article:*
</br>
 https://medium.com/jeremy-codes/adapter-pattern-in-swift-7332e178f112


#### Problems Addressed

The Adapter pattern solves problems like:
- *How can a class be reused that does not have an interface that a client requires?*
- *How can classes that have incompatible interfaces work together?*
- *How can an alternative interface be provided for a class?*

Problems that Adapter solves arise when an existing system needs to integrate a new component that has a similar function but that doesn’t present a common interface and that cannot be modified.

Another key use of Adapter arises *when you do not have access to the original source code.* Incompatible code can be introduced into a project when:
- a __*third-party component*__ is used
- or when you depend on the code produced by another development team working on a related project.

#### Benefits

Adapter allows you to integrate components for which you cannot modify the source code into your application. This is a common problem when you use a third-party framework or when you are consuming the output from another project.


#### Pitfalls

Trying to extend the pattern to force integration of a component that does not provide the functionality intended by the API for which it is being adapted.

#### When to use

Classes, modules, and functions can’t always be modified, especially if they’re from a third-party library. Sometimes you have to adapt instead!

Use Adapter...

- when you need to integrate a component that provides similar functionality to other components in the application but that uses an incompatible API.
- during the implementation of third-party classes when there is a mismatch in the interface and it does not tally with the rest of application code.
- when you need to employ various existing subclasses and they are not complying with a specific functionality.

Adapter is also very often used in systems based on some legacy code.

__*Do not use__* the Adapter pattern when you are able to modify the source code of the component that you want to integrate or when it is possible to migrate the data provided by the component directly into your application.


## In Class Activity I (25 min)

The following playground code currently is intended to utilize the Adapter pattern to link two incompatible components - an `AudioPlayer` and a `VideoPlayer` class - into an *adapter* class (`MyPlayer` class) which integrates the functionality of each component.

However, none of the classes so far implement a `pause()` function, which is highly desired.

The current code is also incomplete.

**TODO:**
1. Complete the current code so that it it runs and prints:
```Swift
Now Playing:  Titanium.aac
Now Playing:  Cat_riding_a_roomba.mp4
```
2. Using the Adapter pattern, add a `pause()` function.

**Playground Code**

```Swift
import UIKit

// Target protocol 1
protocol Player {
    func play(audioType: String, fileName: String)
}

// Target protocol 2

    //TODO: Implement a Pause protocol with a pause() function that accepts 1 parameter: A String called "fileName"


// Adaptee 1
class AudioPlayer {
    func playAudio(fileName: String) {
        print("Now Playing: ", fileName)
    }
}

// Adaptee 2
class VideoPlayer {
    func playVideo(fileName: String) {
        print("Now Playing: ", fileName)
    }
}

// Adapter (class)
class MyPlayer: Player {

    //TODO: create required player variables

    func play(audioType: String, fileName: String) {
        if (audioType == ".mp4"){
            videoPlayer.playVideo(fileName: fileName);
        }else if(audioType == ".aac"){
            audioPlayer.playAudio(fileName: fileName);
        }
    }
}

// Adapter (class extension)

    //TODO: Implement a class extension which adds Pause functionality to MyPlayer


// Usage
let myPlayer = MyPlayer()
myPlayer.play(audioType: ".aac", fileName: "Titanium.aac")
myPlayer.play(audioType: ".mp4", fileName: "Cat_riding_a_roomba.mp4")
myPlayer.pause(fileName: "Cat_riding_a_roomba.mp4")


/* This should print:
 Now Playing:  Titanium.aac
 Now Playing:  Cat_riding_a_roomba.mp4
 Cat_riding_a_roomba.mp4  is now paused...
 */
```

*Adapted from this Java code:*
  http://hackjutsu.com/2015/11/07/Adapter%20Pattern/


<!-- SOLUTION FOR ACTIVITY 1 -- is below Additional Resources... -->


## Overview/TT II  (15 min)

#### The Decorator Pattern &nbsp;&nbsp;&nbsp;:art:

The Decorator pattern is used to extend or alter the functionality of objects __*at run-time*__ by wrapping them in an object of a decorator class. This provides *a flexible alternative to using inheritance* to modify behavior.

It allows behavior to be added to an individual object - __*dynamically,*__ at run-time - without affecting the behavior of other objects from the same class or in cases where subclassing would result in an exponential rise of new classes.


#### Implementation Notes

Decorator and Adapter are two key patterns related to polymorphism that you will see often in Swift. They are implemented using the language keywords `protocol` and `extension` respectively.

The __*primary example*__ of the Decorator pattern __*in Swift*__ is when you create an `extension`.

Decorator consists of base components that are *extended* at *run-time* and ‘decorated’ with decorator classes.

Decorator is typically comprised of these components:

1. **(Abstract) Core Component** — The (abstract) base class or protocol that a base object will subclass or implement.
2. **Concrete Core Component** - Implementation of the Core Component.
3. **Decorator** — The Decorator can extend the Core Component using two forms:
- As an __*Abstract Decorator*__ - A protocol which extends the Core Component protocol
- As a __*Concrete Decorator*__ - An implementation (or subclass) of the Core Component.

The pattern has been implemented correctly when you can select some of the objects created from a class to be modified without affecting all of them and without requiring changes to the original class.

**Key Points**
- Concrete Decorators have the capability of wrapping around Components or other Decorators and building structures.
- Decorator designed so that multiple decorators can be stacked on top of each other, each time adding a new functionality to the overridden method(s).
- Decorators and the original class object share a common set of features.
- To prevent subclassing the Core Component class can be declared `final`.

#### Problems Addressed
What problems can the Decorator design pattern solve?

Responsibilities should be added to (and removed from) an object dynamically at run-time.
- A flexible alternative to subclassing for extending functionality should be provided.
- When using subclassing, different subclasses extend a class in different ways. But an extension is bound to the class at compile-time and can't be changed at run-time.

Decorator is also a solution to the **Exploding Class Hierarchy** problem: An exploding class hierarchy occurs when the number of classes needed to add a new functionality to a given class hierarchy grows exponentially.

*Source: wikipedia*

#### Benefits
The decorator pattern is an alternative to subclassing. Subclassing adds behavior at compile time, and the change affects all instances of the original class; decorating can provide new behavior at run-time for selected objects.

The decorator pattern is often useful for adhering to the __*Single Responsibility Principle,*__ as it allows functionality to be divided between classes with unique areas of concern.

The changes in behavior defined with the decorator pattern can be combined to create complex effects without needing to create large numbers of subclasses.

**TIP** If you do not have control over the class definition (source code) of an object, you can apply the decorator pattern.

#### Pitfalls
The main pitfall is implementing the pattern so that it affects all of the objects created from a given class rather than allowing changes to be applied selectively. A less common pitfall is implementing the pattern so that it has hidden side effects that are not related to the original purpose of the objects being modified.

#### Related Patterns

Decorator is structurally nearly identical to the Chain-of-Responsibility (CoR) pattern.

The difference:
- in CoR, exactly one of the classes handles the request
- for Decorator, all classes handle the request
Decorator in iOS

Decorator is a pattern for object composition, which is something that you are encouraged to do in your own code.

Cocoa Touch, however, provides some classes and mechanisms of its own that are based on the pattern.

Cocoa Touch uses the Decorator pattern in the implementation of several of its classes, including `NSAttributedString`, `NSScrollView`, and `UIDatePicke`r. The latter two classes are examples of compound views, which group together simple objects of other view classes and coordinate their interaction.

In Swift there are two very common implementations of this pattern: **Extensions** and **Delegation.**

*Source: Apple*

#### When to use
Use Decorator when you need to change the behavior of objects without changing the class they are created from or the components that use them.

Another use case: when you must use several existing classes or structs which lack some desired functionality and that cannot be subclassed. Building a new target interface by wrapping these classes with Decorator can be a very clean solution.

Do not use the Decorator pattern when you are able to change the class that creates the objects you want to modify. It is usually simpler and easier to modify the class directly.

#### Simple Example

**The Problem**
In this example, the SimpleCoffee object is constrained to only a single price and a single ingredient. If additional ingredients are desired, the cost of a coffee-based item must increase to reflect the cost of additional or more expensive ingredients added.

**The Solution**
Instead of subclassing SimpleCoffee for every type of coffee or ingredient desired, the Decorator pattern was used to extend the original object with new desired behaviors.

**Playground Code**

```Swift
import UIKit

// Abstract Core Component
protocol Coffee {
    func getCost() -> Double
    func getIngredients() -> String
}

// Concrete Core Component
class SimpleCoffee: Coffee {
    func getCost() -> Double {
        return 1.0
    }

    func getIngredients() -> String {
        return "Coffee"
    }
}

// Decorator (Base) class
class CoffeeDecorator: Coffee {
    private let decoratedCoffee: Coffee
    fileprivate let ingredientSeparator: String = ", "

    required init(decoratedCoffee: Coffee) {
        self.decoratedCoffee = decoratedCoffee
    }

    func getCost() -> Double {
        return decoratedCoffee.getCost()
    }

    func getIngredients() -> String {
        return decoratedCoffee.getIngredients()
    }
}

// Decorator class
final class Milk: CoffeeDecorator {
    required init(decoratedCoffee: Coffee) {
        super.init(decoratedCoffee: decoratedCoffee)
    }

    override func getCost() -> Double {
        return super.getCost() + 0.5
    }

    override func getIngredients() -> String {
        return super.getIngredients() + ingredientSeparator + "Milk"
    }
}

// Decorator class
final class WhipCoffee: CoffeeDecorator {
    required init(decoratedCoffee: Coffee) {
        super.init(decoratedCoffee: decoratedCoffee)
    }

    override func getCost() -> Double {
        return super.getCost() + 0.7
    }

    override func getIngredients() -> String {
        return super.getIngredients() + ingredientSeparator + "Whip"
    }
}

var someCoffee: Coffee = SimpleCoffee()
print("Cost : \(someCoffee.getCost()); Ingredients: \(someCoffee.getIngredients())")
someCoffee = Milk(decoratedCoffee: someCoffee)
print("Cost : \(someCoffee.getCost()); Ingredients: \(someCoffee.getIngredients())")
someCoffee = WhipCoffee(decoratedCoffee: someCoffee)
print("Cost : \(someCoffee.getCost()); Ingredients: \(someCoffee.getIngredients())")

/* Will print:
 Cost : 1.0; Ingredients: Coffee
 Cost : 1.5; Ingredients: Coffee, Milk
 Cost : 2.2; Ingredients: Coffee, Milk, Whip
 */
 ```

*From this series of Swift design pattern articles:*
 https://github.com/ochococo/Design-Patterns-In-Swift
 <!-- under GNU License -->


## In Class Activity II  (25 min)

When completed, the playground code below will aggregate pizza prices based on type of pizza and toppings selected.

**TODO:** Complete the  code so that it it runs and its output matches the 2 scenarios listed in comments below the working code.
1. Implement the toppings decorator classes:</br>
&nbsp;&nbsp;&nbsp; - **Extra Cheese** - costs 1.0 extra</br>
&nbsp;&nbsp;&nbsp; - **Mushrooms** - at 1.49</br>
&nbsp;&nbsp;&nbsp; - **Jalapeno Peppers** - at 1.19</br>
2. Implement the **Gourmet** pizza type. Its price is 7.49
3. Ensure the code works and outputs correctly *for both* the *Plain Margherita* and the *Plain Gourmet* client code scenarios.

**Playground Code**
```Swift
import UIKit

// Abstract Core Component
protocol PizzaBase {
    func getPrice() -> Double
}

// Concrete Core Component
class PlainPizza: PizzaBase {

    var myPrice: Double = 1.0

    func getPrice() -> Double {
        return self.myPrice
    }
}

// Concrete Core Component
class Margherita: PizzaBase {

    var price: Double = 6.99

    func getPrice() -> Double {
        return self.price
    }
}

// Concrete Core Component
class Gourmet: PizzaBase {

    //TODO: Implement the Gourmet pizza; price is 7.49
}

// Decorator (base) class
class ToppingsDecorator: PizzaBase {

    private let pizza: PizzaBase

    required init(pizzaToDecorate: PizzaBase) {
        self.pizza = pizzaToDecorate
    }

    func getPrice() -> Double {
        return pizza.getPrice()
    }
}

// Decorator class (extended)
class ExtraCheeseTopping: ToppingsDecorator {

    //TODO: Implement Extra Cheese -- add 1.0 to current price
}

// Decorator class (extended)
class MushroomTopping: ToppingsDecorator {

    //TODO: Implement adding mushrooms -- add 1.49 to current price

}

// Decorator class (extended)
class JalapenoTopping: ToppingsDecorator {

    //TODO: Implement JalapenoToppingk, add an extra 1.19 for peppers

}

/// Client-code for Margherita
let pizza: PizzaBase = Margherita()
print("Plain Margherita: ", pizza.getPrice())

/// Client-code for Gourmet
//let pizza: PizzaBase = Gourmet()
//print("Plain Gourmet: ", pizza.getPrice())

let moreCheese: ExtraCheeseTopping = ExtraCheeseTopping(pizzaToDecorate: pizza)
print("moreCheese: ", moreCheese.getPrice())

let evenMoreCheese: ExtraCheeseTopping = ExtraCheeseTopping(pizzaToDecorate: moreCheese)
print("evenMoreCheese: ", evenMoreCheese.getPrice())

let mushrooms: MushroomTopping = MushroomTopping(pizzaToDecorate: evenMoreCheese)
print("mushrooms: ", mushrooms.getPrice())

let withPeppers: JalapenoTopping = JalapenoTopping(pizzaToDecorate: mushrooms)
print("withPeppers: ", withPeppers.getPrice())

/* OUTPUT:

 1) For Client-code for Margherita, should print:

 Plain Margherita:  6.99
 moreCheese:  7.99
 evenMoreCheese:  8.99
 mushrooms:  10.48
 withPeppers:  11.67

 1) For Client-code for Gourmet, should print:

 Plain Gourmet:  7.49
 moreCheese:  8.49
 evenMoreCheese:  9.49
 mushrooms:  10.98
 withPeppers:  12.17

 /*
```
*Adapted from this Java example:*

https://stackoverflow.com/questions/2707401/understand-the-decorator-pattern-with-a-real-world-example

<!-- SOLUTION FOR ACTIVITY 2 -- is below Additional Resources... -->


## After Class

1. Look up these other Structural patterns:

- Aggregate
- Extensibility
- Flyweight
- Marker

2. Research the following concepts:

- The pros and cons of `Class Adapter` vs. `Object Adapter`
- The `Single Responsibility Principle`
- The principle of `Composition Over Inheritance`


3. **TODO:** Extend the Media Player app you created and improved in previous classes by implementing the following using either the Adapter or the Decorator pattern:

- Add functionality so that the user can skip to the `Next` or `Previous` video clip

__*Note*__ *- This will require that you have a collection of at several video clips.*


## Wrap Up (5 min)

- Continue working on your current tutorial
- Complete reading
- Complete challenges

## Additional Resources

1. [Slides](https://docs.google.com/presentation/d/1xyvW8EVQLPlqaTkpTg9yhLxHBB_VahGp3hJxdvldeTA/edit?usp=sharing)<br>
2. [Adapter pattern - wikipedia](https://en.wikipedia.org/wiki/Adapter_pattern)
3. [Decorator pattern - wikipedia](https://en.wikipedia.org/wiki/Decorator_pattern)
4. [CocoaDesignPatterns - Apple docs](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CocoaFundamentals/CocoaDesignPatterns/CocoaDesignPatterns.html)
5. [Swift adapter design pattern - an article](https://theswiftdev.com/2018/07/30/swift-adapter-design-pattern/)
6. [Pro Design Patterns in Swift - a book by Adam Freeman](https://www.amazon.com/Design-Patterns-Swift-Adam-Freeman/dp/148420395X) <sup>1</sup>
7. [Single responsibility principle - wikipedia](https://en.wikipedia.org/wiki/Single_responsibility_principle)
8. [Open–closed principle - wikipedia](https://en.wikipedia.org/wiki/Open–closed_principle)
9. [Combinatorial explosion - wikipedia](https://en.wikipedia.org/wiki/Combinatorial_explosion)
10. [Composition over inheritance - wikipedia](https://en.wikipedia.org/wiki/Composition_over_inheritance)
11. [Difference between object adapter pattern and class adapter pattern - stackoverflow](https://stackoverflow.com/questions/9978477/difference-between-object-adapter-pattern-and-class-adapter-pattern)


<!-- SOLUTION FOR ACTIVITY 1:

import UIKit

// Target protocol 1
protocol Player {
    func play(audioType: String, fileName: String)
}

// Target protocol 2
protocol Pause {
    func pause(fileName: String)
}

// Adaptee 1
class AudioPlayer {
    func playAudio(fileName: String) {
        print("Now Playing: ", fileName)
    }
}

// Adaptee 2
class VideoPlayer {
    func playVideo(fileName: String) {
        print("Now Playing: ", fileName)
    }
}

// Adapter (class)
class MyPlayer: Player {

    let videoPlayer = VideoPlayer()
    let audioPlayer = AudioPlayer()

    func play(audioType: String, fileName: String) {
        if (audioType == ".mp4"){
            videoPlayer.playVideo(fileName: fileName);
        }else if(audioType == ".aac"){
            audioPlayer.playAudio(fileName: fileName);
        }
    }
}

// Adapter (class extension)
extension MyPlayer: Pause {
    func pause(fileName: String) {
        print(fileName, " is now paused...")
    }
}

// Usage
let myPlayer = MyPlayer()
myPlayer.play(audioType: ".aac", fileName: "Titanium.aac")
myPlayer.play(audioType: ".mp4", fileName: "Cat_riding_a_roomba.mp4")


myPlayer.pause(fileName: "Cat_riding_a_roomba.mp4")
-->



<!-- SOLUTION FOR ACTIVITY 2:

import UIKit

// Abstract Core Component
protocol PizzaBase {
    func getPrice() -> Double
}

// Concrete Core Component
class PlainPizza: PizzaBase {

    var myPrice: Double = 1.0

    func getPrice() -> Double {
        return self.myPrice
    }
}

// Concrete Core Component
class Margherita: PizzaBase {

    var price: Double = 6.99

    func getPrice() -> Double {
        return self.price
    }
}

// Concrete Core Component
class Gourmet: PizzaBase {

    var price: Double = 7.49

    func getPrice() -> Double {
        return self.price
    }
}

// Decorator (base) class
class ToppingsDecorator: PizzaBase {

    private let pizza: PizzaBase

    required init(pizzaToDecorate: PizzaBase) {
        self.pizza = pizzaToDecorate
    }

    func getPrice() -> Double {
        return pizza.getPrice()
    }
}

// Decorator class (extended)
class ExtraCheeseTopping: ToppingsDecorator {

    override func getPrice() -> Double {
        return super.getPrice() + 1.0
    }
}

// Decorator class (extended)
class MushroomTopping: ToppingsDecorator {

    override func getPrice() -> Double {
        return super.getPrice() + 1.49
    }
}

// Decorator class (extended)
class JalapenoTopping: ToppingsDecorator {

    override func getPrice() -> Double {
        return super.getPrice() + 1.19
    }
}

/// Client-code for Margherita
//let pizza: PizzaBase = Margherita()
//print("Plain Margherita: ", pizza.getPrice())

/// Client-code for Gourmet
let pizza: PizzaBase = Gourmet()
print("Plain Gourmet: ", pizza.getPrice())

let moreCheese: ExtraCheeseTopping = ExtraCheeseTopping(pizzaToDecorate: pizza)
print("moreCheese: ", moreCheese.getPrice())

let evenMoreCheese: ExtraCheeseTopping = ExtraCheeseTopping(pizzaToDecorate: moreCheese)
print("evenMoreCheese: ", evenMoreCheese.getPrice())

let mushrooms: MushroomTopping = MushroomTopping(pizzaToDecorate: evenMoreCheese)
print("mushrooms: ", mushrooms.getPrice())

let withPeppers: JalapenoTopping = JalapenoTopping(pizzaToDecorate: mushrooms)
print("withPeppers: ", withPeppers.getPrice())


/* OUTPUT:

 1) For Client-code for Margherita, should print:

 Plain Margherita:  6.99
 moreCheese:  7.99
 evenMoreCheese:  8.99
 mushrooms:  10.48
 withPeppers:  11.67

 1) For Client-code for Gourmet, should print:

 Plain Gourmet:  7.49
 moreCheese:  8.49
 evenMoreCheese:  9.49
 mushrooms:  10.98
 withPeppers:  12.17

/*

-->
