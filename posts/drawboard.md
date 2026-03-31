---
title: "Drawboard"
author: "Siegfried Gessulat"
date: "2026-03-31"
categories: []
draft: false
---

Sometimes you just want to sketch something and send it to someone. Drawboard lets you do that — open a link, draw, share the URL. The drawing lives in the link itself, so there's nothing to upload or sign up for.

## What it does

Drawboard is a single HTML file hosted on GitHub Pages that runs in your browser. No server, no login, no dependencies. You open the URL and you're drawing — no account, no onboarding.

- **Freehand drawing** with smooth curves, four colours, and adjustable thickness
- **Straight lines** via Shift+drag, snapped to 10-degree angles
- **Text mode** — press `i` to type, Esc to go back
- **Undo**
- **SVG export**
- **A gallery** of saved drawings in localStorage

The main thing is the **URL sharing**. The drawing gets compressed into the URL hash, so you paste a link and the other person sees your drawing. No upload, no expiring links.

## Why it exists

I kept reaching for drawing tools that were way more than I needed when all I wanted was to scribble something and send it to a colleague. Drawboard loads instantly because there's barely anything to load — no framework, no bundle, no API calls. Works offline, works on your phone.

The interface is minimal on purpose — cream canvas, translucent sidebar, four colours instead of forty, a thickness slider instead of a properties panel. Keyboard shortcuts handle the rest (`Tab` cycles colours, `Cmd+Z` undoes, `?` brings up help).

## Under the hood

The URL sharing does more work than it looks:

1. Strokes get simplified using Ramer-Douglas-Peucker to shed redundant points
2. Coordinates are delta-encoded — differences between points, not absolute positions
3. Signed deltas get zigzag-encoded into compact unsigned varints
4. Everything is deflate-compressed via the browser's native CompressionStream API
5. The result is base64url-encoded into the URL hash

A detailed drawing fits in a link short enough that Slack doesn't complain about it. The title comes along too, packed into the hash as a slug.

**[Try it →](https://gessulat.github.io/drawboard)**
