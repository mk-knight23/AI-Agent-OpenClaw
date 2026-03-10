---
name: iot-monitor
description: "Dashboard and API layer for monitoring IoT sensors and edge devices from OpenClaw's full-stack environment. Connects to MQTT brokers, HTTP sensor APIs, or PicoClaw agent heartbeats. Renders a live web dashboard, stores readings in Supabase, and sends alerts via any configured channel (Telegram, Slack, WhatsApp). Adapted from PicoClaw's Go iot-monitor for OpenClaw's Node.js full-stack runtime."
---

# iot-monitor

IoT monitoring dashboard — connect PicoClaw edge devices to OpenClaw's full-stack visibility layer.

## Usage
```
@OpenClaw iot-monitor --connect mqtt://192.168.1.100:1883
@OpenClaw iot-monitor --connect picoclaw://home-server:8080    # PicoClaw agent API
@OpenClaw iot-monitor --connect http://sensor-api.local/metrics
@OpenClaw iot-monitor --dashboard                              # Launch live web UI
```

## Architecture

OpenClaw acts as the **orchestration layer** while PicoClaw runs on the hardware:

```
PicoClaw (Raspberry Pi)  →  MQTT / HTTP  →  OpenClaw Dashboard  →  Supabase + Alerts
```

## Features

### Live Dashboard
- React-based real-time dashboard (WebSocket updates)
- Time-series charts per sensor (last 24h, 7d, 30d)
- Alert history and acknowledgment workflow
- Device status: online/offline/degraded

### Data Ingestion
- MQTT subscription: all topics via `sensors/#` wildcard
- PicoClaw heartbeat API integration
- HTTP polling (configurable interval)
- Schema inference: auto-detects numeric sensors vs. categorical

### Alert Rules
```json
{
  "rules": [
    {"sensor": "temperature", "condition": ">75", "channel": "telegram", "severity": "high"},
    {"sensor": "disk_usage", "condition": ">90%", "channel": "slack", "severity": "critical"}
  ]
}
```

### Storage
- All readings stored in Supabase `iot_readings` table
- Automatic data retention: 90 days (configurable)
- Export: CSV, JSON, InfluxDB line protocol

## Files Created
```
dashboard/                  # Next.js dashboard app
  pages/iot.tsx             # Main monitoring page
  components/SensorChart.tsx
config/iot-monitor.json     # Connection and alert config
MONITOR_SETUP.md            # Integration guide for PicoClaw devices
```

## Philosophy
PicoClaw owns the hardware layer. OpenClaw owns the visibility layer. This skill bridges them: raw sensor data becomes actionable insights in your full-stack dashboard.
