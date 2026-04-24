---
version: alpha
name: [App Name]
description: [One-line description of the design system]
colors:
  primary: "#______"
  on-primary: "#______"
  primary-container: "#______"
  secondary: "#______"
  on-secondary: "#______"
  tertiary: "#______"
  background: "#______"
  surface: "#______"
  on-surface: "#______"
  outline: "#______"
  surface-variant: "#______"
  error: "#______"
  warning: "#______"
  success: "#______"
  info: "#______"
typography:
  display-lg:
    fontFamily: [Font name]
    fontSize: 48px
    fontWeight: 700
    lineHeight: 1.1
    letterSpacing: -0.02em
  headline-lg:
    fontFamily: [Font name]
    fontSize: 32px
    fontWeight: 600
    lineHeight: 1.2
  headline-sm:
    fontFamily: [Font name]
    fontSize: 24px
    fontWeight: 600
    lineHeight: 1.3
  title-md:
    fontFamily: [Font name]
    fontSize: 18px
    fontWeight: 500
    lineHeight: 1.4
  body-lg:
    fontFamily: [Font name]
    fontSize: 18px
    fontWeight: 400
    lineHeight: 1.6
  body-md:
    fontFamily: [Font name]
    fontSize: 16px
    fontWeight: 400
    lineHeight: 1.5
  label-lg:
    fontFamily: [Font name]
    fontSize: 14px
    fontWeight: 500
    lineHeight: 1.4
  label-sm:
    fontFamily: [Font name]
    fontSize: 12px
    fontWeight: 500
    lineHeight: 1.4
spacing:
  xs: 4px
  sm: 8px
  md: 16px
  lg: 24px
  xl: 32px
  2xl: 48px
  gutter: 24px
  margin: 32px
rounded:
  sm: 4px
  md: 8px
  lg: 12px
  xl: 16px
  full: 9999px
components:
  button-primary:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.on-primary}"
    rounded: "{rounded.md}"
    padding: 12px 24px
    typography: "{typography.label-lg}"
  button-primary-hover:
    backgroundColor: "{colors.primary}"
  button-secondary:
    backgroundColor: transparent
    textColor: "{colors.primary}"
    rounded: "{rounded.md}"
    padding: 12px 24px
  button-destructive:
    backgroundColor: "{colors.error}"
    textColor: "#FFFFFF"
    rounded: "{rounded.md}"
    padding: 12px 24px
  input:
    backgroundColor: "{colors.surface}"
    textColor: "{colors.on-surface}"
    rounded: "{rounded.md}"
    padding: 12px 16px
  card:
    backgroundColor: "{colors.surface}"
    rounded: "{rounded.lg}"
    padding: "{spacing.lg}"
---

# Visual Design System: [App Name]

## Overview

[A holistic description of the design's look and feel. Describe the personality of the application:
is it playful or professional? Dense or spacious? Minimalist or richly decorated? Dark or light?
This section guides high-level decisions when no specific token applies.

Include: target audience, emotional tone, key adjectives (e.g., "clean, modern, trustworthy"),
and any reference points or inspirations (e.g., "inspired by Material Design 3 with a warmer palette").]

## Colors

[Describe the color strategy and the role of each palette. Prose may use descriptive names
(e.g., "Midnight Forest Green") that correspond to the systematic token names in the frontmatter.]

