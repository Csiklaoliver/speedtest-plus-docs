# Features

## Opening the controls

On the Speed screen, open the provider drawer and either:

- tap the info button, then **OPEN CONTROLS**
- long-press the provider row

Normal provider and server taps still use the original selection screens.

## Values

| Control | Accepted value |
|---|---|
| Download | Non-negative minimum and maximum Mbps |
| Upload | Non-negative minimum and maximum Mbps |
| Ping | `0` to `9999` ms |
| Jitter | `0` to `9999` ms |
| Packet loss | `0.0` to `100.0` percent |
| ISP | Up to 64 characters |
| Server provider | Up to 64 characters |
| Server location | Up to 64 characters |

Rules:

- Blank fields keep the measured value.
- A speed minimum must not exceed its maximum.
- Equal speed bounds create an exact result.
- Different speed bounds create a varied result inside the range.
- Invalid fields are ignored with a message instead of crashing the app.
- **APPLY** activates the visible values.
- **CLOSE** leaves the active configuration unchanged.
- **DISABLE ALL** returns new tests to measured values.

## Profiles

Three profile slots can store metric and identity controls. Each profile can have a name from 1 to 24 characters.

- **SAVE** stores the values currently shown in the controls.
- **LOAD** fills the controls without activating them.
- Tap **APPLY** after loading a profile.
- **DELETE** removes the selected profile after confirmation.

Themes are global and are not stored inside profiles.

## Themes

The available themes are:

- Classic Cyan
- AMOLED High Contrast
- Midnight Violet
- Emerald
- Sunset
- Deep Ocean
- Neon Cyber
- Ruby Noir
- Arctic Night
- Solar Gold

The selected theme is remembered after the app closes and survives in-place updates. If a test is running, a new theme is saved and applied the next time Speedtest+ opens.

## Active badge and guide

The provider row shows `CUSTOM - N` only while overrides are active. It stays hidden when the active count is zero. `N` counts the active metric and identity categories.

The info button reopens the guide at any time. The guide explains blank fields, exact values, ranges, profiles, applying values, and disabling overrides.

## Consistent local results

For newly completed customized tests, the final local values are reused across:

- the live and completed test screen
- local result details and history
- Compare Your Speed
- provider-aware feedback questions
- local text sharing
- CSV exports

Old saved tests are not changed.

## What remains measured

The official network test, transferred data, official remote submission, advertisements, and public Speedtest report remain measured and separate from the customized saved-result/history representation. Signed-in account history may store that customized representation where enabled, but it does not replace the official measured test report.

## Update prompts

The original vendor update prompt is hidden so it cannot direct this customized build to an incompatible official release. Speedtest+ OTA remains active under **Updates** in **Speedtest+ Controls** and continues to verify each download before opening Android's update screen.

## Troubleshooting

**A value did not change**

Open the controls again, confirm the field is valid, and tap **APPLY** before starting the test.

**The app says it is up to date**

The published version code is not higher than the installed version. This is expected on the latest build.

**Android blocks an update**

Allow this app to install unknown apps when Android requests it, then retry. Android always controls the final installation confirmation.

**A manually selected server does not connect**

Open the provider drawer, tap **Change Test Server**, and choose another nearby server. The server list and manual selection path are supported, but each listed host is operated independently and an individual host can be offline or reject a connection.

**The Video test fails**

Retry after checking the connection and updating the device's media components. Speedtest+ first uses the normal service-provided Video configuration. If that configuration is absent, version 1.3.2 and newer use the Video SDK's bundled production fallback. Playback still requires access to the remote Video asset and a compatible H.264 decoder; failures remain non-fatal.

## Compatibility

Speedtest+ supports Android 7.0 and newer on 64-bit ARM (`arm64-v8a`) devices. The APK is aligned for Android devices using 16 KB memory pages. The original test engine does not ship 32-bit ARM or x86 native libraries, so those architectures cannot be added safely by the modification layer.
