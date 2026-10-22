# CLAUDE.md

Guidance for Claude Code when helping someone install, configure or change **Garage Player** on a Samsung Tizen TV. Powered by TheGarageDevs.com.

## What this repo is

- `player/` — Garage Player, an IPTV web app (`GaragePlay.player`) for M3U/Xtream playlists with EPG. It is also part of `zerodevb/garage-home`.
- `tools/build-install.sh` — signs the app with the user's Samsung certificate, installs it over SDB and opens it. Called by `build-player.sh`.
- `tools/signing/` — Node signing dependencies (from Vetro).
- Optional: the phone QR inbox and the playlist/EPG proxy come from **Vetro with the Garage additions**: `https://github.com/zerodevb/vetro`, branch `garage`, based on upstream `andrearicci93/vetro` commit `e8553f2` (tag `upstream-e8553f2`). Use that branch, not upstream `main`, unless the user asks to update.

## Install walkthrough

Go one step at a time, and check each step before the next. Many steps happen on the TV, so ask the user to do them and to tell you what the screen shows.

1. **Computer.** macOS (scripts use zsh and the Keychain); Node 18+. Install **Tizen Studio** with *TV Extensions* and *Samsung Certificate Extension* from the Package Manager. The scripts expect `~/tizen-studio`.
2. **TV Developer Mode.** Apps → type `12345` with the remote → Developer Mode **On** → Host PC IP = this computer's LAN IP → restart the TV. Find the computer's IP with `ipconfig getifaddr en0`, and the TV's IP in TV Settings → General → Network → Network Status.
3. **Certificate.** In Tizen Studio Certificate Manager, create a **Samsung** certificate profile (TV, signed in with a Samsung account; add the TV's DUID if asked). Note the folder holding `author.p12` and `distributor.p12`, usually `~/SamsungCertificate/<profile>`.
4. **Configure.** `cp garage.env.example garage.env`, then set `TV=<tv-ip>:26101` and `CERT_DIR`. `garage.env` is git-ignored; never commit it.
5. **Signing tools.** `(cd tools/signing && npm ci --ignore-scripts)`.
6. **Vetro (optional).** Clone `zerodevb/vetro`, check out `garage`, and follow its `app/docs/INSTALLATION.md`. Its root setup needs the TV's Host PC IP set to `127.0.0.1` afterwards; follow that guide exactly and let the user make each change on the TV.
7. **Install.** `./build-player.sh`. The script prompts for the certificate passwords and offers to save them in the Keychain. Let the user type them in their own terminal; never ask for passwords in chat, put them in files or pass them on a command line.
8. **Check.** Garage Player opens on the TV. Add a playlist (by URL, or from a phone via the QR code when Vetro runs). Garage Player ships no channels; the user brings their own playlist.

## Connection rules (learned the hard way)

- Installing needs the TV's **Host PC IP = this computer**. Vetro's root needs **Host PC IP = 127.0.0.1**. After installing, remind the user to switch it back to `127.0.0.1` if they use Vetro. Changing it sometimes only takes effect after a TV restart.
- The TV allows **one developer connection at a time**. `build-install.sh` disconnects when done; do not leave `sdb` connected.
- **Never run `sdb kill-server`** while Tizen Studio is open; it comes back in a state that refuses to connect.

## Changing the code

- The TV runs **Chromium 76**. Write ES5-style JavaScript (`var`, `function`); no optional chaining, nullish coalescing, flex `gap` or `inset`. Do not add frameworks or a bundler.
- Files that get packaged are listed in `FILES` in each `build.cjs`; add new files there.
- Preview in a desktop browser by opening `player/index.html` (`player/preview.js` stubs the TV APIs).
- Per-TV settings come from `garage-config.js`, generated at build time from `garage.env` (`tvIp`). Don't hard-code IPs or locations.
- Keep the look: petrol/teal palette and the **garage**player wordmark and the white icon.

## Safety

- Don't edit Samsung system files, boot scripts or firmware on the TV, and don't try to get around Samsung's signature checks. Garage Player is a normal installed app.
- Ask before anything that changes the TV: installing, uninstalling, Vetro actions.
- Don't help find or share pirated streams; Garage Player is for playlists the user has the right to watch.
- Vetro's dashboard is plain HTTP with no login. Keep the TV on a trusted home network.

## License

PolyForm Noncommercial 1.0.0. Noncommercial use, modification and sharing are fine; selling or commercial use is not. Vetro and bundled libraries (qrcode-generator, pako) are MIT.
