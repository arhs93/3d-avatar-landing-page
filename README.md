# 3D Avatar Landing Page

An interactive personal landing page featuring a 3D avatar that follows your mouse, breathes in idle, and dances on click. Built with plain HTML and Three.js — no frameworks, no build tools, no npm.

**Live demo:** [meet-aris.me](https://meet-aris.me)

---

## Features

- 3D avatar with head that follows your mouse (left, right, up, down)
- Breathing idle animation on loop
- Click / tap anywhere → avatar hip hop dances → returns to idle
- Speech bubble above head (disappears on click, returns after dance)
- About page with info cards and zoomed upper body view
- Mobile responsive — touch tracking + scrollable about page
- Zero dependencies — Three.js loaded via CDN

---

## Tech Stack

| Layer | Tool |
|---|---|
| 3D Engine | [Three.js](https://threejs.org/) via CDN |
| Avatar | [Ready Player Me](https://readyplayer.me/) |
| Animations | [Mixamo](https://mixamo.com/) |
| Hosting | Any static host (Cloudways, Netlify, etc.) |

---

## File Structure

```
landing-page/
├── index.html       ← Homepage with avatar + speech bubble
├── about.html       ← About page with info cards
├── idle.glb         ← Avatar with breathing animation (from Mixamo)
├── dance.glb        ← Avatar with hip hop dance animation (from Mixamo)
├── .htaccess        ← Removes .html extension from URLs
└── README.md
```

---

## How to Build Your Own

### Step 1 — Create your 3D avatar

1. Go to [readyplayer.me](https://readyplayer.me)
2. Take a selfie or customize your avatar manually
3. Download as `.glb`

### Step 2 — Add animations

1. Go to [mixamo.com](https://mixamo.com)
2. Upload your `.glb` — Mixamo will auto-rig it
3. Search **"Breathing Idle"** → download as FBX (with skin)
4. Search **"Hip Hop Dancing"** → download as FBX (with skin)
5. Convert both FBX files to GLB using [https://products.aspose.app/3d/conversion/fbx-to-glb](https://products.aspose.app/3d/conversion/fbx-to-glb) or Blender
6. Rename them `idle.glb` and `dance.glb`

### Step 3 — Customize the content

In `index.html`, update the speech bubble text:
```html
<div class="bubble" id="bubble">
  <strong>Hi, I'm [Your Name].</strong>
  <p>Your personal tagline here.</p>
</div>
```

In `about.html`, update the cards with your own info (search for the card sections).

### Step 4 — Run locally

You need a local server to load `.glb` files (browsers block local file access).

**Option A — Python:**
```bash
python3 -m http.server 8000
```
Then open `http://localhost:8000`

**Option B — VS Code:**
Install the Live Server extension → right-click `index.html` → Open with Live Server

### Step 5 — Deploy

**Netlify Drop (easiest):**
1. Go to [netlify.com/drop](https://app.netlify.com/drop)
2. Drag your entire folder onto the page
3. Get an instant public URL

**Any PHP host (Cloudways, shared hosting):**
1. Upload all files to `public_html` via FTP/SFTP
2. The `.htaccess` handles clean URLs automatically

---

## Customization

### Change avatar position
In `index.html` inside the `autoFit` function, adjust:
```js
model.position.x = 0; // positive = right, negative = left
```

### Change camera zoom
```js
camera.position.set(0, 1.0, 3.5); // z = distance (lower = closer)
camera.lookAt(0, 1.0, 0);         // y = height you're looking at
```

### Change head rotation sensitivity
```js
headBone.rotation.y = smoothX * 0.45;  // left/right — lower = subtler
headBone.rotation.x = -smoothY * 0.3; // up/down — lower = subtler
```

### Change background
In the CSS:
```css
body {
  background: radial-gradient(ellipse at center, #1a1a2e 0%, #0a0a0f 100%);
}
```

---

## How the Head Tracking Works

1. Mouse/touch position is normalized to `-1 → 1` on both axes
2. Every frame (60fps), the avatar's head and neck bones are rotated toward the target
3. `lerp` smoothing (10% per frame) creates the natural easing effect

```js
smoothX += (mouseX - smoothX) * 0.08;  // lerp
headBone.rotation.y = smoothX * 0.45;  // apply to bone
```

---

## License

MIT — free to use, modify, and share. A credit link is appreciated but not required.

Built by [Aris Kleidas](https://meet-aris.me)
