# Mouthpiece Mobile App

## Introduction

The Mouthpiece mobile app will be composed of various modules that should function with almost complete independence from one another. These modules will be integrated into the mobile app and connected to one another by the integration team via custom controllers and UI elements.

Module developers are expected to get to know the [Dart programming language](https://dart.dev/), which bears a strong resemblance to Java in its syntax, plus a few handy features.
See the following links for some useful starting insights:
	- https://medium.com/sk-geek/introducing-dart-programming-language-special-features-c79c1a70645e
	- https://dart.dev/guides/language/effective-dart

Teams wishing to use native Android code are welcome to present their case to the Integration team (e.g. Neural Network applications), however such a shift will only be allowed if there is a clear motive and if the given module team is ready to [platform channels](https://flutter.dev/docs/development/platform-integration/platform-channels) to integrate with the Dart and Flutter environment.

## Module Specifications
The below are simple, relatively informal interface definitions that should be conformed to when developing module code. Each team is welcome to create as many helper classes as they wish using their assigned directory (/lib/modules/ModuleName/), however their surface-level class (/lib/modules/ModuleName.dart) will be the sole point of access to that module's functionality.

### NeuralNetwork
TODO

### Converter
TODO

### UserManagement
TODO

### Sharing
TODO

### Notification.dart
TODO