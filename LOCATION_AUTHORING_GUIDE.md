# Scarlett Isles World Hub – Location Authoring Guide

This file explains how to build explorable settlements and local encounter hooks.

## 1. Overland marker entry

Edit `data/markers.json`.

Each overland marker needs:
- `id`: must match the location key in `data/world_content.json`
- `label`: name shown in the modal
- `x`, `y`: normalized coordinates on the overland map
- `thumb`: marker icon image
- `submapImage`: the concept art or preview image shown when the overland marker is clicked

Example:

```json
{
  "id": "nightwood",
  "label": "Nightwood",
  "x": 0.412,
  "y": 0.537,
  "description": "Ancient boughs shelter Nightwood's halls and shrines.",
  "thumb": "assets/markers/marker_gold.png",
  "submapImage": "assets/submaps/nightwood_preview.png"
}
```

## 2. Local settlement entry

Edit `data/world_content.json`.

Each settlement entry lives under `locations` and must use the same key as the marker `id`.

Example:

```json
"nightwood": {
  "label": "Nightwood",
  "previewDescription": "The first click should show concept art and atmosphere text.",
  "description": "Nightwood sits beneath heavy boughs and old authority.",
  "scaleLabel": "Local town scale. Travel miles are paused here.",
  "mapImage": "assets/location_maps/nightwood_map.png",
  "poiMarkers": [
    {
      "id": "nightwood_clan_quarters",
      "label": "Clan Quarters",
      "type": "clan",
      "icon": "🛡",
      "x": 0.58,
      "y": 0.33,
      "description": "The clan quarters serve as Nightwood's political heart.",
      "keyNpcs": ["Quartermaster Edrik Vane", "Captain Seral Thorn"],
      "services": ["Request an audience", "Gather local political rumours"],
      "notes": ["Good place for clan favour scenes."]
    }
  ],
  "encounters": [
    {
      "id": "nightwood_gate_dispute",
      "label": "Gate Dispute",
      "x": 0.16,
      "y": 0.78,
      "templateId": "town_guard_skirmish",
      "description": "A tense confrontation at Nightwood's gate threatens to spill into violence."
    }
  ]
}
```

## 3. What each field does

- `submapImage` in `markers.json`
  - first-layer preview art in the overland marker modal
- `mapImage` in `world_content.json`
  - second-layer explorable top-down town/city map
- `poiMarkers`
  - clickable local points of interest inside the settlement map
- `encounters`
  - clickable local encounter triggers inside the settlement map
- `templateId`
  - points to an encounter template in `world_content.json > encounterTemplates`

## 4. Wilderness encounter hotspots

Edit `wildernessHotspotsByMapId` in `data/world_content.json`.

Example:

```json
"midland_province": [
  {
    "id": "midland_roadside_ruin",
    "label": "Roadside Ruin",
    "x": 0.62,
    "y": 0.52,
    "templateId": "rootbound_ambush",
    "description": "A ruin or overgrown roadside site that can trigger a wilderness encounter."
  }
]
```

## 5. Encounter templates

Edit `encounterTemplates` in `data/world_content.json`.

Use tiers:
- 1 = levels 1 to 4
- 2 = levels 5 to 8
- 3 = levels 9 to 12
- 4 = levels 13+

Example:

```json
"rootbound_ambush": {
  "name": "Rootbound Ambush",
  "description": "A corrupted forest skirmish suited to sacred groves or dark woodland outskirts.",
  "tiers": {
    "1": [{ "name": "2 Rootbound Thralls", "maxHp": 11 }],
    "2": [{ "name": "2 Rootbound Stalkers", "maxHp": 27 }],
    "3": [{ "name": "3 Rootbound Stalkers", "maxHp": 33 }],
    "4": [{ "name": "4 Rootbound Stalkers", "maxHp": 40 }],
    "default": [{ "name": "Rootbound Stalker", "maxHp": 18 }]
  }
}
```

## 6. Asset folder suggestion

- `assets/maps/` for province maps
- `assets/submaps/` for first-click concept art previews
- `assets/location_maps/` for second-click explorable town/city maps
- `assets/markers/` for marker icons
- `assets/vtt/` for encounter battlemaps

## 7. Safe build order

1. Upload the preview concept art to `assets/submaps/`
2. Add or confirm the overland marker in `data/markers.json`
3. Add the matching location entry in `data/world_content.json`
4. Upload the real explorable town/city map to `assets/location_maps/`
5. Set `mapImage` to that uploaded file path
6. Add `poiMarkers`
7. Add `encounters`
8. Add any needed `encounterTemplates`

