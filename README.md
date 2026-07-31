# Power Outlet Card

A custom Lovelace card for Home Assistant to switch outlets and display power, energy and cost with multi-language support.

## Features

- **Switch outlets** — Toggle any switch/smart_plug/outlet entity
- **Power display** — Show current power consumption (W)
- **Energy display** — Show total energy consumption (kWh/MWh/Wh)
- **Cost display** — Show energy costs via Powercalc cost entity OR manual price calculation
- **Manual cost calculation** — Set a price per kWh (e.g. 0.35 €) and the card calculates costs automatically — no Powercalc needed
- **Powercalc auto-detect** — Automatically detects `sensor.{entity}_power`, `_energy`, `_energy_cost` entities
- **Mini sparkline** — Optional 24h power history as small SVG line chart (like the sensor card)
- **3 layouts** — Grid (1-6 columns), List, Compact
- **Multi-language** — German, English, French, Dutch, Spanish (auto-detects HA language)
- **Theme-compatible** — Uses only CSS variables, works with any HA theme
- **Transparent background** — Blends seamlessly with your dashboard
- **Visual editor** — Full configuration via UI, no YAML needed

## Installation

### HACS (recommended)
1. Add this repo as a custom repository in HACS (type: Dashboard)
2. Install
3. Restart Home Assistant
4. Add card to your dashboard

### Manual
1. Copy `power-outlet-card.js` to your `www/custom/` directory
2. Add resource: `/local/custom/power-outlet-card.js` (type: JavaScript Module)
3. Restart Home Assistant
4. Add card to your dashboard

## Configuration

### Visual Editor
Add a new card and search for "Power Outlet". Configure outlets, layout, display options, cost calculation and sparkline in the editor.

### YAML
```yaml
type: custom:power-outlet-card
title: Steckdosen
layout: grid
columns: 2
gap: 8
show_header: true
show_power: true
show_energy: true
show_cost: true
show_icon: true
show_name: true
show_state: true
theme_aware: true
rounded: true
price_per_kwh: 0.35
show_sparkline: true
outlets:
  - entity: switch.waschmaschine
    name: Waschmaschine
    auto_detect: true
  - entity: switch.tv
    name: TV
    power_entity: sensor.tv_power
    energy_entity: sensor.tv_energy
    cost_entity: sensor.tv_energy_cost
```

### Card Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `title` | string | "" | Card title |
| `outlets` | array | [] | List of outlet configs |
| `layout` | string | "grid" | "grid", "list", or "compact" |
| `columns` | int | 2 | Grid columns (1-6, only for grid layout) |
| `gap` | int | 8 | Gap between outlet cards (px) |
| `show_header` | bool | true | Show card title |
| `show_power` | bool | true | Show power consumption |
| `show_energy` | bool | true | Show energy consumption |
| `show_cost` | bool | true | Show energy costs |
| `show_icon` | bool | true | Show outlet icon |
| `show_name` | bool | true | Show outlet name |
| `show_state` | bool | true | Show on/off state |
| `theme_aware` | bool | true | Use theme colors |
| `rounded` | bool | true | Rounded corners |
| `price_per_kwh` | number | 0 | Price per kWh for manual cost calculation (0 = disabled) |
| `show_sparkline` | bool | false | Show 24h power sparkline |

### Outlet Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `entity` | string | required | Switch/smart_plug/outlet entity ID |
| `name` | string | "" | Custom name (empty = entity friendly name) |
| `icon` | string | "" | Custom icon (empty = auto) |
| `power_entity` | string | "" | Power sensor entity |
| `energy_entity` | string | "" | Energy sensor entity |
| `cost_entity` | string | "" | Cost sensor entity (Powercalc, overrides manual calculation) |
| `auto_detect` | bool | true | Auto-detect Powercalc entities |

### Cost Calculation

The card supports two ways to display energy costs:

1. **Powercalc** (automatic): If a `cost_entity` is configured (manually or via auto-detect), the card displays the cost from that entity directly.

2. **Manual calculation** (no Powercalc needed): Set `price_per_kwh` to your electricity price (e.g. `0.35` for 0.35 €/kWh). The card calculates: `cost = energy_kWh × price_per_kwh`. This works with any energy sensor and requires no additional integration.

If both are configured, `cost_entity` takes priority.

## License

MIT