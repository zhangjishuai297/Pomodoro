# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A fully customizable, single-file HTML Pomodoro timer. No build tools, no dependencies — just open `pomodoro.html` in a browser.

## Architecture

All code lives in `pomodoro.html` (HTML + CSS + JS). Three layers:

- **State machine** (`state` var): `focus` → `shortBreak` → `focus` … → `longBreak` → `focus` (next round). Transition logic in `handleSessionComplete()`. Current round/pomodoro tracked via `currentRound`/`currentPom`.
- **Timer engine**: Uses `Date.now()`-based `endTime` for accuracy even when tab is backgrounded. `setInterval` every 200ms for display refresh. `startTimer()`, `pauseTimer()`, `resetTimer()`, `skipSession()`.
- **Persistence**: Settings and daily stats (today's completed pomodoros, total focus seconds) stored in `localStorage`. Daily stats automatically reset when date changes.

## Key behaviors

- **Notification permission** is requested once at page load (`init` block), never on every start click.
- **Audio** uses Web Audio API oscillator (no sound files needed). 3-note ascending beep repeats every 2s until user interacts.
- **Daily stats** keyed by date (`pomodoro_daily_YYYY-MM-DD`). Cross-midnight detection via `setInterval` every 60s.
- **Settings** key: `pomodoro_settings`. Defaults: 25min focus, 5min short break, 15min long break, 4 pomodoros/round, 5 rounds/day.

## Common tasks

- **Launch**: `open pomodoro.html`
- **Lint**: No tooling configured; manually validate HTML/JS syntax with `node --check` on extracted JS, or open in browser.
- **After changes**: Refresh the browser tab — no build step needed.
