
# Flutter Design Patterns

<p align="center">
  <img src="./.github/flutter_logo.png" height="150px" />
</p>

***

<p align="center">
🎉 Ultra-simplified explanation to design patterns in Flutter! 🎉
</p>
<p align="center">
Design patterns are essential in crafting maintainable, scalable, and flexible code for Flutter apps.<br> Here's an <i>easy-to-understand</i> guide to the patterns used in Flutter.
</p>

***

|[Creational Design Patterns](#creational-design-patterns)|[Structural Design Patterns](#structural-design-patterns)|[Behavioral Design Patterns](#behavioral-design-patterns)|
|:-|:-|:-|
|[Factory](#-factory)|[Adapter](#-adapter)|[Observer](#-observer)|
|[Builder](#-builder)|[Decorator](#-decorator)|[Command](#-command)|
|[Singleton](#-singleton)|[Facade](#-facade)|[State](#-state)|
|||[Strategy](#-strategy)|

## Introduction

Design patterns are reusable solutions to common software problems. They help you organize your code for easier maintenance and scalability. In Flutter, design patterns can streamline app development by providing best practices for structuring code.

> **Design patterns in Flutter** are not just libraries you plug in. They are architectural guidelines that help you write clean, reusable code.

⚠️ **Be Careful**
- Design patterns aren't magic—use them where they fit best, not everywhere.
- Avoid forcing patterns; using them unnecessarily can overcomplicate your code.
- **Flutter**, like other frameworks, benefits from patterns but only in appropriate contexts (e.g., state management, dependency injection).

## Types of Design Patterns

* [Creational](#creational-design-patterns)
* [Structural](#structural-design-patterns)
* [Behavioral](#behavioral-design-patterns)

---

## Creational Design Patterns

In Flutter, creational patterns help manage object creation in a clean and scalable way.

### 🏭 Factory
In Flutter, factories are used when you want to instantiate objects dynamically. Consider using a factory method when creating widgets that depend on runtime data.

**Example:**
In Flutter, we use `Factory` to build widgets based on conditions. This avoids hardcoding widget logic in the UI layer.

```dart
class ButtonFactory {
  static Widget createButton(ButtonType type) {
    switch (type) {
      case ButtonType.primary:
        return ElevatedButton(onPressed: () {}, child: Text('Primary'));
      case ButtonType.secondary:
        return OutlinedButton(onPressed: () {}, child: Text('Secondary'));
      default:
        return TextButton(onPressed: () {}, child: Text('Default'));
    }
  }
}
```

**When to use?**  
When you need dynamic widget generation based on runtime logic.

### 🛠 Builder
The builder pattern helps you construct complex widgets or objects step by step.

**Example:**
Use `Builder` in Flutter to decouple widget creation from the main app logic. A common use is within `StreamBuilder` or `FutureBuilder`.

```dart
FutureBuilder<String>(
  future: fetchData(),
  builder: (context, snapshot) {
    if (snapshot.connectionState == ConnectionState.waiting) {
      return CircularProgressIndicator();
    } else if (snapshot.hasError) {
      return Text('Error: ${snapshot.error}');
    } else {
      return Text('Data: ${snapshot.data}');
    }
  },
);
```

**When to use?**  
When you want to separate UI logic from data-fetching or other processes.

### 💍 Singleton
A **Singleton** ensures that there’s only one instance of a particular class throughout your app.

**Example:**
Flutter's `GetIt` package is a popular choice for dependency injection. It uses a singleton pattern to provide services throughout the app.

```dart
final getIt = GetIt.instance;

class ApiService {
  ApiService._privateConstructor();
  static final ApiService _instance = ApiService._privateConstructor();

  factory ApiService() {
    return _instance;
  }

  void fetchData() {
    // fetch data logic
  }
}
```

**When to use?**  
When you want to ensure a single instance of a service or repository in your app.

---

## Structural Design Patterns

In Flutter, structural patterns manage relationships between widgets and how they interact.

### 🔌 Adapter
The **Adapter** pattern makes incompatible interfaces work together.

**Example:**
When you need to adapt data to fit a widget, such as adapting an API response into a widget-friendly format.

```dart
class ApiAdapter {
  static List<Widget> adaptData(List<ApiData> data) {
    return data.map((apiData) => ListTile(
      title: Text(apiData.title),
      subtitle: Text(apiData.subtitle),
    )).toList();
  }
}
```

**When to use?**  
When adapting external data (e.g., JSON or REST API responses) for Flutter widgets.

### ☕ Decorator
The **Decorator** pattern allows behavior to be dynamically added to a widget.

**Example:**
In Flutter, you can add decorators to widgets using widgets like `Container` or `Padding` to modify appearance.

```dart
Container(
  decoration: BoxDecoration(
    border: Border.all(color: Colors.blue, width: 2),
  ),
  child: Padding(
    padding: EdgeInsets.all(8.0),
    child: Text('Hello World'),
  ),
);
```

**When to use?**  
When you want to extend or modify the behavior of a widget dynamically without modifying its structure.

### 🖼 Facade
The **Facade** pattern provides a simplified interface to a complex system.

**Example:**
Flutter's `Bloc` pattern can be thought of as a Facade for managing state. The `Bloc` acts as a single interface that hides the complexity of streams and business logic.

```dart
class CounterBloc extends Bloc<CounterEvent, int> {
  CounterBloc() : super(0);

  @override
  Stream<int> mapEventToState(CounterEvent event) async* {
    if (event is IncrementEvent) {
      yield state + 1;
    }
  }
}
```

**When to use?**  
When you want to provide a unified API for interacting with a complex state management or service.

---

## Behavioral Design Patterns

Behavioral patterns in Flutter focus on how widgets communicate and share responsibility.

### 🔗 Observer
In Flutter, **Observer** pattern is essential for reacting to changes in state.

**Example:**
Flutter's `ValueNotifier` and `ValueListenableBuilder` follow the observer pattern.

```dart
ValueNotifier<int> counter = ValueNotifier<int>(0);

ValueListenableBuilder<int>(
  valueListenable: counter,
  builder: (context, value, child) {
    return Text('Counter: $value');
  },
);
```

**When to use?**  
When you want to update the UI in response to changes in a specific value or state.

### 🏃‍♂️ Command
In the latest versions of Flutter Bloc, the pattern is used to handle UI events through the Bloc by dispatching events and emitting states. Here's how it looks using Bloc and Cubit.

Example: A button that sends an event to change the state:

```dart
// Define the event and state classes
class CounterEvent {}

class IncrementEvent extends CounterEvent {}

class CounterState {
  final int counter;
  CounterState(this.counter);
}

// Define the bloc that handles the events and emits new states
class CounterBloc extends Bloc<CounterEvent, CounterState> {
  CounterBloc() : super(CounterState(0)) {
    on<IncrementEvent>((event, emit) {
      emit(CounterState(state.counter + 1));
    });
  }
}
```

Now, you can wire up the Bloc in the UI with BlocBuilder:

```dart
class CounterPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocProvider(
      create: (_) => CounterBloc(),
      child: Scaffold(
        appBar: AppBar(title: Text('Counter Example')),
        body: Center(
          child: BlocBuilder<CounterBloc, CounterState>(
            builder: (context, state) {
              return Text('Counter: ${state.counter}');
            },
          ),
        ),
        floatingActionButton: FloatingActionButton(
          onPressed: () {
            context.read<CounterBloc>().add(IncrementEvent());
          },
          child: Icon(Icons.add),
        ),
      ),
    );
  }
}

```

**When to use?**  
When separating the action logic (command) from the UI elements that trigger them. Bloc helps keep business logic cleanly separated from UI code.

### 💢 State
The **State** pattern is vital in Flutter for managing widget state.

Here’s a comparison between using StatefulWidget and Bloc to manage state in Flutter.

**StatefulWidget Example:**
Flutter's stateful widgets inherently use the state pattern to manage UI changes based on state.

```dart
class CounterStatefulWidget extends StatefulWidget {
  @override
  _CounterStatefulWidgetState createState() => _CounterStatefulWidgetState();
}

class _CounterStatefulWidgetState extends State<CounterStatefulWidget> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('StatefulWidget Counter')),
      body: Center(
        child: Text('Counter: $_counter'),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        child: Icon(Icons.add),
      ),
    );
  }
}
```

**Bloc Example:**
In contrast to StatefulWidget, using Bloc provides a more scalable solution, especially for apps with complex state management needs.

```dart
// Define the state and event classes (similar to previous example)
class CounterState {
  final int counter;
  CounterState(this.counter);
}

class IncrementEvent extends CounterEvent {}

// The CounterBloc as defined earlier
class CounterBloc extends Bloc<CounterEvent, CounterState> {
  CounterBloc() : super(CounterState(0)) {
    on<IncrementEvent>((event, emit) {
      emit(CounterState(state.counter + 1));
    });
  }
}

// UI using Bloc
class CounterBlocWidget extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return BlocProvider(
      create: (_) => CounterBloc(),
      child: Scaffold(
        appBar: AppBar(title: Text('Bloc Counter')),
        body: Center(
          child: BlocBuilder<CounterBloc, CounterState>(
            builder: (context, state) {
              return Text('Counter: ${state.counter}');
            },
          ),
        ),
        floatingActionButton: FloatingActionButton(
          onPressed: () {
            context.read<CounterBloc>().add(IncrementEvent());
          },
          child: Icon(Icons.add),
        ),
      ),
    );
  }
}

```

**When to use?**  
StatefulWidget: When state is simple and only affects a small, isolated part of the app.

Bloc: For more complex state management or when you need to decouple business logic from UI.


### 💡 Strategy
The **Strategy** pattern lets you select different algorithms at runtime based on the conditions.

**Example:**
In Flutter, we can use the Strategy pattern to change the sorting logic for a list dynamically based on the user's choice.

```dart
abstract class SortStrategy {
  List<int> sort(List<int> data);
}

class BubbleSort implements SortStrategy {
  @override
  List<int> sort(List<int> data) {
    // Bubble sort logic here
    return data;
  }
}

class QuickSort implements SortStrategy {
  @override
  List<int> sort(List<int> data) {
    // Quick sort logic here
    return data;
  }
}

class Sorter {
  SortStrategy strategy;

  Sorter(this.strategy);

  void changeStrategy(SortStrategy strategy) {
    this.strategy = strategy;
  }

  List<int> sort(List<int> data) {
    return strategy.sort(data);
  }
}
```

**When to use?**  
When you need to switch between multiple algorithms dynamically in Flutter, such as different data sorting or filtering strategies.

---

By leveraging these design patterns in Flutter, you can craft well-structured, scalable, and maintainable apps. Keep these patterns in mind as you architect your next project!

## 🚦 Wrap Up Folks
And that’s the summary of design patterns in Flutter. By understanding when and how to use these patterns, you can make your apps more maintainable and scalable. Stay tuned for more Flutter-specific patterns and best practices!
