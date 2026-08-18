# Speedtest+ Android 1.8.12 security and privacy audit

Audit date: 2026-08-18  
Audited release: Android 1.8.12 Bug Doctor  
Audit type: independent static inspection of the published APK and public GitHub materials

## Executive summary

This inspection found no evidence that the custom Speedtest+ code reads SMS messages, reads call history, collects contacts, captures the camera or microphone, steals banking information, or silently installs software.

The custom Speedtest+ telemetry is comparatively small, off by default, and limited to six allowlisted coarse events. However, the complete APK is not telemetry-free. It contains a large inherited Ookla application layer and third-party SDKs, including CellRebel, comScore, Firebase and Google components, Mapbox, install-referrer support, background reporting, and advertising libraries. Some of those components are disabled in this build, while others remain conditionally active.

The most important issues are transparency and build hygiene rather than evidence of malware:

1. The public source kit does not contain the actual Android implementations of several custom Speedtest+ classes, and its telemetry contract does not match the published APK.
2. The APK contains two nonstandard duplicate DEX artifacts that should not be shipped.
3. The APK is signed with a certificate whose identity is explicitly `Debug`.
4. The inherited CellRebel SDK can conditionally observe current call-state transitions and submit voice-call network-quality metrics.
5. Cleartext network traffic is allowed globally.
6. Several unused analytics and advertising SDKs remain bundled even where their Android components are disabled.

These findings do not establish that the application is a virus. They do establish that the project should improve source-to-binary verifiability, remove inherited telemetry that is not required, and reduce the APK's permission and dependency surface.

## Artifact identity

