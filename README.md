# PVZ2 Reimagined

A PvZ2 Gardendless GP-Next datapack project.

## Project layout

```text
pack.json                 # Datapack metadata
jsons/
  config/                 # GP-Next merge/replace configuration
  extensions/             # Optional GP-Next extensions
  features/               # Plant/zombie identities, order, and store data
  lang/                   # Language table patches
  levels/                 # Custom or patched level files
  objects/                # Plant/zombie types, props, and Almanac data
  worldmap/               # Experimental runtime world-map data
reference/original/       # Untouched backups of the exported game data
```

The exported originals are backed up under `reference/original/`. The copies under `jsons/` are active full-data files: edits to them are loaded by GP-Next after **Save & Reload**.

## Recommended workflow

1. Edit the matching full-data file under `jsons/`.
2. Click **Save & Reload** in GP-Next.
3. Check the Data page and Log for the live result and errors.

This project is set up for direct editing of full exports. Keep the files in `reference/original/` unchanged so you can recover from mistakes. Full-data files are convenient but can overwrite whole arrays and may need to be re-exported after a game update.

For a smaller, more update-friendly mod, replace a full file with a minimal `merge` patch later. Do not mix a full export and a minimal patch for the same type unless you understand the load order.

## Plant development

Plant edits normally use:

- `features/PlantFeatures.json` for identity, name, card resource, and availability.
- `objects/PlantTypes.json` for runtime type/inheritance.
- `objects/PlantProps.json` for stats such as cost, cooldown, damage, and toughness.
- `objects/PlantAlmanac.json` for Almanac text and displayed tags.

Keep **Settings → Runtime Extensions → Dynamic Plant Registry** enabled for new or cloned plants.

## Useful console commands

```js
await gpNext.reload()
console.log(gpNext.status())
console.log(gpNext.debug.getPlantRegistry())
```

The developer console is for testing. Permanent changes belong in this datapack.

## Compatibility

This project targets Gardendless 0.7.1 and GP-Next 1.0.0 or newer. Update `pack.json` when the target game version changes.
