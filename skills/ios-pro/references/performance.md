# Performance

Profile before claiming optimized. Use Instruments: Time Profiler, Allocations, Leaks, SwiftUI, Hangs.

## View Updates

- Prefer ternary expressions over `if/else` view branching for toggling modifier values — avoids `_ConditionalContent`, preserves structural identity, prevents recreating platform views.
- Avoid `AnyView` — it breaks diffing and forces the runtime to box the view. Use `@ViewBuilder`, `Group`, or generics.
- Break views into separate `View` structs — not computed properties or `@ViewBuilder` methods. Dedicated structs allow SwiftUI to diff independently.
- Minimize view updates: stable `ForEach` identifiers, `Equatable` conformance on props.
- Move sorting, filtering, and formatting out of `body` — assume `body` is called frequently.
- Keep view initializers minimal. Move non-trivial work into `task()`.

## Async Work

- `task()` over `onAppear()` for async work — auto-cancelled on disappear.
- No synchronous work on the main thread: no file I/O, no network, no heavy computation.
- `os_signpost` intervals around performance-critical paths.

## Lists and Scroll

- `LazyVStack` / `LazyHStack` for large datasets in `ScrollView`.
- `List` with stable `id:` values.
- `scrollContentBackground(.visible)` for opaque, static scroll backgrounds — improves scroll-edge rendering.
- Never load unbounded datasets — paginate.
- Avoid expensive inline transforms in `List`/`ForEach` initializers when repeated often.
- Prefer deriving transformed data with `let` or caching in `@State` — but only with explicit invalidation logic to avoid stale UI.

## Images

- `AsyncImage` or custom caching with `NSCache`. Decode off main thread.

## Formatters

- Never create stored formatter properties (e.g. `DateFormatter`) — use `Text` with a format directly: `Text(Date.now, format: .dateTime.day().month().year())`.

## ViewBuilder Closures

- Prefer `@ViewBuilder let content: Content` on generic views over storing an escaping `() -> Content` closure — the synthesized init handles calling the builder.

## Memory

- Check Instruments → Allocations baseline if profiling is possible.
- Fix retain cycles: `[weak self]` in closures, `unowned` only when lifetime is guaranteed.
