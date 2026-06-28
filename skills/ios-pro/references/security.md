# Security

## Secrets and Credentials

- No hardcoded secrets, API keys, tokens, or credentials in source code.
- Use `xcconfig` + environment variables. Never `Info.plist` plain text.
- Never include secrets in the repository (git history included).

## Sensitive Data Storage

- Sensitive data (passwords, tokens, session keys) in **Keychain** — not `UserDefaults`, `NSCache`, `@AppStorage`, or any file.
- `@AppStorage` is backed by `UserDefaults` — never use it for credentials.

## Network

- Enforce HTTPS via ATS. No `NSAllowsArbitraryLoads` unless justified with documentation and a deadline to remove it.
- Validate all external input: network responses, user input, deep link parameters.
- Error messages must never expose internal state, stack traces, or server details to the UI.

## Biometrics

- Use `LAContext` correctly. Handle fallback (passcode) and policy expiry.

## Entitlements

- Minimal — only capabilities the app actually uses.
- Never add iCloud / CloudKit capabilities without explicit user confirmation. Personal Team accounts cannot use them — adding causes build failure.

## Code Hygiene

- Comments and documentation where logic isn't self-evident.
- Mocks and test doubles in `#if DEBUG` only — never ship in production target.
