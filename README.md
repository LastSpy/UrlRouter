# UrlRouter (Windows)

UrlRouter is a Windows application for intercepting `http/https` links, selecting the application to open them in, and applying routing rules.

`Smart URL router for Windows: rules by host/MIME/extension, security scanning, quick app chooser, and in-app updates.`

## What’s already there

The app can:
- by extension (e.g., `.mp4` -> `mpv`/`VLC`)
- by domain (e.g., `example.com` -> a specific app)
- by MIME type (e.g., `video/*` or `video/mp4` -> the appropriate player)

- Incoming URL as a command-line argument
- Analysis of domain/path/extension
- Attempt to retrieve `Content-Type` via `HEAD`
- Fallback to `GET` (headers only) if `HEAD` did not return a MIME type
- Quick buttons for known applications (mpv, VLC, IrfanView, XnView)
- Application icons on quick-select buttons
- Manual selection of `.exe`
- Saving rules to `%AppData%/UrlRouter/settings.json`
- Button to go to "Default Applications" settings
- Rule priority: domain -> MIME -> extension
- "Modules" screen (URLCheck-style) with module toggles
- "Settings" screen (Theme/Locale/Animations)

### Currently functional modules

- `Log` (logs to `%AppData%/UrlRouter/log.jsonl`)
- `History` (writes to `%AppData%/UrlRouter/history.txt`)
- `Status code` (HEAD + final redirect URL)
- `Unshortener` (applies the found redirect as the working URL)
- `URL Cleaner` (removes `utm_*`, `fbclid`, `gclid`, etc.)
- `Pattern checker` (non-ASCII, Punycode, HTTP without HTTPS)
- `Hosts labeler` (labels hosts from a local list)

The remaining modules are already present in the UI and configuration, but require further implementation of the logic.

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
