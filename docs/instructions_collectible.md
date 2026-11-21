# Adding Collectibles

A quick guide to sprites, JSON definitions, and level overlays.

## 1. Create and Store the Sprite

Create the sprite as a PNG with a transparent background and a resolution of **32 × 32 pixels**. Place it in the appropriate category folder. The sprite resolution remains independent of the size at which the collectible is rendered in the game.

### Naming Convention

Use the format `category_variant[_detail].png`. All names must be lowercase, words are separated by underscores, and the category comes first (typically singular in filenames, while category folders use plural names such as `potions` or `coins`).

```text
resources/
└── images/
    └── collectibles/
        ├── potions/
        │   ├── potion_health_red.png
        │   ├── potion_health_blue.png
        │   └── potion_health_green.png
        └── coins/
            └── coin_bronze.png
```

## 2. Add an Entry to collectibles.json

Add a new entry to the `collectibles` array in `resources/data/collectibles.json`:

```json
{
  "id": "potion_health_red",
  "overlayColor": "#FF0000",
  "pickupRadius": 64.0,
  "popTime": 0.15,
  "popScale": 1.5,
  "rotationSpeed": 6.0,
  "sprite": "collectibles/potions/potion_health_red.png",
  "sound": "SOUND_HEALTH_PICK_UP",
  "effect": {
    "type": "heal",
    "amount": 25
  }
}
```

## 3. Parameters

| Parameter | Type / Format | Description |
| :--- | :--- | :--- |
| `id` | `string` | Unique identifier for the collectible. Usually matches the sprite filename without the `.png` extension. |
| `overlayColor` | `string` (hex RGB) | Unique hex color code (e.g. `#FF0000`) matching the pixel color in the level overlay image. |
| `pickupRadius` | `number` (world units) | Distance within which the player can collect the item (e.g. `64.0`). |
| `popTime` | `number` (seconds) | Duration of the pickup animation in seconds. |
| `popScale` | `number` (multiplier) | Peak scale factor during the pickup animation (`1.5` = 1.5× original size). |
| `rotationSpeed` | `number` | Rotation speed during the pickup animation; higher values spin faster. |
| `sprite` | `string` (path) | Path relative to `resources/images/`, e.g. `collectibles/potions/potion_health_red.png`. |
| `sound` | `string` | Sound key played on pickup. Currently, only the temporary placeholder key `SOUND_HEALTH_PICK_UP` is available while the legacy hardcoded asset/constant definitions are being migrated to JSON. |
| `effect` | `object` | Defines what happens when the item is collected (see Section 5). |

## 4. Pickup Animation: Pop and Rotation

After a collectible is picked up, it plays a short animation before being removed: it briefly scales up while rotating.

- `popTime`: Controls how long the animation runs before the collectible disappears.
- `popScale`: Controls the maximum size multiplier reached during the animation.
- `rotationSpeed`: Controls how fast the collectible rotates during the animation.

## 5. Pickup Effects

The `effect` object defines the gameplay outcome when a collectible is picked up. `type` specifies the effect category, and `amount` defines its value or quantity.

Only currently supported effect types can be used; adding new effect types requires extending the collectible behavior on the game side.

**Healing:**

```json
{ "type": "heal", "amount": 25 }
```

**Currency:**

```json
{ "type": "currency", "amount": 1 }
```

## 6. Level Overlay: Spawn Colors

`overlayColor` must match the exact RGB hex value of the corresponding pixel in the level overlay image. This pixel determines where the collectible spawns.

- Each `overlayColor` should be unique across collectibles so spawn colors do not collide.
- When saving level overlay images, avoid color profile conversions or lossy compression that could alter exact RGB values (e.g. turning `#FF0000` into `#FE0000`).

Examples:

| Collectible | Overlay Color |
| :--- | :--- |
| Red Health Potion | `#FF0000` |
| Blue Health Potion | `#0000FF` |
| Green Health Potion | `#00FF6E` |
| Bronze Coin | `#FFA500` |

## 7. Checklist

- [ ] Create the sprite as a 32 × 32 pixel PNG with a transparent background and place it in the matching category folder.
- [ ] Name the file using `category_variant[_detail].png` in lowercase.
- [ ] Add an entry to `collectibles.json` with a unique `id` and a unique `overlayColor`.
- [ ] Paint the matching pixel color in the level overlay image without altering exact RGB values.
- [ ] Configure `pickupRadius`, animation parameters (`popTime`, `popScale`, `rotationSpeed`), `sound` (currently `SOUND_HEALTH_PICK_UP`), and `effect`.
- [ ] Test in-game that the collectible spawns at the expected position, triggers its effect, plays the sound, and animates cleanly on pickup.
