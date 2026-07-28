# Security and privacy

## Privacy

Speedtest+ profiles, themes, and override settings are stored in the app's private local preferences.

The update checker contacts GitHub Raw for the manifest and GitHub Releases for APK downloads. Those requests expose normal network metadata such as the device IP address and user agent. Override values, profile names, and theme settings are not added to update requests.

The underlying official app has its own network behavior, privacy terms, test submission, advertising, and account features. Those systems are separate from the Speedtest+ update checker.

## Public information

The following information is intentionally public:

- documentation and manifest files
- signed APK Release assets
- APK file size and SHA-256
- release tags and notes
- the public signing certificate contained in every signed APK

An APK's public certificate and file hash are not private signing keys.

## Private information

Never publish:

- a private signing key or keystore
- signing passwords, aliases, or configuration
- GitHub tokens or other credentials
- personal email addresses or local filesystem paths
- temporary signed URLs
- unrelated personal data

Private signing material is required for future in-place updates, so it should be stored securely outside the repository and backed up privately. Deleting it would prevent future OTA updates.

The current baseline is development-signed. Its public certificate identifies compatible updates but does not represent a hardened production release process.

## Update security

The updater uses multiple checks:

- HTTPS-only manifest and APK downloads
- strict version increase
- response and file-size limits
- exact size and SHA-256 verification
- package and version verification
- downloaded signer comparison with the installed app
- Android's independent package and signature verification
- explicit user confirmation

The updater never silently installs an APK and never dynamically loads downloaded code.

## Reporting a security problem

Do not post a key, credential, private URL, personal data, or exploitable update detail in a public issue.

Use GitHub's private vulnerability reporting option in the repository's **Security** tab. Include the affected version, Android version, reproduction steps, and impact without attaching secrets.

If confidential information is accidentally published, revoke or rotate it immediately. Removing the newest commit alone may not remove it from Git history, pull-request refs, forks, caches, or clones.

## Responsible use

Customized local values are intended for personal testing and clearly identified demonstrations. Do not use them to misrepresent measured network performance.
