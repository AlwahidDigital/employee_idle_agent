# Calibration notes (fill during pilot)

## Environment

- Date:
- Cameras used: cam-01, cam-02
- Lighting: day shift / mixed
- Model: YOLOv8s-pose (planned)

## Observed motion scores

| Scenario | Zone | Typical motion score | Notes |
|----------|------|----------------------|-------|
| Active work | | | |
| Idle present | | | |
| Away empty | | | |
| Aisle traffic | | | |

## Decisions

- Final `idle_threshold`: _(default 0.08 until measured)_
- Final `active_threshold`: _(default 0.12)_
- `idle_confirm_seconds`: 180
- `away_confirm_seconds`: 300
- Polygon changes: _(list zone ids)_

## Sign-off

- Calibrated by:
- Date:
- Ready for live ERP posts: no / yes
