# ViewSpot: Real-time Object, Person and Path Detection

ViewSpot is a browser app that uses your webcam to detect objects and people in real time. It draws boxes and labels on the live video, and it can record footage and take screenshots. The app runs entirely in the browser, so no video is sent to a server.

**Live demo:** https://viewspot.vercel.app/

---

## Features

- **Real-time detection:** TensorFlow.js with the COCO-SSD model finds and labels objects and people in each video frame.
- **Recording:** Start and stop recording manually, or turn on auto-record. Footage is saved to your device.
- **Screenshots:** Capture the current frame with its detections.
- **Mirror mode:** Flip the camera view horizontally.
- **Light and dark mode:** Switch the interface theme from the sidebar.
- **Sidebar controls:** All settings are in one place.
- **Path detection (planned):** Track each detected object across frames and show its movement path.

## Tech stack

| Area | Tools |
|---|---|
| Framework | Next.js 14 (App Router), React 18, TypeScript |
| Detection | TensorFlow.js, `@tensorflow-models/coco-ssd` |
| Video | `react-webcam`, Media Capture and Streams API |
| UI | Tailwind CSS, shadcn-style components built on Radix UI, Lucide icons, Font Awesome |
| Theming | `next-themes` |
| Notifications | `sonner` |

## Project structure

```
.
├── app/                 # Next.js pages and layout
├── components/          # UI components (sidebar, camera view, controls)
├── lib/                 # Helpers, including the Tailwind class merge utility
├── public/              # Static files
├── components.json      # shadcn/ui configuration
├── next.config.mjs      # Webpack aliases and security headers
├── tailwind.config.ts   # Tailwind theme and animations
└── package.json
```

## Getting started

### Requirements

- Node.js 18.18 or newer (Node 20 recommended)
- npm
- A webcam, and a browser that supports camera access (Chrome, Edge, or Firefox)

### Install

```bash
git clone <your-repo-url>
cd viewspot
npm install
```

### Run in development

```bash
npm run dev
```

Open http://localhost:3000 in your browser and allow camera access when asked.

### Run a production build

```bash
npm run build
npm start
```

`npm start` only works after `npm run build`.

### Lint

```bash
npm run lint
```

## How it works

1. The browser asks for camera access. `react-webcam` shows the live video.
2. COCO-SSD loads in the browser. Its model weights are downloaded at runtime, so no model file needs to be in the repo.
3. On each frame, the model returns objects with a label, a confidence score, and a bounding box.
4. The app draws the boxes over the video. Recording and screenshots use the same canvas, so the saved footage includes the detections.

## Configuration notes

- **Security headers:** `next.config.mjs` sets Cross-Origin-Opener-Policy and Cross-Origin-Embedder-Policy headers. These are needed for some of the TensorFlow.js and WebAssembly features. Don't remove them without testing.
- **ONNX Runtime alias:** `next.config.mjs` maps `onnxruntime-web$` (exact match only) to its bundled file. The exact match matters because `@huggingface/transformers` imports `onnxruntime-web/webgpu`, which a non-exact alias would break.
- **Large model files:** Files such as `*.onnx`, `*.tflite`, and `*.pt` are in `.gitignore`. Keep model weights out of the repo and load them from a CDN.

## Troubleshooting

| Problem | Fix |
|---|---|
| Camera is black or blocked | Allow camera access for localhost in the browser settings. Close other apps that use the webcam. |
| `npm start` says there is no production build | Run `npm run build` first, or use `npm run dev`. |
| Detection is slow | Use a smaller browser window, or close other tabs that use the GPU. |
| Build fails on `onnxruntime-web` | Check that `next.config.mjs` uses the `onnxruntime-web$` alias. |

## Use cases

- **Home security:** Use a spare laptop or phone as a camera that flags visitors.
- **Office monitoring:** Watch a room for people during off-hours.
- **Pet or baby monitoring:** Check on a pet or baby from another room.
- **Accessibility:** Help people with low vision identify objects around them.

## Roadmap

- Path detection: track objects across frames and draw their movement paths
- Alerts when a chosen object or person appears
- Adjustable detection threshold in the sidebar

## License

Add your license here, for example MIT.
