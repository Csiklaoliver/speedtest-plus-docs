# Releases and updates

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

The current baseline uses version code `258534`. The next OTA build must use `258535` or greater.

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
  "versionCode": 258535,
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
