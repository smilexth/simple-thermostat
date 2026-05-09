<!--
  Sync Impact Report
  Version change: 1.0.0 → 2.0.0 (MAJOR — principle removal)
  Modified principles:
    - REMOVED: "I. Upstream Compatibility" (no longer syncing with upstream)
    - RENUMBERED: II → I, III → II, IV → III
  Added sections: none
  Removed sections: none (Upstream Compatibility principle removed)
  Templates requiring updates:
    - .specify/templates/plan-template.md ✅ no changes needed (generic)
    - .specify/templates/spec-template.md ✅ no changes needed (generic)
    - .specify/templates/tasks-template.md ✅ no changes needed (generic)
  Follow-up TODOs: none
-->

# Simple Thermostat Constitution

## Core Principles

### I. HACS Compliance

The built artifact (`dist/simple-thermostat.js`) MUST be committed
and valid for HACS distribution at all times. The `hacs.json` manifest
MUST remain accurate. Every release MUST be installable via HACS
without manual intervention by the end user.

### II. Web Component Standards

All UI components MUST use Lit 3 (LitElement) and follow the
reactive property pattern. Custom elements MUST be self-contained
with scoped styles. Global state and side effects outside the
component lifecycle MUST be avoided. CSS custom properties MUST
be used for theming to maintain compatibility with Home Assistant
themes.

### III. Build Integrity

`rollup.config.js` is the single source of truth for the build
pipeline. The production build MUST produce a single bundled JS
file. Dev and production builds MUST both succeed before any
commit to master. TypeScript strict mode MUST remain enabled.

## Technology Stack

- **Runtime**: Home Assistant Lovelace (custom card)
- **Framework**: Lit 3 / LitElement 4.x
- **Language**: TypeScript (strict mode)
- **Bundler**: Rollup 4.x
- **Template engine**: Squirrelly (for user-configurable templates)
- **Distribution**: HACS (Home Assistant Community Store)
- **License**: MIT

## Development Workflow

Changes follow this process:

1. Implement and test locally in a Home Assistant dev environment.
2. Run `yarn build` and verify `dist/simple-thermostat.js` is
   produced without errors.
3. Commit both source and built artifact together.

## Governance

This constitution governs all development on this project.
Amendments require updating this file with a version bump and
documenting the rationale in the commit message. All changes
MUST align with the principles above; deviations require explicit
justification in the PR description.

This is an independent project. There is no obligation to track
or merge from any upstream source.

**Version**: 2.0.0 | **Ratified**: 2026-05-10 | **Last Amended**: 2026-05-10
