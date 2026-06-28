# Multi-Platform, iPad, and PencilKit

## Multi-Platform Guards

- Use `#if os(iOS)`, `#if os(macOS)`, `#if os(watchOS)`, `#if os(visionOS)` for platform-specific code.
- Shared business logic in platform-agnostic SPM modules — no UIKit/AppKit imports in shared code.
- State minimum deployment target at start of analysis. Flag APIs that require a higher OS.

## Catalyst

- Test layout on macOS if Catalyst is enabled.
- Never assume touch input — support pointer and keyboard on Catalyst.

## visionOS

- No assumptions about touch — use hover effects, ornaments, and volumes where appropriate.
- `.hoverEffect()` on interactive elements.

## iPad-First Patterns

Apply when the deployment target is iPadOS or the app is iPad-optimized.

- **Navigation**: `NavigationSplitView` (sidebar + detail) as default on iPad, not bare `NavigationStack`.
- **Multitasking**: always test Split View, Slide Over, Stage Manager. Never assume full screen.
- Use size classes (`horizontalSizeClass`) to adapt layout — not device idiom.
- **Apple Pencil**: `UIPencilInteraction` for squeeze/double-tap. Never assume touch-only input.
- **Hover**: `.hoverEffect()` on interactive elements (iPadOS 16+) for Pencil/pointer hover.
- **Keyboard**: `.keyboardShortcut()` on primary actions for hardware keyboard users.
- **Context menus**: `.contextMenu {}` on long-press targets — matches iPad HIG expectations.
- **Popovers**: prefer `.popover` over `.sheet` for transient content on iPad.

## PencilKit

Apply when `PKCanvasView`, `PKDrawing`, or `PKInkingTool` are present.

- **`drawingPolicy = .anyInput`** — replaces the deprecated `allowsFingerDrawing`. 1-finger draws; 2-finger gestures handled by the parent `UIScrollView`.
- **`PKInkingTool(.pen)` not `.pencil`** — `.pencil` renders grain/texture that degrades at large widths; `.pen` gives clean solid strokes.
- **Undo/Redo**: `@Environment(\.undoManager)` does not reach `PKCanvasView`. Implement manual undo/redo with `[PKDrawing]` snapshot stacks; swap by setting `canvas.drawing` directly.
- **Fill layer**: raster flood-fill must live in a separate `UIImage` layer, not inside `PKDrawing`. PencilKit strokes must always render on top of fill regardless of operation order.
- **`PKToolPicker`**: use for Apple Pencil tool selection on iPadOS. Observe `toolPickerVisibilityDidChange` to adjust layout.
- **Canvas coordinate space**: if using a fixed canvas size (e.g. A4 794×1123 pt), use that constant in all services — thumbnail, export, fill. Never derive canvas size from screen bounds.
- **Update loop guard**: `PKCanvasViewDelegate.canvasViewDrawingDidChange` firing during `updateUIView` causes feedback loops. Use an `ignoreNextUpdate` flag to break the cycle.
