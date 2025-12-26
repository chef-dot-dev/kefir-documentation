# 🥛 Kefir API

A simple REST API for managing kefir fermentation lifecycle — from starting a batch to harvesting and maintaining healthy grains.

This API is designed to be lightweight, predictable, and fermentation-friendly.

You are not in control.
But we provide endpoints anyway.

---

## Base URL

```
https://api.kefir.local
```
If this URL doesn’t resolve, check:
- Your DNS
- Your milk
- Your life choices

*All endpoints are JSON-based.*

---

## Authentication

None.
If someone can access your kefir API, they’ve already won.

---

## Content Type

All endpoints speak fluent JSON.

```
Content-Type: application/json
```
Your kefir does not understand XML and finds YAML judgmental.

---

## Endpoints Overview

| Method | Endpoint  | Description                    |
| ------ | --------- | ------------------------------ |
| POST   | `/create` | Start kefir fermentation       |
| GET    | `/status` | Check fermentation status      |
| GET    | `/kefir`  | Harvest finished kefir         |
| POST   | `/health` | Care and maintain kefir grains |

---

## POST `/create` — Start Kefir Fermentation

Begins a new fermentation cycle.
Milk goes in. Time passes. Science happens.

### Request Body

```json
{
  "milk_type": "cow",
  "volume_ml": 1000,
  "temperature_c": 22
}
```

### Parameters

| Field           | Type   | Required | Description                                                       |
| --------------- | ------ | -------- | ------------------------------------------------------------------|
| `milk_type`     | string | yes      | Type of milk (cow, goat, plant*)                                  |
| `volume_ml`     | number | yes      | Amount of milk in milliliters. Recommended Range 500-1000         |
| `temperature_c` | number | no       | Ambient temperature (default: 21°C)                               |

* Plant milk support may result in *experimental kefir*.
* Lower starting volume results in faster fermentation.

### Response

```json
{
  "batch_id": "kefir_9f3a21",
  "status": "fermenting",
  "started_at": "2025-01-10T08:00:00Z",
  "starting_volume": "1000 ml",
  "estimated_harvest_time_in_hours": 72
}
```

Congratulations. You are now responsible for a living system.

---

## GET `/status/{batch_id}` — Fermentation Status

Returns the current state of the active kefir batch.

### Response

```json
{
  "batch_id": "kefir_9f3a21",
  "status": "fermenting",
  "progress_percent": 72,
  "estimated_ready_in_hours": 6
}
```

### Possible Status Values

* `idle - Waiting for grains.`
* `fermenting - Batch is fermenting.`
* `ready - Batch is tangy and pleasant. Ready for consumption.`
* `overfermented - Batch is very tangy. Harvest and diluting recommended.`

---

## GET `/kefir/{batch_id}` — Harvest Kefir

Harvests kefir once fermentation is complete.

### Behavior

* If kefir is **ready**, returns finished kefir data
* If kefir is **not ready**, returns `409 Conflict`

### Successful Response

```json
{
  "batch_id": "kefir_9f3a21",
  "yield_ml": 950,
  "taste_profile": "tangy",
  "grains_returned": true,
  "harvested_at": "2025-01-10T20:15:00Z"
}
```

### Error Response (Not Ready)

```json
{
  "error": "Fermentation not complete",
  "status": "fermenting"
}
```

### Harvest Notes

* Small metal strainer recommended for harvest
* Kefir is thick. Additional tool for straining required. (Spoon, spatula).
* Extra care is required to not squish the kefir grains through strainer.

---

## POST `/health` — Kefir Care & Maintenance

Used to maintain grain morale between batches.

### Request Body

```json
{
  "action": "rinse",
  "water_type": "filtered"
}
```

### Supported Actions

| Action   | Description                    |
| -------- | ------------------------------ |
| `rinse`  | Rinse grains gently            |
| `rest`   | Rest grains in fresh milk      |
| `feed`   | Feed grains without fermenting |
| `revive` | Recover weak grains            |

### Response

```json
{
  "status": "healthy",
  "next_recommended_action": "feed",
  "message": "Kefir grains are active and balanced"
}
```

---

## Error Handling

All errors follow this format:

```json
{
  "error": "Description of the problem"
}
```

### Common HTTP Status Codes

* `200 OK` – Everything is fine
* `400 Bad Request` – You did this
* `409 Conflict` – Kefir disagrees
* `500 Internal Server Error` – The jar has opinions now

---

## Example Flow

1. `POST /create` → start fermentation
2. `GET /status/{batch_id}` → monitor progress
3. `GET /kefir/{batch_id}` → harvest kefir
4. `POST /health/rinse` → rinse for grains
5. Repeat forever 🌀

---

## Notes

* Over-fermentation may result in **very sour kefir**
* Kefir grains are living organisms — treat them kindly
* This API pairs well with patience

---

## Pause Kefir Production

* You can pause the fermentation cycle of your kefir.
* Put grains with a little bit of milk (50-100 ml) into a small container and place in fridge.
* Kefir will survive 2-3 Weeks while in fridge. 
* This process can be repeated indefinietly. Simply rinse grains and repeat the process

---

## License

MIT — Milk Is Transferable 🥛