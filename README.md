# Speedtest+

[![Opt-in installations reported](https://speedtest.oliverprojects.tech/api/badges/installations.svg)](https://speedtest.oliverprojects.tech/)

Speedtest+ is an unofficial Android test build for personal demonstrations and UI testing. It adds local result controls, profiles, themes, consistent saved results, and user-confirmed app updates.

It is not affiliated with or endorsed by Ookla.

## Open source

The original Speedtest+ core, configuration validation, result finalization, theme-code support, API contracts, schemas, tests, and integration documentation are available in the public [Speedtest+ source repository](https://github.com/Csiklaoliver/speedtest-plus-source).

The repository intentionally excludes APK binaries, signing material, credentials, private infrastructure configuration, and Ookla's proprietary application code.

## Device support

- Android 7.0 or newer
- 64-bit ARM (`arm64-v8a`) phone or tablet
- Android 15/16 compatible, including 16 KB memory-page devices

The original test engine does not include 32-bit ARM or x86 native libraries, so those devices are not supported by this build.

## Start here

- [Download SpeedtestPlus.apk](https://github.com/Csiklaoliver/speedtest-plus-docs/releases/latest/download/SpeedtestPlus.apk)
- [Browse the Speedtest+ source code](https://github.com/Csiklaoliver/speedtest-plus-source)
- [Features and usage](FEATURES.md)
- [Releases and updates](RELEASES.md)
- [Security and privacy](SECURITY.md)

## Quick start

1. Install `SpeedtestPlus.apk` and complete the normal onboarding.
2. Open the provider drawer on the Speed screen.
3. Tap the info button or long-press the provider row to open **Speedtest+ Controls**.
4. Enter only the values you want to customize, then tap **APPLY** before **GO**.

Blank fields keep the measured value. Equal speed minimum and maximum values create an exact result. Different bounds create a varied result inside the range.

Use **DISABLE ALL** to return new tests to normal measured values. Saved profiles and the selected theme are retained.

## Highlights

- Custom download, upload, ping, jitter, and packet-loss values
- Custom ISP, server provider, and server location
- Three named profiles
- Ten persistent dark themes
- Stable one-value-per-test speed overrides on the fixed 1k gauge
- Full GO-to-Connecting and dial-opening animation
- Stable real 720p H.264 Video testing on Android 7–16
- No banner or completed-result native ads
- Matching local values across the test screen, saved results, history, comparison cards, sharing, and CSV
- Provider-aware feedback questions
- Silent signed update checks that prompt only for a newer verified build

## Local and official data

Custom values change the app's customized presentation and saved-result/history representation, including account-history storage where enabled. The underlying official test traffic and official remote report remain separate measured data.

Do not present customized values as independently measured network performance. Use them only for personal testing, UI development, or clearly identified demonstrations.

## Updates

The app checks a small public manifest quietly in the background when the main screen resumes, with a one-hour throttle. It shows nothing when the installed build is current or a background check cannot complete. When a newer build exists, it shows the update prompt, verifies the download, and opens Android's standard update screen.

Android still requires user confirmation. The existing app does not need to be uninstalled, and local settings are preserved.

The APK is published as a GitHub Release asset. APKs, signing secrets, and credentials are never committed to Git history.

## Anonymous installation count

If a user explicitly enables anonymous statistics, the app sends one
`installation_reported` event after a successful submission and stores only a
local Boolean marker to avoid sending it again for that installation. The
event contains no installation, account, advertising, Android, or hardware ID.

The badge above shows the same thresholded rolling 90-day aggregate as the
project website. Values below 25 are shown only as `<25`. It is an opted-in
installation-report count, not a unique-person or lifetime-download count.
