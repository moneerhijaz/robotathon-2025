# Build and flash

## Evidence and prerequisites

The root CMake project includes ESP-IDF and registers `main.c`, `arduino_main.cpp`, and the callbacks component. Vendored Arduino accepts IDF 4.4.0–4.4.99. IDF 4.4.8 is recorded in the sibling 2024 template instructions; it is a candidate reproduction version, not a successfully rebuilt 2025 toolchain in this review. Do not bypass the Arduino version check or silently upgrade to IDF 5.

Install ESP-IDF v4.4.8 using its official installation procedure for your OS, including its supported Python environment and ESP32 toolchain. Then, from the repository root in the exported IDF shell:

```bash
idf.py --version
idf.py -B build build
```

Keep the checked-in `sdkconfig` and `sdkconfig.defaults`. Build out of tree; root `CMakeCache.txt` and `CMakeFiles/` were generated on another machine and have been untracked, with local copies retained. The historical `RobotathonESP32` gitlink has no `.gitmodules`; do not rely on recursive submodule initialization to obtain it.

Only after checking wiring and isolating moving hardware:

```bash
# Replace the port with your actual ESP32 device.
idf.py -B build -p /dev/ttyUSB0 flash monitor
```

Exit monitor with Ctrl+]. The firmware enables the Bluetooth allowlist and forgets pairing keys at startup; inspect Bluepad32 pairing configuration before diagnosing connection failures. Flashing modifies the board and was not performed here.

## Hardware validation checklist (not run)

1. Confirm board variant, supply voltage/current limits, wiring, and motor driver ratings against the physical hardware and `Spy/Spy.kicad_sch`.
2. Raise wheels, disconnect launcher supply, and verify an independent power cutoff.
3. Confirm neutral stick output, each motor direction, and servo travel before energizing mechanisms.
4. Test controller loss with power cutoff ready: current callbacks do not implement a motor stop.
5. Calibrate QTR on the actual surface; calibration rotates the robot. Re-enter line mode after calibration returns to manual.
6. Measure color classification, IR thresholds, and timed 90-degree turns under representative lighting and battery state.
7. Record firmware commit, configuration, controller, test surface, actual observations, and failures before claiming competition behavior reproduced.

No hardware-free firmware CI is advertised until the IDF build is reproduced. Do not treat documentation or host syntax checks as a firmware build.
