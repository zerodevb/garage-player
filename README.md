<p align="center">
  <img src="docs/images/icon.png" alt="Garage Player IPTV icon" width="112">
</p>
<h1 align="center"><b>garage</b>player</h1>
<p align="center"><strong>A free IPTV player for Samsung Smart TVs (Tizen).</strong></p>
<p align="center">M3U and Xtream playlists, EPG, favourites and PIN locks — add playlists straight from your phone.</p>
<p align="center">
  <a href="#features">Features</a> ·
  <a href="#remote-controls">Remote</a> ·
  <a href="#requirements">Requirements</a> ·
  <a href="#build-and-install">Build and install</a> ·
  <a href="#faq">FAQ</a> ·
  <a href="#license">License</a>
</p>

Garage Player is an open-source **IPTV player for Samsung Smart TV** that plays **M3U / M3U8** and **Xtream Codes** playlists with an **EPG** TV guide on Tizen. It is the IPTV app from [**Garage Home**](https://github.com/zerodevb/garage-home), packaged on its own. It is designed for the remote, fast on older TVs and brings no ads or accounts. Garage Player does not include any channels; bring your own playlist.

<p align="center">
  <img src="docs/images/garage-player.jpg" alt="Garage Player IPTV channel list with preview" width="1000">
</p>
<p align="center">
  <img src="docs/images/garage-player-screens.jpg" alt="Garage Player screens: manage folders, settings and channel options" width="1000">
</p>

## Features

- **Playlists.** M3U/M3U8 links and Xtream Codes logins, several playlists side by side.
- **Browse fast.** Folders (groups), favourites and search.
- **EPG.** XMLTV guides, gzip supported, matched to channels by id or name; now/next on the overlay.
- **Add from your phone.** Type long links with the SmartThings app keyboard, or (with Vetro) scan the QR code on the TV and paste the link on your phone.
- **Playback keys.** Play, Pause and Stop on the remote, plus Pause/Resume and Stop in the full-screen options.
- **Clear messages.** No-internet notice that resumes on reconnect, loading spinners, and errors that say what to check.
- **PIN locks.** Lock playlists, folders or channels; adult groups are locked automatically.
- **Resets.** Clear favourites, locks or everything from Settings.

## Remote controls

| Key | Action |
| --- | --- |
| OK | Show the channel overlay |
| Hold OK | Channel options (favourite, lock) |
| Up / Down, CH +/− | Change channel |
| Play / Pause / Stop | Pause, resume, back to the channel list |
| Back | Hide the info bar, then go back; on the first screen asks before closing |

## Requirements

- A Samsung Tizen TV with **Developer Mode**, **Tizen Studio** (TV Extensions + Samsung Certificate Extension) and a Samsung **certificate profile**.
- Optional: **[Vetro](https://github.com/andrearicci93/vetro) with the Garage additions** — fork [zerodevb/vetro](https://github.com/zerodevb/vetro), branch `garage` (Vetro 0.3.2 plus the Garage additions; the original tested base is kept as tag `upstream-e8553f2`). It adds the phone QR inbox and a local proxy for playlists and guides that block TV apps. Without it, playlists that allow direct access still work.

## Build and install

> **Using Claude Code?** Open this folder and ask *"install Garage Player on my TV"* — [CLAUDE.md](CLAUDE.md) walks Claude through every step with you.

```bash
cp garage.env.example garage.env     # set TV, CERT_DIR and KEYCHAIN_SERVICE
(cd tools/signing && npm ci --ignore-scripts)
./build-player.sh
```

`garage.env` is git-ignored. Signing passwords are read from the macOS Keychain. The build writes the TV's IP into `garage-config.js` for the phone QR code. Without Vetro, set `GARAGE_STANDALONE=1` in `garage.env`: the phone QR option is hidden and playlists load straight from their servers. Set `window.GP_PROXY` to use a different proxy.

Open `player/index.html` in a desktop browser for a preview.

## FAQ

**How do I watch IPTV on a Samsung TV?**
Install Garage Player with Developer Mode and a Samsung certificate (see above), then add your M3U link or Xtream login, or scan the QR code and paste it on your phone.

**Does it support M3U8 and Xtream Codes?**
Yes: M3U/M3U8 links and Xtream Codes logins, with several playlists side by side.

**Does it have an EPG / TV guide?**
Yes, XMLTV guides (also gzipped), matched to channels automatically, with now/next on the channel overlay.

**Does it come with channels?**
No. Garage Player is only a player; use playlists you have the right to watch.

## Compatibility

Tested on a **Samsung Q60A (QE75Q60AAUXZT, Tizen 6, firmware T-NKLADEUC-2030.1)**. Other Tizen 6+ TVs should work; please report results in an issue.

## Credits

[Vetro](https://github.com/andrearicci93/vetro) signing tools (MIT), [qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator) (MIT), [pako](https://github.com/nodeca/pako) (MIT).

## License

Garage Player is an independent project, not affiliated with or endorsed by Samsung or the Tizen Project. Samsung and Tizen are trademarks of their respective owners.


[PolyForm Noncommercial 1.0.0](LICENSE). Free to use, modify and share for any noncommercial purpose; **selling it or using it commercially is not allowed** without permission. Bundled libraries keep their own MIT licenses.

<p align="center"><br>Powered by <a href="https://thegaragedevs.com"><b>TheGarageDevs.com</b></a></p>
