# YotoBookMaker for Windows

A native Windows 11 app for getting audio onto your [Yoto](https://yotoplay.com) player — split an audiobook into chapters, or upload a folder of audio files directly as a playlist.

> **Independent tool — not affiliated with or endorsed by Yoto Limited.**

![Platform](https://img.shields.io/badge/Windows-10%20%7C%2011-blue) ![License](https://img.shields.io/badge/license-MIT-green)

Also available for **[macOS →](https://github.com/bbenkle/YotoBookMaker)**

---

## Download & Install

1. Go to the [Releases](../../releases) page and download the latest `Setup.exe`
2. Run it — no admin rights or UAC prompt required
3. A Start Menu shortcut is created automatically

On first launch you'll be shown a brief Terms of Use — click **Accept** to continue.

> **Portable option:** a `.zip` is also available on the Releases page if you prefer to run the app without installing it. Extract to any folder and double-click `YotoBookMaker.exe`.

---

## What it does

YotoBookMaker supports three workflows:

**Audiobook workflow** — Load a single audiobook file, split it into chapters using embedded metadata, silence detection, or custom timestamps, add icons, and upload to Yoto in five guided steps.

**Batch split workflow** — Pick a folder of audiobooks and split them all at once with shared output settings. No upload or icon steps — useful for processing a library in one go.

**Playlist workflow** — Pick a folder of audio files (music, narration, existing chapter files, or anything else), review and reorder the tracks, add icons, and upload directly to Yoto as a playlist. No splitting required.

---

## Requirements

- Windows 10 (1903 or later) or Windows 11
- No additional software required — ffmpeg is bundled with the app

> **Note:** On Windows 10 the window background will be a solid color rather than the translucent Mica effect used on Windows 11. All features work identically.

---

## Audiobook workflow

### 1 — Select your audiobook

Drop an audiobook file onto the window or click **Browse Files**. The book title, author, and cover image are read automatically from the file's embedded metadata.

**Supported formats:** M4B · M4A · MP3 · MP4 · WAV · AAC

![](images/Select.png)

---

### 2 — Configure

Choose how chapters should be detected and pick your export settings.

**Chapter detection methods:**

| Method | When to use |
|---|---|
| **Embedded Metadata** | Best choice — works with most M4B files from Audible, iTunes, Libro.fm, etc. Instant and accurate. |
| **Silence Detection** | For files with no chapter markers. Finds chapter breaks by analyzing gaps in the audio. Not suitable for full-cast productions or books with continuous background music. |
| **JSON Timestamps** | Provide a JSON file with exact start times if you need full manual control. |

**Export options:**

| Setting | Options |
|---|---|
| Format | M4A · M4B |
| Bitrate | **Original** (default — no re-encoding) · 32 · 64 · 96 · 128 · 192 kbps |
| Mono | On by default — recommended for Yoto |
| Create subfolder | On by default — creates a `<title>_chapters` folder inside the output location you choose. Turn off to save files directly in the chosen folder. |
| Upload to Yoto after splitting | On by default. Turn off to split only — useful if you want to process the files yourself first. |

**Yoto has a maximum upload size of 500 MB for Make Your Own Playlist.** Please use the size estimator to find the correct export settings for your book.

The estimated export size is shown before you split. When using **Original** quality, the size matches your source file exactly.

#### Silence detection sensitivity

When using silence detection, choose a sensitivity level that suits your recording:

| Level | Best for |
|---|---|
| **Low** | Expressive narrators with dramatic pauses — conservative, only fires on long quiet gaps (≥ 5s) |
| **Medium** | Most professionally produced audiobooks — balanced default |
| **High** | Recordings with slight background noise or music fades — more sensitive, may produce extra splits |

#### JSON Timestamps format

If you choose **JSON Timestamps**, browse to a `.json` file containing your chapter list. The file is validated immediately on selection — you'll see the chapter count (or a specific error) before you proceed.

Each entry needs a title and a start/end time:

```json
[
  { "title": "Opening Credits", "start": "0:00:00", "end": "0:05:42" },
  { "title": "Chapter 1",       "start": "0:05:42", "end": "0:38:17" },
  { "title": "Chapter 2",       "start": "0:38:17", "end": "1:11:04" }
]
```

Times use `H:MM:SS` or `M:SS` format. Milliseconds are also supported (`start_ms` / `end_ms`). An [example file](https://github.com/bbenkle/YotoBookMaker/blob/main/example-chapters.json) is available in the Mac repository.

> **Tip:** if you've already split an audiobook using the Metadata method, the `_chapters.json` file it produces can be used directly as input for JSON detection on another file.

![](images/Configure.png)

---

### 3 — Split

Click **Start Splitting**. A live log shows what's happening as each chapter is detected and exported, along with an elapsed timer. The chapter list stays on screen after splitting so you can review results, and you can re-run with different settings at any time.

Once splitting is complete, a **Move Original to Recycle Bin** button appears if you want to clean up the source file.

![](images/StartSplitting.png)

![](images/SplitOutput.png)

---

### 4 — Icons

Each chapter gets a color-coded pixel art icon, generated entirely on your PC — no internet needed.

- **Generate All** — auto-generate icons for every chapter
- **Randomize All** — give every chapter a unique random color scheme
- **Per-chapter customization** — click any chapter card to generate, redo, or upload your own image
- **Bulk import** — choose a folder of images and they'll be matched to chapters by number in the filename (e.g. `ch-01.png`, `Chapter 22.jpeg`)

![](images/IconGeneration.png)

---

### 5 — Upload

![](images/PlaylistTitleCoverImage.png)

Sign in to your Yoto account — no password is entered into the app; you approve access in your browser using a device code. Then click **Upload to Yoto** and the app handles the rest: uploading audio, icons, and creating a ready-to-play playlist on your card.

If your audiobook file has embedded cover art, it is automatically extracted and pre-filled as the playlist cover image. You can replace it or clear it using the **Choose Image…** button.

Your sign-in is saved securely in the Windows Credential Manager so you only need to do it once.

![](images/YotoSign-in.png)
![](images/YotoURL.png)
![](images/Connected.png)
![](images/UploadYoto.png)

---

## Batch split workflow

Click **Split a Folder of Audiobooks** on the welcome screen and choose a folder. The app scans for audio files and shows the count before you start.

### 1 — Batch Settings

Configure shared output settings for all books in the folder:

| Setting | Options |
|---|---|
| Format | M4A · M4B |
| Bitrate | **Original** (default — no re-encoding) · 32 · 64 · 96 · 128 · 192 kbps |
| Mono | On by default — recommended for Yoto |
| Create subfolder | On by default — creates a `<title>_chapters` folder next to each source file. Turn off to save chapters directly in the same folder as the source file. |
| Move original to Recycle Bin | Off by default. Turn on to move each source file to the Recycle Bin after it is successfully split. |

Chapter detection always uses **Embedded Metadata** in batch mode — silence detection and JSON timestamps require per-file configuration and are not available.

### 2 — Batch Split

Each audiobook is processed one at a time. A row per book shows its status (queued → splitting → done / failed), a live progress bar, and the chapter count on completion.

Use **Cancel** to stop after the current book finishes. A summary is shown when the batch completes.

---

## Playlist workflow

Click **Choose Folder…** in the "Upload audio files" section on the welcome screen. Pick any folder of audio files — music, narration, existing chapter splits, or any other content.

Track titles are read from each file's embedded metadata and fall back to the filename if none is found.

### 1 — Playlist Setup

Review the track list loaded from your folder. You can:

- **Edit track titles** — click any title to change it
- **Reorder tracks** — drag rows into the order you want
- **Remove tracks** — click the trash icon on any row
- **Add more files** — click **Add Files…** to include audio files from other locations
- **Set the playlist name** — used as the title when uploading to Yoto

### 2 — Icons

Same icon step as the audiobook workflow — generate pixel art icons per track, customize colors and patterns, or import your own images.

### 3 — Upload

Sign in to Yoto and click **Upload to Yoto**. The app uploads each track's audio and icon, then creates a ready-to-play playlist on your card.

---

## Logs

Each app session writes a timestamped log file to `%LocalAppData%\YotoBookMaker\logs\`. Click **Show Logs…** at the bottom of the sidebar to open that folder in Explorer.

---

## CLI tool (yotosplit)

`yotosplit.exe` is a command-line tool bundled with the app. It exposes the same audiobook splitting engine so you can script or automate splits without opening the GUI.

### Setup

After installing the app, click **Install CLI Tool…** in the sidebar to get a PowerShell command that adds the install folder to your `PATH`. Once done, `yotosplit` is available in any terminal.

Or run it directly from the install folder (default: `%LocalAppData%\YotoBookMaker\`).

### Usage

```
yotosplit --input <path> [options]
```

### Flags

| Flag | Default | Description |
|---|---|---|
| `--input <path>` | — | Input audiobook file (M4B · M4A · MP3 · MP4 · WAV · AAC) — mutually exclusive with `--folder` |
| `--folder <path>` | — | Folder of audiobooks — splits all audio files using embedded metadata only (silence/JSON not available in folder mode) |
| `--output <path>` | next to input | Output directory |
| `--method <method>` | `metadata` | `metadata` · `silence` · `json` (ignored in `--folder` mode) |
| `--format <fmt>` | `m4a` | `m4a` · `m4b` |
| `--bitrate <kbps>` | `original` | `original` · `32` · `64` · `96` · `128` · `192` |
| `--mono` / `--no-mono` | mono on | Mono downmix (recommended for Yoto) |
| `--no-subfolder` | subfolder on | Save directly in output dir instead of a `<title>_chapters` subfolder |
| `--silence-threshold <dB>` | `-50.0` | Silence threshold in dBFS (silence detection only) |
| `--min-silence <seconds>` | `2.0` | Minimum silence gap (silence detection only) |
| `--json-file <path>` | — | JSON timestamp file (required for `--method json`) |
| `--trash` | off | Move original to Recycle Bin after a successful split |
| `--help` | — | Show usage |

### Output

Progress and log messages are written to **stderr**. On success, each chapter is printed to **stdout** as a tab-separated line:

```
1	Chapter 1	C:\output\01_Chapter_1.m4a
2	Chapter 2	C:\output\02_Chapter_2.m4a
```

This makes it easy to pipe the chapter list into other tools.

**Exit codes:** `0` = success · `1` = error

### Examples

```powershell
# Split using embedded metadata (default), mono M4A, original quality
yotosplit --input "MyBook.m4b"

# Silence detection, 64 kbps, save directly in C:\output
yotosplit --input "MyBook.mp3" --method silence --bitrate 64 --output C:\output --no-subfolder

# JSON timestamps, M4B format, move original to Recycle Bin when done
yotosplit --input "MyBook.m4b" --method json --json-file chapters.json --format m4b --trash

# Split every audiobook in a folder (uses embedded metadata for all files)
yotosplit --folder "C:\Audiobooks"
```

---

## Updates

The app checks for new releases automatically on launch. When an update is available, a badge appears in the sidebar with a link to the release page.

**To update:**
1. Click the update badge in the sidebar — it opens the GitHub release page
2. Download the new `Setup.exe` and run it over your existing installation
3. No uninstall step is needed — the installer detects the previous version and replaces it in place
4. No UAC prompt — the app installs per-user, so no administrator rights are required

**What is preserved across updates:**
- Your Yoto sign-in — stored in Windows Credential Manager, never touched by the installer
- Terms acceptance — stored outside the app files, unaffected by reinstallation

**Upgrading from a ZIP install (v1.2.0 and earlier):** the old ZIP release had no installer, so the new `Setup.exe` cannot detect it automatically. Run `Setup.exe` and accept the default install path — it points to the same folder the ZIP used (`%LocalAppData%\YotoBookMaker\`). The installer only copies application files; it never deletes files it did not place there, so your logs and any other existing data in that folder are preserved. Future updates will upgrade silently with no prompts.

---

## Privacy

| | |
|---|---|
| Audio splitting | Stays entirely on your PC (via bundled ffmpeg — no network calls) |
| Cover art extraction | Stays entirely on your PC (via bundled ffmpeg — no network calls) |
| Icon generation | Stays entirely on your PC |
| Update check | Sends a read-only request to the GitHub Releases API — no personal data |
| Upload | Sent to Yoto's servers — only when you click Upload |

No analytics. No telemetry. No third-party services.

---

## Disclaimer

YotoBookMaker is an independent, community-built tool. It is **not affiliated with, endorsed by, or supported by Yoto Limited**. Yoto, the Yoto logo, and Yoto Player are trademarks of Yoto Limited.

Only upload audio content you own or have the legal right to use — such as DRM-free audiobook purchases or public domain recordings. Do not upload commercially licensed content without the rights holder's authorization.

---

## License

MIT — see [LICENSE](LICENSE) for details.
