[Uploading index (37).html…]()
<!doctype html>
<html lang="et">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1">
<title>Doktor VR</title>
<style>
  html,body{margin:0;height:100%;overflow:hidden;background:#dff0f6;font-family:system-ui,-apple-system,"Segoe UI",sans-serif}
  canvas{display:block}
  #info{position:fixed;left:14px;top:14px;max-width:340px;padding:14px 16px;background:rgba(255,255,255,.93);border:2px solid #3aa0c8;border-radius:12px;font-size:13px;line-height:1.55;color:#1a3a4a;z-index:5}
  #info h2{margin:0 0 6px;font-size:18px;color:#1a7aa8}
  #info button{margin-top:6px;background:#fff;border:1px solid #3aa0c8;border-radius:6px;padding:3px 10px;cursor:pointer}
  #cross{position:fixed;left:50%;top:50%;width:10px;height:10px;margin:-7px 0 0 -7px;border:2px solid #1a7aa8;border-radius:50%;pointer-events:none;z-index:4}
</style>
<script type="importmap">
{"imports":{"three":"https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.module.js","three/addons/":"https://cdn.jsdelivr.net/npm/three@0.160.0/examples/jsm/"}}
</script>
</head>
<body>
<div id="info">
  <h2>DOKTOR VR</h2>
  <b>Quest:</b> vajuta all <b>ENTER VR</b>.<br>
  Päästik või haare – võta tööriist kandikult (ka kaugelt kiirega).<br>
  Vii tööriist patsiendi õige koha juurde ja hoia hetk.<br>
  Lase lahti – tööriist läheb kandikule tagasi.<br>
  Vasak pulk – liigu kabinetis.<br>
  <b>Arvutis:</b> klõpsa ekraanil, hiirega vaatad. Klõpsa tööriista, siis vaata patsiendi kehaosa poole. Parem klõps – pane tagasi. 1–9 võtab tööriista.
  <br><button id="hideInfo">Peida</button>
</div>
<div id="cross"></div>
<script type="module">
import * as THREE from 'three';
import { VRButton } from 'three/addons/webxr/VRButton.js';
import { XRControllerModelFactory } from 'three/addons/webxr/XRControllerModelFactory.js';

const PI = Math.PI, V3 = THREE.Vector3;
const $ = id => document.getElementById(id);
const clamp = (v, a, b) => v < a ? a : v > b ? b : v;
const rnd = (a = 0, b = 1) => a + Math.random() * (b - a);
const pick = a => a[(Math.random() * a.length) | 0];
const _v = new V3(), _v2 = new V3();
$('hideInfo').onclick = () => $('info').style.display = 'none';

/* ---------- renderdaja ---------- */
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setPixelRatio(Math.min(devicePixelRatio, 2)); renderer.setSize(innerWidth, innerHeight);
renderer.outputColorSpace = THREE.SRGBColorSpace; renderer.toneMapping = THREE.ACESFilmicToneMapping; renderer.toneMappingExposure = 1.1;
renderer.shadowMap.enabled = true; renderer.shadowMap.type = THREE.PCFSoftShadowMap;
renderer.xr.enabled = true; renderer.xr.setReferenceSpaceType('local-floor');
document.body.appendChild(renderer.domElement);
const scene = new THREE.Scene(); scene.background = new THREE.Color('#dff0f6');
const camera = new THREE.PerspectiveCamera(70, innerWidth / innerHeight, 0.02, 100); camera.rotation.order = 'YXZ';
addEventListener('resize', () => { camera.aspect = innerWidth / innerHeight; camera.updateProjectionMatrix(); renderer.setSize(innerWidth, innerHeight); });
scene.add(new THREE.HemisphereLight('#ffffff', '#b8c8d0', 1.6));
const key = new THREE.DirectionalLight('#fff6ea', 1.6); key.position.set(1.5, 3.2, 1.2); key.castShadow = true; key.shadow.mapSize.set(1024, 1024);
Object.assign(key.shadow.camera, { left: -3, right: 3, top: 3, bottom: -3, near: 0.5, far: 8 }); key.shadow.bias = -0.0008; scene.add(key);
const lamp = new THREE.PointLight('#ffffff', 6, 6, 1.5); lamp.position.set(0, 2.6, -0.6); scene.add(lamp);

/* ---------- abi ---------- */
function cnv(w, h) { const c = document.createElement('canvas'); c.width = w; c.height = h; return c; }
function ctex(c) { const t = new THREE.CanvasTexture(c); t.colorSpace = THREE.SRGBColorSpace; return t; }
function rr(g, x, y, w, h, r) { g.beginPath(); g.moveTo(x + r, y); g.arcTo(x + w, y, x + w, y + h, r); g.arcTo(x + w, y + h, x, y + h, r); g.arcTo(x, y + h, x, y, r); g.arcTo(x, y, x + w, y, r); g.closePath(); }
function txt(g, s, x, y, size, col, align = 'left', bold = false) { g.font = `${bold ? 'bold ' : ''}${size}px system-ui,sans-serif`; g.fillStyle = col; g.textAlign = align; g.textBaseline = 'middle'; g.fillText(s, x, y); }
function fit(g, s, x, y, size, col, maxW, align = 'center', bold = true) { let fs = size; g.font = `${bold ? 'bold ' : ''}${fs}px system-ui`; while (g.measureText(s).width > maxW && fs > 14) { fs -= 2; g.font = `${bold ? 'bold ' : ''}${fs}px system-ui`; } txt(g, s, x, y, fs, col, align, bold); }
const M = (color, o = {}) => new THREE.MeshStandardMaterial(Object.assign({ color, roughness: 0.6 }, o));
function mesh(geo, mat, x = 0, y = 0, z = 0, parent) { const m = new THREE.Mesh(geo, mat); m.position.set(x, y, z); m.castShadow = true; m.receiveShadow = true; if (parent) parent.add(m); return m; }
function cylBetween(a, b, r, mat, parent) { const d = new V3().subVectors(b, a); const m = new THREE.Mesh(new THREE.CylinderGeometry(r, r, d.length(), 12), mat); m.position.copy(a).add(b).multiplyScalar(0.5); m.quaternion.setFromUnitVectors(new V3(0, 1, 0), d.normalize()); m.castShadow = true; parent.add(m); return m; }
function glowTex() { const c = cnv(64, 64), g = c.getContext('2d'); const gr = g.createRadialGradient(32, 32, 0, 32, 32, 32); gr.addColorStop(0, 'rgba(255,255,255,1)'); gr.addColorStop(1, 'rgba(255,255,255,0)'); g.fillStyle = gr; g.fillRect(0, 0, 64, 64); return ctex(c); }
const GLOW = glowTex();

/* ---------- heli ---------- */
let AC = null, master;
function initAudio() { if (AC) { if (AC.state === 'suspended') AC.resume(); return; } try { AC = new (window.AudioContext || window.webkitAudioContext)(); master = AC.createGain(); master.gain.value = 0.7; master.connect(AC.destination); } catch (e) { } }
function tone(f1, f2, dur, type = 'sine', vol = 0.2, delay = 0) { if (!AC) return; const t = AC.currentTime + delay; const o = AC.createOscillator(), g = AC.createGain(); o.type = type; o.frequency.setValueAtTime(f1, t); o.frequency.exponentialRampToValueAtTime(Math.max(1, f2), t + dur); g.gain.setValueAtTime(vol, t); g.gain.exponentialRampToValueAtTime(0.0001, t + dur); o.connect(g).connect(master); o.start(t); o.stop(t + dur + 0.02); }
function noise(dur, vol, f, delay = 0) { if (!AC) return; const t = AC.currentTime + delay; const b = AC.createBuffer(1, AC.sampleRate * dur | 0, AC.sampleRate), d = b.getChannelData(0); for (let i = 0; i < d.length; i++) d[i] = (Math.random() * 2 - 1) * (1 - i / d.length); const s = AC.createBufferSource(); s.buffer = b; const fl = AC.createBiquadFilter(); fl.type = 'bandpass'; fl.frequency.value = f; const g = AC.createGain(); g.gain.value = vol; s.connect(fl).connect(g).connect(master); s.start(t); }
const sfx = {
  grab: () => tone(700, 1000, 0.06, 'sine', 0.12), drop: () => tone(500, 350, 0.08, 'sine', 0.1),
  good: () => { tone(660, 660, 0.1, 'triangle', 0.15); tone(990, 990, 0.18, 'triangle', 0.15, 0.1); },
  bad: () => tone(240, 170, 0.25, 'sawtooth', 0.06),
  heart: () => { for (let i = 0; i < 3; i++) { tone(70, 50, 0.12, 'sine', 0.5, i * 0.7); tone(60, 45, 0.1, 'sine', 0.4, i * 0.7 + 0.18); } },
  cough: () => { noise(0.18, 0.5, 700); noise(0.15, 0.4, 600, 0.25); },
  aaa: () => { tone(300, 280, 1.0, 'sawtooth', 0.05); tone(900, 850, 1.0, 'sine', 0.04); },
  babble: (hi = 1) => { const n = 3 + (Math.random() * 4 | 0); for (let i = 0; i < n; i++) { const f = (320 + Math.random() * 260) * hi; tone(f, f * (0.8 + Math.random() * 0.5), 0.09, 'triangle', 0.08, i * 0.1); } },
  boing: () => tone(200, 600, 0.25, 'sine', 0.2), pop: () => tone(900, 400, 0.08, 'sine', 0.2),
  win: () => [523, 659, 784, 1046, 1318].forEach((f, i) => tone(f, f, 0.3, 'triangle', 0.14, i * 0.1)),
  spray: () => noise(0.35, 0.25, 4000), door: () => { tone(220, 180, 0.2, 'triangle', 0.1); noise(0.2, 0.2, 500); },
  step: () => tone(120, 90, 0.05, 'sine', 0.08),
};
let musT = 0; const MEL = [523, 587, 659, 784, 880, 784, 659, 587];
function music(dt) { if (!AC || !S.music) return; musT -= dt; if (musT > 0) return; musT = 0.45; if (Math.random() < 0.55) { const f = pick(MEL); tone(f, f, 0.5, 'triangle', 0.025); } }
function haptic(hand, v, ms) { const s = renderer.xr.getSession(); if (!s) return; for (const src of s.inputSources) if (src.handedness === hand) src.gamepad?.hapticActuators?.[0]?.pulse?.(v, ms); }

/* ---------- olek ---------- */
const SAVE = 'doktor_v1';
const S = Object.assign({ cured: 0, stars: 0, easy: true, music: true }, (() => { try { return JSON.parse(localStorage.getItem(SAVE)) || {}; } catch (e) { return {}; } })());
const save = () => { try { localStorage.setItem(SAVE, JSON.stringify(S)); } catch (e) { } };

/* ---------- kabinet ---------- */
function wallTex(c1, c2) { const c = cnv(256, 256), g = c.getContext('2d'); g.fillStyle = c1; g.fillRect(0, 0, 256, 256); g.fillStyle = c2; g.fillRect(0, 180, 256, 8); for (let i = 0; i < 1500; i++) { g.fillStyle = `rgba(0,0,0,${Math.random() * 0.03})`; g.fillRect(Math.random() * 256, Math.random() * 256, 2, 2); } const t = ctex(c); t.wrapS = t.wrapT = THREE.RepeatWrapping; return t; }
function floorTex() { const c = cnv(256, 256), g = c.getContext('2d'); for (let y = 0; y < 4; y++) for (let x = 0; x < 4; x++) { g.fillStyle = (x + y) % 2 ? '#e8eef0' : '#cfe0e6'; g.fillRect(x * 64, y * 64, 64, 64); } const t = ctex(c); t.wrapS = t.wrapT = THREE.RepeatWrapping; t.repeat.set(6, 6); return t; }
function poster(lines, bg, w, h) { const c = cnv(512, 640), g = c.getContext('2d'); g.fillStyle = bg; g.fillRect(0, 0, 512, 640); g.fillStyle = '#fff'; rr(g, 30, 30, 452, 580, 30); g.fill(); lines.forEach(([s, sz, col], i) => fit(g, s, 256, 120 + i * 110, sz, col, 420)); return new THREE.Mesh(new THREE.PlaneGeometry(w, h), new THREE.MeshStandardMaterial({ map: ctex(c), roughness: 0.8 })); }
function buildRoom() {
  const W = 6, D = 6, H = 2.9;
  const floor = mesh(new THREE.PlaneGeometry(W, D).rotateX(-PI / 2), M('#ffffff', { map: floorTex(), roughness: 0.4 }), 0, 0, -1); floor.castShadow = false;
  const wm = M('#ffffff', { map: wallTex('#bfe4ef', '#8ccbe0'), roughness: 0.9 });
  const walls = [[0, H / 2, -1 - D / 2, 0], [0, H / 2, -1 + D / 2, PI], [-W / 2, H / 2, -1, PI / 2], [W / 2, H / 2, -1, -PI / 2]];
  for (const [x, y, z, ry] of walls) { const w = mesh(new THREE.PlaneGeometry(W, H), wm, x, y, z); w.rotation.y = ry; w.castShadow = false; }
  mesh(new THREE.PlaneGeometry(W, D).rotateX(PI / 2), M('#ffffff', { roughness: 1 }), 0, H, -1).castShadow = false;
  // aken
  const sky = cnv(256, 256), sg = sky.getContext('2d'); const gr = sg.createLinearGradient(0, 0, 0, 256); gr.addColorStop(0, '#5aa8e8'); gr.addColorStop(0.7, '#bfe4ff'); gr.addColorStop(0.72, '#6ab04a'); gr.addColorStop(1, '#4a8a3a'); sg.fillStyle = gr; sg.fillRect(0, 0, 256, 256); sg.fillStyle = '#fff'; for (let i = 0; i < 5; i++) { sg.beginPath(); sg.ellipse(40 + i * 50, 50 + (i % 2) * 30, 28, 12, 0, 0, PI * 2); sg.fill(); }
  const win = mesh(new THREE.PlaneGeometry(1.6, 1.1), new THREE.MeshBasicMaterial({ map: ctex(sky) }), 2.99, 1.6, -1.6); win.rotation.y = -PI / 2;
  const fr = M('#ffffff'); for (const [w, h, y, z] of [[0.08, 1.2, 1.6, -0.8], [0.08, 1.2, 1.6, -2.4], [0.08, 0.06, 2.17, -1.6], [0.08, 0.06, 1.03, -1.6], [0.08, 1.1, 1.6, -1.6]]) mesh(new THREE.BoxGeometry(w, h, w === 0.08 && h < 0.1 ? 1.7 : 0.06), fr, 2.97, y, z);
  // uks
  const door = mesh(new THREE.BoxGeometry(0.06, 2.1, 1.0), M('#f4f0e8'), -2.97, 1.05, -2.6); const knob = mesh(new THREE.SphereGeometry(0.04, 12, 8), M('#c0a040', { metalness: 0.8 }), -2.92, 1.0, -2.2);
  ROOM.door = door;
  // voodi
  const bed = new THREE.Group(); bed.position.set(0, 0, -0.95); scene.add(bed);
  mesh(new THREE.BoxGeometry(0.8, 0.5, 1.9), M('#e8ecef'), 0, 0.25, -0.55, bed); mesh(new THREE.BoxGeometry(0.82, 0.08, 1.92), M('#7ac8e0', { roughness: 0.8 }), 0, 0.53, -0.55, bed);
  mesh(new THREE.BoxGeometry(0.6, 0.12, 0.35), M('#ffffff', { roughness: 1 }), 0, 0.63, -1.25, bed);
  // kandik paremal
  const tray = new THREE.Group(); tray.position.set(0.52, 0, -0.22); tray.rotation.y = -0.35; scene.add(tray); ROOM.tray = tray;
  mesh(new THREE.BoxGeometry(0.62, 0.03, 0.46), M('#d8dde0', { metalness: 0.6, roughness: 0.3 }), 0, 0.86, 0, tray);
  for (const [x, z] of [[-0.27, -0.2], [0.27, -0.2], [-0.27, 0.2], [0.27, 0.2]]) mesh(new THREE.CylinderGeometry(0.015, 0.015, 0.85, 8), M('#b0b8bc', { metalness: 0.7 }), x, 0.425, z, tray);
  // laud vasakul
  mesh(new THREE.BoxGeometry(1.2, 0.05, 0.6), M('#e8d8c0'), -1.6, 0.75, -0.5); for (const [x, z] of [[-2.1, -0.75], [-1.1, -0.75], [-2.1, -0.25], [-1.1, -0.25]]) mesh(new THREE.BoxGeometry(0.05, 0.75, 0.05), M('#c8b898'), x, 0.375, z);
  // kapp, taim, plakatid
  mesh(new THREE.BoxGeometry(1.0, 1.8, 0.45), M('#ffffff'), 1.9, 0.9, -3.7); mesh(new THREE.BoxGeometry(0.42, 0.3, 0.02), M('#d04040'), 1.9, 1.5, -3.47); mesh(new THREE.BoxGeometry(0.06, 0.2, 0.025), M('#ffffff'), 1.9, 1.5, -3.455); mesh(new THREE.BoxGeometry(0.2, 0.06, 0.025), M('#ffffff'), 1.9, 1.5, -3.455);
  mesh(new THREE.CylinderGeometry(0.18, 0.14, 0.35, 16), M('#c8704a'), -2.5, 0.175, -3.5); for (let i = 0; i < 9; i++) { const l = mesh(new THREE.SphereGeometry(0.15, 8, 6), M('#4a9a4a'), -2.5 + Math.cos(i) * 0.12, 0.45 + i * 0.07, -3.5 + Math.sin(i) * 0.12); l.scale.set(1, 1.4, 0.5); }
  const p1 = poster([['PESE KÄSI!', 64, '#2a8ab8'], ['🧼', 90, '#000'], ['Seebiga', 44, '#555'], ['20 sekundit', 44, '#555']], '#7ac8e0', 0.6, 0.75); p1.position.set(-1.2, 1.7, -3.99); scene.add(p1);
  const p2 = poster([['OLE TERVE!', 60, '#d04a6a'], ['🍎 Söö puuvilju', 40, '#555'], ['🏃 Liigu palju', 40, '#555'], ['😴 Maga hästi', 40, '#555']], '#f4a0b8', 0.6, 0.75); p2.position.set(-2.99, 1.6, -0.6); p2.rotation.y = PI / 2; scene.add(p2);
}
const ROOM = {};

/* ---------- tööriistad ---------- */
const TOOLS = {};
const TOOL_LIST = [
  ['kraadiklaas', 'Kraadiklaas'], ['stetoskoop', 'Stetoskoop'], ['taskulamp', 'Taskulamp'],
  ['vatitups', 'Vatitups'], ['plaaster', 'Plaaster'], ['side', 'Side / kips'],
  ['rohi', 'Rohi'], ['sustal', 'Süstal'], ['haamer', 'Refleksihaamer'],
];
function buildTool(id) {
  const g = new THREE.Group(); let tip = new V3(0, 0, -0.1);
  const white = M('#ffffff'), metal = M('#c8ccd0', { metalness: 0.8, roughness: 0.25 });
  if (id === 'kraadiklaas') { mesh(new THREE.CylinderGeometry(0.008, 0.008, 0.14, 10).rotateX(PI / 2), white, 0, 0, -0.04, g); mesh(new THREE.SphereGeometry(0.01, 10, 8), M('#e03030'), 0, 0, -0.11, g); mesh(new THREE.BoxGeometry(0.024, 0.012, 0.04), M('#2a6a8a'), 0, 0.008, 0.01, g); tip.set(0, 0, -0.11); }
  if (id === 'stetoskoop') { mesh(new THREE.CylinderGeometry(0.022, 0.022, 0.012, 20).rotateX(PI / 2), metal, 0, 0, -0.1, g); const t = new THREE.Mesh(new THREE.TorusGeometry(0.05, 0.006, 8, 24, PI * 1.3), M('#303a44')); t.rotation.x = PI / 2; t.position.set(0, 0, -0.04); g.add(t); mesh(new THREE.CylinderGeometry(0.005, 0.005, 0.06, 6).rotateX(PI / 2), M('#303a44'), 0, 0, -0.075, g); tip.set(0, 0, -0.106); }
  if (id === 'taskulamp') { mesh(new THREE.CylinderGeometry(0.016, 0.016, 0.13, 12).rotateX(PI / 2), M('#3a6ac8'), 0, 0, 0, g); mesh(new THREE.CylinderGeometry(0.02, 0.016, 0.03, 12).rotateX(PI / 2), metal, 0, 0, -0.075, g); const lens = mesh(new THREE.CircleGeometry(0.017, 16), new THREE.MeshBasicMaterial({ color: '#fff8c0' }), 0, 0, -0.0905, g); lens.rotation.y = PI; const beam = new THREE.Mesh(new THREE.ConeGeometry(0.08, 0.35, 20, 1, true).rotateX(PI / 2).translate(0, 0, -0.27), new THREE.MeshBasicMaterial({ color: '#fff8c0', transparent: true, opacity: 0.18, depthWrite: false, side: THREE.DoubleSide })); beam.rotation.x = PI; beam.position.z = -0.53; beam.visible = false; g.add(beam); g.userData.beam = beam; tip.set(0, 0, -0.09); }
  if (id === 'vatitups') { mesh(new THREE.CylinderGeometry(0.004, 0.004, 0.13, 6).rotateX(PI / 2), white, 0, 0, -0.03, g); mesh(new THREE.SphereGeometry(0.014, 10, 8), M('#ffffff', { roughness: 1 }), 0, 0, -0.1, g); mesh(new THREE.CylinderGeometry(0.018, 0.018, 0.05, 12).rotateX(PI / 2), M('#4ab0e0'), 0, 0, 0.05, g); tip.set(0, 0, -0.1); }
  if (id === 'plaaster') { mesh(new THREE.BoxGeometry(0.08, 0.004, 0.028), M('#f0c8a0', { roughness: 0.9 }), 0, 0, -0.04, g); mesh(new THREE.BoxGeometry(0.028, 0.005, 0.022), M('#ffffff'), 0, 0.001, -0.04, g); tip.set(0, 0, -0.04); }
  if (id === 'side') { mesh(new THREE.CylinderGeometry(0.03, 0.03, 0.06, 16), M('#ffffff', { roughness: 1 }), 0, 0, -0.05, g); mesh(new THREE.CylinderGeometry(0.012, 0.012, 0.062, 10), M('#dddddd'), 0, 0, -0.05, g); tip.set(0, 0, -0.05); }
  if (id === 'rohi') { mesh(new THREE.CylinderGeometry(0.022, 0.022, 0.07, 14).rotateX(PI / 2), M('#e868a0', { transparent: true, opacity: 0.9 }), 0, 0, 0, g); mesh(new THREE.CylinderGeometry(0.014, 0.014, 0.02, 12).rotateX(PI / 2), white, 0, 0, -0.045, g); mesh(new THREE.BoxGeometry(0.03, 0.03, 0.002), M('#ffffff'), 0, 0.0225, 0, g); tip.set(0, 0, -0.055); }
  if (id === 'sustal') { mesh(new THREE.CylinderGeometry(0.01, 0.01, 0.08, 12).rotateX(PI / 2), M('#c8ecff', { transparent: true, opacity: 0.6 }), 0, 0, 0, g); mesh(new THREE.CylinderGeometry(0.006, 0.006, 0.07, 8).rotateX(PI / 2), M('#4a90e0'), 0, 0, 0.03, g); mesh(new THREE.CylinderGeometry(0.0012, 0.0012, 0.04, 6).rotateX(PI / 2), metal, 0, 0, -0.06, g); mesh(new THREE.BoxGeometry(0.03, 0.004, 0.006), M('#ffffff'), 0, 0, 0.07, g); tip.set(0, 0, -0.08); }
  if (id === 'haamer') { mesh(new THREE.CylinderGeometry(0.005, 0.005, 0.15, 8).rotateX(PI / 2), metal, 0, 0, 0, g); const h = mesh(new THREE.ConeGeometry(0.022, 0.05, 3), M('#e04040', { roughness: 0.9 }), 0, 0, -0.08, g); h.rotation.z = PI / 2; tip.set(0, 0, -0.08); }
  g.traverse(o => { if (o.isMesh) o.castShadow = true; });
  const hit = new THREE.Mesh(new THREE.BoxGeometry(0.07, 0.07, 0.17), new THREE.MeshBasicMaterial({ visible: false })); hit.position.z = -0.03; hit.userData.tool = id; g.add(hit);
  return { id, g, tip, hit, slot: new V3(), slotQ: new THREE.Quaternion(), held: null, home: true };
}
let highlight;
function buildTools() {
  TOOL_LIST.forEach(([id, name], i) => {
    const t = buildTool(id); t.name = name; TOOLS[id] = t;
    const x = -0.2 + (i % 3) * 0.2, z = -0.14 + ((i / 3) | 0) * 0.14;
    const p = new V3(x, 0.89, z); ROOM.tray.localToWorld(p); t.slot.copy(p);
    t.slotQ.setFromEuler(new THREE.Euler(0, ROOM.tray.rotation.y, 0)); t.g.position.copy(p); t.g.quaternion.copy(t.slotQ); scene.add(t.g);
    const c = cnv(256, 48), gg = c.getContext('2d'); rr(gg, 2, 2, 252, 44, 14); gg.fillStyle = 'rgba(255,255,255,0.9)'; gg.fill(); fit(gg, name, 128, 26, 28, '#1a4a5a', 240);
    const lab = new THREE.Mesh(new THREE.PlaneGeometry(0.16, 0.03), new THREE.MeshBasicMaterial({ map: ctex(c), transparent: true })); lab.rotation.x = -PI / 2; lab.position.set(x, 0.877, z + 0.06); ROOM.tray.add(lab);
  });
  highlight = new THREE.Sprite(new THREE.SpriteMaterial({ map: GLOW, color: '#ffe060', transparent: true, depthWrite: false, opacity: 0.8 })); highlight.scale.setScalar(0.16); scene.add(highlight);
}
const tipWorld = t => t.g.localToWorld(_v.copy(t.tip));

/* ---------- patsiendid ---------- */
const NAMES = ['Mia', 'Oskar', 'Liisa', 'Robin', 'Emma', 'Karl', 'Sofia', 'Rasmus', 'Lenna', 'Marten', 'Hanna', 'Joosep', 'Eliise', 'Kaspar'];
const ANIMALS = [['Kiisu Miisu', 'cat', '#f0a050'], ['Mõmmi', 'bear', '#8a5a3a'], ['Jänku Juss', 'bunny', '#e8e0d8'], ['Kutsu Muri', 'dog', '#c8a070']];
const SKIN = ['#f6d0b0', '#e8b890', '#c88a60', '#8a5a3a', '#f8dcc4'], HAIR = ['#3a2a1a', '#e0b050', '#8a3a1a', '#1a1a1a', '#c86a3a'], SHIRT = ['#e85a6a', '#5ab0e0', '#f0c040', '#6ac06a', '#a070e0', '#f08a40'];
const CASES = [
  { id: 'palavik', say: 'Mul on nii palav ja pea valutab…', steps: [{ tool: 'kraadiklaas', zones: ['suu', 'otsmik'], fx: 'temp', label: 'Mõõda palavikku' }, { tool: 'rohi', zones: ['suu'], fx: 'med', label: 'Anna palavikurohtu' }] },
  { id: 'koha', say: 'Köh-köh! Ma köhin kogu aeg!', steps: [{ tool: 'stetoskoop', zones: ['rind'], fx: 'lungs', label: 'Kuula kopse' }, { tool: 'rohi', zones: ['suu'], fx: 'med', label: 'Anna köhasiirupit' }] },
  { id: 'kurk', say: 'Mu kurk on nii valus…', steps: [{ tool: 'taskulamp', zones: ['suu'], fx: 'throat', label: 'Vaata kurku' }, { tool: 'rohi', zones: ['suu'], fx: 'med', label: 'Anna kurgurohtu' }] },
  { id: 'korv', say: 'Mu kõrv valutab!', steps: [{ tool: 'taskulamp', zones: ['korv'], fx: 'ear', label: 'Vaata kõrva' }, { tool: 'rohi', zones: ['korv'], fx: 'drops', label: 'Pane kõrvatilku' }] },
  { id: 'polv', say: 'Ma kukkusin ja põlv on katki!', wound: 'polv', steps: [{ tool: 'vatitups', zones: ['polv'], fx: 'clean', label: 'Puhasta haav' }, { tool: 'plaaster', zones: ['polv'], fx: 'plaster', label: 'Pane plaaster' }] },
  { id: 'kasi', say: 'Kass kriimustas mu kätt!', wound: 'kasi', steps: [{ tool: 'vatitups', zones: ['kasi'], fx: 'clean', label: 'Puhasta haav' }, { tool: 'plaaster', zones: ['kasi'], fx: 'plaster', label: 'Pane plaaster' }] },
  { id: 'vaktsiin', say: 'Ma tulin vaktsiini saama. Natuke hirmus…', steps: [{ tool: 'vatitups', zones: ['kasi'], fx: 'clean', label: 'Puhasta käsi' }, { tool: 'sustal', zones: ['kasi'], fx: 'shot', label: 'Tee süst' }, { tool: 'plaaster', zones: ['kasi'], fx: 'plaster', label: 'Pane plaaster' }] },
  { id: 'murd', say: 'Ma kukkusin rattaga ja käsi on haige!', broken: true, steps: [{ tool: 'stetoskoop', zones: ['rind'], fx: 'heart', label: 'Kuula südant' }, { tool: 'side', zones: ['kasi'], fx: 'cast', label: 'Pane käsi kipsi' }, { tool: 'rohi', zones: ['suu'], fx: 'med', label: 'Anna valuvaigistit' }] },
  { id: 'kontroll', say: 'Tere! Ma tulin tervisekontrolli.', steps: [{ tool: 'stetoskoop', zones: ['rind'], fx: 'heart', label: 'Kuula südant' }, { tool: 'haamer', zones: ['polv'], fx: 'reflex', label: 'Kontrolli reflekse' }, { tool: 'kraadiklaas', zones: ['suu', 'otsmik'], fx: 'tempok', label: 'Mõõda temperatuuri' }] },
];
const ZONES = { suu: [0, 1.17, 0.15, 0.075], otsmik: [0, 1.34, 0.13, 0.08], korv: [0.165, 1.24, 0, 0.075], korv2: [-0.165, 1.24, 0, 0.075], rind: [0, 0.92, 0.16, 0.12], kasi: [0.25, 0.83, 0.05, 0.1], polv: [0.1, 0.62, 0.38, 0.09], polv2: [-0.1, 0.62, 0.38, 0.09] };
const ZONE_NAME = { suu: 'suu', otsmik: 'otsmik', korv: 'kõrv', rind: 'rind', kasi: 'käsi', polv: 'põlv' };
let P = null, nextT = 1.5;
function makePatient() {
  const animal = Math.random() < 0.3 ? pick(ANIMALS) : null;
  const skin = animal ? animal[2] : pick(SKIN), shirt = pick(SHIRT);
  const root = new THREE.Group(); scene.add(root);
  const body = new THREE.Group(); root.add(body);
  const sk = M(skin, { roughness: 0.7 }), sh = M(shirt, { roughness: 0.8 }), pants = M(pick(['#3a4a8a', '#4a4a4a', '#6a4a8a', '#2a6a6a']));
  mesh(new THREE.CapsuleGeometry(0.16, 0.22, 6, 16), sh, 0, 0.86, 0, body);
  const neck = mesh(new THREE.CylinderGeometry(0.05, 0.05, 0.08, 10), sk, 0, 1.07, 0, body);
  const head = new THREE.Group(); head.position.set(0, 1.25, 0); body.add(head);
  mesh(new THREE.SphereGeometry(0.16, 24, 18), sk, 0, 0, 0, head);
  const eyeW = M('#ffffff', { roughness: 0.3 }), eyeB = M('#1a1a2a', { roughness: 0.2 });
  for (const s of [-1, 1]) { mesh(new THREE.SphereGeometry(0.03, 12, 10), eyeW, s * 0.055, 0.03, 0.135, head); mesh(new THREE.SphereGeometry(0.018, 10, 8), eyeB, s * 0.055, 0.032, 0.158, head); }
  const cheekM = new THREE.MeshBasicMaterial({ color: '#ff8a9a', transparent: true, opacity: 0.5 });
  const cheeks = [-1, 1].map(s => { const c = mesh(new THREE.CircleGeometry(0.028, 16), cheekM, s * 0.095, -0.03, 0.128, head); c.rotation.y = s * 0.6; return c; });
  const mouthM = M('#8a2a2a');
  const smile = new THREE.Mesh(new THREE.TorusGeometry(0.035, 0.008, 6, 16, PI), mouthM); smile.position.set(0, -0.065, 0.148); smile.rotation.z = PI; head.add(smile);
  const frown = new THREE.Mesh(new THREE.TorusGeometry(0.03, 0.008, 6, 16, PI), mouthM); frown.position.set(0, -0.085, 0.148); head.add(frown);
  const open = mesh(new THREE.SphereGeometry(0.035, 12, 10), M('#5a1a1a'), 0, -0.075, 0.13, head); open.scale.set(1, 1.2, 0.5); open.visible = false;
  if (animal) {
    const t = animal[1];
    if (t === 'cat' || t === 'dog') for (const s of [-1, 1]) { const e = mesh(new THREE.ConeGeometry(0.05, t === 'cat' ? 0.1 : 0.12, 4), sk, s * 0.1, 0.15, 0, head); e.rotation.z = -s * (t === 'dog' ? 2.6 : 0.35); }
    if (t === 'bear') for (const s of [-1, 1]) mesh(new THREE.SphereGeometry(0.05, 12, 10), sk, s * 0.12, 0.13, 0, head);
    if (t === 'bunny') for (const s of [-1, 1]) { const e = mesh(new THREE.CapsuleGeometry(0.025, 0.14, 4, 8), sk, s * 0.06, 0.24, 0, head); e.rotation.z = -s * 0.15; }
    mesh(new THREE.SphereGeometry(0.022, 10, 8), M('#2a1a1a'), 0, -0.015, 0.16, head);
  } else { const hair = mesh(new THREE.SphereGeometry(0.168, 20, 12, 0, PI * 2, 0, PI * 0.5), M(pick(HAIR), { roughness: 0.9 }), 0, 0.01, -0.01, head); hair.rotation.x = -0.25; }
  // käed
  const arms = []; for (const s of [-1, 1]) { const a = new THREE.Group(); a.position.set(s * 0.21, 1.0, 0); body.add(a); cylBetween(new V3(0, 0, 0), new V3(s * 0.04, -0.24, 0.04), 0.045, sh, a); const fa = new THREE.Group(); fa.position.set(s * 0.04, -0.24, 0.04); a.add(fa); cylBetween(new V3(0, 0, 0), new V3(-s * 0.04, -0.06, 0.2), 0.04, sk, fa); mesh(new THREE.SphereGeometry(0.045, 10, 8), sk, -s * 0.04, -0.06, 0.22, fa); arms.push({ a, fa }); }
  // jalad
  const legs = [], hips = []; for (const s of [-1, 1]) { const hip = new THREE.Group(); hip.position.set(s * 0.1, 0.62, 0); body.add(hip); cylBetween(new V3(0, 0, 0), new V3(0, 0, 0.38), 0.065, pants, hip); mesh(new THREE.SphereGeometry(0.066, 12, 10), pants, 0, 0, 0.38, hip); const shin = new THREE.Group(); shin.position.set(0, 0, 0.38); hip.add(shin); cylBetween(new V3(0, 0, 0), new V3(0, -0.5, 0.03), 0.055, sk, shin); mesh(new THREE.BoxGeometry(0.1, 0.07, 0.17), M('#f4f4f4'), 0, -0.53, 0.07, shin); legs.push(shin); hips.push(hip); }
  // haavad, kips, plaaster
  const wound = (z) => { const g = new THREE.Group(); const zz = ZONES[z]; const n = new V3(zz[0], 0, zz[2]).normalize(); if (z === 'polv') n.set(0, 0.4, 1).normalize(); const pos = new V3(zz[0], zz[1], zz[2]).addScaledVector(n, z === 'polv' ? 0.06 : 0.04); g.position.copy(pos); g.lookAt(pos.clone().add(n)); const r = new THREE.Mesh(new THREE.CircleGeometry(0.03, 16), new THREE.MeshBasicMaterial({ color: '#c82a2a' })); g.add(r); const dirt = new THREE.Mesh(new THREE.CircleGeometry(0.035, 10), new THREE.MeshBasicMaterial({ color: '#6a4a2a', transparent: true, opacity: 0.6 })); dirt.position.z = 0.001; g.add(dirt); const pl = new THREE.Mesh(new THREE.BoxGeometry(0.09, 0.035, 0.006), M('#f0c8a0')); pl.position.z = 0.004; pl.visible = false; g.add(pl); body.add(g); return { g, red: r, dirt, pl }; };
  const name = animal ? animal[0] : pick(NAMES);
  const cs = pick(CASES.filter(c => !P || c.id !== P.case.id));
  const p = { root, body, head, arms, legs, hips, cheeks, cheekM, smile, frown, open, name, case: cs, step: 0, wrong: 0, t: 0, state: 'enter', hold: 0, holdZone: null, wrongShown: false, cough: 2, voice: animal ? 1.4 : 1.15, wounds: {}, timer: 0 };
  if (cs.wound) p.wounds[cs.wound] = wound(cs.wound);
  if (cs.id === 'vaktsiin') { p.wounds.kasi = wound('kasi'); p.wounds.kasi.red.visible = false; p.wounds.kasi.dirt.visible = false; }
  if (cs.broken) { arms[1].fa.children.forEach(m => { if (m.material) { m.material = m.material.clone(); m.material.color.lerp(new THREE.Color('#9a6aa8'), 0.35); } }); const cast = new THREE.Mesh(new THREE.CylinderGeometry(0.055, 0.055, 0.2, 14), M('#ffffff', { roughness: 1 })); cast.position.set(-0.02, -0.03, 0.1); cast.quaternion.setFromUnitVectors(new V3(0, 1, 0), new V3(-0.04, -0.06, 0.2).normalize()); cast.visible = false; arms[1].fa.add(cast); p.cast = cast; }
  p.fever = cs.id === 'palavik'; p.coughing = cs.id === 'koha';
  setFace(p, 'sad');
  root.position.set(-2.6, 0, -2.2); root.rotation.y = PI / 2; body.position.y = -0.0;
  return p;
}
function setFace(p, f) { p.smile.visible = f === 'happy'; p.frown.visible = f === 'sad'; p.open.visible = f === 'aaa'; if (f === 'aaa') { p.smile.visible = p.frown.visible = false; } p.cheekM.color.set(p.fever ? '#ff3030' : '#ff8a9a'); p.cheekM.opacity = p.fever ? 0.8 : 0.5; }
function zoneWorld(p, z) { const zz = ZONES[z]; return p.body.localToWorld(_v2.set(zz[0], zz[1], zz[2])); }
const SEAT = new V3(0, 0, -0.85);

/* ---------- jutumull ja kaart ---------- */
let bubble, card, score, ringM, popups = [];
function buildUI() {
  const bc = cnv(1024, 256); bubble = new THREE.Mesh(new THREE.PlaneGeometry(0.9, 0.225), new THREE.MeshBasicMaterial({ map: ctex(bc), transparent: true, depthWrite: false })); bubble.userData.c = bc; bubble.visible = false; bubble.renderOrder = 10; scene.add(bubble);
  card = new Panel(0.5, 0.62, 640, 800, drawCard); card.mesh.position.set(-0.72, 1.3, -0.42); card.mesh.lookAt(0, 1.3, 0.1); scene.add(card.mesh);
  score = new Panel(1.6, 0.5, 1024, 320, (g, Pn) => { rr(g, 6, 6, Pn.W - 12, Pn.H - 12, 40); g.fillStyle = '#ffffff'; g.fill(); g.lineWidth = 10; g.strokeStyle = '#3aa0c8'; g.stroke(); txt(g, 'DOKTOR VR', Pn.W / 2, 80, 72, '#1a7aa8', 'center', true); txt(g, `Ravitud patsiente: ${S.cured}`, Pn.W / 2, 175, 54, '#1a3a4a', 'center', true); txt(g, `★ ${S.stars}`, Pn.W / 2, 255, 60, '#e8a820', 'center', true); }, false);
  score.mesh.position.set(0, 2.25, -3.98); scene.add(score.mesh);
  ringM = new THREE.Mesh(new THREE.RingGeometry(0.06, 0.075, 32, 1, 0, 0.01), new THREE.MeshBasicMaterial({ color: '#40d040', depthTest: false, transparent: true, side: THREE.DoubleSide })); ringM.renderOrder = 20; ringM.visible = false; scene.add(ringM);
  for (let i = 0; i < 4; i++) { const c = cnv(512, 128); const s = new THREE.Sprite(new THREE.SpriteMaterial({ map: ctex(c), transparent: true, depthTest: false })); s.userData.c = c; s.scale.set(0.5, 0.125, 1); s.visible = false; s.renderOrder = 30; scene.add(s); popups.push({ s, life: 0 }); }
}
function sayP(text) { if (!P) return; const c = bubble.userData.c, g = c.getContext('2d'); g.clearRect(0, 0, 1024, 256); rr(g, 10, 10, 1004, 190, 60); g.fillStyle = '#ffffff'; g.fill(); g.lineWidth = 8; g.strokeStyle = '#3aa0c8'; g.stroke(); g.beginPath(); g.moveTo(480, 196); g.lineTo(512, 250); g.lineTo(560, 196); g.closePath(); g.fill(); fit(g, text, 512, 105, 62, '#1a3a4a', 940); bubble.material.map.needsUpdate = true; bubble.visible = true; sfx.babble(P.voice); }
function popup(pos, text, col = '#1a7aa8') { const p = popups.find(p => p.life <= 0) || popups[0]; const g = p.s.userData.c.getContext('2d'); g.clearRect(0, 0, 512, 128); rr(g, 6, 10, 500, 108, 50); g.fillStyle = 'rgba(255,255,255,0.95)'; g.fill(); fit(g, text, 256, 66, 58, col, 480); p.s.material.map.needsUpdate = true; p.s.position.copy(pos); p.life = 2.2; p.s.visible = true; }
class Panel {
  constructor(w, h, W, H, draw, interactive = true) { Object.assign(this, { W, H, draw, buttons: [], hover: -1, hoverNext: -1, last: '' }); this.c = cnv(W, H); this.g = this.c.getContext('2d'); this.t = ctex(this.c); this.mesh = new THREE.Mesh(new THREE.PlaneGeometry(w, h), new THREE.MeshBasicMaterial({ map: this.t, transparent: true })); this.mesh.userData.panel = this; if (interactive) UIT.push(this.mesh); PANELS.push(this); this.refresh(); }
  refresh() { this.g.clearRect(0, 0, this.W, this.H); this.buttons = []; this.draw(this.g, this); this.t.needsUpdate = true; }
  btn(x, y, w, h, label, fn, o = {}) { const i = this.buttons.length; this.buttons.push({ x, y, w, h, fn }); const g = this.g, hov = this.hover === i; rr(g, x, y, w, h, 22); g.fillStyle = o.on ? '#3aa0c8' : hov ? '#d8f0f8' : '#ffffff'; g.fill(); g.lineWidth = 5; g.strokeStyle = '#3aa0c8'; g.stroke(); fit(g, label, x + w / 2, y + h / 2, o.fs || 38, o.on ? '#fff' : '#1a4a5a', w - 20); }
  indexAt(uv) { const x = uv.x * this.W, y = (1 - uv.y) * this.H; return this.buttons.findIndex(b => x >= b.x && x <= b.x + b.w && y >= b.y && y <= b.y + b.h); }
  click(uv) { const i = this.indexAt(uv); if (i >= 0) { tone(1000, 800, 0.04, 'square', 0.05); this.buttons[i].fn(); this.refresh(); } }
}
const PANELS = [], UIT = [];
function drawCard(g, Pn) {
  rr(g, 6, 6, Pn.W - 12, Pn.H - 12, 30); g.fillStyle = '#fffdf6'; g.fill(); g.lineWidth = 10; g.strokeStyle = '#8a6a4a'; g.stroke();
  g.fillStyle = '#b8b8b8'; rr(g, Pn.W / 2 - 80, 0, 160, 40, 10); g.fill();
  txt(g, 'PATSIENDI KAART', Pn.W / 2, 80, 40, '#8a6a4a', 'center', true);
  if (P && P.state !== 'leave') {
    fit(g, P.name, Pn.W / 2, 140, 52, '#1a3a4a', 560);
    let y = 220; P.case.steps.forEach((s, i) => { const done = i < P.step, cur = i === P.step; const show = S.easy || done; g.fillStyle = done ? '#40b040' : cur ? '#3aa0c8' : '#cccccc'; g.beginPath(); g.arc(70, y, 22, 0, PI * 2); g.fill(); txt(g, done ? '✓' : String(i + 1), 70, y + 2, 30, '#fff', 'center', true); fit(g, show ? s.label : '???', 100, y, 36, done ? '#40a040' : '#1a3a4a', 500, 'left', cur); if (show && S.easy) fit(g, TOOLS[s.tool].name + ' → ' + ZONE_NAME[s.zones[0]], 100, y + 38, 26, '#7a8a90', 500, 'left', false); y += 92; });
  } else txt(g, 'Ootan patsienti…', Pn.W / 2, 300, 40, '#7a8a90', 'center');
  Pn.btn(30, Pn.H - 210, 280, 84, 'Lihtne', () => { S.easy = true; save(); }, { on: S.easy }); Pn.btn(330, Pn.H - 210, 280, 84, 'Raske', () => { S.easy = false; save(); }, { on: !S.easy });
  Pn.btn(30, Pn.H - 112, 280, 84, S.music ? 'Muusika sees' : 'Muusika väljas', () => { S.music = !S.music; save(); }, { fs: 32 }); Pn.btn(330, Pn.H - 112, 280, 84, 'Järgmine', () => { if (P && P.state === 'sit') leave(false); }, { fs: 34 });
}

/* ---------- konfetti ---------- */
const CONF = []; let confIM;
function buildConfetti() { confIM = new THREE.InstancedMesh(new THREE.PlaneGeometry(0.03, 0.02), new THREE.MeshBasicMaterial({ side: THREE.DoubleSide }), 200); confIM.frustumCulled = false; scene.add(confIM); for (let i = 0; i < 200; i++) { CONF.push({ p: new V3(), v: new V3(), r: new V3(), life: 0 }); confIM.setMatrixAt(i, new THREE.Matrix4().makeScale(0, 0, 0)); confIM.setColorAt(i, new THREE.Color(pick(['#ff5a6a', '#ffd040', '#40c0ff', '#60e060', '#c070ff']))); } }
function confetti(pos) { for (const c of CONF) { c.p.copy(pos); c.v.set(rnd(-1.5, 1.5), rnd(1.5, 3.5), rnd(-1.5, 1.5)); c.r.set(rnd(0, 6), rnd(0, 6), rnd(0, 6)); c.life = rnd(1.5, 2.5); } }
const _m = new THREE.Matrix4(), _q = new THREE.Quaternion(), _e = new THREE.Euler();
function updateConfetti(dt) { CONF.forEach((c, i) => { if (c.life <= 0) return; c.life -= dt; c.v.y -= 4 * dt; c.v.multiplyScalar(0.985); c.p.addScaledVector(c.v, dt); if (c.p.y < 0.01) { c.p.y = 0.01; c.v.set(0, 0, 0); } c.r.x += dt * 5; c.r.y += dt * 3; confIM.setMatrixAt(i, c.life > 0 ? _m.compose(c.p, _q.setFromEuler(_e.set(c.r.x, c.r.y, c.r.z)), _v.setScalar(1)) : _m.makeScale(0, 0, 0)); }); confIM.instanceMatrix.needsUpdate = true; }

/* ---------- mänguloogika ---------- */
function applyFx(fx) {
  const p = P, head = zoneWorld(p, 'suu').clone().add(new V3(0, 0.35, 0.05));
  switch (fx) {
    case 'temp': popup(head, '39,2 °C – palavik!', '#d04040'); sayP('Mul on palavik…'); break;
    case 'tempok': popup(head, '36,6 °C – korras!', '#40a040'); break;
    case 'lungs': sfx.heart(); popup(head, 'Kopsudes kahiseb…', '#d08020'); break;
    case 'heart': sfx.heart(); popup(head, 'Tuk-tuk – süda on korras!', '#40a040'); break;
    case 'throat': popup(head, 'Kurk on punane!', '#d04040'); break;
    case 'ear': popup(head, 'Kõrv on punane!', '#d04040'); break;
    case 'drops': sfx.pop(); popup(head, 'Tilk-tilk!', '#3aa0c8'); break;
    case 'clean': sfx.spray(); for (const w of Object.values(p.wounds)) { w.dirt.visible = false; w.red.material.color.set('#e88080'); } popup(head, 'Puhas!', '#3aa0c8'); break;
    case 'plaster': for (const w of Object.values(p.wounds)) { w.pl.visible = true; w.red.visible = false; } sfx.pop(); break;
    case 'shot': sfx.pop(); sayP('Auh! …Polnudki nii hull!'); break;
    case 'cast': p.cast.visible = true; sfx.spray(); popup(head, 'Kips on peal!', '#3aa0c8'); break;
    case 'reflex': p.kick = 0.6; sfx.boing(); popup(head, 'Hopsti!', '#3aa0c8'); break;
    case 'med': sfx.good(); p.fever = false; p.coughing = false; sayP(pick(['Mmm, maasikamaitse!', 'Ai kui hea!', 'Juba on parem!'])); setFace(p, 'happy'); break;
  }
}
function treat(dt) {
  if (!P || P.state !== 'sit') { ringM.visible = false; return; }
  const step = P.case.steps[P.step]; let best = null, bd = 9;
  for (const t of Object.values(TOOLS)) { if (!t.held) continue; const tp = tipWorld(t).clone();
    for (const z in ZONES) { const zz = ZONES[z]; const d = tp.distanceTo(zoneWorld(P, z)); if (d < zz[3] + 0.02 && d < bd) { bd = d; best = { t, z: z.replace('2', '') }; } } }
  // lamp avab suu
  const lampNear = TOOLS.taskulamp.held && tipWorld(TOOLS.taskulamp).distanceTo(zoneWorld(P, 'suu')) < 0.25;
  if (lampNear && P.face !== 'aaa') { P.face = 'aaa'; setFace(P, 'aaa'); sfx.aaa(); } else if (!lampNear && P.face === 'aaa') { P.face = null; setFace(P, P.step >= P.case.steps.length ? 'happy' : (P.happy ? 'happy' : 'sad')); }
  if (!best) { P.hold = 0; P.wrongShown = false; ringM.visible = false; return; }
  const ok = step && best.t.id === step.tool && step.zones.includes(best.z);
  ringM.visible = true; ringM.position.copy(zoneWorld(P, best.z === 'korv' ? (tipWorld(best.t).x > zoneWorld(P, 'korv').x - 0.1 ? 'korv' : 'korv2') : best.z)); ringM.lookAt(camera.getWorldPosition(_v));
  if (P.holdKey !== best.t.id + best.z) { P.holdKey = best.t.id + best.z; P.hold = 0; P.wrongShown = false; }
  P.hold += dt; const f = clamp(P.hold / 0.9, 0, 1);
  ringM.geometry.dispose(); ringM.geometry = new THREE.RingGeometry(0.06, 0.075, 32, 1, PI / 2, -f * PI * 2 + 0.001); ringM.material.color.set(ok ? '#40d040' : '#ff6060');
  if (f < 1) { if (Math.random() < dt * 8) haptic(best.t.held, 0.15, 20); return; }
  if (ok) { applyFx(step.fx); P.step++; P.hold = -99; sfx.good(); haptic(best.t.held, 0.6, 80); card.refresh(); if (P.step >= P.case.steps.length) cured(); }
  else if (!P.wrongShown) { P.wrongShown = true; P.wrong++; sfx.bad(); sayP(pick(['Hmm, see ei aita…', 'Kõdi teeb!', 'Kas see on ikka õige?'])); if (S.easy && step) setTimeout(() => P && P.state === 'sit' && sayP(`Proovi: ${TOOLS[step.tool].name}`), 1400); }
}
function cured() {
  const stars = S.easy ? 3 : Math.max(1, 3 - P.wrong); S.cured++; S.stars += stars; save(); score.refresh();
  P.happy = true; P.fever = false; setFace(P, 'happy'); sayP(`Aitäh, doktor! ${'★'.repeat(stars)}`); sfx.win(); confetti(zoneWorld(P, 'rind').clone().add(new V3(0, 0.6, 0))); P.state = 'done'; P.t = 0;
}
function leave(ok = true) { if (!P) return; P.state = 'leave'; P.t = 0; bubble.visible = false; card.refresh(); }
function updatePatient(dt) {
  if (!P) { nextT -= dt; if (nextT <= 0) { P = makePatient(); sfx.door(); card.refresh(); } return; }
  const p = P; p.t += dt; const r = p.root;
  if (p.state === 'enter') { const from = new V3(-2.6, 0, -2.2), mid = new V3(-0.8, 0, -1.35), to = SEAT.clone().add(new V3(0, 0, 0.0)); const T = 3.2; const s = clamp(p.t / T, 0, 1);
    const a = s < 0.6 ? from.clone().lerp(mid, s / 0.6) : mid.clone().lerp(to, (s - 0.6) / 0.4); r.position.copy(a); const dir = s < 0.6 ? mid.clone().sub(from) : to.clone().sub(mid); r.rotation.y = Math.atan2(dir.x, dir.z); p.body.position.y = Math.abs(Math.sin(p.t * 9)) * 0.03 - 0.12 * 0; walkPose(p, p.t);
    if (Math.floor(p.t * 4.5) !== Math.floor((p.t - dt) * 4.5)) sfx.step();
    if (s >= 1) { p.state = 'sit'; p.t = 0; r.position.copy(SEAT); r.rotation.y = 0; sitPose(p); sayP(p.case.say); card.refresh(); } }
  else if (p.state === 'sit' || p.state === 'done') {
    p.head.rotation.y = Math.sin(R.time * 0.7) * 0.15; p.head.rotation.x = Math.sin(R.time * 0.5) * 0.05;
    if (p.coughing && p.state === 'sit') { p.cough -= dt; if (p.cough < 0) { p.cough = rnd(2.5, 4); sfx.cough(); p.coughT = 0.4; } }
    if (p.coughT > 0) { p.coughT -= dt; p.body.rotation.x = Math.sin(p.coughT * 25) * 0.08; } else p.body.rotation.x = 0;
    if (p.kick > 0) { p.kick -= dt; p.legs[1].rotation.x = -Math.sin((0.6 - p.kick) / 0.6 * PI) * 1.0; } else p.legs[1].rotation.x = 0;
    if (p.fever) p.body.position.y = Math.sin(R.time * 30) * 0.002;
    if (p.state === 'done') { p.body.position.y = Math.abs(Math.sin(p.t * 6)) * 0.06; if (p.t > 3) leave(); }
  }
  else if (p.state === 'leave') { const from = SEAT, to = new V3(-2.6, 0, -2.2); const s = clamp(p.t / 2.6, 0, 1); r.position.copy(from).lerp(to, s); const d = to.clone().sub(from); r.rotation.y = Math.atan2(d.x, d.z); standPose(p); walkPose(p, p.t); p.body.position.y = 0.3 + Math.abs(Math.sin(p.t * 9)) * 0.03;
    if (s >= 1) { scene.remove(r); r.traverse(o => { if (o.geometry) o.geometry.dispose(); }); P = null; nextT = 1.2; card.refresh(); } }
  if (bubble.visible && P) { bubble.position.copy(zoneWorld(P, 'suu')).add(new V3(0, 0.42, 0)); bubble.lookAt(camera.getWorldPosition(_v)); }
}
function sitPose(p) { p.body.position.y = 0; p.legs.forEach(l => l.rotation.x = 0); p.hips.forEach(h => h.rotation.x = 0); p.arms.forEach(a => a.a.rotation.x = 0); }
function standPose(p) { p.hips.forEach(h => h.rotation.x = PI / 2); p.legs.forEach(l => l.rotation.x = -PI / 2); }
function walkPose(p, t) { if (p.state === 'enter' || p.state === 'leave') { standPose(p); p.body.position.y = 0.3 + Math.abs(Math.sin(t * 9)) * 0.03; p.hips.forEach((h, i) => h.rotation.x = PI / 2 + Math.sin(t * 9 + i * PI) * 0.35); p.arms.forEach((a, i) => a.a.rotation.x = Math.sin(t * 9 + i * PI) * 0.4); } }

/* ---------- sisend ---------- */
const R = { time: 0 };
const rig = new THREE.Group(); rig.position.set(0, 0, 0.45); scene.add(rig); rig.add(camera);
const ray = new THREE.Raycaster(); const ctrls = [];
function ctrlRay(c, o, d) { _m.identity().extractRotation(c.matrixWorld); o.setFromMatrixPosition(c.matrixWorld); d.set(0, 0, -1).applyMatrix4(_m).normalize(); }
function grab(t, holder, hand) { if (t.held) return; t.held = hand; t.home = false; holder.add(t.g); t.g.position.set(0, 0, -0.02); t.g.quaternion.identity(); if (hand === 'desk') { t.g.position.set(0.18, -0.2, -0.4); t.g.rotation.set(0.25, 0.25, 0); } sfx.grab(); haptic(hand, 0.4, 40); if (t.g.userData.beam) t.g.userData.beam.visible = true; }
function release(t) { if (!t || !t.held) return; t.held = null; scene.attach(t.g); t.returning = 0; sfx.drop(); if (t.g.userData.beam) t.g.userData.beam.visible = false; }
function nearestTool(pos, maxD) { let best = null, bd = maxD; for (const t of Object.values(TOOLS)) { if (t.held) continue; const d = t.g.getWorldPosition(_v).distanceTo(pos); if (d < bd) { bd = d; best = t; } } return best; }
function tryGrab(c) {
  const o = new V3(), d = new V3(); ctrlRay(c, o, d);
  ray.set(o, d); ray.far = 4; const ui = ray.intersectObjects(UIT, false)[0]; if (ui) { ui.object.userData.panel.click(ui.uv); return; }
  if (c.userData.tool) return; let t = nearestTool(o, 0.14);
  if (!t) { const hits = ray.intersectObjects(Object.values(TOOLS).filter(t => !t.held).map(t => t.hit), false); if (hits[0]) t = TOOLS[hits[0].object.userData.tool]; }
  if (t) { grab(t, c, c.userData.hand); c.userData.tool = t; }
}
function buildControllers() {
  const f = new XRControllerModelFactory();
  for (let i = 0; i < 2; i++) {
    const c = renderer.xr.getController(i); c.userData = {}; const gr = renderer.xr.getControllerGrip(i);
    c.addEventListener('connected', e => { c.userData.hand = e.data.handedness; c.userData.src = e.data; });
    const down = () => { initAudio(); tryGrab(c); }, up = () => { if (c.userData.tool) { release(c.userData.tool); c.userData.tool = null; } };
    c.addEventListener('selectstart', down); c.addEventListener('selectend', up); c.addEventListener('squeezestart', down); c.addEventListener('squeezeend', up);
    const line = new THREE.Line(new THREE.BufferGeometry().setFromPoints([new V3(), new V3(0, 0, -1)]), new THREE.LineBasicMaterial({ color: '#3aa0c8', transparent: true, opacity: 0.6 })); c.add(line); c.userData.line = line;
    rig.add(c); gr.add(f.createControllerModel(gr)); rig.add(gr); ctrls.push(c);
  }
}
// arvutis
let deskTool = null; const deskHand = new THREE.Group(); camera.add(deskHand);
const cvs = renderer.domElement; cvs.addEventListener('contextmenu', e => e.preventDefault());
function deskRay() { const o = new V3(), d = new V3(); camera.getWorldPosition(o); camera.getWorldDirection(d); return [o, d]; }
cvs.addEventListener('mousedown', e => {
  initAudio(); if (document.pointerLockElement !== cvs) { cvs.requestPointerLock?.(); return; }
  const [o, d] = deskRay(); ray.set(o, d); ray.far = 5;
  if (e.button === 2) { if (deskTool) { release(deskTool); deskTool = null; } return; }
  const ui = ray.intersectObjects(UIT, false)[0]; if (ui) { ui.object.userData.panel.click(ui.uv); return; }
  const hits = ray.intersectObjects(Object.values(TOOLS).filter(t => !t.held).map(t => t.hit), false);
  if (hits[0]) { if (deskTool) release(deskTool); deskTool = TOOLS[hits[0].object.userData.tool]; grab(deskTool, deskHand, 'desk'); }
});
addEventListener('mousemove', e => { if (document.pointerLockElement !== cvs || renderer.xr.isPresenting) return; rig.rotation.y -= e.movementX * 0.0022; camera.rotation.x = clamp(camera.rotation.x - e.movementY * 0.0022, -1.3, 1.2); });
addEventListener('keydown', e => { initAudio(); const n = parseInt(e.key); if (n >= 1 && n <= 9) { const t = TOOLS[TOOL_LIST[n - 1][0]]; if (deskTool) release(deskTool); deskTool = t; grab(t, deskHand, 'desk'); } if (e.key === 'Escape' && deskTool) { release(deskTool); deskTool = null; } keys[e.code] = true; });
const keys = {}; addEventListener('keyup', e => keys[e.code] = false);
function deskAim() { // arvutis: tööriist liigub patsiendi poole, kuhu vaatad
  if (!deskTool || renderer.xr.isPresenting || !P) return;
  const [o, d] = deskRay(); ray.set(o, d); ray.far = 2.2; const hits = ray.intersectObject(P.body, true);
  if (hits[0]) { const tgt = hits[0].point.clone().addScaledVector(d, -0.02); const tipLocal = deskTool.tip.clone(); deskTool.g.position.copy(deskHand.worldToLocal(tgt.clone())).sub(tipLocal.applyQuaternion(deskTool.g.quaternion)); }
  else { deskTool.g.position.set(0.18, -0.2, -0.4); }
}
function updateMove(dt) {
  let lx = 0, ly = 0; const s = renderer.xr.getSession();
  if (s) for (const src of s.inputSources) { if (src.handedness === 'left' && src.gamepad) { const a = src.gamepad.axes; lx = a.length >= 4 ? a[2] : a[0]; ly = a.length >= 4 ? a[3] : a[1]; } }
  else { lx = (keys.KeyD ? 1 : 0) - (keys.KeyA ? 1 : 0); ly = (keys.KeyS ? 1 : 0) - (keys.KeyW ? 1 : 0); }
  if (Math.abs(lx) < 0.15) lx = 0; if (Math.abs(ly) < 0.15) ly = 0; if (!lx && !ly) return;
  camera.getWorldDirection(_v); _v.y = 0; _v.normalize(); const sp = 1.4 * dt;
  rig.position.x = clamp(rig.position.x + (_v.x * -ly - _v.z * lx) * sp, -2.6, 2.6); rig.position.z = clamp(rig.position.z + (_v.z * -ly + _v.x * lx) * sp, -0.35, 1.7);
}
function updateTools(dt) {
  for (const t of Object.values(TOOLS)) { if (t.held || t.home) continue; t.returning += dt * 3; t.g.position.lerp(t.slot, clamp(t.returning, 0, 1)); t.g.quaternion.slerp(t.slotQ, clamp(t.returning, 0, 1)); if (t.returning >= 1) { t.home = true; t.g.position.copy(t.slot); t.g.quaternion.copy(t.slotQ); } }
  const step = P && P.state === 'sit' ? P.case.steps[P.step] : null;
  if (S.easy && step) { const t = TOOLS[step.tool]; if (!t.held) { highlight.visible = true; highlight.position.copy(t.g.getWorldPosition(_v)); highlight.scale.setScalar(0.15 + Math.sin(R.time * 5) * 0.03); } else { highlight.visible = true; highlight.position.copy(zoneWorld(P, step.zones[0])); highlight.scale.setScalar(0.2 + Math.sin(R.time * 5) * 0.04); } } else highlight.visible = false;
  for (const p of popups) { if (p.life <= 0) continue; p.life -= dt; p.s.position.y += dt * 0.08; p.s.material.opacity = clamp(p.life, 0, 1); if (p.life <= 0) p.s.visible = false; }
}
function updatePointers() {
  for (const Pn of PANELS) Pn.hoverNext = -1;
  const mark = (o, d) => { ray.set(o, d); ray.far = 4; const h = ray.intersectObjects(UIT, false)[0]; if (h) h.object.userData.panel.hoverNext = h.object.userData.panel.indexAt(h.uv); return h; };
  if (renderer.xr.isPresenting) for (const c of ctrls) { const o = new V3(), d = new V3(); ctrlRay(c, o, d); const h = mark(o, d); c.userData.line.visible = !c.userData.tool; c.userData.line.scale.z = h ? h.distance : 1.5; }
  else { const [o, d] = deskRay(); mark(o, d); }
  for (const Pn of PANELS) if (Pn.hoverNext !== Pn.hover) { Pn.hover = Pn.hoverNext; Pn.refresh(); }
}

/* ---------- käivitus ---------- */
buildRoom(); buildTools(); buildUI(); buildConfetti(); buildControllers();
camera.position.set(0, 1.55, 0); camera.rotation.x = -0.22;
document.body.appendChild(VRButton.createButton(renderer, { optionalFeatures: ['local-floor', 'bounded-floor', 'hand-tracking'] }));
renderer.xr.addEventListener('sessionstart', () => { initAudio(); rig.position.set(0, 0, 0.05); camera.position.set(0, 0, 0); camera.rotation.set(0, 0, 0); $('info').style.display = 'none'; $('cross').style.display = 'none'; if (deskTool) { release(deskTool); deskTool = null; } });
renderer.xr.addEventListener('sessionend', () => { rig.position.set(0, 0, 0.45); camera.position.set(0, 1.5, 0); $('cross').style.display = ''; });
const clock = new THREE.Clock(); let acc = 0;
function step(dt) { R.time += dt; updateMove(dt); deskAim(); updatePatient(dt); scene.updateMatrixWorld(); treat(dt); updateTools(dt); updateConfetti(dt); updatePointers(); music(dt); }
renderer.setAnimationLoop(() => { const dt = Math.min(clock.getDelta(), 0.05); step(dt); renderer.render(scene, camera); });
window.G = { S, TOOLS, get P() { return P; }, grab, release, deskHand, step, camera, rig, sim: s => { for (let t = 0; t < s; t += 0.05) step(0.05); }, zoneWorld };
</script>
</body>
</html>
