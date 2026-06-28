# Accessibility and HIG

HIG compliance is non-negotiable.

## Dynamic Type

- Never hard-code font sizes. Use `.font(.body)`, `.font(.headline)`, etc.
- For custom sizes on iOS 18: use `@ScaledMetric` to scale with the user's text size setting.
- `minimumScaleFactor` only as last resort — prefer layouts that reflow.

## Interactive Elements

- All interactive elements need `accessibilityLabel` and `accessibilityHint` where the purpose isn't obvious.
- Minimum tap target: 44×44 pt — enforce strictly.
- Use `Button` not `onTapGesture()` for all tappable elements unless tap location or count is specifically needed.
- If `onTapGesture()` must be used, add `.accessibilityAddTraits(.isButton)`.
- `Button` with image-only label must include text: `Button("Add", systemImage: "plus", action: myAction)`. SwiftUI will apply the correct label style for context (toolbar, etc.) automatically. If visually icon-only is required, apply `.labelStyle(.iconOnly)` to preserve the text for VoiceOver.
- `Menu`: always provide both an image and text — `Menu("Options", systemImage: "ellipsis.circle") { }`. Use `.labelStyle(.iconOnly)` if icon-only is needed visually.
- If button labels are complex or frequently changing (e.g. live share prices), add `accessibilityInputLabels()` for better Voice Control.

## Images

- Decorative images: `Image(decorative:)` or `.accessibilityHidden(true)`.
- Informative images: descriptive `accessibilityLabel()`.
- Flag images with unclear VoiceOver readings (e.g. `Image(.newBanner2026)` — name says nothing).

## VoiceOver

- VoiceOver navigation order must be logical.
- Never use color as the sole differentiator — check `.accessibilityDifferentiateWithoutColor` and show icons, patterns, or strokes alongside color.

## Color and Contrast

- Contrast ratio: ≥4.5:1 for normal text, ≥3:1 for large text.
- Never use `UIColor` inside SwiftUI views — use `Color` or asset catalog colors.

## Motion

- Respect `@Environment(\.accessibilityReduceMotion)`. Replace large motion-based animations with opacity fades when enabled.

## Typography

- Avoid `.caption2` — extremely small. Use `.caption` carefully.
- `bold()` instead of `fontWeight(.bold)` — lets the system pick the correct weight for context.
- `fontWeight()` only for weights other than bold with a specific reason.

## Layout

- Respect safe areas. No hard-coded insets that break on Dynamic Island or notch devices.
- Use SwiftUI's automatic safe area, `safeAreaInsets`, or `layoutMarginsGuide`.
- Prefer flexible frames over fixed — fixed frames break across device sizes and Dynamic Type settings.

## Platform Idioms

- Prefer native controls (`Picker`, `DatePicker`, `List`, `NavigationStack`) over custom reimplementations.
- Haptics: `sensoryFeedback()` modifier only on meaningful user actions — not `UIImpactFeedbackGenerator`.
- Context menus via `.contextMenu {}` on long-press targets.
