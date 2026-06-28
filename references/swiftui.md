# SwiftUI API

## Deprecated → Modern Substitutions (enforce strictly)

| Never use | Use instead |
|---|---|
| `foregroundColor()` | `foregroundStyle()` |
| `.cornerRadius()` | `clipShape(.rect(cornerRadius:))` |
| `NavigationView` | `NavigationStack` + `navigationDestination(for:)` |
| `onChange(of:)` 1-param form | 2-param `{ old, new in }` or 0-param `{}` |
| `onTapGesture()` for buttons | `Button` (unless tap location or count needed) |
| `fontWeight(.bold)` | `bold()` — lets the system choose correct weight for context |
| `UIGraphicsImageRenderer` | `ImageRenderer` |
| `GeometryReader` | `containerRelativeFrame()`, `visualEffect()`, or `Layout` protocol |
| `ScrollView(showsIndicators: false)` | `.scrollIndicators(.hidden)` |
| `Array(x.enumerated())` in ForEach | `ForEach(x.enumerated(), id: \.element.id)` |
| `UIScreen.main.bounds` | `containerRelativeFrame()`, `visualEffect()`, or `GeometryReader` as last resort |
| `UIColor` inside SwiftUI views | `Color` or asset catalog `Color` |
| Hard-coded font sizes | Dynamic Type: `.font(.body)`, `.font(.headline)` etc. |
| Hard-coded padding / spacing | omit unless explicitly requested |
| `tabItem()` | `Tab` API (iOS 18+) |
| `GeometryReader` | `containerRelativeFrame()` or `visualEffect()` |
| Manual `EnvironmentKey` conformance + extension | `@Entry` macro (iOS 18+) |
| `overlay(_:alignment:)` | `overlay(alignment:content:)` |
| `.navigationBarLeading` / `.navigationBarTrailing` | `.topBarLeading` / `.topBarTrailing` |
| `Text("A") + Text("B")` concatenation | Store in `let` vars, then `Text("\(a)\(b)")` |
| `UIImpactFeedbackGenerator` | `sensoryFeedback()` modifier (iOS 17+) |
| `PreviewProvider` | `#Preview` macro |
| `animation(_:)` without `value:` | `.animation(.bouncy, value: someState)` |
| Manual `animatableData` | `@Animatable` macro + `@AnimatableIgnored` for non-animatable props |
| `Image("avatar")` (string literal) | `Image(.avatar)` generated asset symbol when configured |
| WKWebView in UIViewRepresentable | Native `WebView` view (iOS 26+ only — skip for iOS 18 target) |


## View Structure

- Extract subviews into separate `View` structs — never computed properties or methods that return `some View`, even with `@ViewBuilder`. (This is the single most-repeated mistake.)
- Each struct, class, enum in its own Swift file.
- `body` must stay shallow. Flag excessively long `body` properties.
- Button actions extracted from `body` into separate methods — no mixing layout and logic.
- Business logic belongs in view models, not inline in `task()`, `onAppear()`, or `body`.
- `#Preview` not `PreviewProvider`.
- `AnyView` is a last resort — it breaks diffing and costs performance. Use `@ViewBuilder`, `Group`, or generics.
- Prefer `@ViewBuilder let content: Content` on generic views over storing an escaping `() -> Content` closure.
- When toggling modifier values, prefer ternary expressions over `if/else` view branching — avoids `_ConditionalContent`, preserves structural identity.
- `Button` with image-only label must include accessible text: `Button("Add", systemImage: "plus", action: myAction)`.
- If a button action can be a direct parameter, use it: `Button("Save", action: save)` not `Button("Save") { save() }`.
- `task(id:)` for async work tied to view lifecycle — cancels on disappear automatically. Prefer over `onAppear`.
- `#if DEBUG` previews only — never ship mock/preview code in production target.


## TabView

- Use `TabView(selection:)` bound to an enum value, not an integer or string.
- Use `Tab("Home", systemImage: "house", value: .home)` not `tabItem()`.


## Sheets, Alerts, Dialogs

- Sheet for optional data: `sheet(item:)` over `sheet(isPresented:)`.
- Shorthand: `sheet(item: $item, content: SomeView.init)` when view's only init param is the item.
- `alert` with a single dismissal "OK" that does nothing: the button can be omitted entirely.
- `confirmationDialog()` must be attached to the element that triggers it (enables correct Liquid Glass animation on iOS 26+; harmless on iOS 18).


## Animations

- Always provide a `value:` to watch: `.animation(.bouncy, value: score)` — never bare `animation(_:)`.
- Chain animations via `completion:` closure in `withAnimation`, not multiple `withAnimation` calls with delays.
- Prefer `@Animatable` macro over manual `animatableData`. Mark non-animatable properties `@AnimatableIgnored`.


## Shapes and Styling

- `RoundedRectangle` default rounding style is `.continuous` — do not specify it explicitly.
- Fill and stroke a shape with two chained modifiers; an overlay for the stroke is no longer needed (fixed iOS 17+).
- Prefer system hierarchical styles (secondary/tertiary) over manual opacity — the system adapts context automatically.


## Forms and Labels

- Wrap `Slider` and similar controls in `LabeledContent` inside `Form`.
- `LabeledContent` works outside `Form` too — define a custom `LabeledContentStyle` for consistency.
- `Label` preferred over `HStack` for icon + text pairs.


## Empty States

- `ContentUnavailableView` for missing or empty data — not custom designs.
- `ContentUnavailableView.search` for empty search results — includes search term automatically.


## TextField

- Prefer `TextField("Prompt", axis: .vertical)` over `TextEditor` unless a full-screen editing experience is required.
- Numeric input: `TextField("Score", value: $score, format: .number)` with `.keyboardType(.numberPad)` or `.decimalPad`.


## Scroll

- `scrollContentBackground(.visible)` for opaque, static scroll backgrounds — improves scroll-edge rendering.
