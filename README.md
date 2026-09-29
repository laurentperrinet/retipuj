# Jupiter from the Inside

An immersive 3D visualization that allows you to explore the surface of Jupiter from the inside.

- https://neuromatch.social/@laurentperrinet/117347485669197298

## 🌌 The Concept

Instead of looking at the planet from space, this project places you at the center of Jupiter. You are surrounded by a massive spherical shell representing the planetary surface, allowing you to navigate the clouds and storm systems from a central perspective. (Disclaimer — this is a simulation, not a scientific model of Jupiter's interior. Jupiter is not empty, and the interior is not hollow. Right?)

## 🎨 Design Guidelines

### Scientific Approach

The project aims to translate complex astronomical data into an intuitive spatial experience.

- **Projection**: It utilizes equirectangular cylindrical maps wrapped onto a sphere, ensuring that the spatial relationships of Jupiter's cloud bands and the Great Red Spot are preserved from the observer's central perspective.
- **Orientation**: A virtual "SUN" marker provides a cardinal reference point, helping the user understand their orientation within the planetary shell.
- **Scaling**: The use of a wide Field of View (FOV) simulation mimics the experience of being inside a gargantuan sphere, where the horizon curves away from the observer.
- **Tessellation**: The wireframe view is a *geodesic by triangulation* — an icosahedron whose every edge is divided in 10 ([frequency ν = 10](https://en.wikipedia.org/wiki/Geodesic_dome#Icosahedron-based_geodesic_domes)), yielding 2,000 near-equilateral triangular facets.
- **Coordinate system**: A scientific grid of latitude and longitude lines is drawn every 15°, with values labeled in steps of 15° (e.g. *15°N*, *30°S*, *45°E*) typeset in [Computer Modern / Latin Modern](https://www.ctan.org/tex-archive/fonts/lm/), the classic TeX typeface.

### Aesthetics: "The Digital Observatory"

The visual language of this project is inspired by futuristic planetary observatories and heads-up displays (HUDs).

- **Color Palette**: The primary interface uses **"Video Blue" (#00BFFF)** family tones, high-contrast colors that ensure legibility against the organic, warm tones of Jupiter's atmosphere. The HUD is matrix-green over the geodesic wireframe and turns deep blue whenever one of Jupiter's images is shown.
- **Atmosphere**: The movement is intentionally smooth and drifting, evoking a sense of weightlessness and the immense scale of the gas giant.
- **Minimalism**: UI elements are designed to be unobtrusive, with a dedicated "Zen Mode" to remove all digital noise, leaving only the planetary vista.

## 🕹️ Controls

### Navigation & Motion

- **Mouse Steering**: the pointer acts as a directional accelerator. Push the pointer
  **down** to look **down** and right to look right (the natural, non-inverted mapping); the
  further from the centre, the faster the turn, up to the speed cap. Holding a mouse button
  multiplies the authority for a deliberate manoeuvre.
- **Saccade-Safe Anchor**: the neutral point of the controller follows the cursor, so you can
  "jump" the pointer to a new position without inducing a violent camera whip.
- **Kinetic Drift**: motion is governed by an **Ornstein–Uhlenbeck** process — mean reversion
  back to Jupiter's own prograde spin plus a subtle organic volatility, which gives the
  weightless, drifting feel.
- **Circular motion**: azimuth λ and elevation β are free-running angles. The shell **wraps** —
  at ±180° in azimuth and continuously **over both poles** in elevation — so there is never a
  wall to hit; `d(λ+180°, 180°−β)` is the same line of sight as `d(λ, β)`, so no direction is
  ever seen twice.
- **Keyboard Motion (`H J K L`)**: `H` turn left, `L` turn right, `K` look up, `J` look down.
- **Dynamic Zoom**:
    - `D`: zoom in (narrow the FOV)
     - `S`: zoom out (widen the FOV)
     - **Trackpad / wheel**: pinch or scroll to zoom, between 30° and 120°.
- **Reset (`R`)**: eased return to the nominal view (λ = 0°, β = 0°, FOV = 78°).
- **Fullscreen (`F`)**: toggle full-screen mode.

### Views & Overlays

- **View Cycle (`P`)** — `Shift+P` steps backwards. The shells are:
     1. **Cassini–Juno mix** *(the default view on load)* — the equirectangular texture map by
      floppastrogeo (`jupiter_texture_map_of_cassini_and_juno_mixed_by_floppastrogeo_dgjn416.jpg`).
     2. **Cassini** — NASA/JPL enhanced-color cylindrical map (`PIA07782.jpg`).
      3. **Triangulated geodesic** — the sun-lit icosahedral triangulation, ν = 10, 2,000 facets.
  Each view gets its own mesh (never a shared material), so a map can never be "skipped" when
  cycling, and the interface recolours itself: matrix green over the geodesic, **dark blue**
  ink on a pale plate over Jupiter's images.
- **Navigation Data (`V`)** — the scientific overlay:
     - **Velocity vector**: an instrument-sized arrow drawn from the reticle along the
      instantaneous line of sight, with `|ω|` in °/s on the shaft and the `λ̇ / β̇` components
      beside the tip.
     - **Telemetry plate**: azimuth λ, elevation β, |ω|, FOV, the SUN coordinates and the
      bearing of the SUN from the current line of sight.
- **Coordinate Grid (`G`)**: latitude and longitude only, drawn every 15° with the values
  printed every 15° (*15°N*, *30°S*, *045°E*, …) in Computer Modern / Latin Modern at the size
  of a map annotation. The equator and the prime meridian are emphasised.
- **Find the SUN (`T`)**: slew onto the virtual SUN marker. The SUN is a cardinal reference
  fixed at λ = +34°, β = +24°; when it leaves the frame an edge chevron points to it and the
  telemetry plate gives its bearing.
- **Full Zen Mode (`Z`)**: hides every UI element — plates, grid, labels, SUN, vector, cursor —
  leaving only the planetary vista.

## 🚀 Local Setup

To run this project locally:

1. Clone the repository.
2. Start a local web server in the root directory:
    ```bash
    python3 -m http.server 8000
    ```
3. Open your browser and navigate to `http://localhost:8000`.

## 🛠️ Technology Stack

- **[Three.js](https://threejs.org/)**: Powering the 3D spherical projection, geodesic tessellation and camera rotations.
- **[Latin Modern](https://www.ctan.org/tex-archive/fonts/lm/)** (Computer Modern): Scientific typography for the coordinate labels.
- **HTML5/CSS3**: For the minimalist HUD UI and status overlays.
- **Stochastic Calculus**: Implementing Ornstein-Uhlenbeck diffusion for organic motion.
- Vibe coded with [Qwen3.8-Flash-Next](https://ollama.com/library/qwen3.8-flash-next) served by [Ollama](https://ollama.com/) and driven from the ChatGPT desktop app — no OpenAI models involved  (see [Vibe Coding](#-vibe-coding)).



## 🤖 Vibe Coding

This project was **vibe coded**: the code was generated by an AI coding agent from natural-language
prompts, then steered, debugged and refined by hand.

- **Model**: [Qwen3.8-Flash-Next](https://ollama.com/library/qwen3.8-flash-next), the open-weight
  multimodal Mixture-of-Experts from the Qwen family, run here in its local
   `qwen3.8-flash-next:125b-mlx` variant (125B total parameters, 6B active per token).
- **Runtime**: [Ollama](https://ollama.com/), serving that model locally on the same machine, so
  the inference never leaves the laptop.
- **Agent front-end**: the [ChatGPT desktop app](https://chatgpt.com/download) (macOS), pointed at
  the local Ollama model and used to edit the files, run the local server and eyeball the rendered
  result frame by frame.
- **No OpenAI models were used**: every line of this repository was produced by the local Qwen
  model served by Ollama, not by a hosted OpenAI model.

The rest — the scientific choices, the textures, the frequency-&nu; geodesic specification and the
typography — was specified, reviewed and signed by hand.

## 🛰️ Data Sources & Licensing

The planetary surface textures used in this project are sourced from:

- **[Juno Mission Media Gallery](https://www.missionjuno.swri.edu/media-gallery/junocam)**
   (provided by [SWRI](https://www.swri.org/)), and in particular the
   **[JunoCam Maps Archive](https://www.missionjuno.swri.edu/junocam/think-tank/maps-archive)**
   — global cylindrical (equirectangular) maps at 10 px/degree with System III longitudes,
  made by Gerald Eichstädt and John Rogers from NASA / JPL / SwRI / MSSS imagery.
- **[PIA07782 — Jupiter, Cylindrical Map (December 2000)](https://photojournal.jpl.nasa.gov/catalog/PIA07782)**,
  Cassini's best map of Jupiter in enhanced colour, from the
  [NASA JPL Photojournal](https://photojournal.jpl.nasa.gov/); the file ships here as
   `PIA07782.jpg` (also on [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Jupiter_Cylindrical_Map_-_Dec_2000_PIA07782.jpg)).
- **[Floppastrogeo](https://www.deviantart.com/floppastrogeo)** —
   [*Jupiter texture map of Cassini and Juno mixed*](https://www.deviantart.com/floppastrogeo/gallery)
   (Cassini mid-latitudes blended with the Juno polar cyclones), the equirectangular map
   `jupiter_texture_map_of_cassini_and_juno_mixed_by_floppastrogeo_dgjn416.jpg` and the default
  view of this project.

### Licensing

- **Code**: 🄯 This project is **copylefted** and released under the **[GNU AGPL-3.0-or-later](LICENSE)**: you may use, study, share and adapt the code, as long as derivatives carry the same licence and make their source available — including when the visualization is served to users over a network.
- **Imagery**: Imagery is used under **Fair Use** for educational and non-commercial
  visualization purposes. All rights to the original textures belong to their respective
  creators and agencies (NASA / JPL / Caltech / SWRI / MSSS / Floppastrogeo).
    - NASA/JPL products such as PIA07782 are **public domain** (not subject to copyright);
      please keep the credit line *NASA / JPL / Caltech*.
    - The JunoCam maps of the Maps Archive are released **CC-BY** by their authors — credit
      *NASA / JPL / SwRI / MSSS / Gerald Eichstädt / John Rogers*.
    - Community texture maps on DeviantArt (Floppastrogeo, FarGetaNik, Askaniy, …) are usually
      published under **CC-BY-NC-SA**: keep the attribution, do not use them commercially, and
      share derivatives under the same licence.
    - The **geodesic tessellation and the graticule are generated in code**, so the frequency-ν
      view has no third-party imagery at all.
- **Fonts**: annotations are set in **Latin Modern Roman** (the TeX Gyda successor to Computer
  Modern), distributed under the [GUST Font License](https://www.ctan.org/tex-archive/fonts/lm/doc/fonts/lm/README)
  via [CTAN](https://www.ctan.org/pkg/lm), and loaded from
  [jsDelivr](https://cdn.jsdelivr.net/gh/geometalab/Latin-Modern-Math-font@master/fonts/otf/lmroman10-regular.otf)
  with a [cdnFonts](https://www.cdnfonts.com/latin-modern-10.font) fallback; the interface uses
  [Inter](https://rsms.me/inter/).
- **Library**: [three.js](https://threejs.org/) (MIT).

> Not redistributing the imagery: only the two maps needed by the visualization are kept in the
> repository, together with their provenance. See [LICENSE](LICENSE) for the code licence (🄯 Copyleft 2026 Laurent Perrinet).
