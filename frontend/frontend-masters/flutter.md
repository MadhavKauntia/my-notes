# Cross-Platform Mobile Apps with Flutter

## Dart

- Every Dart app has a `main` function
- Full OOP language with type inference
- Null-safety is available as an optional feature in Dart 2.12
- When compiling to Web, remember that the engine is the JavaScript VM

```dart
playingWithLoops() {
    for (var i = 0; i < 10; i++) {}
    for (var i in [1, 2, 3]) {
        print(i);
    }
    while (true) {}
}

playingWithTypes() {
    // Core Data Types
    String stringValue;
    int integerValue;
    double doubleValue;
    bool booleanValue; // values are true and false
    dynamic anything;
    num aNumber;

    // String Literals, double quotes or single quotes
    print("Hello World");
    // Multi-line literal strings
    var multiline = """
    sdfsdf
    """;

    var price = 3;
    var otherPrice = 5.2;

    // Every string can have template expressions using $ or ${}
    // expression result will be converted to string
    var message = "The $price price is ${otherPrice * 1.1}";
}

playingWithCollections() {
    // Lists, we use List<Type> and the type of the collection inside the generic
    // The literal uses the [] constructor
    // ["Argentina", "Brazil"]
    List<String> countries = ["Argentina", "Brazil", "Canada", "Denmark"];

    // We also have sets and maps (dictionaries)
    var strings = {"a", "b", "c", "c"};
    var readWriteMap = {"foo": 1, "bar": 2};
}
```
- Null safety is very similar to Kotlin

## Flutter

- Declarative UI framework
- **Stateless Widgets**: The widgets whose state can not be altered once they are built are called stateless widgets. These widgets are immutable once they are built i.e any amount of change in the variables, icons, buttons, or retrieving data can not change the state of the app.
- **Stateful Widgets**: A Stateful Widget has its own mutable state that it needs to track. It is modified according to the user's input. A Stateful Widget looks after two things primarily, the changed state based on its previous state and an updated view of the user interface.
- To add an assets/images folder in the root of the project, we will also need to specify the path of that folder under `assets` in `pubspec.yaml`.