# Jupiter from the Inside

An immersive 3D visualization that lets you explore the surface of Jupiter **from the inside** —
you sit at the centre of the planet and turn your head, while the cloud tops wrap around you as a
spherical shell.

Single-file web app: [`index.html`](index.html), no build step, no bundler.

- https://neuromatch.social/@laurentperrinet/117347485669197298

## 🌌 The Concept

Instead of looking at the planet from space, this project places you at the center of Jupiter.
You are surrounded by a massive spherical shell representing the planetary surface, allowing you
to navigate the clouds and storm systems from a central perspective.

> **Disclaimer** — this is a *simulation*, not a scientific model of Jupiter's interior.
> Jupiter is not empty, and the interior is not hollow. Right?

What is scientifically modelled is the **viewpoint**, not the interior: the geometry, projection,
illumination and kinematics of an observer fixed at the centre of a sphere.

## 🎨 Design Guidelines

### Scientific Approach

The project aims to translate complex astronomical data into an intuitive spatial experience.

- **Inside-out projection.** The camera sits at the origin and the shell is rendered with
  `THREE.BackSide`, so the map is seen from its concave side. Nothing else is in the scene: no
  floor, no skybox, no planetary body. The observer's position is a *given* of the design, and
  every other choice follows from it.
- **Equirectangular cylindrical maps.** The textures are global cylindrical (equirectangular)
  maps — equal increments of planetocentric latitude and longitude per pixel — wrapped onto the
  sphere. That preserves the spatial relationships of the cloud belts and the Great Red Spot from
  the central perspective, and it is what makes the azimuth seam and the poles manageable.
- **Map selection.** Only maps that cover the full 360° × 180° are used, so the poles are never
  blank: **PIA07782** (*Cassini's Best Maps of Jupiter, Cylindrical Map*, December 2000,
  3601 × 1801 px) and a **Cassini–Juno mix** by floppastrogeo, which is the view you get on load.
  Maps cropped at high latitude were rejected for that reason.
