---
name: Sheprd redesign (unofficial proposal)
description: One ticket system for every agent that needs you, across iOS, widgets, Watch, the web app and sheprd.org.
colors:
  rail: "#cdc8be"
  screen: "#e1ddd5"
  paper: "#f6f3ec"
  paper-2: "#ebe7de"
  ink: "#141210"
  ink-2: "#4f4a44"
  ink-3: "#6b665f"
  rule: "#d3ccbf"
  heat: "#e8742f"
  heat-ink: "#a54515"
  heat-wash: "#f6e2d2"
  failed: "#bf3326"
  failed-wash: "#f5dcd6"
  idle: "#8a8580"
  idle-ink: "#6b665f"
  rail-night: "#1f1d1b"
  screen-night: "#0f0e0d"
  steel: "#24221f"
  steel-2: "#2d2a27"
  ink-night: "#eee9df"
  ink-2-night: "#bbb5ab"
  ink-3-night: "#9a948a"
  rule-night: "#3d3934"
  heat-night: "#ef8443"
  heat-ink-night: "#f49a5e"
  heat-wash-night: "#3b271a"
  failed-night: "#ff7262"
  failed-wash-night: "#3d211c"
  idle-ink-night: "#a39d93"
typography:
  large-title:
    fontFamily: "-apple-system, BlinkMacSystemFont, 'SF Pro Display', 'Helvetica Neue', system-ui, sans-serif"
    fontSize: "34px"
    fontWeight: 700
    lineHeight: "41px"
    letterSpacing: "-0.02em"
  body:
    fontFamily: "-apple-system, BlinkMacSystemFont, 'SF Pro Text', 'Helvetica Neue', system-ui, sans-serif"
    fontSize: "16px"
    fontWeight: 500
    lineHeight: 1.35
  status-word:
    fontFamily: "-apple-system, BlinkMacSystemFont, 'SF Pro Text', 'Helvetica Neue', system-ui, sans-serif"
    fontSize: "13px"
    fontWeight: 600
    lineHeight: 1
  button:
    fontFamily: "-apple-system, BlinkMacSystemFont, 'SF Pro Text', 'Helvetica Neue', system-ui, sans-serif"
    fontSize: "15px"
    fontWeight: 600
    lineHeight: 1
  ticket-header:
    fontFamily: "ui-monospace, 'SF Mono', SFMono-Regular, Menlo, Consolas, monospace"
    fontSize: "11px"
    fontWeight: 600
    lineHeight: 1.1
    letterSpacing: "0.04em"
    fontFeature: "tnum"
  age:
    fontFamily: "ui-monospace, 'SF Mono', SFMono-Regular, Menlo, Consolas, monospace"
    fontSize: "12px"
    fontWeight: 600
    fontFeature: "tnum"
  workspace-mono:
    fontFamily: "ui-monospace, 'SF Mono', SFMono-Regular, Menlo, Consolas, monospace"
    fontSize: "12px"
    fontWeight: 500
  site-display:
    fontFamily: "'Big Shoulders Display', 'Arial Narrow', sans-serif-condensed, sans-serif"
    fontSize: "92px"
    fontWeight: 800
    lineHeight: 0.9
    letterSpacing: "0"
rounded:
  sm: "4px"
  md: "10px"
  lg: "16px"
spacing:
  xs: "4px"
  sm: "8px"
  md: "14px"
  lg: "16px"
  xl: "48px"
