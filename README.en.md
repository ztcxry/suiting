<p align="center">
  <img src="assets/readme/logo.svg" width="72" height="72" alt="Sui Ting's green vinyl record logo">
</p>
<h1 align="center">Sui Ting · 随听</h1>
<p align="center"><strong>Your library. Your mood. Your next batch of music.</strong></p>
<p align="center">Local-first listening · Configurable shuffle · Optional NetEase Cloud Music</p>
<p align="center"><a href="README.md">简体中文</a> · <strong>English</strong></p>
<p align="center">
  <a href="https://github.com/ztcxry/suiting/releases/tag/v0.1.4-public">Get the Android preview</a> ·
  <a href="#quick-start">Run on desktop</a> ·
  <a href="#your-first-listen">First listen</a> ·
  <a href="docs/CHANGELOG-0.1.4.md">Changelog (Chinese)</a>
</p>

**0.1.4 public preview** · Android 8.0+ · Node.js 22+ for the web preview · MIT

Sui Ting is a music player with selection rules you control. Import local audio or sync NetEase Cloud Music playlists, then choose a scope, mood, genre and language to generate the next batch of candidates. You decide when that batch starts playing.

> For example: pick 20 calm, soothing tracks from your liked songs, split the batch between recent additions and older favorites, and skip tracks recently played in Sui Ting.

![Sui Ting 0.1.4 workbench: scope and mood controls on the left, next-batch candidates and explicit playback on the right](docs/images/home-0.1.4.jpg)

*The 0.1.4 web interface with built-in synthetic examples. Example tracks contain no audio. [View full size](docs/images/home-0.1.4.jpg).*

<details>
<summary>See the dark player at a phone-sized viewport</summary>

<p align="center">
  <img src="docs/images/player-0.1.4.jpg" width="320" alt="Sui Ting dark player with track information, seek controls, playback modes, collection and lyrics controls">
</p>

This is the **web interface at phone size**, demonstrated with a self-created silent WAV. It contains no private account data or third-party recordings and does not establish that Android notifications, lock-screen controls or overlays have passed device testing.

</details>

## Listen on your terms

- **Choose your scope.** Select from your imported library, specific playlists or local collection. Selection does not silently expand to the platform's entire catalog.
- **Choose what to skip, too.** Combine moods, genres and languages; exclude candidates for this session or keep song and artist blocklists.
- **Balance recent additions and older favorites.** Set their proportions and a cooldown for tracks already heard. Missing classification or insufficient candidates are reported.
- **Prepare the next batch without interrupting this one.** Generation only updates candidates. The current playlist changes when you explicitly choose to play the batch. A bottom player brings together progress, queue, lyrics and quality controls.
- **Connect NetEase when you want to.** Sync playlists, browse daily recommendations and save candidates as a new playlist. Heart changes are staged locally and submitted or checked manually under Likes sync.
- **Keep local audio local.** Use ordinary audio files, manual classification, tags, LRC / TXT lyrics and JSON metadata backups. Local audio is not uploaded, transcoded or modified.

## Quick start

### Android: install and listen

