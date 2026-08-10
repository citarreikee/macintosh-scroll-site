# Recreate this website from a GLB model

You are a senior creative frontend engineer. You are starting a new Codex conversation with only one user-provided file available:

`macintosh_classic_1991.glb`

Build a faithful, production-quality recreation of this scroll-driven experience. Do not ask the user for more design direction unless the GLB cannot be loaded. Inspect the model first, infer its coordinate system, front direction, bounding box, material/texture setup, and the visible screen area from the baked texture. The model is a Macintosh Classic (1991) with a black-and-white MacPaint-like image on its display.

## Required experience

1. Use the actual GLB as the only computer model. Do not replace it with CSS boxes, primitives, a placeholder, an external model, or a screenshot of the computer.
2. The initial viewport must be extremely minimal: a solid warm gray background and only the 3D Macintosh. No title, nav, hint, progress indicator, cards, or decorative text.
3. The model starts very small (roughly 15% of viewport height) and exactly centered horizontally and vertically.
4. The user scrolls through a tall page while the rendering canvas remains fixed to the viewport. The scroll container exists only to provide animation progress; the canvas must remain `position: fixed`, `inset: 0`, `width: 100vw`, `height: 100dvh`.
5. Drive one continuous GSAP ScrollTrigger timeline. Rotation, translation and zoom must overlap; never make the animation feel like “rotate, stop, then zoom”. Use `scrub` and a smooth easing curve.
6. The Macintosh should rotate a full 360 degrees and finish facing the viewer. During the final zoom, target the screen area rather than the mesh bounding-box center. This model has a screen located above the geometric center, so apply a downward final translation to the model group. Tune the final vertical offset visually until the display—not the whole casing—is centered in the viewport.
7. At the end, the physical screen should fill most of the viewport while its bezel is still readable for a brief moment. Then cross-fade to a real HTML page that looks like the content printed on the model’s display.
8. The takeover page must be a convincing 1-bit classic MacPaint interface: white/gray pixel texture, black menu bar with “File”, “Edit”, “Goodies”, “Font”, “FontSize”, “Style”, an `untitled` window, close/zoom boxes, two-column tool palette, line-weight panel, large black-and-white pixel illustration, and a patterned swatch palette. Use semantic HTML/CSS and inline SVG for the illustration; do not use another screenshot as the final UI.
9. The UI layer starts clipped to the screen rectangle, then expands to `clip-path: inset(0)` while the WebGL canvas fades out. Keep the transition subtle and synchronized.
10. Support responsive viewports, cap pixel ratio around 1.5–1.8, preserve the fixed canvas, and avoid React re-renders on every scroll frame. Respect `prefers-reduced-motion` with a simplified static view.

## Technical constraints

- A static HTML page is acceptable; CDN imports for Three.js, GLTFLoader and GSAP are acceptable.
- Prefer Three.js + GLTFLoader + GSAP ScrollTrigger. Keep the GLB path relative to the HTML file.
- Ensure the file works when served from a simple static server (for example `python3 -m http.server`).
- Add an explicit cache-busting query when loading the model if needed during development.
- Never hide the model because the GLB is still loading; show a small, centered loading fallback only if unavoidable, and remove it once loaded.
- Verify at three scroll positions: top (tiny centered model), mid-rotation (model remains visible while canvas stays fixed), and final (screen-targeted zoom with HTML MacPaint takeover).

## Deliverables

- `index.html`
- `macintosh_classic_1991.glb`
- `README.md` with screenshots, live/demo URL, setup commands, features, and directory tree
- `docs/screenshot.png`

The implementation should be self-contained, visually polished, and explain in comments that the model’s screen is not a separate GLTF node, so its final alignment is calibrated through a screen-center offset on the parent group.