components:
  ticket:
    backgroundColor: "{colors.paper}"
    textColor: "{colors.ink}"
    rounded: "{rounded.sm}"
    padding: "14px 14px 0"
    width: "300px"
  ticket-night:
    backgroundColor: "{colors.steel}"
    textColor: "{colors.ink-night}"
    rounded: "{rounded.sm}"
  button-heat:
    backgroundColor: "{colors.heat}"
    textColor: "{colors.ink}"
    typography: "{typography.button}"
    rounded: "{rounded.md}"
    padding: "0 14px"
    height: "44px"
  button-line:
    backgroundColor: "{colors.paper}"
    textColor: "{colors.ink}"
    typography: "{typography.button}"
    rounded: "{rounded.md}"
    padding: "0 14px"
    height: "44px"
  stub:
    backgroundColor: "{colors.paper}"
    textColor: "{colors.ink}"
    rounded: "{rounded.sm}"
    padding: "7px 12px 7px 16px"
    height: "52px"
  stamp:
    backgroundColor: "{colors.screen}"
    textColor: "{colors.ink}"
    rounded: "{rounded.sm}"
    size: "34px"
  option-row:
    backgroundColor: "{colors.paper-2}"
    textColor: "{colors.ink}"
    rounded: "{rounded.md}"
    height: "44px"
  field:
    backgroundColor: "{colors.paper}"
    textColor: "{colors.ink-3}"
    rounded: "{rounded.md}"
    height: "44px"
---

# Design System: Sheprd redesign (unofficial proposal)

Not made by the Sheprd team. This records the proposal as built in `mock.html`; every value below is read from that file. Sample data and every age in it are illustrative.

## Overview

Each agent that is waiting on the developer is a ticket on a rail. The ticket carries the agent, the machine, the workspace and the wait time. The developer answers on the ticket and it leaves the rail. The metaphor lives in shape and grammar only: a perforated edge, a printed mono header, a rail band, an age bar, a Done stack. Copy stays plain Sheprd vocabulary.

The surface is warm thermal paper (`paper`) on a warm grey rail, in ink, with one accent. Brand orange, taken from the paw icon gradient, lights "Needs you" and nothing else. Dark mode is the night version: a near-black rail, tickets turn dark steel with light ink. The perforation and the mono header carry identity in both modes. The system is flat apart from a soft drop shadow under each ticket.

**Key Characteristics:**
- One accent, one meaning: orange is "Needs you".
- Every state has a word and a shape as well as a color.
- Paper tickets with a masked perforated edge; mono tabular header; monogram stamp for repositories.
- Radius is 4, 10 or 16 and nothing else (system shapes excepted).
- Liquid Glass appears only on system bars.
- One status palette for iOS, widgets, Watch, web app and site.

## Colors

A warm, desaturated paper-and-ink palette with a single orange. Neutrals are tinted toward brown, never cool grey.

