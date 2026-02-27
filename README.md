# 🏭 Limitless CNC — AI-Powered STEP Analyzer

Interactive 3D viewer for CNC machining analysis. Upload any STEP file and get instant AI manufacturing insights.

**[🔗 Live Demo](https://yonatanbitton.github.io/limitless-cnc-viewer/)**

![Viewer Screenshot](screenshot.png)

## Features

- 🧠 **AI Manufacturing Analysis** — Feature recognition, material recommendations, toolpath strategy, cycle time savings
- ⚙ **CNC Machining Simulation** — Animated SVG HUD simulation showing raw stock material being carved by a toolhead along a generated toolpath.
- 📏 **3D Dimension Annotations** — Toggle engineering callouts in 3D space
- 🎬 **Cinematic Camera Presets** — Overview, Top-Down, Profile, Close-Up with smooth transitions
- 📂 **Drag-and-Drop Upload** — Load your own .stp/.step files
- 🌐 **HDRI Reflections** — Photorealistic metallic rendering

## 🤖 How Gemini Parses & Understands STP Files

To prove that Gemini knows how to process complex 3D CAD data, here is exactly how it "reads" an STP (Standard for the Exchange of Product model data) file:

### 1. WASM Geometry Extraction
Gemini leverages `occt-import-js` (Open CASCADE Technology compiled to WebAssembly) to read the raw mathematical definitions within the `.stp` file. Instead of just looking at the surface pixels, it traverses the underlying **B-Rep (Boundary Representation) topology**.

### 2. Topological Analysis
Gemini reads the file at a structural level to understand the part's DNA:
- **Solids:** It identifies how many distinct solid bodies are present.
- **Faces & Edges:** It counts B-Rep faces (e.g., exactly 23 faces on the demo plug) to measure part complexity.
- **Bounding Boxes:** It calculates the exact XYZ dimensional limits to determine the required raw stock block size.

### 3. Heuristic Feature Recognition
Once the topology is parsed, Gemini's spatial logic takes over. By analyzing the bounding boxes and face groupings, it identifies manufacturing features:
- It detects **cylindrical pin cavities** by analyzing internal curved faces.
- It calculates exact **depths** and **diameters** (e.g., Ø0.70mm, depth 0.30mm).
- It categorizes shapes like "octagonal bases" and "pin arrays" to inform the machining strategy.

### 4. Intelligent CAM Translation
Finally, Gemini translates these physical realities into a CNC strategy:
- Because it identified deep cavities, it recommends **helical interpolation**.
- Because it measured the volume of material to remove, it calculates **adaptive clearing** parameters and estimates **cycle time** optimizations.

This allows Gemini to look at an STP file not as an image, but as a structured list of manufacturing operations waiting to happen.

## Tech Stack

| Component | Purpose |
|-----------|---------|
| [Three.js](https://threejs.org/) r160 | 3D rendering engine |
| [occt-import-js](https://github.com/niclaslindstedt/occt-import-js) | STEP → mesh via WebAssembly |
| CSS2DRenderer | 3D dimension labels |
| RoomEnvironment | Procedural HDRI |

## About

Built for [Limitless CNC](https://limitlesscnc.ai/) — an AI CAM Agent that integrates with NX-CAM to automate CNC programming.
