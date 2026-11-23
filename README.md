# 736 Arlington Planner

A 1:1 scale 3D model of the house at 736 Arlington Rd, Redwood City, for planning decoration: walk through it, place furniture, repaint walls, change floors.

One static page (`index.html`), no build step. It loads three.js 0.160.0 from jsDelivr and fonts from Google Fonts. The page is marked `noindex`. This repository is public by the owner's choice. It holds only the page and this README: never commit the owner's photos or video, or anything personal.

## Using it

- **Views:** 3D orbit, top-down floor plan, first-person walk (drag to look, WASD or arrow keys). Show both levels, Main or Upper. Toggles for roof, ceilings, cutaway, yard and labels.
- **Furniture:** pick a piece, click a floor to place it, drag to move, `R` to rotate 15°, `Del` to remove. Size, rotation and colour are in the right-hand panel.
- **Rooms:** click a floor to select a room, then change its wall paint and floor material.
- **Marks:** press *Mark a spot*, click any part of the house and write what should change there. *Copy all marks* gives a list with each spot's room and position to send to whoever edits the model.
- **Doors** open as you walk up to them; a click holds one open.
- **Plan options (structure):** the Plan menu also lists built-in options that change the building, not just furniture. *ADU option* adds a custom-built one-bedroom cottage (≈545 sq ft, one bedroom, a full bath, and a pool bath with its own door onto the patio) on the back of the lot behind the patio, 4 ft from the side and rear lot lines, and removes the trees, the shed and the log pile in its way. Open it directly with `#adu` at the end of the address. Original never shows it. A design started while an option is open keeps that option.
- **Saving:** layouts save to this browser's localStorage (key `arlington736.layout.v1`). Export downloads a JSON file; Import loads one.

## Where the data comes from

| Part | Source | Status |
|---|---|---|
| Room sizes (Rec 26×21, Family 15×31, Kitchen 26×11, Dining 11×13, Sitting Room 21×8, Living 23×16, Bedrooms 2–4, Primary 13×22), 3,205 + 1,000 = 4,205 sq ft | Measured floor plan (Floor Plan Visuals, ANSI Z765) in the listing's virtual tour, TourFactory #2912242 | Real |
| Built 1946, 2 stories, 3 baths, composition roof, no garage, heated pool, 3 fireplaces | Compass / MLS ML81865290 (sold Dec 2021) | Real |
| Lot shape, house position, pool outline, terrace, motor court, shed, driveway | Listing site plan, measured at the house's scale (5.72 px/ft) | Real, but the drawing is approximate: the traced lot is about 12,700 sq ft against 11,623 in public records |
| Brick and siding, black window frames, roof forms, fireplaces, wood ceilings, brick walk, planting | The 47 listing photos | Matched by eye |
| Kitchen open to the dining room (wall between them removed); bar and sitting area turned into Bedroom 5; the sunroom by the pool is an office with a standing desk; the bedroom left of the entry is a second office; grand piano by the sunroom | Owner's renovations | Wall and door position of Bedroom 5 assumed |
| Floors (light oak plank, rose marble on the diagonal in the foyer, Saltillo tile with blue inlay in the sunroom office), the white-and-navy kitchen with island and columns, living-room and office furniture, pool shape and safety fence, terrace furniture | The owner's own photos of the house today (not stored in this repository) | Matched by eye |
| Halls, closets, baths, pantry, mechanical rooms, wall thickness, door and window sizes, ceiling heights (9 ft main, 8.5 ft upper), roof pitches, stair shape, plant sizes | Scaled off the plan drawing or estimated | Estimate |

Treat every dimension as ±1 ft and measure on site before buying furniture.

Sources: https://www.compass.com/listing/736-arlington-road-redwood-city-ca-94062/859590691799040889/ and https://www.tourfactory.com/idxr2912242. The listing's plans and photos are not included here.

## Model conventions

Geometry is defined in feet (x = east, y = south, origin = the house's north-west wall corner) and converted to meters for three.js. `ROOMS` holds room polygons; `WALLS` holds wall segments per level as `[x1, y1, x2, y2, 'e'|'i', openings]`, where an opening is `[distanceFromStart, width, type, sill, top]`.

## Layout file format

```json
{
  "v": 1,
  "items": [
    { "id": "i1", "type": "sofa", "name": "Sofa", "x": 15.39, "z": 24.32, "level": 0,
      "rot": 180, "w": 2.2, "d": 0.95, "h": 0.85, "color": "#8C9388", "elev": 0 }
  ],
  "rooms": { "living": { "wall": "#B9C4B0", "floor": "walnut" } },
  "notes": [ { "id": "n1", "x": 8.44, "y": 2.5, "z": 25.0, "level": 0, "text": "This window is wider" } ]
}
```

- `x`, `z`: position in meters from the house's north-west corner (x east, z south). `level`: 0 main, 1 upper. `rot`: degrees. `w`, `d`, `h`: size in meters. `elev`: height off the floor in meters (used by art frames).
- `type` is one of the furniture catalog keys in `index.html` (`CAT`).
- `notes` are the marks: a point in meters (y is height) and the requested change.
- `rooms` is keyed by room id; `wall` is a hex colour, `floor` one of `oak`, `walnut`, `stone`, `marble`, `tile`, `terracotta`, `carpet`, `concrete`, `deck`.

## Deploying

Cloudflare Pages, project `arlington-3d-planner`:

```sh
CLOUDFLARE_ACCOUNT_ID=<account> wrangler pages deploy . --project-name arlington-3d-planner --branch main
```
