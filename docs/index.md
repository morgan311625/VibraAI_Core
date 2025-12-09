# VibraAI_Core — Offline AI Engine for Telematics

VibraAI_Core is a small offline AI engine written in pure C.
It runs on typical ARM Cortex-M MCUs with sub-millisecond inference
and requires only a few kilobytes of memory.

## Engine overview

Every 2-second window, the engine processes:
- accelerometer data (1 to 3 axes)
- vehicle speed (GPS or CAN)
- timestamp

The engine outputs:
- driver behavior score (0 to 100)
- vehicle anomaly score (0 to 1)
- road quality index (0 to 1)

## Footprint

Flash usage: 20 to 30 KB  
RAM usage: 5 to 8 KB  
Inference latency: less than 1 millisecond  
Dependencies: none (pure ANSI C)

## Integration (C API)

```c
int vibra_init(VibraContext *ctx, const VibraConfig *cfg);

int vibra_infer_window(
    VibraContext *ctx,
    const VibraAccelSample *samples,
    float avg_speed_mps,
    long long timestamp_ms,
    VibraMetrics *out_metrics
);
Contact
If you want an evaluation SDK or integration support, you can contact:

morgan5962@live.fr
