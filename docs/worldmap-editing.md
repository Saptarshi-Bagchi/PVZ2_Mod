# World-map editing

The runtime world map is patched through `jsons/worldmap/gpn-worldmap.json`. Keep the example in `docs/worldmap-template.json` as a reference; it is not loaded by GP-Next.

## One-time setup

1. Put the Luxis Lib folder directly under `gp-next/packs/`, with `pack.json` and `bundle.js` at the pack root. The copy currently nested at `Luxis Lib/Luxis Lib/` is one directory too deep for normal pack discovery.
2. Enable JS modding and enable Luxis Lib in Patcher.
3. Add a `LuxisLibProps` object to the active `PropertySheets` data if you want Luxis options such as `WorldOrder`:

```json
{
  "objclass": "LuxisLibProperties",
  "aliases": ["LuxisLibProps"],
  "objdata": {
    "WorldOrder": ["egypt", "pirate", "cowboy", "future", "dark", "beach", "iceage", "lostcity", "eighties", "dino", "modern"]
  }
}
```

`WorldOrder` changes the world-selector order. It does not define the level path inside a world.

## Generate a starting map

Open a world-map screen and run this in the developer console:

```js
luxisLib.dumpWorldMap()
```

Use the returned world object as the value of `worlds` in `jsons/worldmap/gpn-worldmap.json`, then add `apiVersion: 1` and the `$schema` line. Dumping first is important because `mode: "replace"` takes ownership of the complete graph for that world.

## Reorder levels

Inside a world's `map.mainline`, reorder the node objects. Array order creates the automatic progression links:

```json
"mainline": [
  { "id": "first", "type": "level", "levels": ["egypt1"], "title": "1" },
  { "id": "second", "type": "level", "levels": ["egypt3"], "title": "2" },
  { "id": "third", "type": "level", "levels": ["egypt2"], "title": "3" }
]
```

The displayed title is independent of the level ID. Keep every `id` unique and make every `levels` entry an existing level ID.

## Place nodes

Use an absolute position for a mainline node:

```json
"position": { "x": 500, "y": 260, "z": 0 }
```

For a branch, use `relativePosition`, which is measured from the resolved parent:

```json
"relativePosition": { "x": 220, "y": 180, "z": 0 }
```

`reuseOriginalPositions: true` lets nodes without an explicit position reuse the vanilla position at the same mainline index. `autoLayout` can provide fallback spacing when you want generated positions.

## Add plant nodes

Add a plant node to `mainline` or `branches`:

```json
{
  "id": "egypt-repeater-reward",
  "type": "plant",
  "plantReward": "repeater",
  "title": "Repeater",
  "template": { "type": "plant" },
  "relativePosition": { "x": 220, "y": 180 }
}
```

The `plantReward` must be a registered plant codename. To attach a side node explicitly, add its ID to the parent's `children` array. Mainline items are linked to the next mainline item automatically, before any declared `children`.

## Apply and test

1. Save the JSON file.
2. Enable **Experimental → worldmap-json**.
3. In Patcher, click **Save & Reload**.
4. Open the target world and check node order, links, positions, and the plant reward.

If the map disappears or nodes are missing, restore the Luxis dump and make one change at a time. The schema is available at `docs/schemas/gpn-worldmap.schema.json`.
