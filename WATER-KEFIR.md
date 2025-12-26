# 💧 Water Kefir API

A simple REST API to manage **water kefir fermentation**: start a batch, check progress, harvest when ready, and keep your grains healthy.

All endpoints use JSON.

---

## Base URL

```
https://api.kefir.local
```

---

## Authentication

None.
If someone can access your kefir API, they’ve already won.

---

## Content Type

Send and receive JSON:

```
Content-Type: application/json
```

---

## Endpoints

| Method | Endpoint  | Description                               |
| ------ | --------- | ----------------------------------------- |
| POST   | `/create` | Start a water kefir batch                 |
| GET    | `/status` | Check fermentation status                 |
| GET    | `/kefir`  | Harvest water kefir                       |
| POST   | `/health` | Care / maintenance for water kefir grains |

---

## POST `/create` — Start Water Kefir

Starts a new water kefir fermentation.

### Request Body

```json
{
  "volume_ml": 1000,
  "sugar_percentage": 6,
  "sugar_type": "white",
  "temperature_c": 24,
  "additions": []
}
```

### Fields

| Field                | Type     | Required | Description                                                                                          |
| -------------------- | -------- | -------: | ---------------------------------------------------------------------------------------------------- |
| `volume_ml`          | number   |      yes | Water volume in ml                                                                                   |
| `sugar_percentage`   | number   |      yes | Sugar percentage based on total volume in grams. (Recommended: 4-8%)                                 |
| `sugar_type`         | string   |      yes | Sugar type. Different types of sugar create different flavour profiles.                              |
| `temperature_c`      | number   |       no | Ambient temperature (default: 21)                                                                    |
| `additions`          | string[] |       no | Optional additions (e.g., lemon, ginger, dried fruit)                                                |

### Response

```json
{
  "batch_id": "wk_9f3a21",
  "status": "fermenting",
  "started_at": "2025-01-10T08:00:00Z",
  "expected_fermentation_duration_in_hours": 48
}
```

---

## GET `/status/{batch_id}` — Fermentation Status

Returns the current state of the active batch.

### Response

```json
{
  "batch_id": "wk_9f3a21",
  "status": "fermenting",
  "progress_percent": 72,
  "estimated_ready_in_hours": 6
}
```

### Status Values

* `idle` — no active batch
* `fermenting` — in progress
* `ready` — harvest window
* `overfermented` — will taste drier / more acidic


### Notes

* Water Kefir is ready when bubbles start rising if contents are moved.
* More bubbles mean more activity. Finished product tastes less sweet.

---

## GET `/kefir/{batch_id}` — Harvest Water Kefir

Harvestes the batch if it’s ready.

### Success Response

```json
{
  "batch_id": "wk_9f3a21",
  "yield_ml": 950,
  "taste_profile": "lightly tangy",
  "carbonation_level": "medium",
  "grains_returned": true,
  "harvested_at": "2025-01-10T20:15:00Z"
}
```

### Error Response (Not Ready)

**HTTP 409**

```json
{
  "error": "Fermentation not complete",
  "status": "fermenting"
}
```

---

## POST `/health` — Grain Care

Maintenance actions for water kefir grains.

### Request Body

```json
{
  "action": "feed",
  "notes": "Used brown sugar last batch"
}
```

### Actions

| Action   | Description                               |
| -------- | ----------------------------------------- |
| `rinse`  | Light rinse (only if needed)              |
| `feed`   | Add fresh sugar water (maintenance cycle) |
| `rest`   | Pause grains in a smaller feed            |
| `revive` | Recovery routine for slow grains          |

### Response

```json
{
  "status": "healthy",
  "next_recommended_action": "feed",
  "recommended_wait_hours": 12,
  "message": "Grains are active and stable"
}
```

---

## Error Format

All errors return:

```json
{
  "error": "What went wrong"
}
```

---

## Common HTTP Status Codes

* `200 OK` — success
* `400 Bad Request` — invalid input
* `409 Conflict` — wrong fermentation state (e.g., harvesting too early)
* `500 Internal Server Error` — unexpected issue

---

## Example Flow

1. Start a batch:

   * `POST /create`
2. Monitor:

   * `GET /status/{batch_id}`
3. Harvest:

   * `GET /kefir/{batch_id}`
4. Maintain grains:

   * `POST /health`

---

## Notes

* Harvested Kefir is ready to use with all kinds of fruit juices and other liquids containing sugar.
* Fruit juices usually do not need additional sugar for carbonation.
* Flip-top bottles recommended for filling mixed liquids.
* Burping of bottles recommended after the first 24 hours. (Open bottle and release preassure)
* Mixed liquids are ready when the start foaming while burping. Cooling in the fridge halts the fermentation and makes the liquids foam-free.
* Typical fermentation: **24–48 hours** depending on temperature and sugar source
* “Overfermented” usually means **drier + sharper**
* Grain health is impacted by:

  * mineral content
  * sugar type
  * temperature
  * consistency of feeding

---

## Pause Kefir Production

* You can pause the fermentation cycle of the water kefir.
* Put grains with a little bit of water (50-100 ml) and a teaspoon of sugar into a small container and place in fridge.
* Kefir will survive 2-3 Weeks while in fridge. 
* This process can be repeated indefinietly. Simply rinse grains and repeat the process.

---

## License

MIT — Sugar Is Transferable 💧
