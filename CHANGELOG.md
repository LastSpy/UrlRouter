# Changelog

All notable changes to UrlRouter.

## v0.1.0
Full rewrite from WinForms to Avalonia UI and a complete dashboard redesign.

**Framework migration**
- Rewritten from WinForms to Avalonia 11 / .NET 8 — GPU-accelerated rendering, high-DPI support, proper dark theme without Win32 hacks.
- All pages (Dashboard, History, Rules, M3U Editor, Modules, Settings) reimplemented as Avalonia UserControls with MVVM bindings.

**Dashboard redesign**
- Two-column layout: left column (Working Modules, Open With, Link Inspector) and right column (Scanning Dashboard, provider cards, retractable log).
- URL bar redesigned with two clickable tabs — "URL for Editing (Editable)" and "Effective URL (read)" — and an aligned Apply button.
- Action buttons row above the content grid: `[Shorten URL]`, `[Scan URL Now]`, `[Edit M3U]`, `[Open in Browser (bypass)]`.
- Working Modules card: compact single-line layout — module count and status on one line with `·` separators.
- Scanning Dashboard: collapsible card with an `▲/▼` toggle; left side shows a colored verdict badge (LIKELY SAFE / UNSAFE / UNKNOWN), right side shows an inline scan log.
- Three provider detail cards (VirusTotal, Google Safe Browsing, URLScan.io) each showing Status, HTTP code, Last scan time, and Locked-until time.
- Link Inspector: shows Original → Effective URL flow; effective URL box highlights in blue when the URL was transformed by a module.
- Open With panel: icon + name buttons in a scrollable wrap layout; up to 12 app candidates shown.
- Retractable log: collapsible `∨/∧` section showing scanner and module notes in a monospace block.
- Status bar at the bottom shows protocol handler registration state.
- Sidebar updated with app PNG icon and version label; nav items styled with active/hover states.

**URL scanner**
- PhishTank replaced with URLScan.io as the third scan provider.
  - URLScan.io submits the URL and polls for a result (up to 30 s); reports malicious verdict and score.
  - Supports public / unlisted / private visibility modes; API key is optional for public scans.
- Provider cards now show "Last: HH:mm" (today) or "Last: MM-dd HH:mm" (older) instead of the previous non-functional "Quota: N/A".
- Label "Quota Lock:" renamed to "Locked:" across all provider cards.
- On startup the Scanning Dashboard immediately shows a waiting state ("SCANNING" + spinner) instead of displaying the stale result from the previous session.
- Scanning animation: shield icon cycles through ◐ ◓ ◑ ◒ at 180 ms while a scan is in progress (auto-scan on launch and manual `[Scan URL Now]`).

**Bug fixes**
- Fixed: app window was opening in the background when launched as a protocol handler. Analysis (2–6 s network round-trip) ran before the window was created, causing Windows focus-stealing permission to expire. Fix: window now opens immediately and analysis runs async after the `Opened` event while `SetForegroundWindow` permission is still valid.
- Fixed: Edge browser not detected — was looking in `Program Files` but Edge installs to `Program Files (x86)` on 64-bit Windows.
- Fixed: Opera GX not detected — launcher.exe was used instead of the correct `opera.exe` path under `%LocalAppData%\Programs\Opera GX`.
- Fixed: `[Open in Browser (bypass)]` was silently doing nothing if no browser was found; now shows an error message prompting the user to configure a browser in Settings.
- Fixed: stale `PhishTankLastHttpStatus` reference in verdict calculation updated to `UrlScanLastHttpStatus`.

## v0.0.39
- Fixed: URL Scanner no longer attempts to scan `file://` or other non-web URLs.
  - Scanning is now skipped with an informational note for any URL with a non-`http`/`https` scheme.
  - Previously, scanning a local `.m3u` file caused Google Safe Browsing to return `400 Bad Request` and VirusTotal to return `429`/`404`.
- Fixed: VirusTotal `404` is no longer treated as an error.
  - `404` means the URL is not yet in VirusTotal's database (never analyzed), which is informational and not a threat signal.
  - Note text updated to clearly explain the meaning.
- Improved: scanner status "Last check" label now shows specific labels for common HTTP codes: `NOT FOUND (404)`, `RATE LIMITED (429)`, `BAD REQUEST (400)` instead of the generic `ERROR (N)`.