### Primary
- **Brand Orange** (`heat`, #e8742f; night #ef8443): fill only. The "Needs you" top stripe, the primary button, the age bar once past five minutes, the focus ring. Text on it is ink (`ink`, #141210).
- **Orange Text** (`heat-ink`, #a54515; night `heat-ink-night`, #f49a5e): the text and icon variant of orange, for AA contrast on paper. The "Needs you" status word, an age timer past five minutes, the app tint (AccentColor), nav icon buttons, the current tab.
- **Heat Wash** (`heat-wash`, #f6e2d2; night #3b271a): defined for tints. The build does not paint it on any surface in the mock, so do not rely on it until a surface needs it.

### Neutral
- **Rail Grey** (`rail`, #cdc8be; night `rail-night`, #1f1d1b): the band behind the ticket rail. Flat, no gradient.
- **Screen** (`screen`, #e1ddd5; night `screen-night`, #0f0e0d): the app and page background.
- **Thermal Paper** (`paper`, #f6f3ec) and **Dark Steel** (`steel`, #24221f): ticket, stub, field and card surface. `paper-2` (#ebe7de) / `steel-2` (#2d2a27) is the inset layer for option rows, inline code and the user message.
- **Ink** (`ink`, #141210; night `ink-night`, #eee9df): primary text, logos, the working dot, the age bar, 1 px outline buttons.
- **Ink 2 / Ink 3** (#4f4a44 / #6b665f; night #bbb5ab / #9a948a): secondary text and muted text (Done, timestamps, placeholders).
- **Rule** (`rule`, #d3ccbf; night #3d3934): hairlines, the dashed line under the ticket header, field outlines.

### Status
- **Needs you:** triangle with !, `heat-ink` text, `heat` stripe and button.
- **Working:** ink dot with a pulse ring, ink text.
- **Done:** check, `ink-3` text, struck through, stacked.
- **Failed:** filled square with x, `failed` (#bf3326; night #ff7262).
- **Idle:** open circle, `idle-ink` (#6b665f; night #a39d93).
- **PR state** (Workspaces): the six existing tones (needs attention, blocked or behind, merged, ready to merge, checks or review pending, draft) keep their SF Symbol and show their label as a visible word. Needs attention uses `failed`, draft uses an `idle` dashed outline, the rest are ink outlines. No purple, no orange.

### Named Rules
**The One Meaning Rule.** Orange means "Needs you" and nothing else. CPU or memory at 80% or more is shown in bold with the word "High", not in orange.

**The Five Minute Rule.** Age is a mono `m:ss` timer and a 3 px ink bar that fills linearly over 300 seconds. At 300 seconds both switch to orange (`heat-ink` text, `heat` bar). There is no green, amber or red ramp, and no other color change by age.

**The Word And Shape Rule.** Color is never the only signal. A new status needs a word and a glyph shape before it gets a color.

**The Two Greys Rule.** Failed is red, its own color. Idle is grey, with an open circle. Do not blend them into orange.

## Typography

**UI Font:** SF Pro on iOS, through Dynamic Type styles (`-apple-system, "SF Pro Text"/"SF Pro Display", system-ui` in the web stacks)
**Mono Font:** SF Mono on iOS (`.monospaced()`), `ui-monospace` stack on the web app and site
**Site Display Font:** Big Shoulders Display 800, uppercase (SIL Open Font License 1.1; self-host the woff2 for release; fallback `"Arial Narrow", sans-serif-condensed`)

**Character:** plain system sans for reading, tabular mono for anything printed on a ticket, and a condensed poster face only where the brand speaks on the site.

### Hierarchy
- **Large title** (700, 34/41 px, -0.02em): iOS Home and Workspaces. PWA title is 700, 30/1.1.
- **Body / question** (500, 16/1.35): the ticket question, option rows.
- **Status word** (600, 13/1): "Needs you", "Working", "Failed". Sits under the header with its glyph.
- **Button** (600, 15): Allow once, Deny, Answer.
- **Ticket header** (mono 600, 11/1.1, +0.04em, uppercase, tabular): `AGENT · MACHINE · WORKSPACE`, with the age at the trailing edge.
- **Age** (mono 600, 12, tabular, no tracking).
- **Workspace and branch** (mono 500, 12): branch names, commands, inline code (13.5 on a `paper-2` chip).
- **Site display** (Big Shoulders Display 800, 92/0.9, uppercase, max 11ch; 56 px under 600 px container width; wordmark 24 px): hero headline and wordmark only.

### Named Rules
**The Printed Mono Rule.** Mono is for what a ticket prints: header, age, workspace, branch, command, machine group header. Never for button labels or body prose on iOS and web app.

**The Site Display Rule.** Big Shoulders Display is a site-only display face. iOS and the web app never use it.

## Layout

A phone column of 16 px side margins. Home reads top to bottom as large title, "Needs you" rail, "Working" stubs grouped by machine, then the Done stack. The rail is a horizontal scroll with mandatory snap, 10 px between tickets, 300 px tickets (296 in the PWA, 320 on the 1440 site) so the next ticket peeks. Oldest wait is first. Stubs sit 4 px apart; rows 6 px apart. The site uses 48 px gutters at 1440 and 16 px at 390; the hero is 7fr/5fr at wide width and one column under 600 px, where nav keeps only the logo and "Get Sheprd". Every action target is at least 44 by 44, with 8 px between Allow once and Deny. At accessibility Dynamic Type sizes the rail becomes a vertical stack (spec; not shown in the mock).

## Elevation & Depth

Flat tonal layering with one soft shadow. Depth comes from paper over rail, not from outlines. Each ticket on the rail carries a two-layer drop shadow (`drop-shadow(0 1px 1px) drop-shadow(0 6px 10px)` of the shadow color, light `rgb(40 30 20 / .16)`, dark `rgb(0 0 0 / .5)`); a filter, so the perforation is shadowed too. The Done stack uses three offset paper edges with `0 1px 0 rule, 0 2px 4px shadow`. Tab bar and nav bar are the only glass: system blur 20 px, saturate 1.6, `rgb(250 248 244 / .72)` light or `rgb(28 27 25 / .72)` dark.

### Named Rules
**The Glass Is For Bars Rule.** Liquid Glass and backdrop blur live on system bars only. Answers, toasts, composers, capsules and the PWA toolbar use the paper surface.

**The Soft Shadow Rule.** Shadows are soft and blurred, and only under tickets. No hard offset shadows.

## Shapes

Radius is 4, 10 or 16. 4 for tickets (bottom corners), stubs, stamps, tags, PR state, code chips and the stack. 10 for buttons, fields, option rows, tool rows and the user message. 16 for sheets. System shapes (tab bar capsule, widget corners, device, 50% circles for round buttons and dots) stay as the system draws them. The ticket's signature is the top edge: a mask of 3 pt half circles every 10 px (`radial-gradient(circle at 5px 0, transparent 2.75px, #000 3.25px)` repeated on x), so the edge is cut, not drawn. A stub is perforated on its leading edge instead (same mask rotated, repeated on y). Header and ticket body divide with a 1 px dashed rule.

## Components

### Ticket
The one unit of attention. Paper (or steel) surface, perforated top edge, 14 px padding, 4 px bottom corners. Rows: (1) header, agent logo 16 px in ink, mono `AGENT · MACHINE`, age trailing, dashed rule under; (2) status word with glyph; (3) workspace in mono; (4) question; (5) actions; (6) age bar full-bleed, 3 px. "Needs you" adds a 3 px `heat` stripe above the header. Tear-off is a state of the ticket (see below). The same ticket appears on Home, Chat (docked above the composer, with option rows), lock screen and Live Activity, widgets and Watch, the web app and the site hero.

### Buttons
- **Heat (primary):** `heat` fill, ink text, 10 px radius, 44 px minimum height, 0 14 px padding, flexes to fill the action row. Hover darkens 12%. Active scales 0.97 in 120 ms.
- **Line:** transparent, ink text, inset 1 px ink at 32% opacity, hover fills `paper-2`. Deny and secondary actions.
- Answer on a question ticket is heat fill.

### Stub
Working, failed and idle rows. 52 px minimum height, `paper`, perforated leading edge, 22 px logo column, title and mono subline, status word with glyph and last activity at the trailing edge. Grouped under a mono machine header with a dashed rule and a muted kind (`This Mac`, `SSH`, `Container`). Idle title is `ink-2`.

### Monogram Stamp
34 px square, 4 px radius, a double ink rule (1.5 px, gap, 1 px), two mono capitals (700, 12 px). Ink on screen only: no hashed hue, no gradient.

### Done Stack
One collapsed row of `paper`, 56 px, a pin on its leading edge, check, "Done" and a count, the last title struck through in mono; three paper edges behind it, offset and rotated (7/11 px at 1.6deg, -5/7 at -1.2deg, 3/3 at 0.6deg). Tap expands to the normal list.

### Inputs
Composer field: paper, 10 px radius, 1 px rule inset, 44 px, no glass. Round attach and dictate buttons are 44 px. Search: 10 px radius, 8% ink tint. Option rows: `paper-2`, 10 px radius, 44 px, a radio ring that fills with ink when selected and an inset ink outline.

### Navigation
System tab bar and large title nav; the tint is `heat-ink`. Current tab is `heat-ink` with a 7% ink tint pill. Web app: header row carries the actions; no floating toolbar.

### Load Gauge, PR State, Tag
Gauge: 54 by 4 px ink bar, tabular percent, bold with "High" at 80% or more. PR state and machine tag are 4 px radius, 1 px outlined mono or sans labels with a word and a glyph.

### Motion
Tear-off: on an answer, the ticket lifts 10 and tilts -2deg with opacity .9, 90 ms later slides to `translate(120%, -18px) rotate(4deg)` and fades, 300 ms on `cubic-bezier(.16, 1, .3, 1)`; after 330 ms the slot collapses over 250 ms and the next ticket slides left. On iOS: `.spring(duration: 0.3, bounce: 0)` with `.sensoryFeedback(.success)` fired on tap. **Reduce Motion:** no lift, tilt or slide; the ticket fades out in place (200 ms crossfade in the spec), and the working-dot pulse (1.6 s ping) stops. The age bar fills linearly at 1 s steps.

### Copy
Plain labels only: Home, Needs you, Working, Done, Failed, Idle, High, and the existing PR tone labels. Answer buttons keep the agent's own option names (Allow once, Deny, Answer). No metaphor puns and no food icons. "Rail" and "Ticket" are allowed only as engineering names. Illustrative data is always labelled "Illustrative" or "Ages illustrative". The Live Activity never shows "Always" options.

## Surfaces

- **iOS:** HIG first. Tab bar, nav stack and sheets stay system with Liquid Glass. SF Pro through Dynamic Type; SF Mono only per the Printed Mono Rule; SF Symbols for chrome. Brand tint is `heat-ink` (AccentColor). The world lives in the ticket, rail, stubs, stack, stamp, timer, tint and haptic. Agent logos stay as ink templates, never brand-color tiles.
- **Widgets, lock screen, Watch:** the same ticket, narrower: header, question, Allow once and Deny, age bar. Small widget shows the big count in `heat-ink`.
- **PWA:** the same tokens as CSS variables, the same tickets, system font plus `ui-monospace`, no floating glass toolbar, no periwinkle accent, no iOS-grey copy palette.
- **Site:** the same palette; Big Shoulders Display for headline and wordmark; system sans for body; a Get Sheprd button and a link to the other ways to install; a rail of tickets from several machines labelled Illustrative; no counts or invented claims.

## Dependency

**Age is illustrative.** The remote control server today sends last activity only (`LastAnswer`: now, 5m, 3h). The timer, the age bar and the orange switch need a `waiting_since` timestamp per agent in the "needs you" state. Until it is sent, a ticket shows last activity and no bar.

## Do's and Don'ts

### Do:
- **Do** use orange fill only for "Needs you" and its primary action; use `heat-ink` for orange text.
- **Do** pair every status with a word and a glyph shape.
- **Do** keep radius to 4, 10 or 16.
- **Do** keep ticket header content in the order agent, machine, workspace, age.
- **Do** switch age to orange at 300 seconds exactly, and nowhere earlier.
- **Do** label any sample number or age "Illustrative".

### Don't:
- **Don't** use orange for load, warnings, links or decoration.
- **Don't** add a green, amber or red age ramp.
- **Don't** put Liquid Glass or blur on answers, toasts, composers or the PWA toolbar.
- **Don't** use a hashed or rainbow color for repositories.
- **Don't** write metaphor puns in UI copy.
- **Don't** use Big Shoulders Display outside the site.

### Not canonized
Proposal-page chrome (the sticky banner, `.cap` mono uppercase captions, the device frames, the theme toggle) is the document around the proposal, not the system. Small mono uppercase labels such as the `Ages illustrative` and `Illustrative` tags and section counts exist in the build but are not recorded as a reusable pattern. The lock screen's radial brown gradient is a one-off. The `Heat Wash` and `Failed Wash` tokens are defined but unused by any surface, so treat them as unproven.
