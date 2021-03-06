# Logger

[![Travis](https://img.shields.io/travis/com/leisim/logger/master.svg)](https://travis-ci.com/leisim/logger) [![Version](https://img.shields.io/pub/v/logger.svg)](https://pub.dev/packages/logger) ![Runtime](https://img.shields.io/badge/dart-%3E%3D2.1-brightgreen.svg) ![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg?style=flat)

Small, easy to use and extensible logger which prints beautiful logs.<br>
Inspired by [logger](https://github.com/orhanobut/logger) for Android.

**Show some ❤️ and star the repo to support the project**

### Resources:
- [Documentation](https://pub.dev/documentation/logger/latest/logger/logger-library.html)
- [Pub Package](https://pub.dev/packages/logger)
- [GitHub Repository](https://github.com/leisim/logger)

## Getting Started

Just create an instance of `Logger` and start logging:
```dart
var logger = Logger();

logger.d("Logger is working!");
```

Instead of a string message, you can also pass other objects like `List`, `Map` or `Set`.

## Output

![](https://raw.githubusercontent.com/leisim/logger/master/art/screenshot.png)

## Log Console

If you are creating a Flutter app, you can use the [logger_flutter](#logger-flutter-extension) extension. Shake the phone to show an on device console. 

| ![](https://raw.githubusercontent.com/leisim/logger/master/art/log_console_light.png) | ![](https://raw.githubusercontent.com/leisim/logger/master/art/log_console_dark.png) |
|---|---|

# Documentation

## Log level

You can log with different levels:

```dart
logger.v("Verbose log");

logger.d("Debug log");

logger.i("Info log");

logger.w("Warning log");

logger.e("Error log");

logger.wtf("What a terrible failure log");
```

To show only specific log levels, you can set:

```dart
Logger.level = Level.warning;
```

This hides all `verbose`, `debug` and `info` log events.

## Options

When creating a logger, you can pass some options:

```dart
var logger = Logger(
  filter: null, // Use the default LogFilter (-> only log in debug mode)
  printer: PrettyPrinter(), // Use the PrettyPrinter to format and print log
  output: null, // Use the default LogOutput (-> send everything to console)
);
```

If you use the `PrettyPrinter`, there are more options:

```dart
var logger = Logger(
  printer: PrettyPrinter(
    methodCount: 2, // number of method calls to be displayed
    errorMethodCount: 8, // number of method calls if stacktrace is provided
    lineLength: 120, // width of the output
    colors: true, // Colorful log messages
    printEmojis: true, // Print an emoji for each log message
    printTime: false // Should each log print contain a timestamp
  ),
)
```


## LogFilter

The `LogFilter` decides which log events should be shown and which don't.<br>
The default implementation (`DebugFilter`) shows all logs with `level >= Logger.level` while in debug mode. In release mode all logs are omitted.

You can create your own `LogFilter` like this:
```dart
class MyFilter extends LogFilter {
  @override
  bool shouldLog(LogEvent event) {
    return true;
  }
}
```
This will show all logs even in release mode. (**NOT** a good idea)


## LogPrinter

The `LogPrinter` creates and formats the output, which is then sent to the `LogOutput`.<br>
You can implement your own `LogPrinter`. This gives you maximum flexibility.

A very basic printer could look like this:
```dart
class MyPrinter extends LogPrinter {
  @override
  List<String> log(LogEvent event) {
    return [event.message];
  }
}
```

If you created a cool `LogPrinter` which might be helpful to others, feel free to open a pull request. :)


## LogOutput

`LogOutput` sends the log lines to the desired destination.<br>
The default implementation (`ConsoleOutput`) send every line to the system console.

```dart
class ConsoleOutput extends LogOutput {
  @override
  void output(OutputEvent event) {
    for (var line in event.lines) {
      print(line);
    }
  }
}
```

Possible future `LogOutput`s could send to a file, firebase or to Logcat. Feel free to open pull requests.


## logger_flutter extension

The [logger_flutter](https://pub.dev/packages/logger_flutter) package is an extension for logger. You can add it to any Flutter app. Just shake the phone to show the console.

1. [Add logger_flutter](https://pub.dev/packages/logger_flutter#-installing-tab-) to your `pubspec.yaml`
2. Add the following code into your widget tree

```dart
LogConsoleOnShake(
  child: Container() // Your widgets
),
```

More documentation coming soon.

## MIT License
```
Copyright (c) 2019 Simon Leier

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```