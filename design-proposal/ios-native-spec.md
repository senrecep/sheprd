# iPhone app: native styling with small status changes

Keeps the current native iOS 26 look. Line references are from 3005f801.

## Status palette (shared `Status` enum in `ios/Shared/`)

| State | Tint | SF Symbol | Word |
|---|---|---|---|
| Needs you | `Attention` Color Set | `exclamationmark.circle.fill` | Needs you |
| Working | `.secondary` | `ellipsis` | Working |
| Done | `.secondary` | `checkmark` | Done |
| Failed | `.secondary` | `xmark.octagon.fill` | Failed |
| Idle | none | none | Idle |

Each state has its own symbol silhouette (filled disc, dots, check, octagon), so the states differ without color. Only the needs-you glyph is colored; the others stay monochrome in `.secondary`. `Attention` is systemOrange with its increased-contrast value (#C93400) in light and systemOrange in dark, so 13 pt text passes 4.5:1 (5.3:1 light, 8.3:1 dark). Orange is used only for "needs you": the waiting glyph, the wait time after 5 minutes with its clock symbol, and the Needs you count. The other orange uses move to `.secondary` with their icon kept, or `.red` where something has actually failed.

The enum replaces both of today's status views on Home: `StatusMark` (`Parts.swift:81-103`, a red dot for needs you, `ellipsis`, `checkmark`) and `StatusBadge` (`Parts.swift:107-126`, `exclamationmark.circle.fill`, `ellipsis.circle.fill` with `.pulse`, `checkmark.circle.fill`). Today the same state looks different on chat rows and terminal-agent rows. Used by the app (`Views/Parts.swift`), widgets (`SheprdRemoteWidgets/Parts.swift`) and the Watch.

Failed: chats already get it from `summary.status` (`.failed(message:)`, `Chat/Transcript.swift:117`), which Home rows ignore today. Terminal agents (`DetectedAgent.status`) have no failed state until the daemon sends one.

## Home

- Large title "Home" (`.navigationTitle("Home")`, `.large`; today it is inline with no title, `HomeTab.swift:15`).
- Priority sort: Needs you, Working, Idle, Done (`AgentSort.swift:50`).
- Needs you on top in every sort; hidden when empty.
- Only the Needs you header has a count ("Needs you 2"), in the attention color; VoiceOver reads "Needs you, 2 agents". It equals the rows in that section. The tab badge is changed to that same number: today `.badge(model.fleet.waiting)` (`RootView.swift:142`) counts permission requests across chats, not agents, and leaves out blocked terminal agents. Other headers have no count.
- Needs-you cards and rows show the wait time as "waiting 2m" (Footnote, `.primary`). After 5 minutes it adds `clock.fill`, turns semibold and uses the attention color. VoiceOver reads "Waiting for you, 2 minutes". Chats need `waiting_since` from the daemon; terminal-agent rows can show it now from `changed_ms` (`DetectedAgent.changedMs`).
- `AnswerCard` drops the tinted border; its buttons become `.bordered` with `.tint(.primary)`, and allow-once `.borderedProminent` in the accent, at least 44 pt tall. Today allow-once is `.glassProminent` tinted with the needs-you color (`Parts.swift:255`), the others `.glass` (`:257`, `:264`).
- Text styles only: Body for row titles, Headline for card titles, Subheadline for the meta line, Footnote for ages. Status glyphs scale with the text.
- Dynamic Type: at `dynamicTypeSize >= .accessibility1` titles and meta wrap (`lineLimit(3)`) instead of truncating, the age and glyph move under the title, and the answer buttons stack vertically (`ViewThatFits`).

## Contrast

- Text reaches 4.5:1 (3:1 for 17 pt semibold and up) in light, dark and Increase Contrast, and status glyphs stay readable with Increase Contrast.
- The prominent button keeps white text at 3:1 or more. systemBlue's dark high-contrast value (#409CFF) gives 2.8:1 with white, so the accent is an `AccentColor` set that matches system blue but keeps #0A84FF there.
- Open question for the owner: the meta line and quiet ages use the system secondary label color, about 3.4:1 on white in light. "Use the system label colors" and "4.5:1 for text" disagree here. Options: keep `.secondary` (native), or use `.primary` for the meta line.
- The tab bar's selected label and its badge are drawn by the system and keep its colors.

## Agent logos

Every tile is `.fill.secondary` with a `.primary` template mark, as opencode, amp and pi already are (`Parts.swift:13,19-22`). Same for the widget `AgentBadge` (`SheprdRemoteWidgets/Parts.swift:22`).

## Pull requests

`PullRequestPill` shows the tone word after the number ("#1083 Ready").

## Liquid Glass

Only on the functional layer: nav bar buttons, tab bar, composer. The offline and nearby banners become the tab bar's bottom accessory (`tabViewBottomAccessory`; today a glass capsule in a `safeAreaInset`, `ConnectionToast.swift:135`), so they are part of the tab bar chrome and the list insets above them at every text size. Rows, headers, answer cards and their buttons are opaque; with Reduce Transparency the chrome is solid.

## Motion

When an agent starts needing you, its row moves once from its current place to Needs you with a short spring (a cross-fade under Reduce Motion) and one notification haptic fires on that frame. Nothing animates or buzzes on refresh, a row update or an age tick. The waiting and failed glyphs have no repeating effect; the working glyph's effect (`.variableColor.iterative`, `Parts.swift:93`) stops under Reduce Motion (`isActive: !reduceMotion`).
