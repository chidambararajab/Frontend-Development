# Frontend Web APIs

A compact, interview-ready GitHub-flavored Markdown cheat sheet for senior frontend engineers. Each entry: one-line summary, a common tricky/interview point, and a short usage example.

---

## Table of Contents

* Storage
* Network & Real-time
* DOM & Observers
* Performance & Rendering
* Media & Graphics
* Background, Offline & Workers
* Security & Crypto
* Device & Input
* Utilities & Misc
* Common Interview Gotchas
* Modern APIs often asked in interviews

---

> **Format:** `API — 1‑line summary`
> `Tricky point:` — 1 line
> `Example:` — brief code snippet

---

# Storage

**localStorage** — synchronous key/value persisted across sessions.
**Tricky point:** Blocks main thread; avoid large/frequent writes.
**Example:**

```js
localStorage.setItem('theme','dark');
const t = localStorage.getItem('theme');
```

**sessionStorage** — like localStorage but cleared on tab close.
**Tricky point:** Scoped to tab; not shared between tabs.
**Example:**

```js
sessionStorage.setItem('step','2');
```

**IndexedDB** — async, transactional NoSQL DB for structured/binary data.
**Tricky point:** Handle schema upgrades (`onupgradeneeded`) carefully.
**Example (open):**

```js
const req = indexedDB.open('app',1);
req.onupgradeneeded = e => e.target.result.createObjectStore('notes',{keyPath:'id'});
```

**Cache Storage (Cache API)** — stores Request/Response pairs; used by Service Workers.
**Tricky point:** Separate from HTTP cache headers.
**Example:**

```js
const c = await caches.open('v1');
await c.put('/index.html', new Response('<html/>'));
```

---

# Network & Real‑time

**fetch** — Promise-based HTTP requests (modern replacement for XHR).
**Tricky point:** Cancel with `AbortController`; streaming responses possible.
**Example:**

```js
const ac = new AbortController();
fetch('/api',{signal:ac.signal}).then(r=>r.json());
// ac.abort() to cancel
```

**XMLHttpRequest (XHR)** — legacy API; useful to understand event states/progress.
**Tricky point:** Synchronous XHR blocks UI and is deprecated.
**Example:**

```js
const r = new XMLHttpRequest(); r.open('GET','/api'); r.onload = ()=>console.log(r.responseText); r.send();
```

**WebSocket** — full‑duplex persistent connection for real‑time.
**Tricky point:** Design reconnection/backoff and binary frame handling.
**Example:**

```js
const ws = new WebSocket('wss://example'); ws.onmessage = e=>console.log(e.data); ws.send('hi');
```

**Server‑Sent Events (EventSource)** — simple server→client streaming updates.
**Tricky point:** Unidirectional (server→client) and auto‑reconnect.
**Example:**

```js
const s = new EventSource('/events'); s.onmessage = e=>console.log(e.data);
```

---

# DOM & Observers

**MutationObserver** — observe DOM tree changes (preferred over DOM mutation events).
**Tricky point:** Mutations are batched; avoid heavy work per mutation.
**Example:**

```js
const mo = new MutationObserver(list=>{/* handle */});
mo.observe(document.body,{childList:true,subtree:true});
```

**IntersectionObserver** — detect elements entering/leaving viewport (lazy load).
**Tricky point:** `rootMargin`/`thresholds` change behavior; transforms can affect results.
**Example:**

```js
const io = new IntersectionObserver(e=>e.forEach(en=> en.isIntersecting && load(en.target)));
io.observe(img);
```

**ResizeObserver** — observe element size changes.
**Tricky point:** Avoid layout thrash inside callbacks.
**Example:**

```js
new ResizeObserver(entries=>entries.forEach(e=>console.log(e.contentRect.width))).observe(box);
```

---

# Performance & Rendering

**requestAnimationFrame** — schedule updates before next repaint for smooth animations.
**Tricky point:** Paused in background tabs; prefer over `setTimeout` for paint updates.
**Example:**

```js
function frame(){ /* update */ requestAnimationFrame(frame) }
requestAnimationFrame(frame);
```

**Performance API** (`performance.now`, marks/measures) — high‑resolution timing and custom measures.
**Tricky point:** Use marks/measures to profile critical paths precisely.
**Example:**

```js
performance.mark('start'); /* work */ performance.mark('end');
performance.measure('task','start','end');
```

**PerformanceObserver** — observe browser performance entries (LCP, resource timing, paint).
**Tricky point:** Cross‑origin resources need `Timing-Allow-Origin`.
**Example:**

```js
new PerformanceObserver(list=>console.log(list.getEntries())).observe({entryTypes:['paint','resource']});
```

---

# Media & Graphics

**Canvas (2D)** — draw dynamic 2D graphics; pixel manipulation.
**Tricky point:** Use WebGL for heavy GPU workloads; canvas pixel ops can be slow.
**Example:**

```js
const ctx = canvas.getContext('2d'); ctx.fillRect(0,0,100,100);
```

**WebGL / WebGPU** — GPU-accelerated 3D/compute (WebGPU is modern).
**Tricky point:** Shader compilation and GPU memory cause stalls; async init patterns help.
**Example (WebGL):**

```js
const gl = canvas.getContext('webgl'); gl.clear(gl.COLOR_BUFFER_BIT);
```

**getUserMedia (MediaDevices)** — access camera/mic with user permission.
**Tricky point:** Permissions and properly stopping tracks are important for privacy & battery.
**Example:**

