# hass2pi

Integrate [Home Assistant](https://www.home-assistant.io/) with [AVEVA PI](https://www.aveva.com/en/products/aveva-pi-system/) using Node-RED, MQTT, and the AVEVA Adapter for MQTT.

---

## Overview

**hass2pi** bridges Home Assistant entity state events to AVEVA PI via MQTT. When any entity state changes in Home Assistant, the Node-RED function transforms the event payload into a structured MQTT message that the AVEVA Adapter for MQTT can consume and write into PI Data Archive or Asset Framework.

```
Home Assistant  →  Node-RED (hass2pi function)  →  MQTT Broker  →  AVEVA Adapter for MQTT  →  PI
```

---

## Contents

| File | Description |
|------|-------------|
| [`Node-RED - hass2pi - function.json`](./Node-RED%20-%20hass2pi%20-%20function.json) | Importable Node-RED flow containing the `hass2mqtt` function node |
| [`Enumerations Sets - Home Assistant Binary Sensors.xml`](./Enumerations%20Sets%20-%20Home%20Assistant%20Binary%20Sensors.xml) | AVEVA Asset Framework enumeration set for all Home Assistant binary sensor device classes |

---

## Node-RED Setup

### Prerequisites

- [Node-RED](https://nodered.org/) with the [Home Assistant](https://github.com/zachowj/hass-node-red) add-on installed
- An MQTT broker (e.g. Mosquitto)
- AVEVA Adapter for MQTT configured to subscribe to `hass2pi/#`

### Import the Flow

1. In Node-RED, open the menu → **Import**
2. Paste or upload `Node-RED - hass2pi - function.json`
3. Wire the nodes:

```
[Events: all]  →  [hass2mqtt function]  →  [MQTT out]
```

### MQTT Topic Format

The function publishes to topics in the following format:

```
hass2pi/{entity_name}/{domain}
```

**Examples:**

| Entity ID | MQTT Topic |
|-----------|------------|
| `binary_sensor.front_door` | `hass2pi/front_door/binary_sensor` |
| `sensor.living_room_temperature` | `hass2pi/living_room_temperature/sensor` |
| `input_datetime.alarm_time` | `hass2pi/alarm_time/input_datetime` |

### Payload

The MQTT payload is a JSON object containing the entity's state attributes, with the state value added under the metric key (either the `device_class` attribute, or `state` if no device class is set).

**Example — a door sensor opening:**

```json
{
  "device_class": "door",
  "friendly_name": "Front Door",
  "door": "on"
}
```

### Special Handling: Datetime Entities

For `input_datetime.*` and `datetime.*` entities, the state value is parsed and serialised as an ISO 8601 string before being published.

---

## AVEVA Asset Framework Setup

### Binary Sensor Enumeration Sets

Import `Enumerations Sets - Home Assistant Binary Sensors.xml` into Asset Framework to map the binary `on`/`off` states from Home Assistant to meaningful PI enumeration values.

**To import:**

1. Open **PI System Explorer**
2. Navigate to **Library → Enumeration Sets**
3. Right-click → **Import** → select the XML file

The following enumeration sets are included, matching all [Home Assistant binary sensor device classes](https://www.home-assistant.io/integrations/binary_sensor/):

| Enumeration Set | On State | Off State |
|----------------|----------|-----------|
| `binary_sensor` | On | Off |
| `binary_sensor.battery` | Low | Normal |
| `binary_sensor.battery_charging` | Charging | Not Charging |
| `binary_sensor.cold` | Cold | Normal |
| `binary_sensor.connectivity` | Connected | Disconnected |
| `binary_sensor.door` | Open | Closed |
| `binary_sensor.door (invert)` | Closed | Open |
| `binary_sensor.garage_door` | Open | Closed |
| `binary_sensor.gas` | Gas Detected | No Gas (Clear) |
| `binary_sensor.heat` | Hot | Normal |
| `binary_sensor.light` | Light Detected | No Light |
| `binary_sensor.lock` | Unlocked | Locked |
| `binary_sensor.moisture` | Moisture Detected (Wet) | No Moisture (Dry) |
| `binary_sensor.motion` | Motion Detected | No Motion (Clear) |
| `binary_sensor.moving` | Moving | Not Moving (Stopped) |
| `binary_sensor.occupancy` | Occupied | Not Occupied (Clear) |
| `binary_sensor.opening` | Open | Closed |
| `binary_sensor.plug` | Plugged In | Unplugged |
| `binary_sensor.power` | Power Detected | No Power |
| `binary_sensor.presence` | Home | Away |
| `binary_sensor.problem` | Problem Detected | No Problem (OK) |
| `binary_sensor.running` | Running | Not Running |
| `binary_sensor.safety` | Unsafe | Safe |
| `binary_sensor.smoke` | Sound Detected | No Sound (Clear) |
| `binary_sensor.vibration` | Vibration Detected | No Vibration (Clear) |
| `binary_sensor.window` | Open | Closed |

---

## License

[MIT](./LICENSE)
