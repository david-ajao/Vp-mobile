# Phosphene 1600

**Adaptive visual-encoding simulator using 1,600 virtual spatial territories.**

This repository contains a browser-based computational prototype for a visual-prosthesis research concept. It does **not** simulate cortical physiology, does **not** contain a physical electrode array, and does **not** provide neural-stimulation hardware instructions.

## What the simulator does

The browser simulator implements this computational pipeline:

```text
Camera / demo scene
        ↓
Luminance image
        ↓
Importance analysis
(detail + saliency + context)
        ↓
Adaptive spatial partitioning
        ↓
1,600 virtual encoding territories
        ↓
Optional render-time positional steering
        ↓
Simulated phosphene display
```

### 1,600 virtual elements

The 1,600 elements are **spatial sampling/encoding territories**. They are not physical electrodes and are not TMS coils.

The adaptive allocator uses a quadtree-style partition. Higher-importance regions can be divided into smaller territories, giving them finer spatial sampling, while lower-importance regions retain larger territories so the entire scene remains represented.

The simulator also includes diagnostics for element count, coverage, overlap, brightness, context allocation, and steering constraints.

### Importance signals

The desktop/reference architecture contains four conceptual signals:

- semantic importance
- local detail
- saliency/contrast
- broader context/structure

The **browser build in this repository intentionally does not run YOLO or another neural network**. Its importance engine uses the non-semantic detail, saliency, and context signals and renormalizes their weights. The Python reference prototype contains the separate YOLO-based semantic path.

### Positional steering

The steering experiment is a **render-time positional transformation** of the virtual encoding centers. It does not change the underlying territories or their brightness and does not represent demonstrated physiological current steering in a human cortex.

Any claim about a large increase in effective resolution from steering remains a research hypothesis rather than a demonstrated result of this simulator.

## Repository structure

```text
visual-prosthesis/
├── src/
│   ├── components/simulator/     # Browser UI
│   ├── lib/prosthesis/           # Capture, importance, territories, steering, rendering
│   ├── lib/utils.ts
│   ├── main.tsx
│   └── styles.css
├── public/                       # App assets and PWA files
├── screenshots/                  # Example simulator outputs
├── research/python/              # Reference/experimental Python prototypes
├── index.html
├── vite.config.ts
├── tsconfig.json
├── package.json
├── .gitignore
└── README.md
```

## Running locally

Requirements: a recent Node.js release and a browser with camera support.

```bash
npm install
npm run dev
```

For a production build:

```bash
npm run build
npm run preview
```

The browser will ask for camera permission when the camera mode is used. If camera access is unavailable, the simulator can fall back to its demo scene.

## Python reference prototypes

`research/python/` contains the Python files exported from the original generated workspace. They are kept as **reference/experimental prototypes**, not as the canonical browser implementation.

One important reproducibility note: those Python files currently reference `yolo11n-seg.pt`, while the current local development setup used for the newer prototype has used `yolo26n-seg.pt`. The model reference should therefore be treated as a configuration difference, not as proof that both prototypes use the same model.

## Research limitations

This project is a computational visualization/encoding prototype. In particular:

- It does not establish that adaptive allocation improves human visual perception.
- Simulated phosphene circles are not a physiological model of cortical stimulation.
- Positional steering in this code is not demonstrated electrical current steering in cortex.
- The browser build does not perform semantic object detection with YOLO.
- The project does not claim 20/20 vision or naturalistic vision.
- Any future TMS or implant validation would require appropriate laboratory, clinical, and safety oversight.

## GitHub note

The original exported workspace contained platform-specific Grok/App Builder, Vercel, TanStack, authentication, and database infrastructure. This GitHub version removes that generated workspace layer and keeps the simulator itself plus the research/reference material.

No software license is included yet. Add a license only after deciding what permissions you want other people to have for the code.
