# Sheprd redesign: design spec (unofficial proposal)

Not made by the Sheprd team. Paths are relative to the sheprd repo at d8db7e83. Mock: {{MOCK}}.

Each agent that waits on you is a ticket on a rail. The ticket shows agent, machine, workspace and wait time. You answer on the ticket and it leaves the rail. The metaphor lives in shape only (perforated edge, mono header, rail band, age bar, done stack). Copy stays as it is today, with two changes: on iOS "Recent" becomes "Done" (done chats only), and on the web the "Blocked" status word becomes "Needs you". New labels are "Failed" and "High". Answer buttons keep the agent's own option names, such as Allow once and Deny.

Goal of this proposal: one system across phone, widgets, web app and site, with the same status language and colors everywhere.

## 1. Tokens

### Status palette (one set for iOS, widgets, Watch, web app, site)

Each state has a word and a shape. Color is never the only signal.

| State | Shape | Light | Dark | Contrast on ticket (L / D) |
|---|---|---|---|---|
| Needs you, text | filled triangle with ! | `#a54515` | `#f49a5e` | 5.5 / 7.3 |
| Needs you, fill (button, top stripe), ink text `#141210` | same | `#e8742f` | `#ef8443` | 6.2 / 7.2 (ink on fill) |
| Working | ink dot with a pulse ring | ink `#141210` | ink `#eee9df` | 16.9 / 13.1 |
| Done | check, struck text, stacked | `#6b665f` | `#9a948a` | 5.1 / 5.3 |
| Failed | filled square with x | `#bf3326` | `#ff7262` | 5.1 / 5.9 |
| Idle | open circle | `#6b665f` | `#a39d93` | 5.1 / 5.9 |

Ticket surface: light `#f6f3ec` (thermal paper) on a `#e1ddd5` screen with a `#cdc8be` rail. Dark `#24221f` (steel) on `#0f0e0d` with a `#1f1d1b` rail. Ink light `#141210` / `#4f4a44` / `#6b665f`, dark `#eee9df` / `#bbb5ab` / `#9a948a`. Rule light `#d3ccbf`, dark `#3d3934`.

Age: a mono `m:ss` timer and a 3 pt bar that fills over 5 minutes. Both use ink, and switch to heat orange once the wait passes 5 minutes. No green, amber or red ramp.

Orange means "Needs you" and nothing else. CPU or memory load at 80% or more shows in bold with the word "High", not in orange.

### Radius, type, motion

- Radius: 4 (tickets, stamps, tags, PR state), 10 (buttons, fields, option rows), 16 (sheets). Remove 8/12/14/18/20/22/26/44 and the size x 0.28/0.3 tile radii (`Views/Parts.swift:267`, `Views/SessionSheet.swift:282`, `Views/WorkspaceScreen.swift:120`, `Nearby/ProximityCard.swift:65`, `Views/Composer.swift:41,233`). System shapes (tab bar capsule, widget corners, device) stay as the system draws them.
- Type: SF Pro through Dynamic Type styles for all UI text. SF Mono (`.monospaced()`) is used only for the ticket header (caption2, semibold, uppercase, +0.04 em), the age timer (caption, `.monospacedDigit()`), branch names and commands.
- Motion: answering tears the ticket off. It lifts 10 pt and tilts 2 degrees, then slides off the rail, 300 ms ease-out (`.spring(duration: 0.3, bounce: 0)`), with `.sensoryFeedback(.success)`. The rail then closes the gap: the next ticket slides left under it (mock, still frame at 180 ms). The haptic fires on tap. With Reduce Motion it is a 200 ms crossfade. The working dot pulses at 1.6 s and stops under Reduce Motion.

### SwiftUI

