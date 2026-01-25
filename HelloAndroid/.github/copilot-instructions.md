# Copilot Instructions for HelloAndroid

## Project Overview

**HelloAndroid** is a minimal Android application demonstrating modern Kotlin development with Material Design. It serves as a foundation for expanding the Emily AI voice companion to Android platforms, complementing the browser-based web versions.

## Build & Project Structure

### Gradle Build System
- **Build tool**: Gradle with Android Gradle Plugin 8.4.1
- **Kotlin compiler**: Latest stable (configured via `org.jetbrains.kotlin.android` plugin)
- **Target SDK**: 34 (Android 14), Minimum SDK: 28 (Android 9)
- **Version management**: Root-level `ext {}` variables in `build.gradle` for centralized dependency versions

### Directory Organization
```
HelloAndroid/
├── build.gradle              # Root build config & shared dependency versions
├── gradle.properties         # JVM/Android configuration flags
├── settings.gradle           # Project module settings
├── app/
│   ├── build.gradle          # App-specific build config
│   ├── proguard-rules.pro    # Code obfuscation rules for release builds
│   └── src/
│       └── main/
│           ├── AndroidManifest.xml
│           ├── java/com/example/hello/
│           │   └── MainActivity.kt   # Single Activity entry point
│           └── res/
│               ├── layout/
│               │   └── activity_main.xml
│               └── values/
│                   ├── colors.xml
│                   ├── strings.xml
│                   └── themes.xml
```

## Development Workflow

### Build Commands
```bash
# Debug APK (no optimization)
./gradlew assembleDebug

# Release APK (minified, shrunk, obfuscated)
./gradlew assembleRelease

# Run on connected device/emulator
./gradlew installDebug

# Run tests (add test directory when creating test suite)
./gradlew test

# Clean build
./gradlew clean
```

### Local Environment Setup
- **Android SDK**: API 34+ required (Android Studio auto-manages this)
- **JVM**: Gradle configured with `-Xmx1g` heap (see gradle.properties)
- **Kotlin**: Version pinned in plugin declaration (update via Android Studio)
- **AndroidX**: Enabled globally (`android.useAndroidX=true` in gradle.properties)

### Debugging & Emulation
- Connect physical device or launch Android Studio emulator
- Logcat filters by package: `com.example.hello`
- Breakpoints work in MainActivity.kt directly in Android Studio
- Memory profiling available via Android Profiler (Monitor → Profiler tab)

## Architecture & Patterns

### Activity Lifecycle
The single `MainActivity` extends `AppCompatActivity` with minimal lifecycle override:
```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)  // Inflate layout XML
    }
}
```
- No background services or fragments—startup is synchronous
- UI state persists across lifecycle via `savedInstanceState` (to implement when adding state)

### Resource Structure
- **Strings**: Centralized in `res/values/strings.xml` (support multiple languages via `res/values-XX/`)
- **Colors**: Defined in `res/values/colors.xml`, referenced in themes and layouts
- **Themes**: Material 3 DayNight theme with color variants (primary, secondary, error, etc.)
- **Layouts**: XML-based declarative UI (LinearLayout currently; use ConstraintLayout for complex UIs)

### Material Design Integration
- **Theme parent**: `Theme.MaterialComponents.DayNight.NoActionBar` (dark/light mode aware)
- **Dependencies**: `androidx.appcompat`, `com.google.android.material` (color tokens, components)
- **Status bar color**: Customized via theme attributes (currently purple_700)

## Dependencies & Versions

### Core Libraries
- `androidx.core:core-ktx:1.13.1` — Kotlin extensions, modern coroutine support
- `androidx.appcompat:appcompat:1.7.0` — Backward compatibility, Activity/Fragment base classes
- `com.google.android.material:material:1.12.0` — Material Design components (Button, Card, etc.)

### Dependency Management
Versions centralized in root `build.gradle`:
```groovy
ext {
    coreKtxVersion = '1.13.1'
    appCompatVersion = '1.7.0'
    materialVersion = '1.12.0'
}
```
Reference in app `build.gradle` as `$rootProject.ext.coreKtxVersion`. Update all three in one place.

### Future Dependencies (When Needed)
- **Coroutines**: `org.jetbrains.kotlinx:kotlinx-coroutines-android` for async work
- **Room Database**: `androidx.room:room-*` for local persistence
- **Retrofit/OkHttp**: For API communication with backend services
- **Jetpack Compose**: Modern declarative UI framework (alternative to XML layouts)

## Release & ProGuard Configuration

### Release Build Pipeline
1. `minifyEnabled = true` activates ProGuard code shrinking
2. `shrinkResources = true` removes unused resources
3. ProGuard rules in `proguard-rules.pro` preserve reflection/serialization classes
4. Signed APKs generated when keystore configured (Android Studio: Build → Generate Signed Bundle/APK)

### Lint Configuration
Strict settings in app `build.gradle`:
- `abortOnError = true` — Build fails on critical issues (no warnings allowed in release)
- `warningsAsErrors = true` — Treat all warnings as errors
- `checkReleaseBuilds = true` — Lint runs on release variants

## Integration Points with Emily AI

### Planned Connections
- **Voice input**: Use Web Socket or local HTTP bridge to communicate with browser Emily or backend
- **Diary sync**: Sync entries to same localStorage-compatible persistence layer (Room Database)
- **Camera/mic**: Use Android's `MediaRecorder` and `Camera2 API` for audio/video capture (parallels web version)
- **Sentiment analysis**: Reuse keyword-based sentiment logic or call shared backend model service

### Package Namespace
Currently `com.example.hello`. When expanding to Emily Android:
- Rename to `com.emilyai.mobile` or similar
- Update `AndroidManifest.xml` package attribute
- Update directory structure: `src/main/java/com/emilyai/mobile/`

## Testing & Quality Assurance

### Unit Tests
- Add test source set: `app/src/test/java/com/example/hello/`
- Use JUnit 4 (add to `dependencies`)
- Run: `./gradlew test`

### Instrumented Tests (UI Tests on Device)
- Add Android Test source set: `app/src/androidTest/java/com/example/hello/`
- Use Espresso framework for UI testing
- Run: `./gradlew connectedAndroidTest` (requires connected device/emulator)

### Lint & Static Analysis
- Run manually: `./gradlew lint`
- Results in `app/build/reports/lint-results.html`
- Android Studio inspections (Analyze → Run Inspection by Name)

## When Extending the Codebase

1. **Add new Activities**: Create in `src/main/java/com/example/hello/`, declare in AndroidManifest.xml
2. **Create Fragments**: Use `Fragment` base class for reusable UI modules
3. **Add Resources**: Place in `res/` subdirectories (layouts, drawables, strings)
4. **Update build config**: Add dependencies to root `build.gradle` `ext {}` and app `build.gradle` `dependencies {}`
5. **Version management**: Update SDK targets in app `build.gradle` `android {}` block
6. **Lint warnings**: Fix immediately—build will fail if enabled in strict mode

## Key Commands Reference

| Command | Purpose |
|---------|---------|
| `./gradlew assembleDebug` | Build debug APK |
| `./gradlew assembleRelease` | Build release APK |
| `./gradlew installDebug` | Deploy debug APK to device |
| `./gradlew test` | Run unit tests |
| `./gradlew connectedAndroidTest` | Run device/emulator tests |
| `./gradlew clean` | Remove build artifacts |
| `./gradlew lint` | Run static analysis |

---

**Last Updated**: December 21, 2025  
**Min SDK**: Android 9 (API 28) | **Target SDK**: Android 14 (API 34)  
**Language**: Kotlin | **Build System**: Gradle 8.4.1
