# TOG Rating Card

A Home Assistant Lovelace dashboard card that displays current and forecast sleep clothing recommendations from the [TOG Rating integration](https://github.com/Anton2079/tog-rating). Three card types are available — a compact current-conditions card, a forecast-only card, and a combined card showing all three.

---

## Screenshots

![TOG Rating cards](assets/tog-rating-screenshot-main.png)

![TOG Rating current and forecast cards](assets/tog-rating-screenshot.png)

![TOG Rating entities and device view](assets/tog-rating-screenshot-2.png)

---

## Card Types

### `custom:tog-rating-card` — Combined Card

Displays current conditions, daytime forecast, and nighttime forecast in a single card. Recommended for a dedicated nursery dashboard view.

### `custom:tog-rating-current-card` — Current Conditions Card

Compact card showing only the current recommendation based on live indoor and outdoor temperature. Useful as a quick-glance card in a room overview.

### `custom:tog-rating-forecast-card` — Forecast Card

Shows the daytime and nighttime forecast recommendations without current conditions. Pairs well with a weather or climate card.

---

## Installation

### Via HACS (recommended)

1. In HACS, go to **Frontend > Custom repositories**
2. Add `https://github.com/astrat1/lovelace-tog-rating-card` and select type **Dashboard**
3. Install **TOG Rating Card**
4. Refresh your browser

Or use the button:

[![Add to HACS](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=astrat1&repository=lovelace-tog-rating-card&category=plugin)

### Manual

1. Download `tog-rating-card.js` from the [latest release](https://github.com/astrat1/lovelace-tog-rating-card/releases)
2. Copy it to `config/www/community/lovelace-tog-rating-card/tog-rating-card.js`
3. In HA, go to **Settings > Dashboards > Resources** and add:
   - URL: `/local/community/lovelace-tog-rating-card/tog-rating-card.js`
   - Type: JavaScript module

### Companion Integration

This card requires the TOG Rating integration to provide sensor data:

- Original: [github.com/Anton2079/tog-rating](https://github.com/Anton2079/tog-rating)
- Extended fork (multi-mode, multi-child, milestone automations): [github.com/astrat1/tog-rating](https://github.com/astrat1/tog-rating)

---

## Sensor Naming

The TOG Rating integration creates sensor entities named after the instance title set during config flow setup. If your instance is named **"Nursery TOG"**, the entities will be:

| Sensor | Entity ID |
|---|---|
| Current TOG value | `sensor.nurserytog_current_tog` |
| Current recommendation | `sensor.nurserytog_current_recommendation` |
| Day TOG value | `sensor.nurserytog_day_tog` |
| Day recommendation | `sensor.nurserytog_day_recommendation` |
| Night TOG value | `sensor.nurserytog_night_tog` |
| Night recommendation | `sensor.nurserytog_night_recommendation` |

Entity IDs are derived by lowercasing the instance name and removing spaces. Verify your exact entity IDs in **Developer Tools > States** by searching for your instance name.

---

## Configuration

### Combined Card

```yaml
type: custom:tog-rating-card
title: Nursery TOG
current_tog_entity: sensor.nurserytog_current_tog
current_recommendation_entity: sensor.nurserytog_current_recommendation
day_tog_entity: sensor.nurserytog_day_tog
day_recommendation_entity: sensor.nurserytog_day_recommendation
night_tog_entity: sensor.nurserytog_night_tog
night_recommendation_entity: sensor.nurserytog_night_recommendation
```

### Current Card

```yaml
type: custom:tog-rating-current-card
title: Nursery TOG
current_tog_entity: sensor.nurserytog_current_tog
current_recommendation_entity: sensor.nurserytog_current_recommendation
```

### Forecast Card

```yaml
type: custom:tog-rating-forecast-card
title: TOG Forecast
day_tog_entity: sensor.nurserytog_day_tog
day_recommendation_entity: sensor.nurserytog_day_recommendation
night_tog_entity: sensor.nurserytog_night_tog
night_recommendation_entity: sensor.nurserytog_night_recommendation
```

### Configuration Reference

| Option | Required | Description |
|---|---|---|
| `title` | No | Card title displayed at the top |
| `current_tog_entity` | Combined / Current | Sensor entity for current TOG value |
| `current_recommendation_entity` | Combined / Current | Sensor entity for current recommendation |
| `day_tog_entity` | Combined / Forecast | Sensor entity for daytime forecast TOG |
| `day_recommendation_entity` | Combined / Forecast | Sensor entity for daytime recommendation |
| `night_tog_entity` | Combined / Forecast | Sensor entity for nighttime forecast TOG |
| `night_recommendation_entity` | Combined / Forecast | Sensor entity for nighttime recommendation |

---

## What the Card Displays

Each recommendation panel shows:

- **Headline** — short summary (e.g., "Standard setup", "Cold night — extra warm")
- **TOG value** — numeric TOG rating of the sleep sack or blanket layer
- **Clothing items** — ordered list of what to put on the child (base layer first, then sack or blanket)
- **Effective temperature** — the blended and adjusted temperature used for the recommendation, in °F
- **Outdoor temperature** — raw outdoor reading from the weather entity
- **General message** — one-sentence explanation of why this tier was selected

---

## Styling

The card renders inside a standard `ha-card` element. To apply rounded corners (recommended for Material Design 3 dashboards), use [card-mod](https://github.com/thomasloven/lovelace-card-mod):

```yaml
type: custom:tog-rating-card
title: Nursery TOG
current_tog_entity: sensor.nurserytog_current_tog
current_recommendation_entity: sensor.nurserytog_current_recommendation
day_tog_entity: sensor.nurserytog_day_tog
day_recommendation_entity: sensor.nurserytog_day_recommendation
night_tog_entity: sensor.nurserytog_night_tog
night_recommendation_entity: sensor.nurserytog_night_recommendation
card_mod:
  style: |
    ha-card {
      border-radius: 24px;
      overflow: hidden;
    }
```

---

## Multiple Children

Add one card per child, each pointing to their own integration instance's sensor entities. The integration supports multiple named instances — each child gets their own set of sensors.

```yaml
# Child 1 (instance named "Nursery TOG")
type: custom:tog-rating-card
title: Child 1 TOG
current_tog_entity: sensor.nurserytog_current_tog
current_recommendation_entity: sensor.nurserytog_current_recommendation
day_tog_entity: sensor.nurserytog_day_tog
day_recommendation_entity: sensor.nurserytog_day_recommendation
night_tog_entity: sensor.nurserytog_night_tog
night_recommendation_entity: sensor.nurserytog_night_recommendation

---

# Child 2 (instance named "Room 2 TOG")
type: custom:tog-rating-card
title: Child 2 TOG
current_tog_entity: sensor.room2tog_current_tog
current_recommendation_entity: sensor.room2tog_current_recommendation
day_tog_entity: sensor.room2tog_day_tog
day_recommendation_entity: sensor.room2tog_day_recommendation
night_tog_entity: sensor.room2tog_night_tog
night_recommendation_entity: sensor.room2tog_night_recommendation
```

See the integration README for instructions on adding a second child instance.

---

## Companion Repositories

- **TOG Rating integration (original):** [github.com/Anton2079/tog-rating](https://github.com/Anton2079/tog-rating) — original integration by Anton2079
- **TOG Rating integration (fork):** [github.com/astrat1/tog-rating](https://github.com/astrat1/tog-rating) — adds multi-mode support, toddler blanket mode, automatic base layer recommendations, milestone automations, and multi-child config flow
- **TOG Rating Card (original):** [github.com/Anton2079/lovelace-tog-rating-card](https://github.com/Anton2079/lovelace-tog-rating-card) — original card by Anton2079

---

## Repository Structure

```
lovelace-tog-rating-card/
├── dist/
│   └── tog-rating-card.js    # Compiled card — this is what HACS installs
├── assets/                   # Screenshots used in this README
├── hacs.json                 # HACS plugin metadata
└── README.md
```

HACS installs `dist/tog-rating-card.js` directly. The repository name prefix `lovelace-` is required for HACS dashboard plugin compatibility.
