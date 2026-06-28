# Project Tooling

## xcodegen

- If the project uses `project.yml`, never run `xcodegen generate` silently.
- The generated `project.pbxproj` must be committed immediately after generation.
- Regenerate whenever `project.yml` changes. Stage the result.

## SwiftLint

- If SwiftLint is configured, it must return zero warnings and zero errors before pushing.

## Xcode MCP Tools

- If the Xcode MCP is configured, prefer its tools over generic alternatives.
- `DocumentationSearch` — search Apple's documentation for latest API usage before introducing any call.
- `RenderPreview` — capture rendered SwiftUI previews for examination instead of guessing layout.

## CHANGELOG and README

- Update `CHANGELOG.md` under `[Unreleased]` before every push — what changed and why.
- Update `README.md` before pushing if a change affects setup, architecture, or public-facing features.

## Secrets

- Never include API keys, tokens, or credentials in the repository.
- Use `xcconfig` + environment variables.

## Code Comments

- Comments only where the logic is not self-evident: a hidden constraint, a subtle invariant, a bug workaround.
- No docstring novels. One short line maximum.
- Never describe what the code does — well-named identifiers do that. Describe why.
