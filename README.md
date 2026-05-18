DESIGN.md

A format specification for describing a visual identity to coding agents. DESIGN.md gives agents a persistent, structured understanding of a design system.

The Format

A DESIGN.md file combines machine-readable design tokens (YAML front matter) with human-readable design rationale (markdown prose). Tokens give agents exact values. Prose tells them why those values exist and how to apply them.

--- name: Heritage colors: primary: "#1A1C1E" secondary: "#6C7278" tertiary: "#B8422E" neutral: "#F7F5F2" typography: h1: fontFamily: Public Sans fontSize: 3rem body-md: fontFamily: Public Sans fontSize: 1rem label-caps: fontFamily: Space Grotesk fontSize: 0.75rem rounded: sm: 4px md: 8px spacing: sm: 8px md: 16px --- ## Overview Architectural Minimalism meets Journalistic Gravitas. The UI evokes a premium matte finish — a high-end broadsheet or contemporary gallery. ## Colors The palette is rooted in high-contrast neutrals and a single accent color. - **Primary (#1A1C1E):** Deep ink for headlines and core text. - **Secondary (#6C7278):** Sophisticated slate for borders, captions, metadata. - **Tertiary (#B8422E):** "Boston Clay" — the sole driver for interaction. - **Neutral (#F7F5F2):** Warm limestone foundation, softer than pure white.

An agent that reads this file will produce a UI with deep ink headlines in Public Sans, a warm limestone background, and Boston Clay call-to-action buttons.

Getting Started

Validate a DESIGN.md against the spec, catch broken token references, check WCAG contrast ratios, and surface structural findings — all as structured JSON that agents can act on.

npx @google/design.md lint DESIGN.md

{ "findings": [ { "severity": "warning", "path": "components.button-primary", "message": "textColor (#ffffff) on backgroundColor (#1A1C1E) has contrast ratio 15.42:1 — passes WCAG AA." } ], "summary": { "errors": 0, "warnings": 1, "info": 1 } }

Compare two versions of a design system to detect token-level and prose regressions:

npx @google/design.md diff DESIGN.md DESIGN-v2.md

{ "tokens": { "colors": { "added": ["accent"], "removed": [], "modified": ["tertiary"] }, "typography": { "added": [], "removed": [], "modified": [] } }, "regression": false }

The Specification

The full DESIGN.md spec lives at docs/spec.md. What follows is a condensed reference.

File Structure

A DESIGN.md file has two layers:

YAML front matter — Machine-readable design tokens, delimited by --- fences at the top of the file.Markdown body — Human-readable design rationale organized into ## sections.

The tokens are the normative values. The prose provides context for how to apply them.

Token Schema

version: <string> # optional, current: "alpha" name: <string> description: <string> # optional colors: <token-name>: <Color> typography: <token-name>: <Typography> rounded: <scale-level>: <Dimension> spacing: <scale-level>: <Dimension | number> components: <component-name>: <token-name>: <string | token reference>

# optional, current: "alpha" name: description: # optional colors: : typography: : rounded: : spacing: : components: : : " tabindex="0" role="button" style="box-sizing: border-box; margin: 8px !important; padding: 0px !important; font-size: 14px; font-weight: 500; white-space: nowrap; vertical-align: middle; cursor: pointer; user-select: none; appearance: none; border: 0px; border-radius: 6px; line-height: 20px; display: flex !important; position: relative; color: rgb(9, 105, 218); background-color: rgba(0, 0, 0, 0); box-shadow: none; transition: color 80ms cubic-bezier(0.33, 1, 0.68, 1), background-color 80ms cubic-bezier(0.33, 1, 0.68, 1), box-shadow 80ms cubic-bezier(0.33, 1, 0.68, 1), border-color 80ms cubic-bezier(0.33, 1, 0.68, 1); justify-content: center !important; align-items: center !important; width: 28px; height: 28px;">

Token Types

TypeFormatExampleColor# + hex (sRGB)"#1A1C1E"Dimensionnumber + unit (px, em, rem)48px, -0.02emToken Reference{path.to.token}{colors.primary}Typographyobject with fontFamily, fontSize, fontWeight, lineHeight, letterSpacing, fontFeature, fontVariationSee example above

Section Order

Sections use ## headings. They can be omitted, but those present must appear in this order:

#SectionAliases1OverviewBrand & Style2Colors3Typography4LayoutLayout & Spacing5Elevation & DepthElevation6Shapes7Components8Do's and Don'ts

Component Tokens

Components map a name to a group of sub-token properties:

components: button-primary: backgroundColor: "{colors.tertiary}" textColor: "{colors.on-tertiary}" rounded: "{rounded.sm}" padding: 12px button-primary-hover: backgroundColor: "{colors.tertiary-container}"

Valid component properties: backgroundColor, textColor, typography, rounded, padding, size, height, width.

Variants (hover, active, pressed) are expressed as separate component entries with a related key name.

Consumer Behavior for Unknown Content

ScenarioBehaviorUnknown section headingPreserve; do not errorUnknown color token nameAccept if value is validUnknown typography token nameAccept as valid typographyUnknown component propertyAccept with warningDuplicate section headingError; reject the file

CLI Reference

Installation

npm install @google/design.md

On Windows, quote the package name if your shell treats @ specially (PowerShell, some terminals):

npm install "@google/design.md"

Or run directly (always resolves from the public npm registry):

npx @google/design.md lint DESIGN.md

npm error ENOVERSIONS (“No versions available for @google/design.md”)

The CLI is published as @google/design.md on npm. ENOVERSIONS almost always means npm is not querying the public registry (custom registry= in .npmrc, a corporate mirror that has not synced this package, or a misconfigured @google:registry for the @google scope).

Check your effective registry:

npm config get registry

For a normal install from the internet it should be https://registry.npmjs.org/. After fixing config, retry with npm cache clean --force if a stale 404 was cached.

All commands accept a file path or - for stdin. Output defaults to JSON.


