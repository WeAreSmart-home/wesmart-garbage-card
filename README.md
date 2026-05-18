# WeSmart Garbage Card

Lovelace card for [WeSmart Garbage](https://github.com/WeAreSmart-home/wesmart-garbage). Shows today's and upcoming garbage collections, and includes a built-in schedule editor.

![View mode](https://raw.githubusercontent.com/WeAreSmart-home/wesmart-garbage-card/main/screenshot-view.png)
![Edit mode](https://raw.githubusercontent.com/WeAreSmart-home/wesmart-garbage-card/main/screenshot-edit.png)

---

## Features

- **Hero view** — shows what to put out today or the next upcoming day
- **Weekly calendar** — lists all scheduled collections for the next 7 days
- **Built-in editor** — tap the gear icon to toggle waste types on/off for each day directly from the card
- **Optimistic UI** — the card updates immediately on tap without waiting for the HA state to refresh
- **Theme-aware** — follows light/dark mode automatically, with a customisable accent color

---

## Requirements

- Home Assistant 2024.1.0 or newer
- [WeSmart Garbage integration](https://github.com/WeAreSmart-home/wesmart-garbage) installed and configured

---

## Installation

### Via HACS (recommended)

1. Open HACS → **Frontend**
2. Click the three-dot menu → **Custom repositories**
3. Add `https://github.com/WeAreSmart-home/wesmart-garbage-card` — category **Dashboard**
4. Click **Download**
5. Add the resource in **Settings → Dashboards → Resources** (HACS does this automatically on most setups)

### Manual

1. Copy `wesmart-infinite-garbage-lab-card.js` to `/config/www/`
2. Go to **Settings → Dashboards → Resources** and add:
   - URL: `/local/wesmart-infinite-garbage-lab-card.js`
   - Type: **JavaScript Module**

---

## Usage

Add a manual card to any dashboard:

```yaml
type: custom:wesmart-infinite-garbage-lab-card
```

### Configuration options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `title` | string | `Raccolta Rifiuti` | Card title |
| `icon` | string | `mdi:trash-can` | Header icon |
| `color` | string | `#D97757` | Accent color (hex) |
| `theme` | string | `auto` | `auto`, `light`, or `dark` |
| `show_weekly_schedule` | boolean | `true` | Show the upcoming collections list below the hero |

**Full example:**

```yaml
type: custom:wesmart-infinite-garbage-lab-card
title: Raccolta Rifiuti
icon: mdi:trash-can
color: "#D97757"
theme: auto
show_weekly_schedule: true
```

---

## Waste types

The card has five built-in waste types with preset icons and colors:

| Name | Icon | Color |
|------|------|-------|
| Umido | `mdi:leaf` | Brown |
| Plastica | `mdi:recycle` | Amber |
| Carta | `mdi:package-variant` | Blue |
| Vetro | `mdi:bottle-wine` | Green |
| Indifferenziata | `mdi:delete-empty` | Grey |

---

## Editing the schedule

Tap the gear icon (top right of the card) to enter edit mode. Each waste type shows a row of 7 day buttons (Monday → Sunday). Tap a day to toggle that waste type on or off — the change is saved immediately to the integration's persistent storage.

---

## License

MIT
