# Vaultbridge for Windows

Vaultbridge lets your browser fill in logins from your Keeper vault. A small Windows
app talks to Keeper, and a browser extension (Chrome or Edge) asks the app for the
right login when you open a sign-in page. The extension never sees your vault
directly; everything goes through the app.

## Install

1. Go to **Releases** on this page and download the newest `Vaultbridge-x.y.z-setup.exe`.
2. Run it. No administrator rights are needed.
3. Start Vaultbridge from the Start menu.
4. Load the browser extension by hand: open `chrome://extensions` (Edge:
   `edge://extensions`), turn on **Developer mode**, click **Load unpacked**, and pick
   the folder `venster\pagina` inside the install folder `%LOCALAPPDATA%\Kluis`.

Keeper Commander, which the app uses to reach your vault, needs a 64-bit Python.
On a Windows on ARM PC, install the 64-bit (x64) version of Python.

## Updates

Vaultbridge checks for new versions itself and offers them from the app. You do not
need to download a new setup for each update.

## Windows warning

The releases are not signed with a code-signing certificate yet, so Windows SmartScreen
shows **"Windows protected your PC"** the first time. Click **More info**, then **Run
anyway**. Every release is a new setup file, so you do this once per release.

## Questions and problems

Open an issue on this repository. The app's log file is `%LOCALAPPDATA%\Kluis\Logs\dienst.log`
(paste that path into the address bar of File Explorer); please include the lines around the time of the problem, with any
usernames or addresses removed.
