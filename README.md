[index (39).html](https://github.com/user-attachments/files/33910910/index.39.html)
<!doctype html>
<html lang="et">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1">
<title>Doktor VR</title>
<style>
  html,body{margin:0;height:100%;overflow:hidden;background:#dff0f6;font-family:system-ui,-apple-system,"Segoe UI",sans-serif}
  canvas{display:block}
  #info{position:fixed;left:14px;top:14px;max-width:350px;padding:14px 16px;background:rgba(255,255,255,.93);border:2px solid #3aa0c8;border-radius:12px;font-size:13px;line-height:1.55;color:#1a3a4a;z-index:5}
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
  Päästik/haare – võta tööriist. Iga tööriist töötab päriselt: hõõru, keri, kalluta, koputa, lase lahti (kraadiklaas suhu) jne. Kaart ütleb, mida teha.<br>
  Tühja käega: silita looma, kõdita, tee elustamist (vajuta rinda).<br>
  Enne patsienti pese kraanikausis käed! Vasak pulk – liigu.<br>
  <b>Arvutis:</b> klõpsa ekraanil, hiirega vaatad. WASD liigud. Klõpsa tööriista, siis vaata kehaosa poole. Parem klõps – pane tagasi. Tühja käega klõps patsiendile = silita/kõdita. Klõps kraanikausile = pese käsi. Tööriist käes: hoia vasakut nuppu = kalluta/koputa, liiguta hiirt = hõõru. Parem klõps = lase lahti (jääb patsiendile, kui oled õiges kohas). Tühik = rindkere vajutus.
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
const _v = new V3(), _v2 = new V3(), _m = new THREE.Matrix4(), _q = new THREE.Quaternion(), _e = new THREE.Euler();
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
const hemi = new THREE.HemisphereLight('#ffffff', '#b8c8d0', 1.6); scene.add(hemi);
const key = new THREE.DirectionalLight('#fff6ea', 1.6); key.position.set(1.5, 3.2, 1.2); key.castShadow = true; key.shadow.mapSize.set(1024, 1024);
Object.assign(key.shadow.camera, { left: -3, right: 3, top: 3, bottom: -3, near: 0.5, far: 8 }); key.shadow.bias = -0.0008; scene.add(key);
const lamp = new THREE.PointLight('#ffffff', 6, 6, 1.5); lamp.position.set(0, 2.6, -0.8); scene.add(lamp);

/* ---------- abi ---------- */
function cnv(w, h) { const c = document.createElement('canvas'); c.width = w; c.height = h; return c; }
function ctex(c) { const t = new THREE.CanvasTexture(c); t.colorSpace = THREE.SRGBColorSpace; return t; }
function rr(g, x, y, w, h, r) { g.beginPath(); g.moveTo(x + r, y); g.arcTo(x + w, y, x + w, y + h, r); g.arcTo(x + w, y + h, x, y + h, r); g.arcTo(x, y + h, x, y, r); g.arcTo(x, y, x + w, y, r); g.closePath(); }
function txt(g, s, x, y, size, col, align = 'left', bold = false) { g.font = `${bold ? 'bold ' : ''}${size}px system-ui,sans-serif`; g.fillStyle = col; g.textAlign = align; g.textBaseline = 'middle'; g.fillText(s, x, y); }
function fit(g, s, x, y, size, col, maxW, align = 'center', bold = true) { let fs = size; g.font = `${bold ? 'bold ' : ''}${fs}px system-ui`; while (g.measureText(s).width > maxW && fs > 14) { fs -= 2; g.font = `${bold ? 'bold ' : ''}${fs}px system-ui`; } txt(g, s, x, y, fs, col, align, bold); }
const M = (color, o = {}) => new THREE.MeshStandardMaterial(Object.assign({ color, roughness: 0.6 }, o));
function mesh(geo, mat, x = 0, y = 0, z = 0, parent = scene) { const m = new THREE.Mesh(geo, mat); m.position.set(x, y, z); m.castShadow = true; m.receiveShadow = true; parent.add(m); return m; }
function cylBetween(a, b, r, mat, parent) { const d = new V3().subVectors(b, a); const m = new THREE.Mesh(new THREE.CylinderGeometry(r, r, d.length(), 12), mat); m.position.copy(a).add(b).multiplyScalar(0.5); m.quaternion.setFromUnitVectors(new V3(0, 1, 0), d.normalize()); m.castShadow = true; parent.add(m); return m; }
function glowTex() { const c = cnv(64, 64), g = c.getContext('2d'); const gr = g.createRadialGradient(32, 32, 0, 32, 32, 32); gr.addColorStop(0, 'rgba(255,255,255,1)'); gr.addColorStop(1, 'rgba(255,255,255,0)'); g.fillStyle = gr; g.fillRect(0, 0, 64, 64); return ctex(c); }
const GLOW = glowTex();

/* ---------- heli ja hääl ---------- */
let AC = null, master;
function initAudio() { if (AC) { if (AC.state === 'suspended') AC.resume(); return; } try { AC = new (window.AudioContext || window.webkitAudioContext)(); master = AC.createGain(); master.gain.value = 0.7; master.connect(AC.destination); } catch (e) { } }
function tone(f1, f2, dur, type = 'sine', vol = 0.2, delay = 0) { if (!AC) return; const t = AC.currentTime + delay; const o = AC.createOscillator(), g = AC.createGain(); o.type = type; o.frequency.setValueAtTime(f1, t); o.frequency.exponentialRampToValueAtTime(Math.max(1, f2), t + dur); g.gain.setValueAtTime(vol, t); g.gain.exponentialRampToValueAtTime(0.0001, t + dur); o.connect(g).connect(master); o.start(t); o.stop(t + dur + 0.02); }
function noise(dur, vol, f, delay = 0) { if (!AC) return; const t = AC.currentTime + delay; const b = AC.createBuffer(1, AC.sampleRate * dur | 0, AC.sampleRate), d = b.getChannelData(0); for (let i = 0; i < d.length; i++) d[i] = (Math.random() * 2 - 1) * (1 - i / d.length); const s = AC.createBufferSource(); s.buffer = b; const fl = AC.createBiquadFilter(); fl.type = 'bandpass'; fl.frequency.value = f; const g = AC.createGain(); g.gain.value = vol; s.connect(fl).connect(g).connect(master); s.start(t); }
const sfx = {
  grab: () => tone(700, 1000, 0.06, 'sine', 0.12), drop: () => tone(500, 350, 0.08, 'sine', 0.1), click: () => tone(1000, 800, 0.04, 'square', 0.05),
  good: () => { tone(660, 660, 0.1, 'triangle', 0.15); tone(990, 990, 0.18, 'triangle', 0.15, 0.1); },
  bad: () => tone(240, 170, 0.25, 'sawtooth', 0.06),
  heart: () => { for (let i = 0; i < 3; i++) { tone(70, 50, 0.12, 'sine', 0.5, i * 0.7); tone(60, 45, 0.1, 'sine', 0.4, i * 0.7 + 0.18); } },
  cough: () => { noise(0.18, 0.5, 700); noise(0.15, 0.4, 600, 0.25); },
  aaa: () => { tone(300, 280, 1.0, 'sawtooth', 0.05); tone(900, 850, 1.0, 'sine', 0.04); },
  babble: (hi = 1) => { const n = 3 + (Math.random() * 4 | 0); for (let i = 0; i < n; i++) { const f = (320 + Math.random() * 260) * hi; tone(f, f * (0.8 + Math.random() * 0.5), 0.09, 'triangle', 0.08, i * 0.1); } },
  giggle: () => { for (let i = 0; i < 6; i++) tone(700 + i * 40, 900, 0.07, 'triangle', 0.09, i * 0.09); },
  purr: () => { for (let i = 0; i < 4; i++) noise(0.25, 0.25, 120, i * 0.3); },
  boing: () => tone(200, 600, 0.25, 'sine', 0.2), pop: () => tone(900, 400, 0.08, 'sine', 0.2),
  win: () => [523, 659, 784, 1046, 1318].forEach((f, i) => tone(f, f, 0.3, 'triangle', 0.14, i * 0.1)),
  spray: () => noise(0.35, 0.25, 4000), door: () => { tone(220, 180, 0.2, 'triangle', 0.1); noise(0.2, 0.2, 500); },
  step: () => tone(120, 90, 0.05, 'sine', 0.08), brush: () => noise(0.12, 0.2, 3000), drill: () => tone(1800, 1900, 0.15, 'square', 0.03),
  water: () => noise(0.5, 0.15, 1500), zap: () => { noise(0.4, 0.7, 2500); tone(120, 60, 0.4, 'sawtooth', 0.2); },
  tick: () => tone(1500, 1500, 0.03, 'square', 0.04), beep: () => tone(1000, 1000, 0.12, 'sine', 0.08),
  siren: () => { for (let i = 0; i < 4; i++) { tone(700, 700, 0.3, 'square', 0.04, i * 0.6); tone(950, 950, 0.3, 'square', 0.04, i * 0.6 + 0.3); } },
  coin: () => { tone(988, 988, 0.08, 'square', 0.06); tone(1318, 1318, 0.2, 'square', 0.06, 0.08); },
};
let etVoice = null; try { const pv = () => { etVoice = speechSynthesis.getVoices().find(v => v.lang && v.lang.toLowerCase().startsWith('et')) || null; }; pv(); speechSynthesis.onvoiceschanged = pv; } catch (e) { }
function speak(text, pitch = 1.3) { if (!etVoice) return false; try { speechSynthesis.cancel(); const u = new SpeechSynthesisUtterance(text.replace(/[★✓🧼]/g, '')); u.voice = etVoice; u.lang = etVoice.lang; u.pitch = Math.min(2, pitch); u.rate = 1.05; speechSynthesis.speak(u); return true; } catch (e) { return false; } }
let musT = 0; const MEL = [523, 587, 659, 784, 880, 784, 659, 587];
function music(dt) { if (!AC || !S.music) return; musT -= dt; if (musT > 0) return; musT = 0.45; if (Math.random() < 0.55) { const f = pick(MEL); tone(f, f, 0.5, 'triangle', 0.025); } }
function haptic(hand, v, ms) { const s = renderer.xr.getSession(); if (!s) return; for (const src of s.inputSources) if (src.handedness === hand) src.gamepad?.hapticActuators?.[0]?.pulse?.(v, ms); }

/* ---------- olek ---------- */
const SAVE = 'doktor_v1';
const S = Object.assign({ cured: 0, stars: 0, owned: [], easy: true, music: true, vet: false }, (() => { try { return JSON.parse(localStorage.getItem(SAVE)) || {}; } catch (e) { return {}; } })());
S.wallet ??= S.stars; S.owned ??= [];
const save = () => { try { localStorage.setItem(SAVE, JSON.stringify(S)); } catch (e) { } };
const R = { time: 0, washed: false, washT: 0, washMsg: -9, cpr: 0, chart: 0, deskWash: 0 };

/* ---------- kabinet ---------- */
function wallTex(c1, c2) { const c = cnv(256, 256), g = c.getContext('2d'); g.fillStyle = c1; g.fillRect(0, 0, 256, 256); g.fillStyle = c2; g.fillRect(0, 180, 256, 8); for (let i = 0; i < 1500; i++) { g.fillStyle = `rgba(0,0,0,${Math.random() * 0.03})`; g.fillRect(Math.random() * 256, Math.random() * 256, 2, 2); } const t = ctex(c); t.wrapS = t.wrapT = THREE.RepeatWrapping; return t; }
function floorTex() { const c = cnv(256, 256), g = c.getContext('2d'); for (let y = 0; y < 4; y++) for (let x = 0; x < 4; x++) { g.fillStyle = (x + y) % 2 ? '#e8eef0' : '#cfe0e6'; g.fillRect(x * 64, y * 64, 64, 64); } const t = ctex(c); t.wrapS = t.wrapT = THREE.RepeatWrapping; t.repeat.set(6, 6); return t; }
function poster(lines, bg, w, h) { const c = cnv(512, 640), g = c.getContext('2d'); g.fillStyle = bg; g.fillRect(0, 0, 512, 640); g.fillStyle = '#fff'; rr(g, 30, 30, 452, 580, 30); g.fill(); lines.forEach(([s, sz, col], i) => fit(g, s, 256, 120 + i * 110, sz, col, 420)); return new THREE.Mesh(new THREE.PlaneGeometry(w, h), new THREE.MeshStandardMaterial({ map: ctex(c), roughness: 0.8 })); }
function label(text, w, col = '#2a8ab8') { const c = cnv(512, 128), g = c.getContext('2d'); g.fillStyle = '#ffffff'; rr(g, 4, 4, 504, 120, 30); g.fill(); fit(g, text, 256, 66, 64, col, 480); return new THREE.Mesh(new THREE.PlaneGeometry(w, w / 4), new THREE.MeshBasicMaterial({ map: ctex(c), transparent: true })); }
const ROOM = {}; const OBST = [];
function buildRoom() {
  const W = 6, D = 6, H = 2.9;
  const floor = mesh(new THREE.PlaneGeometry(W, D).rotateX(-PI / 2), M('#ffffff', { map: floorTex(), roughness: 0.4 }), 0, 0, -1); floor.castShadow = false;
  const wm = M('#ffffff', { map: wallTex('#bfe4ef', '#8ccbe0'), roughness: 0.9 });
  for (const [x, y, z, ry] of [[0, H / 2, -4, 0], [0, H / 2, 2, PI], [-3, H / 2, -1, PI / 2], [3, H / 2, -1, -PI / 2]]) { const w = mesh(new THREE.PlaneGeometry(W, H), wm, x, y, z); w.rotation.y = ry; w.castShadow = false; }
  mesh(new THREE.PlaneGeometry(W, D).rotateX(PI / 2), M('#ffffff', { roughness: 1 }), 0, H, -1).castShadow = false;
  const sky = cnv(256, 256), sg = sky.getContext('2d'); const gr = sg.createLinearGradient(0, 0, 0, 256); gr.addColorStop(0, '#5aa8e8'); gr.addColorStop(0.7, '#bfe4ff'); gr.addColorStop(0.72, '#6ab04a'); gr.addColorStop(1, '#4a8a3a'); sg.fillStyle = gr; sg.fillRect(0, 0, 256, 256); sg.fillStyle = '#fff'; for (let i = 0; i < 5; i++) { sg.beginPath(); sg.ellipse(40 + i * 50, 50 + (i % 2) * 30, 28, 12, 0, 0, PI * 2); sg.fill(); }
  const win = mesh(new THREE.PlaneGeometry(1.6, 1.1), new THREE.MeshBasicMaterial({ map: ctex(sky) }), 2.99, 1.6, -2.4); win.rotation.y = -PI / 2;
  mesh(new THREE.BoxGeometry(0.06, 2.1, 1.0), M('#f4f0e8'), -2.97, 1.05, -2.6); mesh(new THREE.SphereGeometry(0.04, 12, 8), M('#c0a040', { metalness: 0.8 }), -2.92, 1.0, -2.2);
  const bed = new THREE.Group(); bed.position.set(0, 0, -0.95); scene.add(bed);
  mesh(new THREE.BoxGeometry(0.8, 0.5, 1.9), M('#e8ecef'), 0, 0.25, -0.55, bed); mesh(new THREE.BoxGeometry(0.82, 0.08, 1.92), M('#7ac8e0', { roughness: 0.8 }), 0, 0.53, -0.55, bed);
  mesh(new THREE.BoxGeometry(0.6, 0.12, 0.35), M('#ffffff', { roughness: 1 }), 0, 0.63, -1.25, bed);
  OBST.push([-0.42, 0.42, -2.48, -0.48]);
  const tray = (x, z, ry, w, d) => { const t = new THREE.Group(); t.position.set(x, 0, z); t.rotation.y = ry; scene.add(t); mesh(new THREE.BoxGeometry(w, 0.03, d), M('#d8dde0', { metalness: 0.6, roughness: 0.3 }), 0, 0.86, 0, t); for (const [a, b] of [[-1, -1], [1, -1], [-1, 1], [1, 1]]) mesh(new THREE.CylinderGeometry(0.015, 0.015, 0.85, 8), M('#b0b8bc', { metalness: 0.7 }), a * (w / 2 - 0.04), 0.425, b * (d / 2 - 0.04), t); return t; };
  ROOM.trayR = tray(0.55, -0.22, -0.35, 0.62, 0.46); ROOM.trayL = tray(-0.8, -0.15, 0.4, 0.84, 0.34);
  mesh(new THREE.BoxGeometry(1.2, 0.05, 0.6), M('#e8d8c0'), -1.9, 0.75, -1.2); for (const [x, z] of [[-2.45, -1.45], [-1.35, -1.45], [-2.45, -0.95], [-1.35, -0.95]]) mesh(new THREE.BoxGeometry(0.05, 0.75, 0.05), M('#c8b898'), x, 0.375, z);
  OBST.push([-2.55, -1.25, -1.55, -0.85]);
  ROOM.chairs = [];
  for (let i = 0; i < 3; i++) { const x = -2.2 + i * 0.7, z = -3.55; mesh(new THREE.BoxGeometry(0.45, 0.06, 0.45), M('#f0a050'), x, 0.45, z); mesh(new THREE.BoxGeometry(0.45, 0.5, 0.06), M('#f0a050'), x, 0.72, z - 0.22); for (const [a, b] of [[-1, -1], [1, -1], [-1, 1], [1, 1]]) mesh(new THREE.CylinderGeometry(0.015, 0.015, 0.45, 6), M('#888'), x + a * 0.19, 0.225, z + b * 0.19); ROOM.chairs.push(new V3(x, 0, z - 0.05)); }
  OBST.push([-2.6, -0.6, -4, -3.2]);
  const sm = label('OOTERUUM', 0.8, '#f08a30'); sm.position.set(-1.5, 1.6, -3.99); scene.add(sm);
  mesh(new THREE.BoxGeometry(1.0, 1.8, 0.45), M('#ffffff'), 1.9, 0.9, -3.7); mesh(new THREE.BoxGeometry(0.42, 0.3, 0.02), M('#d04040'), 1.9, 1.5, -3.47); mesh(new THREE.BoxGeometry(0.06, 0.2, 0.025), M('#ffffff'), 1.9, 1.5, -3.455); mesh(new THREE.BoxGeometry(0.2, 0.06, 0.025), M('#ffffff'), 1.9, 1.5, -3.455);
  OBST.push([1.35, 2.45, -4, -3.4]);
  const p2 = poster([['OLE TERVE!', 60, '#d04a6a'], ['🍎 Söö puuvilju', 40, '#555'], ['🏃 Liigu palju', 40, '#555'], ['😴 Maga hästi', 40, '#555']], '#f4a0b8', 0.6, 0.75); p2.position.set(-2.99, 1.6, -0.9); p2.rotation.y = PI / 2; scene.add(p2);
  const sink = new THREE.Group(); sink.position.set(-1.3, 0, 0.75); sink.rotation.y = PI / 2; scene.add(sink); ROOM.sink = sink;
  mesh(new THREE.BoxGeometry(0.6, 0.8, 0.45), M('#ffffff'), 0, 0.4, 0, sink); mesh(new THREE.CylinderGeometry(0.2, 0.15, 0.1, 24, 1, true), M('#e8f0f4', { side: THREE.DoubleSide, roughness: 0.2 }), 0, 0.85, 0, sink);
  const chrome = M('#c8ccd0', { metalness: 0.9, roughness: 0.2 }); mesh(new THREE.CylinderGeometry(0.02, 0.02, 0.25, 10), chrome, 0, 0.97, -0.17, sink); mesh(new THREE.CylinderGeometry(0.018, 0.018, 0.14, 10).rotateX(PI / 2), chrome, 0, 1.09, -0.11, sink);
  const water = mesh(new THREE.CylinderGeometry(0.012, 0.015, 0.22, 8), new THREE.MeshStandardMaterial({ color: '#8ad0ff', transparent: true, opacity: 0.6, roughness: 0.1 }), 0, 0.97, -0.04, sink); water.visible = false; ROOM.water = water;
  mesh(new THREE.CylinderGeometry(0.035, 0.035, 0.13, 12), M('#60c0f0'), 0.2, 0.87, -0.15, sink);
  const ws = label('🧼 PESE KÄSI', 0.5); ws.position.set(0, 1.35, -0.2); sink.add(ws);
  const sinkHit = new THREE.Mesh(new THREE.BoxGeometry(0.6, 0.6, 0.5), new THREE.MeshBasicMaterial({ visible: false })); sinkHit.position.set(0, 1.0, 0); sink.add(sinkHit); ROOM.sinkHit = sinkHit;
  OBST.push([-1.55, -1.05, 0.45, 1.05]);
}

/* ---------- tööriistad ---------- */
const TOOLS = {};
const TOOL_R = [['kraadiklaas', 'Kraadiklaas'], ['stetoskoop', 'Stetoskoop'], ['taskulamp', 'Taskulamp'], ['vatitups', 'Vatitups'], ['plaaster', 'Plaaster'], ['side', 'Side / kips'], ['rohi', 'Rohi'], ['sustal', 'Süstal'], ['haamer', 'Refleksihaamer']];
const TOOL_L = [['rontgen', 'Röntgen'], ['mansett', 'Vererõhk'], ['prillid', 'Prillid'], ['hambahari', 'Hambahari'], ['puur', 'Hambapuur'], ['pintsetid', 'Pintsetid'], ['kamm', 'Kamm'], ['defi', 'Defibrillaator'], ['kleeps', 'Kleeps'], ['komm', 'Komm']];
const TOOL_LIST = [...TOOL_R, ...TOOL_L];
const goldM = M('#e8c040', { metalness: 0.9, roughness: 0.25 });
function buildTool(id) {
  const g = new THREE.Group(); const tip = new V3(0, 0, -0.1);
  const white = M('#ffffff'), metal = S.owned.includes('kuld') ? goldM : M('#c8ccd0', { metalness: 0.8, roughness: 0.25 }), cz = (r, l) => new THREE.CylinderGeometry(r, r, l, 12).rotateX(PI / 2);
  const T = (geo, mat, x, y, z) => mesh(geo, mat, x, y, z, g);
  switch (id) {
    case 'kraadiklaas': { T(cz(0.008, 0.14), white, 0, 0, -0.04); T(new THREE.SphereGeometry(0.01, 10, 8), M('#e03030'), 0, 0, -0.11); T(new THREE.BoxGeometry(0.03, 0.014, 0.05), M('#2a6a8a'), 0, 0.008, 0.01); const dc = cnv(128, 64); const dm = new THREE.Mesh(new THREE.PlaneGeometry(0.026, 0.044), new THREE.MeshBasicMaterial({ map: ctex(dc) })); dm.rotation.set(-PI / 2, 0, PI / 2); dm.position.set(0, 0.0152, 0.01); g.add(dm); g.userData.disp = { c: dc, m: dm }; tip.set(0, 0, -0.11); break; }
    case 'stetoskoop': { T(new THREE.CylinderGeometry(0.022, 0.022, 0.012, 20).rotateX(PI / 2), metal, 0, 0, -0.1); const t = new THREE.Mesh(new THREE.TorusGeometry(0.05, 0.006, 8, 24, PI * 1.3), M('#303a44')); t.rotation.x = PI / 2; t.position.set(0, 0, -0.04); g.add(t); T(cz(0.005, 0.06), M('#303a44'), 0, 0, -0.075); tip.set(0, 0, -0.106); break; }
    case 'taskulamp': { T(cz(0.016, 0.13), M('#3a6ac8'), 0, 0, 0); T(new THREE.CylinderGeometry(0.02, 0.016, 0.03, 12).rotateX(PI / 2), metal, 0, 0, -0.075); const l = T(new THREE.CircleGeometry(0.017, 16), new THREE.MeshBasicMaterial({ color: '#fff8c0' }), 0, 0, -0.0905); l.rotation.y = PI; const beam = new THREE.Mesh(new THREE.ConeGeometry(0.08, 0.35, 20, 1, true).rotateX(-PI / 2).translate(0, 0, -0.27), new THREE.MeshBasicMaterial({ color: '#fff8c0', transparent: true, opacity: 0.16, depthWrite: false, side: THREE.DoubleSide })); beam.visible = false; g.add(beam); g.userData.beam = beam; tip.set(0, 0, -0.09); break; }
    case 'vatitups': T(cz(0.004, 0.13), white, 0, 0, -0.03); T(new THREE.SphereGeometry(0.014, 10, 8), M('#ffffff', { roughness: 1 }), 0, 0, -0.1); T(cz(0.018, 0.05), M('#4ab0e0'), 0, 0, 0.05); tip.set(0, 0, -0.1); break;
    case 'plaaster': T(new THREE.BoxGeometry(0.08, 0.004, 0.028), M('#f0c8a0', { roughness: 0.9 }), 0, 0, -0.04); T(new THREE.BoxGeometry(0.028, 0.005, 0.022), white, 0, 0.001, -0.04); tip.set(0, 0, -0.04); break;
    case 'side': T(new THREE.CylinderGeometry(0.03, 0.03, 0.06, 16), M('#ffffff', { roughness: 1 }), 0, 0, -0.05); T(new THREE.CylinderGeometry(0.012, 0.012, 0.062, 10), M('#dddddd'), 0, 0, -0.05); tip.set(0, 0, -0.05); break;
    case 'rohi': T(cz(0.022, 0.07), M('#e868a0'), 0, 0, 0); T(cz(0.014, 0.02), white, 0, 0, -0.045); T(new THREE.BoxGeometry(0.03, 0.03, 0.002), white, 0, 0.0225, 0); tip.set(0, 0, -0.055); break;
    case 'sustal': { T(cz(0.011, 0.08), M('#c8ecff', { transparent: true, opacity: 0.45 }), 0, 0, 0); const liq = T(new THREE.CylinderGeometry(0.009, 0.009, 0.075, 12).rotateX(PI / 2).translate(0, 0, -0.0375), M('#ffd040', { transparent: true, opacity: 0.85 }), 0, 0, 0.036); liq.scale.z = 0.01; const pl = new THREE.Group(); g.add(pl); mesh(cz(0.004, 0.08), M('#4a90e0'), 0, 0, 0.075, pl); mesh(new THREE.BoxGeometry(0.03, 0.004, 0.006), white, 0, 0, 0.115, pl); mesh(cz(0.009, 0.006), M('#303030'), 0, 0, 0.037, pl); T(cz(0.0012, 0.04), metal, 0, 0, -0.06); g.userData.liq = liq; g.userData.plunger = pl; tip.set(0, 0, -0.08); break; }
    case 'haamer': { T(cz(0.005, 0.15), metal, 0, 0, 0); const h = T(new THREE.ConeGeometry(0.022, 0.05, 3), M('#e04040', { roughness: 0.9 }), 0, 0, -0.08); h.rotation.z = PI / 2; tip.set(0, 0, -0.08); break; }
    case 'rontgen': T(new THREE.BoxGeometry(0.06, 0.035, 0.11), M('#e8eef2'), 0, 0, 0); T(new THREE.BoxGeometry(0.04, 0.002, 0.04), new THREE.MeshBasicMaterial({ color: '#40e080' }), 0, 0.019, 0.02); T(new THREE.BoxGeometry(0.05, 0.03, 0.004), new THREE.MeshBasicMaterial({ color: '#60a0ff' }), 0, 0, -0.057); tip.set(0, 0, -0.06); break;
    case 'mansett': T(new THREE.CylinderGeometry(0.035, 0.035, 0.06, 16, 1, true), M('#3a7ac8', { side: THREE.DoubleSide }), 0, 0, -0.05); T(new THREE.SphereGeometry(0.02, 10, 8), M('#d04040'), 0, -0.01, 0.05); T(cz(0.003, 0.06), M('#333'), 0, -0.01, 0.01); tip.set(0, 0, -0.05); break;
    case 'prillid': for (const s of [-1, 1]) { const r = new THREE.Mesh(new THREE.TorusGeometry(0.022, 0.004, 8, 20), M('#2a2a3a')); r.position.set(s * 0.03, 0, -0.04); g.add(r); } T(new THREE.BoxGeometry(0.016, 0.004, 0.004), M('#2a2a3a'), 0, 0.005, -0.04); tip.set(0, 0, -0.04); break;
    case 'hambahari': T(cz(0.006, 0.15), M('#40c0a0'), 0, 0, 0); T(new THREE.BoxGeometry(0.012, 0.014, 0.025), M('#ffffff', { roughness: 1 }), 0, 0.008, -0.07); tip.set(0, 0.008, -0.07); break;
    case 'puur': T(cz(0.007, 0.13), white, 0, 0, 0); T(cz(0.0025, 0.03), metal, 0, -0.005, -0.075); tip.set(0, -0.005, -0.09); break;
    case 'pintsetid': for (const s of [-1, 1]) { const b = T(new THREE.BoxGeometry(0.004, 0.003, 0.11), metal, s * 0.006, 0, -0.04); b.rotation.y = s * 0.06; } tip.set(0, 0, -0.095); break;
    case 'kamm': T(new THREE.BoxGeometry(0.11, 0.012, 0.02), M('#e070b0'), 0, 0, -0.03); for (let i = 0; i < 9; i++) T(new THREE.BoxGeometry(0.004, 0.003, 0.02), M('#e070b0'), -0.045 + i * 0.011, 0, -0.05); tip.set(0, 0, -0.05); break;
    case 'defi': T(new THREE.BoxGeometry(0.03, 0.03, 0.09), M('#f0a020'), 0, 0, 0.02); T(new THREE.CylinderGeometry(0.045, 0.045, 0.012, 20).rotateX(PI / 2), M('#5a5a5a', { metalness: 0.6 }), 0, 0, -0.035); T(new THREE.BoxGeometry(0.02, 0.006, 0.03), new THREE.MeshBasicMaterial({ color: '#ffe040' }), 0, 0.017, 0.02); tip.set(0, 0, -0.042); break;
    case 'kleeps': { const sh = new THREE.Shape(); for (let i = 0; i < 10; i++) { const a = i / 10 * PI * 2 - PI / 2, r = i % 2 ? 0.012 : 0.028; i ? sh.lineTo(Math.cos(a) * r, Math.sin(a) * r) : sh.moveTo(Math.cos(a) * r, Math.sin(a) * r); } const s = T(new THREE.ExtrudeGeometry(sh, { depth: 0.003, bevelEnabled: false }), M('#ffd030', { emissive: '#604000' }), 0, 0, -0.03); s.rotation.x = -PI / 2; tip.set(0, 0, -0.03); break; }
    case 'komm': T(new THREE.SphereGeometry(0.018, 14, 10), M('#e83a6a', { roughness: 0.2 }), 0, 0, -0.04); for (const s of [-1, 1]) { const c = T(new THREE.ConeGeometry(0.015, 0.025, 8), M('#ffd040'), s * 0.03, 0, -0.04); c.rotation.z = s * PI / 2; } tip.set(0, 0, -0.04); break;
  }
  const hit = new THREE.Mesh(new THREE.BoxGeometry(0.08, 0.07, 0.17), new THREE.MeshBasicMaterial({ visible: false })); hit.position.z = -0.03; hit.userData.tool = id; g.add(hit);
  return { id, g, tip, hit, slot: new V3(), slotQ: new THREE.Quaternion(), held: null, home: true };
}
let highlight;
function placeTools(list, tray, cols, sx, sz) {
  const rows = Math.ceil(list.length / cols);
  list.forEach(([id, name], i) => {
    const t = buildTool(id); t.name = name; TOOLS[id] = t;
    const x = (i % cols - (cols - 1) / 2) * sx, z = (((i / cols) | 0) - 0.5 * (rows - 1)) * sz - 0.025;
    const p = new V3(x, 0.89, z); tray.localToWorld(p); t.slot.copy(p); t.slotQ.setFromEuler(new THREE.Euler(0, tray.rotation.y, 0)); t.g.position.copy(p); t.g.quaternion.copy(t.slotQ); scene.add(t.g);
    const c = cnv(256, 48), gg = c.getContext('2d'); rr(gg, 2, 2, 252, 44, 14); gg.fillStyle = 'rgba(255,255,255,0.9)'; gg.fill(); fit(gg, name, 128, 26, 28, '#1a4a5a', 240);
    const lab = new THREE.Mesh(new THREE.PlaneGeometry(0.15, 0.028), new THREE.MeshBasicMaterial({ map: ctex(c), transparent: true })); lab.rotation.x = -PI / 2; lab.position.set(x, 0.877, z + 0.058); tray.add(lab);
  });
}
function buildTools() {
  scene.updateMatrixWorld(); placeTools(TOOL_R, ROOM.trayR, 3, 0.2, 0.14); placeTools(TOOL_L, ROOM.trayL, 5, 0.16, 0.15);
  VIAL = new THREE.Group(); ROOM.trayR.add(VIAL); VIAL.position.set(0.25, 0.875, 0.19); mesh(new THREE.CylinderGeometry(0.018, 0.018, 0.05, 14), M('#e8f4ff', { transparent: true, opacity: 0.5 }), 0, 0.025, 0, VIAL); mesh(new THREE.CylinderGeometry(0.015, 0.015, 0.035, 14), M('#ffd040'), 0, 0.02, 0, VIAL); mesh(new THREE.CylinderGeometry(0.012, 0.012, 0.01, 12), M('#d04040'), 0, 0.055, 0, VIAL);
  const cart = new THREE.Group(); cart.position.set(0.8, 0, -1.25); cart.rotation.y = -0.6; scene.add(cart); mesh(new THREE.BoxGeometry(0.4, 0.7, 0.35), M('#e8ecef'), 0, 0.35, 0, cart); mesh(new THREE.BoxGeometry(0.32, 0.18, 0.26), M('#f0a020'), 0, 0.79, 0, cart); const scr = mesh(new THREE.PlaneGeometry(0.14, 0.08), new THREE.MeshBasicMaterial({ color: '#103020' }), -0.05, 0.84, 0.131, cart);
  DEFI_BTN = mesh(new THREE.CylinderGeometry(0.035, 0.035, 0.03, 20).rotateX(PI / 2), M('#e02020', { emissive: '#400000' }), 0.09, 0.8, 0.14, cart); DEFI_BTN.userData.btn = 'defi';
  const dl = label('DEFIBRILLAATOR', 0.3, '#d06010'); dl.position.set(0, 0.93, 0.14); cart.add(dl); OBST.push([0.55, 1.05, -1.5, -1.0]);
  highlight = new THREE.Sprite(new THREE.SpriteMaterial({ map: GLOW, color: '#ffe060', transparent: true, depthWrite: false, opacity: 0.85 })); highlight.scale.setScalar(0.16); scene.add(highlight);
}
const tipWorld = t => t.g.localToWorld(_v.copy(t.tip));
let VIAL, DEFI_BTN, PUMP = null;

/* ---------- haigused ---------- */
const NAMES = ['Mia', 'Oskar', 'Liisa', 'Robin', 'Emma', 'Karl', 'Sofia', 'Rasmus', 'Lenna', 'Marten', 'Hanna', 'Joosep', 'Eliise', 'Kaspar'];
const ANIMALS = [['Kiisu Miisu', 'cat', '#f0a050'], ['Mõmmi', 'bear', '#8a5a3a'], ['Jänku Juss', 'bunny', '#e8e0d8'], ['Kutsu Muri', 'dog', '#c8a070'], ['Rebane Repsu', 'cat', '#e86a2a']];
const SKIN = ['#f6d0b0', '#e8b890', '#c88a60', '#8a5a3a', '#f8dcc4'], HAIR = ['#3a2a1a', '#e0b050', '#8a3a1a', '#1a1a1a', '#c86a3a'], SHIRT = ['#e85a6a', '#5ab0e0', '#f0c040', '#6ac06a', '#a070e0', '#f08a40'];
const st = (tool, zones, fx, label) => ({ tool, zones, fx, label });
const sp = (special, tool, label) => ({ special, tool, label });
const CASES = [
  { id: 'palavik', say: 'Mul on nii palav ja pea valutab…', steps: [st('kraadiklaas', ['suu', 'otsmik'], 'temp', 'Mõõda palavikku'), st('rohi', ['suu'], 'med', 'Anna palavikurohtu')] },
  { id: 'koha', say: 'Köh-köh! Ma köhin kogu aeg!', steps: [st('stetoskoop', ['rind'], 'lungs', 'Kuula kopse'), st('rohi', ['suu'], 'med', 'Anna köhasiirupit')] },
  { id: 'kurk', say: 'Mu kurk on nii valus…', steps: [st('taskulamp', ['suu'], 'throat', 'Vaata kurku'), st('rohi', ['suu'], 'med', 'Anna kurgurohtu')] },
  { id: 'korv', say: 'Mu kõrv valutab!', steps: [st('taskulamp', ['korv'], 'ear', 'Vaata kõrva'), st('rohi', ['korv'], 'drops', 'Pane kõrvatilku')] },
  { id: 'polv', say: 'Ma kukkusin ja põlv on katki!', wound: 'polv', steps: [st('vatitups', ['polv'], 'clean', 'Puhasta haav'), st('plaaster', ['polv'], 'plaster', 'Pane plaaster')] },
  { id: 'vaktsiin', say: 'Ma tulin vaktsiini saama. Natuke hirmus…', shot: true, steps: [st('vatitups', ['kasi'], 'clean', 'Puhasta käsi'), st('sustal', ['kasi'], 'shot', 'Tee süst'), st('plaaster', ['kasi'], 'plaster', 'Pane plaaster')] },
  { id: 'murd', say: 'Ma kukkusin rattaga ja käsi on haige!', broken: true, steps: [st('rontgen', ['kasi'], 'xray', 'Tee röntgenipilt'), st('side', ['kasi'], 'cast', 'Pane käsi kipsi'), st('rohi', ['suu'], 'med', 'Anna valuvaigistit')] },
  { id: 'kontroll', say: 'Tere! Ma tulin tervisekontrolli.', steps: [st('stetoskoop', ['rind'], 'heart', 'Kuula südant'), st('mansett', ['kasi'], 'bp', 'Mõõda vererõhku'), st('haamer', ['polv'], 'reflex', 'Kontrolli reflekse'), st('kraadiklaas', ['suu', 'otsmik'], 'tempok', 'Mõõda temperatuuri')] },
  { id: 'silmad', say: 'Ma ei näe tahvlit hästi…', steps: [sp('chart', null, 'Näita silmatabelil 3 tähte'), st('prillid', ['silmad'], 'glasses', 'Pane prillid ette')] },
  { id: 'hammas', say: 'Mu hammas valutab!', steps: [st('taskulamp', ['suu'], 'jaw', 'Vaata suhu'), sp('brush', 'hambahari', 'Pese pruunid hambad'), sp('drill', 'puur', 'Paranda mustad augud')] },
  { id: 'kiirabi', say: 'Mul on nii halb…', ambulance: true, steps: [st('stetoskoop', ['rind'], 'noheart', 'Kuula südant'), sp('cpr', null, 'Elusta: vajuta rinda'), st('defi', ['rind'], 'shock', 'Defibrillaator rinnale')] },
];
const VET = [
  { id: 'okas', say: 'Ai-ai, mul on okas käpas!', thorn: true, steps: [st('pintsetid', ['kasi'], 'thorn', 'Tõmba okas välja'), st('plaaster', ['kasi'], 'plaster2', 'Pane plaaster')] },
  { id: 'kirbud', say: 'Mul sügeleb kogu aeg!', fleas: true, steps: [st('kamm', ['otsmik', 'rind'], 'comb', 'Kammi kirbud välja'), st('rohi', ['suu'], 'med', 'Anna kirburohtu')] },
  { id: 'koht', say: 'Mu kõht koriseb ja valutab…', steps: [st('stetoskoop', ['rind', 'koht'], 'tummy', 'Kuula kõhtu'), st('rohi', ['suu'], 'med', 'Anna kõhurohtu')] },
];
const ZONES = { suu: [0, 1.17, 0.15, 0.075], otsmik: [0, 1.34, 0.13, 0.08], silmad: [0, 1.28, 0.15, 0.075], korv: [0.165, 1.24, 0, 0.075], korv2: [-0.165, 1.24, 0, 0.075], rind: [0, 0.94, 0.16, 0.12], koht: [0, 0.76, 0.17, 0.1], kasi: [0.25, 0.83, 0.05, 0.1], polv: [0.1, 0.62, 0.38, 0.09], polv2: [-0.1, 0.62, 0.38, 0.09] };
const ZONE_NAME = { suu: 'suu', otsmik: 'pea', silmad: 'silmad', korv: 'kõrv', rind: 'rind', koht: 'kõht', kasi: 'käsi', polv: 'põlv' };

/* ---------- patsient ---------- */
let P = null; const Q = [], LEAVING = []; let arriveT = 1;
function makePatient() {
  const animal = S.vet || Math.random() < 0.25 ? pick(ANIMALS) : null;
  const pool = animal ? (S.vet ? [...VET, CASES[0], CASES[4], CASES[5], CASES[7]] : [...CASES, ...VET]) : CASES;
  const recent = [P, ...Q].filter(Boolean).map(x => x.case.id); const fresh = pool.filter(c => !recent.includes(c.id)); const cs = pick(fresh.length ? fresh : pool);
  const skin = animal ? animal[2] : pick(SKIN), shirt = pick(SHIRT);
  const root = new THREE.Group(); scene.add(root); const body = new THREE.Group(); root.add(body);
  const sk = M(skin, { roughness: 0.7 }), sh = M(shirt, { roughness: 0.8 }), pants = M(pick(['#3a4a8a', '#4a4a4a', '#6a4a8a', '#2a6a6a']));
  mesh(new THREE.CapsuleGeometry(0.16, 0.22, 6, 16), sh, 0, 0.86, 0, body); mesh(new THREE.CylinderGeometry(0.05, 0.05, 0.08, 10), sk, 0, 1.07, 0, body);
  const head = new THREE.Group(); head.position.set(0, 1.25, 0); body.add(head); mesh(new THREE.SphereGeometry(0.16, 24, 18), sk, 0, 0, 0, head);
  const eyeW = M('#ffffff', { roughness: 0.3 }), eyeB = M('#1a1a2a', { roughness: 0.2 });
  const eyes = []; for (const s of [-1, 1]) { const eg = new THREE.Group(); eg.position.set(s * 0.055, 0.03, 0.135); head.add(eg); mesh(new THREE.SphereGeometry(0.03, 12, 10), eyeW, 0, 0, 0, eg); mesh(new THREE.SphereGeometry(0.018, 10, 8), eyeB, 0, 0.002, 0.023, eg); eyes.push(eg); }
  const cheekM = new THREE.MeshBasicMaterial({ color: '#ff8a9a', transparent: true, opacity: 0.5 }); for (const s of [-1, 1]) { const c = mesh(new THREE.CircleGeometry(0.028, 16), cheekM, s * 0.095, -0.03, 0.128, head); c.rotation.y = s * 0.6; }
  const mouthM = M('#8a2a2a'); const smile = new THREE.Mesh(new THREE.TorusGeometry(0.035, 0.008, 6, 16, PI), mouthM); smile.position.set(0, -0.065, 0.148); smile.rotation.z = PI; head.add(smile);
  const frown = new THREE.Mesh(new THREE.TorusGeometry(0.03, 0.008, 6, 16, PI), mouthM); frown.position.set(0, -0.085, 0.148); head.add(frown);
  const open = mesh(new THREE.SphereGeometry(0.035, 12, 10), M('#5a1a1a'), 0, -0.075, 0.13, head); open.scale.set(1, 1.2, 0.5); open.visible = false;
  const glasses = new THREE.Group(); for (const s of [-1, 1]) { const r = new THREE.Mesh(new THREE.TorusGeometry(0.035, 0.005, 8, 20), M('#2a2a3a')); r.position.set(s * 0.055, 0.03, 0.165); glasses.add(r); } const br = new THREE.Mesh(new THREE.BoxGeometry(0.035, 0.006, 0.006), M('#2a2a3a')); br.position.set(0, 0.035, 0.168); glasses.add(br); glasses.visible = false; head.add(glasses);
  if (animal) { const t = animal[1];
    if (t === 'cat' || t === 'dog') for (const s of [-1, 1]) { const e = mesh(new THREE.ConeGeometry(0.05, t === 'cat' ? 0.1 : 0.12, 4), sk, s * 0.1, 0.15, 0, head); e.rotation.z = -s * (t === 'dog' ? 2.6 : 0.35); }
    if (t === 'bear') for (const s of [-1, 1]) mesh(new THREE.SphereGeometry(0.05, 12, 10), sk, s * 0.12, 0.13, 0, head);
    if (t === 'bunny') for (const s of [-1, 1]) { const e = mesh(new THREE.CapsuleGeometry(0.025, 0.14, 4, 8), sk, s * 0.06, 0.24, 0, head); e.rotation.z = -s * 0.15; }
    mesh(new THREE.SphereGeometry(0.022, 10, 8), M('#2a1a1a'), 0, -0.015, 0.16, head); mesh(new THREE.SphereGeometry(0.05, 10, 8), sk, 0, 0.62, -0.2, body); }
  else { const hair = mesh(new THREE.SphereGeometry(0.168, 20, 12, 0, PI * 2, 0, PI * 0.5), M(pick(HAIR), { roughness: 0.9 }), 0, 0.01, -0.01, head); hair.rotation.x = -0.25; }
  const arms = []; for (const s of [-1, 1]) { const a = new THREE.Group(); a.position.set(s * 0.21, 1.0, 0); body.add(a); cylBetween(new V3(0, 0, 0), new V3(s * 0.04, -0.24, 0.04), 0.045, sh, a); const fa = new THREE.Group(); fa.position.set(s * 0.04, -0.24, 0.04); a.add(fa); cylBetween(new V3(0, 0, 0), new V3(-s * 0.04, -0.06, 0.2), 0.04, sk, fa); mesh(new THREE.SphereGeometry(0.045, 10, 8), sk, -s * 0.04, -0.06, 0.22, fa); arms.push({ a, fa }); }
  const legs = [], hips = []; for (const s of [-1, 1]) { const hip = new THREE.Group(); hip.position.set(s * 0.1, 0.62, 0); body.add(hip); cylBetween(new V3(0, 0, 0), new V3(0, 0, 0.38), 0.065, pants, hip); mesh(new THREE.SphereGeometry(0.066, 12, 10), pants, 0, 0, 0.38, hip); const shin = new THREE.Group(); shin.position.set(0, 0, 0.38); hip.add(shin); cylBetween(new V3(0, 0, 0), new V3(0, -0.5, 0.03), 0.055, sk, shin); mesh(new THREE.BoxGeometry(0.1, 0.07, 0.17), M('#f4f4f4'), 0, -0.53, 0.07, shin); legs.push(shin); hips.push(hip); }
  const wound = (z) => { const g = new THREE.Group(); const zz = ZONES[z]; const n = z === 'polv' ? new V3(0, 0.4, 1).normalize() : new V3(zz[0], 0, zz[2]).normalize(); const pos = new V3(zz[0], zz[1], zz[2]).addScaledVector(n, z === 'polv' ? 0.06 : 0.04); g.position.copy(pos); g.lookAt(pos.clone().add(n)); const r = new THREE.Mesh(new THREE.CircleGeometry(0.03, 16), new THREE.MeshBasicMaterial({ color: '#c82a2a' })); g.add(r); const dirt = new THREE.Mesh(new THREE.CircleGeometry(0.035, 10), new THREE.MeshBasicMaterial({ color: '#6a4a2a', transparent: true, opacity: 0.6 })); dirt.position.z = 0.001; g.add(dirt); const pl = new THREE.Mesh(new THREE.BoxGeometry(0.09, 0.035, 0.006), M('#f0c8a0')); pl.position.z = 0.004; pl.visible = false; g.add(pl); body.add(g); return { g, red: r, dirt, pl }; };
  const p = { root, body, head, arms, legs, hips, eyes, blink: 2, cheekM, smile, frown, open, glasses, animal, name: animal ? animal[0] : pick(NAMES), case: cs, step: 0, wrong: 0, t: 0, state: 'toChair', hold: 0, wounds: {}, voice: animal ? 1.45 : 1.15, patience: 1, extras: [] };
  if (cs.wound) p.wounds[cs.wound] = wound(cs.wound);
  if (cs.shot || cs.thorn) { p.wounds.kasi = wound('kasi'); p.wounds.kasi.dirt.visible = false; if (cs.shot) p.wounds.kasi.red.visible = false; }
  if (cs.thorn) { const th = new THREE.Mesh(new THREE.ConeGeometry(0.008, 0.06, 6), M('#5a3a1a')); th.position.set(0.3, 0.84, 0.06); th.rotation.z = -PI / 2.4; body.add(th); p.thorn = th; }
  if (cs.fleas) for (let i = 0; i < 8; i++) { const f = new THREE.Mesh(new THREE.SphereGeometry(0.007, 6, 4), M('#1a1a1a')); f.userData.base = new V3(rnd(-0.12, 0.12), rnd(0.85, 1.38), 0.15); body.add(f); p.extras.push(f); }
  if (cs.broken) { arms[1].fa.children.forEach(m => { if (m.material) { m.material = m.material.clone(); m.material.color.lerp(new THREE.Color('#9a6aa8'), 0.35); } }); const cast = new THREE.Mesh(new THREE.CylinderGeometry(0.055, 0.055, 0.2, 14), M('#ffffff', { roughness: 1 })); cast.position.set(-0.02, -0.03, 0.1); cast.quaternion.setFromUnitVectors(new V3(0, 1, 0), new V3(-0.04, -0.06, 0.2).normalize()); cast.visible = false; arms[1].fa.add(cast); p.cast = cast; }
  p.fever = cs.id === 'palavik'; p.coughing = cs.id === 'koha';
  setFace(p, 'sad'); root.position.copy(DOOR); return p;
}
function setFace(p, f) { p.face = f; p.smile.visible = f === 'happy'; p.frown.visible = f === 'sad' || f === 'closed'; if (f === 'closed') p.frown.scale.set(0.5, 0.3, 1); else p.frown.scale.set(1, 1, 1); p.open.visible = f === 'aaa'; p.cheekM.color.set(p.fever ? '#ff3030' : '#ff8a9a'); p.cheekM.opacity = p.fever ? 0.8 : 0.5; }
const zoneWorld = (p, z) => { const zz = ZONES[z]; return p.body.localToWorld(_v2.set(zz[0], zz[1], zz[2])); };
const SEAT = new V3(0, 0, -0.85), DOOR = new V3(-2.7, 0, -2.4);

/* ---------- paneelid ---------- */
const PANELS = [], UIT = [];
class Panel {
  constructor(w, h, W, H, draw, interactive = true) { Object.assign(this, { W, H, draw, buttons: [], hover: -1, hoverNext: -1 }); this.c = cnv(W, H); this.g = this.c.getContext('2d'); this.t = ctex(this.c); this.mesh = new THREE.Mesh(new THREE.PlaneGeometry(w, h), new THREE.MeshBasicMaterial({ map: this.t, transparent: true })); this.mesh.userData.panel = this; if (interactive) UIT.push(this.mesh); PANELS.push(this); this.refresh(); }
  refresh() { this.g.clearRect(0, 0, this.W, this.H); this.buttons = []; this.draw(this.g, this); this.t.needsUpdate = true; }
  btn(x, y, w, h, lbl, fn, o = {}) { const i = this.buttons.length; this.buttons.push({ x, y, w, h, fn, dis: o.dis }); const g = this.g, hov = this.hover === i && !o.dis, fs = o.fs || 38; rr(g, x, y, w, h, o.r ?? 22); g.fillStyle = o.dis ? '#e4e8ea' : o.on ? (o.col || '#3aa0c8') : hov ? '#d8f0f8' : (o.bg || '#ffffff'); g.fill(); if (o.line !== false) { g.lineWidth = 5; g.strokeStyle = o.dis ? '#c0c8cc' : (o.col || '#3aa0c8'); g.stroke(); } fit(g, lbl, x + w / 2, y + h / 2 + (o.sub ? -fs * 0.3 : 0), fs, o.dis ? '#9aa4a8' : o.on ? '#fff' : (o.tc || '#1a4a5a'), w - 20); if (o.sub) fit(g, o.sub, x + w / 2, y + h / 2 + fs * 0.5, fs * 0.6, o.on ? '#fff' : '#7a8a90', w - 20, 'center', false); }
  indexAt(uv) { const x = uv.x * this.W, y = (1 - uv.y) * this.H; return this.buttons.findIndex(b => x >= b.x && x <= b.x + b.w && y >= b.y && y <= b.y + b.h); }
  click(uv) { const i = this.indexAt(uv); if (i >= 0) { if (this.buttons[i].dis) { sfx.bad(); return; } sfx.click(); this.buttons[i].fn(); this.refresh(); } }
}
let bubble, card, score, shop, chart, xray, ringM; const popups = [];
function buildUI() {
  const bc = cnv(1024, 256); bubble = new THREE.Mesh(new THREE.PlaneGeometry(0.9, 0.225), new THREE.MeshBasicMaterial({ map: ctex(bc), transparent: true, depthWrite: false })); bubble.userData.c = bc; bubble.visible = false; bubble.renderOrder = 10; scene.add(bubble);
  card = new Panel(0.5, 0.7, 640, 900, drawCard); card.mesh.position.set(-1.15, 1.48, -0.66); card.mesh.lookAt(0, 1.45, 0.1); scene.add(card.mesh);
  score = new Panel(1.6, 0.5, 1024, 320, (g, Pn) => { rr(g, 6, 6, Pn.W - 12, Pn.H - 12, 40); g.fillStyle = '#ffffff'; g.fill(); g.lineWidth = 10; g.strokeStyle = '#3aa0c8'; g.stroke(); txt(g, S.vet ? 'LOOMAARST VR' : 'DOKTOR VR', Pn.W / 2, 80, 72, '#1a7aa8', 'center', true); txt(g, `Ravitud: ${S.cured}   Ootel: ${Q.length}`, Pn.W / 2, 175, 54, '#1a3a4a', 'center', true); txt(g, `★ ${S.stars}`, Pn.W / 2, 255, 60, '#e8a820', 'center', true); }, false);
  score.mesh.position.set(0.3, 2.25, -3.98); scene.add(score.mesh);
  shop = new Panel(1.0, 1.15, 820, 940, drawShop); shop.mesh.position.set(2.97, 1.45, -0.5); shop.mesh.rotation.y = -PI / 2; scene.add(shop.mesh);
  chart = new Panel(0.55, 0.8, 560, 820, drawChart); chart.mesh.position.set(1.45, 1.45, -1.9); chart.mesh.lookAt(0, 1.45, 0.1); scene.add(chart.mesh);
  xray = new Panel(0.7, 0.6, 700, 600, drawXray, false); xray.mesh.position.set(0.95, 1.85, -1.2); xray.mesh.lookAt(0, 1.6, 0.1); scene.add(xray.mesh);
  ringM = new THREE.Mesh(new THREE.RingGeometry(0.06, 0.075, 32, 1, 0, 0.01), new THREE.MeshBasicMaterial({ color: '#40d040', depthTest: false, transparent: true, side: THREE.DoubleSide })); ringM.renderOrder = 20; ringM.visible = false; scene.add(ringM);
  for (let i = 0; i < 5; i++) { const c = cnv(512, 128); const s = new THREE.Sprite(new THREE.SpriteMaterial({ map: ctex(c), transparent: true, depthTest: false })); s.userData.c = c; s.scale.set(0.5, 0.125, 1); s.visible = false; s.renderOrder = 30; scene.add(s); popups.push({ s, life: 0 }); }
}
const cprNeed = () => S.easy ? 15 : 30;
function drawCard(g, Pn) {
  rr(g, 6, 6, Pn.W - 12, Pn.H - 12, 30); g.fillStyle = '#fffdf6'; g.fill(); g.lineWidth = 10; g.strokeStyle = '#8a6a4a'; g.stroke();
  g.fillStyle = '#b8b8b8'; rr(g, Pn.W / 2 - 80, 0, 160, 40, 10); g.fill(); txt(g, 'PATSIENDI KAART', Pn.W / 2, 80, 40, '#8a6a4a', 'center', true);
  if (P && ['sit', 'done', 'bonus'].includes(P.state)) {
    fit(g, P.name, Pn.W / 2, 140, 52, '#1a3a4a', 560);
    let y = 215; const row = (done, cur, lbl, sub, num) => { g.fillStyle = done ? '#40b040' : cur ? '#3aa0c8' : '#cccccc'; g.beginPath(); g.arc(64, y, 22, 0, PI * 2); g.fill(); txt(g, done ? '✓' : num, 64, y + 2, 28, '#fff', 'center', true); fit(g, lbl, 96, y, 34, done ? '#40a040' : '#1a3a4a', 510, 'left', cur); if (sub) fit(g, sub, 96, y + 34, 24, '#7a8a90', 510, 'left', false); y += 82; };
    row(R.washed || P.state !== 'sit', !R.washed && P.state === 'sit', 'Pese käed', S.easy && !R.washed ? 'kraanikauss on vasakul taga' : null, '0');
    P.case.steps.forEach((s, i) => { const done = i < P.step, cur = i === P.step && R.washed; const show = S.easy || done; let sub = null; if (show && S.easy) { sub = (s.tool ? TOOLS[s.tool].name : s.special === 'chart' ? 'Silmatabel paremal' : 'Tühjad käed – vajuta rinda') + (s.zones ? ' → ' + ZONE_NAME[s.zones[0]] : '') + (s.tool && HOW[s.tool] ? ': ' + HOW[s.tool] : ''); if (cur && s.special === 'cpr') sub += `  (${R.cpr}/${cprNeed()})`; if (cur && s.special === 'chart') sub += `  (${R.chart}/3)`; } row(done, cur, show ? s.label : '???', sub, String(i + 1)); });
    if (P.state === 'bonus' && !P.gifted) fit(g, 'Boonus: anna kleeps või komm!', Pn.W / 2, y + 10, 32, '#e8a820', 560);
  } else txt(g, Q.length ? 'Patsient tuleb…' : 'Ootan patsienti…', Pn.W / 2, 300, 40, '#7a8a90', 'center');
  Pn.btn(30, Pn.H - 300, 280, 80, 'Lihtne', () => { S.easy = true; save(); }, { on: S.easy }); Pn.btn(330, Pn.H - 300, 280, 80, 'Raske', () => { S.easy = false; save(); }, { on: !S.easy });
  Pn.btn(30, Pn.H - 205, 280, 80, 'Loomaarst', () => { S.vet = !S.vet; save(); score.refresh(); }, { on: S.vet, fs: 34, col: '#e08a30' }); Pn.btn(330, Pn.H - 205, 280, 80, S.music ? 'Muusika sees' : 'Muusika väljas', () => { S.music = !S.music; save(); }, { fs: 30 });
  Pn.btn(30, Pn.H - 110, 580, 80, 'Järgmine patsient', () => { if (P && P.state === 'sit') leave(); }, { fs: 34 });
}
const SHOP = [['lill', 'Lillepott', 3], ['karu', 'Kaisukaru', 6], ['pallid', 'Õhupallid', 8], ['vaip', 'Vikerkaarevaip', 10], ['akvaarium', 'Akvaarium', 15], ['maal', 'Maal seinale', 12], ['disko', 'Diskokera', 20], ['kuld', 'Kuldsed tööriistad', 30]];
function drawShop(g, Pn) {
  rr(g, 6, 6, Pn.W - 12, Pn.H - 12, 30); g.fillStyle = '#fffaf0'; g.fill(); g.lineWidth = 10; g.strokeStyle = '#e8a820'; g.stroke();
  txt(g, 'AUHINNAPOOD', Pn.W / 2, 70, 56, '#c88a10', 'center', true); txt(g, `Sul on ★ ${S.wallet}`, Pn.W / 2, 130, 40, '#5a4a2a', 'center', true);
  SHOP.forEach(([id, n, price], i) => { const own = S.owned.includes(id); Pn.btn(30 + (i % 2) * 390, 170 + ((i / 2) | 0) * 185, 370, 165, n, () => buy(id, price, n), { on: own, dis: !own && S.wallet < price, sub: own ? 'olemas ✓' : `★ ${price}`, col: '#e8a820', fs: 36 }); });
}
const CHART = [['E'], ['F', 'P'], ['T', 'O', 'Z'], ['L', 'P', 'E', 'D'], ['P', 'E', 'C', 'F', 'D']];
function drawChart(g, Pn) {
  rr(g, 6, 6, Pn.W - 12, Pn.H - 12, 20); g.fillStyle = '#ffffff'; g.fill(); g.lineWidth = 8; g.strokeStyle = '#3aa0c8'; g.stroke(); txt(g, 'SILMATABEL', Pn.W / 2, 50, 34, '#3aa0c8', 'center', true);
  const sizes = [190, 130, 95, 70, 52]; let y = 100;
  CHART.forEach((row, r) => { const s = sizes[r]; const w = row.length * s * 0.95; row.forEach((L, i) => { const x = Pn.W / 2 - w / 2 + i * s * 0.95; Pn.btn(x, y, s * 0.9, s * 0.95, L, () => chartClick(r, L), { line: false, fs: s * 0.8, tc: '#111', bg: 'rgba(255,255,255,0)', r: 10 }); }); y += s + 14; });
}
const XR = { on: false, crack: false, prog: 1 };
function drawXray(g, Pn) {
  rr(g, 0, 0, Pn.W, Pn.H, 24); g.fillStyle = '#d8dde0'; g.fill(); rr(g, 24, 24, Pn.W - 48, Pn.H - 48, 12); g.fillStyle = XR.on ? '#0a1a2e' : '#2a3038'; g.fill();
  if (!XR.on) { txt(g, 'RÖNTGEN', Pn.W / 2, Pn.H / 2, 60, '#6a7480', 'center', true); return; }
  g.save(); g.beginPath(); g.rect(0, 0, Pn.W, 24 + (Pn.H - 48) * (XR.prog ?? 1)); g.clip(); g.fillStyle = '#e8f0ff'; g.shadowColor = '#9ac8ff'; g.shadowBlur = 20;
  rr(g, 120, 90, 300, 48, 24); g.fill(); rr(g, 400, 100, 170, 42, 20); g.fill(); rr(g, 400, 158, 170, 34, 17); g.fill();
  for (let i = 0; i < 4; i++) { rr(g, 580, 90 + i * 34, 60, 22, 11); g.fill(); }
  g.beginPath(); g.arc(110, 114, 40, 0, PI * 2); g.fill(); g.shadowBlur = 0;
  g.restore(); if (XR.prog < 1) { g.fillStyle = '#60c0ff'; g.fillRect(24, 24 + (Pn.H - 48) * XR.prog, Pn.W - 48, 4); return; }
  if (XR.crack) { g.strokeStyle = '#ff3030'; g.lineWidth = 8; g.beginPath(); g.moveTo(480, 92); g.lineTo(470, 110); g.lineTo(492, 124); g.lineTo(478, 146); g.stroke(); txt(g, 'Luumurd!', 485, 260, 56, '#ff6060', 'center', true); }
}
function sayP(text, p = P) { if (!p) return; const c = bubble.userData.c, g = c.getContext('2d'); g.clearRect(0, 0, 1024, 256); rr(g, 10, 10, 1004, 190, 60); g.fillStyle = '#ffffff'; g.fill(); g.lineWidth = 8; g.strokeStyle = '#3aa0c8'; g.stroke(); g.beginPath(); g.moveTo(480, 196); g.lineTo(512, 250); g.lineTo(560, 196); g.closePath(); g.fill(); fit(g, text, 512, 105, 62, '#1a3a4a', 940); bubble.material.map.needsUpdate = true; bubble.visible = true; bubble.userData.p = p; if (!speak(text, p.voice + 0.1)) sfx.babble(p.voice); }
function popup(pos, text, col = '#1a7aa8') { const p = popups.find(p => p.life <= 0) || popups[0]; const g = p.s.userData.c.getContext('2d'); g.clearRect(0, 0, 512, 128); rr(g, 6, 10, 500, 108, 50); g.fillStyle = 'rgba(255,255,255,0.95)'; g.fill(); fit(g, text, 256, 66, 58, col, 480); p.s.material.map.needsUpdate = true; p.s.position.copy(pos); p.life = 2.2; p.s.visible = true; }

/* ---------- hambad ---------- */
let jaw; const TEETH = [];
function buildJaw() {
  jaw = new THREE.Group(); jaw.position.set(-0.3, 1.12, -0.5); jaw.lookAt(0, 1.3, 0.2); jaw.visible = false; scene.add(jaw);
  const gum = M('#e88a9a', { roughness: 0.7 });
  for (const up of [1, -1]) { const gm = new THREE.Mesh(new THREE.TorusGeometry(0.11, 0.022, 10, 30, PI), gum); gm.rotation.x = PI / 2; gm.position.y = up * 0.04; jaw.add(gm);
    for (let i = 0; i < 8; i++) { const a = PI * (i + 0.5) / 8; const t = new THREE.Mesh(new THREE.BoxGeometry(0.03, 0.04, 0.03), M('#fbfaf2', { roughness: 0.3 })); t.position.set(Math.cos(a) * 0.11, up * 0.015, Math.sin(a) * 0.11); t.rotation.y = PI / 2 - a; jaw.add(t);
      const spot = new THREE.Mesh(new THREE.SphereGeometry(0.011, 8, 6), M('#7a5a2a', { roughness: 1 })); spot.position.set(0, 0, 0.015); spot.scale.z = 0.4; t.add(spot); spot.visible = false;
      const hole = new THREE.Mesh(new THREE.CircleGeometry(0.008, 10), new THREE.MeshBasicMaterial({ color: '#1a1a1a' })); hole.position.set(0, -up * 0.006, 0.0155); t.add(hole); hole.visible = false;
      TEETH.push({ t, spot, hole, dirty: false, holeOn: false, work: 0 }); } }
  const lm = label('Patsiendi hambad (suurendus)', 0.32, '#1a4a5a'); lm.position.set(0, 0.13, 0.05); jaw.add(lm);
}
function setupTeeth() { for (const t of TEETH) { t.dirty = false; t.holeOn = false; t.work = 0; t.spot.visible = false; t.hole.visible = false; t.t.material.color.set('#fbfaf2'); } const idx = [...TEETH.keys()].sort(() => Math.random() - 0.5); idx.slice(0, 5).forEach(i => { TEETH[i].dirty = true; TEETH[i].spot.visible = true; }); idx.slice(5, 7).forEach(i => { TEETH[i].holeOn = true; TEETH[i].hole.visible = true; }); }

/* ---------- kaunistused ---------- */
const DECOR = {}; let fishes = [], disco;
function buildDecor() {
  const g = (id) => { const gr = new THREE.Group(); gr.visible = S.owned.includes(id); scene.add(gr); DECOR[id] = gr; return gr; };
  let d = g('lill'); mesh(new THREE.CylinderGeometry(0.16, 0.12, 0.3, 16), M('#c8704a'), 2.5, 0.15, 1.5, d); for (let i = 0; i < 8; i++) { const c = ['#ff5a8a', '#ffd040', '#a070ff', '#ff8a40'][i % 4]; mesh(new THREE.CylinderGeometry(0.005, 0.005, 0.4, 6), M('#4a9a4a'), 2.5 + Math.cos(i) * 0.07, 0.5, 1.5 + Math.sin(i) * 0.07, d); mesh(new THREE.SphereGeometry(0.045, 10, 8), M(c), 2.5 + Math.cos(i) * 0.07, 0.72, 1.5 + Math.sin(i) * 0.07, d); }
  d = g('karu'); const kb = M('#a86a3a', { roughness: 1 }); mesh(new THREE.SphereGeometry(0.1, 14, 10), kb, 0.25, 0.7, -2.25, d); mesh(new THREE.SphereGeometry(0.075, 14, 10), kb, 0.25, 0.84, -2.25, d); for (const s of [-1, 1]) { mesh(new THREE.SphereGeometry(0.03, 10, 8), kb, 0.25 + s * 0.055, 0.91, -2.25, d); mesh(new THREE.SphereGeometry(0.035, 10, 8), kb, 0.25 + s * 0.09, 0.66, -2.2, d); } mesh(new THREE.SphereGeometry(0.012, 8, 6), M('#111'), 0.25, 0.84, -2.18, d);
  d = g('pallid'); for (let i = 0; i < 6; i++) { const x = -2.4 + (i % 3) * 0.18, z = -2.0 + (i / 3 | 0) * 0.15, y = 2.0 + Math.random() * 0.3; const b = mesh(new THREE.SphereGeometry(0.11, 16, 12), M(['#ff4a6a', '#40a0ff', '#ffd040', '#60d060', '#c060ff', '#ff8a30'][i], { roughness: 0.2 }), x, y, z, d); b.scale.y = 1.2; cylBetween(new V3(x, y - 0.13, z), new V3(-2.25, 0.8, -1.7), 0.002, M('#ffffff'), d); }
  d = g('vaip'); { const c = cnv(256, 256), gg = c.getContext('2d'); ['#ff4a4a', '#ff9a30', '#ffe040', '#50d050', '#40a0ff', '#a060ff'].forEach((col, i) => { gg.fillStyle = col; gg.beginPath(); gg.arc(128, 128, 128 - i * 20, 0, PI * 2); gg.fill(); }); const v = mesh(new THREE.CircleGeometry(0.9, 40).rotateX(-PI / 2), new THREE.MeshStandardMaterial({ map: ctex(c), roughness: 1 }), 0, 0.005, 0.7, d); v.castShadow = false; }
  d = g('akvaarium'); mesh(new THREE.BoxGeometry(0.5, 0.3, 0.25), new THREE.MeshStandardMaterial({ color: '#80c8ff', transparent: true, opacity: 0.35, roughness: 0.05 }), -1.9, 0.93, -1.2, d); mesh(new THREE.BoxGeometry(0.48, 0.04, 0.23), M('#e8d8a0'), -1.9, 0.8, -1.2, d);
  fishes = [0, 1, 2].map(i => { const f = mesh(new THREE.SphereGeometry(0.025, 10, 8), M(['#ff8a20', '#ffd020', '#ff4a8a'][i]), -1.9, 0.9, -1.2, d); f.scale.set(1.5, 1, 0.6); return f; });
  d = g('maal'); { const c = cnv(512, 384), gg = c.getContext('2d'); const gr = gg.createLinearGradient(0, 0, 0, 384); gr.addColorStop(0, '#ffb070'); gr.addColorStop(1, '#ff6a8a'); gg.fillStyle = gr; gg.fillRect(0, 0, 512, 384); gg.fillStyle = '#ffe080'; gg.beginPath(); gg.arc(256, 230, 70, 0, PI * 2); gg.fill(); gg.fillStyle = '#3a6aa8'; gg.fillRect(0, 250, 512, 134); gg.fillStyle = '#2a4a2a'; for (let i = 0; i < 6; i++) { gg.beginPath(); gg.moveTo(i * 100, 260); gg.lineTo(i * 100 + 40, 150); gg.lineTo(i * 100 + 80, 260); gg.fill(); } mesh(new THREE.BoxGeometry(0.9, 0.7, 0.03), M('#b8862a'), -0.9, 1.7, -3.98, d); mesh(new THREE.PlaneGeometry(0.8, 0.6), new THREE.MeshStandardMaterial({ map: ctex(c) }), -0.9, 1.7, -3.96, d); }
  d = g('disko'); disco = mesh(new THREE.IcosahedronGeometry(0.15, 1), M('#e0e8f0', { metalness: 1, roughness: 0.1, flatShading: true }), 0, 2.55, -1.6, d); cylBetween(new V3(0, 2.7, -1.6), new V3(0, 2.9, -1.6), 0.004, M('#888'), d);
  const dl = [new THREE.PointLight('#ff4aa0', 0, 4), new THREE.PointLight('#40c0ff', 0, 4)]; dl.forEach(l => d.add(l)); disco.userData.lights = dl;
}
function buy(id, price, name) { if (S.owned.includes(id) || S.wallet < price) return; S.wallet -= price; S.owned.push(id); save(); sfx.win(); if (DECOR[id]) DECOR[id].visible = true; if (id === 'kuld') for (const t of Object.values(TOOLS)) t.g.traverse(o => { if (o.isMesh && o.material && o.material.metalness > 0.7) o.material = goldM; }); popup(shop.mesh.position.clone().add(new V3(-0.3, 0.7, 0)), `Ostetud: ${name}!`, '#c88a10'); score.refresh(); }

/* ---------- konfetti ja mullid ---------- */
const CONF = []; let confIM;
function buildConfetti() { confIM = new THREE.InstancedMesh(new THREE.PlaneGeometry(0.03, 0.02), new THREE.MeshBasicMaterial({ side: THREE.DoubleSide }), 220); confIM.frustumCulled = false; scene.add(confIM); for (let i = 0; i < 220; i++) { CONF.push({ p: new V3(), v: new V3(), r: new V3(), life: 0, g: 4 }); confIM.setMatrixAt(i, _m.makeScale(0, 0, 0)); confIM.setColorAt(i, new THREE.Color(pick(['#ff5a6a', '#ffd040', '#40c0ff', '#60e060', '#c070ff']))); } }
function confetti(pos) { for (const c of CONF) { c.p.copy(pos); c.v.set(rnd(-1.5, 1.5), rnd(1.5, 3.5), rnd(-1.5, 1.5)); c.r.set(rnd(0, 6), rnd(0, 6), rnd(0, 6)); c.life = rnd(1.5, 2.5); c.g = 4; } }
function bubbles(pos, n = 12) { let k = 0; for (const c of CONF) { if (c.life > 0) continue; c.p.copy(pos).add(new V3(rnd(-0.06, 0.06), 0, rnd(-0.06, 0.06))); c.v.set(rnd(-0.1, 0.1), rnd(0.2, 0.5), rnd(-0.1, 0.1)); c.life = rnd(0.8, 1.4); c.g = -0.2; if (++k >= n) break; } }
function updateConfetti(dt) { CONF.forEach((c, i) => { if (c.life <= 0) return; c.life -= dt; c.v.y -= c.g * dt; c.v.multiplyScalar(0.985); c.p.addScaledVector(c.v, dt); if (c.p.y < 0.01) { c.p.y = 0.01; c.v.set(0, 0, 0); } c.r.x += dt * 5; c.r.y += dt * 3; confIM.setMatrixAt(i, c.life > 0 ? _m.compose(c.p, _q.setFromEuler(_e.set(c.r.x, c.r.y, c.r.z)), _v.setScalar(1)) : _m.makeScale(0, 0, 0)); }); confIM.instanceMatrix.needsUpdate = true; }

/* ---------- ravi ---------- */
const headPos = () => zoneWorld(P, 'suu').clone().add(new V3(0, 0.35, 0.05));
let flashT = 0;
function applyFx(fx) {
  const p = P, hp = headPos();
  switch (fx) {
    case 'temp': TOOLS.kraadiklaas.reading = 39.2; popup(hp, '39,2 °C – palavik!', '#d04040'); sayP('Mul on palavik…'); break;
    case 'tempok': TOOLS.kraadiklaas.reading = 36.6; popup(hp, '36,6 °C – korras!', '#40a040'); break;
    case 'lungs': sfx.heart(); popup(hp, 'Kopsudes kahiseb…', '#d08020'); break;
    case 'heart': sfx.heart(); popup(hp, 'Tuk-tuk – süda on korras!', '#40a040'); break;
    case 'noheart': popup(hp, 'Süda ei löö! Elusta kiiresti!', '#d04040'); sfx.beep(); break;
    case 'tummy': noise(0.6, 0.3, 200); popup(hp, 'Kõht koriseb!', '#d08020'); break;
    case 'bp': sfx.beep(); popup(hp, 'Vererõhk 120/80 – korras!', '#40a040'); break;
    case 'throat': popup(hp, 'Kurk on punane!', '#d04040'); break;
    case 'ear': popup(hp, 'Kõrv on punane!', '#d04040'); break;
    case 'drops': sfx.pop(); popup(hp, 'Tilk-tilk!', '#3aa0c8'); break;
    case 'clean': sfx.spray(); for (const w of Object.values(p.wounds)) { w.dirt.visible = false; w.dirt.material.opacity = 0.6; w.red.material.color.set('#e88080'); } popup(hp, 'Puhas!', '#3aa0c8'); break;
    case 'plaster': for (const w of Object.values(p.wounds)) { w.pl.visible = true; w.red.visible = false; } sfx.pop(); break;
    case 'plaster2': if (p.wounds.kasi) { p.wounds.kasi.pl.visible = true; p.wounds.kasi.red.visible = false; } sfx.pop(); break;
    case 'shot': sfx.pop(); p.scared = false; sayP('Auh! …Polnudki nii hull!'); break;
    case 'cast': p.cast.visible = true; p.cast.scale.y = 1; sfx.spray(); popup(hp, 'Kips on peal!', '#3aa0c8'); break;
    case 'xray': XR.on = true; XR.crack = true; XR.prog = 1; xray.refresh(); sfx.beep(); popup(hp, 'Vaata röntgenipilti!', '#3aa0c8'); break;
    case 'reflex': p.kick = 0.6; sfx.boing(); popup(hp, 'Hopsti!', '#3aa0c8'); break;
    case 'glasses': p.glasses.visible = true; sayP('Vau! Nüüd näen kõike!'); break;
    case 'jaw': jaw.visible = true; setupTeeth(); popup(hp, 'Hambad on näha – vaata vasakule!', '#3aa0c8'); break;
    case 'thorn': if (p.thorn) p.thorn.visible = false; sfx.pop(); sayP('Ai! …Oi, kui hea!'); break;
    case 'comb': p.extras.forEach(f => f.visible = false); sfx.brush(); popup(hp, 'Kirbud on läinud!', '#40a040'); break;
    case 'shock': sfx.zap(); flashT = 0.25; p.jolt = 0.3; setTimeout(() => { if (P === p) { p.lying = false; p.wake = 0.5; sayP('Mis juhtus? Aitäh, doktor, sa päästsid mu!'); setFace(p, 'happy'); } }, 900); break;
    case 'med': sfx.good(); p.fever = false; p.coughing = false; sayP(pick(['Mmm, maasikamaitse!', 'Ai kui hea!', 'Juba on parem!'])); setFace(p, 'happy'); break;
  }
}
function nextStep() { P.step++; P.hold = -99; P.prog = 0; P.hinted = false; P.spots = null; P.charged = 0; hideSpots(); sfx.good(); card.refresh(); if (P.step >= P.case.steps.length) cured(); }
const MODE = { kraadiklaas: 'leave', stetoskoop: 'spots', taskulamp: 'aim', vatitups: 'rub', plaaster: 'stick', side: 'rub', rohi: 'tilt', sustal: 'inject', haamer: 'tap', rontgen: 'rub', mansett: 'cuff', prillid: 'stick', pintsetid: 'pull', kamm: 'rub', defi: 'defi' };
const RUBNEED = { vatitups: 0.45, side: 1.2, rontgen: 0.6, kamm: 0.8 };
const HOW = { kraadiklaas: 'pane suhu, lase lahti, oota piiksu', stetoskoop: 'kuula kõiki rohelisi punkte', taskulamp: 'suuna valgus sisse', vatitups: 'hõõru haava edasi-tagasi', plaaster: 'vii haavale ja lase lahti', side: 'keri ümber käe ringiratast', rohi: 'kalluta pudelit suu juures', sustal: 'täida viaalist, siis hoia käes', haamer: 'koputa kiiresti põlvele', rontgen: 'libista mööda kätt', mansett: 'pane käele, siis pumpa pirni', prillid: 'vii näole ja lase lahti', pintsetid: 'haara okkast ja tõmba', kamm: 'kammi edasi-tagasi', defi: 'hoia rinnal, siis punane nupp', hambahari: 'hõõru pruune hambaid', puur: 'hoia augu peal' };
const HINT = { kraadiklaas: 'Pane suhu ja lase lahti!', stetoskoop: 'Kuula kõiki 4 punkti!', taskulamp: 'Suuna valgus sisse!', vatitups: 'Hõõru edasi-tagasi!', plaaster: 'Lase haava peal lahti!', side: 'Keri ümber käe!', rohi: 'Kalluta pudelit!', sustal: 'Täida süstal viaalist!', haamer: 'Koputa kiiresti!', rontgen: 'Libista mööda kätt!', mansett: 'Pane käele ja lase lahti!', prillid: 'Lase näo ees lahti!', pintsetid: 'Haara okkast ja tõmba!', kamm: 'Kammi edasi-tagasi!', defi: 'Hoia rinnal – laadib!' };
const spotMarks = [];
function hideSpots() { spotMarks.forEach(m => m.visible = false); }
function setRing(f) { ringM.geometry.dispose(); ringM.geometry = new THREE.RingGeometry(0.06, 0.075, 32, 1, PI / 2, -Math.max(0.001, f) * PI * 2); }
function showRing(z, f, col) { ringM.visible = true; ringM.position.copy(zoneWorld(P, z)); ringM.lookAt(camera.getWorldPosition(_v)); setRing(f); ringM.material.color.set(col || '#40d040'); }
function done(step) { applyFx(step.fx); nextStep(); }
function nearEar(tp) { return tp.distanceTo(zoneWorld(P, 'korv')) < tp.distanceTo(zoneWorld(P, 'korv2')) ? 'korv' : 'korv2'; }
function setSyringe(t, v) { t.g.userData.liq.scale.z = Math.max(0.01, v); t.g.userData.plunger.position.z = -0.04 * (1 - v); }
function rubFx(id, f) {
  if (id === 'vatitups') for (const w of Object.values(P.wounds)) { w.dirt.material.opacity = 0.6 * (1 - f); }
  if (id === 'side' && P.cast) { P.cast.visible = true; P.cast.scale.y = Math.max(0.05, f); }
  if (id === 'rontgen') { XR.on = true; XR.crack = true; const q = Math.floor(f * 10) / 10; if (q !== XR.prog) { XR.prog = q; xray.refresh(); } }
  if (id === 'kamm') P.extras.forEach((fl, i) => fl.visible = i >= Math.round(f * P.extras.length));
}
const rubSnd = { vatitups: () => noise(0.1, 0.12, 2500), side: () => noise(0.12, 0.2, 700), rontgen: () => tone(2200, 2200, 0.03, 'sine', 0.03), kamm: () => noise(0.1, 0.15, 3500) };
function pour(tp, dir) { let k = 0; for (const c of CONF) { if (c.life > 0) continue; c.p.copy(tp); c.v.copy(dir).multiplyScalar(0.4).add(new V3(rnd(-0.03, 0.03), 0, rnd(-0.03, 0.03))); c.life = 0.25; c.g = 6; if (++k >= 2) break; } }
function doMode(step, dt, best) {
  const t = TOOLS[step.tool]; const mode = MODE[step.tool]; if (!t.held) return false;
  const tp = tipWorld(t).clone(); const inZone = best && best.t === t && step.zones.includes(best.z);
  if (!P.hinted && HINT[t.id]) { P.hinted = true; popup(headPos(), HINT[t.id], '#3aa0c8'); }
  switch (mode) {
    case 'rub': { if (!inZone) return false; P.prog = (P.prog || 0) + Math.min(t.move, 0.05); const f = clamp(P.prog / RUBNEED[t.id], 0, 1); showRing(best.zw, f); rubFx(t.id, f); if (t.move > 0.003) { if (Math.random() < dt * 8) rubSnd[t.id](); haptic(t.held, 0.15, 15); } if (f >= 1) done(step); return true; }
    case 'aim': { const zn = step.zones[0] === 'korv' ? nearEar(tp) : step.zones[0]; const target = zoneWorld(P, zn).clone(); const dir = new V3(0, 0, -1).applyQuaternion(t.g.getWorldQuaternion(new THREE.Quaternion())); const to = target.clone().sub(tp); if (to.length() < 0.45 && (dir.angleTo(to) < 0.35 || to.length() < 0.05)) { P.prog = (P.prog || 0) + dt; const f = clamp(P.prog / 1.5, 0, 1); showRing(zn, f); if (f >= 1) done(step); } else ringM.visible = false; return true; }
    case 'tilt': { const ear = step.zones.includes('korv'); const zn = ear ? nearEar(tp) : 'suu'; if (tp.distanceTo(zoneWorld(P, zn)) > 0.13) { ringM.visible = false; if (t.held === 'desk') t.g.rotation.x = 0.25; return !!inZone; } const dir = new V3(0, 0, -1).applyQuaternion(t.g.getWorldQuaternion(new THREE.Quaternion())); if (t.held === 'desk') t.g.rotation.x = R.deskUse ? -1.1 : 0.25; const tilted = dir.y < -0.3 || (t.held === 'desk' && R.deskUse); if (tilted) { P.prog = (P.prog || 0) + dt; pour(tp, dir); if (Math.random() < dt * 2.5) tone(300, 200, 0.12, 'sine', 0.12); } const f = clamp((P.prog || 0) / 1.5, 0, 1); showRing(zn, f, tilted ? '#40d040' : '#ffb020'); if (f >= 1) { if (t.held === 'desk') t.g.rotation.x = 0.25; done(step); } return true; }
    case 'tap': { if (!inZone) { t.wasIn = false; return false; } const fast = t.speed > 0.5 || (t.held === 'desk' && R.deskTap); R.deskTap = false; if (!t.wasIn && fast) { t.wasIn = true; done(step); } else { t.wasIn = true; showRing(best.zw, 0.05, '#ffb020'); } return true; }
    case 'spots': { if (!P.spots) { const belly = P.case.id === 'koht'; const pts = belly ? [[-0.06, 0.78], [0.06, 0.78], [0, 0.72], [0, 0.84]] : [[-0.065, 0.99], [0.065, 0.99], [-0.06, 0.88], [0.06, 0.88]]; P.spots = pts.map(([x, y], i) => { let m = spotMarks[i]; if (!m) { m = new THREE.Mesh(new THREE.RingGeometry(0.012, 0.02, 20), new THREE.MeshBasicMaterial({ color: '#40e060', depthTest: false, transparent: true, side: THREE.DoubleSide })); m.renderOrder = 21; scene.add(m); spotMarks[i] = m; } m.material.color.set('#ffb020'); return { p: new V3(x, y, 0.172), t: 0, done: false, m }; }); sayP(belly ? 'Kõht koriseb…' : 'Hingan sisse… ja välja…'); }
      let all = true; for (const sp of P.spots) { const w = P.body.localToWorld(sp.p.clone()); sp.m.visible = true; sp.m.position.copy(w); sp.m.lookAt(camera.getWorldPosition(_v)); if (sp.done) continue; all = false; if (tp.distanceTo(w) < 0.045) { sp.t += dt; if (sp.t > 0.5) { sp.done = true; sp.m.material.color.set('#40e060'); tone(70, 50, 0.12, 'sine', 0.5); tone(60, 45, 0.1, 'sine', 0.4, 0.18); haptic(t.held, 0.5, 60); } } }
      if (all) done(step); else ringM.visible = false; return true; }
    case 'inject': { if (!t.filled) { const vp = VIAL.getWorldPosition(new V3()).add(new V3(0, 0.05, 0)); if (tp.distanceTo(vp) < 0.07) { t.fill = (t.fill || 0) + dt; setSyringe(t, t.fill); if (Math.random() < dt * 5) tone(500 + t.fill * 400, 600, 0.05, 'sine', 0.03); if (t.fill >= 1) { t.filled = true; sfx.pop(); popup(headPos(), 'Süstal täis! Nüüd käsivarde.', '#3aa0c8'); } } return !!inZone; }
      if (!inZone) return false; P.prog = (P.prog || 0) + dt; const f = clamp(P.prog / 1.6, 0, 1); setSyringe(t, 1 - f); showRing(best.zw, f); haptic(t.held, 0.1, 15); if (f >= 1) { t.filled = false; t.fill = 0; done(step); } return true; }
    case 'pull': { if (!P.thorn) return false; if (!P.thornHeld) { const th = P.thorn.getWorldPosition(new V3()); if (tp.distanceTo(th) < 0.045) { P.thornHeld = true; P.thornStart = tp.clone(); sfx.click(); haptic(t.held, 0.4, 40); sayP('Ai-ai…'); } else return !!inZone; }
      const d = tp.distanceTo(P.thornStart); P.thorn.position.copy(P.body.worldToLocal(tp.clone())); const f = clamp(d / 0.1, 0, 1); showRing('kasi', f); if (f >= 1) { P.thorn.visible = false; done(step); } return true; }
    case 'defi': { if (P.charged) { showRing('rind', 1, (R.time * 4 % 1) < 0.5 ? '#ff3030' : '#ffb020'); return true; } if (!inZone) { P.prog = 0; return false; } P.prog = (P.prog || 0) + dt; const f = clamp(P.prog / 2, 0, 1); showRing('rind', f, '#ffb020'); if (Math.random() < dt * 12) tone(400 + f * 1400, 420 + f * 1400, 0.04, 'sine', 0.05); if (f >= 1) { P.charged = R.time; popup(headPos(), 'Laetud! Kõik eemale – vajuta PUNAST nuppu!', '#d04040'); sfx.beep(); } return true; }
    case 'leave': case 'stick': case 'cuff': { if (!inZone) return false; showRing(best.zw, 1, '#ffb020'); return true; }
  }
  return false;
}
function pressDefi() { if (!P || P.state !== 'sit') { sfx.beep(); return; } const step = P.case.steps[P.step]; if (step && step.tool === 'defi' && P.charged) { applyFx('shock'); nextStep(); } else { sfx.bad(); popup(DEFI_BTN.getWorldPosition(new V3()).add(new V3(0, 0.2, 0)), 'Enne laadi: hoia elektroodi rinnal', '#d08020'); } }
function startCuff() { P.pumps = 0; PUMP = new THREE.Group(); const b = mesh(new THREE.SphereGeometry(0.03, 14, 10), M('#d03030', { roughness: 0.5 }), 0, 0, 0, PUMP); b.scale.y = 1.4; b.userData.btn = 'pump'; PUMP.position.set(0.36, 0.72, 0.2); P.body.add(PUMP); cylBetween(new V3(0.25, 0.83, 0.05), new V3(0.36, 0.75, 0.2), 0.003, M('#333'), P.body); popup(headPos(), 'Nüüd pumpa punast pirni!', '#3aa0c8'); }
function pump() { if (!P || !PUMP || P.pumps >= 6) return; P.pumps++; noise(0.15, 0.3, 300); PUMP.children[0].scale.set(0.8, 1.1, 0.8); setTimeout(() => PUMP && PUMP.children[0].scale.set(1, 1.4, 1), 150); popup(zoneWorld(P, 'kasi').clone().add(new V3(0, 0.25, 0)), `${P.pumps * 30} mmHg`, '#3a7ac8');
  if (P.pumps >= 6) { popup(headPos(), 'Lasen õhku välja…', '#3a7ac8'); noise(1.4, 0.15, 900); P.deflate = 1.6; } }
function updateCuff(dt) { if (!P || !(P.deflate > 0)) return; P.deflate -= dt; if (P.deflate > 0) return; if (P.state !== 'sit') return; const step = P.case.steps[P.step]; if (step && step.tool === 'mansett') { freeTool(TOOLS.mansett); removePump(); done(step); } }
function removePump() { if (PUMP) { PUMP.parent && PUMP.parent.remove(PUMP); PUMP = null; } }
function attachTool(t) { P.body.attach(t.g); t.attached = true; t.home = false; t.returning = undefined; }
function freeTool(t) { if (!t.attached) return; t.attached = false; scene.attach(t.g); t.returning = 0; t.home = false; }
function freeAll() { for (const t of Object.values(TOOLS)) freeTool(t); removePump(); if (P) P.thermo = null; }
function onRelease(t) {
  if (!P) return false; const tp = tipWorld(t).clone(); const near = z => tp.distanceTo(zoneWorld(P, z)) < ZONES[z][3] + 0.05;
  if (P.state === 'bonus' && !P.gifted && ((t.id === 'kleeps' && (near('kasi') || near('rind'))) || (t.id === 'komm' && (near('kasi') || near('suu'))))) { gift(t.id); return false; }
  const step = P.state === 'sit' && P.case.steps[P.step]; if (!step || step.tool !== t.id || !R.washed || !step.zones || !step.zones.some(near)) return false;
  const mode = MODE[t.id];
  if (mode === 'stick') { done(step); return false; }
  if (mode === 'leave') { attachTool(t); P.thermo = { t0: R.time, done: false, target: P.case.id === 'palavik' ? 39.2 : 36.6 }; setFace(P, 'closed'); sayP('Mmm-mmm…'); return true; }
  if (mode === 'cuff') { attachTool(t); startCuff(); return true; }
  return false;
}
function updateThermo() {
  const t = TOOLS.kraadiklaas, d = t.g.userData.disp; let val = null;
  if (P && P.thermo) { const e = R.time - P.thermo.t0; val = Math.min(P.thermo.target, 35 + e / 4 * (P.thermo.target - 35)); if (e >= 4 && !P.thermo.done) { P.thermo.done = true; for (let i = 0; i < 3; i++) tone(2400, 2400, 0.08, 'square', 0.05, i * 0.15); popup(headPos(), 'Piiks! Võta kraadiklaas välja.', '#3aa0c8'); } }
  else if (t.reading) val = t.reading;
  const txtv = val ? val.toFixed(1) + '°' : '--.-'; if (d.last === txtv) return; d.last = txtv; const g = d.c.getContext('2d'); g.fillStyle = '#c8e0c0'; g.fillRect(0, 0, 128, 64); txt(g, txtv, 64, 34, 46, '#102010', 'center', true); d.m.material.map.needsUpdate = true;
}
function treat(dt) {
  for (const t of Object.values(TOOLS)) { if (!t.held) { t.lastTip = null; t.move = 0; t.speed = 0; continue; } const tp = tipWorld(t).clone(); t.move = t.lastTip ? tp.distanceTo(t.lastTip) : 0; t.speed = t.move / Math.max(dt, 1e-3); t.lastTip = tp; }
  updateThermo();
  if (!P || !['sit', 'bonus'].includes(P.state)) { ringM.visible = false; hideSpots(); return; }
  const step = P.state === 'sit' ? P.case.steps[P.step] : null;
  let best = null, bd = 9;
  for (const t of Object.values(TOOLS)) { if (!t.held) continue; const tp = tipWorld(t).clone(); for (const z in ZONES) { const d = tp.distanceTo(zoneWorld(P, z)); if (d < ZONES[z][3] + 0.02 && d < bd) { bd = d; best = { t, z: z.replace('2', ''), zw: z }; } } }
  const lampNear = TOOLS.taskulamp.held && !P.lying && tipWorld(TOOLS.taskulamp).distanceTo(zoneWorld(P, 'suu')) < 0.25;
  if (!P.thermo) { if (lampNear && P.face !== 'aaa') { P.prevFace = P.face; setFace(P, 'aaa'); sfx.aaa(); } else if (!lampNear && P.face === 'aaa') setFace(P, P.prevFace || 'sad'); }
  if (P.case.shot && P.step < 2 && TOOLS.sustal.held && tipWorld(TOOLS.sustal).distanceTo(zoneWorld(P, 'rind')) < 0.45) { if (!P.scared) { P.scared = true; sayP('Oi-oi, ma kardan! Ma ei vaata!'); } } else if (P.scared && !TOOLS.sustal.held) P.scared = false;
  if (P.state === 'bonus') { if (best && ['kleeps', 'komm'].includes(best.t.id)) showRing(best.zw, 1, '#ffb020'); else ringM.visible = false; return; }
  if (step && step.tool && !step.special && R.washed && doMode(step, dt, best)) return;
  if (!best) { P.hold = 0; ringM.visible = false; return; }
  const ok = step && step.tool === best.t.id && step.zones && step.zones.includes(best.z);
  if (ok && !R.washed) { showRing(best.zw, 1, '#ffb020'); if (R.time - R.washMsg > 4) { R.washMsg = R.time; sayP('Doktor, pese enne käed!'); } return; }
  if (ok) { ringM.visible = false; return; }
  if (P.holdKey !== best.t.id + best.z) { P.holdKey = best.t.id + best.z; P.hold = 0; P.wrongShown = false; }
  P.hold += dt; showRing(best.zw, clamp(P.hold / 0.9, 0, 1), '#ff6060');
  if (P.hold >= 0.9 && !P.wrongShown) { P.wrongShown = true; P.wrong++; sfx.bad(); sayP(pick(['Hmm, see ei aita…', 'Kõdi teeb!', 'Kas see on ikka õige?'])); if (S.easy && step) setTimeout(() => P && P.state === 'sit' && sayP(`Proovi: ${step.tool ? TOOLS[step.tool].name : step.label}`), 1400); }
}
function updateSpecial(dt) {
  if (!P || P.state !== 'sit' || !R.washed) return; const step = P.case.steps[P.step]; if (!step || !step.special) return;
  if (step.special === 'brush' || step.special === 'drill') {
    const tool = TOOLS[step.tool]; if (!tool.held) return; const tp = tipWorld(tool).clone(); const brush = step.special === 'brush';
    for (const t of TEETH) { if (!(brush ? t.dirty : t.holeOn)) continue; if (t.t.getWorldPosition(_v2).distanceTo(tp) < 0.04) { t.work += brush ? Math.min(tool.move || 0, 0.03) * 5 : dt; if (Math.random() < dt * 6) brush ? sfx.brush() : sfx.drill(); haptic(tool.held, 0.2, 20); if (brush && Math.random() < dt * 15) bubbles(tp, 1);
      if (t.work > 0.6) { t.work = 0; if (brush) { t.dirty = false; t.spot.visible = false; } else { t.holeOn = false; t.hole.visible = false; t.t.material.color.set('#e0ecff'); } sfx.pop(); } } }
    if (!TEETH.some(t => brush ? t.dirty : t.holeOn)) { popup(headPos(), brush ? 'Hambad säravad!' : 'Augud parandatud!', '#40a040'); if (!brush) { sayP('Hammas ei valuta enam!'); setFace(P, 'happy'); } nextStep(); }
  }
  if (step.special === 'cpr') {
    const chest = zoneWorld(P, 'rind').clone(); P.beat = (P.beat || 0) - dt; if (P.beat <= 0) { P.beat = 60 / 110; sfx.tick(); }
    for (const c of ctrls) { if (!c.userData.src || c.userData.tool) continue; const cp = new V3().setFromMatrixPosition(c.matrixWorld); const near = Math.hypot(cp.x - chest.x, cp.z - chest.z) < 0.22 && cp.y < chest.y + 0.35 && cp.y > chest.y - 0.15;
      const u = c.userData; if (!near) { u.maxY = cp.y; u.armed = true; continue; } u.maxY = Math.max(u.maxY ?? cp.y, cp.y);
      if (u.armed && cp.y < u.maxY - 0.04) { u.armed = false; u.minY = cp.y; cprPush(u.hand); } else if (!u.armed) { u.minY = Math.min(u.minY, cp.y); if (cp.y > u.minY + 0.03) { u.armed = true; u.maxY = cp.y; } } }
  }
}
function cprPush(hand) { if (!P) return; R.cpr++; P.press = 0.15; haptic(hand, 0.5, 40); tone(180, 120, 0.06, 'sine', 0.15); if (R.cpr % 5 === 0) card.refresh(); if (R.cpr >= cprNeed()) { R.cpr = 0; popup(headPos(), 'Hästi! Nüüd defibrillaator!', '#40a040'); nextStep(); } }
function chartClick(row, L) {
  if (!P || P.state !== 'sit') return;
  const step = P.case.steps[P.step];
  if (step && step.special === 'chart') { if (!R.washed) { sayP('Doktor, pese enne käed!'); return; } R.chart++; sayP(row >= 3 ? 'Hmm… ma ei näe seda…' : `See on ${L}!`); if (R.chart >= 3) { R.chart = 0; popup(headPos(), 'Patsient vajab prille!', '#d08020'); nextStep(); } else card.refresh(); }
  else sayP(P.glasses.visible || row < 3 ? `See on ${L}!` : 'Ei näe…');
}
function cured() {
  const stars = S.easy ? 3 : Math.max(1, 3 - P.wrong); S.cured++; S.stars += stars; S.wallet += stars; save(); score.refresh(); shop.refresh();
  P.fever = false; setFace(P, 'happy'); sayP(`Aitäh, doktor! ${'★'.repeat(stars)}`); sfx.win(); sfx.coin(); confetti(zoneWorld(P, 'rind').clone().add(new V3(0, 0.6, 0)));
  freeAll(); P.state = 'done'; P.t = 0; P.hug = 1.2; R.washed = false; jaw.visible = false; XR.on = false; XR.crack = false; xray.refresh();
}
function gift(id) { if (P.gifted) return; P.gifted = true; S.stars++; S.wallet++; save(); score.refresh(); shop.refresh(); sfx.coin(); sfx.giggle(); sayP(id === 'kleeps' ? 'Jee, kleeps! Aitäh!' : 'Mmm, komm! Aitäh!'); confetti(headPos()); P.hug = 1.2; if (id === 'kleeps') { const s = TOOLS.kleeps.g.children[0].clone(); s.position.set(0.07, 0.98, 0.17); s.rotation.set(0, 0, 0); s.scale.setScalar(1.3); P.body.add(s); } popup(headPos(), '+1 ★ boonus', '#e8a820'); P.t = 0; card.refresh(); }
function leave() { if (!P) return; freeAll(); hideSpots(); P.state = 'leave'; P.t = 0; P.lying = false; bubble.visible = false; jaw.visible = false; XR.on = false; xray.refresh(); card.refresh(); }
function poke(z, hand) {
  if (!P || !['sit', 'done', 'bonus'].includes(P.state) || P.lying) return; if (R.time - (P.pokeT || -9) < 1.2) return; P.pokeT = R.time;
  if ((z === 'otsmik' || z === 'korv') && P.animal) { sfx.purr(); popup(headPos(), '♥ Nurr-nurr', '#e04a8a'); setFace(P, 'happy'); haptic(hand, 0.3, 200); }
  else if (z === 'koht' || z === 'rind') { sfx.giggle(); P.tickle = 0.6; sayP(pick(['Hihii, kõdi!', 'Hahaa, lõpeta!', 'Kõdi teeb!'])); haptic(hand, 0.3, 120); }
  else if (z === 'otsmik') { sfx.giggle(); popup(headPos(), '♥', '#e04a8a'); }
}
function updateHands() { if (!P) return; const step = P.state === 'sit' && P.case.steps[P.step]; if (step && step.special === 'cpr') return; for (const c of ctrls) { if (!c.userData.src || c.userData.tool) continue; const cp = new V3().setFromMatrixPosition(c.matrixWorld); for (const z of ['otsmik', 'korv', 'korv2', 'koht']) if (cp.distanceTo(zoneWorld(P, z)) < ZONES[z][3] + 0.03) poke(z.replace('2', ''), c.userData.hand); } }

/* ---------- patsientide liikumine ---------- */
function walkTo(p, target, dt, speed = 0.9) { const d = _v.copy(target).sub(p.root.position); d.y = 0; const L = d.length(); if (L < 0.03) return true; const s = Math.min(L, speed * dt); p.root.position.addScaledVector(d.normalize(), s); p.root.rotation.y = Math.atan2(d.x, d.z); p.walkT = (p.walkT || 0) + dt; walkPose(p, p.walkT); if (Math.floor(p.walkT * 4.5) !== Math.floor((p.walkT - dt) * 4.5)) sfx.step(); return false; }
function standPose(p) { p.hips.forEach(h => h.rotation.x = PI / 2); p.legs.forEach(l => l.rotation.x = -PI / 2); p.body.position.set(0, 0.3, 0); p.body.rotation.set(0, 0, 0); p.body.scale.set(1, 1, 1); }
function sitPose(p) { p.body.position.set(0, 0, 0); p.body.rotation.set(0, 0, 0); p.body.scale.set(1, 1, 1); p.legs.forEach(l => l.rotation.x = 0); p.hips.forEach(h => h.rotation.x = 0); p.arms.forEach(a => a.a.rotation.set(0, 0, 0)); }
function lyingPose(p) { p.body.rotation.set(-PI / 2, 0, 0); p.body.position.set(0, 0.74, 0.54); p.hips.forEach(h => h.rotation.x = PI / 2); p.legs.forEach(l => l.rotation.x = 0); p.arms.forEach(a => a.a.rotation.set(0, 0, 0)); }
function walkPose(p, t) { standPose(p); p.body.position.y = 0.3 + Math.abs(Math.sin(t * 9)) * 0.03; p.hips.forEach((h, i) => h.rotation.x = PI / 2 + Math.sin(t * 9 + i * PI) * 0.35); p.arms.forEach((a, i) => a.a.rotation.x = Math.sin(t * 9 + i * PI) * 0.4); }
function removePatient(p) { scene.remove(p.root); p.root.traverse(o => o.geometry && o.geometry.dispose()); }
function updatePatients(dt) {
  arriveT -= dt; const maxQ = S.easy ? 1 : 3;
  if (arriveT <= 0 && Q.length < maxQ) { arriveT = S.easy ? 2 : rnd(18, 28); const p = makePatient(); p.chair = [0, 1, 2].find(i => !Q.some(q => q.chair === i)); Q.push(p); sfx.door(); if (p.case.ambulance) sfx.siren(); score.refresh(); card.refresh(); }
  for (const p of [...Q]) {
    p.t += dt;
    if (p.state === 'toChair') { if (walkTo(p, ROOM.chairs[p.chair], dt)) { p.state = 'wait'; p.root.rotation.y = 0; sitPose(p); p.body.position.y = -0.12; } }
    else if (p.state === 'wait') { p.head.rotation.y = Math.sin(R.time * 0.8 + p.chair) * 0.3;
      if (!S.easy) { p.patience -= dt / 75; setFace(p, p.patience > 0.4 ? 'happy' : 'sad'); if (p.patience <= 0) { Q.splice(Q.indexOf(p), 1); p.state = 'leave'; LEAVING.push(p); sayP('Ma ei jõua enam oodata!', p); score.refresh(); continue; } }
      if (!P && Q[0] === p) { Q.shift(); P = p; p.state = 'toBed'; p.walkT = 0; score.refresh(); card.refresh(); } }
  }
  for (const p of [...LEAVING]) if (walkTo(p, DOOR, dt, 1.1)) { removePatient(p); LEAVING.splice(LEAVING.indexOf(p), 1); }
  if (!P) return; const p = P; p.t += dt;
  if (p.state === 'toBed') { if (!p.passedMid) { if (walkTo(p, new V3(-0.8, 0, -1.35), dt)) p.passedMid = true; } else if (walkTo(p, SEAT, dt)) { p.state = 'sit'; p.t = 0; p.root.position.copy(SEAT); p.root.rotation.y = 0; sitPose(p); R.washed = false; R.cpr = 0; R.chart = 0; sayP(p.case.say); card.refresh(); if (p.case.ambulance) setTimeout(() => { if (P === p && p.step === 0) { p.lying = true; setFace(p, 'sad'); popup(headPos(), 'Patsient minestas!', '#d04040'); sayP('…'); } }, 1600); } }
  else if (['sit', 'done', 'bonus'].includes(p.state)) {
    if (p.lying) { lyingPose(p); if (p.press > 0) { p.press -= dt; p.body.scale.set(1, 1, 1 - p.press * 0.5); } else p.body.scale.set(1, 1, 1); if (p.jolt > 0) { p.jolt -= dt; p.body.position.y += Math.sin(p.jolt * 60) * 0.02; } }
    else {
      if (p.wake > 0) { p.wake -= dt; sitPose(p); }
      { const cl = p.body.worldToLocal(camera.getWorldPosition(new V3())).sub(new V3(0, 1.25, 0)); const yaw = clamp(Math.atan2(cl.x, cl.z), -0.7, 0.7), pitch = clamp(-Math.atan2(cl.y, Math.hypot(cl.x, cl.z)), -0.45, 0.45); p.head.rotation.y += (yaw - p.head.rotation.y) * Math.min(1, dt * 3); p.head.rotation.x += (pitch - p.head.rotation.x) * Math.min(1, dt * 3); }
      p.blink -= dt; const bl = p.blink < 0.12 ? 0.1 : 1; p.eyes.forEach(e => e.scale.y = bl); if (p.blink < 0) p.blink = rnd(2, 5);
      if (p.coughing && p.state === 'sit' && !TOOLS.stetoskoop.held) { p.cough = (p.cough ?? 2) - dt; if (p.cough < 0) { p.cough = rnd(2.5, 4); sfx.cough(); p.coughT = 0.4; } }
      let rx = 0; if (p.coughT > 0) { p.coughT -= dt; rx = Math.sin(p.coughT * 25) * 0.08; }
      if (p.tickle > 0) { p.tickle -= dt; p.body.rotation.z = Math.sin(p.tickle * 40) * 0.05; } else p.body.rotation.z = 0;
      if (p.kick > 0) { p.kick -= dt; p.legs[1].rotation.x = -Math.sin((0.6 - p.kick) / 0.6 * PI); } else p.legs[1].rotation.x = 0;
      const A = p.arms;
      if (p.scared) A.forEach((a, i) => { a.a.rotation.x = -2.0; a.a.rotation.z = (i ? 1 : -1) * 0.45; });
      else if (p.hug > 0) { p.hug -= dt; A.forEach((a, i) => { a.a.rotation.x = -1.4; a.a.rotation.z = (i ? -1 : 1) * 0.35; }); rx = -0.1; }
      else if (p.state === 'bonus' && !p.gifted) { A[1].a.rotation.x = -1.2; A[0].a.rotation.x *= 0.85; }
      else A.forEach(a => { a.a.rotation.x *= 0.85; a.a.rotation.z *= 0.85; });
      p.body.rotation.x = rx;
      p.body.position.y = p.fever ? Math.sin(R.time * 30) * 0.002 : 0;
      if (p.state === 'done') { p.body.position.y = Math.abs(Math.sin(p.t * 6)) * 0.05; if (p.t > 2.5) { p.state = 'bonus'; p.t = 0; sayP(p.animal ? 'Kas ma saan maiust?' : 'Kas ma saan kleepsu või kommi?'); card.refresh(); } }
      if (p.state === 'bonus' && p.t > (p.gifted ? 3 : 8)) leave();
    }
    for (const f of p.extras) if (f.visible) f.position.copy(f.userData.base).add(_v.set(Math.sin(R.time * 9 + f.id) * 0.02, Math.abs(Math.sin(R.time * 7 + f.id * 2)) * 0.03, 0));
  }
  else if (p.state === 'leave') { if (walkTo(p, DOOR, dt, 1.0)) { removePatient(p); P = null; card.refresh(); } }
  const bp = bubble.userData.p; if (bubble.visible && bp && bp.root.parent) { bubble.position.copy(zoneWorld(bp, 'suu')).add(new V3(0, bp.lying ? 0.25 : 0.42, 0)); bubble.lookAt(camera.getWorldPosition(_v)); } else bubble.visible = false;
}

/* ---------- kätepesu ---------- */
function updateWash(dt) {
  const fau = ROOM.sink.localToWorld(new V3(0, 0.95, -0.03)); let washing = false;
  for (const c of ctrls) { if (!c.userData.src) continue; if (new V3().setFromMatrixPosition(c.matrixWorld).distanceTo(fau) < 0.2) washing = true; }
  if (R.deskWash > 0) { R.deskWash -= dt; washing = true; }
  ROOM.water.visible = washing;
  if (washing && !R.washed) { R.washT += dt; if (Math.random() < dt * 3) sfx.water(); if (Math.random() < dt * 20) bubbles(fau.clone().add(new V3(0, -0.05, 0)), 1); for (const c of ctrls) if (c.userData.src) haptic(c.userData.hand, 0.08, 20);
    if (R.washT > 1.6) { R.washed = true; R.washT = 0; sfx.good(); popup(fau.clone().add(new V3(0, 0.35, 0)), 'Käed puhtad! ✓', '#40a040'); card.refresh(); } }
  else if (!washing) R.washT = Math.max(0, R.washT - dt * 0.5);
}

/* ---------- sisend ---------- */
const rig = new THREE.Group(); rig.position.set(0, 0, 0.45); scene.add(rig); rig.add(camera);
const ray = new THREE.Raycaster(); const ctrls = [];
function ctrlRay(c, o, d) { _m.identity().extractRotation(c.matrixWorld); o.setFromMatrixPosition(c.matrixWorld); d.set(0, 0, -1).applyMatrix4(_m).normalize(); }
function grab(t, holder, hand) { if (t.held) return;
  if (t.attached) { t.attached = false; if (t.id === 'kraadiklaas' && P && P.thermo) { const st0 = P.case.steps[P.step]; if (P.thermo.done && st0 && st0.tool === 'kraadiklaas') { P.thermo = null; setFace(P, 'sad'); done(st0); } else { P.thermo = null; setFace(P, 'sad'); popup(headPos(), 'Liiga vara! Oota piiksu.', '#d08020'); } } if (t.id === 'mansett') { removePump(); if (P) { P.pumps = 0; P.deflate = 0; } } }
  if (t.id === 'kraadiklaas' && !(P && P.thermo)) t.reading = null;
  t.held = hand; t.home = false; holder.add(t.g); t.g.position.set(0, 0, -0.02); t.g.quaternion.identity(); if (hand === 'desk') { t.g.position.set(0.18, -0.2, -0.4); t.g.rotation.set(0.25, 0.25, 0); } sfx.grab(); haptic(hand, 0.4, 40); if (t.g.userData.beam) t.g.userData.beam.visible = true; }
function release(t) { if (!t || !t.held) return; const stay = onRelease(t); t.held = null; if (t.g.userData.beam) t.g.userData.beam.visible = false; if (stay) { sfx.drop(); return; } scene.attach(t.g); t.returning = 0; sfx.drop(); if (t.g.userData.beam) t.g.userData.beam.visible = false; }
function nearestTool(pos, maxD) { let best = null, bd = maxD; for (const t of Object.values(TOOLS)) { if (t.held) continue; const d = t.g.getWorldPosition(_v).distanceTo(pos); if (d < bd) { bd = d; best = t; } } return best; }
function tryGrab(c) {
  const o = new V3(), d = new V3(); ctrlRay(c, o, d); ray.set(o, d); ray.far = 5; const ui = ray.intersectObjects(UIT, false)[0]; if (ui) { ui.object.userData.panel.click(ui.uv); return; }
  if (c.userData.tool) return; let t = nearestTool(o, 0.13);
  if (!t) { const hits = ray.intersectObjects(Object.values(TOOLS).filter(t => !t.held).map(t => t.hit), false); if (hits[0] && hits[0].distance < 3) t = TOOLS[hits[0].object.userData.tool]; }
  if (t) { grab(t, c, c.userData.hand); c.userData.tool = t; }
}
function buildControllers() {
  const f = new XRControllerModelFactory();
  for (let i = 0; i < 2; i++) {
    const c = renderer.xr.getController(i); c.userData = {}; const gr = renderer.xr.getControllerGrip(i);
    c.addEventListener('connected', e => { c.userData.hand = e.data.handedness; c.userData.src = e.data; if (!gr.userData.glove) { gr.userData.glove = makeGlove(e.data.handedness === 'left' ? -1 : 1); gr.add(gr.userData.glove); c.userData.glove = gr.userData.glove; } });
    c.addEventListener('disconnected', () => { c.userData.src = null; });
    const down = () => { initAudio(); if (touchButton(c)) return; tryGrab(c); }, up = () => { if (c.userData.tool) { release(c.userData.tool); c.userData.tool = null; } };
    c.addEventListener('selectstart', down); c.addEventListener('selectend', up); c.addEventListener('squeezestart', down); c.addEventListener('squeezeend', up);
    const line = new THREE.Line(new THREE.BufferGeometry().setFromPoints([new V3(), new V3(0, 0, -1)]), new THREE.LineBasicMaterial({ color: '#3aa0c8', transparent: true, opacity: 0.6 })); c.add(line); c.userData.line = line;
    rig.add(c); rig.add(gr); ctrls.push(c);
  }
}
function makeGlove(side) {
  const g = new THREE.Group(); const gm = M('#6aaee8', { roughness: 0.45 }); const palm = mesh(new THREE.BoxGeometry(0.075, 0.028, 0.09), gm, 0, -0.01, 0.03, g); palm.scale.set(1, 1, 1);
  const fingers = []; for (let i = 0; i < 4; i++) { const f = new THREE.Group(); f.position.set(-0.027 + i * 0.018, -0.01, -0.015); g.add(f); mesh(new THREE.CapsuleGeometry(0.008, 0.045 - Math.abs(i - 1.5) * 0.006, 4, 8).rotateX(PI / 2).translate(0, 0, -0.03), gm, 0, 0, 0, f); fingers.push(f); }
  const th = new THREE.Group(); th.position.set(side * 0.04, -0.012, 0.03); g.add(th); mesh(new THREE.CapsuleGeometry(0.009, 0.035, 4, 8).rotateX(PI / 2).translate(0, 0, -0.025), gm, 0, 0, 0, th); th.rotation.y = side * 0.6; fingers.push(th);
  const cuff = mesh(new THREE.CylinderGeometry(0.04, 0.042, 0.03, 14).rotateX(PI / 2), gm, 0, -0.01, 0.085, g);
  g.rotation.x = -0.5; g.userData.fingers = fingers; return g;
}
function updateGloves(dt) { for (const c of ctrls) { const gl = c.userData.glove; if (!gl) continue; const curl = c.userData.tool ? 1.1 : 0.15; gl.userData.fingers.forEach((f, i) => { if (i < 4) f.rotation.x += (-curl - f.rotation.x) * Math.min(1, dt * 12); }); } }
function touchButton(c) { const cp = new V3().setFromMatrixPosition(c.matrixWorld); const o = new V3(), d = new V3(); ctrlRay(c, o, d); ray.set(o, d); ray.far = 3; const objs = [DEFI_BTN]; if (PUMP) objs.push(PUMP.children[0]); for (const b of objs) { if (b.getWorldPosition(new V3()).distanceTo(cp) < 0.09) { pressBtn(b); return true; } } const h = ray.intersectObjects(objs, false)[0]; if (h && !c.userData.tool) { pressBtn(h.object); return true; } return false; }
function pressBtn(b) { if (b.userData.btn === 'defi') { b.position.z -= 0.008; setTimeout(() => b.position.z += 0.008, 150); pressDefi(); } else if (b.userData.btn === 'pump') pump(); }
let btnNear = new Map();
function updateButtonsTouch() { for (const c of ctrls) { if (!c.userData.src) continue; const cp = new V3().setFromMatrixPosition(c.matrixWorld); const objs = [DEFI_BTN]; if (PUMP) objs.push(PUMP.children[0]); for (const b of objs) { const k = c.id + '_' + b.id; const n = b.getWorldPosition(new V3()).distanceTo(cp) < 0.05; if (n && !btnNear.get(k)) pressBtn(b); btnNear.set(k, n); } } }
let deskTool = null; const deskHand = new THREE.Group(); camera.add(deskHand);
const cvs = renderer.domElement; cvs.addEventListener('contextmenu', e => e.preventDefault());
function deskRay() { const o = new V3(), d = new V3(); camera.getWorldPosition(o); camera.getWorldDirection(d); return [o, d]; }
cvs.addEventListener('mousedown', e => {
  initAudio(); if (document.pointerLockElement !== cvs) { cvs.requestPointerLock?.(); return; }
  const [o, d] = deskRay(); ray.set(o, d); ray.far = 6;
  if (e.button === 2) { if (deskTool) { release(deskTool); deskTool = null; } return; }
  { const objs = [DEFI_BTN]; if (PUMP) objs.push(PUMP.children[0]); const hb = ray.intersectObjects(objs, false)[0]; if (hb) { pressBtn(hb.object); return; } }
  const ui = ray.intersectObjects(UIT, false)[0]; if (ui) { ui.object.userData.panel.click(ui.uv); return; }
  const hits = ray.intersectObjects(Object.values(TOOLS).filter(t => !t.held).map(t => t.hit), false);
  if (hits[0] && (!deskTool || hits[0].distance < 1.2 || TOOLS[hits[0].object.userData.tool].attached)) { if (deskTool) release(deskTool); deskTool = TOOLS[hits[0].object.userData.tool]; grab(deskTool, deskHand, 'desk'); return; }
  if (ray.intersectObject(ROOM.sinkHit, false)[0]) { R.deskWash = 1.8; return; }
  if (deskTool) { R.deskUse = true; R.deskTap = true; return; }
  if (!deskTool && P) { const h = ray.intersectObject(P.body, true)[0]; if (h) { let bz = null, bd = 9; for (const z in ZONES) { const dd = h.point.distanceTo(zoneWorld(P, z)); if (dd < bd) { bd = dd; bz = z.replace('2', ''); } } if (bd < 0.2) poke(bz, 'desk'); } }
});
addEventListener('mouseup', e => { if (e.button === 0) R.deskUse = false; });
addEventListener('mousemove', e => { if (document.pointerLockElement !== cvs || renderer.xr.isPresenting) return; rig.rotation.y -= e.movementX * 0.0022; camera.rotation.x = clamp(camera.rotation.x - e.movementY * 0.0022, -1.3, 1.2); });
const keys = {};
addEventListener('keydown', e => { initAudio(); const n = parseInt(e.key); if (n >= 1 && n <= 9) { const t = TOOLS[TOOL_LIST[n - 1][0]]; if (deskTool) release(deskTool); deskTool = t; grab(t, deskHand, 'desk'); } if (e.key === 'Escape' && deskTool) { release(deskTool); deskTool = null; } if (e.code === 'Space') { e.preventDefault(); if (!keys.Space) { const s = P && P.state === 'sit' && P.case.steps[P.step]; if (s && s.special === 'cpr' && R.washed) cprPush('desk'); } } keys[e.code] = true; });
addEventListener('keyup', e => keys[e.code] = false);
function deskAim() {
  if (!deskTool || renderer.xr.isPresenting) return;
  const [o, d] = deskRay(); ray.set(o, d); ray.far = 2.5; const targets = [VIAL]; if (P) targets.push(P.body); if (jaw.visible) targets.push(jaw);
  const hits = ray.intersectObjects(targets, true);
  if (hits[0]) { const tgt = hits[0].point.clone().addScaledVector(d, -0.01); deskTool.g.position.copy(deskHand.worldToLocal(tgt.clone())).sub(deskTool.tip.clone().applyQuaternion(deskTool.g.quaternion)); }
  else deskTool.g.position.set(0.18, -0.2, -0.4);
}
function blocked(x, z) { if (x < -2.75 || x > 2.75 || z < -3.75 || z > 1.75) return true; for (const [x0, x1, z0, z1] of OBST) if (x > x0 - 0.15 && x < x1 + 0.15 && z > z0 - 0.15 && z < z1 + 0.15) return true; return false; }
function updateMove(dt) {
  let lx = 0, ly = 0; const s = renderer.xr.getSession();
  if (s) for (const src of s.inputSources) { if (src.handedness === 'left' && src.gamepad) { const a = src.gamepad.axes; lx = a.length >= 4 ? a[2] : a[0]; ly = a.length >= 4 ? a[3] : a[1]; } }
  else { lx = (keys.KeyD ? 1 : 0) - (keys.KeyA ? 1 : 0); ly = (keys.KeyS ? 1 : 0) - (keys.KeyW ? 1 : 0); }
  if (Math.abs(lx) < 0.15) lx = 0; if (Math.abs(ly) < 0.15) ly = 0; if (!lx && !ly) return;
  camera.getWorldDirection(_v); _v.y = 0; _v.normalize(); const spd = 1.4 * dt;
  const nx = rig.position.x + (_v.x * -ly - _v.z * lx) * spd, nz = rig.position.z + (_v.z * -ly + _v.x * lx) * spd;
  if (!blocked(nx, nz)) { rig.position.x = nx; rig.position.z = nz; } else if (!blocked(nx, rig.position.z)) rig.position.x = nx; else if (!blocked(rig.position.x, nz)) rig.position.z = nz;
}
function updateTools(dt) {
  for (const t of Object.values(TOOLS)) { if (t.held || t.home || t.attached) continue; t.returning += dt * 3; t.g.position.lerp(t.slot, clamp(t.returning, 0, 1)); t.g.quaternion.slerp(t.slotQ, clamp(t.returning, 0, 1)); if (t.returning >= 1) { t.home = true; t.g.position.copy(t.slot); t.g.quaternion.copy(t.slotQ); } }
  highlight.visible = false;
  if (S.easy && P && ['sit', 'bonus'].includes(P.state)) {
    let pos = null, sc = 0.15;
    if (P.state === 'bonus') { if (!P.gifted) { const t = TOOLS.kleeps; pos = t.held ? zoneWorld(P, 'kasi').clone() : t.g.getWorldPosition(new V3()); } }
    else if (!R.washed) { pos = ROOM.sink.localToWorld(new V3(0, 1.0, -0.03)); sc = 0.3; }
    else { const step = P.case.steps[P.step]; if (step) { if (step.tool) { const t = TOOLS[step.tool]; if (!t.held) pos = t.g.getWorldPosition(new V3()); else if (step.zones) { pos = zoneWorld(P, step.zones[0]).clone(); sc = 0.2; } else { const tt = TEETH.find(x => step.special === 'brush' ? x.dirty : x.holeOn); if (tt) { pos = tt.t.getWorldPosition(new V3()); sc = 0.08; } } } else if (step.special === 'chart') { pos = chart.mesh.position.clone(); sc = 0.5; } else if (step.special === 'cpr') { pos = zoneWorld(P, 'rind').clone(); sc = 0.25; } } }
    if (pos) { highlight.visible = true; highlight.position.copy(pos); highlight.scale.setScalar(sc + Math.sin(R.time * 5) * sc * 0.2); }
  }
  for (const p of popups) { if (p.life <= 0) continue; p.life -= dt; p.s.position.y += dt * 0.08; p.s.material.opacity = clamp(p.life, 0, 1); if (p.life <= 0) p.s.visible = false; }
  if (flashT > 0) { flashT -= dt; hemi.intensity = 1.6 + flashT * 30; } else hemi.intensity = 1.6;
  fishes.forEach((f, i) => { const a = R.time * (0.6 + i * 0.2) + i * 2; f.position.set(-1.9 + Math.cos(a) * 0.17, 0.88 + i * 0.04 + Math.sin(a * 2) * 0.02, -1.2 + Math.sin(a) * 0.07); f.rotation.y = -a - PI / 2; });
  if (disco && DECOR.disko.visible) { disco.rotation.y += dt; disco.userData.lights.forEach((l, i) => { l.intensity = 2; l.position.set(Math.cos(R.time * 1.5 + i * PI) * 1.5, 2.2, -1.2 + Math.sin(R.time * 1.5 + i * PI) * 1.2); }); }
}
function updatePointers() {
  for (const Pn of PANELS) Pn.hoverNext = -1;
  const mark = (o, d) => { ray.set(o, d); ray.far = 5; const h = ray.intersectObjects(UIT, false)[0]; if (h) h.object.userData.panel.hoverNext = h.object.userData.panel.indexAt(h.uv); return h; };
  if (renderer.xr.isPresenting) for (const c of ctrls) { const o = new V3(), d = new V3(); ctrlRay(c, o, d); const h = mark(o, d); c.userData.line.visible = !c.userData.tool; c.userData.line.scale.z = h ? h.distance : 1.5; }
  else { const [o, d] = deskRay(); mark(o, d); }
  for (const Pn of PANELS) if (Pn.hoverNext !== Pn.hover) { Pn.hover = Pn.hoverNext; Pn.refresh(); }
}

/* ---------- käivitus ---------- */
buildRoom(); buildTools(); buildUI(); buildJaw(); buildDecor(); buildConfetti(); buildControllers();
camera.position.set(0, 1.55, 0); camera.rotation.x = -0.22;
document.body.appendChild(VRButton.createButton(renderer, { optionalFeatures: ['local-floor', 'bounded-floor', 'hand-tracking'] }));
renderer.xr.addEventListener('sessionstart', () => { initAudio(); rig.position.set(0, 0, 0.05); camera.position.set(0, 0, 0); camera.rotation.set(0, 0, 0); $('info').style.display = 'none'; $('cross').style.display = 'none'; if (deskTool) { release(deskTool); deskTool = null; } });
renderer.xr.addEventListener('sessionend', () => { rig.position.set(0, 0, 0.45); camera.position.set(0, 1.55, 0); $('cross').style.display = ''; });
const clock = new THREE.Clock(); let acc = 0;
function step(dt) { R.time += dt; updateMove(dt); deskAim(); updatePatients(dt); scene.updateMatrixWorld(); treat(dt); updateCuff(dt); updateGloves(dt); updateButtonsTouch(); updateSpecial(dt); updateHands(); updateWash(dt); updateTools(dt); updateConfetti(dt); updatePointers(); music(dt); acc += dt; if (acc > 0.5) { acc = 0; if (P && P.case.steps[P.step]?.special === 'cpr') card.refresh(); } }
renderer.setAnimationLoop(() => { const dt = Math.min(clock.getDelta(), 0.05); step(dt); renderer.render(scene, camera); });
window.G = { S, R, TOOLS, TEETH, CASES, VET, get P() { return P; }, Q, grab, release, deskHand, step, camera, rig, sim: s => { for (let t = 0; t < s; t += 0.05) step(0.05); }, zoneWorld, cprPush, chartClick, buy, makePatient, pump, pressDefi, get VIAL() { return VIAL; }, setP: p => { P = p; } };
</script>
</body>
</html>
