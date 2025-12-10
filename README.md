# VibraAI_Core

VibraAI_Core is a small offline AI engine designed for **telematics and vehicle monitoring devices**.

It runs entirely in **pure C**, with an **8-bit quantized model** (a few KB) and **sub-millisecond inference** on typical ARM Cortex-M MCUs.

The engine processes, every 2-second window:

- accelerometer data (1 to 3 axes)
- vehicle speed (GPS or CAN)
- a timestamp per window

And produces three metrics:

- **driver_score** (0–100) – driver behavior / smooth vs aggressive
- **vehicle_anomaly** (0–1) – vibration anomaly indicator for the vehicle
- **road_quality** (0–1) – relative road roughness index

---

## Key properties

- **Language**: C (no external ML framework required)
- **Runtime**: tiny, MCU-friendly
- **Model**: 8-bit quantized, a few KB
- **Latency**: \< 1 ms on typical Cortex-M (e.g. 64 MHz M4F)
- **Memory footprint (typical)**  
  - Flash: ~20–30 KB runtime + 8–12 KB model  
  - RAM: \< 10 KB working memory per window

---

## Integration overview

The engine is meant to be compiled directly into an existing firmware.

Typical integration steps:

1. Configure your accelerometer sampling (50–200 Hz, 3-axis recommended).
2. Aggregate samples into 2-second windows.
3. Call the C API with:
   - pointer to the window buffer
   - average speed for the window
   - timestamp
4. Read the three output scores and forward them to your platform / logger.

No cloud, no on-device training, no external ML runtime:  
**all inference is done locally on the MCU.**

---

## Use cases

- Fleet telematics and driver scoring  
- Vehicle vibration monitoring / anomaly detection  
- Road quality mapping and infrastructure analytics  
- Embedded / IoT devices where cloud connectivity is limited or expensive

---

## Evaluation SDK

A closed-source **evaluation SDK** (C library + headers + example integration) is available for companies that want to test the engine on their own hardware.

If you are interested in evaluating VibraAI_Core on your devices, feel free to contact me:

- **Email**: _\[your email here]_  

I can provide:

- an evaluation binary,
- a short technical sheet,
- and a minimal C example for a typical Cortex-M platform.


## Community & Support
Join the Discord server for discussions, support and early-access:
https://discord.gg/FrcTc6vv4M
