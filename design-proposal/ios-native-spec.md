# iPhone app: native styling with small status changes

Keeps the current native iOS 26 look. Line references are from 3005f801.

## Status palette (shared `Status` enum in `ios/Shared/`)

| State | Color | SF Symbol | Word |
|---|---|---|---|
| Needs you | `Attention` Color Set | `exclamationmark.circle.fill` | Needs you |
| Working | `.yellow` | `ellipsis` | Working |
| Done | `.teal` | `checkmark` | Done |
| Failed | `.red` | `xmark.octagon.fill` | Failed |
| Idle | none | none | Idle |

Each state has its own symbol silhouette (filled disc, dots, check, octagon), so the states differ in grayscale too. `Attention` is systemOrange with its increased-contrast value (#C93400) in light and systemOrange in dark, so 13 pt text passes 4.5:1 (5.3:1 light, 8.3:1 dark). Orange is used only for "needs you": the waiting glyph, the wait time after 5 minutes and the Needs you count. Used by the app (`Views/Parts.swift`), widgets (`SheprdRemoteWidgets/Parts.swift`) and the Watch.

## Home

- Large title "Home" (`.navigationTitle("Home")`, `.large`; today it is inline with no title, `HomeTab.swift:15`).
- Priority sort: Needs you, Working, Idle, Done (`AgentSort.swift:50`).
- Needs you on top in every sort; hidden when empty.
- Only the Needs you header has a count ("Needs you 2"), in the attention color; VoiceOver reads "Needs you, 2 agents". It equals the rows in that section and the system tab badge (`.badge(model.fleet.waiting)`). Other headers have no count.
- Needs-you cards and rows show the wait time as "waiting 2m" (Footnote). After 5 minutes it adds `clock.fill`, turns semibold and uses the attention color. Needs `waiting_since` from the daemon.
- `AnswerCard` drops the tinted border; its buttons become `.bordered` with allow-once `.borderedProminent` in the default accent, at least 44 pt tall.
- Text styles only: Body for row titles, Headline for card titles, Subheadline for the meta line, Footnote for ages.

## Agent logos

Every tile is `.fill.secondary` with a `.primary` template mark, as opencode, amp and pi already are (`Parts.swift:13,19-22`).

## Pull requests

`PullRequestPill` shows the tone word after the number ("#1083 Ready").

## Liquid Glass

Only on the functional layer: nav bar buttons, tab bar, offline and nearby banners (floating capsules over the list), composer. Rows, headers, answer cards and their buttons are opaque; with Reduce Transparency the chrome is solid.

## Motion

When an agent starts needing you, its row moves in once with a short spring (a cross-fade under Reduce Motion) and one notification haptic fires on that frame. Nothing animates or buzzes on refresh or an age tick; the waiting glyph does not pulse.
