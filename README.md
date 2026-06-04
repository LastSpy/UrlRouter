# UrlRouter (Windows)

UrlRouter is a Windows application that intercepts `http://`, `https://`, and `httpproxy://` links,
lets you choose which application to open them in, and applies automatic routing rules based on
domain, MIME type, or file extension.

`Smart URL router for Windows: rules by host/MIME/extension, security scanning, quick app chooser, and in-app updates.`

Built with **Avalonia 11 / .NET 8** — GPU-accelerated rendering, high-DPI, native dark/light theme.

---

## Features

### Routing rules
- By domain — `example.com` → specific app
- By file extension — `.mp4` → mpv / VLC
- By MIME type — `video/*` → media player, `image/*` → image viewer
- Rule priority: domain → MIME → extension
- Rule Manager UI: view, add, edit, delete, reorder rules

### URL interception
- Registers as default handler for `http://`, `https://`, `httpproxy://`
- Self-registers on first launch — no manual registry editing required
- Intercepts media streams from Android/WSA apps (e.g. Anixart via `httpproxy://`)
- Receives URL as a command-line argument from Windows shell

### URL analysis
- Fetches `Content-Type` via `HEAD` (fallback to `GET`) to determine MIME type
- Extracts host, path extension, and redirect chain
- Skips HTTP analysis for non-web schemes (`httpproxy://`, etc.)

### App selection (Open With panel)
- Quick-select buttons for detected apps (up to 12), with icons extracted from the exe
- Apps are filtered by content type (media players for video, image viewers for images, browsers for web)
- Browsers are discovered automatically from the Windows registry — custom install paths included
- "Open in browser (bypass)" — skips UrlRouter routing and opens directly

### URL Security Scanner
- **VirusTotal** — malicious/suspicious detection (API key required)
- **Google Safe Browsing** — threat matches (API key required)
- **URLScan.io** — full sandbox scan, public/unlisted/private visibility (API key optional)
- Scanning Dashboard with overall verdict (LIKELY SAFE / UNSAFE / UNKNOWN)
- Scanning animation while analysis runs; per-provider cards show last-run time and lock status
- Auto-scan on open or manual "Scan URL Now" mode
- Rate limiting with configurable per-provider intervals

### Modules
| Module | Description |
|---|---|
| Log | Logs processed URLs to `%AppData%/UrlRouter/log.jsonl` |
| History | Writes URLs to `%AppData%/UrlRouter/history.txt` |
| Input text | Allows URL editing before opening |
| Status code | HEAD request + final redirect URL |
| Unshortener | Replaces short URL with the resolved redirect target |
| URL Shortener | Creates a short link via a shortener service |
| URL Cleaner | Removes tracking parameters (`utm_*`, `fbclid`, `gclid`, …) via ClearURLs-compatible rules |
| Queries Remover | Manually strip specific query parameters by name or pattern |
| Pattern checker | Warns about non-ASCII domains, Punycode, HTTP-only links |
| TLD checker | Warns about risky TLDs (`.zip`, `.mov`, `.gq`, `.tk`, …) |
| Hosts labeler | Labels hosts from a local suspicious-host list |
| URI Parts | Shows scheme / host / path / query breakdown |
| Open & Share | Enables "Open in browser (bypass)" and share buttons |
| URL Scanner | VirusTotal / Google Safe Browsing / URLScan.io (see above) |
| Changelog note | Appends build version to module notes |
| Debug module | Shows internal analysis timestamps and effective URL |

### Other
- Light / Dark theme (live switching, no restart)
- Retractable log, collapsible Scanning Dashboard
- Link Inspector — shows Original → Effective URL transformation
- M3U Editor — inline stream URL extractor with quality switching
- Scan History page
- In-app updater (GitHub Releases, configurable manifest URL, "Check for updates" button)
- Home screen when launched without a URL (registration status, quick access)
- Installer built with Inno Setup (`UrlRouter-Setup.exe`)

---

## Installation

1. Download and run `UrlRouter-Setup.exe`
2. Launch `UrlRouter.exe` once — the home screen will appear and the app will register itself
3. Click **Set as default browser (HTTP / HTTPS)** — Windows Default Apps will open
4. Select **UrlRouter** as the default browser for HTTP and HTTPS

To intercept `httpproxy://` streams (e.g. from Android/WSA apps): registration is automatic.

---

## Data and settings

All data is stored in `%AppData%/UrlRouter/`:

| File / folder | Contents |
|---|---|
| `settings.json` | Rules, module options, UI preferences |
| `log.jsonl` | URL processing log |
| `history.txt` | URL history |
| `lang/` | Cached language packs (downloaded on demand) |
| `updates/` | Downloaded installer files |
| `crash.log` | Unhandled exception log |

---

## Changelog

Version history: [CHANGELOG.md](CHANGELOG.md)

---

## Ideas for the next step

- **Language pack system** — JSON packs hosted in `lang/` on GitHub; users pick a language in Settings and the app downloads and applies the pack live. Community can contribute translations via fork+PR with no app update required.
- `More...` button for quick apps (show all candidates when more than 12)
- Export/import rules (`json`)
- History UI with filters and re-open actions
- Risk score (`0..100`) with clear reasons from scanner providers
- Domain allowlist/blocklist with warning flow
- Better updater UX (release notes + download progress bar)
- MSIX package support for enterprise-style deployment
