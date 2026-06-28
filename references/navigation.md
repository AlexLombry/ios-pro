# Navigation and Presentation

## Navigation Stack

- Use `NavigationStack` or `NavigationSplitView` — never `NavigationView`.
- Strongly prefer `navigationDestination(for:)` over `NavigationLink(destination:)`.
- Never mix `navigationDestination(for:)` and `NavigationLink(destination:)` in the same hierarchy — causes significant problems.
- Register `navigationDestination(for:)` once per data type — flag duplicates.
- iPad default: `NavigationSplitView` (sidebar + detail), not bare `NavigationStack`. `NavigationStack` alone is a phone pattern.

## Toolbar Placement

- Use `.topBarLeading` / `.topBarTrailing` — `.navigationBarLeading` / `.navigationBarTrailing` are deprecated.

## Sheets and Popovers

- For optional data: `sheet(item:)` over `sheet(isPresented:)` — safely unwraps the optional.
- Shorthand when the presented view takes only the item as its init param: `sheet(item: $item, content: SomeView.init)`.
- On iPad: prefer `.popover` over `.sheet` for transient content. `.sheet` is for full flows.

## Alerts

- Attach `alert` to the element logically connected to the alert's context.
- If an alert has a single "OK" button that only dismisses, the button can be omitted entirely: `.alert("Title", isPresented: $show) { }`.

## Confirmation Dialogs

- Attach `confirmationDialog()` to the UI element that triggers it — enables the correct source animation (Liquid Glass on iOS 26+; cosmetically neutral on iOS 18).

## TabView

- Bind to an enum value, not an integer or string: `Tab("Home", systemImage: "house", value: .home)`.
