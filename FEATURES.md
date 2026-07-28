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

The selected theme is remembered after the app closes. If a test is running, a new theme is saved and applied the next time Speedtest+ opens.

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

The official network test, transferred data, remote submission, advertisements, and public Speedtest report remain measured and separate from the customized local presentation.

## Troubleshooting

**A value did not change**

Open the controls again, confirm the field is valid, and tap **APPLY** before starting the test.

**The app says it is up to date**

The published version code is not higher than the installed version. This is expected on the latest build.

**Android blocks an update**

Allow this app to install unknown apps when Android requests it, then retry. Android always controls the final installation confirmation.
