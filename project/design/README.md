# Luwte: design visuals

AI-generated concept visuals of the Luwte sensor unit and how it sits next to an awning.

> **Not committed yet:** `luwte-awning-scene.png`, `luwte-awning-retracted.png` and `luwte-gust-retract.mp4`. In those, the unit came out about 2.5× too large (a ~39 cm box instead of 15 cm, measured against the bricks). They will be regenerated at the correct scale and committed then. They show the intended **look**, not exact engineering: dimensions and parts come from [../proposal.md](../proposal.md) and [../materials.md](../materials.md).

| File | What it is | Made with |
|---|---|---|
| `luwte-device-render.png` | Product render of the sensor unit: IP65 box, cup anemometer, light-sensor dome, radiation shield, cable glands pointing down | GPT Image 2.5 |
| `luwte-device.glb` | 3D model of the sensor unit (PBR textures, ~1.5 M triangles, 43 MB), generated from the render | Tripo H3.1 |
| `luwte-awning-scene.png` | The unit on a Dutch brick house next to an extended striped drop-arm awning | GPT Image 2.5 |
| `luwte-awning-retracted.png` | Same scene with the awning retracted (end frame for the animation) | GPT Image 2.5 |
| `luwte-gust-retract.mp4` | 8 s animation: a gust arrives, the anemometer spins up, the awning retracts | Kling 3.0 pro |

## Viewing the 3D model

- Drag `luwte-device.glb` into <https://gltf-viewer.donmccurdy.com> or open it in Blender (File → Import → glTF 2.0).
- The model is normalised to 1 unit tall. Scale it to about 0.30 m for real size: a 150 mm box, a 120 mm mast and the rotor.
- It's a visual mesh, **not a CAD model**. Don't 3D-print parts from it; model the printed brackets, cups and hub separately to the dimensions in the proposal.

## Design choices shown

- **Anemometer on top, clear of the fabric**, so the awning never blocks the wind measurement.
- **Light sensor above the awning**, so extending the awning doesn't shade its own light sensor.
- **Radiation shield on the side**, so direct sun doesn't heat the outdoor temperature sensor.
- **Cable glands pointing down**, so rain can't run into the box.
