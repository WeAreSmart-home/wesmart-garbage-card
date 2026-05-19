# WeSmart Garbage Card

Lovelace card for [WeSmart Garbage](https://github.com/WeAreSmart-home/wesmart-garbage). Shows today's and tomorrow's collections side by side in a hero section, upcoming collections in a list, and includes a built-in schedule editor.

![View mode](https://raw.githubusercontent.com/WeAreSmart-home/wesmart-garbage-card/main/screenshot-view.png)
![Edit mode](https://raw.githubusercontent.com/WeAreSmart-home/wesmart-garbage-card/main/screenshot-edit.png)

---

## Features

- **Phase-aware hero** — shows today and tomorrow side by side; after `remind_hour` (default 18:00) today's group fades to gray and tomorrow's collection becomes the focus
- **Upcoming list** — lists all scheduled collections from the day after tomorrow onward
- **Built-in editor** — tap the gear icon to toggle waste types on/off for each day directly from the card
- **Optimistic UI** — the card updates immediately on tap without waiting for HA state to refresh
- **Theme-aware** — follows light/dark mode automatically with a customisable accent color

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
| `color` | string | `#D97757` | Accent color (hex) — drives the full palette |
| `theme` | string | `auto` | `auto`, `light`, or `dark` |
| `show_weekly_schedule` | boolean | `true` | Show the upcoming collections list (day after tomorrow onward) |
| `remind_hour` | integer | `18` | Hour (0–23) from which the evening urgency phase activates |

**Full example:**

```yaml
type: custom:wesmart-infinite-garbage-lab-card
title: Raccolta Rifiuti
icon: mdi:trash-can
color: "#D97757"
theme: auto
show_weekly_schedule: true
remind_hour: 18
```

---

## Phase system

The card automatically determines a visual phase based on the current day and time. No configuration is required beyond `remind_hour`.

| Phase | Condition | Hero label | Visual |
|-------|-----------|------------|--------|
| `soon` | Next pickup in 2+ days | "Prossimo: [day]" | Neutral |
| `tonight` | Tomorrow has a pickup, before `remind_hour` | "Esporre stasera" | Muted text |
| `urgent` | Tomorrow has a pickup, at or after `remind_hour` | "Esporre adesso" | Amber text + pulsing dot + warm border — today's group fades to gray |
| `today` | Pickup is today | "Ritiro oggi" | Green text + dot + soft green border |

### Hero layout

When collections are scheduled for **both today and tomorrow**, the two groups appear side by side separated by a vertical divider. Once the clock reaches `remind_hour`, today's group fades out (grayscale + reduced opacity) and tomorrow's group becomes the visual focus.

When only today or only tomorrow has a collection, a single group is shown centered.

When neither today nor tomorrow has a collection, the card falls back to showing the next upcoming pickup normally.

### Testing the urgency phase during the day

Set `remind_hour: 10` in the card YAML to simulate the evening state at 10:00. Revert to `18` (or remove the option entirely) for production use.

```yaml
type: custom:wesmart-infinite-garbage-lab-card
remind_hour: 10   # for daytime testing — remove or set to 18 for production
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

Tap the gear icon (top right of the card) to enter edit mode. Each waste type shows a row of 7 day buttons (Monday → Sunday). Tap a day to toggle that waste type on or off — the change is saved immediately to the integration's persistent storage and reflected in the hero without a page reload.

---

## License

MIT
