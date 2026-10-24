[Uploading index (36).html…]()
<!doctype html>
<html lang="et">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1">
<title>Meistrimees VR</title>
<style>
  html,body{margin:0;height:100%;overflow:hidden;background:#9cc8e8;font-family:system-ui,-apple-system,"Segoe UI",sans-serif}
  canvas{display:block}
  #load{position:fixed;inset:0;display:flex;flex-direction:column;align-items:center;justify-content:center;background:linear-gradient(#8ec0e6,#e8f2d8);z-index:20;transition:opacity .6s;color:#2a3a22}
  #load h1{font-size:48px;margin:0 0 8px;letter-spacing:.06em;color:#6a3a1a}
  #info{position:fixed;left:14px;top:14px;max-width:360px;padding:14px 16px;background:rgba(255,250,238,.92);border:2px solid #b8865a;border-radius:12px;font-size:13px;line-height:1.55;color:#3a2a1a;z-index:5}
  #info h2{margin:0 0 6px;font-size:18px;color:#8a4a1a}
  #info .k{display:inline-block;min-width:14px;padding:0 5px;border:1px solid #b8865a;border-radius:4px;background:#fff;font-size:11px;margin-right:2px}
  #info button{margin-top:6px;background:#fff;border:1px solid #b8865a;border-radius:6px;padding:3px 10px;cursor:pointer}
  #cross{position:fixed;left:50%;top:50%;width:10px;height:10px;margin:-7px 0 0 -7px;border:2px solid rgba(255,255,255,.95);border-radius:50%;pointer-events:none;z-index:4;box-shadow:0 0 3px #000}
</style>
<script type="importmap">
{"imports":{
  "three":"https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.module.js",
  "three/addons/":"https://cdn.jsdelivr.net/npm/three@0.160.0/examples/jsm/"
}}
</script>
</head>
<body>
<div id="load"><h1>MEISTRIMEES</h1><p>Valmistan ehitusplatsi…</p></div>
<div id="info">
  <h2>MEISTRIMEES VR</h2>
  <b>Quest:</b> vajuta all <b>ENTER VR</b>. Vasaku käe randmel on menüü.<br>
  Parem päästik – kasuta tööriista (hoia ja liiguta = terve sein)<br>
  Vasak päästik – lammuta · A – pööra · B – lenda<br>
  X – järgmine klots · Y – järgmine tööriist · parem haare – ava uks<br>
  <b>Arvutis:</b> klõpsa ekraanil, hiirega vaatad<br>
  <span class="k">W</span><span class="k">A</span><span class="k">S</span><span class="k">D</span> liigu · <span class="k">Space</span> hüppa/üles · <span class="k">C</span> alla<br>
  vasak klõps – tööriist · parem klõps – lammuta<br>
  <span class="k">Tab</span> menüü · <span class="k">1</span>–<span class="k">6</span> tööriist · <span class="k">Q</span> klots · <span class="k">R</span> pööra<br>
  <span class="k">F</span> lenda · <span class="k">E</span> ava uks · <span class="k">Z</span> võta tagasi
  <br><button id="hideInfo">Peida</button>
</div>
<div id="cross"></div>
<script type="module">
import * as THREE from 'three';
import { VRButton } from 'three/addons/webxr/VRButton.js';
import { XRControllerModelFactory } from 'three/addons/webxr/XRControllerModelFactory.js';
import { mergeGeometries } from 'three/addons/utils/BufferGeometryUtils.js';

/* ================= ABI ================= */
const PI = Math.PI, V3 = THREE.Vector3;
const $ = id => document.getElementById(id);
const clamp = (v, a, b) => v < a ? a : v > b ? b : v;
const lerp = (a, b, t) => a + (b - a) * t;
const smooth = (a, b, x) => { const t = clamp((x - a) / (b - a), 0, 1); return t * t * (3 - 2 * t); };
const nextFrame = () => new Promise(r => requestAnimationFrame(() => r()));
function mulberry(a) { return function () { a |= 0; a = a + 0x6D2B79F5 | 0; let t = Math.imul(a ^ a >>> 15, 1 | a); t = t + Math.imul(t ^ t >>> 7, 61 | t) ^ t; return ((t ^ t >>> 14) >>> 0) / 4294967296; }; }
const R0 = mulberry(4242); const rnd = (a = 0, b = 1) => a + R0() * (b - a);
const _v = new V3(), _v2 = new V3(), _q = new THREE.Quaternion(), _m = new THREE.Matrix4(), _m2 = new THREE.Matrix4(), _c = new THREE.Color(), _e = new THREE.Euler();
$('hideInfo').onclick = () => $('info').style.display = 'none';

/* ================= RENDER ================= */
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setPixelRatio(Math.min(devicePixelRatio, 2)); renderer.setSize(innerWidth, innerHeight);
renderer.outputColorSpace = THREE.SRGBColorSpace; renderer.toneMapping = THREE.ACESFilmicToneMapping; renderer.toneMappingExposure = 1.05;
renderer.shadowMap.enabled = true; renderer.shadowMap.type = THREE.PCFSoftShadowMap;
renderer.xr.enabled = true; renderer.xr.setReferenceSpaceType('local-floor');
document.body.appendChild(renderer.domElement);
const scene = new THREE.Scene();
const SKY = new THREE.Color('#a8d4f0'); scene.background = SKY; scene.fog = new THREE.Fog('#c4e0f0', 70, 330);
const camera = new THREE.PerspectiveCamera(72, innerWidth / innerHeight, 0.05, 1500); camera.rotation.order = 'YXZ';
addEventListener('resize', () => { camera.aspect = innerWidth / innerHeight; camera.updateProjectionMatrix(); renderer.setSize(innerWidth, innerHeight); });
const SUN_DIR = new V3(0.55, 0.75, 0.38).normalize();
const sun = new THREE.DirectionalLight('#fff4e0', 2.6); sun.position.copy(SUN_DIR).multiplyScalar(60); sun.castShadow = true;
sun.shadow.mapSize.set(2048, 2048); const sc = sun.shadow.camera; sc.left = -30; sc.right = 30; sc.top = 30; sc.bottom = -30; sc.near = 1; sc.far = 150; sun.shadow.bias = -0.0005; sun.shadow.normalBias = 0.03;
scene.add(sun, sun.target); scene.add(new THREE.HemisphereLight('#cfe6ff', '#6a8a4a', 1.1));

/* ================= TEKSTUURID ================= */
function cnv(w, h) { const c = document.createElement('canvas'); c.width = w; c.height = h; return c; }
function ctex(c, rep = true) { const t = new THREE.CanvasTexture(c); t.colorSpace = THREE.SRGBColorSpace; t.anisotropy = Math.min(8, renderer.capabilities.getMaxAnisotropy()); if (rep) t.wrapS = t.wrapT = THREE.RepeatWrapping; return t; }
function noiseFill(g, w, h, n, a, cols) { const r = mulberry(n); for (let i = 0; i < n; i++) { g.fillStyle = cols[(r() * cols.length) | 0]; g.globalAlpha = a * r(); g.fillRect(r() * w, r() * h, 1 + r() * 3, 1 + r() * 3); } g.globalAlpha = 1; }
const TEX = {};
function makeTextures() {
  let c = cnv(256, 256), g = c.getContext('2d'); // palgid
  for (let k = 0; k < 4; k++) { const y = k * 64; const gr = g.createLinearGradient(0, y, 0, y + 64); gr.addColorStop(0, '#5a3418'); gr.addColorStop(0.15, '#a8703e'); gr.addColorStop(0.5, '#c48a52'); gr.addColorStop(0.85, '#8a5a2e'); gr.addColorStop(1, '#4a2a12'); g.fillStyle = gr; g.fillRect(0, y, 256, 64); }
  g.strokeStyle = 'rgba(70,40,15,0.35)'; for (let i = 0; i < 40; i++) { const y = Math.random() * 256; g.beginPath(); g.moveTo(0, y); g.bezierCurveTo(80, y + 3, 170, y - 3, 256, y + 1); g.stroke(); }
  TEX.log = ctex(c);
  c = cnv(256, 256); g = c.getContext('2d'); g.fillStyle = '#ece6dc'; g.fillRect(0, 0, 256, 256); // lauad (heledad, värvitavad)
  for (let x = 0; x < 256; x += 32) { g.fillStyle = 'rgba(0,0,0,0.28)'; g.fillRect(x, 0, 3, 256); g.fillStyle = 'rgba(255,255,255,0.35)'; g.fillRect(x + 3, 0, 2, 256); }
  g.strokeStyle = 'rgba(90,70,50,0.12)'; for (let i = 0; i < 70; i++) { const x = Math.random() * 256; g.beginPath(); g.moveTo(x, 0); g.lineTo(x + Math.random() * 6 - 3, 256); g.stroke(); }
  TEX.plank = ctex(c);
  c = cnv(256, 256); g = c.getContext('2d'); g.fillStyle = '#e8e4dc'; g.fillRect(0, 0, 256, 256); // põrand
  for (let y = 0; y < 256; y += 32) { g.fillStyle = 'rgba(0,0,0,0.25)'; g.fillRect(0, y, 256, 3); const off = (y / 32) % 2 * 90; g.fillRect(off + 60, y, 3, 32); g.fillRect(off + 190, y, 3, 32); }
  noiseFill(g, 256, 256, 1500, 0.15, ['#8a6a4a', '#fff']); TEX.floor = ctex(c);
  c = cnv(256, 256); g = c.getContext('2d'); g.fillStyle = '#f0ece4'; g.fillRect(0, 0, 256, 256); noiseFill(g, 256, 256, 6000, 0.12, ['#b0a898', '#ffffff']); TEX.plaster = ctex(c);
  c = cnv(256, 256); g = c.getContext('2d'); g.fillStyle = '#5a5a58'; g.fillRect(0, 0, 256, 256); // kivi
  const r = mulberry(7); for (let y = 0; y < 256; y += 42) for (let x = -20; x < 276; x += 52) { const xx = x + (y / 42 % 2) * 26 + r() * 8, ww = 44 + r() * 8, hh = 36 + r() * 4; const v = 110 + r() * 60; g.fillStyle = `rgb(${v},${v - 4},${v - 10})`; g.beginPath(); g.ellipse(xx + ww / 2, y + hh / 2 + 2, ww / 2, hh / 2, 0, 0, PI * 2); g.fill(); g.fillStyle = 'rgba(255,255,255,0.12)'; g.beginPath(); g.ellipse(xx + ww / 2 - 4, y + hh / 2 - 4, ww / 3, hh / 4, 0, 0, PI * 2); g.fill(); }
  TEX.stone = ctex(c);
  c = cnv(256, 256); g = c.getContext('2d'); g.fillStyle = '#c8c0b4'; g.fillRect(0, 0, 256, 256); // tellis
  for (let y = 0; y < 256; y += 32) for (let x = -64; x < 256; x += 64) { const xx = x + (y / 32 % 2) * 32; const v = r(); g.fillStyle = `rgb(${150 + v * 40 | 0},${58 + v * 20 | 0},${40 + v * 14 | 0})`; g.fillRect(xx + 3, y + 3, 58, 26); }
  noiseFill(g, 256, 256, 2500, 0.15, ['#000', '#fff']); TEX.brick = ctex(c);
  c = cnv(256, 256); g = c.getContext('2d'); g.fillStyle = '#d8d8d8'; g.fillRect(0, 0, 256, 256); // katusekivid
  for (let y = 0; y < 256; y += 32) for (let x = -32; x < 256; x += 32) { const xx = x + (y / 32 % 2) * 16; const gr = g.createLinearGradient(0, y, 0, y + 32); gr.addColorStop(0, '#f4f4f4'); gr.addColorStop(1, '#9a9a9a'); g.fillStyle = gr; g.beginPath(); g.moveTo(xx + 2, y); g.lineTo(xx + 30, y); g.lineTo(xx + 30, y + 26); g.quadraticCurveTo(xx + 16, y + 34, xx + 2, y + 26); g.closePath(); g.fill(); }
  TEX.roof = ctex(c);
  c = cnv(512, 512); g = c.getContext('2d'); g.fillStyle = '#5e8e3c'; g.fillRect(0, 0, 512, 512); noiseFill(g, 512, 512, 30000, 0.35, ['#4a7a2c', '#78a84a', '#86b456', '#3e6a26', '#9ab860']); TEX.grass = ctex(c); TEX.grass.repeat.set(120, 120);
  c = cnv(256, 256); g = c.getContext('2d'); g.fillStyle = '#e0cc98'; g.fillRect(0, 0, 256, 256); noiseFill(g, 256, 256, 9000, 0.3, ['#c8b07a', '#f0e0b0', '#b89a68']); TEX.sand = ctex(c); TEX.sand.repeat.set(10, 10);
  c = cnv(64, 256); g = c.getContext('2d'); g.fillStyle = '#ece8e0'; g.fillRect(0, 0, 64, 256); for (let i = 0; i < 26; i++) { g.fillStyle = '#2a2a2a'; g.fillRect(Math.random() * 64, Math.random() * 256, 6 + Math.random() * 18, 2 + Math.random() * 3); } TEX.birch = ctex(c);
}
function glowTex() { const c = cnv(128, 128), g = c.getContext('2d'); const gr = g.createRadialGradient(64, 64, 0, 64, 64, 64); gr.addColorStop(0, 'rgba(255,255,255,1)'); gr.addColorStop(0.3, 'rgba(255,255,255,0.6)'); gr.addColorStop(1, 'rgba(255,255,255,0)'); g.fillStyle = gr; g.fillRect(0, 0, 128, 128); return ctex(c, false); }

/* ================= KLOTSID ================= */
const PAINT = [['Naturaal', '#c8955a'], ['Punane', '#9a2e20'], ['Valge', '#f4f1ea'], ['Kollane', '#e8c048'], ['Roheline', '#4e7e3c'], ['Sinine', '#3e6eae'], ['Hall', '#7a7e86'], ['Must', '#2c2c30']];
const BT = {
  palk: { name: 'Palk', cost: { puit: 2 }, shape: 'cube', mat: 'log' },
  laud: { name: 'Laudsein', cost: { puit: 1 }, shape: 'cube', mat: 'plank', paint: 1 },
  krohv: { name: 'Krohv', cost: { kivi: 1, liiv: 1 }, shape: 'cube', mat: 'plaster', paint: 2 },
  kivi: { name: 'Kivisein', cost: { kivi: 2 }, shape: 'cube', mat: 'stone' },
  tellis: { name: 'Tellis', cost: { kivi: 1, liiv: 1 }, shape: 'cube', mat: 'brick' },
  aken: { name: 'Aken', cost: { puit: 1, liiv: 2 }, shape: 'window', paint: 2 },
  uks: { name: 'Uks', cost: { puit: 2, raud: 1 }, shape: 'door', paint: 0 },
  porand: { name: 'Põrand', cost: { puit: 1 }, shape: 'slab', mat: 'floor', paint: 0 },
  katus: { name: 'Katus', cost: { puit: 1, kivi: 1 }, shape: 'wedge', mat: 'roof', paint: 6 },
  hari: { name: 'Katusehari', cost: { puit: 1, kivi: 1 }, shape: 'ridge', mat: 'roof', paint: 6 },
  trepp: { name: 'Trepp', cost: { puit: 1 }, shape: 'stairs', mat: 'floor', paint: 0 },
  aed: { name: 'Aed', cost: { puit: 1 }, shape: 'fence', paint: 2 },
  tala: { name: 'Post', cost: { puit: 1 }, shape: 'post', mat: 'log' },
};
const BORDER = Object.keys(BT);
const RES_NAME = { puit: 'Puit', kivi: 'Kivi', liiv: 'Liiv', raud: 'Raud' };
const costTxt = c => Object.entries(c).map(([k, v]) => `${v} ${RES_NAME[k].toLowerCase()}`).join(' + ');

function boxUV(w, h, d) { const g = new THREE.BoxGeometry(w, h, d); const uv = g.attributes.uv; const dims = [[d, h], [d, h], [w, d], [w, d], [w, h], [w, h]]; for (let f = 0; f < 6; f++) for (let i = 0; i < 4; i++) { const k = f * 4 + i; uv.setXY(k, uv.getX(k) * dims[f][0], uv.getY(k) * dims[f][1]); } return g.toNonIndexed(); }
const bx = (w, h, d, x, y, z) => boxUV(w, h, d).translate(x, y, z);
function polyGeo(polys, center) { // tasapinnalised hulknurgad, normaalid väljapoole
  const P = [], U = [];
  for (const poly of polys) {
    const a = new V3(...poly[0]); const n = new V3().subVectors(new V3(...poly[1]), a).cross(new V3().subVectors(new V3(...poly[2]), a));
    const cen = poly.reduce((s, p) => s.add(new V3(...p)), new V3()).multiplyScalar(1 / poly.length);
    let pts = poly; if (n.dot(cen.clone().sub(center)) < 0) pts = [...poly].reverse();
    const an = [Math.abs(n.x), Math.abs(n.y), Math.abs(n.z)]; const ax = an.indexOf(Math.max(...an));
    const uv = p => ax === 0 ? [p[2] + 0.5, p[1]] : ax === 1 ? [p[0] + 0.5, p[2] + 0.5] : [p[0] + 0.5, p[1]];
    for (let i = 1; i < pts.length - 1; i++) for (const p of [pts[0], pts[i], pts[i + 1]]) { P.push(...p); U.push(...uv(p)); }
  }
  const g = new THREE.BufferGeometry(); g.setAttribute('position', new THREE.Float32BufferAttribute(P, 3)); g.setAttribute('uv', new THREE.Float32BufferAttribute(U, 2)); g.computeVertexNormals(); return g;
}
const merge = gs => mergeGeometries(gs.map(g => g.index ? g.toNonIndexed() : g), false);
const MAT = {};
const TP = {}; // tüüp -> osad
function buildBlockParts() {
  const std = (o) => new THREE.MeshStandardMaterial(Object.assign({ roughness: 0.82, metalness: 0 }, o));
  MAT.log = std({ map: TEX.log }); MAT.plank = std({ map: TEX.plank }); MAT.plaster = std({ map: TEX.plaster, roughness: 0.95 });
  MAT.stone = std({ map: TEX.stone, roughness: 0.95 }); MAT.brick = std({ map: TEX.brick, roughness: 0.9 }); MAT.floor = std({ map: TEX.floor }); MAT.roof = std({ map: TEX.roof, roughness: 0.7 });
  MAT.trim = std({ color: '#ffffff', roughness: 0.6 }); MAT.dark = std({ color: '#3a3430', roughness: 0.5, metalness: 0.5 });
  MAT.glass = new THREE.MeshStandardMaterial({ color: '#b8e0f4', transparent: true, opacity: 0.32, roughness: 0.05, metalness: 0.2, side: THREE.DoubleSide, depthWrite: false });
  const cube = bx(1, 1, 1, 0, 0.5, 0);
  const wedge = polyGeo([[[-.5, 0, -.5], [.5, 0, -.5], [.5, 0, .5], [-.5, 0, .5]], [[-.5, 0, .5], [.5, 0, .5], [.5, 1, .5], [-.5, 1, .5]], [[-.5, 0, -.5], [.5, 0, -.5], [.5, 1, .5], [-.5, 1, .5]], [[-.5, 0, -.5], [-.5, 0, .5], [-.5, 1, .5]], [[.5, 0, -.5], [.5, 0, .5], [.5, 1, .5]]], new V3(0, 0.3, 0.15));
  const ridge = polyGeo([[[-.5, 0, -.5], [.5, 0, -.5], [.5, 0, .5], [-.5, 0, .5]], [[-.5, 0, -.5], [.5, 0, -.5], [.5, .5, 0], [-.5, .5, 0]], [[-.5, 0, .5], [.5, 0, .5], [.5, .5, 0], [-.5, .5, 0]], [[-.5, 0, -.5], [-.5, 0, .5], [-.5, .5, 0]], [[.5, 0, -.5], [.5, 0, .5], [.5, .5, 0]]], new V3(0, 0.15, 0));
  const stairs = merge([bx(1, 0.5, 0.5, 0, 0.25, -0.25), bx(1, 1, 0.5, 0, 0.5, 0.25)]);
  const slab = bx(1, 0.15, 1, 0, 0.075, 0);
  const post = bx(0.24, 1, 0.24, 0, 0.5, 0);
  const fence = merge([bx(0.12, 1, 0.12, 0, 0.5, 0), bx(1, 0.09, 0.05, 0, 0.38, 0), bx(1, 0.09, 0.05, 0, 0.78, 0)]);
  const winFrame = merge([bx(0.12, 1, 0.2, -0.44, 0.5, 0), bx(0.12, 1, 0.2, 0.44, 0.5, 0), bx(1, 0.12, 0.2, 0, 0.06, 0), bx(1, 0.12, 0.2, 0, 0.94, 0), bx(0.05, 0.78, 0.08, 0, 0.5, 0), bx(0.78, 0.05, 0.08, 0, 0.55, 0), bx(1.06, 0.05, 0.32, 0, 0.12, 0.05)]);
  const glass = new THREE.PlaneGeometry(0.78, 0.78).translate(0, 0.5, 0);
  const doorFrame = merge([bx(0.08, 2, 0.22, -0.46, 1, 0), bx(0.08, 2, 0.22, 0.46, 1, 0), bx(1, 0.1, 0.22, 0, 1.95, 0)]);
  const leaf = bx(0.84, 1.88, 0.06, 0.42, 0.96, 0);
  const knob = merge([new THREE.SphereGeometry(0.035, 10, 8).translate(0.74, 1.0, 0.06), new THREE.SphereGeometry(0.035, 10, 8).translate(0.74, 1.0, -0.06)]);
  const P = (geo, mat, paint, door) => ({ geo, mat, paint: !!paint, door, im: null, cap: 0 });
  for (const t of BORDER) {
    const d = BT[t];
    if (d.shape === 'cube') TP[t] = [P(cube, MAT[d.mat], d.paint !== undefined)];
    else if (d.shape === 'wedge') TP[t] = [P(wedge, MAT.roof, true)];
    else if (d.shape === 'ridge') TP[t] = [P(ridge, MAT.roof, true)];
    else if (d.shape === 'stairs') TP[t] = [P(stairs, MAT.floor, true)];
    else if (d.shape === 'slab') TP[t] = [P(slab, MAT.floor, true)];
    else if (d.shape === 'post') TP[t] = [P(post, MAT.log, false)];
    else if (d.shape === 'fence') TP[t] = [P(fence, MAT.trim, true)];
    else if (d.shape === 'window') TP[t] = [P(winFrame, MAT.trim, true), P(glass, MAT.glass, false)];
    else if (d.shape === 'door') TP[t] = [P(doorFrame, MAT.trim, true), P(leaf, MAT.plank, true, 'leaf'), P(knob, MAT.dark, false, 'leaf')];
  }
}

/* ================= OLEK ================= */
const SAVE = 'meister_v1';
const DEF = () => ({ mode: 'easy', res: { puit: 0, kivi: 0, liiv: 0, raud: 0 }, tools: { haamer: 0, saag: 0, pintsel: 0, kirves: 1, kirka: 0, labidas: 0 }, dur: {}, blocks: [], project: null, done: [], fly: true, music: true, grid: true, shadows: true, tool: 'haamer', block: 'palk', paint: 1 });
let S = DEF();
try { const d = JSON.parse(localStorage.getItem(SAVE) || 'null'); if (d) { S = Object.assign(DEF(), d); S.res = Object.assign(DEF().res, d.res); S.tools = Object.assign(DEF().tools, d.tools); } } catch (e) { }
let dirtySave = false; function save() { try { S.blocks = []; for (const [k, b] of BLOCKS) if (b.t !== '_') { const [i, j, kk] = k.split(',').map(Number); S.blocks.push([i, j, kk, BORDER.indexOf(b.t), b.r, b.c, b.o ? 1 : 0]); } localStorage.setItem(SAVE, JSON.stringify(S)); } catch (e) { } dirtySave = false; }
setInterval(() => { if (dirtySave) save(); }, 4000);
const easy = () => S.mode === 'easy';
const has = t => easy() || S.tools[t] > 0;
const tier = t => easy() ? 3 : S.tools[t];
const R = { tool: S.tool, rot: 0, drag: null, working: false, workT: 0, swing: 0, msg: '', msgT: 0, queue: [], vy: 0, grounded: true, snap: false, time: 0, menuDesk: false, mouseUse: false, hint: '', resetArm: 0, soundT: 0, lastLack: -9 };
const TOOLS = [['haamer', 'Haamer'], ['saag', 'Saag'], ['pintsel', 'Pintsel'], ['kirves', 'Kirves'], ['kirka', 'Kirka'], ['labidas', 'Labidas']];
const TOOL_NAME = Object.fromEntries(TOOLS);
const TIER_NAME = ['', 'Puu', 'Kivi', 'Raud'];

/* ================= KLOTSIDE KAART ================= */
const PLOT = 20, MAXK = 14;
const BLOCKS = new Map(); const DIRTY = new Set();
const key = (i, j, k) => i + ',' + j + ',' + k;
function getB(i, j, k) { const b = BLOCKS.get(key(i, j, k)); if (b && b.t === '_') return BLOCKS.get(b.ref); return b; }
function getRaw(i, j, k) { return BLOCKS.get(key(i, j, k)); }
const inPlot = (i, j, k) => i >= -PLOT && i < PLOT && j >= -PLOT && j < PLOT && k >= 0 && k < MAXK;
function addBlock(i, j, k, t, r, c, o = false) {
  const kk = key(i, j, k); BLOCKS.set(kk, { t, r, c: c ?? (BT[t].paint ?? 0), o });
  if (t === 'uks') BLOCKS.set(key(i, j, k + 1), { t: '_', ref: kk });
  DIRTY.add(t); dirtySave = true;
}
function removeAt(i, j, k) {
  let kk = key(i, j, k); let b = BLOCKS.get(kk); if (!b) return null;
  if (b.t === '_') { kk = b.ref; b = BLOCKS.get(kk); if (!b) return null; [i, j, k] = kk.split(',').map(Number); }
  BLOCKS.delete(kk); if (b.t === 'uks') BLOCKS.delete(key(i, j, k + 1));
  DIRTY.add(b.t); dirtySave = true; return { i, j, k, ...b };
}
function ensureIM(part, n) {
  if (part.im && part.cap >= n) return part.im;
  if (part.im) { scene.remove(part.im); part.im.dispose(); }
  part.cap = Math.max(32, 1 << Math.ceil(Math.log2(Math.max(1, n))));
  const im = new THREE.InstancedMesh(part.geo, part.mat, part.cap); im.frustumCulled = false;
  im.castShadow = part.mat !== MAT.glass; im.receiveShadow = true; scene.add(im); part.im = im; return im;
}
const BASE = new THREE.Matrix4(), LEAF = new THREE.Matrix4();
function rebuildType(t) {
  const list = []; for (const [k, b] of BLOCKS) if (b.t === t) list.push([k, b]);
  for (const part of TP[t]) {
    const im = ensureIM(part, list.length); im.count = list.length;
    list.forEach(([k, b], idx) => {
      const [i, j, kk] = k.split(',').map(Number);
      BASE.makeRotationY(b.r * PI / 2).setPosition(i + 0.5, kk, j + 0.5);
      if (part.door) { LEAF.makeTranslation(-0.42, 0, 0).multiply(_m2.makeRotationY(b.o ? -PI / 2 : 0)); _m.multiplyMatrices(BASE, LEAF); im.setMatrixAt(idx, _m); }
      else im.setMatrixAt(idx, BASE);
      if (part.paint) im.setColorAt(idx, _c.set(PAINT[b.c][1]));
    });
    im.instanceMatrix.needsUpdate = true; if (im.instanceColor) im.instanceColor.needsUpdate = true;
  }
}
// kujude kõrgus (põrkumiseks)
function localXZ(r, wx, wz) { const s = Math.sin(r * PI / 2), c = Math.cos(r * PI / 2); return [wx * c - wz * s, wx * s + wz * c]; }
function topAt(b, wx, wz) { // wx,wz = suhteline klotsi keskpunktist; tagastab kõrguse 0..1 või -1 = läbitav
  const sh = BT[b.t].shape; if (sh === 'cube' || sh === 'window') return 1;
  if (sh === 'door') return b.o ? -1 : 1;
  if (sh === 'slab') return 0.15;
  const [lx, lz] = localXZ(b.r, wx, wz);
  if (sh === 'wedge') return clamp(lz + 0.5, 0.05, 1);
  if (sh === 'stairs') return lz < 0 ? 0.5 : 1;
  if (sh === 'ridge') return clamp(0.5 - Math.abs(lz), 0.05, 0.5);
  if (sh === 'post') return Math.abs(lx) < 0.17 && Math.abs(lz) < 0.17 ? 1 : -1;
  if (sh === 'fence') return Math.abs(lz) < 0.12 || (Math.abs(lx) < 0.12) ? 1 : -1;
  return 1;
}
function cellTop(i, j, k, x, z) { const raw = getRaw(i, j, k); if (!raw) return -1; const b = raw.t === '_' ? BLOCKS.get(raw.ref) : raw; if (!b) return -1; if (raw.t === '_') return b.o ? -1 : 1; return topAt(b, x - (i + 0.5), z - (j + 0.5)); }

/* ================= MAAILM ================= */
const LAKE = new V3(0, 0, 50), LAKE_R = 14;
const TREES = [], ROCKS = []; const treeIMs = {}; let stumpIM, rockIM, ironIM;
function buildWorld() {
  // taevas
  const dg = new THREE.SphereGeometry(900, 32, 16); const col = new Float32Array(dg.attributes.position.count * 3); const ca = new THREE.Color('#e4f0f4'), cb = new THREE.Color('#5a9ad8');
  for (let i = 0; i < dg.attributes.position.count; i++) { const t = clamp(dg.attributes.position.getY(i) / 900, 0, 1); const c = ca.clone().lerp(cb, Math.pow(t, 0.5)); col.set([c.r, c.g, c.b], i * 3); }
  dg.setAttribute('color', new THREE.BufferAttribute(col, 3)); scene.add(new THREE.Mesh(dg, new THREE.MeshBasicMaterial({ vertexColors: true, side: THREE.BackSide, fog: false, depthWrite: false })));
  const GL = glowTex(); const sunS = new THREE.Sprite(new THREE.SpriteMaterial({ map: GL, color: '#fff8e0', fog: false, depthWrite: false, transparent: true })); sunS.position.copy(SUN_DIR).multiplyScalar(800); sunS.scale.setScalar(160); scene.add(sunS);
  // pilved
  const cc = cnv(256, 128), cg = cc.getContext('2d'); for (let i = 0; i < 40; i++) { const x = 40 + Math.random() * 176, y = 50 + Math.random() * 40, s = 20 + Math.random() * 40; const gr = cg.createRadialGradient(x, y, 0, x, y, s); gr.addColorStop(0, 'rgba(255,255,255,0.5)'); gr.addColorStop(1, 'rgba(255,255,255,0)'); cg.fillStyle = gr; cg.fillRect(0, 0, 256, 128); }
  const ct = ctex(cc, false); for (let i = 0; i < 26; i++) { const s = new THREE.Sprite(new THREE.SpriteMaterial({ map: ct, fog: false, transparent: true, depthWrite: false, opacity: 0.9 })); const a = rnd(0, PI * 2), d = rnd(250, 600); s.position.set(Math.cos(a) * d, rnd(120, 220), Math.sin(a) * d); s.scale.set(rnd(160, 300), rnd(60, 100), 1); scene.add(s); }
  // maa
  const ground = new THREE.Mesh(new THREE.CircleGeometry(700, 64).rotateX(-PI / 2), new THREE.MeshStandardMaterial({ map: TEX.grass, roughness: 1 })); ground.receiveShadow = true; scene.add(ground);
  // mäed kaugel
  const hm = new THREE.MeshStandardMaterial({ color: '#5e8a58', roughness: 1, flatShading: true });
  for (let i = 0; i < 26; i++) { const a = i / 26 * PI * 2 + rnd(-0.1, 0.1), d = rnd(230, 330); const h = new THREE.Mesh(new THREE.IcosahedronGeometry(1, 1), hm); h.position.set(Math.cos(a) * d, -10, Math.sin(a) * d); h.scale.set(rnd(50, 90), rnd(22, 48), rnd(50, 90)); scene.add(h); }
  // järv ja rand
  const sand = new THREE.Mesh(new THREE.CircleGeometry(LAKE_R + 4.5, 48).rotateX(-PI / 2), new THREE.MeshStandardMaterial({ map: TEX.sand, roughness: 1 })); sand.position.set(LAKE.x, 0.02, LAKE.z); sand.receiveShadow = true; scene.add(sand);
  const water = new THREE.Mesh(new THREE.CircleGeometry(LAKE_R, 48).rotateX(-PI / 2), new THREE.MeshStandardMaterial({ color: '#2e6a9a', roughness: 0.08, metalness: 0.35, transparent: true, opacity: 0.92 })); water.position.set(LAKE.x, 0.05, LAKE.z); scene.add(water);
  const dock = new THREE.Group(); const dm = new THREE.MeshStandardMaterial({ map: TEX.floor, color: '#a07a50' }); for (let i = 0; i < 9; i++) { const p = new THREE.Mesh(boxUV(2, 0.08, 0.6), dm); p.position.set(5, 0.35, 34 + i * 0.7); p.castShadow = p.receiveShadow = true; dock.add(p); } for (const z of [34, 40]) for (const x of [4.1, 5.9]) { const pl = new THREE.Mesh(new THREE.CylinderGeometry(0.1, 0.1, 1, 8), MAT.log); pl.position.set(x, 0.1, z); dock.add(pl); } scene.add(dock);
  // ruudustik ehitusplatsil
  const grid = new THREE.GridHelper(PLOT * 2, PLOT * 2, '#ffffff', '#ffffff'); grid.material.transparent = true; grid.material.opacity = 0.16; grid.position.y = 0.012; scene.add(grid); WORLD.grid = grid;
  const corner = new THREE.MeshStandardMaterial({ color: '#e05a20' }); for (const x of [-PLOT, PLOT]) for (const z of [-PLOT, PLOT]) { const p = new THREE.Mesh(new THREE.CylinderGeometry(0.08, 0.08, 1, 8), corner); p.position.set(x, 0.5, z); scene.add(p); }
  // puud
  const trunkM = new THREE.MeshStandardMaterial({ color: '#6a4a30', roughness: 1 }), pineT = new THREE.MeshStandardMaterial({ color: '#a0603a', roughness: 1 }), birchT = new THREE.MeshStandardMaterial({ map: TEX.birch, roughness: 1 });
  const spruceC = new THREE.MeshStandardMaterial({ color: '#2e5a32', roughness: 1, flatShading: true }), pineC = new THREE.MeshStandardMaterial({ color: '#3e6a36', roughness: 1, flatShading: true }), birchC = new THREE.MeshStandardMaterial({ color: '#6ea442', roughness: 1, flatShading: true });
  const G = {
    kuusk: [new THREE.CylinderGeometry(0.16, 0.24, 1.6, 7).translate(0, 0.8, 0), merge([new THREE.ConeGeometry(1.9, 2.6, 8).translate(0, 2.3, 0), new THREE.ConeGeometry(1.45, 2.2, 8).translate(0, 3.6, 0), new THREE.ConeGeometry(0.95, 1.9, 8).translate(0, 4.8, 0)]), trunkM, spruceC],
    mand: [new THREE.CylinderGeometry(0.14, 0.22, 4.6, 7).translate(0, 2.3, 0), merge([new THREE.IcosahedronGeometry(1.5, 0).scale(1.3, 0.7, 1.3).translate(0, 4.9, 0), new THREE.IcosahedronGeometry(1.1, 0).scale(1.2, 0.7, 1.2).translate(0.6, 5.6, 0.3)]), pineT, pineC],
    kask: [new THREE.CylinderGeometry(0.12, 0.17, 3.6, 7).translate(0, 1.8, 0), merge([new THREE.IcosahedronGeometry(1.4, 1).scale(1, 1.25, 1).translate(0, 4.2, 0), new THREE.IcosahedronGeometry(0.9, 1).translate(0.7, 3.4, 0.2)]), birchT, birchC],
  };
  const pos = [];
  for (let t = 0; pos.length < 170 && t < 5000; t++) {
    const a = rnd(0, PI * 2), d = 24 + Math.pow(rnd(), 0.8) * 55; const x = Math.cos(a) * d, z = Math.sin(a) * d;
    if (Math.abs(x) < 23 && Math.abs(z) < 23) continue; if (Math.abs(x) < 15 && z > 19 && z < 34) continue; if (Math.hypot(x - LAKE.x, z - LAKE.z) < LAKE_R + 6) continue;
    if (pos.some(p => Math.hypot(p[0] - x, p[1] - z) < 2.6)) continue; pos.push([x, z]);
  }
  const types = { kuusk: [], mand: [], kask: [] };
  for (const [x, z] of pos) { const r = rnd(); const ty = r < 0.45 ? 'kuusk' : r < 0.7 ? 'mand' : 'kask'; const tr = { type: ty, x, z, s: rnd(0.75, 1.3), rot: rnd(0, 6), idx: types[ty].length, hp: 0, state: 'up', t: 0, dir: 0, respawn: 0 }; types[ty].push(tr); TREES.push(tr); }
  for (const ty in G) { const [tg, cgeo, tm, cm] = G[ty]; const n = types[ty].length; const a = new THREE.InstancedMesh(tg, tm, n), b = new THREE.InstancedMesh(cgeo, cm, n); a.castShadow = b.castShadow = true; a.receiveShadow = b.receiveShadow = true; scene.add(a, b); treeIMs[ty] = [a, b]; }
  stumpIM = new THREE.InstancedMesh(new THREE.CylinderGeometry(0.22, 0.26, 0.35, 8).translate(0, 0.17, 0), trunkM, TREES.length); stumpIM.count = TREES.length; scene.add(stumpIM);
  for (const tr of TREES) setTreeMatrix(tr, 1, 0);
  for (let i = 0; i < TREES.length; i++) stumpIM.setMatrixAt(i, _m.makeScale(0, 0, 0));
  // kivid
  const rockGeo = (seed, iron) => { const g = new THREE.IcosahedronGeometry(1, 1); const p = g.attributes.position; const r = mulberry(seed); const cl = new Float32Array(p.count * 3); for (let i = 0; i < p.count; i++) { _v.fromBufferAttribute(p, i); const k = 0.8 + r() * 0.35; p.setXYZ(i, _v.x * k, Math.max(-0.3, _v.y * k * 0.75), _v.z * k); const v = 0.35 + r() * 0.15; const sp = iron && r() < 0.3; cl.set(sp ? [0.75, 0.38, 0.12] : [v, v * 0.98, v * 0.95], i * 3); } g.setAttribute('color', new THREE.BufferAttribute(cl, 3)); g.computeVertexNormals(); return g; };
  const rm = new THREE.MeshStandardMaterial({ vertexColors: true, roughness: 0.95, flatShading: true });
  rockIM = new THREE.InstancedMesh(rockGeo(3, false), rm, 40); ironIM = new THREE.InstancedMesh(rockGeo(9, true), rm, 12); rockIM.castShadow = ironIM.castShadow = true; rockIM.receiveShadow = ironIM.receiveShadow = true; scene.add(rockIM, ironIM);
  let ni = 0, nr = 0;
  for (let t = 0; (ni < 12 || nr < 40) && t < 4000; t++) {
    const a = rnd(0, PI * 2), d = rnd(24, 60); const x = Math.cos(a) * d, z = Math.sin(a) * d;
    if (Math.abs(x) < 23 && Math.abs(z) < 23) continue; if (Math.abs(x) < 15 && z > 19 && z < 34) continue; if (Math.hypot(x - LAKE.x, z - LAKE.z) < LAKE_R + 5) continue;
    if (pos.some(p => Math.hypot(p[0] - x, p[1] - z) < 2) || ROCKS.some(r => Math.hypot(r.x - x, r.z - z) < 3)) continue;
    const iron = ni < 12 && rnd() < 0.3; if (!iron && nr >= 40) continue;
    const r = { x, z, r: iron ? rnd(0.7, 1.0) : rnd(0.6, 1.2), iron, im: iron ? ironIM : rockIM, idx: iron ? ni++ : nr++, hp: 0, alive: true, respawn: 0, rot: rnd(0, 6) }; ROCKS.push(r); setRock(r, 1);
  }
}
const WORLD = {};
function setTreeMatrix(tr, scale, fall) {
  _m.makeTranslation(tr.x, 0, tr.z);
  if (fall) { _m2.makeRotationAxis(_v.set(Math.cos(tr.dir), 0, Math.sin(tr.dir)), fall); _m.multiply(_m2); }
  _m.multiply(_m2.makeRotationY(tr.rot)).multiply(_m2.makeScale(tr.s * scale, tr.s * scale, tr.s * scale));
  for (const im of treeIMs[tr.type]) { im.setMatrixAt(tr.idx, _m); im.instanceMatrix.needsUpdate = true; }
}
function setRock(r, s) { _m.compose(_v.set(r.x, r.r * 0.3, r.z), _q.setFromEuler(_e.set(0, r.rot, 0)), _v2.setScalar(r.r * s)); r.im.setMatrixAt(r.idx, _m); r.im.instanceMatrix.needsUpdate = true; }

/* ================= TÖÖKODA ================= */
function buildWorkshop() {
  const wood = new THREE.MeshStandardMaterial({ map: TEX.floor, color: '#b88a5a', roughness: 0.8 });
  const tb = new THREE.Group(); tb.position.set(-4.4, 0, 24.6); tb.rotation.y = 0.5;
  const top = new THREE.Mesh(boxUV(2.2, 0.12, 1), wood); top.position.y = 0.9; tb.add(top);
  for (const x of [-1, 1]) for (const z of [-0.4, 0.4]) { const l = new THREE.Mesh(boxUV(0.1, 0.9, 0.1), wood); l.position.set(x, 0.45, z); tb.add(l); }
  const vise = new THREE.Mesh(boxUV(0.25, 0.18, 0.2), MAT.dark); vise.position.set(0.85, 1.05, 0); tb.add(vise);
  tb.traverse(o => { if (o.isMesh) { o.castShadow = o.receiveShadow = true; } }); scene.add(tb);
  for (const [x, z] of [[4.6, 24.4], [-3.4, 30], [3.4, 30]]) for (const dx of [-0.75, 0.75]) { const p = new THREE.Mesh(boxUV(0.12, 2.2, 0.12), wood); p.position.set(x + dx * Math.cos(Math.atan2(-x, 26 - z)), 1.1, z - dx * Math.sin(Math.atan2(-x, 26 - z))); p.castShadow = true; scene.add(p); }
}

/* ================= HELI ================= */
let AC = null, master, musicBus;
function initAudio() {
  if (AC) { if (AC.state === 'suspended') AC.resume(); return; }
  try { AC = new (window.AudioContext || window.webkitAudioContext)(); } catch (e) { return; }
  master = AC.createGain(); master.gain.value = 0.7; master.connect(AC.destination);
  musicBus = AC.createGain(); musicBus.gain.value = S.music ? 1 : 0; musicBus.connect(master);
  const dl = AC.createDelay(); dl.delayTime.value = 0.35; const fb = AC.createGain(); fb.gain.value = 0.3; musicBus.connect(dl); dl.connect(fb).connect(dl); dl.connect(master);
  const buf = AC.createBuffer(1, AC.sampleRate * 3, AC.sampleRate), d = buf.getChannelData(0); let l = 0; for (let i = 0; i < d.length; i++) { l = (l + 0.02 * (Math.random() * 2 - 1)) / 1.02; d[i] = l * 3; }
  const wind = AC.createBufferSource(); wind.buffer = buf; wind.loop = true; const wf = AC.createBiquadFilter(); wf.type = 'lowpass'; wf.frequency.value = 400; const wg = AC.createGain(); wg.gain.value = 0.05; wind.connect(wf).connect(wg).connect(master); wind.start();
}
function tone(f1, f2, dur, type = 'sine', vol = 0.2, delay = 0, bus) { if (!AC) return; const t = AC.currentTime + delay; const o = AC.createOscillator(), g = AC.createGain(); o.type = type; o.frequency.setValueAtTime(f1, t); o.frequency.exponentialRampToValueAtTime(Math.max(1, f2), t + dur); g.gain.setValueAtTime(vol, t); g.gain.exponentialRampToValueAtTime(0.0001, t + dur); o.connect(g).connect(bus || master); o.start(t); o.stop(t + dur + 0.02); }
function noise(dur, vol, type, f1, f2, delay = 0) { if (!AC) return; const t = AC.currentTime + delay; const buf = AC.createBuffer(1, Math.max(1, AC.sampleRate * dur | 0), AC.sampleRate), d = buf.getChannelData(0); for (let i = 0; i < d.length; i++) d[i] = (Math.random() * 2 - 1) * (1 - i / d.length); const s = AC.createBufferSource(); s.buffer = buf; const f = AC.createBiquadFilter(); f.type = type; f.frequency.setValueAtTime(f1, t); f.frequency.exponentialRampToValueAtTime(f2, t + dur); const g = AC.createGain(); g.gain.value = vol; s.connect(f).connect(g).connect(master); s.start(t); }
const sfx = {
  hammer: () => { noise(0.07, 0.5, 'lowpass', 2500, 600); tone(190, 90, 0.09, 'triangle', 0.25); },
  saw: () => { for (let i = 0; i < 3; i++) noise(0.14, 0.25, 'bandpass', 2600, 1800, i * 0.15); },
  chop: () => { tone(150, 60, 0.12, 'triangle', 0.35); noise(0.06, 0.35, 'lowpass', 1800, 400); },
  mine: () => { tone(1300, 900, 0.08, 'square', 0.06); tone(2600, 2000, 0.06, 'sine', 0.08); noise(0.05, 0.25, 'highpass', 3000, 1500); },
  dig: () => noise(0.22, 0.35, 'lowpass', 900, 200),
  pop: () => tone(520, 820, 0.07, 'sine', 0.15),
  paint: () => noise(0.25, 0.18, 'highpass', 3000, 5000),
  fall: () => { noise(1.1, 0.35, 'lowpass', 1500, 120); tone(70, 40, 0.5, 'sine', 0.4, 1.0); noise(0.4, 0.5, 'lowpass', 600, 80, 1.0); },
  fanfare: () => [523, 659, 784, 1046].forEach((f, i) => tone(f, f, 0.35, 'triangle', 0.15, i * 0.12)),
  deny: () => tone(220, 160, 0.2, 'sawtooth', 0.07),
  click: () => tone(1100, 900, 0.04, 'square', 0.05),
  door: () => { tone(300, 180, 0.25, 'triangle', 0.12); noise(0.2, 0.15, 'bandpass', 800, 400); },
  bird: () => { const f = 2400 + Math.random() * 1600, n = 2 + (Math.random() * 4 | 0); for (let i = 0; i < n; i++) tone(f * (1 + Math.random() * 0.2), f * 0.8, 0.09, 'sine', 0.03, i * 0.12); },
};
const PENTA = [261.6, 293.7, 329.6, 392, 440, 523.3, 587.3, 659.3];
let musT = 1, birdT = 3;
function updateAmbient(dt) {
  if (!AC) return; musicBus.gain.setTargetAtTime(S.music ? 1 : 0, AC.currentTime, 0.4);
  birdT -= dt; if (birdT < 0) { birdT = 2 + Math.random() * 6; sfx.bird(); }
  if (!S.music) return; musT -= dt; if (musT > 0) return; musT = 0.55;
  if (Math.random() < 0.6) { const f = PENTA[(Math.random() * PENTA.length) | 0]; tone(f, f * 0.99, 1.2, 'triangle', 0.035, 0, musicBus); if (Math.random() < 0.3) tone(f / 2, f / 2, 1.6, 'sine', 0.03, 0, musicBus); }
}
function haptic(hand, v, ms) { const s = renderer.xr.getSession(); if (!s) return; for (const src of s.inputSources) if (src.handedness === hand) src.gamepad?.hapticActuators?.[0]?.pulse?.(v, ms); }

/* ================= PANEELID ================= */
const PANELS = [], UIT = [];
function rr(g, x, y, w, h, r) { g.beginPath(); g.moveTo(x + r, y); g.arcTo(x + w, y, x + w, y + h, r); g.arcTo(x + w, y + h, x, y + h, r); g.arcTo(x, y + h, x, y, r); g.arcTo(x, y, x + w, y, r); g.closePath(); }
function txt(g, s, x, y, size, col, align = 'left', bold = false) { g.font = `${bold ? 'bold ' : ''}${size}px system-ui,sans-serif`; g.fillStyle = col; g.textAlign = align; g.textBaseline = 'middle'; g.fillText(s, x, y); }
function fitTxt(g, s, x, y, size, col, maxW, align = 'center', bold = true) { let fs = size; g.font = `${bold ? 'bold ' : ''}${fs}px system-ui`; while (g.measureText(s).width > maxW && fs > 14) { fs -= 2; g.font = `${bold ? 'bold ' : ''}${fs}px system-ui`; } txt(g, s, x, y, fs, col, align, bold); }
function woodBg(g, W, H, title) {
  rr(g, 6, 6, W - 12, H - 12, 28); g.fillStyle = '#fbf4e4'; g.fill(); g.lineWidth = 12; g.strokeStyle = '#8a5a30'; g.stroke();
  if (title) { rr(g, 6, 6, W - 12, 100, 28); g.fillStyle = '#8a5a30'; g.fill(); g.fillRect(6, 60, W - 12, 46); txt(g, title, W / 2, 58, 52, '#fff6e0', 'center', true); }
}
class Panel {
  constructor({ w, h, W, H, draw, sig = null, interactive = true, overlay = false }) {
    Object.assign(this, { W, H, draw, sig, last: '', buttons: [], hover: -1, hoverNext: -1 });
    this.c = cnv(W, H); this.g = this.c.getContext('2d'); this.t = ctex(this.c, false);
    this.mesh = new THREE.Mesh(new THREE.PlaneGeometry(w, h), new THREE.MeshBasicMaterial({ map: this.t, transparent: true, toneMapped: false, fog: false, depthTest: !overlay, depthWrite: !overlay }));
    if (overlay) this.mesh.renderOrder = 1000; this.mesh.userData.panel = this; PANELS.push(this); if (interactive) UIT.push(this.mesh); this.refresh();
  }
  refresh() { this.g.clearRect(0, 0, this.W, this.H); this.buttons = []; this.draw(this.g, this); this.t.needsUpdate = true; }
  check() { if (!this.sig) return; const s = this.sig(); if (s !== this.last) { this.last = s; this.refresh(); } }
  btn(x, y, w, h, label, fn, o = {}) {
    const i = this.buttons.length; this.buttons.push({ x, y, w, h, fn, dis: !!o.dis }); const g = this.g; const hov = this.hover === i && !o.dis;
    const col = o.col || '#c87a3a';
    rr(g, x, y, w, h, Math.min(20, h / 3)); g.fillStyle = o.dis ? '#ddd6cc' : o.on ? col : hov ? '#ffe2b8' : '#fff'; g.fill();
    g.lineWidth = hov || o.on ? 7 : 4; g.strokeStyle = o.dis ? '#b8b0a4' : col; g.stroke();
    if (o.swatch) { rr(g, x + 12, y + 12, w - 24, h - 24, 12); g.fillStyle = o.swatch; g.fill(); }
    const fs = o.fs || 40, tc = o.dis ? '#9a9088' : o.on ? '#fff' : '#3a2a1a';
    if (label) fitTxt(g, label, x + w / 2, y + h / 2 + (o.sub ? -fs * 0.3 : 0), fs, o.swatch ? (o.dark ? '#fff' : '#222') : tc, w - 16);
    if (o.sub) fitTxt(g, o.sub, x + w / 2, y + h / 2 + fs * 0.5, fs * 0.6, o.on ? '#fff' : '#7a6a5a', w - 16, 'center', false);
  }
  indexAt(uv) { const x = uv.x * this.W, y = (1 - uv.y) * this.H; return this.buttons.findIndex(b => x >= b.x && x <= b.x + b.w && y >= b.y && y <= b.y + b.h); }
  click(uv) { const i = this.indexAt(uv); if (i < 0) return; const b = this.buttons[i]; if (b.dis) { sfx.deny(); return; } sfx.click(); b.fn(); for (const p of PANELS) p.last = ''; }
}
function placeFacing(p, x, y, z) { p.mesh.position.set(x, y, z); p.mesh.lookAt(0, y, 26); scene.add(p.mesh); }
const RECIPES = [
  { tool: 'kirves', tier: 1, name: 'Puukirves', cost: { puit: 3 } }, { tool: 'kirka', tier: 1, name: 'Puukirka', cost: { puit: 4 } },
  { tool: 'haamer', tier: 1, name: 'Haamer', cost: { puit: 2, kivi: 2 } }, { tool: 'saag', tier: 1, name: 'Saag', cost: { puit: 3, kivi: 1 } },
  { tool: 'labidas', tier: 1, name: 'Labidas', cost: { puit: 3 } }, { tool: 'pintsel', tier: 1, name: 'Pintsel ja värvid', cost: { puit: 1, liiv: 2 } },
  { tool: 'kirves', tier: 2, name: 'Kivikirves', cost: { puit: 2, kivi: 4 } }, { tool: 'kirka', tier: 2, name: 'Kivikirka', cost: { puit: 2, kivi: 4 } },
  { tool: 'kirves', tier: 3, name: 'Raudkirves', cost: { puit: 2, raud: 4 } }, { tool: 'kirka', tier: 3, name: 'Raudkirka', cost: { puit: 2, raud: 4 } },
];
const DUR = { kirves: [0, 25, 50, 100], kirka: [0, 25, 50, 100], haamer: [0, 300], saag: [0, 200], labidas: [0, 60], pintsel: [0, 400] };
let msgPanel, wrist, hintSprite;
function resLine() { return easy() ? 'Materjale on lõputult' : `Puit ${S.res.puit} · Kivi ${S.res.kivi} · Liiv ${S.res.liiv} · Raud ${S.res.raud}`; }
function buildPanels() {
  // töölaud
  placeFacing(new Panel({ w: 1.3, h: 1.4, W: 1024, H: 1100, sig: () => [S.mode, JSON.stringify(S.res), JSON.stringify(S.tools)].join('|'), draw: (g, P) => {
    woodBg(g, P.W, P.H, 'TÖÖLAUD'); fitTxt(g, resLine(), P.W / 2, 140, 38, '#5a3a1a', P.W - 60);
    if (easy()) { txt(g, 'Lihtsas režiimis on kõik', P.W / 2, 420, 44, '#5a3a1a', 'center', true); txt(g, 'tööriistad juba olemas!', P.W / 2, 480, 44, '#5a3a1a', 'center', true); txt(g, 'Vaheta raskust seadetes, et', P.W / 2, 600, 36, '#7a6a5a', 'center'); txt(g, 'tööriistu ise meisterdada.', P.W / 2, 650, 36, '#7a6a5a', 'center'); return; }
    RECIPES.forEach((rc, i) => { const y = 180 + i * 90; const own = S.tools[rc.tool] >= rc.tier; const can = Object.entries(rc.cost).every(([k, v]) => S.res[k] >= v); const lower = S.tools[rc.tool] < rc.tier - 1;
      txt(g, rc.name, 50, y + 30, 38, '#3a2a1a', 'left', true); txt(g, costTxt(rc.cost), 50, y + 66, 26, '#7a6a5a');
      P.btn(P.W - 330, y + 6, 280, 76, own ? 'Olemas ✓' : 'Meisterda', () => craft(rc), { dis: own || !can || lower, on: own, col: own ? '#5a9a3a' : '#c87a3a', fs: 34 }); });
  } }), -4.6, 1.75, 24.0);
  // projektid
  placeFacing(new Panel({ w: 1.3, h: 1.2, W: 1024, H: 945, sig: () => [S.project, S.done.join(), projSig()].join('|'), draw: (g, P) => {
    woodBg(g, P.W, P.H, 'PROJEKTID');
    PROJECTS.forEach((p, i) => { const y = 130 + i * 140; const pr = projProgress(p); const done = S.done.includes(p.id);
      txt(g, p.name + (done ? ' ✓' : ''), 50, y + 40, 42, '#3a2a1a', 'left', true); txt(g, p.desc, 50, y + 82, 26, '#7a6a5a');
      rr(g, 50, y + 104, 520, 18, 9); g.fillStyle = '#e8dccb'; g.fill(); if (pr > 0) { rr(g, 50, y + 104, Math.max(18, 520 * pr), 18, 9); g.fillStyle = done ? '#5a9a3a' : '#c87a3a'; g.fill(); }
      txt(g, Math.round(pr * 100) + '%', 590, y + 113, 26, '#5a3a1a', 'left', true);
      P.btn(P.W - 300, y + 18, 250, 96, S.project === p.id ? 'Valitud' : 'Vali', () => { S.project = S.project === p.id ? null : p.id; dirtySave = true; refreshBlueprint(); if (S.project) say(`Projekt: ${p.name}. Ehita sinise plaani järgi!`); }, { on: S.project === p.id, col: '#3e7ec8', fs: 38 }); });
  } }), 4.6, 1.65, 24.0);
  // abi
  placeFacing(new Panel({ w: 1.5, h: 1.2, W: 1024, H: 820, interactive: false, draw: (g, P) => {
    woodBg(g, P.W, P.H, 'KUIDAS MÄNGIDA'); const L = ['Vasaku käe randmel on menüü: tööriistad ja klotsid.', 'Parem päästik: kasuta tööriista.', 'Hoia päästikut ja liiguta – ehitad terve seina!', 'Vasak päästik: lammuta klots.', 'A: pööra klotsi · B: lendamine sees / väljas', 'X: järgmine klots · Y: järgmine tööriist', 'Parem haare: ava uks / võta klotsi tüüp', 'Projektid: vali maja ja ehita sinise plaani järgi.', 'Tavaline: raiu puid, kaeva kive ja liiva,', 'ning meisterda töölaual tööriistu.'];
    L.forEach((s, i) => txt(g, s, 50, 150 + i * 64, 35, '#3a2a1a'));
  } }), -3.4, 1.6, 30.4);
  // seaded
  placeFacing(new Panel({ w: 1.3, h: 1.15, W: 1024, H: 900, sig: () => [S.mode, S.fly, S.grid, S.shadows, S.music, R.resetArm > 0].join('|'), draw: (g, P) => {
    woodBg(g, P.W, P.H, 'SEADED'); txt(g, 'Raskus', 50, 150, 40, '#3a2a1a', 'left', true);
    [['easy', 'Lihtne', 'kõik on olemas'], ['normal', 'Tavaline', 'kogu ja meisterda'], ['hard', 'Raske', 'tööriistad kuluvad']].forEach(([m, n, d], i) => P.btn(50 + i * 315, 180, 295, 130, n, () => setMode(m), { on: S.mode === m, sub: d, col: '#5a9a3a', fs: 42 }));
    const tg = (x, y, label, v, fn) => P.btn(x, y, 445, 110, label, fn, { on: v, sub: v ? 'sees' : 'väljas', col: '#3e7ec8', fs: 40 });
    tg(50, 350, 'Lendamine', S.fly, () => { S.fly = !S.fly; dirtySave = true; });
    tg(529, 350, 'Ruudustik', S.grid, () => { S.grid = !S.grid; WORLD.grid.visible = S.grid; dirtySave = true; });
    tg(50, 480, 'Varjud', S.shadows, () => { S.shadows = !S.shadows; sun.castShadow = S.shadows; dirtySave = true; });
    tg(529, 480, 'Muusika', S.music, () => { S.music = !S.music; initAudio(); dirtySave = true; });
    P.btn(50, 640, 924, 120, R.resetArm > 0 ? 'KINDEL? Vajuta uuesti' : 'Alusta uuesti (kustutab kõik)', resetGame, { col: '#c84a3a', fs: 40 });
  } }), 3.4, 1.55, 30.4);
  // randmemenüü
  wrist = new Panel({ w: 0.27, h: 0.29, W: 1000, H: 1080, sig: () => [R.tool, S.block, S.paint, S.mode, S.fly, JSON.stringify(S.res), JSON.stringify(S.tools), R.rot, JSON.stringify(S.dur)].join('|'), draw: (g, P) => {
    rr(g, 4, 4, P.W - 8, P.H - 8, 30); g.fillStyle = 'rgba(251,244,228,0.97)'; g.fill(); g.lineWidth = 10; g.strokeStyle = '#8a5a30'; g.stroke();
    fitTxt(g, resLine(), P.W / 2, 50, 36, '#5a3a1a', P.W - 50);
    TOOLS.forEach(([t, n], i) => { const own = has(t); P.btn(24 + (i % 3) * 320, 90 + ((i / 3) | 0) * 116, 308, 104, n, () => selectTool(t), { on: R.tool === t, dis: !own, col: '#c87a3a', fs: 38, sub: own ? (['kirves', 'kirka'].includes(t) ? TIER_NAME[tier(t)] : (S.mode === 'hard' && S.dur[t] !== undefined ? `${S.dur[t]} korda` : null)) : 'puudub' }); });
    const y0 = 330;
    if (R.tool === 'haamer') {
      BORDER.forEach((t, i) => P.btn(24 + (i % 4) * 240, y0 + ((i / 4) | 0) * 118, 228, 106, BT[t].name, () => { S.block = t; dirtySave = true; }, { on: S.block === t, col: '#3e7ec8', fs: 30, sub: easy() ? null : costTxt(BT[t].cost) }));
    } else if (R.tool === 'pintsel') {
      PAINT.forEach(([n, c], i) => P.btn(24 + (i % 4) * 240, y0 + ((i / 4) | 0) * 160, 228, 148, n, () => { S.paint = i; dirtySave = true; }, { on: S.paint === i, swatch: c, dark: i === 1 || i === 4 || i === 5 || i === 6 || i === 7, col: '#3e7ec8', fs: 32 }));
      txt(g, 'Hoia päästikut ja värvi!', P.W / 2, y0 + 380, 36, '#5a3a1a', 'center', true);
    } else {
      const info = { saag: ['Saag lõikab klotse ära.', 'Saad materjali tagasi.'], kirves: ['Mine puu juurde ja', 'hoia päästikut. Saad puitu.'], kirka: ['Mine kivi juurde ja hoia päästikut.', 'Raud vajab kivikirkat.'], labidas: ['Mine järve äärde liivale', 'ja kaeva liiva.'] }[R.tool] || ['', ''];
      txt(g, info[0], P.W / 2, y0 + 120, 38, '#3a2a1a', 'center', true); txt(g, info[1], P.W / 2, y0 + 180, 38, '#3a2a1a', 'center', true);
      if (!easy()) txt(g, 'Uusi tööriistu tee töölaual.', P.W / 2, y0 + 280, 32, '#7a6a5a', 'center');
    }
    const yb = P.H - 140;
    P.btn(24, yb, 308, 116, 'Lenda', () => { S.fly = !S.fly; R.vy = 0; dirtySave = true; say(S.fly ? 'Lendamine sees' : 'Lendamine väljas'); }, { on: S.fly, col: '#5a9a3a', fs: 40 });
    P.btn(346, yb, 308, 116, 'Pööra', () => { R.rot = (R.rot + 1) % 4; }, { col: '#3e7ec8', fs: 40, sub: `${R.rot * 90}°` });
    P.btn(668, yb, 308, 116, 'Tagasi', undo, { col: '#c87a3a', fs: 40, sub: 'võta tagasi' });
  } });
  wrist.mesh.position.set(0, 0.06, 0.06); wrist.mesh.rotation.set(-0.85, 0, 0);
  msgPanel = new Panel({ w: 1.4, h: 0.2, W: 1024, H: 144, interactive: false, overlay: true, draw: (g, P) => { if (!R.msg) return; rr(g, 8, 8, P.W - 16, P.H - 16, 34); g.fillStyle = 'rgba(251,244,228,0.95)'; g.fill(); g.lineWidth = 6; g.strokeStyle = '#8a5a30'; g.stroke(); fitTxt(g, R.msg, P.W / 2, P.H / 2 + 2, 48, '#3a2a1a', P.W - 70); } });
  msgPanel.mesh.position.set(0, -0.32, -1.5); msgPanel.mesh.visible = false; camera.add(msgPanel.mesh);
  const hc = cnv(512, 96); hintSprite = new THREE.Sprite(new THREE.SpriteMaterial({ map: ctex(hc, false), transparent: true, depthTest: false, fog: false })); hintSprite.userData.c = hc; hintSprite.scale.set(1.6, 0.3, 1); hintSprite.renderOrder = 900; hintSprite.visible = false; scene.add(hintSprite);
}
function setHint(text, pos) {
  if (!text) { hintSprite.visible = false; return; } hintSprite.visible = true; hintSprite.position.copy(pos);
  if (R.hint !== text) { R.hint = text; const c = hintSprite.userData.c, g = c.getContext('2d'); g.clearRect(0, 0, 512, 96); rr(g, 4, 4, 504, 88, 30); g.fillStyle = 'rgba(62,126,200,0.92)'; g.fill(); fitTxt(g, text, 256, 50, 46, '#fff', 480); hintSprite.material.map.needsUpdate = true; }
}

/* ================= SÕNUMID JA TEGEVUSED ================= */
function say(t) { if (R.msg === t && R.msgT > 0) { R.msgT = 3.5; return; } if (R.msgT > 1.5) { if (!R.queue.includes(t)) { R.queue.push(t); if (R.queue.length > 3) R.queue.shift(); } return; } R.msg = t; R.msgT = 3.5; msgPanel.refresh(); msgPanel.mesh.visible = true; }
function updateMsg(dt) { if (R.msgT > 0) { R.msgT -= dt; msgPanel.mesh.material.opacity = clamp(R.msgT * 2, 0, 1); if (R.msgT <= 0) { msgPanel.mesh.visible = false; R.msg = ''; } } else if (R.queue.length) { const t = R.queue.shift(); R.msg = t; R.msgT = 3.5; msgPanel.refresh(); msgPanel.mesh.visible = true; } }
function selectTool(t) { if (!has(t)) { say(`${TOOL_NAME[t]} puudub – meisterda see töölaual!`); sfx.deny(); return; } R.tool = t; S.tool = t; R.drag = null; R.working = false; updateToolModel(); dirtySave = true; }
function cycleTool() { const own = TOOLS.map(x => x[0]).filter(has); if (!own.length) return; const i = own.indexOf(R.tool); selectTool(own[(i + 1) % own.length]); say(TOOL_NAME[R.tool]); }
function cycleBlock(d = 1) { if (R.tool === 'pintsel') { S.paint = (S.paint + d + PAINT.length) % PAINT.length; say('Värv: ' + PAINT[S.paint][0]); return; } if (R.tool !== 'haamer' && has('haamer')) selectTool('haamer'); S.block = BORDER[(BORDER.indexOf(S.block) + d + BORDER.length) % BORDER.length]; say(BT[S.block].name); dirtySave = true; }
function setMode(m) { S.mode = m; if (m !== 'easy' && !has(R.tool)) { const own = TOOLS.map(x => x[0]).find(has); if (own) selectTool(own); } updateToolModel(); dirtySave = true; say({ easy: 'Lihtne: kõik materjalid ja tööriistad on olemas!', normal: 'Tavaline: raiu puid ja meisterda tööriistu.', hard: 'Raske: tööriistad kuluvad, materjali tuleb vähem.' }[m]); }
function craft(rc) { if (!Object.entries(rc.cost).every(([k, v]) => S.res[k] >= v)) return; for (const [k, v] of Object.entries(rc.cost)) S.res[k] -= v; S.tools[rc.tool] = rc.tier; S.dur[rc.tool] = DUR[rc.tool][rc.tier]; sfx.fanfare(); say(`Valmis: ${rc.name}!`); if (rc.tool === R.tool || ['haamer'].includes(rc.tool)) updateToolModel(); dirtySave = true; save(); }
function wear(t, n = 1) { if (S.mode !== 'hard' || S.dur[t] === undefined) return; S.dur[t] -= n; if (S.dur[t] <= 0) { S.tools[t] = 0; delete S.dur[t]; sfx.deny(); say(`${TOOL_NAME[t]} läks katki! Tee töölaual uus.`); if (R.tool === t) { R.drag = null; R.working = false; const own = TOOLS.map(x => x[0]).find(has); if (own) selectTool(own); } } }
function resetGame() { if (R.resetArm <= 0) { R.resetArm = 4; return; } localStorage.removeItem(SAVE); location.reload(); }
const afford = (c, n = 1) => easy() || Object.entries(c).every(([k, v]) => S.res[k] >= v * n);
function pay(c, s = 1) { if (easy()) return; for (const [k, v] of Object.entries(c)) S.res[k] = Math.max(0, S.res[k] - v * s); }
const yieldK = () => S.mode === 'hard' ? 0.6 : 1;

/* ================= PROJEKTID ================= */
function B() { const m = new Map(); return { m, put: (i, j, k, t, r = 0) => m.set(key(i, j, k), { i, j, k, t, r }), del: (i, j, k) => m.delete(key(i, j, k)) }; }
function ring(b, x0, z0, W, D, k0, k1, t) { for (let k = k0; k < k1; k++) for (let x = x0; x < x0 + W; x++) for (let z = z0; z < z0 + D; z++) if (x === x0 || x === x0 + W - 1 || z === z0 || z === z0 + D - 1) b.put(x, z, k, t); }
function floor(b, x0, z0, W, D, k, t) { for (let x = x0; x < x0 + W; x++) for (let z = z0; z < z0 + D; z++) b.put(x, z, k, t); }
function gable(b, x0, z0, W, D, k0, wallT) {
  let top = k0;
  for (let m = 0; ; m++) {
    const back = z0 - 1 + m, front = z0 + D - m, k = k0 + m; if (back > front) break; top = k;
    if (back === front) { for (let x = x0 - 1; x <= x0 + W; x++) b.put(x, back, k, 'hari', 0); break; }
    for (let x = x0 - 1; x <= x0 + W; x++) { b.put(x, back, k, 'katus', 0); b.put(x, front, k, 'katus', 2); }
    for (let z = Math.max(back + 1, z0); z <= Math.min(front - 1, z0 + D - 1); z++) { b.put(x0, z, k, wallT); b.put(x0 + W - 1, z, k, wallT); }
  }
  return top;
}
function door(b, x, z, k, r = 0) { b.put(x, z, k, 'uks', r); b.del(x, z, k + 1); }
const PROJECTS = [
  { id: 'kuur', name: 'Puukuur', desc: 'väike laudadest kuur', build: () => { const b = B(), x0 = -14, z0 = -12; ring(b, x0, z0, 3, 3, 0, 2, 'laud'); door(b, x0 + 1, z0 + 2, 0); gable(b, x0, z0, 3, 3, 2, 'laud'); return b; } },
  { id: 'aiamaja', name: 'Aiamaja', desc: 'akendega laudmaja', build: () => { const b = B(), x0 = -15, z0 = -1; floor(b, x0 + 1, z0 + 1, 3, 2, 0, 'porand'); ring(b, x0, z0, 5, 4, 0, 3, 'laud'); door(b, x0 + 2, z0 + 3, 0); b.put(x0 + 1, z0 + 3, 1, 'aken'); b.put(x0 + 3, z0 + 3, 1, 'aken'); b.put(x0, z0 + 1, 1, 'aken', 1); b.put(x0 + 4, z0 + 2, 1, 'aken', 1); gable(b, x0, z0, 5, 4, 3, 'laud'); return b; } },
  { id: 'saun', name: 'Saun', desc: 'palksaun korstnaga', build: () => { const b = B(), x0 = 8, z0 = -12; ring(b, x0, z0, 5, 4, 0, 3, 'palk'); door(b, x0 + 1, z0 + 3, 0); b.put(x0 + 3, z0 + 3, 1, 'aken'); b.put(x0 + 4, z0 + 1, 1, 'aken', 1); const top = gable(b, x0, z0, 5, 4, 3, 'palk'); for (let k = 3; k <= top + 1; k++) b.put(x0 + 3, z0 + 1, k, 'tellis'); return b; } },
  { id: 'garaaz', name: 'Garaaž', desc: 'telliskivist, lame katus', build: () => { const b = B(), x0 = 9, z0 = 0; ring(b, x0, z0, 4, 6, 0, 3, 'tellis'); door(b, x0 + 1, z0 + 5, 0); door(b, x0 + 2, z0 + 5, 0); b.put(x0, z0 + 3, 1, 'aken', 1); floor(b, x0, z0, 4, 6, 3, 'porand'); return b; } },
  { id: 'talumaja', name: 'Talumaja', desc: 'suur palkmaja kivivundamendil', build: () => { const b = B(), x0 = -4, z0 = -12, W = 8, D = 7; ring(b, x0, z0, W, D, 0, 1, 'kivi'); floor(b, x0 + 1, z0 + 1, W - 2, D - 2, 1, 'porand'); ring(b, x0, z0, W, D, 1, 4, 'palk'); door(b, x0 + 3, z0 + D - 1, 1); b.put(x0 + 3, z0 + D, 0, 'trepp', 2);
    for (const x of [x0 + 1, x0 + 5, x0 + 6]) b.put(x, z0 + D - 1, 2, 'aken'); for (const x of [x0 + 2, x0 + 5]) b.put(x, z0, 2, 'aken'); b.put(x0, z0 + 3, 2, 'aken', 1); b.put(x0 + W - 1, z0 + 3, 2, 'aken', 1);
    const top = gable(b, x0, z0, W, D, 4, 'palk'); for (let k = 1; k <= top + 1; k++) b.put(x0 + 5, z0 + 3, k, 'tellis'); return b; } },
];
for (const p of PROJECTS) p.cells = [...p.build().m.values()];
function projProgress(p) { let n = 0; for (const c of p.cells) { const b = getRaw(c.i, c.j, c.k); if (b && b.t === c.t) n++; } return n / p.cells.length; }
let projSigCache = '';
function projSig() { return projSigCache; }
let bpIM;
function buildBlueprintMesh() { bpIM = new THREE.InstancedMesh(new THREE.BoxGeometry(0.94, 0.94, 0.94).translate(0, 0.5, 0), new THREE.MeshBasicMaterial({ color: '#ffffff', transparent: true, opacity: 0.32, depthWrite: false, fog: false }), 700); bpIM.count = 0; bpIM.frustumCulled = false; bpIM.renderOrder = 5; scene.add(bpIM); }
const BP_COL = { palk: '#c88a4a', laud: '#e05a40', krohv: '#ffffff', kivi: '#8a8a8a', tellis: '#c85a3a', aken: '#60d0ff', uks: '#ffd040', porand: '#e0b070', katus: '#7a6aff', hari: '#a080ff', trepp: '#f0c080', aed: '#ffffff', tala: '#a06030' };
let BP_ACTIVE = new Map();
function refreshBlueprint() {
  BP_ACTIVE = new Map(); bpIM.count = 0; projSigCache = PROJECTS.map(p => Math.round(projProgress(p) * 100)).join(',');
  const p = PROJECTS.find(p => p.id === S.project); if (!p) return;
  const todo = p.cells.filter(c => { const b = getRaw(c.i, c.j, c.k); return !b || b.t !== c.t; });
  if (!todo.length) { if (!S.done.includes(p.id)) completeProject(p); return; }
  const minK = Math.min(...todo.map(c => c.k)); let n = 0;
  for (const c of todo) { if (c.k > minK + 0) continue; BP_ACTIVE.set(key(c.i, c.j, c.k), c); const wrong = !!getRaw(c.i, c.j, c.k);
    _m.makeTranslation(c.i + 0.5, c.k, c.j + 0.5); bpIM.setMatrixAt(n, _m); bpIM.setColorAt(n, _c.set(wrong ? '#ff3030' : BP_COL[c.t])); n++; }
  bpIM.count = n; bpIM.instanceMatrix.needsUpdate = true; if (bpIM.instanceColor) bpIM.instanceColor.needsUpdate = true;
}
function completeProject(p) {
  S.done.push(p.id); sfx.fanfare(); setTimeout(sfx.fanfare, 500); say(`Projekt valmis: ${p.name}! Tubli meister!`);
  if (!easy()) { for (const k in S.res) S.res[k] += 10; say('Preemia: +10 igat materjali!'); }
  const c = p.cells.reduce((s, c) => s.add(_v.set(c.i + 0.5, c.k, c.j + 0.5)), new V3()).multiplyScalar(1 / p.cells.length);
  for (let i = 0; i < 6; i++) setTimeout(() => firework(c.clone().add(new V3(rnd(-3, 3), rnd(6, 10), rnd(-3, 3)))), i * 350);
  save();
}

/* ================= EFEKTID ================= */
const PARTS = []; let partIM; const POPS = [];
function buildFX() {
  partIM = new THREE.InstancedMesh(new THREE.BoxGeometry(1, 1, 1), new THREE.MeshBasicMaterial({ color: '#ffffff', fog: false }), 400); partIM.count = 400; partIM.frustumCulled = false; scene.add(partIM);
  for (let i = 0; i < 400; i++) { PARTS.push({ p: new V3(), v: new V3(), life: 0, s: 0.05, g: 9, c: new THREE.Color() }); partIM.setMatrixAt(i, _m.makeScale(0, 0, 0)); }
  for (let i = 0; i < 6; i++) { const c = cnv(512, 128); const s = new THREE.Sprite(new THREE.SpriteMaterial({ map: ctex(c, false), transparent: true, depthTest: false, fog: false })); s.userData.c = c; s.scale.set(1.2, 0.3, 1); s.visible = false; s.renderOrder = 950; scene.add(s); POPS.push({ s, life: 0 }); }
}
function burst(pos, color, n, speed, size = 0.06, g = 9, life = 1) { let k = 0; for (const p of PARTS) { if (p.life > 0) continue; p.p.copy(pos); p.v.set(rnd(-1, 1), rnd(0.2, 1.2), rnd(-1, 1)).multiplyScalar(speed); p.life = life * rnd(0.6, 1); p.s = size * rnd(0.6, 1.4); p.g = g; p.c.set(color); if (++k >= n) break; } }
function firework(pos) { const cols = ['#ff4040', '#ffd040', '#40c0ff', '#60ff60', '#ff60ff', '#ffffff']; const col = cols[(Math.random() * cols.length) | 0]; let k = 0; for (const p of PARTS) { if (p.life > 0) continue; p.p.copy(pos); p.v.set(rnd(-1, 1), rnd(-1, 1), rnd(-1, 1)).normalize().multiplyScalar(rnd(5, 8)); p.life = rnd(1.2, 1.8); p.s = 0.14; p.g = 2; p.c.set(col); if (++k >= 70) break; } tone(200, 60, 0.6, 'sine', 0.3); noise(0.5, 0.4, 'lowpass', 2000, 100); }
function pop(pos, text, col = '#5a3a1a') { const p = POPS.find(p => p.life <= 0) || POPS[0]; const g = p.s.userData.c.getContext('2d'); g.clearRect(0, 0, 512, 128); rr(g, 6, 14, 500, 100, 40); g.fillStyle = 'rgba(255,250,236,0.92)'; g.fill(); fitTxt(g, text, 256, 66, 60, col, 480); p.s.material.map.needsUpdate = true; p.s.position.copy(pos); p.life = 1.8; p.s.visible = true; }
function updateFX(dt) {
  PARTS.forEach((p, i) => { if (p.life <= 0) return; p.life -= dt; if (p.life <= 0) { partIM.setMatrixAt(i, _m.makeScale(0, 0, 0)); return; } p.v.y -= p.g * dt; p.p.addScaledVector(p.v, dt); if (p.p.y < 0.02 && p.g > 5) { p.p.y = 0.02; p.v.multiplyScalar(0.3); } partIM.setMatrixAt(i, _m.compose(p.p, _q.identity(), _v.setScalar(p.s * Math.min(1, p.life * 2)))); partIM.setColorAt(i, p.c); });
  partIM.instanceMatrix.needsUpdate = true; if (partIM.instanceColor) partIM.instanceColor.needsUpdate = true;
  for (const p of POPS) { if (p.life <= 0) continue; p.life -= dt; p.s.position.y += dt * 0.5; p.s.material.opacity = clamp(p.life, 0, 1); if (p.life <= 0) p.s.visible = false; }
}

/* ================= MÄNGIJA JA TÖÖRIISTAD ================= */
const rig = new THREE.Group(); rig.position.set(0, 0, 26); scene.add(rig); rig.add(camera);
const toolHolder = new THREE.Group(); let toolModel = null;
function toolMesh(t) {
  const g = new THREE.Group(); const wood = new THREE.MeshStandardMaterial({ color: '#a0703e', roughness: 0.8 });
  const metal = new THREE.MeshStandardMaterial({ color: ['#c8a070', '#b0a898', '#8a8e96', '#d0d4d8'][tier(t)] || '#8a8e96', roughness: 0.35, metalness: 0.7 });
  const handle = (len) => { const h = new THREE.Mesh(new THREE.CylinderGeometry(0.016, 0.018, len, 8).rotateX(PI / 2).translate(0, 0, -len / 2 + 0.06), wood); g.add(h); };
  if (t === 'haamer') { handle(0.34); const h = new THREE.Mesh(new THREE.BoxGeometry(0.05, 0.13, 0.05), metal); h.position.set(0, 0.02, -0.27); g.add(h); }
  else if (t === 'kirves') { handle(0.45); const h = new THREE.Mesh(new THREE.BoxGeometry(0.02, 0.1, 0.12), metal); h.position.set(0, 0.06, -0.36); g.add(h); const b = new THREE.Mesh(new THREE.BoxGeometry(0.012, 0.16, 0.03), metal); b.position.set(0, 0.06, -0.43); g.add(b); }
  else if (t === 'kirka') { handle(0.45); const h = new THREE.Mesh(new THREE.ConeGeometry(0.02, 0.32, 6).rotateZ(PI / 2), metal); h.position.set(0, 0, -0.38); g.add(h); h.rotation.y = PI / 2; h.rotation.z = 0; const h2 = new THREE.Mesh(new THREE.BoxGeometry(0.03, 0.32, 0.03), metal); h2.position.set(0, 0, -0.38); g.add(h2); }
  else if (t === 'labidas') { handle(0.6); const b = new THREE.Mesh(new THREE.BoxGeometry(0.16, 0.012, 0.2), metal); b.position.set(0, 0, -0.62); g.add(b); }
  else if (t === 'saag') { const h = new THREE.Mesh(new THREE.BoxGeometry(0.03, 0.08, 0.1), wood); g.add(h); const b = new THREE.Mesh(new THREE.BoxGeometry(0.004, 0.1, 0.45), metal); b.position.set(0, -0.01, -0.28); g.add(b); }
  else if (t === 'pintsel') { handle(0.22); const f = new THREE.Mesh(new THREE.BoxGeometry(0.06, 0.02, 0.03), metal); f.position.set(0, 0, -0.17); g.add(f); const br = new THREE.Mesh(new THREE.BoxGeometry(0.06, 0.018, 0.06), new THREE.MeshStandardMaterial({ color: PAINT[S.paint][1] })); br.position.set(0, 0, -0.21); br.userData.brush = true; g.add(br); }
  g.traverse(o => { if (o.isMesh) o.castShadow = true; }); return g;
}
function updateToolModel() { if (toolModel) toolHolder.remove(toolModel); toolModel = has(R.tool) ? toolMesh(R.tool) : null; if (toolModel) toolHolder.add(toolModel); }
// klotsi eelvaade
const ghostMat = new THREE.MeshBasicMaterial({ color: '#60ff80', transparent: true, opacity: 0.45, depthWrite: false, fog: false });
const ghostBad = new THREE.MeshBasicMaterial({ color: '#ff5040', transparent: true, opacity: 0.45, depthWrite: false, fog: false });
const ghosts = {}; let ghostBox, selBox;
function buildGhosts() {
  for (const t of BORDER) { const g = new THREE.Group(); for (const part of TP[t]) { const m = new THREE.Mesh(part.geo, ghostMat); if (part.door) { m.position.x = -0.42; } g.add(m); } g.visible = false; scene.add(g); ghosts[t] = g; }
  ghostBox = new THREE.InstancedMesh(new THREE.BoxGeometry(1.0, 1.0, 1.0).translate(0, 0.5, 0), new THREE.MeshBasicMaterial({ color: '#ffffff', transparent: true, opacity: 0.35, depthWrite: false, fog: false }), 512); ghostBox.count = 0; ghostBox.frustumCulled = false; ghostBox.renderOrder = 6; scene.add(ghostBox);
  selBox = new THREE.LineSegments(new THREE.EdgesGeometry(new THREE.BoxGeometry(1.02, 1.02, 1.02).translate(0, 0.5, 0)), new THREE.LineBasicMaterial({ color: '#ffffff', fog: false, depthTest: false, transparent: true })); selBox.renderOrder = 7; selBox.visible = false; scene.add(selBox);
}
function hideGhosts() { for (const t in ghosts) ghosts[t].visible = false; ghostBox.count = 0; selBox.visible = false; }

/* ================= KIIR JA SIHTIMINE ================= */
const REACH = 14, GATHER_REACH = 4.5;
function gridRay(o, d, maxT) {
  let i = Math.floor(o.x), j = Math.floor(o.z), k = Math.floor(o.y);
  const si = Math.sign(d.x), sj = Math.sign(d.z), sk = Math.sign(d.y);
  const tdx = si ? Math.abs(1 / d.x) : Infinity, tdz = sj ? Math.abs(1 / d.z) : Infinity, tdk = sk ? Math.abs(1 / d.y) : Infinity;
  let tmx = si > 0 ? (i + 1 - o.x) / d.x : si < 0 ? (o.x - i) / -d.x : Infinity;
  let tmz = sj > 0 ? (j + 1 - o.z) / d.z : sj < 0 ? (o.z - j) / -d.z : Infinity;
  let tmk = sk > 0 ? (k + 1 - o.y) / d.y : sk < 0 ? (o.y - k) / -d.y : Infinity;
  for (let s = 0; s < 300; s++) {
    let n, t;
    if (tmx < tmz && tmx < tmk) { i += si; t = tmx; tmx += tdx; n = [-si, 0, 0]; } else if (tmz < tmk) { j += sj; t = tmz; tmz += tdz; n = [0, 0, -sj]; } else { k += sk; t = tmk; tmk += tdk; n = [0, -sk, 0]; }
    if (t > maxT || k < 0) return null;
    if (getRaw(i, j, k)) return { i, j, k, n, t };
  }
  return null;
}
function target(o, d) {
  let best = gridRay(o, d, REACH);
  if (d.y < -1e-4) { const tg = -o.y / d.y; if (tg > 0 && tg < REACH && (!best || tg < best.t)) { const x = o.x + d.x * tg, z = o.z + d.z * tg; best = { ground: true, t: tg, i: Math.floor(x), j: Math.floor(z), k: -1, n: [0, 1, 0], px: x, pz: z }; } }
  if (!best) return null; best.place = { i: best.i + best.n[0], j: best.j + best.n[2], k: best.k + best.n[1] }; return best;
}
function autoRot(d) { let r = Math.abs(d.x) > Math.abs(d.z) ? (d.x > 0 ? 1 : 3) : (d.z > 0 ? 0 : 2); return (r + R.rot) % 4; }
function playerCells() { const p = rig.position, out = new Set(); const cp = camera.getWorldPosition(_v); for (const [x, z] of [[cp.x, cp.z], [p.x, p.z]]) for (const dx of [-0.3, 0.3]) for (const dz of [-0.3, 0.3]) for (let k = Math.floor(p.y); k <= Math.floor(p.y + 1.75); k++) out.add(key(Math.floor(x + dx), Math.floor(z + dz), k)); return out; }
function canPlace(c, t, pc) {
  if (!inPlot(c.i, c.j, c.k) || getRaw(c.i, c.j, c.k) || pc.has(key(c.i, c.j, c.k))) return false;
  if (t === 'uks') { if (!inPlot(c.i, c.j, c.k + 1) || getRaw(c.i, c.j, c.k + 1) || pc.has(key(c.i, c.j, c.k + 1))) return false; }
  return true;
}
function boxCells(a, b) { const out = []; const [i0, i1] = [Math.min(a.i, b.i), Math.max(a.i, b.i)], [j0, j1] = [Math.min(a.j, b.j), Math.max(a.j, b.j)], [k0, k1] = [Math.min(a.k, b.k), Math.max(a.k, b.k)]; for (let k = k0; k <= k1; k++) for (let i = i0; i <= i1; i++) for (let j = j0; j <= j1; j++) out.push({ i, j, k }); return out; }
function domAxis(d) { return Math.abs(d.x) > Math.abs(d.z) ? new V3(1, 0, 0) : new V3(0, 0, 1); }
function startDrag(o, d, h) {
  let start, N;
  if (R.tool === 'haamer') { start = h.place; if ((h.n[1] === 1 && d.y < -0.55) || h.n[1] === -1) N = new V3(0, 1, 0); else N = domAxis(d); }
  else { if (h.ground) return; start = { i: h.i, j: h.j, k: h.k }; N = new V3(Math.abs(h.n[0]), Math.abs(h.n[1]), Math.abs(h.n[2])); }
  R.drag = { start, cur: { ...start }, N, P0: new V3(start.i + 0.5, start.k + 0.5, start.j + 0.5), rot: autoRot(d) };
}
function dragUpdate(o, d) {
  const D = R.drag; const den = d.dot(D.N); if (Math.abs(den) < 1e-3) return;
  const t = _v.copy(D.P0).sub(o).dot(D.N) / den; if (t < 0 || t > 40) return;
  const p = _v2.copy(o).addScaledVector(d, t); const c = { i: Math.floor(p.x), j: Math.floor(p.z), k: Math.floor(p.y) };
  if (D.N.x) c.i = D.start.i; if (D.N.y) c.k = D.start.k; if (D.N.z) c.j = D.start.j;
  c.i = clamp(c.i, D.start.i - 15, D.start.i + 15); c.j = clamp(c.j, D.start.j - 15, D.start.j + 15); c.k = clamp(c.k, Math.max(0, D.start.k - 15), Math.min(MAXK - 1, D.start.k + 15)); D.cur = c;
}
const UNDO = [];
function pushUndo(a) { UNDO.push(a); if (UNDO.length > 40) UNDO.shift(); }
function finishDrag() {
  const D = R.drag; R.drag = null; if (!D) return; const cells = boxCells(D.start, D.cur);
  if (R.tool === 'haamer') {
    const t = S.block, def = BT[t], pc = playerCells(); const placed = []; let lack = false;
    for (const c of cells) { if (!canPlace(c, t, pc)) continue; if (!afford(def.cost)) { lack = true; break; } pay(def.cost); addBlock(c.i, c.j, c.k, t, D.rot); placed.push({ i: c.i, j: c.j, k: c.k, t, r: D.rot, c: def.paint ?? 0 }); }
    if (placed.length) { pushUndo({ type: 'place', items: placed }); sfx.hammer(); setTimeout(sfx.pop, 80); haptic('right', 0.5, 40); wear('haamer', placed.length); const c = placed[placed.length - 1]; burst(_v.set(c.i + 0.5, c.k + 0.1, c.j + 0.5), '#d8c8a8', 8, 2, 0.04); }
    if (lack) { say(`Materjali ei jätku! ${def.name}: ${costTxt(def.cost)}`); sfx.deny(); }
    else if (!placed.length) sfx.deny();
  } else if (R.tool === 'saag') {
    const removed = []; const seen = new Set();
    for (const c of cells) { const raw = getRaw(c.i, c.j, c.k); if (!raw) continue; const kk = raw.t === '_' ? raw.ref : key(c.i, c.j, c.k); if (seen.has(kk)) continue; seen.add(kk); const [i, j, k] = kk.split(',').map(Number); const b = removeAt(i, j, k); if (b) { removed.push(b); if (!easy()) for (const [rk, v] of Object.entries(BT[b.t].cost)) S.res[rk] += v; burst(_v.set(i + 0.5, k + 0.5, j + 0.5), '#c8a070', 10, 3, 0.05); } }
    if (removed.length) { pushUndo({ type: 'remove', items: removed }); sfx.saw(); haptic('right', 0.6, 120); wear('saag', removed.length); }
  } else if (R.tool === 'pintsel') {
    const items = []; const seen = new Set();
    for (const c of cells) { const raw = getRaw(c.i, c.j, c.k); if (!raw) continue; const kk = raw.t === '_' ? raw.ref : key(c.i, c.j, c.k); if (seen.has(kk)) continue; seen.add(kk); const b = BLOCKS.get(kk); if (!b || BT[b.t].paint === undefined || b.c === S.paint) continue; items.push({ kk, old: b.c }); b.c = S.paint; DIRTY.add(b.t); }
    if (items.length) { pushUndo({ type: 'paint', items }); sfx.paint(); haptic('right', 0.3, 60); wear('pintsel', items.length); dirtySave = true; } else sfx.deny();
  }
  refreshBlueprint();
}
function removeOne(h) {
  if (!h || h.ground) return; if (!easy() && !has('saag')) { if (R.time - R.lastLack > 2) { R.lastLack = R.time; say('Lammutamiseks on vaja saagi – tee see töölaual!'); } return; }
  const b = removeAt(h.i, h.j, h.k); if (!b) return; if (!easy()) for (const [rk, v] of Object.entries(BT[b.t].cost)) S.res[rk] += v;
  pushUndo({ type: 'remove', items: [b] }); sfx.saw(); haptic('left', 0.5, 80); burst(_v.set(b.i + 0.5, b.k + 0.5, b.j + 0.5), '#c8a070', 10, 3, 0.05); wear('saag'); refreshBlueprint();
}
function undo() {
  const a = UNDO.pop(); if (!a) { say('Pole midagi tagasi võtta'); return; }
  if (a.type === 'place') for (const it of a.items) { const b = removeAt(it.i, it.j, it.k); if (b && !easy()) for (const [rk, v] of Object.entries(BT[b.t].cost)) S.res[rk] += v; }
  else if (a.type === 'remove') for (const it of a.items) { if (getRaw(it.i, it.j, it.k)) continue; if (!afford(BT[it.t].cost)) continue; pay(BT[it.t].cost); addBlock(it.i, it.j, it.k, it.t, it.r, it.c, it.o); }
  else if (a.type === 'paint') for (const it of a.items) { const b = BLOCKS.get(it.kk); if (b) { b.c = it.old; DIRTY.add(b.t); } }
  sfx.pop(); say('Võetud tagasi'); refreshBlueprint();
}
function interact(o, d) {
  const h = gridRay(o, d, 6); if (!h) return; const raw = getRaw(h.i, h.j, h.k); const kk = raw.t === '_' ? raw.ref : key(h.i, h.j, h.k); const b = BLOCKS.get(kk);
  if (b && b.t === 'uks') { b.o = !b.o; DIRTY.add('uks'); sfx.door(); dirtySave = true; return; }
  if (b) { if (has('haamer')) { selectTool('haamer'); S.block = b.t; say('Valitud: ' + BT[b.t].name); } }
}
// kogumine
function rayCyl(o, d, x, z, rad, h) { const ox = o.x - x, oz = o.z - z; const a = d.x * d.x + d.z * d.z; if (a < 1e-6) return -1; const b = 2 * (ox * d.x + oz * d.z); const c = ox * ox + oz * oz - rad * rad; const disc = b * b - 4 * a * c; if (disc < 0) return -1; const t = (-b - Math.sqrt(disc)) / (2 * a); if (t < 0) return -1; const y = o.y + d.y * t; return y < 0 || y > h ? -1 : t; }
function raySphere(o, d, c, r) { const L = _v.copy(c).sub(o); const tc = L.dot(d); if (tc < 0) return -1; const d2 = L.lengthSq() - tc * tc; if (d2 > r * r) return -1; return tc - Math.sqrt(r * r - d2); }
function gatherTarget(o, d) {
  if (R.tool === 'kirves') { let best = null, bt = GATHER_REACH; for (const tr of TREES) { if (tr.state !== 'up') continue; const t = rayCyl(o, d, tr.x, tr.z, 0.5 * tr.s, 3.5); if (t >= 0 && t < bt) { bt = t; best = tr; } } return best ? { kind: 'tree', obj: best, t: bt } : null; }
  if (R.tool === 'kirka') { let best = null, bt = GATHER_REACH; for (const r of ROCKS) { if (!r.alive) continue; const t = raySphere(o, d, _v2.set(r.x, r.r * 0.3, r.z), r.r * 1.05); if (t >= 0 && t < bt) { bt = t; best = r; } } return best ? { kind: 'rock', obj: best, t: bt } : null; }
  if (R.tool === 'labidas' && d.y < -0.05) { const t = -o.y / d.y; if (t > GATHER_REACH) return null; const x = o.x + d.x * t, z = o.z + d.z * t; const dl = Math.hypot(x - LAKE.x, z - LAKE.z); if (dl > LAKE_R - 0.5 && dl < LAKE_R + 4.5) return { kind: 'sand', t, p: new V3(x, 0.05, z) }; }
  return null;
}
const gather = { obj: null, work: 0, snd: 0 };
function updateGather(dt, o, d) {
  if (!R.working || !['kirves', 'kirka', 'labidas'].includes(R.tool)) { R.swing = Math.max(0, R.swing - dt * 4); return; }
  const tg = gatherTarget(o, d); R.swing += dt * 9;
  if (!tg) { gather.obj = null; gather.work = 0; return; }
  const id = tg.obj || 'sand'; if (gather.obj !== id) { gather.obj = id; gather.work = 0; }
  const speed = [0, 1, 1.7, 2.8][tier(R.tool)] || 1;
  if (tg.kind === 'rock' && tg.obj.iron && tier('kirka') < 2) { if (R.time - R.lastLack > 2) { R.lastLack = R.time; say('Raua jaoks on vaja kivikirkat!'); } return; }
  gather.work += dt * speed; gather.snd -= dt;
  const hitP = o.clone().addScaledVector(d, tg.t);
  if (gather.snd <= 0) { gather.snd = 0.38; haptic('right', 0.5, 50); if (tg.kind === 'tree') { sfx.chop(); burst(hitP, '#c8a070', 5, 2.5, 0.05); } else if (tg.kind === 'rock') { sfx.mine(); burst(hitP, tg.obj.iron ? '#c87a3a' : '#9a9a98', 5, 2.5, 0.05); } else { sfx.dig(); burst(hitP, '#e0cc98', 8, 2, 0.05); } }
  const need = tg.kind === 'sand' ? 1.3 : 3.0;
  if (gather.work < need) return;
  gather.work = 0; gather.obj = null;
  if (tg.kind === 'tree') { const tr = tg.obj; tr.state = 'fall'; tr.t = 0; tr.dir = Math.atan2(tr.x - o.x, -(tr.z - o.z)); sfx.fall(); const n = Math.max(1, Math.round((tr.type === 'mand' ? 7 : tr.type === 'kask' ? 5 : 6) * yieldK())); S.res.puit += n; pop(new V3(tr.x, 2.2, tr.z), `+${n} puitu`); wear('kirves'); }
  else if (tg.kind === 'rock') { const r = tg.obj; r.alive = false; r.respawn = 120; setRock(r, 0); sfx.boom?.(); noise(0.4, 0.5, 'lowpass', 1500, 100); burst(_v.set(r.x, r.r * 0.4, r.z), '#8a8a88', 20, 4, 0.08); const res = r.iron ? 'raud' : 'kivi'; const n = Math.max(1, Math.round((r.iron ? 3 : 5) * yieldK())); S.res[res] += n; pop(new V3(r.x, 1.4, r.z), `+${n} ${res}`); wear('kirka'); }
  else { const n = Math.max(1, Math.round(2 * yieldK())); S.res.liiv += n; pop(tg.p.clone().setY(1.2), `+${n} liiva`); wear('labidas'); }
  dirtySave = true;
}
function updateTrees(dt) {
  for (const tr of TREES) {
    if (tr.state === 'fall') { tr.t += dt; const a = Math.min(PI / 2, tr.t * tr.t * 1.4); setTreeMatrix(tr, 1, a); if (tr.t > 1.6) { tr.state = 'gone'; tr.respawn = 90; setTreeMatrix(tr, 0.0001, 0); stumpIM.setMatrixAt(TREES.indexOf(tr), _m.makeTranslation(tr.x, 0, tr.z).multiply(_m2.makeScale(tr.s, tr.s, tr.s))); stumpIM.instanceMatrix.needsUpdate = true; burst(new V3(tr.x + Math.cos(tr.dir) * 0, 0.5, tr.z), '#5a8a3a', 20, 3, 0.08); } }
    else if (tr.state === 'gone') { tr.respawn -= dt; if (tr.respawn <= 0) { tr.state = 'grow'; tr.t = 0; stumpIM.setMatrixAt(TREES.indexOf(tr), _m.makeScale(0, 0, 0)); stumpIM.instanceMatrix.needsUpdate = true; } }
    else if (tr.state === 'grow') { tr.t += dt; setTreeMatrix(tr, smooth(0, 4, tr.t), 0); if (tr.t >= 4) tr.state = 'up'; }
  }
  for (const r of ROCKS) if (!r.alive) { r.respawn -= dt; if (r.respawn <= 0) { r.alive = true; setRock(r, 1); } }
}

/* ================= LIIKUMINE ================= */
function blockedAt(x, z, feet, stepTol) {
  for (const [dx, dz] of [[0, 0], [0.25, 0], [-0.25, 0], [0, 0.25], [0, -0.25], [0.18, 0.18], [-0.18, 0.18], [0.18, -0.18], [-0.18, -0.18]]) {
    const px = x + dx, pz = z + dz; const i = Math.floor(px), j = Math.floor(pz);
    for (let k = Math.floor(feet) - 1; k <= Math.floor(feet + 1.7); k++) { if (k < 0) continue; const h = cellTop(i, j, k, px, pz); if (h < 0) continue; if (k + h > feet + stepTol && k < feet + 1.7) return true; }
  }
  for (const tr of TREES) if (tr.state === 'up' && Math.abs(tr.x - x) < 1 && Math.abs(tr.z - z) < 1 && Math.hypot(tr.x - x, tr.z - z) < 0.35 + 0.25 * tr.s) return true;
  for (const r of ROCKS) if (r.alive && Math.hypot(r.x - x, r.z - z) < r.r * 0.9 + 0.2 && feet < r.r * 0.8) return true;
  if (Math.hypot(x - LAKE.x, z - LAKE.z) < LAKE_R - 0.6 && feet < 0.5) return true;
  if (Math.hypot(x, z) > 90) return true;
  return false;
}
function groundAt(x, z, feet) {
  let g = 0;
  for (const [dx, dz] of [[0, 0], [0.15, 0], [-0.15, 0], [0, 0.15], [0, -0.15]]) { const px = x + dx, pz = z + dz; const i = Math.floor(px), j = Math.floor(pz);
    for (let k = Math.floor(feet + 0.55); k >= Math.max(0, Math.floor(feet) - 2); k--) { const h = cellTop(i, j, k, px, pz); if (h < 0) continue; const top = k + h; if (top <= feet + 0.56) { g = Math.max(g, top); break; } } }
  return g;
}
const IN = { lx: 0, ly: 0, rx: 0, ry: 0 }; const keys = {}; const btnPrev = {};
const dz = v => Math.abs(v) < 0.15 ? 0 : v;
function readXR() {
  const s = renderer.xr.getSession(); if (!s) return;
  for (const src of s.inputSources) { const gp = src.gamepad; if (!gp) continue; const h = src.handedness, ax = gp.axes; const x = ax.length >= 4 ? ax[2] : ax[0] || 0, y = ax.length >= 4 ? ax[3] : ax[1] || 0;
    if (h === 'left') { IN.lx = dz(x); IN.ly = dz(y); } else if (h === 'right') { IN.rx = dz(x); IN.ry = dz(y); }
    for (const bi of [4, 5]) { const p = !!gp.buttons[bi]?.pressed, k = h + bi; if (p && !btnPrev[k]) onButton(h, bi); btnPrev[k] = p; } }
}
function onButton(h, bi) { if (h === 'right' && bi === 4) { R.rot = (R.rot + 1) % 4; sfx.click(); } else if (h === 'right' && bi === 5) { S.fly = !S.fly; R.vy = 0; say(S.fly ? 'Lendamine sees' : 'Lendamine väljas'); } else if (h === 'left' && bi === 4) cycleBlock(1); else if (h === 'left' && bi === 5) cycleTool(); }
function readKeys() {
  if (renderer.xr.isPresenting) return; const ax = (a, b) => (keys[a] ? 1 : 0) - (keys[b] ? 1 : 0);
  IN.lx = ax('KeyD', 'KeyA'); IN.ly = ax('KeyS', 'KeyW'); IN.rx = 0; IN.ry = S.fly ? ax('Space', 'KeyC') : 0;
}
function updateMove(dt) {
  const p = rig.position; camera.getWorldDirection(_v); _v.y = 0; if (_v.lengthSq() < 1e-4) _v.set(0, 0, -1); _v.normalize();
  const rx = -_v.z, rz = _v.x; const sp = (S.fly ? 6 : 3.6) * dt;
  const mx = (_v.x * -IN.ly + rx * IN.lx) * sp, mz = (_v.z * -IN.ly + rz * IN.lx) * sp;
  const tol = S.fly ? 0.02 : 0.55;
  if (!blockedAt(p.x + mx, p.z + mz, p.y, tol)) { p.x += mx; p.z += mz; } else if (!blockedAt(p.x + mx, p.z, p.y, tol)) p.x += mx; else if (!blockedAt(p.x, p.z + mz, p.y, tol)) p.z += mz;
  if (renderer.xr.isPresenting) { if (Math.abs(IN.rx) > 0.7) { if (!R.snap) { rig.rotation.y += IN.rx > 0 ? -PI / 6 : PI / 6; R.snap = true; } } else if (Math.abs(IN.rx) < 0.3) R.snap = false; }
  const ground = groundAt(p.x, p.z, p.y);
  if (S.fly) { const vy = (renderer.xr.isPresenting ? -IN.ry : IN.ry) * 4 * dt; const ny = Math.max(ground, Math.min(30, p.y + vy)); if (vy > 0 && blockedAt(p.x, p.z, ny, 0.02)) { } else p.y = ny; R.vy = 0; R.grounded = p.y <= ground + 0.01; }
  else { R.vy -= 14 * dt; let ny = p.y + R.vy * dt; if (R.vy > 0 && cellTop(Math.floor(p.x), Math.floor(p.z), Math.floor(ny + 1.8), p.x, p.z) >= 0) { R.vy = 0; ny = p.y; } if (ny <= ground) { ny = ground; R.vy = 0; R.grounded = true; } else if (p.y - ground < 0.56 && R.vy <= 0 && R.grounded) { ny = ground; R.vy = 0; } else R.grounded = false; p.y = ny; }
}

/* ================= SISEND ================= */
const ray = new THREE.Raycaster(); ray.far = 14;
const uiHit = (o, d) => { ray.set(o, d); const h = ray.intersectObjects(UIT.filter(m => m.parent && m.visible), false); return h[0] || null; };
function ctrlRay(c, o, d) { _m.identity().extractRotation(c.matrixWorld); o.setFromMatrixPosition(c.matrixWorld); d.set(0, 0, -1).applyMatrix4(_m).normalize(); }
function deskRay(o, d) { camera.getWorldPosition(o); camera.getWorldDirection(d); }
const ctrls = []; let rightC = null, leftC = null;
function buildControllers() {
  const cmf = new XRControllerModelFactory();
  for (let k = 0; k < 2; k++) {
    const c = renderer.xr.getController(k); const gr = renderer.xr.getControllerGrip(k); c.userData = { hand: null, src: null };
    c.addEventListener('connected', e => { c.userData.hand = e.data.handedness; c.userData.src = e.data; if (e.data.handedness === 'left') { leftC = c; gr.add(wrist.mesh); } else { rightC = c; gr.add(toolHolder); toolHolder.position.set(0, 0, 0); toolHolder.rotation.set(-0.5, 0, 0); } });
    c.addEventListener('disconnected', () => { c.userData.src = null; });
    c.addEventListener('selectstart', () => { initAudio(); const o = new V3(), d = new V3(); ctrlRay(c, o, d); const u = uiHit(o, d); if (u) { activateUI(u); c.userData.ui = true; return; } c.userData.ui = false; if (c.userData.hand === 'right') useStart(o, d); else removeOne(target(o, d)); });
    c.addEventListener('selectend', () => { if (c.userData.hand === 'right' && !c.userData.ui) useEnd(); c.userData.ui = false; });
    c.addEventListener('squeezestart', () => { const o = new V3(), d = new V3(); ctrlRay(c, o, d); interact(o, d); });
    const line = new THREE.Line(new THREE.BufferGeometry().setFromPoints([new V3(), new V3(0, 0, -1)]), new THREE.LineBasicMaterial({ color: '#ffffff', transparent: true, opacity: 0.7, fog: false })); c.add(line); c.userData.line = line;
    rig.add(c); gr.add(cmf.createControllerModel(gr)); rig.add(gr); ctrls.push(c);
  }
}
function activateUI(h) { const P = h.object.userData.panel; if (P) P.click(h.uv); }
function useStart(o, d) {
  if (!has(R.tool)) { say(`${TOOL_NAME[R.tool]} puudub!`); return; }
  if (['haamer', 'saag', 'pintsel'].includes(R.tool)) { const h = target(o, d); if (!h) return; if (R.tool !== 'haamer' && !easy() && R.tool === 'saag' && !has('saag')) return; startDrag(o, d, h); }
  else R.working = true;
}
function useEnd() { if (R.drag) finishDrag(); R.working = false; }
addEventListener('keydown', e => {
  if (e.code === 'Tab') e.preventDefault(); if (keys[e.code]) return; keys[e.code] = true; initAudio();
  const n = { Digit1: 'haamer', Digit2: 'saag', Digit3: 'pintsel', Digit4: 'kirves', Digit5: 'kirka', Digit6: 'labidas' }[e.code]; if (n) selectTool(n);
  switch (e.code) {
    case 'KeyQ': cycleBlock(1); break; case 'KeyR': R.rot = (R.rot + 1) % 4; say(`Pööre ${R.rot * 90}°`); break;
    case 'KeyF': S.fly = !S.fly; R.vy = 0; say(S.fly ? 'Lendamine sees' : 'Lendamine väljas'); break; case 'KeyZ': undo(); break;
    case 'KeyE': { const o = new V3(), d = new V3(); deskRay(o, d); interact(o, d); break; }
    case 'Tab': case 'KeyM': R.menuDesk = !R.menuDesk; placeDeskMenu(); break;
    case 'Space': if (!S.fly && R.grounded) { R.vy = 5.2; R.grounded = false; } e.preventDefault(); break;
  }
});
addEventListener('keyup', e => { keys[e.code] = false; });
function placeDeskMenu() { if (renderer.xr.isPresenting) return; if (R.menuDesk) { camera.add(wrist.mesh); wrist.mesh.position.set(0, -0.02, -0.42); wrist.mesh.rotation.set(0, 0, 0); } else camera.remove(wrist.mesh); }
const cvs = renderer.domElement;
cvs.addEventListener('contextmenu', e => e.preventDefault());
cvs.addEventListener('mousedown', e => {
  initAudio(); if (document.pointerLockElement !== cvs) { cvs.requestPointerLock?.(); return; }
  const o = new V3(), d = new V3(); deskRay(o, d); const u = uiHit(o, d);
  if (e.button === 0) { if (u) { activateUI(u); return; } useStart(o, d); R.mouseUse = true; } else if (e.button === 2) removeOne(target(o, d));
});
addEventListener('mouseup', e => { if (e.button === 0 && R.mouseUse) { R.mouseUse = false; useEnd(); } });
addEventListener('wheel', e => { if (document.pointerLockElement === cvs) cycleBlock(e.deltaY > 0 ? 1 : -1); });
addEventListener('mousemove', e => { if (document.pointerLockElement !== cvs || renderer.xr.isPresenting) return; rig.rotation.y -= e.movementX * 0.0025; camera.rotation.x = clamp(camera.rotation.x - e.movementY * 0.0025, -1.5, 1.5); });

/* ================= PÕHITSÜKKEL ================= */
const O = new V3(), D = new V3();
function updatePointing(dt) {
  for (const P of PANELS) P.hoverNext = -1;
  const mark = h => { const P = h.object.userData.panel; if (P) P.hoverNext = P.indexAt(h.uv); };
  let haveRay = false;
  if (renderer.xr.isPresenting) {
    for (const c of ctrls) { const L = c.userData.line; if (!c.userData.src) { L.visible = false; continue; } L.visible = true; const o = new V3(), d = new V3(); ctrlRay(c, o, d); const u = uiHit(o, d); if (u) { mark(u); L.scale.z = u.distance; L.material.color.set('#ffd080'); } else { L.material.color.set('#ffffff'); const h = c === rightC ? target(o, d) : gridRay(o, d, REACH); L.scale.z = h ? h.t : 3; }
      if (c === rightC) { O.copy(o); D.copy(d); haveRay = !u; } }
    if (!rightC) haveRay = false;
  } else { deskRay(O, D); const u = uiHit(O, D); if (u) mark(u); haveRay = !u; $('cross').style.borderColor = u ? '#ffd080' : '#fff'; }
  for (const P of PANELS) if (P.hoverNext !== P.hover) { P.hover = P.hoverNext; P.refresh(); }
  hideGhosts(); setHint(null);
  if (!haveRay && !R.drag) { updateGather(dt, O, D); return; }
  if (R.drag) dragUpdate(O, D);
  if (['haamer', 'saag', 'pintsel'].includes(R.tool) && has(R.tool)) {
    if (R.drag) {
      const cells = boxCells(R.drag.start, R.drag.cur).slice(0, 512); const pc = playerCells(); let n = 0;
      if (R.tool === 'haamer' && cells.length === 1) { const c = cells[0]; const ok = canPlace(c, S.block, pc); const g = ghosts[S.block]; g.visible = true; g.position.set(c.i + 0.5, c.k, c.j + 0.5); g.rotation.y = R.drag.rot * PI / 2; g.children.forEach(m => m.material = ok ? ghostMat : ghostBad); }
      else for (const c of cells) { let col; if (R.tool === 'haamer') { col = canPlace(c, S.block, pc) ? '#60ff80' : null; } else { col = getRaw(c.i, c.j, c.k) ? (R.tool === 'saag' ? '#ff8040' : PAINT[S.paint][1]) : null; } if (!col) continue; ghostBox.setMatrixAt(n, _m.makeTranslation(c.i + 0.5, c.k, c.j + 0.5)); ghostBox.setColorAt(n, _c.set(col)); n++; }
      ghostBox.count = n; ghostBox.instanceMatrix.needsUpdate = true; if (ghostBox.instanceColor) ghostBox.instanceColor.needsUpdate = true;
    } else {
      const h = target(O, D);
      if (h) {
        if (R.tool === 'haamer') { const c = h.place; const ok = canPlace(c, S.block, playerCells()); const g = ghosts[S.block]; g.visible = true; g.position.set(c.i + 0.5, c.k, c.j + 0.5); g.rotation.y = autoRot(D) * PI / 2; g.children.forEach(m => m.material = ok ? ghostMat : ghostBad);
          const bp = BP_ACTIVE.get(key(c.i, c.j, c.k)); if (bp) setHint(BT[bp.t].name + (bp.t !== S.block ? ' ←' : ' ✓'), _v.set(c.i + 0.5, c.k + 1.6, c.j + 0.5)); }
        else if (!h.ground) { selBox.visible = true; selBox.position.set(h.i + 0.5, h.k, h.j + 0.5); selBox.material.color.set(R.tool === 'saag' ? '#ff8040' : PAINT[S.paint][1]); }
      }
    }
  }
  updateGather(dt, O, D);
}
let ready = false; const clock = new THREE.Clock(); let accUI = 0;
function step(dt) {
  R.time += dt; IN.lx = IN.ly = IN.rx = IN.ry = 0; readXR(); readKeys();
  updateMove(dt); rig.updateMatrixWorld(true);
  updatePointing(dt);
  for (const t of DIRTY) rebuildType(t); DIRTY.clear();
  updateTrees(dt); updateFX(dt); updateMsg(dt); updateAmbient(dt);
  if (toolModel) { toolModel.rotation.x = -Math.abs(Math.sin(R.swing)) * 0.9; const br = toolModel.children.find(c => c.userData.brush); if (br) br.material.color.set(PAINT[S.paint][1]); }
  if (R.resetArm > 0) R.resetArm -= dt;
  accUI += dt; if (accUI > 0.25) { accUI = 0; for (const p of PANELS) p.check(); }
}
renderer.setAnimationLoop(() => { const dt = Math.min(clock.getDelta(), 0.05); if (ready) step(dt); renderer.render(scene, camera); });

async function init() {
  makeTextures(); await nextFrame(); buildBlockParts(); buildWorld(); await nextFrame(); buildWorkshop(); buildFX(); buildGhosts(); buildBlueprintMesh(); buildPanels(); buildControllers();
  for (const b of S.blocks) { const t = BORDER[b[3]]; if (t) addBlock(b[0], b[1], b[2], t, b[4], b[5], !!b[6]); }
  WORLD.grid.visible = S.grid; sun.castShadow = S.shadows;
  camera.add(toolHolder); toolHolder.position.set(0.26, -0.26, -0.42); toolHolder.rotation.set(-0.2, 0.15, 0);
  if (!has(R.tool)) { const own = TOOLS.map(x => x[0]).find(has); R.tool = own || 'kirves'; }
  updateToolModel(); refreshBlueprint(); camera.position.set(0, 1.65, 0);
  document.body.appendChild(VRButton.createButton(renderer, { optionalFeatures: ['local-floor', 'bounded-floor', 'hand-tracking'] }));
  renderer.xr.addEventListener('sessionstart', () => { initAudio(); $('info').style.display = 'none'; $('cross').style.display = 'none'; camera.position.set(0, 0, 0); camera.rotation.set(0, 0, 0); if (R.menuDesk) { R.menuDesk = false; camera.remove(wrist.mesh); } camera.remove(toolHolder); if (rightC) { const gr = renderer.xr.getControllerGrip(ctrls.indexOf(rightC)); gr.add(toolHolder); toolHolder.position.set(0, 0, 0); toolHolder.rotation.set(-0.5, 0, 0); } });
  renderer.xr.addEventListener('sessionend', () => { $('cross').style.display = ''; camera.position.set(0, 1.65, 0); camera.add(toolHolder); toolHolder.position.set(0.26, -0.26, -0.42); toolHolder.rotation.set(-0.2, 0.15, 0); });
  $('load').style.opacity = 0; setTimeout(() => $('load').remove(), 700); ready = true;
  say(easy() ? 'Tere, meister! Vasaku käe randmel on menüü.' : 'Tere, meister! Raiu kirvega puid ja meisterda töölaual.');
  window.G = { S, R, rig, camera, BLOCKS, addBlock, PROJECTS, projProgress, step, keys, selectTool, setMode, finishDrag, startDrag, target, TREES, ROCKS, sim: s => { for (let t = 0; t < s; t += 0.05) step(0.05); } };
}
init();
</script>
</body>
</html>