- **Primary ([token: primary]):** [Main brand color — describe its role and emotional quality]
- **Secondary ([token: secondary]):** [Supporting color — where and why it's used]
- **Tertiary ([token: tertiary]):** [Third accent — used sparingly for highlights or differentiation]
- **Neutral/Surface:** [Background and surface strategy — warm vs. cool, light vs. dark]

### Semantic colors

- **Error ([token: error]):** [Destructive actions, validation failures]
- **Warning ([token: warning]):** [Caution states, approaching limits]
- **Success ([token: success]):** [Confirmations, positive feedback]
- **Info ([token: info]):** [Informational banners, tips]

### Dark mode (if applicable)

[If the app supports a dark theme, specify how the palette maps. At minimum, list the tokens
that change and their dark-mode hex values. If the app is light-only or dark-only, state that
explicitly and omit this subsection.]

## Typography

[Describe the typography strategy: the font families chosen, why they work together, and the
overall type personality (institutional, playful, technical, editorial).]

- **Headlines:** [Font and weight used for headlines — describe the voice it creates]
- **Body:** [Font and weight for body text — readability considerations]
- **Labels:** [Font used for UI elements, buttons, captions — any special treatments]
- **Code (if applicable):** [Monospace font for technical content]

[Note: The relationship between headline and body fonts matters. Using the same family
(e.g., Inter for both) conveys uniformity and cleanliness. Mixing families (e.g., a serif
headline with a sans-serif body) creates visual contrast and editorial feel. State the
intended effect.]

## Layout

[Describe the layout and spacing strategy.]

### Layout principles

[Describe the layout approach: max content width, grid system (if any), responsive breakpoints,
and how content areas relate to each other. For example: "Content is constrained to 1200px max
width, centered. Sidebar is 280px fixed. Main content uses a 12-column grid with 24px gutters."]

### Spacing scale

[The spacing tokens are defined in the frontmatter. Here, explain the philosophy: Is it an 8px
base scale? 4px for micro-adjustments? How should implementers choose between spacing levels?]

- Use `xs` (4px) for tight inner padding, icon gaps
- Use `sm` (8px) for compact element spacing, chip padding
- Use `md` (16px) for default content padding, card padding
- Use `lg` (24px) for section spacing, major content gaps
- Use `xl` (32px) for page margins, large section separators

## Elevation & Depth

[How the design conveys depth and hierarchy. Some designs use shadows (layered depth);
others stay flat (using borders or color to separate layers). State which approach this
design uses.]

### Shadow scale (if applicable)

| Level         | Properties                           | Usage                             |
| ------------- | ------------------------------------ | --------------------------------- |
| `elevation-0` | none                                 | Flat elements, inline content     |
| `elevation-1` | [e.g., 0 1px 3px rgba(0,0,0,0.12)]   | Cards, raised surfaces            |
| `elevation-2` | [e.g., 0 4px 8px rgba(0,0,0,0.16)]   | Dropdowns, popovers, menus        |
| `elevation-3` | [e.g., 0 8px 24px rgba(0,0,0,0.20)]  | Dialogs, modals, floating panels  |

[If the design is flat: "This design does not use shadows. Hierarchy is conveyed through
background color changes and border usage."]

## Shapes

[Describe the shape language: sharp and architectural, soft and rounded, or mixed with intent.]

The `rounded` tokens in the frontmatter define corner radii:
- `sm` (4px): Chips, small badges, tags
- `md` (8px): Buttons, inputs, cards
- `lg` (12px): Dialogs, modals, large containers
- `full` (9999px): Avatars, circular elements

## Components

[Style guidance for the UI components most relevant to this application. The component tokens
in the frontmatter define the precise values using `{path.to.token}` references. Here, provide
context on variants, states, and usage patterns.]

### Buttons

The button component tokens are defined in the frontmatter. Key variants:

- **Primary:** For the single most important action per screen
- **Secondary:** For secondary actions that don't compete with primary
- **Tertiary:** For low-emphasis actions, often in toolbars or inline
- **Destructive:** For delete, remove, or other destructive actions

**States:** hover (darken 8%), active (darken 12%), disabled (40% opacity), focus (2px ring using `primary`).

### Inputs / Form fields

[Text field styling: describe border treatment, focus ring, label position (floating vs. above),
helper text placement, error state styling, disabled state, sizing variants.]

### Cards

[Background, border, elevation level, padding (reference `{spacing.lg}`), corner radius
(reference `{rounded.lg}`), hover behavior if interactive.]

### Navigation

[Nav bar / sidebar / tabs / breadcrumbs — whichever is relevant. Active state, hover state,
icon + label alignment, selected indicator style.]

### Modals / Dialogs

[Overlay color, dialog surface, corner radius, elevation, max width, padding, close behavior.]

### Tables / Data display (if applicable)

[Header styling, row striping, hover, sorting indicators, pagination, cell padding.]

### Toasts / Notifications (if applicable)

[Position, duration, color mapping to severity (use semantic color tokens), dismiss behavior, stacking.]

## Do's and Don'ts

### Do

- Use the primary color only for the single most important action per screen
- Maintain WCAG AA contrast ratios (4.5:1 for normal text, 3:1 for large text)
- Use consistent spacing tokens — never use arbitrary pixel values
- Keep interactive touch targets at least 44x44px on mobile
- Reference design tokens using `{path.to.token}` syntax in component definitions

### Don't

- Don't mix rounded and sharp corners in the same view
- Don't use more than two font weights on a single screen
- Don't use color as the only way to convey meaning — always pair with text or icons
- Don't put elevated elements on top of other elevated elements
- Don't use raw hex values in components — always reference color tokens

## Accessibility

[Key accessibility requirements for this design system. These are non-negotiable constraints
that the implementing agent must follow.]

- **Minimum contrast ratio:** WCAG AA — 4.5:1 for normal text, 3:1 for large text
- **Focus indicators:** 2px solid ring using `{colors.primary}`, visible on all interactive elements
- **Touch targets:** Minimum 44x44px on mobile, 32x32px on desktop
- **Reduced motion:** Respect `prefers-reduced-motion` — disable animations, use instant transitions
- **Screen reader:** All interactive elements must have accessible names; decorative images use `aria-hidden`
