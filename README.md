# B-17G Tailgunner

A browser game: you're in the Cheyenne tail turret of a B-17G at 24,500 ft, holding off Bf 109s and Fw 190s with twin .50-cals. Single HTML file, Three.js from a CDN, no build step.

## Play

- **Online:** enable GitHub Pages (Settings → Pages → Deploy from branch → `main` / root). The game will be at `https://jefrix.github.io/B17/`.
- **Locally:** the models load over HTTP, so opening `index.html` straight from disk won't find them. From the repo folder run
  `python3 -m http.server 8000` and open <http://localhost:8000>.

**Controls:** mouse to aim · hold left mouse or Space to fire · **G** toggles the K-14 gyro lead pip · **M** mutes audio.
Short bursts — sustained fire overheats the barrels.

## Layout

```
index.html        the whole game (HTML + Tailwind HUD + Three.js module)
assets/b17.glb    player bomber + 2 formation wingmen
assets/bf109.glb  Messerschmitt Bf 109 (Fw 190s are still procedural)
```

The game auto-loads `assets/b17.glb` and `assets/bf109.glb`. You can also hot-swap any `.glb` at runtime with
**LOAD .GLB MODELS** or drag-and-drop; filenames containing `b17`/`fortress`/`boeing` replace the bombers, anything
with `109`/`messerschmitt` replaces the fighters.

## Models

Both models in `assets/` are web-optimized copies of the originals:

| File | Original | Optimized | What changed |
|---|---|---|---|
| `b17.glb` | 34 MB, ~830k verts | 2.5 MB, ~146k verts | 20 hidden cockpit-gauge meshes removed, engines simplified, meshopt-compressed |
| `bf109.glb` | 8.3 MB | 2.0 MB | simplified, meshopt-compressed |

Because they're meshopt-compressed, `index.html` registers `MeshoptDecoder` on the GLTF loader. If you drop in a
new raw model it still loads — the decoder is only used when a file needs it.

To re-optimize a new model, [glTF-Transform](https://gltf-transform.dev) does it in one line:

```
npx @gltf-transform/cli optimize input.glb output.glb --compress meshopt --texture-compress false
```

## Credits

- **B-17:** ["Boeing B-17 Flying Fortress"](https://sketchfab.com/3d-models/boeing-b-17-flying-fortress-927f07f6ddcf470ab0387ce5829024d5)
  by [helijah](https://sketchfab.com/helijah), licensed [CC BY 4.0](http://creativecommons.org/licenses/by/4.0/).
  Modified: cockpit gauges removed, geometry simplified, compressed. *(Attribution is required by the licence and is
  also shown on the game's briefing screen — keep it if you republish.)*
- **Bf 109E-3 "Black 12":** Aviation Gazetteer / Marcus Greenwood — <https://airmuseums.co.uk>, CC0 1.0 (public domain).
- [Three.js](https://threejs.org) r160, [Tailwind CSS](https://tailwindcss.com).