- Add `AccentColor` (`#a54515` any, `#f49a5e` dark) and the colorsets to `SheprdRemote/Assets.xcassets` and `SheprdRemoteWatch/Assets.xcassets`. Add a widget asset catalog (or a shared one in `ios/Shared/`) to the SheprdRemoteWidgets sources with the colorsets and `Agents/*`, and set `ASSETCATALOG_COMPILER_GLOBAL_ACCENT_COLOR_NAME: AccentColor` for all three targets in `ios/project.yml`. This replaces system blue (`.tint` at `Views/ChatScreen.swift:111,154,290`, `Views/NewSheets.swift:257,259,309`, `Views/SessionSheet.swift:114,116`, `Views/EntryView.swift:13`, `Views/WorkspaceScreen.swift:185`, `Views/TerminalKeys.swift:186`).
- Add colorsets `Paper`, `Ink`, `Ink2`, `Ink3`, `Rule`, `Rail`, `Heat`, `HeatText`, `Failed`, `Idle`, each with light and dark values from the table.
- Add a shared `Status` enum in `ios/Shared/Status.swift`, and add that path to the SheprdRemoteWatch sources in `project.yml`:

```swift
enum Status { case needsYou, working, done, failed, idle, unknown
  var color: Color { switch self {
    case .needsYou: .heatText; case .working: .ink
    case .done, .idle, .unknown: .ink3; case .failed: .failed } }
  var symbol: String { switch self {
    case .needsYou: "exclamationmark.triangle.fill"; case .working: "circle.fill"
    case .done: "checkmark"; case .failed: "xmark.square.fill"; case .idle: "circle"
    case .unknown: "questionmark.circle" } }
  var label: LocalizedStringKey { /* "Needs you", "Working", "Done", "Failed", "Idle", "Unknown" */ } }
```

It replaces `.red/.yellow/.teal/.green` in `Views/Parts.swift:70-77`, the widget `Palette` in `SheprdRemoteWidgets/Parts.swift:7-9`, the Watch dot colors in `SheprdRemoteWatch/SheprdRemoteWatchApp.swift:59-98`, and the PR tones in `Views/WorkspacesTab.swift:360-370` (the six existing tones keep their `symbol` and show their `label` as a visible word; no purple, no orange).

Mapping: `Home.AgentStatus.blocked` to needsYou, `.unknown` to unknown, the rest by name; `ChatActivityState.Phase.question` to needsYou, `.finished` to done, `.failed` to failed.

### Web app (`crates/sheprd-remote-control/src/web/assets/input.css:10-55`)

```css
:root{ --bg:#e1ddd5; --rail:#cdc8be; --surface:#f6f3ec; --raised:#ebe7de; --line:#d3ccbf;
  --text:#141210; --muted:#4f4a44; --faint:#6b665f;
  --accent:#a54515; --heat:#e8742f; --on-heat:#141210;
  --st-needs:#a54515; --st-working:#141210; --st-done:#6b665f; --st-failed:#bf3326; --st-idle:#6b665f;
  --r1:4px; --r2:10px; --r3:16px; }
@media (prefers-color-scheme:dark){ :root{ --bg:#0f0e0d; --rail:#1f1d1b; --surface:#24221f;
  --raised:#2d2a27; --line:#3d3934; --text:#eee9df; --muted:#bbb5ab; --faint:#9a948a;
  --accent:#f49a5e; --heat:#ef8443; --st-needs:#f49a5e; --st-working:#eee9df;
  --st-done:#9a948a; --st-failed:#ff7262; --st-idle:#a39d93; } }
```

