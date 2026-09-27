# How is it to look Jupiter from Inside?

An immersive 3D visualization that allows you to explore the surface of Jupiter from the inside.

## 🌌 The Concept
Instead of looking at the planet from space, this project places you at the center of Jupiter. You are surrounded by a massive spherical shell representing the planetary surface, allowing you to navigate the clouds and storm systems from a central perspective.

## 🎨 Design Guidelines

### Scientific Approach
The project aims to translate complex astronomical data into an intuitive spatial experience.
- **Projection**: It utilizes equirectangular cylindrical maps wrapped onto a sphere, ensuring that the spatial relationships of Jupiter's cloud bands and the Great Red Spot are preserved from the observer's central perspective.
- **Orientation**: A virtual "SUN" marker provides a cardinal reference point, helping the user understand their orientation within the planetary shell.
- **Scaling**: The use of a wide Field of View (FOV) simulation mimics the experience of being inside a gargantuan sphere, where the horizon curves away from the observer.

### Aesthetics: "The Digital Observatory"
The visual language of this project is inspired by futuristic planetary observatories and heads-up displays (HUDs). 
- **Color Palette**: The primary interface uses **"Video Blue" (#00BFFF)**, a high-contrast neon blue that ensures legibility against the organic, warm tones of Jupiter's atmosphere.
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
    - `F`: Toggle Full-screen mode.

### Utility & Experience
- **Navigation Data (`V`)**: Toggles the status overlay:
    - **Speed Arrow**: A vector indicator showing the current rotational velocity and direction.
    - **SUN Marker**: A fixed reference point on the horizon to help maintain orientation.
- **Full Zen Mode (`Z`)**: Hides all UI elements, including coordinates, labels, and the cursor, leaving only the celestial grid for a pure, meditative experience.

## 🚀 Local Setup

To run this project locally:

1. Clone the repository.
2. Start a local web server in the root directory:
   ```bash
   python3 -m http.server 8000
   ```
3. Open your browser and navigate to `http://localhost:8000`.

## 🛠️ Technology Stack
- **Three.js**: Powering the 3D spherical projection and camera rotations.
- **HTML5/CSS3**: For the minimalist UI and status overlays.
- **Stochastic Calculus**: Implementing OU-diffusion for organic motion.

## 🛰️ Data Sources & Licensing
The planetary surface textures used in this project are sourced from:
- **Juno Mission Media Gallery** (provided by SWRI).
- **Floppastrogeo** (Cassini/Juno mixed map via DeviantArt).

### Licensing
- **Code**: This project is released under the GNU GPL v3.0 License, allowing for free use, modification, and distribution under the same license.
- **Imagery**: Imagery is used under **Fair Use** for educational and non-commercial visualization purposes. All rights to the original textures belong to their respective creators and agencies (NASA/SWRI/Floppastrogeo).
