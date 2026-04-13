# todoapp

A focused Flutter to-do application with a simple architecture and clear UI composition.
The app entry point is [lib/main.dart](lib/main.dart), state orchestration is in [lib/home_page.dart](lib/home_page.dart), and reusable UI components live in [lib/util/](lib/util/), especially [lib/util/todo_tile.dart](lib/util/todo_tile.dart), [lib/util/dialog_box.dart](lib/util/dialog_box.dart), and [lib/util/my_button.dart](lib/util/my_button.dart).

This first release prioritizes:
- predictable local state updates
- reusable widget composition
- mobile-native interaction patterns (including swipe actions)

## Interesting Techniques Used

- Stateful list mutation with explicit UI invalidation in [lib/home_page.dart](lib/home_page.dart) using `setState`, plus closure-based callbacks passed into child widgets for controlled updates.
- Lazy list item construction with `ListView.builder` in [lib/home_page.dart](lib/home_page.dart), which scales better than rendering a full static list up front.
- Swipe-to-reveal destructive actions in [lib/util/todo_tile.dart](lib/util/todo_tile.dart) via [flutter_slidable](https://pub.dev/packages/flutter_slidable), with an `ActionPane` and stretch motion behavior.
- Dialog-driven task creation in [lib/home_page.dart](lib/home_page.dart) and [lib/util/dialog_box.dart](lib/util/dialog_box.dart) through `showDialog` + `AlertDialog` + `TextField` composition.
- Theme token seeding in [lib/main.dart](lib/main.dart) using `ColorScheme.fromSeed`, which keeps color decisions centralized and consistent.
- Checkbox-driven completion state in [lib/util/todo_tile.dart](lib/util/todo_tile.dart), conceptually aligned with web checkbox semantics in [MDN: input type="checkbox"](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/input/checkbox).
- Modal interaction pattern comparable to [MDN: HTML dialog element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dialog) for teams that also build web UIs.
- Touch gesture mental model that maps well to [MDN: Pointer Events](https://developer.mozilla.org/en-US/docs/Web/API/Pointer_events) when designing cross-platform interaction behavior.

## Non-Obvious Technologies and Libraries

- [Flutter](https://flutter.dev): cross-platform UI runtime used for mobile, desktop, and web targets in this repo.
- [flutter_slidable](https://pub.dev/packages/flutter_slidable): production-friendly swipe actions for list rows.
- [flutter_lints](https://pub.dev/packages/flutter_lints): baseline static analysis profile, wired through [analysis_options.yaml](analysis_options.yaml).
- [Material Icons](https://fonts.google.com/icons): enabled by `uses-material-design` in [pubspec.yaml](pubspec.yaml), used for icon glyph rendering.
- [Roboto](https://fonts.google.com/specimen/Roboto): default Android Material text face (no custom font override is configured in [pubspec.yaml](pubspec.yaml)).
- Kotlin Gradle DSL in [android/](android/) (`*.kts`) and CMake-based desktop runners in [linux/](linux/) and [windows/](windows/) for platform builds.

## Project Structure

```text
analysis_options.yaml
pubspec.yaml
README.md
android/
build/
ios/
	Runner/
		Assets.xcassets/
lib/
	util/
linux/
macos/
	Runner/
		Assets.xcassets/
native_assets/
native_hooks/
web/
	icons/
windows/
```

Interesting directories:
- [lib/](lib/): application logic and widget composition.
- [lib/util/](lib/util/): reusable presentation components (`ToDoTile`, dialog, button).
- [web/icons/](web/icons/): web icon assets used by the generated web target.
- [ios/Runner/Assets.xcassets/](ios/Runner/Assets.xcassets/): iOS asset catalog.
- [macos/Runner/Assets.xcassets/](macos/Runner/Assets.xcassets/): macOS asset catalog.
- [native_assets/](native_assets/) and [native_hooks/](native_hooks/): native integration scaffolding included by current toolchain.
