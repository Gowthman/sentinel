# sentinel
Sentinel — an explainable AI watchdog for embedded Linux that detects abnormal process behavior and automatically identifies the root cause, not just the symptom. Built for C-DAC Hackathon 2026, PS-17.
# Sentinel

**An explainable AI watchdog for embedded Linux processes.**

Sentinel continuously monitors per-process CPU, memory, disk I/O, network, threads, and file descriptors on embedded Linux via `/proc`. It uses a statistical baseline plus a lightweight Isolation Forest model to detect abnormal behavior — then, unlike most monitoring tools, it goes one step further: a root-cause correlation engine cross-references the surrounding metrics to identify the *likely cause* (memory leak, CPU saturation, disk bottleneck, crash, and more) and generates a plain-language explanation backed by evidence. All of it runs on-device, with no cloud dependency, on a lightweight local dashboard.

> Built for **C-DAC Hackathon 2026**, Problem Statement 17 — *Embedded Linux Process Watchdog Dashboard* — by **Team CRAZY TECHIES**.

![Sentinel Architecture](docs/architecture.png)

---

## Why Sentinel

Most process-monitoring tools stop at detection — a metric crosses a threshold, an alert fires, and a human is left to figure out what actually happened. Sentinel closes that gap: a dual-signal anomaly fusion layer (statistics + ML) feeds a dedicated root-cause engine that distinguishes between failure classes using multi-metric evidence, and explains its reasoning in plain language instead of raw numbers.

## Problem Statement

> "Embedded Linux lacks a unified monitoring system that continuously tracks application health, detects abnormal resource usage, and highlights the root cause of performance degradation." — C-DAC PS-17

## Key Features

- Continuous per-process + system telemetry via `/proc`, low overhead by design
- Statistical baseline (EWMA/z-score) + Isolation Forest for multivariate anomaly detection
- Root-cause correlation engine covering: CPU saturation, memory leak, memory pressure, disk bottleneck, network bottleneck, process crash, deadlock/hang, excessive process creation, zombie accumulation, file-descriptor exhaustion
- Plain-language, evidence-backed explanations — not just alert labels
- Local web dashboard: Overview, Process List, Process Details, Anomaly Timeline, Root Cause Analysis, Alerts, Historical Trends
- Push-button fault-injection demo mode for reliable live demonstrations
- Fully on-device — no cloud dependency

## Architecture

```
Linux Processes
      ↓
System Monitoring Layer        (/proc, /sys polling)
      ↓
Metrics Collection Agent       (fixed-interval sampling)
      ↓
Time-Series Storage            (SQLite)
      ↓
Statistical Baseline  +  Isolation Forest (ML)
      ↓
Anomaly Scoring & Fusion       (dual-signal, debounced)
      ↓
Root Cause Correlation Engine
      ↓
Alert & Explanation Generator
      ↓
Backend API (FastAPI)
      ↓
Dashboard (React)
      ↓
Alerts / Recommendations
```

Full diagram: [`docs/architecture.png`](docs/architecture.png)

## Tech Stack

Python · scikit-learn (Isolation Forest) · SQLite · FastAPI · React · Linux `/proc` / `/sys`

## Project Structure

```
sentinel/
├── collector/     # Process monitor + metrics collector
├── detection/     # Statistical baseline + Isolation Forest
├── rootcause/     # Root-cause correlation engine
├── alerts/        # Alert manager & explanation generator
├── dashboard/     # Frontend
├── demo/          # Fault-injection demo scripts
├── docs/          # Architecture diagram, technical documentation
└── README.md
```

## Novelty

Most watchdog tools flag *that* something is wrong. Sentinel is built around identifying *why* — a dual-signal anomaly fusion layer feeding a rule-based root-cause correlator that distinguishes failure classes using multi-metric evidence, not a single threshold.

## Status

🚧 Active development for C-DAC Hackathon 2026, Stage 2.

## License

[MIT](LICENSE)

## Team

**CRAZY TECHIES** — C-DAC Hackathon 2026, Problem Statement 17
**CRAZY TECHIES** — C-DAC Hackathon 2026, Problem Statement 17
