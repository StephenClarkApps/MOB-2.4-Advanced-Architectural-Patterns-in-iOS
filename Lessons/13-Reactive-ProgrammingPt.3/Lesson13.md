# Reactive Programming (Part 3 of 3)

<!-- INSTRUCTOR NOTES:
1) For Activity 1:
- solutions are inline below each example/exercise
3) for Activity 2:
- Solution app is not included here...download it from source...(starter app was zipped and sent via slack)
-->

## Minute-by-Minute

| **Elapsed** | **Time**  | **Activity**              |
| ----------- | --------- | ------------------------- |
| 0:00        | 0:05      | Objectives                |
| 0:05        | 0:20      | Initial Exercise          |
| 0:20        | 0:15      | Overview                  |
| 0:35        | 0:20      | In Class Activity I       |
| 0:55        | 0:10      | BREAK                     |
| 1:05        | 0:20      | Overview                  |
| 1:25        | 0:25      | In Class Activity II      |
| TOTAL       | 1:50      |                           |


## Learning Objectives (5 min)

By the end of this lesson, you should be able to...

1. Describe:
- RxSwift **Operator Types,** along with examples of each type
- The benefits derived from combining **RxSwift** with the **MVVM** design pattern
2. Implement basic examples of:
- **UI components** (Table Views) constructed using **RxSwift** (Operators, Subjects) and **MVVM** (viewModels)

## Initial Exercise (20 min)

### As A Class

Let's use this time to (optionally):
1. review or clarify recent lessons
2. review progress on your Course Project
3. prepare for the upcoming Final Exam

## Overview/TT I (20 min)

### Operators expanded

The RxSwift library currently offers most (but not all) of the standard Rx operators, which are typically grouped into types describing general functional behavior.

Below is a partial list of RxSwift operators by type, followed by only a few of the operators which exemplify each type.


<!-- TODO: add short descriptions of each example below each type -->


**Transforming Operators** &mdash; Operators that transform `Next` event elements emitted by an Observable sequence.

- map, scan, flatMap, flatMapLatest

**Combination Operators** &mdash; Operators that combine multiple source Observables into a single Observable.

- startWith, merge, zip, switchLatest

**Filtering and Conditional Operators** &mdash; Operators that selectively emit elements from a source Observable sequence.

- filter, distinctUntilChanged, take, takeUntil, skip

**Mathematical and Aggregate Operators** &mdash; Operators that operate on the entire sequence of items emitted by an Observable.

- toArray, reduce, concat

**Connectable Operators** &mdash; Connectable Observable sequences resembles ordinary Observable sequences, except that they do not begin emitting elements when subscribed to, but instead, only when their connect() method is called. In this way, you can wait for all intended subscribers to subscribe to a connectable Observable sequence __*before*__ it begins emitting elements.

- publish, replay, multicast

**Error Handling Operators** &mdash; Operators that help to recover from error notifications from an Observable.

- catchError, catchErrorJustReturn, retry


> For more RxSwift operators and examples of each, see the Rx.playground in the RxSwift library, as a starting point for your exploration into RxSwift operators.

...and, by the way, you can also __*create your own operators!*__

https://github.com/ReactiveX/RxSwift/blob/master/Documentation/GettingStarted.md#custom-operators

<!-- TODO: end: You can create your own - show link  -->


<!-- TODO: pick a couple and show examples...esp. those that the class was confused by -->
<!-- TODO: retry()? -->


## In Class Activity I (30 min)

### As A Class - Discuss the questions posed below each example

__*Example 1:*__ A Combination Operator &mdash; `combineLatest`

Combines up to 8 source Observable sequences into a single new Observable sequence and will begin emitting from the combined Observable sequence the latest elements of each source Observable sequence &mdash; once all source sequences have emitted at least one element &mdash; and also when any of the source Observable sequences emits a new element.

There is also a variant of combineLatest that takes an Array (or any other collection of Observable sequences):

```Swift
  example("Array.combineLatest") {
      let disposeBag = DisposeBag()

      let stringObservable = Observable.just("❤️")
      let fruitObservable = Observable.from(["🍎", "🍐", "🍊"])
      let animalObservable = Observable.of("🐶", "🐱", "🐭", "🐹")

      Observable.combineLatest([stringObservable, fruitObservable, animalObservable]) {
              "\($0[0]) \($0[1]) \($0[2])"
          }
          .subscribe(onNext: { print($0) })
          .disposed(by: disposeBag)
  }
```

