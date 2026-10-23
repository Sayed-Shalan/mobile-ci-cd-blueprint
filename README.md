# mobile-ci-cd-blueprint

Reusable GitHub Actions workflows copied from flagship repositories after their checks passed on this machine on 23 October 2026. GitHub has not executed these workflows yet, so this README has no status badge.

Nothing here publishes a package, an APK, or a release. Every workflow sets `permissions: contents: read`. Caching comes from `subosito/flutter-action` (`cache: true`) and `gradle/actions/setup-gradle`.

## Workflows

| File | Copied from | What it runs |
| --- | --- | --- |
| `flutter-app.yml` | `flutter-architecture-blueprint` | `flutter pub get`, optional `flutter gen-l10n` plus `git diff --exit-code -- lib/l10n`, `dart format`, `flutter analyze`, `flutter test` |
| `dart-package.yml` | `dart-api-toolkit` and `flutter-pagination-toolkit` | `dart pub get`, format, analyze, test, `dart run example/main.dart` |
| `android-app.yml` | `android-architecture-blueprint` | JDK 17, Android SDK, `sdkmanager` for the compile SDK, `./gradlew test lintDebug assembleDebug` |
| `android-jvm.yml` | `android-concurrency-lab` | JDK 17, `./gradlew test` |
| `dependency-review.yml` | GitHub `dependency-review-action` | Pull requests only. Needs the dependency graph enabled. |

The Android app workflow's `assembleDebug` is the build check. It does not sign a release. See [docs/signing.md](docs/signing.md).

## Callers

Pin a commit once this repository is public. Example for the Flutter showcase, with localization checked:

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:

permissions:
  contents: read

jobs:
  flutter:
    uses: Sayed-Shalan/mobile-ci-cd-blueprint/.github/workflows/flutter-app.yml@<commit>
    with:
      flutter-version: "3.41.6"
      check-l10n: true
```

Dart package:

```yaml
jobs:
  package:
    uses: Sayed-Shalan/mobile-ci-cd-blueprint/.github/workflows/dart-package.yml@<commit>
    with:
      sdk: "3.11.4"
```

Android app:

```yaml
jobs:
  android:
    uses: Sayed-Shalan/mobile-ci-cd-blueprint/.github/workflows/android-app.yml@<commit>
    with:
      compile-sdk-package: "platforms;android-37.0"
```

Android JVM lab:

```yaml
jobs:
  jvm:
    uses: Sayed-Shalan/mobile-ci-cd-blueprint/.github/workflows/android-jvm.yml@<commit>
```

Dependency review, on pull requests, and only if the dependency graph is enabled:

```yaml
jobs:
  dependencies:
    uses: Sayed-Shalan/mobile-ci-cd-blueprint/.github/workflows/dependency-review.yml@<commit>
```

The flagship repositories keep their own `.github/workflows/ci.yml`. Those files are the ones that were run locally. Switching them to `uses:` waits until this repository is public and a caller run has passed.

## License

[MIT](LICENSE)
