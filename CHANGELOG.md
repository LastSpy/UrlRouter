# Changelog

All notable changes to UrlRouter.

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
