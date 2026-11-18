# Factory CCTV AI Agent — Employee Idle Time (Phase 1)

On-prem computer-vision pipeline that detects employee **idle** and **away** time at factory workstations from existing CCTV feeds, then POSTs structured events to your software/ERP API.

**Phase 1 scope:** Employee idle time only.  
**Deferred:** Tools/fixed-asset theft, raw-material theft (see [docs/phase2-deferred.md](docs/phase2-deferred.md)).

## Architecture overview

```
CCTV (RTSP) → Video Ingest → YOLO Person Detect + ByteTrack
    → Zone ROI Engine → Idle State Machine → Event Queue
        → ERP API Adapter (+ PostgreSQL audit store)
```

Full design: [docs/architecture.md](docs/architecture.md)

## Repository layout

| Path | Purpose |
|------|---------|
| `config/cameras.yaml` | Pilot cameras, RTSP, coverage |
| `config/zones.yaml` | Workstation ROI polygons |
| `config/idle_rules.yaml` | Thresholds, shifts, breaks, escalations |
| `config/erp_api.yaml` | Outbound API endpoint & auth |
| `config/hardware.yaml` | On-prem GPU sizing for pilot |
| `docs/` | Architecture, API contract, checklists |
| `schemas/idle_event.schema.json` | Event payload JSON Schema |
| `openapi/erp-events.yaml` | OpenAPI for outbound events |
| `recordings/` | Place pilot CCTV sample clips here |

## Quick start (after camera credentials are filled in)

1. Complete [docs/camera-zone-inventory.md](docs/camera-zone-inventory.md) with real RTSP URLs.
2. Confirm [config/idle_rules.yaml](config/idle_rules.yaml) against your shifts.
3. Set ERP URL/token in [config/erp_api.yaml](config/erp_api.yaml) (use env vars for secrets).
4. Drop sample clips into `recordings/` per [docs/pilot-recordings.md](docs/pilot-recordings.md).
5. Proceed to implementation spike: RTSP → YOLOv8 person detect on `cam-01`.

## Tech stack (locked)

- Ingest: RTSP / ONVIF (OpenCV / FFmpeg)
- Detect: YOLOv8n/s + ByteTrack + YOLOv8-pose
- Host: On-prem NVIDIA GPU (or Jetson for 1–2 cams)
- Services: Python, FastAPI, Redis Streams, PostgreSQL
- Outbound: HTTPS REST with retry + idempotency keys

## Privacy

Phase 1 uses anonymous track IDs only — no face recognition.