Replace the color tokens in both scheme blocks (`input.css:10-31` dark and `:33-55` light) and the `@theme inline` color mappings (`:57-74`). Keep `--term-*` and `--t0` to `--t15` (ANSI). This removes the iOS system greys (`:11-17`, `:35-41`), the periwinkle `--accent` (`#7c9cff` at `:18`, `#3b5bdb` at `:42`), the Catppuccin agent-status set (`:24-26`, `:48-49`) and the PR orange and purple (`:22-23`, `:46-47`). `--amber`/`--green`/`--red` collapse into the status tokens, so the Tailwind mappings at `:66-68` and the `bg-/text-amber|green|red` classes in `render/*.rs` and `app.js` move to them. Map `--st-blocked` to needs and keep an `--st-unknown`; update `render/status.rs:40,42`, `input.css:118,128,131,135` and `render/tests/lists.rs:112`. Manifest `theme_color`/`background_color` (`assets/manifest.webmanifest:10-11`) and the `shell.html:6-7` theme-color metas become `#0f0e0d` dark and `#e1ddd5` light. Font stays the system stack, with `ui-monospace` for headers and ages.

### Site (`website/site/assets/site.css:2-19`)

`--paper:#e1ddd5; --paper-2:#f6f3ec; --ink:#141210; --ink-2:#4f4a44; --rule:#d3ccbf; --orange:#a54515; --orange-fill:#e8742f;` plus a dark set equal to the web app. Smallest version: keep the serif, paper and chapter layout chosen in #1028 and only align the orange and status marks (`site.css:74`: `--working: #f39a5a`, `--blocked: #f0705a`). Fuller version (shown in the mock): the serif stack (`site.css:15`) becomes the system sans for body, with `ui-monospace` for ticket headers and ages. Site headline and wordmark use Big Shoulders Display 800, uppercase (SIL Open Font License 1.1), self-hosted as woff2 in `website/site/assets/fonts/`, fallback `"Arial Narrow", sans-serif`, no third-party font request. iOS keeps SF Pro. The `base.css:13` focus ring (`#d65f2a`) becomes `--orange-fill`. The `Illustration` tag (`base.css:19`) moves from orange to ink-2 with a dashed rule.

## 2. Components

| Component | Replaces | Notes |
|---|---|---|
| `Ticket` | `AnswerCard` `Views/Parts.swift:228-282`, `.card()` `Views/SessionSheet.swift:279` for attention | Paper surface, perforated top edge (a mask of 3 pt half circles every 10 pt), r4 bottom corners. Header row: agent logo (template, ink), `AGENT · MACHINE`, age at the trailing edge. Then the status word, workspace (mono), question, actions, `AgeBar`. "Needs you" adds a 3 pt heat stripe under the perforation. Allow once and Answer = heat fill with ink text. Deny = 1 pt ink outline. No `.glass`/`.glassProminent` on answers (`Parts.swift:251-260`). "Always" keeps Face ID. |
| `RailView` | "Needs you" section of `ChatList` `Views/ChatList.swift:16-20,47` | Horizontal `ScrollView` with `.scrollTargetBehavior(.viewAligned)`, 300 pt tickets, next ticket peeks. Flat warm-steel band (`Rail`) behind it, with no gradient or highlight. The only depth is a soft shadow under each ticket. Oldest wait first. |
| `TicketStub` | `ChatRowLabel`, `AgentTerminalRowLabel` `Views/Parts.swift:128,167` | Working, failed and idle rows, perforated on the leading edge, grouped under a mono machine header (`MACHINE  SSH`). Trailing: status word and shape, then `LastAnswer` (`:205`) unchanged. |
| `AgeBar` | new | 3 pt, ink, fills over 5 min, heat past 5 min. Hidden when `waiting_since` is missing. |
| `MonogramStamp` | `RepositoryAvatar` `Views/WorkspacesTab.swift:257-270` | 34 pt r4 square, double 1.5 pt ink rule, two mono capitals from the repo name. Ink on paper only. No hashed hue, no `.gradient`. |
| `SpikedStack` | "Recent" section, which becomes "Done" (done chats only; idle chats move to stubs) | One collapsed row: check, "Done", count, the last title struck through, with three offset paper edges below it (rotated 0.6 to 1.6 degrees). Tap to expand into the normal list. |
| Agent logo | `AgentIcon` `Views/Parts.swift:7-37`, widget tiles `SheprdRemoteWidgets/Parts.swift:37-41` | Logo stays, rendered as a template in ink. No brand-color tile, no letter fallback in widgets (use `agent-generic`). |
| Composer | `Views/Composer.swift:41,233` | Flat paper field r10 with a 1 pt rule, no glass. Send/stop stays a 44 pt circle. |
| Load gauge | `LoadGauge` `Views/WorkspacesTab.swift:210,219` | 54×4 pt ink bar plus a tabular percentage, and "High" in bold at 80% or more. |

