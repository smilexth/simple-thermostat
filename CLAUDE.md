# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Simple Thermostat is an independent fork of `nervetattoo/simple-thermostat` — a custom Lovelace card for Home Assistant that provides a compact, modular thermostat UI. Built with Lit 3 (LitElement), TypeScript, and bundled via Rollup. Distributed through HACS.

## Build Commands

```bash
yarn build          # Production build → dist/simple-thermostat.js (minified)
yarn dev            # Watch mode (tsc --watch)
```

There is no test runner configured in package.json. Tests exist in `src/test/` but have no run script.

## Architecture

**Entry point**: `src/simple-thermostat.ts` registers two custom elements:
- `simple-thermostat` → `SimpleThermostat` class from `src/main.ts`
- `simple-thermostat-editor` → `SimpleThermostatEditor` from `src/editor.ts`

**Rendering pattern**: Components in `src/components/` are pure functions returning Lit `html` templates (not LitElement subclasses). The main `SimpleThermostat` class calls them from its `render()` method:
- `renderHeader()` — card header with icon, name, faults, toggle
- `renderSensors()` — sensor display (v2 config) or `renderTemplated()` (v3 config with Squirrelly)
- `renderModeType()` — HVAC mode, fan, preset, swing control buttons
- `renderInfoItem()` — individual sensor item layout

**Config parsing**: `src/config/` contains functions that transform raw card YAML config into runtime structures. Key types are in `src/config/card.ts` (CardConfig interface, MODES enum).

**State management**: Temperature updates are debounced (500ms). The `hass` setter syncs HA state into the component, with `_updatingValues` flag preventing overwrite of pending local changes.

**Sensor config versions**: v2 uses entity-based sensors; v3 uses Squirrelly template strings with custom variables.

## Build Pipeline

Rollup produces two outputs from `src/simple-thermostat.ts`:
- `dist/simple-thermostat.js` — production (minified, no sourcemaps)
- `dist/simple-thermostat.debug.js` — debug (sourcemaps, unminified)

CSS is processed through PostCSS → postcss-lit (converts to Lit `css` tagged templates). All dependencies (lit, debounce-fn, squirrelly) are bundled into the single output file.

## HACS Distribution

The built `dist/simple-thermostat.js` MUST be committed — HACS serves it directly. The `hacs.json` manifest points to `dist/` (`content_in_root: false`). Always run `yarn build` and commit the dist artifact with source changes.

## Key Files

- `src/main.ts` — Main card class (~530 lines), all core logic
- `src/types.ts` — Shared types (HASS, Sensor, HVAC_MODES, ControlMode)
- `src/config/card.ts` — CardConfig interface defining all YAML options
- `src/styles.css` — Scoped styles (imported as Lit css via rollup plugin)
- `rollup.config.js` — Build config with shared plugin chain