| Property | Audited value |
|---|---|
| Release | [Android 1.8.12 Bug Doctor](https://github.com/Csiklaoliver/speedtest-plus-docs/releases/tag/android-v1.8.12-bug-doctor) |
| File | `SpeedtestPlus.apk` |
| Package | `org.zwanoo.android.speedtest` |
| Version name | `1.8.12` |
| Version code | `258552` |
| File size | `32,653,410` bytes |
| SHA-256 | `ae84d01b9ce922ff2ce778884002aa22e5a38e0b3520d8355b502bf96b3ddb5a` |
| Minimum Android version | Android 7.0, API 24 |
| Target SDK | API 35 |
| Signing schemes | APK Signature Scheme v2 and v3 |
| Certificate subject | `CN=Debug, OU=Debug, O=Debug, L=Debug, ST=Debug, C=US` |
| Certificate SHA-256 | `12650506207293a6def641c60648bb776f0db13b0d69f87e3c3e7b9a60e8065d` |

The downloaded release APK matched the SHA-256 published for that release. This proves that the inspected file is the same binary as the published release asset. It does not, by itself, prove that the APK was reproducibly built from the public source repository.

## Scope and limitations

The audit covered:

- APK structure and ZIP integrity
- signing certificate and APK signing schemes
- Android manifest permissions, components, providers, receivers, activities, and services
- near-complete Java and DEX decompilation
- custom Speedtest+ code paths
- third-party SDK inventory
- native library inventory
- statically discoverable URLs and endpoints
- telemetry payload construction
- update download and installation logic
- Bug Doctor launch behavior
- current-call-state handling
- comparison with the public source and documentation repositories

This was a static audit. It was not a live packet capture, penetration test, audit of private servers, or audit of the current Google Play version of Ookla Speedtest. Conditional behavior controlled by remote configuration requires a runtime network test to determine exactly which paths execute on a particular device at a particular time.

Static presence of an SDK or URL proves capability and code surface, not that every bundled service is active or transmitting.

## Findings summary

| ID | Priority | Finding | Result |
|---|---|---|---|
| STP-001 | High | Public source does not reproduce the custom APK implementation | Transparency gap |
| STP-002 | High | Inherited CellRebel call-state and network-quality telemetry remains present | Privacy reduction recommended |
| STP-003 | High | Two duplicate nonstandard DEX artifacts are packaged | Remove before next release |
| STP-004 | Medium | Release certificate identifies itself as Debug | Replace with dedicated release identity |
| STP-005 | Medium | Cleartext traffic is globally permitted | Restrict to required hosts |
| STP-006 | Medium | Unused analytics and advertising SDK code remains bundled | Remove unused dependencies |
| STP-007 | Medium | Broad inherited permissions and components remain | Prune and document |
| STP-008 | Low | CellRebel phone-state receiver is exported without a component permission | Remove or protect receiver |
| STP-009 | Low | Custom telemetry method relies on consent checks in its callers | Add defense-in-depth check |
| STP-010 | Informational | Bug Doctor server behavior is not independently verifiable from public source | Publish protocol and retention details |

## STP-001: Public source and APK mismatch

The [Speedtest+ source repository](https://github.com/Csiklaoliver/speedtest-plus-source) describes itself as a source kit and intentionally excludes Ookla proprietary code. That limitation is understandable. However, the APK also contains original Speedtest+ implementations that are not present in the public repository, including the telemetry, hub, updater, controls, and state classes.

The public telemetry contract also differs from the published binary:

| Public contract | Published Android APK |
|---|---|
| Placeholder server `speedtest.example.invalid` | `https://speedtest.oliverprojects.tech` |
| `POST /v1/events` | `POST /api/events` |
| Batched wrapper and schema version | Single JSON event object |
| UUID event ID and occurrence timestamp | No event ID or occurrence timestamp in the custom payload |
| Publicly described field schema | `consent`, `event`, `appVersion`, `androidVersion`, `deviceClass`, `networkClass`, and `theme` |

This means the public repository cannot currently be used to verify the exact custom behavior of the release APK.

Recommended action:

- Publish every original Speedtest+ Android implementation that can legally be published.
- Make the documented API contract match the production wire format exactly.
- Publish the exact patching and packaging steps.
- Publish an SBOM and a manifest of expected DEX files, native libraries, permissions, and endpoints.
- Document which portions cannot be published and why.

## STP-002: CellRebel current-call-state telemetry

The inherited Ookla layer contains the CellRebel SDK. Ookla [announced its acquisition of CellRebel in 2022](https://www.businesswire.com/news/home/20220727005067/en/Ookla-Acquires-CellRebel).

The APK's CellRebel receiver listens for Android phone-state changes such as `RINGING`, `OFFHOOK`, and `IDLE`. When the relevant remotely supplied CellRebel setting is enabled, the SDK can record call start and end times and submit voice-call network-quality metrics to the default endpoint:

`https://metricreceiver.cellrebel.com/mobile/voice_call_metrics`

The associated model exposes a broad set of possible radio, cell, carrier, device, network, IP, and location fields. Whether an individual field is populated depends on permissions, Android restrictions, SDK settings, and remote configuration.

Important distinction:

- The APK does not request `READ_CALL_LOG`.
- No code path was found that queries the Android call-history provider.
- No SMS-reading permission or SMS-content query was found.
- On modern Android, `READ_PHONE_STATE` does not grant access to historical call records or unrestricted phone numbers.
- The inherited SDK can still observe current call-state transitions and calculate call duration for network-quality measurements.

Therefore, saying that the application reads call history is inaccurate. Saying that the inherited CellRebel layer may observe current call state and duration is accurate.

Recommended action:

- Remove CellRebel if it is not necessary for Speedtest+ functionality.
- Remove its receiver, workers, endpoint configuration, and initialization path.
- Remove `READ_PHONE_STATE` and background location if no remaining feature requires them.
- Until removal, disclose the exact trigger, fields, retention, legal basis, and opt-out behavior.

## STP-003: Duplicate nonstandard DEX artifacts

The APK contains the normal `classes.dex` through `classes7.dex` set, plus these nonstandard valid DEX files:

| File | Size | SHA-256 |
|---|---:|---|
| `controls-classes6.dex` | 4,197,036 bytes | `ed6ce7ba5f64ab8484abb01781f961f30fab9cff01806f4ba0e58c6b990c0aff` |
| `controls-health-classes6.dex` | 4,200,592 bytes | `b07eb83ae4bbe26f48bfb658e71e921c2e70d8536af29020aaf52f62473db628` |

They contain duplicate older versions of custom classes. Android does not automatically place arbitrarily named DEX files on the normal application class path, and no custom dynamic-loader path for these artifacts was found. Nevertheless, malware scanners will inspect them as executable bytecode, and their presence makes the release harder to explain or reproduce.

Recommended action:

- Remove both artifacts from packaging.
- Add a build check that permits only the expected `classes*.dex` files.
- Fail release CI when duplicate class definitions or unexpected executable files are detected.

## STP-004: Development signing identity

The release is signed with a certificate whose subject and issuer use `Debug` for every identity field. The APK is not marked debuggable or test-only, and a Debug certificate name does not prove that the private key is public or compromised. It is still a supply-chain credibility problem for a broadly distributed release.

Recommended action:

- Generate and secure a dedicated Speedtest+ release key.
- Protect it with encrypted backups and restricted CI access.
- Publish only the public certificate fingerprint.
- Document the migration because Android will not accept an in-place update signed by an unrelated key without a supported signing-key transition.
- Keep the documentation consistent about whether a build is development-signed or production-signed.

## STP-005: Global cleartext traffic

The application permits cleartext HTTP traffic globally through its network-security configuration. This may exist to support legacy speed-test servers, but the scope is broader than necessary and creates scanner warnings.

Recommended action:

- Default to `cleartextTrafficPermitted="false"`.
- If a required speed-test protocol cannot use TLS, allow only the minimum documented destinations.
- Do not permit cleartext traffic for update, telemetry, report, or account endpoints.

## STP-006: Inherited telemetry and advertising surface

The complete APK includes substantial inherited code from:

- CellRebel
- comScore, including a native `libcomScore.so`
- Firebase and Google Analytics-related components
- Google Mobile Ads
- Amazon advertising and APS
- Mapbox, including its native maps libraries and events endpoint code
- install-referrer support
- background-reporting managers
- Ookla analytics and server-selection reporting
- OneTrust consent-management components
- ZDBB-related reporting code

Several advertising activities, providers, and services are explicitly disabled in the manifest. Firebase Analytics and Crashlytics automatic collection are also disabled through manifest metadata. Their disabled state matters, but disabled components and bundled code are not the same as removing the dependency.

The inherited original Speedtest layer has a much larger telemetry and reporting surface than the custom Speedtest+ additions. This is a statement about the audited code surface. It is not proof that every service is active, and it is not sufficient by itself to label the official application spyware.

The full Speedtest+ APK still includes this inherited layer. Therefore, the current package should not be described as telemetry-free even though the custom additions are comparatively limited.

Recommended action:

- Remove unused SDK bytecode, resources, native libraries, manifest entries, and initialization code rather than only disabling visible components.
- Produce a dependency and endpoint inventory for every release.
- Perform runtime traffic captures with analytics disabled and enabled.

## STP-007: Permissions and components

Notable requested permissions include:

- `INTERNET`
- `ACCESS_NETWORK_STATE`
- `ACCESS_WIFI_STATE`
- `READ_PHONE_STATE`
- `READ_BASIC_PHONE_STATE`
- coarse, fine, and background location
- `RECEIVE_BOOT_COMPLETED`
- foreground service and data-sync foreground service
- `WAKE_LOCK`
- notifications
- billing
- `REQUEST_INSTALL_PACKAGES`
- legacy external-storage permissions
- biometric and fingerprint permissions

The following high-risk permissions are not requested:

- `READ_CALL_LOG`
- `WRITE_CALL_LOG`
- `READ_SMS`
- `READ_CONTACTS`
- camera
- microphone recording
- accessibility service binding

The absence of those permissions is strong evidence against claims that this build directly reads call history, SMS content, contacts, camera data, microphone audio, or banking interfaces through an accessibility service.

Recommended action:

- Remove every permission not required by a documented active feature.
- Explain remaining permissions in an in-app privacy screen.
- Add a CI manifest-diff check to prevent permissions from returning unnoticed.

## STP-008: Exported CellRebel receiver

The CellRebel `PhoneStateReceiver` is exported and does not declare a component-level permission. Android and platform restrictions still limit what a spoofed broadcast can accomplish, but an unprotected exported receiver creates avoidable attack and scanner surface.

Recommended action:

- Remove the receiver with the SDK.
- If it must remain, set it non-exported unless an external sender is strictly required, or protect it with an appropriate permission and validate every incoming intent.

## STP-009: Custom Speedtest+ telemetry

The custom telemetry class accepts only these allowlisted events:

- `controls_opened`
- `hub_opened`
- `bug_report_opened`
- `theme_code_exported`
- `theme_code_imported`
- `installation_reported`

The constructed JSON contains:

- `consent`
- event name
- app version
- Android version bucket
- device class, phone or tablet
- network class, such as cellular, Wi-Fi, Ethernet, offline, or unknown
- coarse theme category

No custom payload field was found for:

- installation ID
- advertising ID
- account ID
- phone number
- call history
- SMS content
- contacts
- exact location
- exact speed result
- selected test server
- ISP name
- hardware serial, IMEI, MEID, or IMSI

The event endpoint is:

`https://speedtest.oliverprojects.tech/api/events`

Telemetry is off by default. Current call sites check the user's analytics opt-in before recording an event. The public telemetry method itself does not repeat that check internally, so future code could accidentally bypass consent if it calls the method directly.

Recommended action:

- Enforce consent again inside the lowest-level send method.
- Publish the exact implementation and server-side retention rules.
- Add automated tests proving zero custom telemetry requests before consent.

Normal HTTPS operation reveals transport metadata such as the source IP address to the receiving server even when the IP address is not included as a JSON field.

## Updater analysis

The updater requests Android's unknown-source installation capability, which will attract scanner attention. The inspected implementation does not silently install an APK.

It performs these checks:

- fixed HTTPS GitHub Raw manifest location
- HTTPS-only redirects and APK URL
- manifest and APK size limits
- stable-channel validation
- strict version increase
- exact expected file size
- SHA-256 verification
- package-name verification
- version verification
- signer-fingerprint verification
- Android's own package and signature checks
- explicit user confirmation in Android's installer

The manifest endpoint is:

`https://raw.githubusercontent.com/Csiklaoliver/speedtest-plus-docs/main/ota/manifest.json`

The mutable manifest is not separately signed, but an attacker who changed only the manifest would still need an APK signed by an accepted application signing key to pass the final verification.

Recommended action:

- Publish the updater implementation.
- Use a dedicated release key.
- Consider removing `REQUEST_INSTALL_PACKAGES` and opening the GitHub release page instead if in-app updates are not essential.

## Bug Doctor analysis

The active Android class records the coarse opt-in event `bug_report_opened` and opens this HTTPS page after an explicit user action:

`https://speedtest.oliverprojects.tech/report`

No application-side screenshot capture or automatic screenshot upload was found in that class. The public repositories do not contain the complete report website and server implementation, so server-side processing, retention, and deletion behavior were outside this audit's verifiable scope.

Recommended action:

- Open-source the report protocol and relevant server implementation where possible.
- Show the exact report content before submission.
- Publish screenshot, report, log, and IP-retention periods.
- Provide a deletion contact or automated deletion mechanism.

## Other custom network behavior

The custom connection-health function performs:

- a DNS lookup for `www.speedtest.net`
- an HTTPS range request to `https://www.speedtest.net/speedtest-config.php`

No custom identifier payload is constructed for that health check.

The custom theme importer validates a small color-only schema with size and integrity limits. It does not accept URLs or executable content. Export writes a user-selected theme code to the clipboard; the inspected custom implementation does not enumerate arbitrary clipboard history.

Result sharing constructs user-triggered text from the visible result and Android's normal sharing flow. No custom network submission occurs in the result-share builder itself.

## Common scanner findings explained

### ContentResolver and parsed URI

The frequently photographed `ContentResolver` finding comes from the Glide image library resolving Android MediaStore images, videos, thumbnails, and contact-photo URIs. It is not evidence of an SMS or call-log query. The application also lacks the permissions needed to freely enumerate contacts or call history.

### Starting background services

The shown Google classes are Analytics and Tag Manager library code that starts an analytics service. Starting a service is a generic Android capability and does not establish malicious behavior. Whether a service is enabled, initialized, and transmitting must be evaluated separately.

### Root checks

Strings such as `/system/xbin/su`, `/system/bin/su`, and `Superuser.apk` are present in inherited Crashlytics and comScore environment-detection code. Root detection is used by many crash, fraud, integrity, and analytics SDKs. It is not proof of malware defense evasion, although unnecessary root-detection code should still be removed with unused SDKs.

### Advertising classes

Google and Amazon advertising classes are bundled. Several related Android components are disabled in this build. Static scanners correctly identify the code's presence but cannot infer from presence alone that an advertisement request was made during a particular session.

## Claims supported by this audit

The evidence supports these statements:

- The exact inspected release does not request permission to read SMS or call history.
- No custom Speedtest+ code path was found that steals banking information or credentials.
- The custom Speedtest+ event telemetry is opt-in and comparatively coarse.
- The inherited original Speedtest layer contains a substantially larger telemetry capability than the custom Speedtest+ additions.
- The current Speedtest+ APK still contains some of that inherited telemetry.
- CellRebel can conditionally observe current call state and duration, but this is not call-history access.
- The updater is user-confirmed and performs multiple package, hash, version, and signer checks.
- Generic static-scanner API categories are not proof of malicious use.

The evidence does not support these absolute statements:

- Every possible behavior of the app or its servers has been proven harmless.
- The APK contains no telemetry.
- The inherited official application is conclusively spyware.
- Every bundled analytics or advertising SDK is active.
- A matching published APK hash proves a reproducible source build.

## Recommended release plan

Before the next public Android release:

1. Publish the complete original Speedtest+ Android implementation and synchronize the API contract.
2. Remove the two duplicate nonstandard DEX files.
3. Remove CellRebel and inherited background measurement not required by Speedtest+.
4. Remove unused analytics, advertising, install-referrer, and consent SDKs from the binary.
5. Prune permissions, exported components, services, providers, and receivers.
6. Restrict or remove global cleartext traffic.
7. Move to a dedicated protected release-signing identity.
8. Add CI checks for permissions, endpoints, unexpected DEX files, duplicate classes, signer identity, and reproducible hashes.
9. Publish an SBOM and a complete endpoint matrix for each release.
10. Perform and publish a controlled runtime network capture with custom analytics both disabled and enabled.

## Reporting corrections or vulnerabilities

This document records the behavior of the identified Android 1.8.12 APK. Future releases may differ.

For an ordinary factual correction, open a GitHub issue with the exact class, method, APK version, payload, endpoint, and reproduction steps. Do not treat an automated category label by itself as proof.

For a vulnerability or sensitive report, use GitHub's private vulnerability reporting feature in the repository's Security tab. Never publish credentials, signing keys, private URLs, personal data, or exploit details that would put users at immediate risk.

