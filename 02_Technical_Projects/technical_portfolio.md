# 4. Technical Portfolio (Embedded Systems / Edge AI)

The portfolio is built around a **single evolving project**: a *Sensor Anomaly Monitor* — a real-time data acquisition and anomaly detection system. Each phase re-implements the same application on a different architectural base, creating a coherent, cross-comparable portfolio.

> **Core thesis**: *Same problem. Different architecture. Each time, deeper.*

The pipeline is constant across all phases:
```
[Sensor Acquisition] → [Feature Extraction] → [Anomaly Detection] → [Alert / Log Output]
```

---

## 4.1 Stack Progression

| Phase | Period | Architectural Base | Status |
| :---- | :----- | :----------------- | :----- |
| Phase 1 | Summer 2026 | STM32 + HAL + FreeRTOS | 🔵 In Progress |
| Phase 2 | Autumn 2026 | Embedded Linux (Buildroot) + ONNX Runtime | 🔲 Planned |
| Phase 3+ | 2027+ | Zephyr / Rust / CAN bus | 🔲 Future |

---

## 4.2 Phase 1 — STM32 + FreeRTOS (Summer 2026)

**Base**: STM32 Nucleo · HAL + LL drivers · FreeRTOS · C

**Sensor**: IMU (I2C) — accelerometer + gyroscope data stream

**Detection**: Statistical thresholding (mean ± 2σ) as baseline; TFLite Micro quantized model as stretch goal

**FreeRTOS tasks**:
- `task_acquire` — poll sensor at fixed rate via I2C
- `task_process` — compute rolling statistics / run inference
- `task_output` — UART log + GPIO alert pin

**Deliverables**:
- Public GitHub repo: firmware source, wiring diagram, README with setup instructions
- Measured: inference/detection latency (ms), RAM usage, false positive rate

---

## 4.3 Phase 2 — Embedded Linux (Autumn 2026)

**Base**: Raspberry Pi 4 · Buildroot image · C++ userspace daemon

**Application**: Same sensor monitor pipeline, ported to Linux userspace — same sensor, same detection logic

**Key differences from Phase 1**:
- Scheduling via `pthread` / Linux scheduler instead of FreeRTOS tasks
- Sensor I/O via sysfs / I2C device file
- ONNX Runtime (C++ API) replacing TFLite Micro
- Output: structured JSON log via stdout, managed by systemd

**Deliverables**:
- Buildroot config + userspace daemon on GitHub
- Brief latency/footprint comparison vs. Phase 1 (bare-metal vs. Linux)

---

## 4.4 Future Directions (2027+)

Each future phase applies the same pipeline to a new architectural base:

- **Zephyr RTOS + CAN bus** — port to Zephyr on STM32/nRF52; swap IMU input for CAN frame stream (extends IDS research).
- **Sovereign Silicon (RISC-V)** — implement custom hardware accelerators for the anomaly pipeline on a RISC-V FPGA platform (e.g., PolarFire). Focus on ISO 26262 safety extensions.
- **Memory-Safe Rust + CRA** — full rewrite in Rust using `no_std` + RTIC. Implement "secure-by-design" principles required by the European Cyber Resilience Act.

---

## 4.5 Technology Radar

| Technology | Priority | Rationale |
| :--------- | :------- | :-------- |
| C/C++ (bare-metal + HAL) | ✅ Core | Hiring baseline for all embedded roles |
| FreeRTOS | ✅ Core | Phase 1 scheduling base |
| Embedded Linux (Buildroot) | 🔴 High | Required for systems-level roles (NXP, Nokia) |
| TFLite Micro / ONNX Runtime | 🔴 High | Edge AI differentiator — thread constant |
| Rust (embedded) | 🔴 High | Memory safety + CRA compliance (50% production jump) |
| RISC-V | 🔴 High | Open hardware; target for "Sovereign Silicon" track |
| CAN bus / ISO 11898 | 🟡 Medium | Automotive — bridges IDS research to portfolio |
| Zephyr RTOS | 🟡 Medium | Growing adoption; Phase 3 base |
| Docker / CI (GitHub Actions) | ✅ Core | Portfolio reproducibility |
| CRA Compliance | ✅ Core | Legal mandate for European connected devices |
