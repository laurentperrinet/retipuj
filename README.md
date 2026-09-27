# Jupiter Interior Vista

An immersive 3D visualization that allows you to explore the surface of Jupiter from the inside.

## 🌌 The Concept
Instead of looking at the planet from space, this project places you at the center of Jupiter. You are surrounded by a massive spherical shell representing the planetary surface in visible light, allowing you to navigate the clouds and storm systems from a central perspective.

## 🕹️ Controls

- **Navigation**: Move your mouse away from the center of the screen to accelerate rotation. The further the cursor is from the center, the faster you rotate.
- **View Mode**: Press **'P'** to toggle between the structural wireframe and the high-resolution planetary surface texture.
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

## 🛰️ Data Source
The planetary surface texture used in this project is sourced from the **Juno Mission Media Gallery** (provided by SWRI).

*Fair Use Note: This project uses this imagery for educational and non-commercial visualization purposes to demonstrate a perspective-shifted planetary projection.*
