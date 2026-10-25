# Lumgate releases

Signed release binaries for **Lumgate**, a macOS menu-bar app. This repository contains **no source code**:
each release only carries a zipped, code-signed `.app` as a release asset.

## How the app trusts these files

You do not have to trust this repository, GitHub, or the network. The app installs an update only when all of
these checks pass:

1. **Signed manifest.** The app fetches a small manifest (version, build, download URL, SHA-256, size) that must
   carry a valid **Ed25519 signature** from the release key pinned inside the app. The private key never leaves
   the publisher's machine.
2. **Exact file.** The downloaded zip must match the SHA-256 and size in the signed manifest, and must come from
   this repository's release assets over HTTPS.
3. **Same signer.** The unpacked app must have the expected bundle identifier and build number, and its Apple code
   signature must satisfy the running app's designated requirement (same Developer ID team). When the installed
   app is notarized, the update must be notarized by Apple too.
4. **No downgrades.** The app never installs a build older than, or equal to, one it has already run.
5. **Your choice.** The app asks before installing (Update now / Later / Skip this version) and keeps the previous
   version until the new one has started successfully.

A file uploaded here without the release key's signature is ignored by the app.

## Manual install

Download the latest `.zip` from Releases, unzip it, and move the app to `/Applications`. You can check the
signature with:

```sh
codesign --verify --deep --strict --verbose=2 /Applications/Lumgate.app
spctl --assess --type exec --verbose /Applications/Lumgate.app
```

## Reporting a security issue

Please report vulnerabilities privately through GitHub's "Report a vulnerability" (Security tab) on this
repository rather than in a public issue.