> NOTE: Because the combineLatest variant that takes a collection passes an array of values to the selector function, it requires that all source Observable sequences are of the same type.

**Q:** What will be the output of the `print()` function? Why?

<!-- SOLUTION:
This produces:
--- Array.combineLatest example ---
❤️ 🍎 🐶
❤️ 🍐 🐶
❤️ 🍐 🐱
❤️ 🍊 🐱
❤️ 🍊 🐭
❤️ 🍊 🐹 -->


__*Example 2:*__ A Transforming Operator &mdash; `scan`

Begins with an initial `seed value`, and then applies an `accumulator closure` to each element emitted by an Observable sequence, and returns each intermediate result as a single-element Observable sequence.

```swift
  example("scan") {
      let disposeBag = DisposeBag()

      Observable.of(10, 100, 1000)
          .scan(1) { aggregateValue, newValue in
              aggregateValue + newValue
          }
          .subscribe(onNext: { print($0) })
          .disposed(by: disposeBag)
  }

  /* OUTPUT:
  11
  111
  1111
  */
```

**Q:** In this example, the `observable` calls `scan()` on itself. What is the `seed value`? What will the output be?

```Swift
  let observable = Observable<String>.create { (observer) -> Disposable in
      observer.onNext("D")
      observer.onNext("U")
      observer.onNext("M")
      observer.onNext("M")
      observer.onNext("Y")
      return NopDisposable.instance
  }

  observable.scan("") { (lastValue, currentValue) -> String in
  	// The new value emitted is the LAST value emitted + current value:
      return lastValue + currentValue
      }.subscribeNext { (element) in
          print(element)
      }.disposed(disposeBag)
      }
  }
```

<!-- SOLUTION: This will print:
D
DU
DUM
DUMM
DUMMY -->


**Q:** What is this code doing? What advantage might provide?

```Swift
  myButton.rx_tap.scan(false) { lastState, newValue in
      return !lastState
  }
```

*Sources:* </br>
- Rx.playground in RxSwift library
- http://swiftpearls.com/RxSwift-for-dummies-2-Operators.html





<!-- TODO: Add these operators to exercise?

takeUntil

scan
  - has example in RxSwift
    - needs better example?

debounce?

 -->



<!-- TODO: get exercises for Operators  -->


## Overview/TT II (20 min)

### RxSwift and UI Components (Table Views)

In Wikipedia's definition of [Reactive programming](https://en.wikipedia.org/wiki/Reactive_programming), these 2 overall benefits standout:
- Rx has been proposed as a way to simplify the creation of interactive user interfaces and near-real-time system animation.
- In MVC architecture, Rx can facilitate changes in an underlying __*model*__ that are reflected automatically in an associated __*view.*__

Why is this important in iOS?

&mdash; Because there is an increasingly high demand for smooth UI transitions, pixel-perfect apps, and fewer steps to populate UI widgets with user data.

And Rx lets you create amazing UI behavior without managing the state complexity of variable interactions, and with fewer lines of code.

Two critical ways that Rx helps you handle data changes to the UI properly are by using:
- **Bindings**
- **Reactive Data Sources**

#### Bindings

**Binding** &mdash; A means of keeping `model` and `view` values *synchronized* without you having to write a lot of “glue code.” It allows you to establish a mediated connection between a view and a piece of data, “binding” them such that a change in one is reflected in the other. <sup>1</sup>

Binding is one of key ways that Rx enables building apps in a *declarative* way. It creates a connected relationship between two entities.

Think of those entities as:
- A "producer," which produces the value
- A "consumer," which processes the values from the producer

> Notes:
> - It is required that the `consumer` conforms to the `ObserverType` protocol.
> - A `consumer` cannot return a value.


<!-- TODO: insert diagram -->


##### The `bind(to:)` Function
The primary method for binding an observable to another entity is the `bind(to:)` function.


<!-- TODO: add other uses of bind(to:), see reference ...examples, "background tasks" -->

__*Example 1*__

> When you bind an observable subscription to the `text` property, the property returns a new observer which executes its block parameter when each
> value is emitted. In other words: any time the `text` property receives a new value, it runs the code `label.text = text`

```swift
Observable.combineLatest(firstName.rx.text, lastName.rx.text) { $0 + " " + $1 }
    .map { "Greetings, \($0)" }
    .bind(to: greetingLabel.rx.text)
```

__*Example 2*__

> This also works with `UITableView`s and `UICollectionView`s. Here, you call `bindTo(_:)` to associate the `viewModel` observable with the code that should get executed for each row in the table view.

