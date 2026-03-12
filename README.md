# 🎵 Spotify Lyrics Tracker

A real-time lyrics popup tracker that syncs with your Spotify playback. See the current lyric highlighted as your song plays — in a sleek floating popup window.

![Lyrics Tracker](https://img.shields.io/badge/Spotify-Web_API-1DB954?style=flat&logo=spotify&logoColor=white)
![Hosted](https://img.shields.io/badge/Hosted-Netlify-00C7B7?style=flat&logo=netlify&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-blue?style=flat)

---

## ✨ Features

- 🎧 **Real-time track detection** — automatically detects what's playing on Spotify
- 📝 **Synced lyrics** — line-by-line lyrics that follow your song in real time
- 🪟 **Popup window** — floating mini-player you can keep on top while using other apps
- 🔍 **Smart lyrics matching** — scores results by track name, artist, and duration to avoid wrong matches
- 🔄 **Auto-refresh** — token refresh handled automatically, no re-login needed
- 🌐 **No backend needed** — pure HTML/JS, runs entirely in the browser

---

## 🚀 Live Demo

> [https://your-app-name.netlify.app/spotify-lyrics-tracker.html](https://subtle-donut-c418ab.netlify.app/)

---

## 🛠️ Setup Guide

### 1. Create a Spotify Developer App

1. Go to [developer.spotify.com/dashboard](https://developer.spotify.com/dashboard)
2. Click **Create app**
3. Fill in:
   - **App name:** `Lyrics Tracker`
   - **App description:** `Real-time lyrics popup`
   - **Redirect URI:** your Netlify URL (e.g. `https://your-app.netlify.app/spotify-lyrics-tracker.html`)
   - **APIs used:** ✅ Web API
4. Click **Save**
5. Go to **Settings** → copy your **Client ID**

### 2. Deploy to Netlify

1. Go to [app.netlify.com/drop](https://app.netlify.com/drop)
2. Drag and drop `spotify-lyrics-tracker.html`
3. Copy the generated `https://` URL
4. Paste that URL into your Spotify app's Redirect URIs (step 1 above)

### 3. Use the App

1. Open your Netlify URL in the browser
2. Paste your **Client ID**
3. Click **Connect with Spotify**
4. Authorize the app
5. Play a song on Spotify 🎵
6. Click **Pop out** for the floating lyrics window!

---

## 🔐 How Authentication Works

This app uses the **PKCE (Proof Key for Code Exchange)** flow — the secure, modern way to authenticate with Spotify from a browser without a backend server.

```
User clicks Connect
  → Generate random code verifier + SHA-256 challenge
  → Redirect to Spotify login with challenge
  → Spotify redirects back with auth code
  → Exchange code for access token (+ refresh token)
  → Token auto-refreshes when expired
```

No client secret is ever exposed. ✅

---

## 📡 APIs Used

| Service | Purpose |
|---|---|
| [Spotify Web API](https://developer.spotify.com/documentation/web-api) | Currently playing track, playback progress |
| [LRCLIB](https://lrclib.net) | Free synced lyrics database |

---

## 📁 Project Structure

```
Spotify/
└── spotify-lyrics-tracker.html   # Single file — everything is here
└── README.md                     # This file
```

---

## ⚙️ How Lyrics Sync Works

1. Spotify API returns the current track + playback position (ms)
2. LRCLIB returns timestamped lyrics in LRC format: `[mm:ss.xx] lyric line`
3. Every 500ms, the app compares playback position to lyric timestamps
4. The matching line gets highlighted and auto-scrolled into view

**Lyrics matching uses a scoring system:**
- ✅ Exact track name match → +10 points
- ✅ Artist name match → +5 points
- ✅ Duration within 3 seconds → +4 points
- ✅ Has synced lyrics → +3 points
- ❌ Duration off by 30+ seconds → -5 points

Only results above a minimum score threshold are accepted, avoiding wrong song matches.

---

## 🧩 Troubleshooting

| Problem | Fix |
|---|---|
| `INVALID_CLIENT: Invalid redirect URI` | Make sure the redirect URI in Spotify dashboard **exactly matches** your Netlify URL |
| `response_type must be code` | Old implicit flow — use the latest version of the file (PKCE flow) |
| No lyrics found | Song may not be in LRCLIB database (common for rare/Korean tracks) |
| Popup blocked | Allow popups for your Netlify URL in browser settings |
| Stuck on "Nothing playing" | Make sure Spotify is actively playing (not paused for too long) |

---

## 📝 Notes

- The app only requests read-only Spotify scopes (`user-read-currently-playing`, `user-read-playback-state`) — it cannot control playback or access your library
- Tokens are stored in `localStorage` and auto-refresh — you only need to log in once
- LRCLIB is a free, open-source lyrics database. Coverage is best for popular English songs

---

## 📄 License

MIT — free to use, modify, and share.
