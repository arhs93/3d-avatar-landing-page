# 3D Avatar Landing Page

An interactive personal landing page with a 3D avatar that follows your mouse, breathes on idle, and dances on click. Built with plain HTML and Three.js — no frameworks, no npm, no build tools.

**Live demo:** [meet-aris.me](https://meet-aris.me)

---

## What's Inside

```
landing-page/
├── index.html    ← Homepage — avatar, speech bubble, dance on click
├── about.html    ← About page — cards left/right, zoomed upper body
├── idle.glb      ← Avatar + breathing idle animation (Mixamo)
└── dance.glb     ← Avatar + hip hop dance animation (Mixamo)
```

---

## Stack

- **Three.js** via CDN — 3D rendering, bone rotation, animation
- **Avaturn** — custom 3D avatar that looks like you
- **Mixamo** — free animations (idle + dance), exported as GLB
- **Cloudways (PHP app)** — deployed as a static site on an existing server

---

## How It Works

**Head tracking:**
Mouse position is normalized to `-1 → 1`. Every frame the head and neck bones lerp toward the target angle:
```js
smoothX += (mouseX - smoothX) * 0.08;
headBone.rotation.y = smoothX * 0.45;
headBone.rotation.x = -smoothY * 0.3;
```

**Auto-sizing:**
The GLB is scaled to a standard 1.8 unit height regardless of source scale:
```js
const scale = 1.8 / height;
model.scale.setScalar(scale);
```

**Dance interaction:**
Click loads the dance model, plays it once with `LoopOnce`, then swaps back to idle on finish.

---

## Setup

### 1. Get your avatar
- Go to [avaturn.me](https://avaturn.me), create your avatar, download `.glb`


### 2. Add animations
- Upload your `.glb` to [mixamo.com](https://mixamo.com)
- Download **Breathing Idle** and **Hip Hop Dancing** as FBX (with skin)
- Convert FBX → GLB using [Aspose](https://products.aspose.app/3d/conversion/fbx-to-glb) or Blender
- Rename: `idle.glb` and `dance.glb`

### 3. Run locally
```bash
python3 -m http.server 8000
# open http://localhost:8000
```
> A local server is required — browsers block loading local `.glb` files directly.

### 4. Deploy on Cloudways
I had an existing Cloudways server, so I created a new **PHP application** on it and deployed the files via SFTP (FileZilla):

1. In Cloudways dashboard → your server → **Add Application** → choose **PHP**
2. Connect via FileZilla using your server's **Master Credentials** (SFTP, port 22)
3. Navigate to `/applications/{app-folder}/public_html/`
4. Upload all files: `index.html`, `about.html`, `idle.glb`, `dance.glb`
5. Add your domain under **Domain Management** → point DNS A record to your server IP
6. Install SSL under **SSL Certificate**
7. Purge **Varnish cache** after every upload

---

## Customize

| What | Where |
|---|---|
| Speech bubble text | `index.html` → `.bubble` div |
| About page content | `about.html` → `.card` divs |
| Avatar position | `autoFit()` → `model.position.x` |
| Camera zoom | `camera.position.set(0, 1.0, 3.5)` — lower z = closer |
| Head rotation amount | `smoothX * 0.45` — lower = subtler |
| Background color | CSS `body` → `radial-gradient` |

---

## License

MIT — free to use and modify. Credit appreciated.

Built by [Aris Kleidas](https://meet-aris.me)
