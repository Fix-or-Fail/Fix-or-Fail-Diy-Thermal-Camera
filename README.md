# Fix Or Fail Thermal Camera — V6.3 Final Production

## START HERE — Wiring and pinout

Before powering the hardware, read these two root-level files:

- **`PINOUT.md`** — exact GPIO table matching the final `Config.h`
- **`WIRING_GUIDE.md`** — step-by-step wiring, power checks and first startup
- **`WIRING_DIAGRAM.txt`** — one-page connection map

The GY-MCU90640 in this build is powered from **3.3 V**. Its **TX goes to GPIO44 (ESP32 RX)** and its **RX goes to GPIO43 (ESP32 TX)**.


A polished ESP32-S3 thermal-camera firmware for the **GY-MCU90640 UART 32×24 thermal sensor**, **ILI9341 320×240 TFT**, and **XPT2046 touch controller**.

This repository packages the final production branch that was derived from the V6.2 stability build. The UART stress test achieved **119 accepted frames, 0 decode failures, 0 invalid-temperature frames, 0 skipped sync bytes, and a 1544-byte RX high-water mark** during the final validation run.

## Highlights

- Instant autonomous boot when the ESP32-S3 receives USB/5V power; no Serial Monitor is required.
- 16 KB thermal UART RX buffer with robust `0x5A 0x5A` resynchronisation.
- Bad-pixel repair for a small number of impossible samples while rejecting genuinely corrupt frames.
- Smooth anti-flicker thermal renderer with cached interpolation and 40 MHz TFT SPI target.
- 32×24 thermal input rendered to a clean 320×240 display.
- Full-screen thermal mode and Android-style top-left menu; no bottom menu tabs.
- Movable compact temperature cards and movable/hideable inspection panel.
- Three inspection points, ΔT, smart hotspot, min/max, zoom, mirror, palettes, Repair Mode and isotherm.
- 8-point touch calibration stored in ESP32 NVS.
- Built-in stability and UART diagnostics (`B`, `U`, `D`).

## Repository layout

```text
firmware/FixOrFail_ThermalCam_V6_3_FinalProduction/  Arduino sketch and source
/docs/                                               Wiring, setup, diagnostics and guides
/.github/                                            Issue templates
```

## Start here

1. Read [Hardware & Pinout](docs/HARDWARE_PINOUT.md).
2. Wire the display, touch controller and thermal module using [Wiring Guide](docs/WIRING_GUIDE.md).
3. Install the Arduino libraries in [Flashing Guide](docs/FLASHING_GUIDE.md).
4. Open `firmware/FixOrFail_ThermalCam_V6_3_FinalProduction/FixOrFail_ThermalCam_V6_3_FinalProduction.ino`.
5. Select your ESP32-S3 board and upload.
6. Run the 8-point touch calibration if needed.
7. Run `B` and `U` in Serial Monitor to validate a new build.

## Hardware used by this firmware

- ESP32-S3 N16R8 class development board
- GY-MCU90640 UART thermal module, 32×24
- ILI9341 320×240 SPI TFT
- XPT2046 resistive touch controller

See the exact GPIO map in [docs/HARDWARE_PINOUT.md](docs/HARDWARE_PINOUT.md).

## Important electrical notes

The GY-MCU90640 in this build is powered from the ESP32's **3.3V rail**. GPIO pins are 3.3V logic and must **never be driven with 5V**. If powering the whole unit from an external 5V supply, feed the board through its appropriate **5V/VIN** input and common GND; do not put 5V onto the 3V3 pin.

## Auto-rotate

Automatic physical orientation detection is **not enabled** in V6.3 because the current hardware has no confirmed accelerometer/IMU. See [docs/AUTO_ROTATE_OPTION.md](docs/AUTO_ROTATE_OPTION.md) for a safe future upgrade path.

## Validation status

The firmware source in this release is the same production branch used for the final V6.2/V6.3 hardware diagnostics. It has been runtime stress-tested on the project hardware. This GitHub package itself has not been rebuilt by an ESP32 Arduino compiler in this packaging environment, so users should compile it locally before flashing.

## License

MIT. See [LICENSE](LICENSE).
