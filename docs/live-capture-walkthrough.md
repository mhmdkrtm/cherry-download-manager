# Cherry 0.4.2 — live capture and Link Grabber walkthrough

This guide covers the public live-capture and page-scanning flows in Cherry 0.4.2.

## Live capture

Open **Live**, choose **New recording**, paste a public live URL and press **Start recording**. Cherry identifies the source type, shows what it can do, and records without asking you to log in or install a site plugin. Press **Stop** on an active session; the recording is finalized and remains in **Downloads**.

## Link Grabber

Open **Link Grabber**, paste one page URL per line into **Scan pages** and press **Scan**. Cherry fetches the page, finds media and file links, groups quality variants and stages them for review. Scanning never starts a download. Select what you want and queue it in one batch.

## Source behaviour

| Source | What Cherry does | What it does not do |
| --- | --- | --- |
| TikTok, Bigo, Kick, Twitch, YouTube | Public live and page URLs go through the bundled yt-dlp path. Live and VOD entries appear in the review table. | No login automation, no cookie import, no premium or account bypass. |
| Tango | Public room URLs are resolved to their stream manifest and recorded as continuous HLS. | Followers-only rooms are not automated. A copied or stale token can expire; rescan the room instead. |
| SuperLive | Public rooms are captured while the page is open and the stream is live. | Captures stop when the page closes or the stream ends. No account or hoster plugin. |
| Direct `.m3u8` / `.mpd` | FFmpeg records the manifest continuously and writes a fragmented MP4. Stop finalizes a playable file. | Output goes only to the download folder you configure. |
| Direct live FLV / named platform URL | Existing live path, with the same Downloads row and Stop behaviour. | No DRM, paywall or captcha solving. |

## Safety rules

- Scanner and live-manifest fetches require `http(s)` on ports 80 or 443; private, loopback, link-local and metadata hosts are rejected.
- DNS answers are validated and the connection is pinned. Redirects are capped at five and response bodies at 5 MB.
- Live capture opens only the validated manifest. `file:` and `data:` inputs are rejected.
- A finished browser capture must already be inside the current download directory before it becomes a Downloads row.
- Stopping a live HLS/DASH recording waits for cleanup and does not mark a missing or empty file as complete.
- Firefox is unsupported in 0.4.2. Chrome, Edge, Brave, Opera and Vivaldi share one extension build.

## Known limits

- Tango and SuperLive need public live URLs to verify against the live sites. No login, cookie or account automation is included.
- Always reload the extension from the 0.4.2 release build if an older unpacked extension is still installed.
- A first-run default-browser prompt was checked in the 0.4.2 baseline but was not repeated in the final silent install check.
