# CLAUDE.md

This repo is a fork of [three.js](https://github.com/mrdoob/three.js) (default branch: `dev`) used for building 3D web games, plus a set of Blender agent skills for making the game assets.

## Layout

- `src/` – three.js core (ES modules). `examples/jsm/` – addons (loaders like `GLTFLoader`, controls, post-processing).
- `examples/` – live demos; good reference for patterns. `editor/` – the three.js editor.
- `tsl/` – Three Shading Language (WebGPU node materials).
- `build/` – generated bundles; don't hand-edit, rebuild with `npm run build`.
- `test/` – unit (`test/unit`) and e2e screenshot tests (`test/e2e`).
- `docs/`, `manual/`, `llms.txt` – API docs and guides. Check `llms.txt` for a quick API overview.
- `blender-skills/` – 28 Blender 5.2 agent skills (from EL4CTEO/blender-skills, MIT) for modeling, UVs, PBR materials, rigging, animation, optimization/LODs and export.

## Commands

- `npm install` – install dev dependencies.
- `npm start` – build and serve on http://localhost:8080.
- `npm run build` – build `build/three.module.js` etc. via rollup.
- `npm test` – lint core + unit tests (headless via puppeteer).
- `npm run lint-fix` – auto-fix lint across all folders.

## Code style

- Follow the existing three.js style (enforced by ESLint; run `npm run lint`): tabs, spaces inside parentheses, semicolons, ES modules.
- Keep changes to `src/` minimal; put game-specific code in its own folder (e.g. `games/<name>/`) that imports from `src/` or `build/three.module.js`.

## Blender → three.js asset pipeline

- Skills live in `blender-skills/skills/<skill-name>/SKILL.md`; read the relevant one before writing `bpy` code. Start with `blender-core`.
- Export game assets as glTF binary (`.glb`) with the `blender-export-pipelines` skill's `web` preset; see `blender-skills/skills/blender-export-pipelines/references/godot-web.md`.
- Use `blender-game-optimization` for poly budgets and LODs, and `blender-texture-baking` to bake materials to textures (three.js can't read Blender shader nodes).
- Load assets with `GLTFLoader` from `examples/jsm/loaders/GLTFLoader.js`; use `DRACOLoader`/`KTX2Loader` for compressed meshes and textures.
- Units: 1 Blender unit = 1 meter = 1 three.js unit. glTF export converts Blender's Z-up to three.js Y-up.
