# Jupiter Interior Vista

An immersive 3D visualization that allows you to explore the surface of Jupiter from the inside.

## 🌌 The Concept
Instead of looking at the planet from space, this project places you at the center of Jupiter. You are surrounded by a massive spherical shell representing the planetary surface in visible light, allowing you to navigate the clouds and storm systems from a central perspective.

## 🕹️ Controls

- **Navigation**: Move your mouse away from the center of the screen to accelerate rotation. The further the cursor is from the center, the faster you rotate.
- **Drift**: The movement is designed to be smooth and drifting, encouraging a slow, meditative exploration of the interior.
- **Zoom**: Use your trackpad (pinch) or mouse wheel to zoom in and out (adjusts Field of View).
- **Navigation Data**: Press the **'S'** key to toggle the status overlay:
    - **Speed Arrow**: A visual indicator showing the direction and magnitude of your current rotational velocity.
    - **SUN Marker**: A reference point on the horizon labeled "SUN" to help you maintain orientation.

## 🚀 Local Setup

To run this project locally:

1. Clone the repository.
2. Start a local web server in the root directory:
   ```bash
   python3 -m http.server 8000
   ```
3. Open your browser and navigate to `http://localhost:8000`.

## 🛠️ Technology Stack
- **Three.js**: Used for the 3D projection, spherical geometry, and camera rotations.
- **HTML5/CSS3**: For the UI overlays and status indicators.
