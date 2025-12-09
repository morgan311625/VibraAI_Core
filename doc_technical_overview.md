# VibraAI_Core — Technical Overview (Evaluation)

This document gives a high-level view of the constraints and integration requirements
for the VibraAI_Core engine.

It is intentionally generic and does not expose internal training details.

---

## 1. Runtime footprint (typical)

These numbers are indicative and may vary slightly depending on compiler and options.

- **Flash / ROM**
  - Runtime code: ~20–30 KB
  - Quantized model: ~8–12 KB
- **RAM**
  - Working memory per 2-second window: \< 10 KB

Target platforms:

- ARM Cortex-M0/M3/M4F class MCUs
- Clock speeds from ~32 MHz and up
- Zephyr, FreeRTOS or bare-metal environments

---

## 2. Input signal requirements

**Accelerometer**

- Axes: 1 to 3 (3-axis recommended)
- Sampling rate: **50–200 Hz**
- Resolution: standard MEMS accelerometers are sufficient

**Windowing**

- Default window length: **2 seconds**
- Each window is processed independently
- For each window you provide:
  - pointer to the accelerometer samples
  - average vehicle speed for the window
  - timestamp (ms or s)

---

## 3. Outputs

For each processed window, the engine returns:

- `driver_score` (0–100)  
- `vehicle_anomaly` (0–1)  
- `road_quality` (0–1)  

These scores are designed to be:

- smoothed on the host side (moving averages, thresholds)
- forwarded to your telematics / analytics backend

---

## 4. Integration pattern (high-level)

A typical integration loop looks like:

1. Configure accelerometer + speed source (GPS / CAN).
2. Buffer N samples corresponding to a 2-second window.
3. Call the VibraAI_Core API function with the window.
4. Read back the three scores.
5. Optionally log or transmit them.

The SDK provides:

- C header(s) for the public API,
- a pre-compiled binary for the core engine,
- and a small reference example for a generic MCU.

---

For access to the evaluation SDK or more detailed documentation,
please contact:

- **Email**: _\[your email here]_