Download the Android preview APK from the [upstream 0.1.4 Release](https://github.com/ztcxry/suiting/releases/tag/v0.1.4-public). It requires **Android 8.0+** and runs independently of a desktop server.

This is a public preview package. Updates to the upstream public preview can be installed over the previous version when signed with the same certificate. Back up your data and **do not uninstall first**. A self-built APK with a different signature cannot replace the original package directly. See the [build and signing instructions](docs/DEVELOPMENT.md) (Chinese).

### Desktop: run the web preview

Install **Node.js 22+**. The first dependency installation needs an internet connection.

```sh
git clone https://github.com/ztcxry/suiting.git
cd suiting
npm ci
npm start
```

Once the terminal reports that the server has started, open **<http://127.0.0.1:4392/>**. Keep the terminal running; press `Ctrl+C` to stop. On Windows, you can also double-click **`启动随听.cmd`** in the downloaded project to install dependencies and open the browser automatically.

If you already have the project, enter its directory instead of cloning again. If the port is occupied, stop the previous instance or choose another port using the `PORT` environment variable. Do not delete your library or blindly upgrade dependencies to resolve a startup issue.

## Your first listen

The first visit shows a three-step guide. Reopen it with **Guide (`指南`)** in the header. **The current interface is in Chinese**; the labels below match the app rather than implying an English UI is available.

1. **Bring your music.** Choose **Import local music (`导入本地音乐`)**, or **Connect NetEase Cloud Music (`连接网易云音乐`) → Generate login QR code (`生成登录二维码`)**. Scan with the NetEase app, read your playlists and select **Sync (`同步`)**. On one phone, save the QR image and use the scanner's album option.
2. **Choose how to listen.** In the workbench, choose playlists, moods and a track count, then select **Generate random queue (`生成随机队列`)**. Start with no mood restriction and 20 tracks; adjust genre, language and advanced rules later.
3. **Start listening.** Review **Next-batch candidates (`下一批候选`)** and select **Play this batch (`播放这批`)**. With an existing playlist, choose **Replace playlist and play (`替换播放列表并播放`)**. To listen in the official client, use **Save to NetEase (`保存到网易云`)** and confirm creation of a new playlist.

**Signing in does not sync playlists; syncing playlists does not download audio.** After login, select **Read my playlists (`读取我的歌单`)**, then sync the playlists you want. Sui Ting does not inherit the official client's login state or control its playback queue.

| Action | Result |
| --- | --- |
| Generate / another batch | Updates candidates; keeps current playback |
| Play this batch | Hands off candidates and starts playback |
| Reorder / clear candidates | Organizes candidates without stopping playback |
| Collect in Sui Ting | Saves to the local collection |
| NetEase heart | Stages an account-specific local action; writes to NetEase only on manual sync |

Song and artist blocklists immediately remove matching songs from the active playlist. Session exclusions only filter candidates; they do not retroactively interrupt current playback. Artist rules use platform IDs, falling back to complete credited names when IDs are unavailable.

## FAQ

<details>
<summary>Why are there so few matches? Does “recent” mean newly released?</summary>

Classification comes from file metadata, auxiliary NetEase encyclopedia data or manual labels. There is **no AI audio classification**. The relaxed mode can fill from related or unclassified songs according to your settings; strict mode uses exact labels only. Try completing classification, reducing filters or adjusting cooldown, and inspect the batch's rule-and-match explanation.

“Recent” primarily means **recently added to a playlist**, using a pool of 100 tracks by default. When addition dates are missing, playlist order may be used with an explanation. Cooldown uses playback within Sui Ting and manual “mark as heard” actions, not your full NetEase listening history.

</details>

<details>
<summary>Do I need to sign in after restarting? How do backups and upgrades work?</summary>

Web-preview login state stays in the local server's memory, so restarting it requires another QR login. Library metadata and imported audio copies live in the current browser. Use the same browser and address: `localhost` and `127.0.0.1` have separate storage.

Export JSON under **Connect music → Backups and local data (`连接音乐 → 备份与本机资料`)**. It excludes audio, file grants, credentials and pending likes journals. Keep your original audio separately. Android retains read-only grants to selected files; moved files or revoked permissions require selecting them again.

Pending likes are stored separately per account and survive in-place upgrades. Uninstalling or clearing app/site data removes them; sync manually before clearing data. The platform does not expose per-song like timestamps, so Sui Ting cannot reconstruct an exact official “liked today” list.

</details>

<details>
<summary>Can it play subscription tracks or downloaded NCM files?</summary>

Local playback supports ordinary MP3, FLAC, M4A, AAC, WAV, OGG and OPUS files; codec support depends on the device. Encrypted NCM / QMC files cannot be played directly.

The NetEase integration is unofficial and its interfaces may change. Playback depends on account permissions, subscription, region and rights. Sui Ting does not bypass paywalls, trial restrictions or DRM, and does not unlock encrypted music.

</details>

## Preview status and privacy

NetEase and search Easter eggs are off by default and can be managed under **My → Feature plugins & DIY (`我的 → 功能插件与 DIY`)**. Easter eggs use original animated records and short tunes. Local listening and selection do not require network plugins. The project has no ads, analytics or telemetry; see the [privacy documentation](PRIVACY.md) (Chinese).

The [0.1.4 verification record](docs/CHANGELOG-0.1.4.md#验证边界) reports **256 JavaScript tests, 71 JVM tests** and an optimized Android build passing. These are recorded release checks, not a claim of complete device or real-account end-to-end validation. Background/lock-screen playback, notification actions, overlay permissions and persistent file grants still need device testing.

That record also discloses 5 high-severity findings in the Node runtime dependency audit; it does not claim that all security issues are resolved. QQ Music, arbitrary script installation and AI audio classification are not supported.

## Development and contributions

```sh
npm ci
npm test
npm run check
npm start
```

[Contributing](CONTRIBUTING.md) · [Build & Android](docs/DEVELOPMENT.md) · [Architecture](docs/ARCHITECTURE.md) · [Plugin development](PLUGINS.md) · [Release checklist](docs/PUBLISHING.md)

The linked developer documents are currently in Chinese. Issues and PRs are welcome. Include OS/browser/device versions, reproduction steps and error details. Do not include credentials, private playlists, signing keys or third-party recordings.

## Origin and license

This repository is based on [ztcxry/suiting](https://github.com/ztcxry/suiting) and preserves its copyright and license notices. APK and release links above point to releases maintained by the upstream author.

Original project code is licensed under the [MIT License](LICENSE), credited to SuiTing contributors. Third-party components retain [their own licenses](THIRD_PARTY_NOTICES.md). The project license does not grant rights to music, lyrics or other third-party content.