```swift
viewModel
    .rows
    .bind(to: resultsTableView.rx.items(cellIdentifier: "WikipediaSearchCell",
    cellType: WikipediaSearchCell.self)) { (_, viewModel, cell) in
        cell.title = viewModel.title
        cell.url = viewModel.url
    }
    .disposed(by: disposeBag)
```


<!-- Question re: Data Source - is this an example of doing that with Rx? -->

*Source:* </br>
`Why.md` in Documentation section of RxSwift library.


</br>

<!-- ### data sources

Writing table and collection view data sources is tedious. There is a large number of delegate methods that need to be implemented for the simplest case possible.  -->


### RxSwift and MVVM

When applied appropriately, the MVVM pattern can result in an app with fewer defects that is more easily tested and maintained.

Two of MVVM's primary benefits are:
- Data immutability  &mdash; Guarantees total control over updates triggered by the UI. Guards against issued caused by application *state.*
- Testability is greatly improved &mdash; With ViewModels, much of an app can be easily tested in a "headless" manner (without any views).

In MVVM, the `ViewModel` is responsible for interacting with `Model` data, applying business logic, and preparing (formatting) data for user (View) presentation. The `ViewModel` may also execute database or network interactions.

The new role of the `View Controller` is to simply *bind* the data exposed by the `ViewModel` to the `View`. The `View Controller` also handles user interactions like button taps and gestures, notifying the `ViewModel` of those events.

Ways to implement __*data binding*__ on the `ViewModel` include:
- Delegation
- Key-Value Observing
- Reactive Programming

MVVM works especially well with RxSwift/RxCocoa because these Rx frameworks have built-in functionality that let's you bind Observables to UI components.


<!-- TODO:
- add an example. this seems ok, but Swift code needs updates:
https://stackoverflow.com/questions/46201795/rxswift-rxcocoa-and-uitableview
-->

## In Class Activity II (30 min)

__*Required resources:*__
- Xcode with RxSwift enabled
- the starter app, `MVVMRx_SampleProject-master`

### Part 1 - Individually

__*Exercise 1:*__ Find and examine:

- The `ObserverType protocol` (in RxSwift library)
- Descriptions of `bindTo(_:)` functions (in RxCocoa)

__*Exercise 2:*__ In the starter app, the code for the `albums` scene is complete. But the code for the `tracks` scene needs to be completed in 3 places.

- Search for the 3 `TODO:`s specific to this lesson (there are more than 3 `TODO:`s in the app).
- Using clues from how the `albums` scene was implemented, implement which is missing for each of the 3 `TODO:`s for the `albums` scene

**Q:** For Part 3 of this exercise, what exactly is the nature of the "observable" bound to the code called for each cell/row?

**Q:** In HomeViewModel, note the construction of the 4 PublishSubjects. What benefit(s) does the `error` PublishSubject provide?

### Part 2 - As A Class

**Q:** How does this app implement MVVM?


</br>

*Source for starter app:* </br>
https://medium.com/flawless-app-stories/practical-mvvm-rxswift-a330db6aa693
</br>

## After Class

1. Research:

- `ControlPropertyType`
- `Binder`
- The `retry()` operator
- The `throttle()` operator, compared with the`debounce` operator
- What is an `Action` in Rx
- Converting Data Sources to Rx
- `Driver` (RxCocoa) <sup>2</sup>

## Wrap Up (5 min)

- Continue working on your Course Project
- Complete reading
- Complete challenges

## Additional Resources

1. [Slides](https://docs.google.com/presentation/d/1DbNZp_lyenlqqRh05TB2QaJHStCz4Q9xDX21sjE3JFQ/edit#slide=id.p)
2. [What Are Bindings - from Apple docs](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CocoaBindings/Concepts/WhatAreBindings.html)  <sup>1</sup>
3. [RxDataSources - from RxSwiftCommunity](https://github.com/RxSwiftCommunity/RxDataSources)
4. [Driver - from RxSwift documentation](https://github.com/ReactiveX/RxSwift/blob/master/Documentation/Traits.md#driver)
<sup>2</sup>
5. [A Practical Intro to RxSwift - an article](http://adamborek.com/practical-introduction-rxswift/)
6. [Build an App with RxSwift and MVVM - an article](https://medium.com/textileio/build-your-first-set-app-using-mvvm-and-rxswift-5a37d027950f)

<!-- ...TODO: and add Links to more MVVM at end   -->
