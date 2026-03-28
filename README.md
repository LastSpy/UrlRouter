# UrlRouter (Windows)

UrlRouter is a Windows application that intercepts `http://`, `https://`, and `httpproxy://` links, lets you choose which application to open them in, and applies automatic routing rules based on domain, MIME type, or file extension.

`Smart URL router for Windows: rules by host/MIME/extension, security scanning, quick app chooser, and in-app updates.`

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

### App selection UI
- Quick-select buttons for detected apps (up to 9), with icons
- Apps are filtered by content type (media players for video, image viewers for images, etc.)
- Manual `.exe` picker
- "Remember choice" — saves a rule for future URLs of the same type
- Editable effective URL field (modify before opening)

### Modules
| Module | Description |
|---|---|
| Log | Logs processed URLs to `%AppData%/UrlRouter/log.jsonl` |
| History | Writes URLs to `%AppData%/UrlRouter/history.txt` |
| Status code | HEAD request + final redirect URL |
| Unshortener | Replaces short URL with the resolved redirect target |
| URL Cleaner | Removes tracking parameters (`utm_*`, `fbclid`, `gclid`, etc.) |
| Pattern checker | Warns about non-ASCII domains, Punycode, HTTP-only links |
| TLD checker | Warns about risky TLDs (`.zip`, `.mov`, `.gq`, `.tk`, etc.) |
| Hosts labeler | Labels hosts from a local suspicious-host list |
| URL Scanner | VirusTotal, Google Safe Browsing, PhishTank integration (API keys required) |

### Other
- Light / Dark theme
- Home screen when launched without a URL (registration status, quick access to settings)
- In-app updater (GitHub Releases, configurable manifest URL)
- Installer built with Inno Setup (`UrlRouter-Setup.exe`)

## Installation

1. Download and run `UrlRouter-Setup.exe`
2. Launch `UrlRouter.exe` once — the home screen will appear and the app will register itself
3. In the home screen click **Set as default browser (HTTP / HTTPS)** — Windows Default Apps will open
4. Select **UrlRouter** as the default browser for HTTP and HTTPS

To intercept `httpproxy://` streams (e.g. from Android/WSA apps): registration is automatic, no extra steps needed.

## Data and settings

All data is stored in `%AppData%/UrlRouter/`:

| File | Contents |
|---|---|
| `settings.json` | Rules, module options, UI preferences |
| `log.jsonl` | URL processing log |
| `history.txt` | URL history |

## Changelog

Version history: [CHANGELOG.md](CHANGELOG.md)

## Ideas for the next step

- `More...` button for quick apps (show all candidates when more than 9)
- Export/import rules (`json`)
- History UI with filters and re-open actions
- Risk score (`0..100`) with clear reasons from scanner providers
- Domain allowlist/blocklist with warning flow
- Better updater UX (release notes + download progress)
- Full localization via resources (RU/EN)
- MSIX package support for enterprise-style deployment
