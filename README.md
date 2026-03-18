# EigenFlow

> **Real-time motion tracking with mathematical visual overlays and generative sound — all in a single browser tab.**

EigenFlow is a browser-based creative tool that uses your camera (or a video file) to detect and track motion blobs in real time, mapping their position, velocity, and size onto a live sound engine and a set of mathematical visual overlays. No install, no dependencies, no server — just open and move.

---

## ✨ Features

### 🎥 Motion Detection & Tracking
- Real-time **frame-differencing** blob detection directly on the camera feed
- Adjustable **sensitivity**, minimum and maximum blob size
- **Blob identity tracking** across frames — each detected region maintains a consistent ID and movement history
- **Overlap detection** — highlights when two blobs intersect
- Works with **front camera**, **rear camera**, or **uploaded video files**
- **Canvas recording** — capture your session directly to a `.webm` video file

### ⬡ Mathematical Overlays
Seven toggleable real-time visual overlays, each computed from live blob positions:

| Overlay | What it renders |
|---------|----------------|
| **Convex Hull** | Smallest enclosing polygon across all blobs, with centroid lines |
| **Voronoi** | Nearest-blob territory map — colour-fills the frame by ownership |
| **Force Field** | Gravitational attraction arrows pointing toward blob centres |
| **Lissajous** | Parametric motion trail tracing each blob's path over time |
| **Delaunay** | Optimal triangulation connecting blob centre points |
| **Fourier Rings** | Expanding ripple rings emitted on direction change |
| **Reaction-Diffusion** | Gray-Scott chemical simulation seeded by blob positions — generates organic Turing-pattern textures |

### 🔊 Sound Engine
- Each tracked blob generates a **live synthesiser voice** — position maps to pitch (X-axis) and filter brightness (Y-axis)
- Notes are **scale-quantised** — pitch always lands on a musical scale degree
- Six built-in scales: Pentatonic Minor, Pentatonic Major, Whole Tone, Natural Minor, Major, Chromatic
- **Drone pad** — layered detuned oscillators provide a harmonic foundation tuned to the current scale root
- **Convolution reverb** with adjustable wet/dry mix
- **Waveshaper distortion** with velocity-reactive drive
- **Pitch bars** visualiser in the video overlay
- **Note chips** — active scale notes light up in real time as blobs trigger them
- Controls: drone volume, pitch range (octaves), volume sensitivity, attack/release, filter base frequency, reverb mix, distortion amount

---

## 🚀 Getting Started

No build step, no install, no API key required.

```bash
# Clone the repo
git clone https://github.com/your-username/eigenflow.git

# Open in browser
open index.html
```

Or just **double-click `index.html`** — it opens directly in any modern browser.

**Camera permission** will be requested on first load. No audio input is used — only the camera.

---

## 🛠 How It Works

### Motion Detection Pipeline
Each frame is drawn to an offscreen canvas. A **frame-difference mask** is computed by comparing luminance values pixel-by-pixel against the previous (temporally smoothed) frame. The mask is then **dilated** to connect nearby regions, and a **BFS flood-fill** labels connected components into blobs with bounding boxes and centroid coordinates.

### Sound Mapping
Each blob's normalised X position is quantised to the nearest scale degree and converted to Hz using the equal-temperament formula `440 × 2^((midi−69)/12)`. Y position controls filter cutoff (top of frame = open/bright, bottom = dark). Blob velocity (derived from the trail history) drives volume and distortion amount. Up to 6 simultaneous voices share a common reverb/distortion chain.

### Overlay Rendering
All overlays are drawn on a transparent `<canvas>` layered over the live video feed. Each is computed from scratch each frame using the current blob array:
- **Convex Hull** — Andrew's monotone chain algorithm
- **Voronoi** — brute-force nearest-centroid per downsampled pixel grid
- **Delaunay** — Bowyer-Watson incremental insertion
- **Reaction-Diffusion** — Gray-Scott model (Du=0.16, Dv=0.08, F=0.060, k=0.062)
- **Fourier Rings** — ripple emitted when blob direction change exceeds threshold

---

## 📁 File Structure

```
eigenflow/
├── index.html    # Entire application — self-contained single file
├── README.md     # This file
└── LICENSE       # MIT License
```

---

## 🎛 Controls Reference

| Control | Description |
|---------|-------------|
| Camera / Video File | Toggle between live camera and uploaded video |
| Front / Back | Switch between front and rear camera |
| Record / Save | Capture canvas output to `.webm` |
| Sensitivity | Motion detection threshold (lower = more sensitive) |
| Min / Max blob (px²) | Size filter — ignore blobs outside this area range |
| Playback speed | Video file playback rate multiplier |
| 🔊 Sound toggle | Enable/disable the synthesis engine |
| Scale selector | Musical scale for pitch quantisation |
| Drone volume | Level of the background pad layer |
| Pitch range | Number of octaves mapped across the frame width |
| Vol sensitivity | How strongly blob velocity drives note volume |
| Attack / Release | Voice envelope time constant |
| Filter (Y base) | Filter cutoff frequency at the bottom of the frame |
| Reverb mix | Wet/dry balance for the convolution reverb |
| Distortion | Waveshaper drive amount |
| Overlay toggles | Enable/disable each of the 7 visual overlays |

---

## 🌐 Browser Compatibility

| Browser | Status |
|---------|--------|
| Chrome / Edge | ✅ Fully supported |
| Safari (iOS / macOS) | ✅ Supported |
| Firefox | ✅ Supported |
| Samsung Internet | ✅ Supported |

Requires: `getUserMedia`, `AudioContext`, `Canvas 2D API` — all available in any modern browser released after 2018.

---

## ⚠️ Notes

- Sound requires a user gesture (tap the 🔊 button) due to browser autoplay policy
- Performance depends on device — on older phones, reduce blob count using the min/max size sliders
- The Reaction-Diffusion overlay is the most computationally intensive — disable it on lower-end devices if frame rate drops

---

## Credits

Created by **Abhishek Jawahar**

Built entirely with:
- [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API)
- [Canvas 2D API](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)
- [MediaDevices API](https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices)

Zero external dependencies.

---

## 📄 License

MIT License © 2026 Abhishek Jawahar — see [LICENSE](./LICENSE) for full text.