## v0.0.38
- M3U Editor: added video quality panel.
  - Detects the current quality from stream URLs (e.g. `720p`) and highlights the active button.
  - Quick-switch buttons for 360p / 480p / 720p / 1080p replace the quality in all entries at once (both stream URLs and referrer headers).
  - `Check max ▶` button probes the CDN server via HTTP HEAD requests to find the highest actually available quality, then offers to apply it automatically.

## v0.0.37
- Added M3U Editor: when the effective URL is a local `.m3u` or `.m3u8` file, an `Edit .m3u` button appears on the main screen.
  - Opens a dedicated editor with syntax-highlighted file content (directives, stream URLs, comments each in distinct colors).
  - Detected stream URLs are listed separately; clicking `Use as effective URL` extracts the URL and returns it to the main window for opening with a media player.
  - File content is editable and can be saved back to disk via `Save file`.
- Fixed: `Open in browser (bypass)` now correctly uses the Windows default browser.
  - Reads the default browser from `HKCU\Software\Microsoft\Windows\Shell\Associations\UrlAssociations\https\UserChoice` in the registry.
  - Falls back to known browser paths only if the registry lookup fails or returns UrlRouter itself.
  - Removed the previous hardcoded Edge fallback.
- Added `.m3u` to the recognized media extensions list (previously only `.m3u8` was listed).

## v0.0.36
- Added `httpproxy://` protocol interception for media streams from Android/WSA apps.
  - UrlRouter now registers itself as handler for `httpproxy://` alongside `http://` and `https://`.
  - Media-proxy URLs are passed directly to the player without HTTP analysis (no HEAD/GET probe).
  - Matching rules and manual app selection work the same as for regular URLs.
