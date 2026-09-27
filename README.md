# Jupiter from the Inside

An immersive 3D visualization that allows you to explore the surface of Jupiter from the inside.

## 🌌 The Concept

Instead of looking at the planet from space, this project places you at the center of Jupiter. You are surrounded by a massive spherical shell representing the planetary surface, allowing you to navigate the clouds and storm systems from a central perspective.

## 🎨 Design Guidelines

### Scientific Approach

The project aims to translate complex astronomical data into an intuitive spatial experience.

- **Projection**: It utilizes equirectangular cylindrical maps wrapped onto a sphere, ensuring that the spatial relationships of Jupiter's cloud bands and the Great Red Spot are preserved from the observer's central perspective.
- **Orientation**: A virtual "SUN" marker provides a cardinal reference point, helping the user understand their orientation within the planetary shell.
- **Scaling**: The use of a wide Field of View (FOV) simulation mimics the experience of being inside a gargantuan sphere, where the horizon curves away from the observer.
- **Tessellation**: The wireframe view is a *géode par triangulation* — an icosahedron whose every edge is divided in 10 ([frequency ν = 10](https://fr.wikipedia.org/wiki/G%C3%A9ode_(g%C3%A9om%C3%A9trie)#G%C3%A9ode_par_triangulation)), yielding 2,000 near-equilateral triangular facets.
- **Coordinate system**: A scientific grid of latitude and longitude lines is drawn every 15°, with values labeled in steps of 15° (e.g. *15°N*, *30°S*, *45°E*) typeset in [Computer Modern / Latin Modern](https://www.ctan.org/tex-archive/fonts/lm/), the classic TeX typeface.

### Aesthetics: "The Digital Observatory"

The visual language of this project is inspired by futuristic planetary observatories and heads-up displays (HUDs).

- **Color Palette**: The primary interface uses **"Video Blue" (#00BFFF)** family tones, high-contrast colors that ensure legibility against the organic, warm tones of Jupiter's atmosphere. The HUD is matrix-green over the geodesic wireframe and turns deep blue whenever one of Jupiter's images is shown.
- **Atmosphere**: The movement is intentionally smooth and drifting, evoking a sense of weightlessness and the immense scale of the gas giant.
- **Minimalism**: UI elements are designed to be unobtrusive, with a dedicated "Zen Mode" to remove all digital noise, leaving only the planetary vista.

## 🕹️ Controls

### Navigation & Motion

- **Mouse Steering**: The mouse acts as a directional accelerator. Unlike traditional joysticks, the system uses a **Saccade-Safe Anchor**. The acceleration is determined by the distance between your cursor and a virtual neutral point that smoothly follows your mouse. This allows you to "jump" the cursor to a new position (saccade) to realign your control without inducing violent camera whips.
- **Kinetic Drift**: The movement is governed by an **Ornstein-Uhlenbeck Diffusion process**. This ensures that the camera naturally drifts back toward a state of rest (mean reversion)—specifically converging to the actual self-rotation speed of Jupiter—while introducing a subtle, organic volatility (simulating planetary winds).
- **Vim Motion Keys**:
    - `h`: Rotate Left
    - `l`: Rotate Right
    - `k`: Rotate Up
    - `j`: Rotate Down
- **Dynamic Zoom**:
    - `S`: Zoom In (Decrease FOV)
    - `D`: Zoom Out (Increase FOV)
    - **Trackpad/Wheel**: Pinch or scroll to zoom.
- **Fullscreen**:
    - `F`: Toggle full-screen mode.

### Views & Overlays

- **View Cycle (`P`)**: Cycles through the three shells:
    1. **Géode** — the icosahedral triangulation wireframe (ν = 10).
    2. **Cassini** — NASA/JPL enhanced-color global view (`PIA07782.jpg`).
    3. **Cassini–Juno mix** — the equirectangular texture map by floppastrogeo.
- **Navigation Data (`V`)**: Toggles the scientific overlay:
    - **Coordinate grid**: latitude/longitude lines every 15°, labeled in Computer Modern.
    - **Speed Arrow**: A large vector indicator showing the current rotational velocity and direction, with a live `ω` readout in rad/s.
    - **SUN Marker**: A fixed reference point on the horizon to help maintain orientation.
- **Full Zen Mode (`Z`)**: Hides all UI elements, including coordinates, labels, and the cursor, leaving only the planetary vista for a pure, meditative experience.

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

## 🛰️ Data Sources & Licensing

The planetary surface textures used in this project are sourced from:

- **[Juno Mission Media Gallery](https://www.missionjuno.swri.edu/mediagallery)** (provided by [SWRI](https://www.swri.org/)).
- **[PIA07782 — Jupiter, Global View, Enhanced Color](https://photojournal.jpl.nasa.gov/catalog/PIA07782)** (Cassini imaging, NASA/JPL; also on [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:PIA07782major.jpg)).
- **[floppastrogeo](https://www.deviantart.com/floppastrogeo)** — [*Jupiter texture map of Cassini and Juno mixed*](https://www.deviantart.com/floppastrogeo/gallery) (mixed Cassini/Juno equirectangular map via DeviantArt).

### Licensing

- **Code**: This project is released under the **[MIT License](LICENSE)**.
- **Imagery**: Imagery is used under **Fair Use** for educational and non-commercial visualization purposes. All rights to the original textures belong to their respective creators and agencies (NASA / SWRI / floppastrogeo).
