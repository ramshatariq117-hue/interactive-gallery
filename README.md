# Rams Ha Art Studio — Interactive Gallery

A single-page 3D gallery: a dark hallway with 5 themed rooms (Lobby 1–5), built with [Three.js](https://threejs.org/). Visitors type their name, walk around with WASD + mouse, and click paintings to reveal a "vintage note" — a couple of which reveal a free-print message instead.

## 1. How to run it locally
You can't just double-click `index.html` in some browsers because of security rules around loading textures. Easiest fix:
1. Install [VS Code](https://code.visualstudio.com/) and the **Live Server** extension, OR
2. Open a terminal in this folder and run `python3 -m http.server`, then visit `http://localhost:8000`

## 2. How to put it on GitHub (free hosting via GitHub Pages)
1. Create a new repo on GitHub (e.g. `rams-gallery`).
2. Upload `index.html`, the `images` folder, and this `README.md`.
3. Go to **Settings → Pages**, set branch to `main`, folder to `/root`, save.
4. GitHub gives you a live link like `https://yourname.github.io/rams-gallery/` — that's the link your QR code should point to.
5. Generate a QR code for that link at a free tool like [qr-code-generator.com](https://www.qr-code-generator.com/) and print it on your digital prints.

## 3. How it's structured (plain English)
- **The hallway** is one long dark corridor. Rooms are boxes built off to the left or right at fixed points along it.
- **Each room** is defined once near the bottom of `index.html`, inside the `rooms` array — one object per lobby, with its wall color, light color/behavior, and a list of paintings.
- **Paintings** are just flat rectangles with a placeholder texture drawn by code (so nothing breaks if you haven't added real images yet). Each one has a `story` line that appears when clicked.
- **"Free print" paintings** are marked `winner:true` — when clicked they show the celebratory message instead of the normal note. Change which paintings are winners any time.
- **Flicker/mood**: each room has a `flicker` mode (`flicker`, `ember`, `lamp`, `strobe`, or `pulse` for Lobby 1's glowing door) that jitters its light every frame for that "unstable bulb" feel.
- **Sound** is generated in-browser (a low drone + filtered noise + occasional random blips) — no audio files needed to get started, so nothing to download or license. You can absolutely swap this for real ambient tracks later (see below).

## 4. Swapping in your real paintings
Find the `rooms` array in `index.html`. Each painting looks like this:
```js
{x:-1.8, y:1.6, z:3.9, ry:0, label:'A-F 01', story:'Your description here', hue:100, img:'images/painting1.jpg'}
```
- Drop your image into the `images` folder.
- Add `img:'images/painting1.jpg'` (matching the filename) to that painting's entry.
- If the file is missing or misnamed, it just quietly falls back to the placeholder — nothing breaks.
- `x`, `y`, `z`, `ry` control position/rotation on the wall if you want to rearrange them.

## 5. Adding real audio (optional, next step)
Right now all sound is generated with code (Web Audio API), so there's nothing to license or host. When you're ready for real ambience (fireplace crackle, wind, distant footsteps):
1. Add short `.mp3`/`.ogg` files to an `audio` folder.
2. In `index.html`, inside `startAudio()`, replace the noise buffer with:
   ```js
   const audioEl = new Audio('audio/fireplace.mp3');
   audioEl.loop = true; audioEl.volume = 0.3; audioEl.play();
   ```
3. Do this per room if you want each lobby to sound different — happy to help wire that up once you're at that stage.

## 6. What's simplified for this first version (safe to extend later)
- Movement stays inside the whole level's outer boundary, but doesn't collide with individual room walls yet — you can technically clip through a wall corner. Fine for a v1; real wall collision can be added later.
- Lobby 1's "3–4 alphabetical sub-rooms" are represented as labeled zones within one room rather than fully separate rooms, to keep the file manageable — easy to split into fully separate rooms later if you want deeper navigation.
- Only 2–5 placeholder paintings per room — duplicate any painting's code block to add more.

## 7. Quick customization cheat sheet
| Want to change... | Edit... |
|---|---|
| Room wall/light color | `wallColor`, `lightColor` in that room's config |
| Neon sign text/color | `tag`, `neon`, `neonHex` |
| A painting's story text | `story` inside that painting's object |
| Which paintings are "winners" | add/remove `winner:true` |
| Movement speed | `const speed = 3.2*dt;` in `updateMovement()` |
| Welcome message wording | inside `enterBtn.onclick` |

Enjoy — and don't stare too long at Lobby 5.
