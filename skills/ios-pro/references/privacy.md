# Privacy and App Store Compliance

## Privacy Manifests

- `PrivacyInfo.xcprivacy` required when using covered APIs: `UserDefaults`, file timestamp APIs, disk space APIs, active keyboard APIs.
- List all third-party SDKs that need their own privacy manifests.

## App Tracking Transparency

- Call `ATTrackingManager.requestTrackingAuthorization` before any IDFA access.

## Permission Strings

- `NSCameraUsageDescription`, `NSLocationWhenInUseUsageDescription`, etc. — use accurate, user-facing language. No generic "This app requires access" strings.
- Only request permissions the app actually uses.

## Data Collection

- No collection of data not declared in the App Store Privacy Nutrition Label.
- Declare all data types, their uses, and whether they are linked to identity.

## App Store Review Guidelines

- Follow §2 (Safety) and §5 (Legal).
- Flag anything that could trigger a rejection: web content rendering, user-generated content without moderation, custom payment flows, subscription terms.
- No undocumented private API usage — verify with `DocumentationSearch` or SDK headers.