Liquid Glass stays on system bars only: tab bar, navigation bar, sheets. Remove it from `ConnectionToast` (`Views/ConnectionToast.swift:109`), `NearbyBanner` (`Nearby/NearbyBanner.swift:37`), `Composer` (`Views/Composer.swift:41,233`), `RunningCapsule` (`Views/ChatScreen.swift:327`), `ForgetComputer` (`Views/ForgetComputer.swift:83`) and `WorkspaceScreen` (`Views/WorkspaceScreen.swift:120`), and use the paper surface.

## 3. Dependency: `waiting_since`

Today the remote control server sends last activity only (`LastAnswer` shows now/5m/3h). There is no time at which the agent started waiting. The age timer and `AgeBar` need the server (`crates/sheprd-remote-control`) to send `waiting_since` (RFC 3339 or epoch ms) for every agent in the "needs you" state, in `home` and `chat_changes` (`/api/v2/live`). The app then copies it into `WidgetSummary` (`summary.json`, `ios/Shared/WidgetSummary.swift`) for widgets and the Watch. If the core does not expose when an agent started waiting, that has to come first. Until then the ticket shows `LastAnswer` and no bar. Ages in the mock are illustrative.

## 4. Accessibility

- VoiceOver: each ticket is one element. Needs you: "Claude on build-box needs you. ledger, fix/rounding. Run cargo test? Waiting 6 minutes." Actions as `.accessibilityActions` (Allow once, Deny, Answer). Working: "Codex on MacBook Pro, working, last activity now." Failed: "Pi on build-box failed, 14 minutes ago." Idle and Done follow the same pattern. Status marks get labels (promised in `docs/ios-app-plan.md`).
- Dynamic Type: the header and question wrap. At accessibility sizes the rail becomes a vertical stack, and the actions stack.
- Reduce Motion: tear-off becomes a crossfade, the pulse stops, `WorkingOrbit` keeps its static frame (`Views/Parts.swift:42-64`).
- Contrast: all text pairs in the table are AA (4.5:1) or better on their surface, in both modes. Text on the screen background uses ink or ink-2 only; ink-3 and heat-ink are for paper. Increase Contrast swaps ink-3 for ink-2.
- Targets: every action is at least 44×44 pt. The space between Allow once and Deny is 8 pt.

## 5. Phases

1. Tokens and tint: colorsets, `AccentColor`, `Status` enum in the app, widgets and Watch; radius scale.
2. Ticket and rail: `Ticket`, `RailView`, `TicketStub`, `SpikedStack`, `MonogramStamp`, composer; remove glass from small chrome. `AgeBar` ships behind `waiting_since`.
3. Outside the app: Live Activity and lock screen (`SheprdRemoteWidgets/LockScreenView.swift`, `AnswerButtons.swift`), widgets (`AttentionWidget.swift:91-119`), Watch.
4. Web app: tokens in `input.css`; on Home (`render/home.rs:92-169`) the strip (`:172-205`) becomes the rail; on Board (`:207-272`) Needs you becomes the rail, Working rows become stubs and Idle becomes one collapsed stack. New workspace and the Home/Board switch (with its waiting dot) move from the `.glass` toolbar (`render/home.rs:309-322`) into the header row next to the existing settings link.
5. Site: orange and status marks in `site.css`, 390 nav without wrapping; optionally the fuller version with the hero rail. Install commands stay as the site has them (see #1035).
