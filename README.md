> [!IMPORTANT]
> **WARNING:** Tuya Developer Platform DP labels for the tested device are misleading or swapped. Do not rely on those labels alone; use the verified DP mapping below.

# Vybra Tower Fan - Full LocalTuya Integration Guide (Home Assistant)

This guide documents a LocalTuya DP mapping established through reverse engineering and testing of a Vybra Tower Fan. Some Tuya IoT DP labels for this device are swapped or misleading. The mapping is verified for the specific device tested here; other hardware or firmware variants may differ.

## Prerequisites

* Home Assistant with LocalTuya installed.
* A Vybra Tower Fan reachable from Home Assistant on the local network (Tuya local port 6668).
* The device ID and local key for your own device, obtained through an authorized setup method. Do not publish these credentials.

---

# 🛠️ Hardware & Testing Specifications

* **Device:** Vybra Tower Fan (Smart Wi-Fi)
* **Integration:** LocalTuya
* **Connectivity:** Local Wi-Fi (Port 6668)
* **Tested with:** Home Assistant 2026.x + LocalTuya

---

# 📊 Verified DP Map

| DP ID | Function | Values / Type | Note |
| :---: | :--- | :--- | :--- |
| **1** | Main Power | True / False | On/Off |
| **2** | Fan Speed | 1 - 9 (Integer) | Speed control |
| **3** | Wind Modes | `normal`, `heavy`, `sleep`, `fresh` | See "Wind Modes" section below |
| **8** | Oscillation | True / False | Turns rotation On/Off |
| **10** | Temperature | Sensor (°C) | Real-time room temperature |
| **11** | Timer | 0 - 12 (Number) | Countdown timer in hours |
| **102** | Buzzer | True / False | Beep sound toggle |
| **103** | Heater | True / False | PTC Heating Element |
| **106** | UV Lamp | True / False | Sterilization light |

---

# 🔌 Known Working LocalTuya Entity Types

If you are configuring the device via the LocalTuya UI, use these exact entity platforms for the corresponding DPs:

* `fan` → **DP 1** (Power) + **DP 2** (Speed)
* `select` → **DP 3** (Modes)
* `switch` → **DP 8**, **DP 102**, **DP 103**, **DP 106**
* `sensor` → **DP 10** (Temperature)
* `number` → **DP 11** (Timer)

---

# 🌬️ Wind Modes (DP 3)

The internal naming of the modes is counter-intuitive. Use these exact strings (lowercase):

* `heavy` → Strong/High constant wind
* `sleep` → Quiet/Low constant wind
* `fresh` → Medium constant wind
* `normal` → Natural/Pulsating wind (simulates a breeze)

---

# ⚠️ WARNING: Configuration Risk

Modifying `core.config_entries` directly inside the `.storage` folder is highly risky. A single missing comma or bracket can break your entire Home Assistant setup.

It is strongly recommended to configure the device via the LocalTuya UI.

Only use the JSON example below for reference or if you are an advanced user.

---

# ⚙️ JSON Configuration Example (Advanced)

```json
"entities": [
  {
    "friendly_name": "Vybra Heater",
    "id": 103,
    "platform": "switch"
  },
  {
    "friendly_name": "Vybra Sound",
    "id": 102,
    "platform": "switch"
  },
  {
    "friendly_name": "Vybra Oscillation",
    "id": 8,
    "platform": "switch"
  },
  {
    "friendly_name": "Vybra UV",
    "id": 106,
    "platform": "switch"
  },
  {
    "friendly_name": "Vybra Mode",
    "id": 3,
    "platform": "select",
    "select_options": "normal;heavy;sleep;fresh"
  },
  {
    "friendly_name": "Vybra Timer",
    "id": 11,
    "max_value": 12.0,
    "min_value": 0.0,
    "platform": "number",
    "step_size": 1.0
  },
  {
    "friendly_name": "Vybra Tower Fan",
    "id": 1,
    "platform": "fan",
    "fan_speed_control": 2,
    "fan_speed_max": 9,
    "fan_speed_min": 1
  },
  {
    "friendly_name": "Vybra Temperature",
    "id": 10,
    "platform": "sensor",
    "device_class": "temperature",
    "unit_of_measurement": "°C"
  }
]
```

---

# 🔍 Known Issues (Wi-Fi Drops)

The tested device has occasionally become unavailable in Home Assistant. The root cause has not been established.

* **Observed behavior:** The device may become `unavailable` in Home Assistant, sometimes with `receive loop has terminated` warnings in the logs.

* **Possible cause:** The observed Wi-Fi drops appear to originate from the device or network side, but this has not been proven; a LocalTuya issue has not been ruled out.
* **Recovery:** Check device power and network connectivity first. An automatic smart-plug power-cycle is not a universal fix and should not be used as a safety control. Consider it only with a suitably rated smart plug, only if power-cycling this particular device is safe, and never during heating operation (the fan contains a PTC heater).

---

# ☕ Support

This mapping is based on direct reverse engineering and testing. Optional support: [Revolut Me](https://revolut.me/mariannud).

---

**Contributed by:** MA-Linkestis (2026)
