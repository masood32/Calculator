# Zakat & Fitra Calculator — Android App

A native Android WebView application that wraps the Zakat & Fitra calculator HTML file, providing a full-screen app experience with native file saving (PNG/PDF export to Downloads folder).

---

## Prerequisites

| Tool | Version |
|------|---------|
| Android Studio | Hedgehog (2023.1.1) or newer |
| JDK | 17 (bundled with Android Studio) |
| Android SDK | API 34 (Android 14) |
| Gradle | 8.x (auto-managed) |
| Device / Emulator | Android 5.0 (API 21) or higher |

---

## Project Structure

```
ZakatCalculatorApp/
├── app/
│   ├── src/
│   │   └── main/
│   │       ├── assets/
│   │       │   └── zakat-fitra-calculator.html   ← Calculator (copy from web)
│   │       ├── java/com/masoodai/zakaat/
│   │       │   └── MainActivity.kt               ← WebView host + JS bridge
│   │       ├── res/
│   │       │   ├── values/
│   │       │   │   ├── colors.xml                ← Gold/dark brand colors
│   │       │   │   ├── strings.xml               ← App name
│   │       │   │   └── themes.xml                ← Splash screen + main theme
│   │       │   └── mipmap-*/                     ← App launcher icons
│   │       └── AndroidManifest.xml               ← Permissions + activity config
│   └── build.gradle                              ← App-level build config
├── build.gradle                                  ← Project-level build config
└── settings.gradle
```

---

## How to Build

### Step 1 — Open in Android Studio
1. Launch Android Studio
2. Click **File → Open**
3. Select the `ZakatCalculatorApp/` folder
4. Wait for Gradle sync to complete

### Step 2 — Build APK
- **Debug APK** (for testing):
  Go to **Build → Build Bundle(s) / APK(s) → Build APK(s)**
  Output: `app/build/outputs/apk/debug/app-debug.apk`

- **Release APK** (for distribution):
  Go to **Build → Generate Signed Bundle / APK**
  Choose APK → create or select a keystore → build

### Step 3 — Install on Device
```bash
# Via ADB (USB debugging enabled on device)
adb install app/build/outputs/apk/debug/app-debug.apk
```
Or simply click the **Run** button in Android Studio with a connected device/emulator.

---

## Key Components

### MainActivity.kt

The single activity that hosts a full-screen `WebView`:

- Enables JavaScript, DOM storage, and file access
- Sets a custom `WebViewClient` to handle page navigation
- Injects `ZakatInterface` as a JavaScript bridge object

```kotlin
webView.addJavascriptInterface(ZakatInterface(this), "ZakatInterface")
webView.loadUrl("file:///android_asset/zakat-fitra-calculator.html")
```

### JavaScript Bridge (ZakatInterface)

Handles the Export button since `<a download>` does not work in Android WebView.

When the user taps Export → PNG or Export → PDF:
1. The HTML/JS captures the canvas and calls `window.ZakatInterface.saveFile(base64, fileName, mimeType)`
2. The Kotlin bridge decodes the base64 string
3. On **Android 10+**: saves via `MediaStore.Downloads`
4. On **Android 9 and below**: saves directly to `Environment.DIRECTORY_DOWNLOADS`
5. A `Toast` message confirms the save

### Permissions (AndroidManifest.xml)

| Permission | Purpose |
|-----------|---------|
| `INTERNET` | Load CDN libraries (html2canvas, jsPDF) for export |
| `WRITE_EXTERNAL_STORAGE` | Save files on Android 9 and below (maxSdkVersion 28) |

> Android 10+ does not require `WRITE_EXTERNAL_STORAGE` for saving to Downloads via MediaStore.

---

## App Configuration

| Property | Value |
|----------|-------|
| Package name | `com.masoodai.zakaat` |
| Min SDK | 21 (Android 5.0 Lollipop) |
| Target SDK | 34 (Android 14) |
| Orientation | Portrait only |
| Theme | Dark gold (`#c9a84c` on `#0f2027`) |

---

## Splash Screen

Uses `androidx.core:core-splashscreen` library.

- Background: `#0f2027` (dark navy)
- Icon: App launcher icon (`ic_launcher`)
- Automatically transitions to the main calculator screen

Configured in `res/values/themes.xml`:
```xml
<style name="Theme.ZakatCalculator.Splash" parent="Theme.SplashScreen">
    <item name="windowSplashScreenBackground">@color/dark_bg</item>
    <item name="windowSplashScreenAnimatedIcon">@mipmap/ic_launcher</item>
    <item name="postSplashScreenTheme">@style/Theme.ZakatCalculator</item>
</style>
```

---

## Updating the Calculator

To update the HTML content:

1. Edit `zakat-fitra-calculator.html` in the web version (`ttol/`)
2. Copy the updated file to `ZakatCalculatorApp/app/src/main/assets/`
3. Rebuild and reinstall the app

```bash
cp ttol/zakat-fitra-calculator.html ZakatCalculatorApp/app/src/main/assets/
```

The HTML file runs entirely offline from the `assets/` folder — no internet required for the calculator itself. Internet is only needed when using Export (loads `html2canvas` and `jsPDF` from CDN).

---

## Tested Android Versions

| Android Version | API Level | Status |
|----------------|-----------|--------|
| Android 5.0–5.1 | 21–22 | Supported (min) |
| Android 8.0–9.0 | 26–28 | Supported (legacy file save) |
| Android 10–11 | 29–30 | Supported (MediaStore) |
| Android 12–14 | 31–34 | Supported (target) |

---

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Gradle sync fails | Check internet connection; try **File → Sync Project with Gradle Files** |
| App crashes on launch | Ensure `zakat-fitra-calculator.html` exists in `app/src/main/assets/` |
| Export does nothing | Check INTERNET permission; test on a device with internet access |
| White screen on load | Verify JavaScript is enabled in WebView settings |
| File not saved | Grant Storage permission manually in **Settings → Apps → Zakat Calc → Permissions** (Android 9 and below) |

---

## Disclaimer

This app is a guide only. Zakat rules may vary by school of thought. Always verify your calculation with a qualified Islamic scholar.
