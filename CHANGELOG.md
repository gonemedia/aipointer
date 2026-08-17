# Changelog

All notable changes to AIPointer are documented here. Format loosely follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and the project uses
[Semantic Versioning](https://semver.org/spec/v2.0.0.html).

The project was originally scaffolded on **2026-05-13** and tagged **v1.0.0** on
the same day after a focused build sprint.

---

## [1.1.8] — 2026-05-31

Multi-monitor support, and speech-to-text that runs on your own machine.

### Added

- **On-device speech-to-text.** Your voice is transcribed locally — multilingual,
  German included — with no API key and no cloud round-trip. Install it on demand
  in Settings; skip it and voice input keeps using your cloud provider. It joins
  the on-device read-aloud voices already in AIPointer, so the whole voice loop
  can now run offline.

### Fixed

- **Multi-monitor support.** Trigger AIPointer on any screen: the box, the
  screenshot, the drag-to-capture region and your typing all happen on whichever
  display the cursor is on. With two or more monitors the prompt could previously
  close itself after a couple of keystrokes.

---

## [1.1.7] — 2026-05-25

### Fixed

- **Local read-aloud loads cleanly on every Mac.** v1.1.6 could fail on the first
  Read aloud with a "could not load sharp module" message on some signed builds,
  because two slightly different copies of the same internal library ended up
  bundled side by side. v1.1.7 ships a single consistent runtime.
- **The voice picker keeps your selection.** Choosing a different Kokoro voice and
  clicking Save now persists across sessions. In v1.1.6 the change was silently
  dropped and reverted to the default the next time Settings opened.

### Added

- **`▶` preview button next to the Kokoro voice dropdown** (Settings → Voice →
  Local). Hear a short test sentence in the currently picked voice before you
  commit, and audition all 28 voices side by side without spending a real query.
  Click again while playing to stop; previewed voices are cached, so re-clicking
  the same one is instant.

---

## [1.1.6] — 2026-05-24 (hotfix on v1.1.5)

v1.1.6 is a reliability hotfix on top of v1.1.5. No new features beyond
the Kokoro voice picker — the rest of this release is making the v1.1.5
voice engine + Finder auto-attach actually work on a signed, notarized
macOS build. v1.1.5 shipped with the Local TTS runtime missing from the
installer and a couple of regression-class bugs in the TTS / auto-attach
paths; v1.1.6 ships the fixes.

### Fixed

- **Local TTS actually loads on signed builds.** 
- **Finder / Explorer auto-attach silent failure.** 
- **System TTS feels instant again.** 
- **Build YAML drift.** 

### Added

- **Kokoro voice picker.** 


---

## [1.1.5] — 2026-05-24

Voice engine picker, live model picker, primary and fallback providers, Finder/Explorer auto-attach, multi-file upload, and light-theme readability.

### Added

- **Voice engine picker (Settings, Voice).** Choose how AIPointer handles speech, per install:
  - **System** (default): your computer's built-in voice for both read-aloud and voice input. No setup, no key, no download. Works for everyone out of the box.
  - **Local**: a free voice that runs entirely on your machine, powered by Kokoro. Nothing leaves your computer. One-time download of about 231 MB, with the size shown before the download starts and a progress bar while it runs. Fully offline afterwards, and removable any time. Local voice input (speech-to-text) follows in v1.2.0; until then it uses the System voice for input.
  - **Cloud**: use your own provider key for the highest-quality voices.
- **Live model picker for every provider.** Each provider now has a Refresh button that pulls its current model list using your saved key, so you can pin a specific model without waiting for an app update. Leave it on Auto-pick to let AIPointer choose for you.
- **Primary and fallback providers.** Pick a main provider and arrange up to 3 backups, each with its own pinned model. If a provider is down or rate-limited, AIPointer automatically moves to the next one. A wrong or expired key still stops on that provider so you can fix it. Open it with `/fallback`.
- **Finder and Explorer auto-attach.** Select up to 5 files in Finder (Mac) or Explorer (Windows), press the AIPointer hotkey, and they attach to your next question automatically. No dragging, no paperclip. On Mac this only reads your selection when Finder is the active window, so the hotkey from inside another app never grabs files in the background. Linux uses the paperclip button.
- **Multi-file upload, up to 5 files.** Click the paperclip or type `/attach` to add up to 5 files (was 3). Images come in as visual context; text, code, CSV, JSON, Markdown and similar are read into the prompt; PDFs, Word and Excel files are referenced by name so the model knows they exist. 20 MB per file.
  - A dark-orange "files attached" chip appears above the prompt, matching the existing screenshot and clipboard chips. Hover to expand it or clear the queue.
  - The screenshot is automatically hidden while files are queued, so the model focuses on your files.
  - Press Enter with files attached and no text, and AIPointer summarizes them automatically.
- **Four new slash commands:** `/attach`, `/voice`, `/models`, `/fallback`, each jumping straight to the relevant place. Available in the command menu and by voice in English, German, French, Spanish, Italian, Portuguese and Dutch.

### Fixed

- **"Drag to capture a region" hint is now readable and centered.** On some backgrounds it could look black-on-grey and sat off to one side. It now matches the main pill surface with a clear accent border, and is properly centered.
- **Light-theme readability.** Bright accent colors on the new settings could wash out on the white panel. Text and inputs now use higher-contrast variants in light mode so everything stays legible.
- **Read-aloud reliability.** Cloud read-aloud could silently fail and leave you with no audio. Failures now fall back to the system voice cleanly, and an amber note explains which provider failed and why. A separate playback glitch that occasionally produced no sound at all is fixed.
- **Finder auto-attach errors are explained.** If macOS hasn't been given permission to read your Finder selection, an on-screen note now tells you exactly where to enable it in System Settings, instead of files just not showing up.
- **Save button now switches to Close after a successful save.**
- **Fallback editor no longer overflows** the settings window with long model names.

### Changed

- Settings now migrate transparently from earlier versions; existing setups keep working unchanged until you opt into a new voice engine.
- Provider keys are stored encrypted via the operating system's secure storage.
- Version rolled from 1.1.2 to 1.1.5 across the app, About panel, and documentation.

### Note on read-aloud voices

An earlier experiment using Hugging Face's hosted text-to-speech was dropped before release because the service proved too unreliable in practice. Cloud read-aloud in 1.1.5 uses OpenAI or Gemini directly (with OpenRouter as a best-effort option), and the new Local engine gives you a fully offline voice. For the most reliable cloud read-aloud, a direct OpenAI or Gemini key works best.

### Licensing

- Kokoro voice model: Apache-2.0 (compatible with AIPointer's BSL-1.1).
- Whisper / whisper.cpp speech models: MIT (compatible). Attribution in NOTICE.md.

---

## [1.1.2] — 2026-05-19 (license switch, chat-only, accent picker, pause, activation toggle, chip refresh)

### Changed — License

- **AIPointer switches from MIT to the Business Source License 1.1.**
  Licensor: Mario Simic. Change Date: **2030-05-19** — on that date,
  the codebase automatically converts to the **Apache License,
  Version 2.0**. Older releases (≤ v1.1.1) stay under MIT; their
  tags and binaries are unaffected. Source remains public on GitHub.
  Personal, educational, and internal business use stay free.
  Commercial redistribution, SaaS hosting, white-labeling,
  bundling, and resale require a written commercial license from
  the Licensor. See [NOTICE.md](NOTICE.md) for the public reasoning
  and [COMMERCIAL-LICENSE.md](COMMERCIAL-LICENSE.md) for commercial
  inquiries.

### Added

- **Chat-only mode.** Toggle in `Settings → Behaviour`. When on, the
  box opens *without* an auto-attached cursor screenshot, so you can
  use AIPointer as a pure chat window for text follow-ups without
  having to dismiss the Screenshot chip on every trigger. A small
  camera button appears inside the prompt input whenever no
  screenshot is attached — click it to attach one for that single
  query. Default off (preserves v1.1.1 cursor-anchored behavior).
- **Cursor accent picker.** New `Settings → Appearance → Cursor accent`
  row with three brand presets — **Lime** (`#a7f56b`, default and
  original brand mark), **Blue** (`#0a84ff`, the same macOS-system-blue
  used by the Screenshot chip), and **Magenta** (`#d946ef`, a
  cyberpunk-violet alternative). The CursorHalo comet trail + glow
  and every Tailwind `accent`-driven surface (status dot, active
  buttons, halo shadow, focus rings, etc.) re-tint live without an
  app restart. No free-form color picker — palette stays curated.
- **Pause / resume for text-to-speech.** The Read button under each
  response is now a real play / pause toggle: first click starts
  playback, second click pauses at the current position, third
  click resumes from where you left off. The button shows a pause
  glyph + "Pause" label while playing and a play glyph + "Resume"
  while paused. The voice-loop Stop entry points (spoken stop-words
  like "stop" / "halt" / "sei ruhig", ESC, new query) still fully
  end playback. Works for both Web Speech API and provider-TTS
  (OpenAI / Gemini). Fixes [#3](https://github.com/gonemedia/aipointer/issues/3).
- **Activation section in Settings.** New top-level Section grouping
  the Keyboard hotkey dropdown and a Mouse-wiggle on/off toggle.
  Mouse-wiggle activation has existed since v1.1.0 but had no UI
  control; the toggle takes effect live, defaults to on (preserves
  v1.1.0 behavior), and persists across restart. No sensitivity
  slider. Fixes [#4](https://github.com/gonemedia/aipointer/issues/4).
- **NOTICE.md** documenting the license switch in plain language.
- **COMMERCIAL-LICENSE.md** describing what triggers a commercial
  license requirement and where to inquire.
- **FAQ entries** for "How do I open AIPointer?" (keyboard hotkey,
  mouse-wiggle, pill click + wiggle-off note) and for Chat-only
  mode.

### Changed

- **Screenshot and Clipboard chips are now floating bubbles.** They
  render above the BottomPill as rounded-full toast-style pills
  with the same `ap-frosted-skin` micro-grain and five-layer shadow
  as the BottomPill, instead of as banner-style strips inside the
  prompt input row. The chips use a high-opacity tinted fill
  (macOS-system-blue for Screenshot, deep neutral with accent icon
  for Clipboard) with white text in both themes — backdrop bleed
  no longer made the text invisible when light-theme AIPointer
  floated over a dark webpage. The prompt input reclaims the
  vertical space the old banner strip occupied.
- **System prompt version is now dynamic.** `SYSTEM_PROMPT` carries
  a `{{VERSION}}` placeholder that gets substituted with
  `app.getVersion()` at request time, so the LLM always knows
  the exact running version without anyone having to bump a string
  literal on every release.
- **All repository links point to `github.com/gonemedia/aipointer`**
  (about window, settings footer, tray menu, FAQ, save-template
  footer, system prompt, package.json). Author profile links to
  `github.com/talentsache` remain unchanged (Mario's personal
  GitHub user). The About window now shows "AIPointer on GitHub"
  instead of the bare URL.
- **SPDX-License-Identifier headers** added to
  `electron/main.ts`, `renderer/main.tsx`, and the LLM router.
- **README** license section, footer line, and "What it is"
  paragraph rewritten for BSL-1.1. About dialog, Save-as-HTML
  footer, system prompt self-knowledge, and Settings footer all
  updated to reflect the new license.

### Fixed

- **Manual theme choice is now persistent across macOS appearance
  changes.** Picking "Light" or "Dark" in `Settings → Appearance`
  would silently flip back to the system theme whenever macOS
  toggled appearance, because a stray `prefers-color-scheme`
  listener kept mutating `html.dark` regardless of the user's
  saved preference. The `themePreference` effect now owns the
  dark-class entirely; the initial mount only applies the system
  value once for first paint.
- **Chip text contrast.** See the floating-chip note above.

### Notes

AIPointer aligns its license with [Skales](https://skales.app), its
sibling project, which is also BSL-1.1. Source stays available on
GitHub. Personal, educational, and internal business use remain
free. Commercial redistribution, SaaS hosting, white-labeling, and
resale require a license. The BSL clauses are time-limited by
design: on **2030-05-19** AIPointer automatically reverts to the
Apache License, Version 2.0.

---

## [1.1.1-stabilization-4] — 2026-05-17 (theme, frosted skin, chip blue)

### Added

- **Theme picker.** Settings → Appearance → System / Light / Dark.
  Stored as `themePreference`; the renderer's html.dark class is
  driven by the preference instead of being hard-bound to the OS
  media query. Users who keep macOS in Light can now pick AIPointer
  Dark (and vice versa).
- **Premium pill skin (`.ap-frosted-skin`).** Combines two effects
  to approximate macOS Control Center glass without OS vibrancy:
  - a near-imperceptible SVG `fractalNoise` overlay (~5 % opacity,
    `overlay` blend) gives the surface the micro-grain of physical
    frosted glass instead of a flat colored fill
  - a five-layer `shadow-glass` token (inner top highlight, inner
    ring, near drop-shadow, mid-distance shadow, wide ambient) gives
    the pill perceived 3-D thickness, the way macOS HUD tiles read
    floating even at a glance

### Changed

- **Dark theme surface** `neutral-900` (#171717 near-black) →
  `neutral-800` (#262626 soft grey). Matches the Claude-sidebar
  grey the user explicitly asked for; harsh black hole is gone.
- **Light theme surface** pure `white` → `neutral-100` (#f5f5f5
  warm light grey). Reads as a macOS Control Center light tile
  instead of a sharp white block.
- **Pill alpha** lifted to 0.92 / 0.93 — the "leicht durchsichtig"
  feel the user disliked is gone; the surface now feels solid +
  premium.
- **Screenshot chip + clipboard-toggle accent** purple `#a855f7` →
  macOS system blue `#0a84ff`. Purple read as "cyberpunk" on the
  dark-grey HUD surface; system blue is the macOS sidebar / accent
  family and looks calm next to the lime brand mark.

### Fixed

- **Save was blocked when no provider key was edited.** The form
  validation tried to prefix-check an empty string when the saved
  key lived in the keychain (`hasKey=true`, form value empty), so
  saving any unrelated setting (theme, audio, anything) errored
  with "check API key". Empty form-key now means "unchanged, keep
  saved value" and skips the prefix check.
- **Theme picker stayed Save→Close.** `themePreference` was in the
  `currentSerial` JSON body but missing from its `useMemo`
  dependency array, so the dirty-state memo never recomputed when
  the theme button was clicked. Added to deps.
- **Theme picker was visually wedged above the Behaviour heading.**
  Moved into its own `Appearance` Section so the information
  hierarchy stays clean.

---

### Added

- **`/lastsession` slash command.** Reopens the most recent saved
  session into the response area so an accidentally-closed box doesn't
  cost the user the answer they were just reading. Works only when
  Memory → Save sessions is on. Surfaces a friendly "no sessions to
  restore" hint when the workspace is empty.

### Changed

- **Glassmorphism pass tuned to macOS Control Center.** Pill bg alpha
  dropped from 0.86 → 0.58 (dark) and 0.9 → 0.62 (light), backdrop
  blur from `xl` (24 px) → 28 px, saturate from 150 % → 180 %. The
  result is real frosted glass — the desktop, dock, and tooltips
  behind the pill show through softly instead of being hidden under
  what was effectively a flat dark tile. ReducedTransparency setting
  still escapes to fully opaque.
- **Action footer (Read / Save / Copy)** no longer paints itself a
  solid `bg-neutral-900` / `bg-white` block. Inherits the outer glass
  so the row reads as part of the same surface, not a separate
  component glued to the bottom.
- BottomPill backdrop-blur upped to 24 px + saturate 180 % to match.

### Fixed

- Outside-click / outside-typing **no longer auto-dismisses** the box
  once `llmState === 'done'`. The user is reading the answer and
  switching apps to apply it; dismissing destroyed the session and
  forced a re-trigger that re-captured the AIPointer overlay itself.
  The 30 s in-box idle timer is now 120 s post-done so the catch-all
  "user forgot" path still works.
- Voice-mic button auto-submits the transcript instead of appending
  it to the input and waiting for Enter (single-button = single
  action).
- Screenshot + clipboard-toggle chip colors now use Tailwind
  `dark:` variants so the text contrast inverts correctly between
  light system (dark pill) and dark system (white pill). Earlier the
  chip text disappeared on the white pill.

---

Post-release hardening pass after the v1.1.1 wide rollout hit ~300 users
in 72 hours. Focus: kill the Child Mode "describes the wallpaper instead
of opening the game" regression, surface the keychain-failure mode that
was stranding Returning Windows users in onboarding, and rebuild the
prompt-box input model so screenshot and clipboard context are explicit
chips the user controls instead of always-on polling that surprises.

### Added

- **Pre-LLM intent router wired up.** The Child Mode intent router
  (already defined in v1.1.0 but never invoked) now runs before every
  Child Mode query. Phrases like "öffne den Rechner", "ich will
  spielen", "spiele Mozart", "such nach Dinosaurier", "mach lauter"
  dispatch directly to a tool call (launch_app / open_url / play_music
  / search_web / set_volume) without an LLM round-trip. The vision
  LLM never sees them — fixes the "describes the wallpaper" regression
  parents reported.
- **`play_music`, `search_web`, `set_volume` tools.** Cross-platform
  audio control (macOS osascript / Windows SendKeys / Linux pactl);
  Child Mode hard-routes music to YouTube Kids and search to Kiddle.
  Child Mode caps the absolute volume at 70 for hearing protection.
- **Spoken stop-words.** Saying "stop", "halt", "pause", "be quiet",
  "sei ruhig", "shut up" (+ FR/ES/IT/PT/NL equivalents, max 4 words)
  while in the voice-loop halts TTS, stops the mic, and disarms the
  re-arm so the loop pauses until the user presses the mic again.
- **Provider health banner.** When a stored API key cannot be decrypted
  (Windows DPAPI mismatch, macOS keychain revoked, dev-build vs
  production-build code-signing mismatch), a toast sits centered above
  the BottomPill with an "Open Settings" button that drops the user
  straight into the provider section to re-enter the key. Replaces
  the previous "no provider configured" silent failure that looked
  like the Save had simply not worked.
- **Update notification on the BottomPill.** First time an update is
  detected in a session, the pill auto-expands for ~4.5 s with
  "AIPointer ⦿ — New Update Available · click to install", then folds
  to a purple dot. Hover re-expands. Click opens Settings → Updates
  with a live download-progress bar and a Restart-&-install button.
  The tray menu also surfaces the update as its first entry.
- **Screenshot + Clipboard chips above the prompt input.** Purple
  "Screenshot attached" chip shows by default — click `×` to send
  the next prompt without vision context. Green clipboard chip is
  now opt-in via a paperclip toggle button left of the mic. Replaces
  the always-on clipboard poll that was leaking unrelated text into
  unrelated queries.
- **`Key saved ✓` badge in Settings.** Replaces the empty input that
  previously made users think their Save had failed (renderer never
  holds plaintext keys in memory by design). `Replace` button switches
  back to an input. `Test` from the saved-badge state decrypts the
  stored key on demand so the validation actually exercises the right
  value.
- **Independent child-mode voice toggle.** Adult and child voice
  preferences are stored in separate fields (`audioMode` /
  `childAudioMode`). Toggling off in Child Mode actually persists and
  doesn't get overwritten on the next mode switch.
- **⦿ brand mark** trails AIPointer in BottomPill, tray (tooltip +
  Trigger / About / Quit), Settings header, About panel, and remaining
  onboarding strings.
- **CHANGELOG.md** entries documenting every voice command, tool,
  and chip behavior — this very section.

### Changed

- **Voice-mic button auto-submits.** Earlier the non-loop branch
  appended the transcript to the input field and waited for an Enter
  press. A dedicated mic button should be the submit — fixed.
- **`secure-keys` log spam deduped.** Per-session one log line per
  `stage:provider:reason` tuple instead of one per `decryptProviders()`
  call (which fired dozens of times per LLM query).
- **Tray menu surfaces update as first item.** Was buried under
  "Check for updates"; now `🟢 Update available — install vX.Y.Z` is
  the first entry when present.
- **Rectangle selection fill** lowered from 10 % → 6 % opacity so the
  drawn region tints the captured screenshot less.
- **Provider key prefix validation server-side.** Save rejects
  obviously-wrong keys with a clear error per provider (`sk-or-` for
  OpenRouter, `sk-ant-` for Anthropic, `sk-` for OpenAI, 20+
  alphanumeric for Gemini). Previously any string ≥ 8 chars was
  accepted into the encrypted store.

### Fixed

- **Provider health banner click reaches its button.** Banner lives
  inside the click-through overlay; now hit-tests its bounds via the
  same hover/interactivity pattern as the BottomPill so "Open
  Settings" no longer disappears into the underlying app.
- **Provider health banner is centered.** framer-motion's y-animation
  was clobbering the Tailwind `-translate-x-1/2`. Split positioning
  wrapper from the animated element.
- **Provider health banner sits above the pill, not at the menubar.**
  Repositioned from top of overlay to a bottom toast slot 72 px above
  the BottomPill, tracking `pillBottomOffset`.
- **Drag-rectangle suppresses clipboard.** Drawing a region to scope
  the vision context no longer also attaches whatever happened to be
  in the clipboard.

### Reverted

- **Adult-Mode screenshot heuristic.** The v1.1.0-rc shipped a regex
  that dropped the screenshot for prompts that looked like action
  verbs ("open", "play", "search"). Wrong layer: vision context IS
  the product's core differentiator. Adult Mode always attaches the
  screenshot now; the user decides per-query via the chip dismiss.
- **`Ctrl+Shift+Drag` freeform-highlight.** Functionally identical to
  the regular drag-to-screenshot, just modifier-key juggling on macOS
  for no UX gain.

### Known limitations

- **Dev-build keychain mismatch.** macOS `safeStorage` ties encryption
  to the app's code-signing identity. A dev build (ad-hoc signed)
  cannot decrypt ciphertext written by the production-signed binary.
  The provider health banner is the right signal; re-entering the
  key in Settings re-encrypts under the current signature.

---

## [1.1.0] — 2026-05-16

Major feature release. Audience feedback on v1.0.0 landed at 7/10; v1.1.0
targets 10/10 by hardening the first-run experience, adding a true Child Mode
behavior layer, and polishing the visual + interaction layer. The Pillbox UI
itself is byte-identical to v1.0.0 — Child Mode is a pure behavior layer
(system prompt, tool gating, sanitizer, URL whitelist).

### Added

- **Onboarding wizard.** Separate framed window on first launch — 5 steps:
  Welcome + privacy, Mode pick (Adult / Child), PIN setup,
  Autostart + updates, Provider + API key. Cancel quits cleanly; complete
  boots the rest of the app. Keychain prompt fires exactly once, at the
  moment the user explicitly clicks Complete.
- **Child Mode.** Locale-driven safe-browsing layer that produces kid-friendly
  responses (EN/DE), restricts tools to fetch_url / open_url / launch_app,
  validates every outbound URL against a per-language allowlist, swaps the
  system prompt, and stamps a stricter HTML sanitizer schema. PIN-gated
  Settings, /quit, and mode-switch back to Adult. Voice-first ON by default
  with a slower TTS rate (0.9).
- **launch_app tool.** Open whitelisted desktop apps (paint, calculator,
  notes, word, excel, mail, browser) with an approval pill. Cross-platform
  (`open -a`, `start`, `execFile`). Parent-configurable allow-list in Child
  Mode via `childModeAllowedApps`.
- **Voice command router.** Pre-LLM phrase router. Says "open settings" /
  "einstellungen öffnen" / "ayuda" / "aiuto" — fires the matching slash
  command in EN, DE, FR, ES, IT, PT, NL without an LLM round-trip.
  Exact / prefix / Levenshtein-≤-2 matching.
- **Cross-platform autostart.** Toggleable "Start AIPointer at login" with a
  "Start in the tray (no window)" sub-toggle. `app.setLoginItemSettings` on
  macOS / Windows, ~/.config/autostart/aipointer.desktop on Linux.
- **Mouse-wiggle activation.** Wiggle the mouse left-right-left within 700ms
  to summon AIPointer. Disable in Settings → Behaviour (on by default).
- **Left-side hotkey listening.** New hotkey choices: `MetaLeft`, `CtrlLeft`,
  `AltLeft`, `ShiftLeft`, plus `MetaEither` / `CtrlEither` that listen on
  both sides simultaneously.
- **Dynamic pill.** Bottom pill drifts gently toward the cursor with spring
  inertia, never snapping. Pure visual; the pill's hit-test position
  remains centered.
- **Live mode switching.** Adult ↔ Child without restart. System prompt,
  tool registry, sanitize schema, voice defaults all rebuild on the fly.
- **PIN module.** PBKDF2-SHA256 (200k iterations, 16-byte salt, timing-safe
  compare). Plain electron-store storage on purpose — no safeStorage so
  PIN setup during onboarding doesn't trigger a keychain prompt.
- **Per-language whitelist module.** EN + DE kid-safe domain lists with
  optional path-prefix gating (bbc.co.uk → /cbeebies + /bitesize only).
- **Intent router.** Pre-LLM keyword/regex matcher for Child Mode — "I want
  to play" → games URL, "show me pictures of …" → image search, etc.
  Short-circuits to a direct action without an LLM round-trip.
- **XSS test suite.** 15 attack vectors against the child sanitize schema +
  12 host edge-cases against the URL validator + 6 PIN unit tests +
  10 voice-command tests. `npm test` runs them all (vitest + jsdom).
- **AUTHORSHIP.md** (`docs/AUTHORSHIP.md`). Canonical provenance / DNA
  record. Referenced by README, CHANGELOG, and the hidden `/origin`
  slash command. Closes a long-standing dangling reference.
- **Diagnostics.** Hidden `/diagnostics` slash command (Adult mode) prints
  version, provider configuration, permissions state, hotkey, default
  crop, and the last 5 in-memory errors. Useful for support emails.

### Changed

- **Default screenshot crop 1024×768 → 512×384.** ~75% drop in vision token
  cost with negligible legibility loss for cursor-centered content. Crop
  size now reads from settings (`screenshotCropW` / `screenshotCropH`).
  Region selection (drag-rectangle) is unchanged.
- **Glassmorphism restored.** Main prompt box, settings, and bottom pill
  now use `backdrop-blur-xl backdrop-saturate-150` with reduced bg alpha
  and an inner-ring highlight. New `reducedTransparency` setting and a
  CSS-var-based escape hatch for older Intel Macs.
- **Voice-first default in Child Mode.** Runtime override on `audioMode`
  forces it ON when in Child Mode, leaving the stored adult preference
  intact for the switch back.
- **Provider key storage is now lazy.** SettingsGet IPC no longer
  decrypts API keys; the renderer never sees plaintext keys at load.
  Decryption happens only at query time (router) or on an explicit
  `Show key` IPC call. First-run keychain prompt is fully deferred to
  the wizard's Complete step.

### Fixed

- **TTS continues after mic press.** `useTTS` now tracks a `speakId`
  version token; when `stop()` is called mid-fetch, the post-await
  branch checks the token and refuses to start a new audio element.
  Previously a pending `synthesizeSpeech` IPC could resolve and quietly
  restart playback after the user pressed the mic, producing parallel
  voice + recording.
- **Audio-mode loop: mic press during speaking phase.** The loop's
  `armNextRef` is now reset when the user manually presses the mic, so
  it doesn't auto-restart speaking after the user takes control.
- **Keychain prompt on first run before any disclaimer.** SettingsGet IPC
  uses a new lightweight variant (`getSettingsLight`) that never touches
  safeStorage. The first encryptString call happens at the moment the
  user submits their key in the onboarding wizard — with user-active
  context — not at startup.
- **macOS traffic-lights flicker.** Already shipped in v1.0.0 (overlay.ts
  setWindowButtonVisibility + ready-to-show), verified intact.

### Security

- **Child Mode URL validation is defense-in-depth.** The intent router,
  the LLM tool declaration set, and the `open_url` execute-time
  validator each independently enforce the whitelist. A jailbroken LLM
  cannot open an off-whitelist URL because `open_url.execute` rejects it
  with a verbose error that the LLM re-verbalises as a kid-friendly
  redirect.
- **Stricter sanitize schema for Child Mode** drops `<a>`, `<details>`,
  `<summary>`, `<kbd>`, `<mark>`, `<sub>`, `<sup>` entirely and
  restricts class values to a `kid-*` allow-prefix.
- **PIN hashing uses PBKDF2-SHA256** (200k iterations, 16-byte salt,
  timing-safe compare). Stored plain in electron-store config.json by
  design (no safeStorage trigger). Brute-force is offline-resistant up
  to consumer-grade.
- **Provider keys never leave main process plaintext.** The IPC
  surface returns empty strings for `apiKey`; the renderer must
  explicitly call `revealProviderKey` to see one, which fires the
  keychain prompt with full user context.
- **Tools blocked in Child Mode:** `read_clipboard`, `copy_to_clipboard`,
  `reveal_in_finder`, `save_document` — both removed from the
  LLM-visible declarations AND refused at execute time as a safety net.

### Provenance

- DNA / authorship file is now real: see [docs/AUTHORSHIP.md](docs/AUTHORSHIP.md).
- v1.1.0 provenance: `aipointer/v1.1.0/ms/talentsache/skales/20260516`.

---

## [1.0.0] — 2026-05-13

Public-ready release. Single-shot vision Q&A overlay turned into a 4-provider,
agentic, voice-enabled AI cursor companion.

### Added — providers

- **OpenRouter** as the primary provider (Gemini 3 Flash by default, `:online`
  for web-grounded answers).
- **Anthropic** direct integration (Claude Haiku 4.5 / Sonnet 4.6, Messages API
  with full `tool_use` streaming parse).
- **OpenAI** direct integration (GPT-5-mini / GPT-5).
- **Google Gemini** direct integration (Gemini 2.5 Flash / Pro,
  `streamGenerateContent` SSE, function calling, Google Search grounding).
- Automatic provider fallback chain (OpenRouter → Anthropic → OpenAI → Gemini)
  on retryable HTTP errors.
- Smart per-tier model selection: long-form templates auto-bump to the
  capable model.

### Added — agentic tools (function calling, approval-gated)

- `fetch_url(url)` — fetch HTML, extract OG meta + body text.
- `read_clipboard()` — explicit clipboard read.
- `open_url(url)` — open in default browser. Approval required.
- `copy_to_clipboard(text)` — write to clipboard. Approval required.
- `save_document(suggested_name?, content?)` — open native Save dialog. Overlay
  is auto-lowered while the dialog is open so it can't disappear behind the
  always-on-top window. Approval required.
- `reveal_in_finder(path?)` — open Finder/Explorer at workspace path. Approval
  required.
- Inline green-check / red-x approval card surfaced in the response area;
  optional "auto-approve actions" toggle in settings.

### Added — templates (slash commands + implicit prose triggers)

- `/summary` — long structured doc with title, key points, detail, sources.
- `/brief` — TL;DR + 3-5 bullets.
- `/translate` — clean translation block.
- `/explain` — in-short / why / how / example.
- `/code` — answer + fenced code.
- `/improve` — rewritten + change log.
- `/define` — dictionary entry with pronunciation.
- `/solve` — math/logic with stepwise derivation.
- `/reply` — three reply variants (casual / professional / direct).
- `/identify` — bolded ID + confidence + visual reasons + sources.

### Added — voice

- Microphone input via Web Audio MediaRecorder with voice-activity detection
  (auto-stops after 1.5 s of silence). Live pulse-ring intensity indicator.
- Transcription routed through Gemini → OpenAI Whisper → OpenRouter, with
  optional explicit language hint.
- Read-aloud (TTS) for the response: OpenAI `gpt-4o-mini-tts` → Gemini
  `gemini-2.5-flash-preview-tts` → OpenRouter, decoded inline.
- 17-language picker in settings (auto-detect + 16 explicit languages).

### Added — UI & UX

- **Single morphing box** combining input, optional clipboard chip, autocomplete
  dropdown, scrollable response area, and a solid action footer.
- **Bottom pill** that auto-collapses to a status dot and expands on hover;
  click triggers AI mode just like the long-press hotkey. Cursor hit-zone
  detection keeps the click-through overlay responsive.
- **Intro splash** — branded pulsing-halo animation on first launch.
- **Custom About window** (replacing the unreliable macOS system About panel
  for dock-hidden apps).
- **Draggable box** with a grip handle and drag-from-empty-area, viewport-sized
  constraints.
- **User profile** in settings (name + role + preferred reply language) gets
  inlined into the system prompt for personalised replies.
- **System tray** with branded entries: Trigger AIPointer, Settings,
  Privacy Policy, Check for updates, Install update (contextual, appears
  only when a download is pending), About AIPointer v1.0.0,
  Open aipointer.app, Open GitHub repo, Built by Mario Simic,
  Powered by Skales, Quit.
- **Privacy Policy link** in the Settings About disclaimer, opening
  `https://aipointer.app/privacy` via the main process. Disclaimer
  rewritten as three short paragraphs explicitly covering screenshot,
  clipboard, prompt, voice in/out, and profile data flows.
- **8-item FAQ accordion** in settings: how-to-invoke, provider choice, saving,
  commands, privacy, fallback, tools/approvals, voice.
- **Light / dark theme inversion** based on `prefers-color-scheme`, with a
  consistent neutral-gray base (no blue tint).

### Added — memory & persistence

- Multi-turn session continuity within a single AI mode session.
- Optional JSON + Tariq-style A4 HTML transcript saved on dismiss to
  `<workspace>/sessions/`.
- `/history` slash command lists the last 20 saved sessions; click reveals in
  Finder/Explorer.
- Configurable context token budget (default 8 000) with automatic
  oldest-first turn truncation that always preserves the system message and the
  initial screenshot-carrying user message.
- "Clear history" action with file count feedback in settings.

### Added — providers & settings

- Settings UI split into 10 sections: Providers, Behaviour, Memory, Workspace,
  Profile, Input, Capabilities, Updates, About, How to use & FAQ.
- Custom user-selectable trigger hotkey (Right-Cmd / Right-Ctrl / Right-Alt /
  Right-Shift). Setting is read every keydown so changes take effect without
  restart.
- Workspace folder picker with overlay auto-lowering for native dialog
  visibility.
- **Personalize prompts** toggle in the Profile section (default off). When
  off, the user profile is never injected into the system prompt and is
  never sent to the chosen AI provider. GDPR-friendly default.
- **Auto-check for updates** toggle in the Updates section (default on).
  Controls only the scheduled background check; manual "Check for updates"
  from the tray menu works regardless.

### Added — branding & assets

- Programmatically generated 1024×1024 app icon (lime dot in dark capsule with
  halo and specular highlight), rendered from inline SVG via Sharp at startup
  and mirrored to `assets/icon.png` for `electron-builder`.
- Refined tray icon (open ring + filled center dot) as a macOS template image.
- Custom About window with light/dark adaptation, branded links to
  `aipointer.app`, `github.com/gonemedia/aipointer`, `github.com/talentsache`,
  and `skales.app`. Draggable, ESC-dismissible.

### Added — auto-update + release tooling

- Custom auto-updater (no `electron-updater` dependency) that fetches
  `https://aipointer.app/updates/latest.json`, compares semver, downloads
  the platform-specific installer to a temp folder, verifies a 128-char
  hex SHA-512, and hands off to the OS-native installer (Windows NSIS
  silent, macOS DMG open, Linux file manager). First check 30 seconds
  after launch, then once every 24 hours. 401 / 403 / 404 fail silently
  on the background path; manual checks surface a clear error.
- `electron/updater/` module split into `types.ts`, `check.ts`,
  `download.ts`, `install.ts`, `index.ts`. Renderer only sees state
  snapshots over the `updates:state` IPC channel.
- `scripts/release-build.sh` end-to-end release script: raises the open-file
  ulimit, builds all targets, computes hex SHA-512 for every artifact,
  generates `latest.json` and the matching electron-builder yml files
  (`latest-mac.yml`, `latest.yml`, `latest-linux.yml`), organizes
  everything into `release/release-artifacts/` with an upload checklist.
  Supports `--mac`, `--win`, `--linux`, `--no-sign`, `--no-notarize`.
- `electron-builder.yml` rewritten with hardened-runtime entitlements,
  Developer ID Application identity, conditional notarization gated on
  the Apple env vars, and a `publish.url` pointing at the AIPointer
  update channel.
- Build entitlements extended with `allow-jit`, `allow-unsigned-executable-memory`,
  `allow-dyld-environment-variables`, `disable-library-validation`,
  `device.audio-input`, and `automation.apple-events`. Camera left
  explicitly false.

### Added — provenance & DNA

- `AIPOINTER_PROVENANCE` constant exported from the main entry.
- Inline HTML watermark embedded in every saved document via the save template.
- Hidden Tailwind class `ms-aipointer-authentic` referenced from a one-pixel
  span in the About window.
- Hidden `/origin` slash command that prints the authorship card.
- PNG metadata (Software + Author XMP tag) embedded in the generated icon.
- System-prompt provenance directive entangled with the identity block so it
  can't be removed without breaking the bot's self-knowledge replies.
- `docs/AUTHORSHIP.md` documenting all of the above.

### Changed

- System prompt rewritten three times across the sprint, finalised as a
  multi-section directive covering self-knowledge, scope, tool guidance,
  implicit template routing, conversation continuity, and the Skales
  out-of-scope referral.
- System prompt's "Tool output handling" section rewritten as an explicit
  data-vs-instruction contract that covers tool output, screenshot, and
  clipboard inputs.
- Settings storage migrated from a single `openrouterApiKey` legacy field to a
  full `providers` object with per-provider toggle + key, plus
  `userProfile`, `voiceLanguage`, `triggerHotkey`, `memorySaveEnabled`,
  `maxContextTokens`, `autoApproveTools`, `personalizePrompts`, and
  `autoCheckUpdates`.
- Provider router refactored from a single OpenRouter-only client into a
  pluggable `ProviderClient` interface with four implementations sharing a
  common streaming-tool-call accumulator.
- Production Content Security Policy is now strict and provider-specific.
  Allowed `connect-src` hosts: openrouter.ai, api.anthropic.com,
  api.openai.com, generativelanguage.googleapis.com, aipointer.app.
  Production drops `unsafe-eval` from `script-src`. Dev keeps a permissive
  policy in `renderer/index.html` for Vite HMR; production is injected by
  a `transformIndexHtml` plugin at build time.
- `fetch_url` tool now sends a generic Chrome User-Agent per platform so
  fetched pages cannot fingerprint that the request came from AIPointer.
  Provider API calls keep the AIPointer UA where attribution is
  appropriate.
- Tool output wrapping: removed the heuristic regex array that prepended
  a warning header when output "looked like injection". The delimiters
  paired with the system prompt's data-vs-instruction directive are the
  actual defense; the heuristic gave false confidence.

### Security

- Removed an auto-approve bypass that affected destructive tools.
  Read-only tools can still skip the prompt; `open_url`,
  `copy_to_clipboard`, `save_document`, and `reveal_in_finder` always
  require explicit approval.
- Added an SSRF guard to `fetch_url`. DNS-resolved hosts in loopback,
  RFC1918, link-local, ULA, CGNAT, multicast, or cloud-metadata ranges
  are rejected. Manual redirect follow re-checks each hop.
- `reveal_in_finder` is now workspace-bound. Paths outside the configured
  workspace are rejected and the tool falls back to the workspace root.
- API keys are stored via Electron `safeStorage` (Keychain on macOS,
  DPAPI on Windows, libsecret on Linux). Replaces the previous hardcoded
  electron-store encryption key. A per-install random envelope key is
  used for non-key settings.
- Process-level `unhandledRejection` and `uncaughtException` handlers in
  main so a stray network rejection does not silently terminate.
- Content Security Policy meta tag on the renderer (see Changed for the
  production whitelist).
- `will-navigate` and `will-redirect` handlers on overlay windows refuse
  any URL that is not the dev server or the bundled `dist/index.html`.
  Http(s) destinations are routed through `shell.openExternal`.
- 401 / 403 from a provider no longer trigger the fallback chain. The
  auth error surfaces immediately so the user fixes the key in
  `/settings` instead of burning quota across other providers.
- Microphone entitlement is enabled in the macOS build so signed builds
  can capture audio for voice input.
- `copy_to_clipboard` clamps writes to 100 KB and strips ANSI / control
  characters.
- `save_document` sanitizes `suggested_name` before passing it through to
  the native dialog.
- `open_url` approval prompt now shows the full `scheme://host` in the
  prominent label, not only the host, to defeat a
  `google.com.attacker.com` style display trick.
- Overlay windows ship with `sandbox: true` in packaged builds. Dev keeps
  `sandbox: false` for HMR.
- Markdown sanitizer no longer accepts the `style` attribute. Closes a
  background-image exfiltration path that would survive a strict
  `connect-src` because `img-src` needs `data:` for inline images.
- Session metadata stores paths relative to the workspace folder. The
  reveal-in-finder IPC handler resolves the relative path against the
  workspace and refuses any input that escapes the boundary.
- Stream readers in all four providers (OpenRouter, OpenAI, Anthropic,
  Gemini) respect AbortController cancellation. ESC during a stream
  cancels the reader immediately instead of letting the server drain.

### Removed

- The original "settings modal" pattern (a small centered dialog) — replaced
  with the full 720 × 720 settings panel.
- Per-query "primary / fallback model" pickers — replaced with automatic
  per-provider model selection so the user never has to think about model IDs.

### Fixed

- macOS Save dialog appearing behind the always-on-top overlay (overlay is now
  auto-lowered for the dialog's lifetime).
- Light/dark theme regression where the overlay's dark glass would invert
  incorrectly on certain `prefers-color-scheme` transitions — now driven by an
  explicit `.dark` class on `<html>`.
- Slash command Enter required two presses to execute — now executes on a single
  Enter when the input exactly matches a command.
- Bottom pill not responding to hover/click despite `pointer-events-auto`
  because the parent window was click-through — now toggles
  `setIgnoreMouseEvents` based on cursor hit-zone.
- Drag constraints too tight (~360 px) — now viewport-sized, free drag within
  the display.

### Added — late v1.0.0 polish (same-day 2026-05-13)

- **Region screenshot gesture.** While the box is open, holding the trigger key
  again switches the overlay into selection-arm mode (cursor → crosshair, hint
  pill "Drag to capture a region"). Left-mouse drag draws a lime-bordered
  selection rectangle with live size readout; release captures exactly that
  region (via `sharp.extract` against the display thumbnail) and replaces the
  default 1024×768 cursor-centered crop for the current query. ESC mid-drag
  cancels cleanly without dismissing the box. Multi-display aware — the rect's
  display-local coords convert through `display.workArea` offsets.
- **Voice-first conversation mode** (Settings → Behaviour, default off). When
  on, triggering AIPointer opens the box already listening: VAD-stopped
  transcript auto-submits, the response is read back via TTS, the mic
  re-opens for the next turn. A compact phase pill below the drag handle
  shows the current leg (Listening → Thinking → Speaking → Done · say a
  follow-up). The mic-level pulses on the Listening dot. ESC breaks the
  loop. Robust against double-arm, double-TTS, and stale closures via
  three guard refs (`audioActiveRef`, `spokenResponseRef`, `armNextRef`).
- **Per-provider key tester.** Each provider card in Settings has a `Test`
  button next to the API-key input. The main process hits a provider-specific
  validation endpoint via Electron's `net` module (OpenRouter `/credits`,
  Anthropic `/models`, OpenAI `/models`, Gemini `/models?key=`) and surfaces
  the result inline (✓ valid, ✕ with error message). For OpenRouter the
  success label includes the remaining balance, e.g. "$3.42 left", parsed
  from `total_credits - total_usage`. The result is invalidated automatically
  when the user edits the key.
- **Gemini-inspired cursor halo.** The single lime gradient was replaced with
  a three-layer composition: outer brand-lime glow (8 px blur, dezenter than
  before), middle near-white luminous ring, inner 6 × 6 px white pixel-pip
  with a thin lime outline. Anchors the eye to the exact OS cursor pixel
  even on busy backgrounds while staying brand-consistent.
- **Live clipboard polling.** While AI mode is active, the main process polls
  the OS clipboard every 400 ms via a dedicated `pollClipboardIfChanged()`
  helper. New copies (Cmd+C in another app) surface the clipboard chip in
  the box within half a second. Clicking the X on the chip "dismisses" the
  current clipboard — polling and the next submit both ignore it until the
  text actually changes.
- **Tray-menu cleanup.** Removed the "Toggle DevTools" entry (only useful for
  developers; end users shouldn't see it). DevTools remain accessible via
  `Cmd+Opt+I` when needed.
- **/help expanded** with sections on "What AIPointer sees" (screenshot +
  clipboard + region awareness), "What AIPointer can do" (tools with auto vs
  approval markers), "Gestures" (full keyboard/mouse map), and "Privacy".
- **FAQ expanded** with 8 new entries: region screenshots, what AIPointer
  sees, tool capabilities, multi-display behaviour, power/perf impact,
  uninstall paths, keyboard shortcuts, voice-first mode.

### Fixed — late v1.0.0 polish (same-day 2026-05-13)

- **Flicker root-cause hunt.** Three independent contributors found and
  killed:
  - `useLinkMetas.reset` returned a fresh function identity on every render,
    so the dependent `useEffect([hasResponse, resetMetas])` fired in a tight
    loop (Maximum update depth exceeded). `reset` is now `useCallback`-
    stabilised and `setMetas` is idempotent on empty state.
  - `motion.div` in PromptInput had `layout` (= true), spring-animating
    every height change. With the new 400 ms clipboard poll surfacing the
    chip on a delay, the box pumped vertically. Changed to `layout="position"`
    so position changes still animate but size changes apply instantly.
  - `setOverlayInteractive` in the main process called
    `setIgnoreMouseEvents` + `setFocusable` + `focus()`/`blur()` on every
    invocation, even when the state hadn't changed. On macOS each of those
    triggers a window-frame repaint. Now caches the last-applied state per
    display and no-ops if unchanged. Window is also permanently focusable
    instead of toggled per hover.
- **Cursor-hit-test hysteresis.** Box hover test now uses 14 px halo to
  enter, 80 px to leave. Without hysteresis cursor wobble at the box edge
  flipped `boxHovered` every frame, toggling `setIgnoreMouseEvents` and
  causing visual flicker. The wider leave-halo also covers the natural lag
  between cursor position (33 ms IPC) and box position during a fast drag.
- **Drag-then-input dead.** After dragging the box once, the input field
  became unclickable and a second drag was impossible. Root cause: the
  forwardedRef pointed to the OUTER motion.div (positioned at the entry
  anchor via `x, y` props), but drag-translation is applied to the INNER
  motion.div via CSS transform. `getBoundingClientRect()` on the outer
  returned the original anchor position, so the App.tsx hit-test thought
  the cursor was outside the box, flipped `boxHovered` to false, and the
  overlay went click-through. Moved `ref={forwardedRef}` onto the inner
  motion.div so its bounds follow the visible (transformed) position.
- **Settings won't open on multi-display.** The `onSettingsOpen` IPC
  listener filtered on `isPrimary`, so requests from the tray menu silently
  opened settings on the primary display when the user was on a secondary
  one. The filter is gone — settings opens on whichever overlay's renderer
  receives the broadcast first.
- **Splash blocked browser access on first run.** First-run UX auto-opened
  Settings on top of the splash, so the user couldn't reach Chrome to copy
  their OpenRouter key. Auto-open now waits for `splashDone`, and an
  `OverlaySettingsMode` IPC lowers every overlay's `alwaysOnTop` level
  while Settings is open so Cmd+Tab to a browser actually works.
- **Outside-click dismiss didn't fire on a click-through overlay.** The
  React backdrop handler was useless because `setIgnoreMouseEvents(true)`
  forwarded the clicks to the underlying app. Now the main-process uiohook
  mouse handler reads the current box bounds (broadcast from the renderer
  on every layout change) and decides single- vs double-click dismiss
  itself, governed by the new `dismissOnDoubleClick` setting (default on).
- **TTS-on-OpenRouter-only.** `useTTS` falls back to the Web Speech API
  whenever the provider TTS call errors (OpenRouter has no `/audio/speech`
  endpoint at all). The Read-aloud button now always does something useful.
- **White-on-white markdown in dark mode.** `html.dark { color: ... }` in
  `globals.css` overrides the cascade so streamed Markdown is readable on
  the white card.
- **Clipboard-context auto-fills despite explicit dismiss.** X-clicking the
  chip now stores the dismissed text as `dismissedClipboard` and both the
  poller and the next `runQuery` skip it until the OS clipboard contents
  change. No more sneaking dismissed text back into prompts.

---

## [0.3.0] — 2026-05-13

Pre-v1.0 polish + agentic foundation.

### Added

- Initial slash command system with autocomplete dropdown (5 commands).
- First six templates (`/summary`, `/brief`, `/translate`, `/explain`,
  `/code`, `/improve`).
- OpenRouter `:online` routing for web-grounded answers.
- Link card previews with OG image fetching.
- HTML / Markdown / PDF document save via native dialog.
- Markdown response rendering with `react-markdown`, GFM, and sanitised inline
  HTML for richer Gemini outputs (tables, callouts, cards).
- Tariq-style A4 HTML save template with print-ready CSS and KPI / card / step
  primitives.

### Changed

- Response rendering moved from plain text to Markdown + selective HTML inline.
- Save / Copy buttons moved from a floating absolute overlay into a solid
  footer row beneath the response so they no longer cover the text.

### Fixed

- Settings modal "navy/lila" colour (saturated slate) replaced with neutral
  gray.

---

## [0.2.0] — 2026-05-13

### Added

- Tool calling foundation: shared agentic loop, JSON-schema tool declarations,
  inline activity rendering ("Reading example.com", "Saving document").
- Workspace folder picker.
- Session save (JSON + HTML) toggle.

### Changed

- Box position pinned to the cursor at trigger time, then user-draggable.
- Settings refactored from "modal" to full panel.

---

## [0.1.0] — 2026-05-13

Initial scaffold (Phase 1 per the original brief).

### Added

- Electron + React + TypeScript + Vite project structure with
  `vite-plugin-electron`.
- Transparent always-on-top overlay window, one per display.
- Long-press global hook via `uiohook-napi` (Right-Cmd hold for 200 ms or
  right-mouse hold for 250 ms).
- 60 Hz cursor tracking and the lime-green halo with spring-animated follow.
- Vision Q&A pipeline: screenshot capture (1024 × 768 around cursor),
  base64 JPEG, clipboard context, OpenRouter streaming chat completions.
- macOS Accessibility + Screen Recording permission detection and prompts.
- Single OpenRouter-only provider with `gemini-3-flash-preview`.

---

[1.0.0]: https://github.com/gonemedia/aipointer/releases/tag/v1.0.0
[0.3.0]: https://github.com/gonemedia/aipointer/releases/tag/v0.3.0
[0.2.0]: https://github.com/gonemedia/aipointer/releases/tag/v0.2.0
[0.1.0]: https://github.com/gonemedia/aipointer/releases/tag/v0.1.0
