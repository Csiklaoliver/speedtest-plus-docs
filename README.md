# Speedtest+

Speedtest+ is an unofficial Android test build for personal demonstrations and UI testing. It adds local result controls, profiles, themes, consistent saved results, and user-confirmed app updates.

It is not affiliated with or endorsed by Ookla.

## Start here

- [Download SpeedtestPlus.apk](https://github.com/Csiklaoliver/speedtest-plus-docs/releases/latest/download/SpeedtestPlus.apk)
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
- Matching local values across the test screen, saved results, history, comparison cards, sharing, and CSV
- Provider-aware feedback questions
- Signed update checks from the app

## Local and official data

Custom values change the app's customized presentation and saved-result/history representation, including account-history storage where enabled. The underlying official test traffic and official remote report remain separate measured data.

Do not present customized values as independently measured network performance. Use them only for personal testing, UI development, or clearly identified demonstrations.

## Updates

The app checks a small public manifest and offers only builds with a higher Android version code. It verifies the download before opening Android's standard update screen. Android still requires user confirmation, but the existing app does not need to be uninstalled and local settings are preserved.

The APK is published as a GitHub Release asset. APKs, signing secrets, and credentials are never committed to Git history.
