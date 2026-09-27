# How is it to look Jupiter from Inside?

An immersive 3D visualization that allows you to explore the surface of Jupiter from the inside.

## 🌌 The Concept
Instead of looking at the planet from space, this project places you at the center of Jupiter. You are surrounded by a massive spherical shell representing the planetary surface in visible light, allowing you to navigate the clouds and storm systems from a central perspective. This project is mostly for fun and educational purposes, providing a unique *point of view* to experience the scale and beauty of Jupiter's atmosphere.

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

- **Navigation**: Move your mouse away from the center of the screen to accelerate rotation. The further the cursor is from the center, the faster you rotate.
- **View Mode**: Press **'P'** to cycle through:
    - **Mixed Map** (Default): Cassini/Juno hybrid texture.
    - **Juno Map**: Official Juno mission texture.
    - **Structural Grid**: High-contrast wireframe.
- **Coordinate Grid**: Press **'C'** to toggle the latitude and longitude grid overlay.
- **Drift**: The movement is designed to be smooth and drifting, encouraging a slow, meditative exploration of the interior.
- **Zoom**: Use your trackpad (pinch) or mouse wheel to zoom in and out (adjusts Field of View).
- **Navigation Data**: Press the **'V'** key to toggle the status overlay:
    - **Speed Arrow**: A visual indicator showing the direction and magnitude of your current rotational velocity.
    - **SUN Marker**: A reference point on the horizon labeled "SUN" to help you maintain orientation.
- **Zen Mode**: Press **'H'** to hide all UI elements for an unobstructed view.
- **Manual Motion**: Use **'J', 'K', 'L', 'M'** for precise rotational adjustments.

## 🚀 Local Setup

To run this project locally (required for texture loading due to browser security):

1. Clone the repository.
2. Start a local web server in the root directory:
   ```bash
   python3 -m http.server 8000
   ```
3. Open your browser and navigate to `http://localhost:8000`.

## 🛠️ Technology Stack
- **Three.js**: Used for the 3D projection, spherical geometry, and camera rotations.
- **HTML5/CSS3**: For the UI overlays and status indicators.

## 🛰️ Data Sources & Licensing
The planetary surface textures used in this project are sourced from:
- **Juno Mission Media Gallery** (provided by SWRI).
- **Floppastrogeo** (Cassini/Juno mixed map via DeviantArt).

### Licensing
- **Code**: This project is released under the MIT License.
- **Imagery**: Imagery is used under **Fair Use** for educational and non-commercial visualization purposes. All rights to the original textures belong to their respective creators and agencies (NASA/SWRI/Floppastrogeo).
