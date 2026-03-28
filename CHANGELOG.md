# Changelog

All notable changes to UrlRouter.

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

