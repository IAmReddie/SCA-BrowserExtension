# SoundCloud Discord Controller — Chrome Extension

Control your SoundCloud playback from Discord. Use slash commands like `/skip`, `/fm`, `/pause` and more — each person controls only their own SoundCloud tab.

---

## Is it safe?

Yes. Here's exactly what the extension does and doesn't do:

**It only does three things:**
1. Opens a WebSocket connection to the bot's server so it can receive playback commands
2. Reads your currently playing track info (title, artist, artwork) from the SoundCloud page when you ask for it with `/fm` or `/status`
3. Clicks the play/pause/skip/like buttons on the SoundCloud page on your behalf

**It never:**
- Reads your SoundCloud login credentials or account details
- Accesses any page other than `soundcloud.com`
- Sends any browsing data, history, or personal information anywhere
- Modifies anything on SoundCloud other than the playback controls you explicitly trigger
- Communicates with any server other than the bot's backend (`sound-cloud-discord-bot.replit.app`)

**Permissions explained:**

| Permission | Why it's needed |
|---|---|
| `scripting` | To click play/pause/skip buttons on the SoundCloud page |
| `storage` | To save your token and server URL locally in Chrome |
| `activeTab` | To find your open SoundCloud tab when a command arrives |
| `alarms` | To keep the background connection alive so commands work instantly |

The source code for the extension is fully visible in this repository — nothing is minified or obfuscated.

---

## How it works

```
You type /skip in Discord
        ↓
Discord sends the command to the bot
        ↓
Bot looks up your registered token
        ↓
Bot sends the command over WebSocket to your extension
        ↓
Extension clicks the Skip button on your SoundCloud tab
        ↓
Bot replies with the new track info
```

Your token is a random 32-character string generated locally in your browser. It never contains any personal information — it's just how the bot knows which extension to send commands to. No two users share a token.

The extension stays connected in the background as long as Chrome is running. It automatically reconnects if the connection drops.

---

## Requirements

- Chrome, Edge, Brave, or any Chromium-based browser
- SoundCloud open in a tab and logged in
- The bot added to your Discord (server or personal apps)

---

## Installation

1. **Download** `soundcloud-discord-extension.zip` from the bot's landing page or releases
2. **Unzip** it into a permanent folder on your computer (don't delete it after — Chrome needs the folder to stay there)
3. Open your browser and go to `chrome://extensions`
4. Enable **Developer mode** using the toggle in the top-right corner
5. Click **Load unpacked** and select the folder you unzipped
6. The SoundCloud Controller icon will appear in your toolbar

> **Note:** Chrome may show a warning that the extension isn't from the Web Store — this is expected for any extension installed manually. It does not mean the extension is unsafe.

---

## Setup (one-time)

1. Make sure SoundCloud is open in a tab and you're logged in
2. Click the extension icon in the toolbar — you'll see your **unique token** and a Copy button
3. In Discord, run:
   ```
   /register <your-token>
   ```
4. The bot will confirm you're linked — you're all set

---

## Discord Commands

Slash commands work in servers, in DMs with the bot, and in DMs with other users (if you've added the bot to your personal apps).

| Command | What it does |
|---|---|
| `/play` | Resume playback |
| `/pause` | Pause playback |
| `/skip` | Skip to next track |
| `/prev` | Go to previous track |
| `/like` | Like the current track |
| `/unlike` | Unlike the current track |
| `/mute` | Mute |
| `/unmute` | Unmute |
| `/volume <0–100>` | Set volume |
| `/fm` or `/status` | Show what's currently playing |
| `/register <token>` | Link your Discord account to this extension |
| `/unregister` | Unlink your account |

Prefix commands also work in servers and in DMs with the bot:
`+skip`, `+prev`, `+pause`, `+play`, `+like`, `+unlike`, `+mute`, `+unmute`, `+vol 75`, `+fm`

---

## Troubleshooting

**Extension shows "Disconnected"**
Click the extension icon and hit **Reconnect**. If it stays disconnected, make sure the bot server is online at `sound-cloud-discord-bot.replit.app`.

**Commands say "extension isn't connected"**
Open the extension popup and check it shows "Connected". If not, hit Reconnect or reload your SoundCloud tab.

**`/register` says the token wasn't found**
Make sure you copied the full token from the extension popup (it's a 32-character hex string). Tokens are case-sensitive.

**Commands stopped working after a while**
The extension now has a built-in keepalive that runs every ~25 seconds. If commands stop responding, click Reconnect in the popup. If you had an older version installed, reinstall from the latest zip to get this fix.
