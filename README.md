# aNONradio Player

A single-file, no-dependency web player for [aNONradio.net](https://anonradio.net) — a community-run internet radio station on SDF. Open the HTML file in a browser and go.

![status](https://img.shields.io/badge/dependencies-none-brightgreen) ![type](https://img.shields.io/badge/format-single%20HTML%20file-blue)

## Features

- **Play / stop** the live HTTPS stream (port 8443), with automatic reconnect on drop (exponential backoff, capped retries)
- **On-air indicator** — lamp + status text reflect connecting / buffering / live states
- **Volume control** and Media Session integration (play/pause from OS media keys / lock screen)
- **Now playing + full weekly schedule**, computed entirely offline from the listener's system clock — see below
- Retro amber CRT/receiver aesthetic, no build step, no external assets

## Why the schedule is offline, not polled

aNONradio's Icecast status endpoint and schedule pages don't send `Access-Control-Allow-Origin`, so a browser `fetch()` from this page is blocked by CORS no matter which of their endpoints it targets — polling live would just fail silently.

Instead, the station's weekly lineup (normally published as a recurring iCal feed at `anonradio.net/schedule/anonradio.ics`) is captured as a static snapshot baked into the HTML. "Now playing" and the schedule table are derived purely from the viewer's system clock against that snapshot — zero network requests, so there's nothing for CORS to block, and it works from a plain `file://` page.

**Trade-off:** the schedule is a point-in-time snapshot (dated in the source). If aNONradio changes their DJ lineup, the table won't reflect it until the snapshot is retaken and the embedded data regenerated.

## Usage

Just open `anonradio-player.html` in any modern browser. No server, no install, no build.

## License

MIT.
