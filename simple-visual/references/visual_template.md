# Visual Design System: [App Name]

## Overview

[A holistic description of the design's look and feel. Describe the personality of the application:
is it playful or professional? Dense or spacious? Minimalist or richly decorated? Dark or light?
This section guides high-level decisions when no specific token applies.

Include: target audience, emotional tone, key adjectives (e.g., "clean, modern, trustworthy"),
and any reference points or inspirations (e.g., "inspired by Material Design 3 with a warmer palette").]

## Colors

### Primary palette

| Token          | Hex       | Role                                                    |
| -------------- | --------- | ------------------------------------------------------- |
| `primary`      | `#______` | [Main brand color — used for primary actions and focus states] |
| `on-primary`   | `#______` | [Text/icon color on primary surfaces]                   |
| `primary-container` | `#______` | [Lighter variant for backgrounds, cards, or chips that carry primary meaning] |

### Secondary palette

| Token            | Hex       | Role                                                  |
| ---------------- | --------- | ----------------------------------------------------- |
| `secondary`      | `#______` | [Supporting color — used for secondary actions, accents, or categories] |
| `on-secondary`   | `#______` | [Text/icon color on secondary surfaces]               |

### Tertiary palette (optional)

| Token          | Hex       | Role                                                    |
| -------------- | --------- | ------------------------------------------------------- |
| `tertiary`     | `#______` | [Third accent color — used sparingly for highlights, tags, or differentiation] |

### Neutral / surface palette

| Token          | Hex       | Role                                                    |
| -------------- | --------- | ------------------------------------------------------- |
| `background`   | `#______` | [App background]                                        |
| `surface`      | `#______` | [Card, sheet, dialog backgrounds]                       |
| `on-surface`   | `#______` | [Primary text color on surface/background]              |
| `outline`      | `#______` | [Borders, dividers, subtle boundaries]                  |
| `surface-variant` | `#______` | [Differentiated surface — e.g., sidebar, secondary panels] |

### Semantic / feedback colors

| Token     | Hex       | Role                                                       |
| --------- | --------- | ---------------------------------------------------------- |
| `error`   | `#______` | [Destructive actions, error states, validation failures]   |
| `warning` | `#______` | [Caution states, approaching limits, non-blocking alerts]  |
| `success` | `#______` | [Confirmations, completed actions, positive feedback]      |
| `info`    | `#______` | [Informational banners, tips, neutral notifications]       |

### Dark mode (if applicable)

[If the app supports a dark theme, specify how the palette maps. At minimum, list the tokens
that change and their dark-mode hex values. If the app is light-only or dark-only, state that
explicitly and omit this subsection.]

## Typography

### Font families

| Role      | Family          | Weight(s)       | Notes                                   |
| --------- | --------------- | --------------- | --------------------------------------- |
| Display   | [Font name]     | [e.g., 700]     | [Used for hero text, marketing, splash] |
| Headline  | [Font name]     | [e.g., 600]     | [Section headers, page titles]          |
| Title     | [Font name]     | [e.g., 500]     | [Card titles, dialog titles, sub-headers] |
| Body      | [Font name]     | [e.g., 400]     | [Paragraph text, descriptions, content] |
| Label     | [Font name]     | [e.g., 500]     | [Buttons, chips, tabs, captions]        |
| Code      | [Monospace font] | [e.g., 400]    | [Code blocks, technical content — omit if not applicable] |

### Type scale

| Level          | Size   | Line height | Letter spacing | Usage                          |
| -------------- | ------ | ----------- | -------------- | ------------------------------ |
| Display Large  | [px]   | [px or %]   | [em or px]     | [When to use]                  |
| Headline Large | [px]   | [px or %]   | [em or px]     | [When to use]                  |
| Headline Small | [px]   | [px or %]   | [em or px]     | [When to use]                  |
| Title Medium   | [px]   | [px or %]   | [em or px]     | [When to use]                  |
| Body Large     | [px]   | [px or %]   | [em or px]     | [When to use]                  |
| Body Medium    | [px]   | [px or %]   | [em or px]     | [When to use]                  |
| Label Large    | [px]   | [px or %]   | [em or px]     | [When to use]                  |
| Label Small    | [px]   | [px or %]   | [em or px]     | [When to use]                  |

[Note: The relationship between headline and body fonts matters. Using the same family
(e.g., Inter for both) conveys uniformity and cleanliness. Mixing families (e.g., a serif
headline with a sans-serif body) creates visual contrast and editorial feel. State the
intended effect.]

## Spacing & Layout

### Spacing scale

[Define the spacing scale used for padding, margins, and gaps. A consistent scale prevents
ad-hoc values from creeping in.]

| Token  | Value  | Usage                                              |
| ------ | ------ | -------------------------------------------------- |
| `xs`   | [px]   | [Tight inner padding, icon gaps]                   |
| `sm`   | [px]   | [Compact element spacing, chip padding]            |
| `md`   | [px]   | [Default content padding, card padding]            |
| `lg`   | [px]   | [Section spacing, major content gaps]              |
| `xl`   | [px]   | [Page margins, large section separators]           |

### Layout principles

