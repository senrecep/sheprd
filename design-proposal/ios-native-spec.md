# iPhone app: native styling with small status changes

Keeps the current native iOS 26 look. Line references are from 3005f801.

## Status palette (shared `Status` enum in `ios/Shared/`)

| State | Color | SF Symbol | Word |
|---|---|---|---|
| Needs you | `.orange` | `exclamationmark.circle.fill` | Needs you |
| Working | `.yellow` | `ellipsis.circle.fill` | Working |
| Done | `.teal` | `checkmark.circle.fill` | Done |
| Failed | `.red` | `xmark.circle.fill` | Failed |
| Idle | none | none | Idle |

Orange is used only for "needs you". Used by the app (`Views/Parts.swift`), widgets (`SheprdRemoteWidgets/Parts.swift`) and the Watch.

## Home

- Priority sort: Needs you, Working, Idle, Done (`AgentSort.swift:50`).
- Needs you on top in every sort; hidden when empty.
- Headers show counts ("Working 3"); VoiceOver reads "Working, 3 agents".
- Needs-you cards and rows show the wait time; after 5 minutes it uses `.orange` and semibold. Needs `waiting_since` from the daemon.

## Agent logos

Every tile is `.fill.secondary` with a `.primary` template mark, as opencode, amp and pi already are (`Parts.swift:13,19-22`).

## Pull requests

`PullRequestPill` shows the tone word after the number ("#1083 Ready").

## Liquid Glass

Only where the app uses it now: tab bar, offline and nearby banners, composer. Rows and answer cards stay solid.
