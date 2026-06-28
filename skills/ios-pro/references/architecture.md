# Architecture

## Identification

Always name the current architecture explicitly: MVC, MVVM, VIPER, TCA, flat, spaghetti. Never refactor without stating what exists.

## Target Architecture

- MVVM + Clean layers for standard apps: Views, ViewModels, Models, Services, Repositories, Coordinators, Utilities.
- Modular SPM-based architecture when the project is large enough to benefit from isolation.
- Feature-based folder layout — group by feature, not by type.

## Value Types vs Reference Types

- Prefer `struct` and `enum` over `class` unless identity or lifecycle requires a reference type.
- `@Observable` ViewModels are `class` — that's correct — but mark `@MainActor`.

## Dependency Injection

- DI over singletons. Singletons only for true global state (`UserDefaults`, `FileManager`, `URLSession`).
- No massive Views, massive ViewModels, or massive AppDelegates.
- No duplicated logic. Naming per Swift API Design Guidelines.

## Design Tokens

- Place standard fonts, sizes, colors, stack spacing, padding, rounding, and animation timings into a shared enum of constants. Uniform design, easy to adjust.

## Bug Prevention

- Guard at system boundaries (network response, user input, deep link params). Trust internal code and framework guarantees.
- Exhaustive `switch` on enums — no `default:` unless `@unknown` from ObjC.
- `Result<Success, Failure>` or `throws` for fallible operations. No error swallowing.
- State machines for complex flows: valid states as `enum`, invalid transitions impossible by type.
- Never use `DispatchQueue.asyncAfter` as a timing workaround — it's a bug, not a fix.
- No time-based race assumptions.

## SPM

- Modular code via local or remote SPM packages.
- No CocoaPods unless a legacy dependency forces it — document why.
- Shared business logic in platform-agnostic SPM modules (no UIKit/AppKit imports).
