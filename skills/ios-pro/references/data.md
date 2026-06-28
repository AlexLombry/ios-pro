# Data Flow, Shared State, and Property Wrappers

## Observation Framework (iOS 17+, required at iOS 18+)

- Use `@Observable` instead of `ObservableObject` / `@Published` for all ViewModels.
- `@Observable` classes must be `@MainActor` unless the project enables global Main Actor isolation.
- Use `@Bindable` for binding into `@Observable` objects.
- Never use `ObservableObject`, `@Published`, `@StateObject`, `@ObservedObject`, or `@EnvironmentObject` in new code. Retain only in legacy contexts where changing architecture is costly — call it out explicitly.
- If `ObservableObject` is unavoidable (e.g. a Combine-based debouncer), add `import Combine` explicitly — it is no longer auto-imported via SwiftUI.

## Shared State

- All shared data: `@Observable` classes with `@State` (ownership) and `@Bindable` / `@Environment` (passing).
- `@State` must be `private` and owned only by the view that created it.
- Carve logic into separate `@Observable` classes rather than leaving it inline in `body`.

## Bindings

- Strongly prefer bindings from `@State`, `@Binding`, or `@Bindable` — not `Binding(get:set:)` in view body.
- Use `onChange()` to trigger side effects instead of a custom binding setter.
- Numeric `TextField`: use the `format:` initializer plus `.keyboardType(.numberPad)` or `.decimalPad` — the keyboard modifier alone is not sufficient.

## @AppStorage

- Never use `@AppStorage` inside an `@Observable` class, even with `@ObservationIgnored` — it will not trigger view updates.
- Never store usernames, passwords, or sensitive data in `@AppStorage`. Use Keychain.

## Working with Collections

- Prefer structs conforming to `Identifiable` over `id: \.someProperty` in `ForEach`.
- `ForEach(x.enumerated(), id: \.element.id)` — no `Array()` wrapper.

## @Environment

- Use `@Entry` macro (iOS 18+) to define custom `EnvironmentValues`, `FocusValues`, `Transaction`, and `ContainerValues` keys. Replaces the legacy `EnvironmentKey` + extension pattern.

## SwiftData (iOS 17+)

- Prefer SwiftData over CoreData for new persistence. Use `@Model`, `ModelContainer`, `ModelContext` correctly.
- Retain CoreData only for existing data with migration cost — document why.
- `ModelContext.fetchCount()` does not live-update on data changes unless something else (e.g. `@Query`) triggers the update. Use carefully.

## SwiftData + CloudKit

- Never use `@Attribute(.unique)` — not compatible with CloudKit sync.
- All model properties must have default values or be optional.
- All relationships must be optional.
