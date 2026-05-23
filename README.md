> [!IMPORTANT]
> **WARNING:** Opening a Tuya Developer Platform account alone won't do the job for you, because the DP labels there are completely scrambled and incorrect. This mapping is the only way to get everything working properly!

# Vybra Tower Fan - Full LocalTuya Integration Guide (Home Assistant)

After 15+ hours of reverse engineering and trial-and-error, I have successfully mapped all functions of the Vybra Tower Fan for LocalTuya. This device has a very non-standard DP mapping where many labels are swapped or misleading in the Tuya IoT logs.

## 🛠️ Hardware & Testing Specifications
* **Device:** Vybra Tower Fan (Smart Wi-Fi)
* **Integration:** LocalTuya
* **Connectivity:** Local Wi-Fi (Port 6668)
* **Tested with:** Home Assistant 2026.x + LocalTuya

## 📊 Verified DP Map (The "Golden" List)

| DP ID | Function | Values / Type | Note |
| :---: | :--- | :--- | :--- |
| **1** | Main Power | True / False | On/Off |
| **2** | Fan Speed | 1 - 9 (Integer) | Speed control |
| **3** | Wind Modes | normal, heavy, sleep, fresh | See "Wind Modes" section below |
| **8** | Oscillation | True / False | Turns rotation On/Off |
| **10** | Temperature | Sensor (°C) | Real-time room temperature |
| **11** | Timer | 0 - 12 (Number) | Countdown timer in hours |
| **102** | Buzzer | True / False | Beep sound toggle |
| **103** | Heater | True / False | PTC Heating Element |
| **106** | UV Lamp | True / False | Sterilization light |

## 🔌 Known Working LocalTuya Entity Types
If you are configuring the device via the LocalTuya UI, use these exact entity platforms for the corresponding DPs:
* `fan` -> **DP 1** (Power) + **DP 2** (Speed)
* `select` -> **DP 3** (Modes)
* `switch` -> **DP 8**, **DP 102**, **DP 103**, **DP 106**
* `sensor` -> **DP 10** (Temperature)
* `number` -> **DP 11** (Timer)

## 🌬️ Wind Modes (DP 3) - Crucial Info!
The internal naming of the modes is counter-intuitive. Use these exact strings (lowercase):
* `heavy` -> Strong/High constant wind.
* `sleep` -> Quiet/Low constant wind.
* `fresh` -> Medium constant wind.
* `normal` -> Natural/Pulsating wind (Simulates a breeze).

## ⚠️ WARNING: Configuration Risk
Modifying `core.config_entries` directly in the `.storage` folder is highly risky. A single missing comma or bracket can break your entire Home Assistant setup. It is strongly recommended to configure the device via the LocalTuya UI. Only use the JSON below for reference or if you are an advanced user.

## ⚙️ JSON Configuration Example (Advanced)
```json
"entities": [
  {"friendly_name": "Vybra Heater", "id": 103, "platform": "switch"},
  {"friendly_name": "Vybra Sound", "id": 102, "platform": "switch"},
  {"friendly_name": "Vybra Oscillation", "id": 8, "platform": "switch"},
  {"friendly_name": "Vybra UV", "id": 106, "platform": "switch"},
  {"friendly_name": "Vybra Mode", "id": 3, "platform": "select", "select_options": "normal;heavy;sleep;fresh"},
  {"friendly_name": "Vybra Timer", "id": 11, "max_value": 12.0, "min_value": 0.0, "platform": "number", "step_size": 1.0},
  {"friendly_name": "Vybra Tower Fan", "id": 1, "platform": "fan", "fan_speed_control": 2, "fan_speed_max": 9, "fan_speed_min": 1},
  {"friendly_name": "Vybra Temperature", "id": 10, "platform": "sensor", "device_class": "temperature", "unit_of_measurement": "°C"}
]


## 🔍 Known Issues (Wi-Fi Drops)
Like many Tuya devices, the Vybra Tower Fan suffers from unstable or poorly optimized Wi-Fi firmware.
* **The Issue:** You might occasionally see the device go `unavailable` in Home Assistant or see `receive loop has terminated` warnings in your logs.
* **The Fix:** This is a firmware issue, not a LocalTuya bug. It is highly recommended to plug the fan into a smart plug and create an automation to power-cycle it if it drops off the network for too long.

---

### ☕ Support my work!
This mapping took over **15+ hours** of reverse engineering and dozens of Home Assistant restarts to perfect. If this guide saved you from the same frustration, feel free to support my work!

**Every small tip is appreciated!**
* **Revolut Me:** [revolut.me/mariannud](https://revolut.me/mariannud)

Thank you!

---
**Contributed by:** MA-Linkestis (2026)
