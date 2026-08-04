# Releases and updates

This repository is the canonical public release destination for Speedtest+.
Final Android and iOS artifacts, checksums, download links, and OTA manifest
updates belong here. The source repository is reserved for source code, tests,
build instructions, and CI artifacts.

## Speedtest+ Android 1.8.9 gauge/server QA

This debug-signed QA build addresses the reports collected from Discord and
TikTok:

- waits for the native 900 ms gauge-opening sequence before the first custom
  frame, so the number, blue leading arc, gray trailing bar, and needle start
  together
- keeps one selected target per download/upload phase instead of jumping
  between callback values
- refreshes server page zero after connectivity changes, including Redmi
  Wi-Fi/mobile transitions
- keeps the fixed 1k gauge and cancels queued custom frames when controls are
  disabled

This is a clean-install QA artifact, not a production OTA update. Its Android
debug certificate is intentionally different from the stable 1.4.8 production
certificate, so Android will reject it as an in-place update. The stable OTA
manifest remains unchanged until a matching production signature is supplied.

- [Download SpeedtestPlus_1.8.9_animfix_debugsigned.apk](https://github.com/Csiklaoliver/speedtest-plus-docs/releases/download/android-v1.8.9-qa/SpeedtestPlus_1.8.9_animfix_debugsigned.apk)
- SHA-256: `A3ED7D9CBB04AC2CD0129014C044188AD5E2A6817983B3DFA115BCAF50E261BB`
- Size: `29,859,938` bytes

## Speedtest+ iOS 0.1.10 controls rebind

This unsigned maintenance build repairs the iOS provider-row rebuild path:

- rechecks the current provider row during layout instead of trusting a stale
  button tag
- reattaches the info button, long-press gesture, and invisible 48 pt hotspot
  after server/provider rows are replaced
- keeps the native provider and server selection controls untouched

The published IPA is unsigned. Sign it with your own legitimate sideloading
method before installing:

- [Download SpeedtestPlus_iOS_0.1.10_CONTROLS_REBIND_UNSIGNED.ipa](https://github.com/Csiklaoliver/speedtest-plus-docs/releases/download/ios-v0.1.10-controls-rebind/SpeedtestPlus_iOS_0.1.10_CONTROLS_REBIND_UNSIGNED.ipa)
- SHA-256: `DD2D4BDF89AD5BE1146E8327138B39018030DC5F8BF61BB815ECBEFFF55654E0`
- Size: `47,892,831` bytes

## Speedtest+ iOS 0.1.9 controls reliability

This unsigned maintenance build is the current recommended iOS build:

- rebinds the private provider-row entry point after all ISP/server subviews
  are rebuilt, including provider and location labels
- keeps the info button beside the ISP row and preserves native server
  selection; no floating S+ controller is added
- keeps long-press and the optional password lock working when the provider
  row is rebuilt
- applies runtime-safe Liquid Glass to the custom controls surface and keeps
  its theme tint synchronized, with older-iOS and Reduce Transparency
  fallbacks

The published IPA is unsigned. Sign it with your own legitimate sideloading
method before installing:

- [Download SpeedtestPlus_iOS_0.1.9_CONTROLS_GLASS_UNSIGNED.ipa](https://github.com/Csiklaoliver/speedtest-plus-docs/releases/download/ios-v0.1.9-controls-glass/SpeedtestPlus_iOS_0.1.9_CONTROLS_GLASS_UNSIGNED.ipa)
- SHA-256: `de4205b339c80c9357f391d156a59c4f0c8b03fc777dfc120a011ac159f50d23`
- Size: `47,891,917` bytes

## Speedtest+ iOS 0.1.8 controls reliability

This unsigned maintenance build contains the complete provider-row controls
fix from 0.1.7 plus live theme refresh:

- rebinds after the ISP view, ISP label, server view, provider label, and
  location label are assembled
- keeps the only Speedtest+ entry point beside the ISP row; no floating S+
  control is added and native server selection remains untouched
- keeps the 48-point info button and row long-press available for the optional
  password lock
- uses runtime-safe Liquid Glass material on the custom controls panel and
  refreshes its tint when a theme changes, with dark-blur/opaque fallbacks for
  older iOS and Reduce Transparency

The published IPA is unsigned. Sign it with your own legitimate sideloading
method before installing:

- [Download SpeedtestPlus_iOS_0.1.8_CONTROLS_GLASS_UNSIGNED.ipa](https://github.com/Csiklaoliver/speedtest-plus-docs/releases/download/ios-v0.1.8-controls-glass/SpeedtestPlus_iOS_0.1.8_CONTROLS_GLASS_UNSIGNED.ipa)
- SHA-256: `2beb6630231487e74f83f089890e832dd39f1665588c0e0eb9ce630d0f6f6469`
- Size: `47,891,739` bytes

## Speedtest+ iOS 0.1.7 controls reliability

This unsigned maintenance build uses the last working provider-row button as
its baseline and fixes the lazy-view rebuild path that could make controls
disappear or stop responding:

- rebinds after the ISP view, ISP label, server view, provider label, and
  location label are assembled
- keeps the only Speedtest+ entry point beside the ISP row; no floating S+
  control is added and native server selection remains untouched
- keeps the 48-point info button and row long-press available for the optional
  password lock
- adds runtime-safe Liquid Glass material to the custom controls panel, with a
  dark-blur/opaque fallback for older iOS and Reduce Transparency

The published IPA is unsigned. Sign it with your own legitimate sideloading
method before installing:

- [Download SpeedtestPlus_iOS_0.1.7_CONTROLS_GLASS_UNSIGNED.ipa](https://github.com/Csiklaoliver/speedtest-plus-docs/releases/download/ios-v0.1.7-controls-glass/SpeedtestPlus_iOS_0.1.7_CONTROLS_GLASS_UNSIGNED.ipa)
- SHA-256: `d8b63095aa60de8690423366c6ac47b4587d7bad3c9e14f326f9e1ea4d57f01d`
- Size: `47,891,693` bytes

## Speedtest+ iOS 0.1.6 controls hotfix

This maintenance build restores the reliable Speedtest+ controls entry path:

- keeps the custom info button visible when controls are password-protected so it can unlock the panel
- rebinds the button and provider-row gestures after provider views rebuild
- retries attachment during layout when the provider host is created late
- uses a provider-host fallback if a private ISP label accessor changes
- keeps the custom hotspot outside normal server-selection controls

The published IPA is unsigned. Sign it with your own legitimate sideloading method before installing:

- [Download SpeedtestPlus_iOS_0.1.6_CONTROLS_HOTFIX_UNSIGNED.ipa](https://github.com/Csiklaoliver/speedtest-plus-docs/releases/download/ios-v0.1.6-controls-hotfix/SpeedtestPlus_iOS_0.1.6_CONTROLS_HOTFIX_UNSIGNED.ipa)
- SHA-256: `90f8e4202747d44013287e6067172366196a278b2c593f0008ab2d60fe03bf15`
- Size: `47,887,707` bytes

## Speedtest+ iOS 0.1.5 ISP controls fix

This follow-up iOS build fixes reports that holding the ISP row did nothing:

- rebinds the long-press after provider views are rebuilt
- enables interaction on the provider label and row
- adds an invisible 48dp provider-row fallback hotspot that opens controls

The published IPA is unsigned. Sign it with your own legitimate sideloading method before installing:

- [Download SpeedtestPlus_iOS_0.1.5_ISP_ROW_CONTROLS_UNSIGNED.ipa](https://github.com/Csiklaoliver/speedtest-plus-docs/releases/download/ios-v0.1.5-isp-controls/SpeedtestPlus_iOS_0.1.5_ISP_ROW_CONTROLS_UNSIGNED.ipa)
- SHA-256: `e1b19c54a2cda786109dda0bccaa1e61ad8d546d1c498a7d20eee9b56ac3e46d`
- Size: `47,886,159` bytes

## Speedtest+ 1.4.8

This Android controls reliability release:

- gives every Speedtest+ input a clear keyboard Done action
- hides the software keyboard before Save, Load, Delete, Apply, Disable All, and Close
- keeps profile and metric edits usable on keyboards that do not dismiss automatically
- preserves the original blue-leading, gray-following gauge animation, fixed 1k gauge, ten themes, profiles, customized results, sharing, CSV, and OTA updates
- keeps the official provider/server selection and measured network engine unchanged

The signed Android APK is available here:

- [Download SpeedtestPlus.apk](https://github.com/Csiklaoliver/speedtest-plus-docs/releases/download/v1.4.8/SpeedtestPlus.apk)
- SHA-256: `ec85bd102ba4f9dbdd2cc7d5bbcdeddc7878549703e3b9f6e9148ae9ad5f0149`
- Size: `29,855,842` bytes

## Speedtest+ for iOS 0.1.5

This iOS controls-entry update:

- keeps the ISP/provider row as the only controls entry point
- opens controls with a long-press on the ISP/provider row
- places the 48-point info button beside the ISP name for the guide
- removes the floating and navigation-bar S+ controls
- includes the alert and keyboard presentation fixes from the previous maintenance build

The published IPA is unsigned. Sign it with your own legitimate sideloading method before installing:

- [Download SpeedtestPlus_iOS_0.1.5_ISP_ROW_ONLY_UNSIGNED.ipa](https://github.com/Csiklaoliver/speedtest-plus-docs/releases/download/ios-v0.1.5/SpeedtestPlus_iOS_0.1.5_ISP_ROW_ONLY_UNSIGNED.ipa)
- SHA-256: `f8f1373afc76c1c9a370b3480adaad40dbec30df438c0f81bed32653d13afbeb`
- Size: `47,874,667` bytes

## Speedtest+ for iOS 0.1.3

The iOS maintenance release:

- places the info icon beside the ISP name instead of floating over the Speed screen
- keeps the provider-row long-press shortcut and active override badge
- removes the legacy floating control during a live upgrade
- prevents duplicate `SpeedtestPlus.dylib` entries when an already-patched IPA is repackaged

The published IPA is unsigned. Sign it with your own legitimate sideloading method before installing:

- [Download SpeedtestPlus_iOS_0.1.3_UNSIGNED.ipa](https://github.com/Csiklaoliver/speedtest-plus-docs/releases/download/ios-v0.1.3/SpeedtestPlus_iOS_0.1.3_UNSIGNED.ipa)
- SHA-256: `de8f6755d4191b2b9342179ea0acdc5e4c9831a6747ee86436889dfc3b6b30c4`
- Size: `47,871,897` bytes

## Speedtest+ 1.4.7

This focused animation and speed-range release:

- restores both the Gauge renderer and its coordinator feed to the supplied original implementation
- restores the original blue-leading, gray-following gauge motion
- preserves the fixed 1k scale while allowing the displayed result to exceed the gauge
- raises the custom-speed ceiling from 100,000 Mbps to the saved-result model limit of 73,786,976,294,838 Mbps
- keeps finite, non-negative, ordered-range validation so invalid input cannot crash the app
- preserves the official server-selection and connection engine; a selected-server test completed successfully during verification
- passes an in-place Android install, cold start, v2/v3 signature verification, and 16 KB zip-alignment verification

## Speedtest+ 1.4.6

This is the verified replacement for 1.4.5. It contains the same Gauge-opening
and anonymous-installation-count changes, plus a corrected register type in the
new telemetry event mapper. The fix was validated by an in-place emulator
install, cold start, live process check, and a clean Android log without a
`VerifyError` or fatal startup exception.

## Speedtest+ 1.4.5 (superseded)

Do not install this intermediate build. Android's runtime verifier rejects the
first implementation of the new installation-event mapper. Version 1.4.6
contains the corrected implementation.

This animation and privacy-statistics release:

- restores `Gauge.setTargetFillPercentage()` to the exact supplied reference implementation
- moves the fixed-1k safety clamp into the live-speed coordinator so it cannot flatten the GO opening sweep
- keeps the moving colored edge/circle synchronized with the dark-gray opening arc
- sends one anonymous installation report only after explicit statistics consent
- marks a successful report locally so an installation is not counted again after an app update
- retries a failed report on a later initialization without keeping an event queue or identifier
- publishes only a thresholded rolling 90-day aggregate on the website and GitHub badge
- preserves profiles, themes, customized results, smooth live ramps, final values, and Android 7.0+ support

## Speedtest+ 1.4.4

This strict 1.3.0 animation-path release:

- restores the 1.3.0 gauge coordinator code exactly
- restores the 1.3.0 download/upload phase viewholder code exactly
- keeps custom state resets outside the GO, Connecting, and gauge-opening UI path
- begins custom motion only from the first native live-speed callback
- resets safely across direction changes, cancelled tests, and retests
- preserves the original Connecting, dark-arc, labels/needle, `0.00`, and live-value order
- keeps smooth five-second download/upload ramps and natural post-target drops
- preserves exact finalized results, the fixed 1k gauge, themes, profiles, no-ad layout, and local result overrides

## Speedtest+ 1.4.3

This native-animation correction release:

- removes the competing custom display timer that caused values to alternate between `1.00` and the configured speed
- returns gauge movement to the app's native frame callback so the needle, fill, and number stay synchronized
- matches the 1.3.0 opening order: Connecting, dark arc, labels and needle, `0.00`, then live readings
- keeps one animation reset at each native download and upload phase boundary
- ramps smoothly for five seconds and retains small natural drops after reaching the selected target
- preserves the exact finalized download and upload values in the completed result
- accepts custom speed values up to `73,786,976,294,838 Mbps`, the safe signed 64-bit result-model limit
- preserves the fixed 1k gauge, themes, profiles, no-ad layout, Video test fallback, and Android 7.0+ support

## Speedtest+ 1.4.2

This animation and result-consistency release:

- restores the supplied reference APK's complete Connecting-to-gauge transition
- reveals the gauge arc, scale, needle, unit, and `0.00` in their native order
- starts custom motion only after the opening sequence reaches its live-value handoff
- ramps custom download and upload readings smoothly from zero
- keeps small realistic dips after the selected target is reached instead of freezing
- chooses one randomized value per configured range and reuses it for the live target and saved result
- keeps equal minimum/maximum values exact
- preserves the fixed 1k gauge while safely clamping only its fill and needle
- retains the no-ad layout without the former empty native-ad space
- preserves profiles, themes, active controls, saved results, and Android 7.0+ support

## Speedtest+ 1.3.3

This compatibility and presentation release:

- removes banner and completed-result native ads
- collapses the former native-ad placeholder so no blank panel remains
- restores the complete GO-to-Connecting and dial-opening animation
- keeps the intentionally fixed 1k gauge and clamps only its needle/fill
- keeps the speed mode icon hidden until a real or custom value is ready
- checks for updates silently in the background when the main screen resumes
- throttles automatic checks to once per hour and prompts only for a newer verified build
- removes the Video screen's premature autoplay race
- uses a real single-rendition 720p H.264 Video test to avoid the adaptive-HLS crash in the bundled ExoPlayer on newer Android versions
- preserves active controls, profiles, themes, and saved settings during the in-place update
- retains Android 7.0+ support and 16 KB page alignment

## Speedtest+ 1.3.2

This maintenance release fixes the remaining test-start and Video issues:

- preserves the selected fixed 1k gauge instead of expanding it for custom values above 1,000 Mbps
- safely clamps the gauge needle and fill while still showing the exact larger custom value
- restores the animated GO-to-Connecting transition before the testing layout opens
- restores Video tests when the normal service supplies no Video configuration by using the Video SDK's bundled production fallback
- keeps normal service-provided Video configuration as the first choice
- completes adaptive and fixed Video stages and opens the Video result screen
- preserves active controls, profiles, themes, and saved settings during the in-place update
- retains Android 7.0+ support and 16 KB page alignment

## Speedtest+ 1.3.1

This maintenance release focuses on follower-reported test-screen and compatibility bugs:

- hides the value icon, unit, and `0.00` placeholder until the first real/custom speed is ready
- finalizes one custom speed target per direction so the displayed value no longer jumps during a test
- expands the gauge scale for custom values above 1,000 Mbps
- hardens HLS video transitions and prefers broadly supported H.264 playback
- keeps Video failures non-fatal and provides useful device troubleshooting
- verifies manual server selection and a full measured test against an alternate server
- preserves the unmodified measured path when all controls are disabled
- retains Android 7.0+ support and 16 KB page alignment for current Android devices

## For users

Install the first build from the [latest GitHub Release](https://github.com/Csiklaoliver/speedtest-plus-docs/releases/latest). After that:

1. Open **Speedtest+ Controls**.
2. Scroll to **Updates**.
3. Tap **CHECK FOR UPDATES**.
4. Accept the download if a newer build is available.
5. Approve Android's standard update screen.

You do not need to uninstall the current app. Profiles, themes, and active controls remain stored during an in-place update.

Android 8 or newer may first ask you to allow this app to install unknown apps. This permission is controlled by Android and can be revoked later.

## Version rules

An update is accepted only when:

- the package name is unchanged
- the new `versionCode` is greater than the installed value
- the APK is signed by the same identity as the installed app
- the download matches the published size and SHA-256
- the user approves Android's installation prompt

The current baseline uses version code `258540`. The next OTA build must use `258541` or greater.

## Publishing an update

Keep the private signing material outside this repository. Never place a key, password, token, certificate private key, or signing configuration in Git, a Release, an issue, or a pull request.

Release sequence:

1. Increase `versionCode` and set the new `versionName`.
2. Build, align, and sign the APK with the same existing signing identity.
3. Verify the APK package, version, alignment, and signature locally.
4. Name the release file `SpeedtestPlus.apk`.
5. Calculate its exact byte size and SHA-256.
6. Create a new GitHub Release and upload the APK.
7. Download the public asset and verify its size and SHA-256 again.
8. Update `ota/manifest.json` with the verified release data.
9. Publish the manifest last.
10. Test the update from the previous production build.

Example Release upload:

```powershell
gh release create v1.4.0 SpeedtestPlus.apk `
  --repo Csiklaoliver/speedtest-plus-docs `
  --title "Speedtest+ 1.4" `
  --notes "Signed update."
```

Use a new tag for every build. Never replace the bytes at an existing release URL.

## Manifest

Installed builds read:

```text
https://raw.githubusercontent.com/Csiklaoliver/speedtest-plus-docs/main/ota/manifest.json
```

The updater uses these fields:

| Field | Purpose |
|---|---|
| `schemaVersion` | Manifest format, currently `1` |
| `channel` | Update channel, currently `stable` |
| `packageName` | Required Android package |
| `versionCode` | Numeric update order |
| `versionName` | User-facing version |
| `apkUrl` | Public HTTPS Release asset |
| `sha256` | Exact APK SHA-256 |
| `sizeBytes` | Exact APK byte size |
| `notes` | Text shown for an available update |

Example:

```json
{
  "schemaVersion": 1,
  "channel": "stable",
  "packageName": "org.zwanoo.android.speedtest",
  "versionCode": 258538,
  "versionName": "1.4.0",
  "apkUrl": "https://github.com/Csiklaoliver/speedtest-plus-docs/releases/download/v1.4.0/SpeedtestPlus.apk",
  "sha256": "replace-with-the-lowercase-sha256",
  "sizeBytes": 12345678,
  "notes": "Small fixes and improvements."
}
```

## Verification performed by the app

Before opening the installer, the updater checks:

- HTTPS for the manifest, APK URL, and redirects
- manifest format and stable channel
- a strictly higher version code
- response and download size limits
- exact APK size and SHA-256
- exact package name and version
- the downloaded signer set against the installed signer set

The APK is downloaded to private app cache. It is not loaded into the running process. Android performs its own package and signature checks before showing the confirmation screen.

## Recovery

If a published manifest is wrong, restore the previous manifest immediately. Do not replace an already published APK. Correct the problem in a new build with a higher version code and a new Release tag.

Losing the accepted private signing key prevents future in-place updates. Keep it private, backed up securely, and separate from this public repository.

The current baseline is development-signed and intended for testing. Do not treat its signing setup as a hardened production release system.
