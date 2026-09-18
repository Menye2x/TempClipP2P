# Temporary Clipboard P2P

A lightweight, privacy-focused temporary clipboard web app for sending text between paired devices in real time.

The app is designed around a simple idea:

> Pair trusted devices, send text in either direction, and keep clipboard content only in browser RAM for the lifetime of the active session.

## Concept

Temporary Clipboard P2P creates a live browser-to-browser clipboard session.

One device starts the session and can display a short-lived QR pairing code. Another device scans the QR and joins the same live room. Once paired, both devices can send and receive text in either direction.

Clipboard content is intentionally ephemeral. The app does not deliberately persist clipboard text to:

- localStorage
- sessionStorage
- IndexedDB
- cookies
- a database
- cloud storage

Refreshing or closing the page destroys the local clipboard state in browser RAM.

The host also clears connected peers when the session ends or the host connection disappears.

## Main Features

- Two-way text synchronization between paired devices
- WebRTC DataChannel based clipboard transport
- QR pairing workflow
- Pairing QR expires after 30 seconds
- Pairing QR regenerates automatically
- Successful pairing immediately revokes the used pairing token
- Multiple linked devices can participate in one live session
- Send plain text, URLs, notes, and code snippets
- Copy clipboard items
- Delete individual items
- Clear all temporary clipboard items
- Remove a linked device
- End Session confirmation popup
- Refresh Session control
- Refresh acknowledgement popup
- Linked device status list
- RAM-only clipboard state
- Automatic clipboard clearing when the host connection closes
- Responsive desktop/mobile interface
- No account required by the app
- No app-specific API key required
- Footer attribution and creator link

## How It Works

1. Open the app on the first device.
2. The browser creates a live WebRTC peer session.
3. Click **+ Add** to open the pairing QR.
4. The QR contains:
   - the host peer identifier
   - a random one-time pairing token
5. The pairing token is valid for 30 seconds.
6. Scan the QR using the second device.
7. The second browser opens the same app URL with the temporary pairing information.
8. The host validates the pairing token.
9. If valid, the device is linked and the token is immediately revoked.
10. Both devices can now send text in either direction.
11. Clipboard items remain only in active browser memory.
12. Refreshing, closing, or ending the session clears the temporary clipboard state.

## Privacy Model

The clipboard itself is intentionally temporary.

The app does not use browser persistence APIs for clipboard content. Clipboard entries are stored in JavaScript memory only.

WebRTC DataChannel is used for peer clipboard traffic.

A public PeerJS signaling service is currently used only to help browsers discover and establish peer connections. Signaling infrastructure is not used by this app as a clipboard database.

Because this project uses public free infrastructure, availability and service limits may change.

## Important Browser Behavior

Closing or refreshing a tab destroys the JavaScript state for that device.

If the host refreshes or closes the page:

- the host clipboard is destroyed
- the WebRTC session ends
- connected clients detect the lost connection
- their temporary clipboard is cleared by the app

Browser shutdown, crashes, network loss, or power loss can prevent graceful unload events, so the app also reacts to the WebRTC connection closing.

## Running Locally

You can open `index.html` directly in a browser to inspect and test the interface.

However, cross-device QR pairing requires the same page URL to be reachable by both devices.

For practical cross-device use, serve `index.html` from:

- GitHub Pages
- another static HTTPS host
- or a LAN-accessible web server

### Simple local server

If Python is installed:

```bash
python -m http.server 8080
```

Then open:

```text
http://localhost:8080
```

For another device on the same LAN, use the computer's LAN IP address instead of `localhost`.

Some browser features and mobile environments work more reliably over HTTPS.

## GitHub Pages

This repository is static and can be hosted directly with GitHub Pages.

Typical setup:

1. Push the repository to GitHub.
2. Open the repository settings.
3. Open **Pages**.
4. Select the branch containing `index.html`.
5. Publish from the repository root.
6. Open the generated HTTPS URL on your devices.

## Project Structure

```text
temporary-clipboard-p2p/
├── index.html
├── README.md
└── .gitignore
```

## External Resources

The current single-file app loads:

- PeerJS from jsDelivr
- QRCode.js from jsDelivr

These are loaded at runtime from public CDN URLs.

## Current Scope

The project is intentionally focused on **text only**.

File transfer, image transfer, and persistent cloud clipboard storage are outside the current scope.

## Session Controls

### End Session

`End Session` displays a confirmation dialog before disconnecting devices and clearing the temporary clipboard.

### Refresh Session

`Refresh Session` reloads the application, destroys the current RAM state, starts a fresh session, and displays a **Successfully Refreshed** acknowledgement popup.

## Attribution

Created by [Menye2x](https://github.com/Menye2x)

©2026 Gaius Fridolf. All rights reserved.
