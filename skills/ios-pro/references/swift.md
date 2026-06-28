# Swift

## Language Rules

- Prefer Swift-native string methods: `replacing("a", with: "b")` not `replacingOccurrences(of:with:)`.
- Prefer modern Foundation URLs: `URL.documentsDirectory`, `appending(path:)` — never `URL(fileURLWithPath:)` or raw path strings.
- Never use C-style number formatting: `String(format: "%.2f", x)`. Use `FormatStyle`: `x.formatted(.number.precision(.fractionLength(2)))` or `Text(x, format: .number.precision(.fractionLength(2)))`.
- Never use `DateFormatter`, `NumberFormatter`, or `MeasurementFormatter`. Use `FormatStyle` / `ParseStrategy`.
- Prefer static member lookup: `.circle` not `Circle()`, `.borderedProminent` not `BorderedProminentButtonStyle()`.
- Prefer `Double` over `CGFloat` except in optionals or `inout` parameters; Swift bridges freely otherwise.
- Use `count(where:)` instead of `.filter { }.count`.
- Prefer `Date.now` over `Date()` for clarity.
- When dealing with people's names, use `PersonNameComponents` with modern formatting — not `Text("\(firstName) \(lastName)")`.
- If a type is repeatedly sorted with the same closure, conform it to `Comparable` to centralize sort order.
- Prefer `Date(string, strategy: .iso8601)` for string-to-date parsing over manual `DateFormatter`.
- For date display: use "y" not "yyyy" for years so localization works correctly; prefer `Text(date, format: .dateTime.day().month().year())` over manual formatters.
- Prefer `if let value {` shorthand over `if let value = value {`.
- Omit `return` in single-expression functions. Use `if` and `switch` as expressions when assigning or returning.
- For user-driven text filtering, use `localizedStandardContains()` — not `contains()` or `localizedCaseInsensitiveContains()`.
- Avoid force unwrap (`!`) and force `try!` unless failure is truly unrecoverable; prefer `if let`, `guard let`, nil-coalescing, `try?`, or `do-catch`. When force-unwrap remains, it must be explained with a comment.
- Errors from user-triggered actions must never be swallowed silently with `print(error)`. Show an alert or equivalent.
- Use `Result<Success, Failure>` or `throws` for fallible operations. No empty `catch {}`.
- Exhaustive `switch` on enums — no `default:` unless the enum is `@unknown` from ObjC.
- Avoid implicit optionals on stored properties except `@IBOutlet`.
- When `import SwiftUI` is present, do not add `import UIKit` or `import AppKit` to access `UIImage` / `NSImage` — they are imported automatically.


## Swift Concurrency

- Prefer `async/await` over closure-based APIs wherever both exist.
- No GCD — no `DispatchQueue` of any kind, including `DispatchQueue.main.async`. Use `@MainActor` or `await MainActor.run {}` exclusively.
- Never use `Task.sleep(nanoseconds:)`. Use `Task.sleep(for:)`.
- Use `actor` for shared mutable state. No `NSLock`, `DispatchSemaphore`, or manual locking unless documented with justification.
- Enforce `@MainActor` on all UI-touching types and functions.
- All `Task {}` fire-and-forget calls must handle errors — no empty `catch {}`.
- Propagate `CancellationError`. Never ignore `Task.isCancelled`.
- No data races. All shared mutable state behind `actor` or `@MainActor`.
- `Task.detached()` is almost always wrong — flag every usage and verify the intent.
- `MainActor.run()` may be unnecessary if the project sets default actor isolation to Main Actor — check before adding.
- Prefer `AsyncSequence` / `AsyncStream` over Combine publishers for new code.
- Assume strict concurrency is active (`SWIFT_STRICT_CONCURRENCY = complete`). Eliminate all `Sendable` warnings.
- Flag `@Sendable` violations and data race warnings.


## Fix Retain Cycles

- `[weak self]` in closures capturing reference types.
- `unowned` only when lifetime is guaranteed by the type system.
