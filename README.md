# 🔥 Forest Fire Detection using Raspberry Pi

An IoT environmental monitor that continuously reads **temperature and humidity** with a DHT11 sensor and shows live readings on a 16×2 I²C LCD. Rising temperature combined with falling humidity is the classic early sign of forest-fire risk, and this project is the sensing and display base for a fire-detection node.

![Raspberry Pi](https://img.shields.io/badge/Raspberry%20Pi-A22846?logo=raspberrypi&logoColor=white)
![Python](https://img.shields.io/badge/Python-3-3776AB?logo=python&logoColor=white)
![IoT](https://img.shields.io/badge/IoT-sensors-orange)

## How it works

```
DHT11 sensor ──GPIO4──▶ Raspberry Pi ──I²C──▶ 16×2 LCD (PCF8574 @ 0x27)
                           │
                           └──▶ console log
```

Every 2 seconds the script:
1. reads temperature (°C) and relative humidity (%) from the DHT11
2. writes `Temp: XXC` on LCD line 1 and `Humidity: XX%` on line 2
3. logs the reading to the console, and retries if a read fails (DHT11 reads fail often, so failures are handled without crashing)

On `Ctrl+C` the LCD is cleared and released cleanly.

## Hardware

| Component | Qty |
|---|---|
| Raspberry Pi (any model with a 40-pin header) | 1 |
| DHT11 temperature & humidity sensor | 1 |
| 16×2 LCD with PCF8574 I²C backpack | 1 |
| Jumper wires / breadboard | – |

### Wiring

| Device | Pin | Raspberry Pi |
|---|---|---|
| DHT11 | VCC | Pin 1 (3.3V) |
| DHT11 | DATA | Pin 7 (GPIO4) |
| DHT11 | GND | Pin 6 (GND) |
| LCD | VCC | Pin 17 (3.3V) |
| LCD | SDA | Pin 3 (GPIO2) |
| LCD | SCL | Pin 5 (GPIO3) |
| LCD | GND | Pin 9 (GND) |

## Setup

1. Enable I²C: `sudo raspi-config` → *Interface Options* → *I2C*.
2. Check the LCD address (it should show `27`):
   ```bash
   sudo apt install -y i2c-tools && i2cdetect -y 1
   ```
3. Install the dependencies:
   ```bash
   sudo apt install -y libgpiod2
   pip install -r requirements.txt
   ```
4. Run:
   ```bash
   python3 forest_fire_monitor.py
   ```

## Next steps

- Fire-risk alert: buzzer or LED when the temperature crosses a threshold and humidity drops below one
- Flame (IR) and smoke (MQ-2) sensors for direct fire detection
- Send alerts over Wi-Fi (MQTT, Telegram or email) from remote forest nodes
- Log readings to the cloud for trend analysis

## License

[MIT](LICENSE) © Chinmay Kambali
