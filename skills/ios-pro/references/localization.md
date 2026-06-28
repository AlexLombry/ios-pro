# Localization

## String Catalogs (preferred)

- Use `Localizable.xcstrings` with symbol keys: `extractionState` set to "manual", key as `"helloWorld"` style.
- Access via generated symbols: `Text(.helloWorld)` not `Text("Hello, world!")`.
- When adding new keys, offer to translate into all languages the project already supports.

## Fallback: String(localized:)

- If the project does not use xcstrings, use `String(localized:)` or `LocalizedStringKey` for all user-facing strings. No raw string literals in UI.

## Rules for All Localization Approaches

- No string concatenation for localized text — word order differs by language.
- Prefer automatic grammar agreement for English, French, German, Portuguese, Spanish, Italian: `Text("^[\(count) person](inflect: true)")`.
- Support RTL layouts: use `leading`/`trailing` not `left`/`right` in all padding, alignment, and edge insets.
- For date display: use "y" not "yyyy" for years (correct in all localizations).
- Convert dates from strings with modern API: `Date(string, strategy: .iso8601)` not `DateFormatter`.
- Test with Xcode pseudolocalization: Scheme → App Language → Double-Length Pseudolanguage.