- **Coordinate system.** A scientific graticule of latitude and longitude is drawn every 15° and
  labelled every 15° (*15°N*, *30°S*, *045°E*, …) in
  [Computer Modern / Latin Modern](https://www.ctan.org/tex-archive/fonts/lm/), the classic TeX
  typeface. The equator and the prime meridian are emphasised, the 45° multiples are brighter than
  the intermediate parallels and meridians, and the labels are sized like a printed map annotation
  rather than UI text. Longitude is printed as East or West from the centre of the map
   (000°–180°E, 015°–165°W), which is the only defensible prime meridian for a texture whose own
  meridian is arbitrary.
- **Tessellation.** The wireframe view is a *geodesic by triangulation* — an icosahedron whose
  every edge is divided in 10 ([frequency ν = 10, class I](https://en.wikipedia.org/wiki/Geodesic_dome#Icosahedron-based_geodesic_domes)),
  yielding `20 · ν² = 2 000` near-equilateral triangular facets. Because a frequency-10 geodesic no
  longer *looks* icosahedral, the **30 generating great-circle edges** and the **12 pentagonal
  vertices** (where five facets meet) are drawn brighter on a slightly larger radius, so the
  icosahedral ancestry reads at a glance.
- **Layered radii, no z-fighting.** Everything lives on nested radii around the observer —
  graticule 0.980 · R, geodesic edges 0.9905 · R, shaded carrier 0.9915 · R, parent icosahedron
  0.9935 · R, image shell 0.999 · R (R = 100) — so overlays never fight the map for the depth test.
- **Illumination.** The tessellation is lit by the **virtual SUN**: a
  `DirectionalLight` along the SUN direction, an ambient fill, and a cool counter-fill, on a
  `MeshLambertMaterial` with `flatShading` — one flat tone per facet, which is what makes the
  triangulation legible as a structure rather than as noise. The SUN is a *cardinal reference*,
  fixed at **λ = +34°, β = +24°**: ahead-right of the default view and high enough to rake the
  shell like a terminator. It is deliberately inside the 78° field of view on load (~41° off
  axis), so the light direction is visible the moment the page appears.
- **Colour fidelity.** The whole pipeline is sRGB-correct: `renderer.outputColorSpace = SRGBColorSpace`
  and `texture.colorSpace = SRGBColorSpace` on every map, with maximum anisotropy and mipmapping.
  Without the colour-space declaration, the enhanced-colour maps render visibly washed out next to
  the same file opened in an image viewer — a discrepancy that was reported by eye and fixed by
  the colour-management declaration, not by fiddling with saturation.
- **Motion model.** Free motion is governed by an **Ornstein–Uhlenbeck** process: exponential
  relaxation of the angular velocity towards a drift attractor, plus organic volatility. It never
  diverges (a plain random walk would), it always returns to a resting state, and the resting
  state is Jupiter's own prograde spin. The retrograde direction is damped four times harder than
  the prograde one, so the shell has a preferred sense of rotation.
- **Topology of the view.** Azimuth λ is folded to ±180°; elevation β is *deliberately left
  unbounded* and mirrored when it passes a pole, so flying over the north pole hands you a
  continuous, roll-free view of the southern sky. The identity
  `d(λ + 180°, 180° − β) ≡ d(λ, β)` means every line of sight is seen exactly once — there is no
  duplicated hemisphere — and the texture wraps with `RepeatWrapping` in azimuth so there is no
  seam to hit. There is never a wall.
- **Measured angular rate, not nominal rate.** The instrument reads the **true** angular velocity
  of the line of sight, `|ω|`, obtained from the chord of the path sampled symmetrically about
  *now* — not from `hypot(λ̇, β̇)`. On a sphere, λ̇ is weighted by cos β: at β = 89.5° a nominal
  azimuthal rate of 85.94°/s is a real rotation of 0.75°/s, so a naive readout is dominated by
  azimuthal bookkeeping near the poles and the velocity arrow points wherever the sub-pixel noise
  leans. The arrow therefore draws only above 0.02°/s of *real* rotation, and its heading comes
  from the tangent of the sampled path, which folds correctly over the poles.
- **Steering sign convention.** Pushing the pointer **down** looks **down**, and right looks right
  — the non-inverted mapping — verified against the direction of image flow rather than by
  impression.

### Aesthetics: "The Digital Observatory"

The visual language is inspired by futuristic planetary observatories and heads-up displays.

- **Color Palette**: The primary interface uses **"Video Blue" (#00BFFF)** family tones, high-contrast
  colors that ensure legibility against the organic, warm tones of Jupiter's atmosphere. The HUD is
  matrix-green (#00FFAA) over the geodesic wireframe and turns deep instrument blue (#10386F /
  #0B3D91 ink on a pale plate) whenever one of Jupiter's images is shown. The rule is luminance
  contrast against what is *behind* the text, not a single brand colour.
- **Instrument metaphor.** Every plate is an instrument, not a web widget: a pale or dark backing
  plate with a soft shadow, `Computer Modern` for anything scientific (coordinates, labels,
  attribution) and [Inter](https://rsms.me/inter/) for interface chrome, keys shown as keycaps
  (`<kbd>`) beside a word label, and an `aria-pressed` state so toggles are readable by assistive
  tech as well as by eye.
- **Atmosphere.** The movement is intentionally smooth and drifting, evoking weightlessness and the
  immense scale of the gas giant. There is no roll, no artificial horizon, and no clamping — the
  shell moves, you turn your head.
- **Minimalism.** UI elements are designed to be unobtrusive. The top-left plate was deliberately
  stripped of its keystroke manual (that job belongs to the buttons), and a dedicated **Zen Mode**
  removes all digital noise, leaving only the planetary vista.
- **Feedback without clutter.** Cycling a view announces the map by name and provenance in a
  centred banner for a moment; the SUN is marked when in frame and reduced to an edge chevron plus
  a telemetry bearing ("◇ turn →") when out of it — dead astern, where no honest bearing exists,
  the chevron stays out of the way instead of popping around.

## 🕹️ Controls

### Navigation & Motion

- **Mouse Steering**: the pointer acts as a directional accelerator. Push the pointer **down** to
  look **down** and right to look right (the natural, non-inverted mapping); the further from the
  centre, the faster the turn, up to the speed cap. A 26 px dead zone keeps the view still while
  your hand is on the pointer, and holding a mouse button multiplies the authority ×2.1 for a
  deliberate manoeuvre.
- **Saccade-Safe Anchor**: the neutral point of the controller follows the cursor, so you can
  "jump" the pointer to a new position without inducing a violent camera whip. All speed comes from
  the pointer's offset from *that* anchor, not from the centre of the display.
- **Kinetic Drift**: motion is governed by an **Ornstein–Uhlenbeck** process — mean reversion back
  to Jupiter's own prograde spin plus a subtle organic volatility, which gives the weightless,
  drifting feel. Steering relaxes to the requested rate with a 0.34 s time constant, and inertia
  glides back to the planetary drift with a 3.6 s one.
- **Circular motion**: azimuth λ and elevation β are free-running angles. The shell **wraps** — at
  ±180° in azimuth and continuously **over both poles** in elevation — so there is never a wall to
  hit; `d(λ+180°, 180°−β)` is the same line of sight as `d(λ, β)`, so no direction is ever seen
  twice.
- **Keyboard Motion (`H J K L`)**: `H` turn left, `L` turn right, `K` look up, `J` look down —
  Vim-style, one row, no arrow keys needed.
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
  3. **Triangulated geodesic** — the sun-lit icosahedral triangulation, ν = 10, 2 000 facets.
  Each view gets its own mesh and its own material (never a shared material), so a map can never be
  "skipped" when cycling, and the interface recolours itself: matrix green over the geodesic,
  **dark blue** ink on a pale plate over Jupiter's images. A cache-busting revision is appended to
  every map URL, because a stale or cached-404 reply is the usual reason a freshly added map
  "never appears".
- **Navigation Data (`V`)** — the scientific overlay:
  - **Velocity vector**: an instrument-sized arrow drawn from the reticle along the instantaneous
    line of sight, carrying only `|ω|` in °/s on its shaft (the `λ̇ / β̇` components were moved to
    the plate, where they are readable, instead of crowding the tip). Its resting length is
    0.11 · min(W,H) and it grows as √|ω| up to ≈ 0.44 · min(W,H), so an arrow that *looks* fast
    genuinely *is* fast.
  - **Reticle**: a centre ring and four tick marks — the reference for "where am I looking".
  - **Telemetry plate**: four rows — `λ / β`, `|ω| / FOV`, `λ̇ / β̇`, and `☉ SUN / bearing`
    (signed rates in °/s with a real minus sign, and the SUN's bearing with a "● in view" /
    "◇ turn →" hint).
- **Coordinate Grid (`G`)**: latitude and longitude only, drawn every 15° with the values printed
  every 15° (*15°N*, *30°S*, *045°E*, …) in Computer Modern / Latin Modern at the size of a map
  annotation. The equator and the prime meridian are emphasised.
- **Find the SUN (`T`)**: slew onto the virtual SUN marker. The SUN is a cardinal reference fixed
  at λ = +34°, β = +24°; when it leaves the frame an edge chevron points to it and the telemetry
  plate gives its bearing.
- **Full Zen Mode (`Z`)**: hides every UI element — plates, grid, labels, SUN, vector, cursor —
  leaving only the planetary vista. The pointer still steers in Zen: what Zen removes is the
  *instruments*, not the *immersion*. Only the Zen button stays, dimmed until you hover it, and
  the bar's plate turns transparent.

### The instrument bar & the header

- **Instrument bar (bottom-right)**: physical buttons for every keystroke —
  `◀ ▲ ▼ ▶` motion (the twins of `H K J L`, hold to keep turning),
  `P View`, `V Nav data`, `G Grid`, `T Find SUN`, `S Wider`, `D Tighter` (hold to keep zooming),
  `R Reset`, `F Fullscreen`, `Z Zen`. Motion keys are also on the bar as a directional pad, because
  a mouse-free visitor still needs a way to turn; the pointer remains the primary controller, so
  holding a motion button parks the pointer until you let go, and hovering the bar parks it too —
  the instrument you are pointing at never steers the view.
- **Header (top)**: copyleft, licence, source and imagery provenance, on one centred line above the
  plates. It used to be a footer, which collided with the instrument bar. The strip itself is
  click-through (`pointer-events: none`) but its links are not, and every external link opens in a
  new tab with `rel="noopener"`; the text scales down on narrow windows and is hidden below 680 px.
- **Layout rule**: plates never overlap. The header reserves a height (`--header-h`) and the
  top-left plate and the GitHub badge are offset below it.

## 🔬 Calibration & Verification

The constants below are the ones in [`index.html`](index.html) and are the result of iterative
eyeballing plus headless checks.

| Quantity | Value | Why |
|---|---|---|
| Shell radius `R` | 100 (abstract units) | scale-free; only ratios matter |
| Map tessellation | 128 × 64 segments | smooth silhouette, cheap geometry |
| Geodesic frequency ν | 10 → `detail = 9` | 2 000 near-equilateral facets; three.js divides an edge into `detail + 1` |
| Graticule step | 15°, labelled every 15° | scientific readability without a grey wall of lines |
| Label height | 1.55 world units (rasterised at 46 px) | reads as type printed *on a map*, not as UI |
| Pointer sensitivity | 0.0032 rad/s per px | ≈18°/s per 100 px beyond the dead zone |
| Dead zone / drag boost | 26 px / ×2.1 | stillness under the hand; deliberate manoeuvres |
| Speed cap `MAX_SPEED` | 2.8 rad/s (≈160°/s) | fast enough to re-aim in ~2 s, slow enough to stay orientable |
| Steering / idle time constants | 0.34 s / 3.6 s | responsive acceleration, long weightless glide back to the drift |
| Idle drift | 0.85°/s prograde, ×0.25 retrograde | Jupiter-like spin feel, preferred sense of rotation |
| Heading sampling | ±20 ms about *now* | exact tangent of a steady turn, folds over the poles |
| Arrow floor | 0.02°/s of real rotation | below that, pointing is noise |
| FOV | 30° – 120°, default 78° | 78° gives the "inside a gargantuan sphere" feel on load |
| SUN | λ = +34°, β = +24° | ahead-right, inside the load FOV, rakes the tessellation |

How these were checked, rather than assumed:

- the true-rate and pole-folding identities were verified by **sampling the sphere** — ~865 k poses
  for the chord/heading consistency (worst gap 0.03°), a 714 k-pose audit of the arrow sign against
  the actual image motion (zero conflicts), and 260 k poses for the SUN chevron (no non-finite
  bearings);
- a full-viewport sweep before those fixes found ~5 400 poses where the arrow was more than 45°
  off the real direction of travel — the reason `|ω|` is measured and not computed from λ̇, β̇;
- `node --check` on the module for syntax, HTTP checks on both map URLs for availability, and
  frame-by-frame inspection of the rendered page in the browser for everything else.

Known imprecision, kept honest: the idle drift is *nominally* 0.85°/s, but the fast steering
relaxation competes with the slow idle relaxation, so the resting drift you actually observe is
closer to ~0.07°/s — the two relaxations are coupled, so the slow one never fully pulls the
velocity to its own attractor. It reads as an ambient rotation, which is the intent; the number is
a target, not a measurement.

## 🚀 Local Setup

To run this project locally:

1. Clone the repository.
2. Start a local web server in the root directory:
   ```bash
   python3 -m http.server 8000
   ```
3. Open your browser and navigate to `http://localhost:8000`.

The page needs network access on first load (three.js from a CDN, the two webfonts). Opening
`index.html` directly from the filesystem also works, in which case the cache-busting query
strings are skipped.

## 🛠️ Technology Stack

- **[Three.js](https://threejs.org/)** (r160, MIT): the 3D spherical projection, geodesic
  tessellation and camera rotations.
- **[Latin Modern](https://www.ctan.org/tex-archive/fonts/lm/)** (Computer Modern): scientific
  typography for the coordinate labels and every instrument caption.
- **[Inter](https://rsms.me/inter/)**: interface chrome.
- **HTML5 / CSS3**: the minimalist HUD — CSS custom properties carry the whole two-theme palette,
  a second full-screen 2D `<canvas>` carries the vector and the SUN chevron.
- **Stochastic calculus**: Ornstein–Uhlenbeck diffusion for organic, non-diverging motion.
- Vibe coded with [Qwen3.8-Flash-Next](https://ollama.com/library/qwen3.8-flash-next) served by
  [Ollama](https://ollama.com/) and driven from the ChatGPT desktop app — no OpenAI models involved
  (see Vibe Coding).

## 🤖 Vibe Coding

This project was **vibe coded**: the code was generated by an AI coding agent from natural-language
prompts, then steered, debugged and refined by hand.

- **Model**: [Qwen3.8-Flash-Next](https://ollama.com/library/qwen3.8-flash-next), the open-weight
  multimodal Mixture-of-Experts from the Qwen family, run here in its local
  `qwen3.8-flash-next:125b-mlx` variant (125B total parameters, 6B active per token).
- **Runtime**: [Ollama](https://ollama.com/), serving that model locally on the same machine, so the
  inference never leaves the laptop.
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

- **Code**: 🄯 This project is **copylefted** and released under the **[GNU AGPL-3.0-or-later](LICENSE)**:
  you may use, study, share and adapt the code, as long as derivatives carry the same licence and
  make their source available — including when the visualization is served to users over a network.
  The copyleft symbol 🄯 is used in place of ©, on purpose.
- **Imagery**: Imagery is used under **Fair Use** for educational and non-commercial visualization
  purposes. All rights to the original textures belong to their respective creators and agencies
  (NASA / JPL / Caltech / SWRI / MSSS / Floppastrogeo).
   - NASA/JPL products such as PIA07782 are **public domain** (not subject to copyright); please
    keep the credit line *NASA / JPL / Caltech*.
   - The JunoCam maps of the Maps Archive are released **CC-BY** by their authors — credit
    *NASA / JPL / SwRI / MSSS / Gerald Eichstädt / John Rogers*.
   - Community texture maps on DeviantArt (Floppastrogeo, FarGetaNik, Askaniy, …) are usually
    published under **CC-BY-NC-SA**: keep the attribution, do not use them commercially, and share
    derivatives under the same licence.
   - The **geodesic tessellation and the graticule are generated in code**, so the frequency-ν view
    has no third-party imagery at all.
- **Fonts**: annotations are set in **Latin Modern Roman** (the TeX successor to Computer Modern),
  distributed under the [GUST Font License](https://www.ctan.org/tex-archive/fonts/lm/doc/fonts/lm/README)
  via [CTAN](https://www.ctan.org/pkg/lm), and loaded from
   [jsDelivr](https://cdn.jsdelivr.net/gh/geometalab/Latin-Modern-Math-font@master/fonts/otf/lmroman10-regular.otf)
  with a [cdnFonts](https://www.cdnfonts.com/latin-modern-10.font) fallback; the interface uses
   [Inter](https://rsms.me/inter/) (SIL OFL 1.1). Labels are rasterised only once
   `document.fonts` confirms the face is really loaded, so a slow CDN degrades the typography
  without silently substituting a serif.
- **Library**: [three.js](https://threejs.org/) (MIT), imported through an `importmap` from
   `unpkg`.

> Not redistributing the imagery: only the two maps needed by the visualization are kept in the
> repository, together with their provenance. See [LICENSE](LICENSE) for the code licence
> (🄯 Copyleft 2026 Laurent Perrinet).

## 📜 Provenance of the choices

The project was developed as a series of focused sessions; each decision below traces to the
question that produced it.

| Decision | What settled it |
|---|---|
| three.js, single HTML file | chosen at the outset as the lightest way to project a grid seen from the centre of a sphere |
| Cassini–Juno mix as the default view | it covers the Cassini mid-latitudes and the Juno polar cyclones in one equirectangular map |
| full-coverage maps only | a map that misses the poles leaves blank sky where the observer looks up |
| sRGB colour management | *PIA07782 looks more saturated as an image than in your rendering* |
| pointer-down ⇒ look-down | the inverted mapping felt wrong on every attempt |
| Ornstein–Uhlenbeck motion | *Exploration drift must not diverge* |
| saccade-safe anchor | all speed control must come from the pointer's position, and a saccade must be allowed to re-aim it |
| measured `\|ω\|` and path-chord heading | the arrow pointed at noise near the poles; λ̇, β̇ are not the angular velocity |
| `\|ω\|` on the shaft only, components on the plate | the vector carried too much text to read as an instrument |
| instrument bar with physical buttons | every keystroke should have a touchable twin — except that the pointer *is* the motion controller, so motion buttons park the pointer |
| Zen keeps the pointer alive | zen is about removing instruments, not interaction |
| footer ⇒ header attribution strip | the footer overlapped the instrument bar, and its links were click-through |
| `target="_blank" rel="noopener"` on external links | the attribution links were not usable from inside the HUD |
| GitHub badge in the corner | the licence requires an obvious door to the source |
| AGPL-3.0-or-later with 🄯 instead of © | the code stays copylefted, including when served over a network |
| "Triangulated geodesic" wording | the geodesic must read as an icosahedron: 30 generating edges + 12 pentagonal vertices |
| two-theme palette (green / dark blue) | green is a wireframe colour, not a colour to print over Jupiter's clouds |

A practical caveat for anyone continuing this work: the sessions all share **one working tree**, so
two agents editing `index.html` at the same time will silently revert each other's features — which
is exactly what happened a few times ("the DeviantArt map is not showing", "the grid disappeared").
One checkout per thread, or worktrees, avoids it; `git status` clean plus a feature you cannot see
in the browser is the tell.