[Describe the layout approach: max content width, grid system (if any), responsive breakpoints,
and how content areas relate to each other. For example: "Content is constrained to 1200px max
width, centered. Sidebar is 280px fixed. Main content uses a 12-column grid with 24px gutters."]

### Corner radius

| Token           | Value  | Usage                                           |
| --------------- | ------ | ----------------------------------------------- |
| `radius-sm`     | [px]   | [Chips, small badges, tags]                     |
| `radius-md`     | [px]   | [Buttons, inputs, cards]                        |
| `radius-lg`     | [px]   | [Dialogs, modals, large containers]             |
| `radius-full`   | [px or 9999px] | [Avatars, circular elements]            |

## Elevation

[How the design conveys depth and hierarchy. Some designs use shadows (layered depth);
others stay flat (using borders or color to separate layers). State which approach this
design uses.]

### Shadow scale (if applicable)

| Level        | Properties                                      | Usage                            |
| ------------ | ----------------------------------------------- | -------------------------------- |
| `elevation-0` | none                                           | [Flat elements, inline content]  |
| `elevation-1` | [e.g., 0 1px 3px rgba(0,0,0,0.12)]            | [Cards, raised surfaces]         |
| `elevation-2` | [e.g., 0 4px 8px rgba(0,0,0,0.16)]            | [Dropdowns, popovers, menus]     |
| `elevation-3` | [e.g., 0 8px 24px rgba(0,0,0,0.20)]           | [Dialogs, modals, floating panels] |

[If the design is flat: "This design does not use shadows. Hierarchy is conveyed through
background color changes and border usage."]

## Iconography (optional)

[Specify the icon set (e.g., Lucide, Material Symbols, Heroicons, custom), preferred style
(outlined, filled, rounded), default size, and stroke width. If icons are not a significant
part of the app, omit this section.]

## Motion & Animation (optional)

[If the design uses transitions or animations, specify the timing functions, durations, and
which interactions are animated. If the app is static or animation is not a priority, omit
this section.]

| Interaction        | Duration | Easing                    | Notes                       |
| ------------------ | -------- | ------------------------- | --------------------------- |
| Button press       | [ms]     | [e.g., ease-out]          | [Scale, color, or ripple]   |
| Page transition    | [ms]     | [e.g., ease-in-out]       | [Fade, slide, or none]      |
| Modal open/close   | [ms]     | [e.g., ease-out]          | [Fade + scale]              |
| Hover state        | [ms]     | [e.g., ease]              | [Color shift, underline]    |

## Components

[Style guidance for the UI components most relevant to this application. Focus on what the
implementing agent needs to know to build consistent components. You don't need to cover
every component — cover the ones that matter for this project.]

### Buttons

| Variant     | Background       | Text color       | Border           | Corner radius   | Padding         |
| ----------- | ---------------- | ---------------- | ---------------- | --------------- | --------------- |
| Primary     | `primary`        | `on-primary`     | none             | `radius-md`     | [px]            |
| Secondary   | transparent      | `primary`        | 1px `primary`    | `radius-md`     | [px]            |
| Tertiary    | transparent      | `primary`        | none             | `radius-md`     | [px]            |
| Destructive | `error`          | `#FFFFFF`        | none             | `radius-md`     | [px]            |

States: hover (darken 8%), active (darken 12%), disabled (40% opacity), focus (2px ring `primary`).

[Add or remove component subsections based on the project's needs. Common components to specify:]

### Inputs / Form fields

[Text field styling: border, focus ring, label position (floating vs. above), helper text,
error state, disabled state, sizing.]

### Cards

[Background, border, elevation level, padding, corner radius, hover behavior if interactive.]

### Navigation

[Nav bar / sidebar / tabs / breadcrumbs — whichever is relevant. Active state, hover state,
icon + label alignment, selected indicator style.]

### Modals / Dialogs

[Overlay color, dialog surface, corner radius, elevation, max width, padding, close behavior.]

### Tables / Data display (if applicable)

[Header styling, row striping, hover, sorting indicators, pagination, cell padding.]

### Chips / Tags (if applicable)

[Variants (filter, selection, action), sizing, selected state, removable indicator.]

### Toasts / Notifications (if applicable)

[Position, duration, color mapping to severity, dismiss behavior, stacking.]

## Do's and Don'ts

### Do

- [Actionable positive guideline — e.g., "Use the primary color only for the single most important action per screen"]
- [e.g., "Maintain WCAG AA contrast ratios (4.5:1 for normal text, 3:1 for large text)"]
- [e.g., "Use consistent spacing tokens — never use arbitrary pixel values"]
- [e.g., "Keep interactive touch targets at least 44x44px on mobile"]

### Don't

- [Actionable negative guideline — e.g., "Don't mix rounded and sharp corners in the same view"]
- [e.g., "Don't use more than two font weights on a single screen"]
- [e.g., "Don't use color as the only way to convey meaning — always pair with text or icons"]
- [e.g., "Don't put elevated elements on top of other elevated elements"]

## Accessibility

[Key accessibility requirements for this design system. These are non-negotiable constraints
that the implementing agent must follow.]

- Minimum contrast ratio: [e.g., WCAG AA — 4.5:1 for normal text, 3:1 for large text]
- Focus indicators: [e.g., 2px solid ring using `primary`, visible on all interactive elements]
- Touch targets: [e.g., minimum 44x44px on mobile, 32x32px on desktop]
- Reduced motion: [e.g., respect `prefers-reduced-motion` — disable animations, use instant transitions]
- Screen reader: [e.g., all interactive elements must have accessible names; decorative images use `aria-hidden`]
