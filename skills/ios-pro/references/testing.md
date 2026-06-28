# Testing

## Frameworks

- **Swift Testing** (`@Test`, `@Suite`, `#expect`, `#require`) for all new tests (Xcode 16+).
- **XCTest** for existing test suites and UI tests where Swift Testing is not yet supported.

## What to Test

- Unit test: ViewModels, Services, Repositories, pure logic. No UI in unit tests.
- Integration test: network layer with `URLProtocol` stubbing. No live network in CI.
- Snapshot tests for design-critical components (`swift-snapshot-testing` if justified).
- Add a regression test for every bug fixed.
- 80%+ coverage on business logic. Do not test Apple frameworks.

## Mocks

- Mocks and test doubles via protocols + `#if DEBUG` implementations.
- Never ship mock code in the production target.

## SwiftUI Testing

- Prefer unit testing logic in view models over UI tests.
- UI tests only where unit tests are not possible.

## CI

- No live network calls in CI — stub with `URLProtocol`.
- Tests must be deterministic — no time-based assumptions, no random ordering side effects.