- Fixed: non-web schemes (httpproxy://, rtmp://, etc.) no longer crash the analysis pipeline.
- Fixed: opening without a URL now shows the UrlRouter home screen instead of a text message.

## v0.0.35
- Fixed: UrlRouter now registers itself in the Windows registry as a protocol handler on first launch or when opened without arguments.
  - Writes `UrlRouterURL` ProgId with shell command `UrlRouter.exe "%1"` under `HKCU\SOFTWARE\Classes`.
  - Registers `Capabilities` + `URLAssociations` (http/https) under `HKCU\SOFTWARE\UrlRouter`.
  - Registers under `HKCU\SOFTWARE\RegisteredApplications` so UrlRouter appears in Windows "Default apps" browser list.
- Fixed: launching without a URL argument now auto-registers and opens Default Apps settings instead of showing an unhelpful error.
- Fixed: "Default associations" button in Settings also re-registers before opening Default Apps settings (handles exe relocation).

## v0.0.34
- Added new `Rule Manager` UI on the main screen (`Rules` button):
  - view all saved rules in a single table,
  - add/edit/delete rules,
  - move rules up/down,
  - sort rules by type/value.
- Added dedicated rule editor dialog with validation for type/value/app path.
- Installer build generated (`UrlRouter-Setup.exe`).
- Improved quick app detection for custom URI schemes:
  - added better handling for IDE schemes (`vscode://`, `vscode-insiders://`, `vscodium://`, `cursor://`, `windsurf://`),
  - added protocol-handler discovery from Windows registry (`<scheme>\\shell\\open\\command`).
- Improved quick app detection for direct download links:
  - added download/archive/java link classification by extension and MIME,
  - expanded suggested app groups for download managers, archive tools, and Java runtimes.

## v0.0.32
- Added proper application icon packaging for Windows shell integration:
  - embedded `assets/UrlRouter.ico` into the app executable,
  - configured installer setup icon to use the same brand icon.
- Improved icon consistency for taskbar, Start menu, installer, and app shortcuts.

## v0.0.31
- Main window quick shortcuts redesigned for better density and clarity:
  - moved app shortcuts into a compact top area near module/settings controls,
  - switched to multi-row compact layout (up to 9 visible shortcuts),
  - removed repeated `Open with` text from each button,
  - kept icon-first shortcut style with shortened app names.
- Added a shared `Open with:` heading for the whole shortcut group.
- Improved dynamic sizing for the top controls area to avoid clipping and overlap.

## v0.0.30
- Main screen layout refresh:
  - replaced plain modules text area with a dedicated `Module status` block (same visual style as `Scanner status`),
  - improved readability and grouping of module output.
- Quick app shortcuts moved higher in the UI:
  - `Open with ...` shortcuts are now shown near `Modules` / `Settings` / `Rule tester`,
  - bottom area is cleaner and less crowded as module blocks grow.

## v0.0.29
- Installer build generated (UrlRouter-Setup.exe).

## v0.0.28
- Added new module: `TLD checker`.
- TLD checker warns about frequently abused top-level domains and adds informational notes for normal TLDs.
- Installer build generated (`UrlRouter-Setup.exe`).

## v0.0.27
- Auto-update GitHub API mapping fix verified in installer build.
- Installer build generated (`UrlRouter-Setup.exe`).

## v0.0.26
- Added update banner on main screen with `Update` button for in-app installer download/start.
- Improved non-intrusive update UX (status in UI instead of immediate popup flow).
- Installer build generated (`UrlRouter-Setup.exe`).

## v0.0.25
- Added project publishing essentials for GitHub:
  - `.gitignore` for build artifacts and local files,
  - `LICENSE` (MIT, author: LastSpy),
  - updated repository guidance in `README.md`.
- Added and connected application branding icon across main dialogs.
- Continued installer release flow (Inno Setup) for latest build delivery.

## v0.0.24
- Improved scanner final clarity:
  - explicit verdict (`UNSAFE` / `LIKELY SAFE` / `UNKNOWN`),
  - checked providers count and alert count in summary,
  - cleaner replacement of scanner notes/warnings on re-scan.
- Expanded scanner status details per provider:
  - `next use`, `quota left`, `reset at`, `blocked until` (when available),
  - better visual interpretation of HTTP status and state.
- Added "Last check" line in scanner panel (provider + timestamp + success/error).
- Settings updates:
  - added `About` button,
  - author shown as `LastSpy`,
  - removed visible `Updates URL` field.
- Auto-update flow upgraded:
  - uses GitHub Releases source by default (`LastSpy/UrlRouter`),
  - keeps manual `Check updates now` and startup auto-check.

## v0.0.23
- Added first in-app updater flow:
  - update manifest URL in Settings,
  - manual "Check updates now",
  - optional auto-check on startup,
  - download and start installer from app.
- Improved scanner status panel sizing so long status text is readable and does not clip.

## v0.0.22
- Reworked scanner UI block on main screen with clearer visual grouping.
- Added color cues for scanner mode, rate-limit, provider states, and warnings.
- Improved readability of scanner details and risk indicators.

## v0.0.21
- Installer and publish pipeline stabilization.
- Better handling for missing Inno Setup / fallback install path in build script.
- General packaging reliability improvements.

## v0.0.20
- Main screen cleanup and layout polishing.
- Better separation of routing info and module details.

## v0.0.19
- Scanner status checker improvements.
- Better visibility of provider HTTP status and timing information.

## v0.0.18
- Added API-key help links and scanner configuration quality-of-life updates.
- Improved module option persistence.

## v0.0.17
- Added manual/auto scanner controls and provider toggles.
- Extended scanner settings with more actionable options.

## v0.0.16
- Modules UI redesign toward card-based layout (URLCheck-like structure).
- Grouping and better visual hierarchy for working/experimental modules.

## v0.0.15
- Added automatic app discovery from registry and browser associations.
- Improved icon extraction and quick app suggestions.

## v0.0.14
- Expanded portable app scanning support.
- Improved candidate collection and relevance.

## v0.0.13
- URL Cleaner advanced model updates.
- Better parsing compatibility with complex ClearURLs-like rules.

## v0.0.12
- URL Cleaner updater/editor work:
  - remote catalog handling,
  - advanced JSON configuration support.

## v0.0.11
- UI readability updates for inspector and short-link details.
- Better sizing and text behavior in key status areas.

## v0.0.10
- Theme persistence refinements.
- Better dark-mode consistency across forms and controls.

## v0.0.9
- Improved action placement (Settings/default associations/rule controls).
- Better behavior for editable effective URL flow.

## v0.0.8
- Added installer scripting via Inno Setup.
- First one-click install path integrated with app publish output.

## v0.0.7
- Initial scanner and safety module groundwork.
- First provider integration path and status outputs.

## v0.0.6
- Extended module framework and settings persistence.
- More configurable module behavior.

## v0.0.5
- Added MIME-aware matching and improved routing logic.
- Better parity with URLCheck-like decision behavior.

## v0.0.4
- Expanded rule system (host / extension / MIME).
- Better saved-choice handling and rule application flow.

## v0.0.3
- Improved URL metadata probing (HEAD/GET fallback behavior).
- Better robustness around redirects/content-type detection.

## v0.0.2
- Added default handler registration scripts for HTTP/HTTPS.
- Better Windows integration workflow.

## v0.0.1
- Initial WinForms prototype:
  - accept URL argument,
  - show app chooser,
  - open URL with selected app,
  - basic settings persistence.