```js
const s = await navigator.mediaDevices.getUserMedia({video:true}); video.srcObject = s;
```

**Web Audio API** (`AudioContext`) — audio graph, effects, analysis.
**Tricky point:** Autoplay restrictions require user gestures to start an AudioContext.
**Example:**

```js
const ac = new AudioContext(); const o = ac.createOscillator(); o.connect(ac.destination); o.start();
```

---

# Background, Offline & Workers

**Service Worker** — scriptable network proxy enabling offline, caching, and push.
**Tricky point:** Lifecycle (install/activate) and scope can cause surprising update behavior.
**Example (register):**

```js
navigator.serviceWorker.register('/sw.js').then(r=>console.log('sw',r));
```

**Web Workers / SharedWorker** — run JS off the main thread for CPU work.
**Tricky point:** Data transfer uses structured clone; large ArrayBuffers should be `transfer`ed.
**Example:**

```js
const w = new Worker('w.js'); w.postMessage({data}); w.onmessage = e=>console.log(e.data);
```

**Background Sync / Periodic Sync** — schedule work from SW when connectivity/per browser allows.
**Tricky point:** Browser support and reliability are limited; not a server cron substitute.
**Example (SW):**

```js
self.addEventListener('sync', e => { if(e.tag==='retry') e.waitUntil(doRetry()); });
```

---

# Security & Crypto

**Web Crypto API (SubtleCrypto)** — secure hashing, signing, encrypting (async).
**Tricky point:** Never implement crypto primitives yourself; use `crypto.subtle`.
**Example:**

```js
const d = new TextEncoder().encode('msg');
const hash = await crypto.subtle.digest('SHA-256', d);
```

**Credentials Management / WebAuthn** — passwordless auth and platform authenticators (passkeys).
**Tricky point:** Attestation and origin checks; UX & fallback are important.
**Example (create):**

```js
const cred = await navigator.credentials.create({publicKey: publicKeyOptions});
```

**Content Security Policy (CSP)** — header/meta to mitigate XSS by restricting sources.
**Tricky point:** Inline scripts need nonces or hashes; misconfig can break apps.
**Example (header):**

```
Content-Security-Policy: default-src 'self'; script-src 'self' 'nonce-abc'
```

---

# Device & Input

**Pointer Events** — unified pointer API (mouse, touch, stylus).
**Tricky point:** Use pointer capture for drag semantics; differences from touch events matter.
**Example:**

```js
el.addEventListener('pointerdown', e => el.setPointerCapture(e.pointerId));
```

**Geolocation API** — get device location (permission required).
**Tricky point:** Use `watchPosition` carefully due to battery drain and privacy.
**Example:**

```js
navigator.geolocation.getCurrentPosition(pos => console.log(pos.coords.latitude));
```

**Clipboard API** — async clipboard read/write (permissioned; writes should follow user gesture).
**Tricky point:** `read` is more restricted than `write`; always secure context.
**Example:**

```js
await navigator.clipboard.writeText('text'); const t = await navigator.clipboard.readText();
```

---

# Utilities & Misc

**History API** (`pushState`/`replaceState`) — manipulate URL/state without navigation.
**Tricky point:** Handle `popstate` for back/forward behavior and manage scroll restoration.
**Example:**

```js
history.pushState({page:2}, '', '/page2'); window.onpopstate = e => {/* handle */};
```

**BroadcastChannel** — pub/sub between same‑origin tabs/windows.
**Tricky point:** Simpler than `localStorage` events; limited to same origin.
**Example:**

```js
const bc = new BroadcastChannel('chat'); bc.postMessage({text:'hi'});
```

---

# Common interview gotchas (cheat bullets)

* **Sync APIs** (e.g., `localStorage`, sync XHR) block the main thread — performance risk.
* **Service Worker lifecycle** and cache invalidation cause surprising behavior.
* **Structured clone vs transferable**: postMessage cloning costs vs transferring ArrayBuffers.
* **Permissions & user gesture**: many APIs require HTTPS and/or a user gesture (clipboard, AudioContext, getUserMedia).
* **Cross-origin timing**: `performance` entries and `fetch` headers affected by CORS and `Timing-Allow-Origin`.

---

# Modern APIs often asked in interviews

* **Web Animations API** — native JS animations (`element.animate()`); performant and controllable.

```js
el.animate([{transform:'translateX(0)'},{transform:'translateX(100px)'}],{duration:300});
```

* **Web Share API** — native share sheet (mobile/Desktop support varies).

```js
await navigator.share({title:'Hi', text:'Look', url:location.href});
```

* **Payment Request API** — integrate native payment UIs for checkout flows.
* **OffscreenCanvas** — render canvas in Web Worker for multithreaded drawing.
* **WebRTC** — P2P audio/video/data (signaling and NAT traversal required).
* **File System Access API** — read/write local files with permission (powerful, gated).
* **WebGPU** — next‑gen GPU compute/graphics (emerging; asked for performance deep dives).
* **Web Bluetooth / Web USB / Web Serial** — device APIs (permission & security sensitive).

---

# Quick printing tips

* Use a mono font for code blocks when printing.
* Collapse long examples to single lines if printing many items.
* Convert this Markdown to PDF via GitHub render or `pandoc`/VS Code "Print" for a single‑page layout.

---

*This cheat sheet fits a single A4/Letter page when printed with compact styles (reduce margins/font-size).*

---

*Generated: Frontend Web APIs — concise reference for interviews.*
