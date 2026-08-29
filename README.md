# Vaultbridge — releases

Downloads for Vaultbridge, and the manifest its built-in updater reads.

The source code is not here. This repository exists so an installed app can ask
"is there a newer version?" without credentials, while the source repository stays
private — so the binaries live apart from the code.

Vaultbridge is one thing in three parts, and they belong to the same version:

| | what it is |
|---|---|
| **Kluis.app** (macOS) | the app that talks to the local Keeper service |
| **Kluis** (Windows) | the same app, as a WebView2 shell around the same window |
| **the Chrome extension** | never talks to the vault, only to the app |

The extension and the app speak a bridge protocol that can change between versions.
A new extension against an old app gives "unknown action"; the other way round you
get a half-working window. Take them together.

## Installing on macOS

Open `Kluis-<version>.dmg`, then **drag Kluis to Applications and start it there**.
Not from the disk image: the path of a mounted image disappears when you eject it,
and the browser would then start a helper that no longer exists. Kluis tells you if
you forget.

The first time: right-click Kluis → Open → Open. That's needed once, and the reason
is below.

There is nothing else to register. On every start, Kluis writes its host manifest to
every Chromium browser on the Mac — Chrome, Brave, Edge, Vivaldi, Arc, Dia, Chromium.
Install another browser later and starting Kluis once is enough.

The extension is not in the Chrome Web Store: unpack
`vaultbridge-extensie-<version>.zip` and load it via `chrome://extensions` → Developer
mode → "Load unpacked". The extension ID is fixed, so this is a one-time step.

## Installing on Windows

Run `Kluis-<version>-setup.exe`. SmartScreen will warn: choose More info → Run anyway.
The installer registers the host for Chrome, Edge and Brave, and cleans up any
registration from an earlier version. Load the extension the same way as on macOS.

## These builds are not signed, and here that matters more than usual

None of these builds is code-signed for distribution: on macOS there is an *Apple
Development* certificate and no Developer ID, so no notarisation; on Windows there is
nothing. Gatekeeper and SmartScreen will warn you — and, this is the point, **you
cannot tell a genuine build from a modified one.**

For a browser that would be annoying. For this program it is not: it reads your
entire vault. A swapped download is not "a weird browser", it is someone else reading
your passwords. So do not pass these builds to anyone outside the two people building
them, until there is a Developer ID and an Authenticode certificate.

## latest.json

The updater fetches
[`latest.json`](https://raw.githubusercontent.com/qntn-dev/vaultbridge-releases/main/latest.json)
and compares `versie` with its own. Per platform it holds the download URL, the size
and a SHA-256, so a truncated or swapped download is caught before anything runs.

That hash proves integrity, not authorship: it travels the same path as the file it
describes. Only signing fixes that, and it isn't there yet — see above.

The format is described in [docs/manifest.md](docs/manifest.md) (in Dutch, like the
rest of the project's internal docs).
