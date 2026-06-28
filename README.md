# ios-pro

A Claude Code skill for senior-level iOS code review and refactoring.

**Target**: iOS 18+ · Swift 6.2+ · Strict concurrency · SwiftUI-first

---

## What it does

Invoked via `/ios-pro`, it acts as a Senior Apple Platform Engineer and reviews code across 13 domains, loading only the reference files relevant to the task:

| Reference | Covers |
|---|---|
| `swift.md` | Swift language rules, concurrency, type safety |
| `swiftui.md` | API substitutions, views, modifiers, animations |
| `architecture.md` | MVVM/Clean, DI, value types, bug prevention |
| `data.md` | `@Observable`, property wrappers, SwiftData |
| `navigation.md` | NavigationStack, sheets, alerts, dialogs |
| `accessibility.md` | Dynamic Type, VoiceOver, HIG, tap targets |
| `performance.md` | Profiling, lazy views, view identity, async |
| `security.md` | Keychain, ATS, secrets, entitlements |
| `privacy.md` | Privacy Manifests, ATT, App Store compliance |
| `localization.md` | xcstrings, RTL, pseudolocalization |
| `testing.md` | Swift Testing, XCTest, coverage, mocks |
| `multiplatform.md` | iPad, PencilKit, visionOS, `#if os()` |
| `tooling.md` | xcodegen, SwiftLint, Xcode MCP, CHANGELOG |

---

## Credits

The SwiftUI-specific rules (API substitutions, data flow, navigation, accessibility, performance, views) are derived from the **[swiftui-pro agent skill](https://github.com/twostraws/swiftui-agent-skill)** by [Paul Hudson](https://github.com/twostraws) — an excellent, well-maintained skill worth using on its own.

This skill extends that foundation with the broader scope of Apple platform engineering: architecture, security, privacy manifests, App Store compliance, localization, testing strategy, PencilKit, iPad-first patterns, multi-platform guards, and project tooling — arranged for personal workflow and opinionated defaults (iOS 18+, Swift 6.2+, xcstrings, modular SPM).

---

## Installation

### Via marketplace (recommended)

```
/plugin marketplace add AlexLombry/ios-pro
/plugin install ios-pro
```

### Via skills install

```
/skills install AlexLombry/ios-pro
```

### Manual

Clone into your Claude Code skills directory:

```bash
git clone https://github.com/AlexLombry/ios-pro ~/.claude/skills/ios-pro
```

---

## Usage

```
/ios-pro
```

For a targeted review, tell it which area to focus on — it will load only the relevant reference files rather than the full set.
