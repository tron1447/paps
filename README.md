[Uploading index (42).html…]()
<!doctype html>
<html lang="et">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Kukerpall VR</title>
<style>
:root{--ink:#1d1b3a;--card:rgba(255,255,255,.95);--pink:#ff3d8b;--blue:#2f9bff}
html,body{margin:0;height:100%;overflow:hidden;background:#8fd3ff;font-family:system-ui,sans-serif;color:var(--ink)}
#menu{position:fixed;inset:0;display:flex;align-items:center;justify-content:center;padding:16px;box-sizing:border-box;z-index:10;overflow:auto}
#menu .card{background:var(--card);border-radius:22px;padding:20px 22px;max-width:440px;width:100%;box-shadow:0 10px 40px rgba(0,0,0,.18);margin:auto}
#menu h1{margin:0 0 4px;font-size:30px;color:var(--pink)}
#menu p{margin:4px 0 10px;font-size:14px;line-height:1.45}
#menu label.t{display:block;font-size:13px;font-weight:700;margin:10px 0 4px}
#menu input[type=text]{width:100%;box-sizing:border-box;font-size:18px;padding:9px 12px;border-radius:12px;border:2px solid #ccd}
#code{text-transform:uppercase;letter-spacing:3px;font-weight:700}
.row{display:flex;gap:8px;flex-wrap:wrap;align-items:center}
.row label{display:flex;align-items:center;gap:6px;font-size:14px;margin-right:10px}
.sw{width:34px;height:34px;border-radius:50%;border:3px solid #fff;box-shadow:0 0 0 2px #ccd;cursor:pointer;padding:0}
.sw.on{box-shadow:0 0 0 3px var(--ink)}
.hat{border:2px solid #ccd;background:#fff;border-radius:10px;padding:6px 10px;font-size:14px;cursor:pointer}
.hat.on{border-color:var(--ink);background:#eef}
button.big{flex:1;min-width:150px;font-size:17px;font-weight:700;padding:13px;border:0;border-radius:14px;color:#fff;cursor:pointer;margin-top:14px}
#solo{background:var(--blue)}#online{background:var(--pink)}
#status{margin-top:10px;font-size:14px;min-height:18px}
#hud{position:fixed;left:12px;top:12px;background:var(--card);padding:8px 12px;border-radius:12px;font-size:13px;line-height:1.45;max-width:360px;display:none;z-index:5}
details{font-size:13px;margin-top:10px}
canvas{touch-action:none}
#touch{display:none;position:fixed;inset:0;pointer-events:none;z-index:6;user-select:none;-webkit-user-select:none}
#touch.on{display:block}
#stick{position:absolute;left:24px;bottom:28px;width:140px;height:140px;border-radius:50%;background:rgba(255,255,255,.28);border:3px solid rgba(255,255,255,.7);pointer-events:auto;touch-action:none}
#knob{position:absolute;left:45px;top:45px;width:50px;height:50px;border-radius:50%;background:rgba(255,255,255,.9)}
.tb{position:absolute;pointer-events:auto;touch-action:none;border:0;border-radius:50%;font-weight:800;color:#fff;font-size:16px;box-shadow:0 4px 12px rgba(0,0,0,.2)}
#tJump{right:28px;bottom:36px;width:96px;height:96px;background:var(--pink)}
#tGrab{right:136px;bottom:30px;width:72px;height:72px;background:var(--blue)}
#tEmote{right:40px;bottom:146px;width:60px;height:60px;background:#f59e0b;font-size:24px}
#startBtn{display:none;position:fixed;left:50%;top:14px;transform:translateX(-50%);z-index:8;font-size:20px;font-weight:800;padding:14px 26px;border:0;border-radius:16px;background:#22c55e;color:#fff;box-shadow:0 6px 18px rgba(0,0,0,.2);cursor:pointer}
small{opacity:.8}
</style>
<script type="importmap">{"imports":{"three":"https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.module.js","three/addons/":"https://cdn.jsdelivr.net/npm/three@0.160.0/examples/jsm/"}}</script>
</head>
<body>
<div id="menu"><div class="card">
  <h1>Kukerpall VR</h1>
  <p>Takistusradade peomäng robotite ja sõpradega. Arvutis ja VR-is saab mängida koos – kõik avavad sama lingi ja panevad sama ruumi koodi.</p>
  <label class="t" for="name">Sinu nimi</label>
  <input type="text" id="name" maxlength="12" placeholder="Nimi">
  <label class="t">Värv</label><div class="row" id="colors"></div>
  <label class="t">Müts</label><div class="row" id="hats"></div>
  <label class="t" for="code">Ruumi kood (sõpradel sama)</label>
  <input type="text" id="code" maxlength="6">
  <label class="t">VR-is liikumine</label>
  <div class="row">
    <label><input type="radio" name="loco" value="arms" checked> Kätega (ahv)</label>
    <label><input type="radio" name="loco" value="stick"> Pulgaga</label>
  </div>
  <div class="row" style="margin-top:6px">
    <label><input type="checkbox" id="easy"> Lihtne režiim (noorematele)</label>
    <label><input type="checkbox" id="music" checked> Muusika</label>
  </div>
  <div class="row">
    <button class="big" id="solo">Mängi robotitega</button>
    <button class="big" id="online">Mängi sõpradega</button>
  </div>
  <div id="status"></div>
  <details><summary><b>Juhtimine</b></summary>
  <p><b>Arvutis:</b> klõpsa ekraanil, WASD + hiir, tühik = hüppa, E (hoia) = haara, 1–4 = emotikonid, Enter = alusta, R = tagasi ootesaali (ainult juht).<br>
  <b>VR kätega:</b> lükka kätega vastu maad ja seinu, vehi hooga, et hüpata. Parem pulk vasak/parem = pööra, <b>üles/alla = maapinna kõrgus</b>. Grip = haara. Käsi üle pea ja lehvita = „Tere!”. Pulgavajutus = emotikon, Y = „GG”. B (hoia) = tagasi ootesaali.<br>
  <b>VR pulgaga:</b> vasak pulk = liigu, A/X = hüppa, muu sama.</p></details>
  <p><small><b>Telefonis:</b> vasak ring = liigu, lohista ekraani = vaata ringi, nupud = hüppa / haara / emotikon.<br>
  <b>Alustamine:</b> üksi mängides vajuta „Alusta” või astu rohelisele ringile. Sõpradega mängides alustab <b>esimene ruumi tulnud mängija</b>.</small></p>
</div></div>
<div id="hud"></div>
<button id="startBtn">▶ ALUSTA MÄNGU</button>
<div id="touch">
  <div id="stick"><div id="knob"></div></div>
  <button class="tb" id="tJump">HÜPPA</button>
  <button class="tb" id="tGrab">HAARA</button>
  <button class="tb" id="tEmote">😊</button>
</div>
<script type="module">
import * as THREE from 'three';
import { VRButton } from 'three/addons/webxr/VRButton.js';
const TRYSTERO_URL = 'https://cdn.jsdelivr.net/npm/trystero@0.25.0/+esm';

// =====================================================================
// PÕHI
// =====================================================================
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setPixelRatio(Math.min(devicePixelRatio, 2));
renderer.setSize(innerWidth, innerHeight);
renderer.xr.enabled = true;
renderer.xr.setReferenceSpaceType('local-floor');
document.body.appendChild(renderer.domElement);
const vrBtn = VRButton.createButton(renderer);
vrBtn.style.zIndex = 20;
document.body.appendChild(vrBtn);

const scene = new THREE.Scene();
scene.background = new THREE.Color(0x8fd3ff);
scene.fog = new THREE.Fog(0x8fd3ff, 45, 170);
const camera = new THREE.PerspectiveCamera(70, innerWidth / innerHeight, 0.05, 300);
const rig = new THREE.Group();
scene.add(rig); rig.add(camera);
camera.position.set(0, 1.6, 0);
addEventListener('resize', () => { camera.aspect = innerWidth / innerHeight; camera.updateProjectionMatrix(); renderer.setSize(innerWidth, innerHeight); });

scene.add(new THREE.HemisphereLight(0xffffff, 0x9977bb, 1.15));
const sun = new THREE.DirectionalLight(0xffffff, 1.3); sun.position.set(10, 30, 12); scene.add(sun);

const mats = {};
const mat = c => mats[c] || (mats[c] = new THREE.MeshLambertMaterial({ color: c }));
const V3 = THREE.Vector3;
const now = () => performance.now() / 1000;
const rnd = (a, b) => a + Math.random() * (b - a);

const goo = new THREE.Mesh(new THREE.PlaneGeometry(500, 500), mat(0xff5fa2));
goo.rotation.x = -Math.PI / 2; goo.position.y = -6; scene.add(goo);
for (let i = 0; i < 30; i++) {
  const g = new THREE.Group();
  for (let k = 0; k < 3; k++) { const s = new THREE.Mesh(new THREE.SphereGeometry(2 + Math.random() * 2, 10, 8), mat(0xffffff)); s.position.set(k * 2.5, Math.random(), 0); g.add(s); }
  const side = Math.random() < 0.5 ? -1 : 1;
  g.position.set(side * (25 + Math.random() * 40), 8 + Math.random() * 22, 25 - Math.random() * 160); scene.add(g);
}

// =====================================================================
// MÄNGU OLEK (jagatud; peremees otsustab)
// =====================================================================
const net = { on: false, id: 'mina', room: null, isHost: true, hostId: 'mina', offset: 0, hasOffset: false, peers: new Map() };
const T = () => net.isHost ? now() : now() + net.offset;
const show = { sid: '', stage: 'lobby', rn: 0, rt: 'lobby', ti: 0, rid: '', sa: 0, ea: 0, part: [], alive: [], inR: [], q: [], fo: [],
  tails: [], winner: '', easy: false, crowns: {}, final: false, lastOut: [], quota: 0, ready: 0 };
const hostOnly = { padT: 0, falls: {}, immune: {}, prevType: '', tailTimer: 0 };
const S = () => show.easy ? 0.65 : 1;
const SURVIVAL = { areen: 1, plaadid: 1 };
let myName = 'Mängija', loco = 'arms', roomCode = '', musicOn = true, wantEasy = false;
const HUMAN_COLORS = [0xff3d8b, 0x2f9bff, 0x22c55e, 0xf59e0b, 0x8b5cf6, 0x06b6d4, 0xef4444, 0xfacc15];
const HATS = ['Puudub', 'Müts', 'Kõrvad', 'Sarv', 'Lill', 'Propeller'];
let myColor = HUMAN_COLORS[Math.floor(Math.random() * HUMAN_COLORS.length)], myHat = 1;
let started = false, iAmOut = false, spectating = false, myFinishTime = 0;

// =====================================================================
// HELID (WebAudio, kõik sünteesitud)
// =====================================================================
let ac = null, master, sfxGain, musicGain;
function audioInit() {
  if (ac) { if (ac.state === 'suspended') ac.resume(); return; }
  const AC = window.AudioContext || window.webkitAudioContext; if (!AC) return;
  ac = new AC(); master = ac.createGain(); master.gain.value = 0.6; master.connect(ac.destination);
  sfxGain = ac.createGain(); sfxGain.connect(master);
  musicGain = ac.createGain(); musicGain.gain.value = musicOn ? 0.16 : 0; musicGain.connect(master);
  startMusic();
}
function tone(f0, f1, dur, type = 'square', vol = 0.3, delay = 0, dest) {
  const t = ac.currentTime + delay, o = ac.createOscillator(), g = ac.createGain();
  o.type = type; o.frequency.setValueAtTime(f0, t); o.frequency.exponentialRampToValueAtTime(Math.max(20, f1), t + dur);
  g.gain.setValueAtTime(vol, t); g.gain.exponentialRampToValueAtTime(0.001, t + dur);
  o.connect(g); g.connect(dest || sfxGain); o.start(t); o.stop(t + dur + 0.05);
}
let noiseBuf = null;
function noise(dur, vol, freq = 1200, delay = 0) {
  if (!noiseBuf) { noiseBuf = ac.createBuffer(1, ac.sampleRate * 2, ac.sampleRate); const d = noiseBuf.getChannelData(0); for (let i = 0; i < d.length; i++) d[i] = Math.random() * 2 - 1; }
  const t = ac.currentTime + delay, s = ac.createBufferSource(), f = ac.createBiquadFilter(), g = ac.createGain();
  s.buffer = noiseBuf; f.type = 'bandpass'; f.frequency.value = freq; f.Q.value = 0.8;
  g.gain.setValueAtTime(vol, t); g.gain.exponentialRampToValueAtTime(0.001, t + dur);
  s.connect(f); f.connect(g); g.connect(sfxGain); s.start(t); s.stop(t + dur + 0.05);
}
const SFX = {
  jump: () => tone(300, 700, 0.15, 'square', 0.12),
  bounce: () => tone(140, 900, 0.4, 'sine', 0.35),
  plop: () => { tone(420, 60, 0.4, 'sine', 0.35); noise(0.25, 0.2, 500); },
  ding: () => { tone(880, 880, 0.12, 'triangle', 0.25); tone(1320, 1320, 0.3, 'triangle', 0.25, 0.1); },
  beep: () => tone(660, 660, 0.15, 'square', 0.18),
  go: () => tone(990, 990, 0.45, 'square', 0.22),
  cheer: () => { noise(1.4, 0.22, 2400); noise(1.0, 0.15, 900, 0.1); },
  out: () => [392, 370, 349, 311].forEach((f, i) => tone(f, f * 0.97, 0.32, 'sawtooth', 0.12, i * 0.3)),
  fanfare: () => { [523, 659, 784, 1047].forEach((f, i) => tone(f, f, 0.22, 'square', 0.18, i * 0.14)); tone(1047, 1047, 0.7, 'square', 0.18, 0.6); },
  hit: () => { noise(0.15, 0.3, 700); tone(220, 80, 0.15, 'square', 0.18); },
  grab: () => tone(500, 300, 0.1, 'triangle', 0.2),
  tail: () => tone(600, 1300, 0.25, 'triangle', 0.25),
  pop: () => tone(800, 1600, 0.08, 'sine', 0.2),
  tap: () => tone(180, 120, 0.05, 'sine', 0.12),
};
function sfx(n) { if (!ac) return; try { SFX[n](); } catch (e) {} }
const BASS = [48, 0, 48, 0, 55, 0, 48, 0, 53, 0, 53, 0, 55, 0, 52, 0];
const MEL = [72, 0, 76, 79, 0, 76, 72, 0, 74, 0, 77, 0, 79, 77, 76, 74];
const DRUM = [1, 0, 2, 0, 1, 0, 2, 0, 1, 0, 2, 0, 1, 1, 2, 0];
function note(m, t, d, type, v) {
  const f = 440 * Math.pow(2, (m - 69) / 12), o = ac.createOscillator(), g = ac.createGain();
  o.type = type; o.frequency.value = f;
  g.gain.setValueAtTime(v, t); g.gain.exponentialRampToValueAtTime(0.001, t + d);
  o.connect(g); g.connect(musicGain); o.start(t); o.stop(t + d + 0.05);
}
function startMusic() {
  let step = 0, nextT = ac.currentTime + 0.1;
  setInterval(() => {
    if (!ac || ac.state !== 'running') return;
    if (nextT < ac.currentTime) nextT = ac.currentTime + 0.05;
    while (nextT < ac.currentTime + 0.3) {
      const s = step % 16, bar = Math.floor(step / 16) % 4, tr = [0, 0, 5, 7][bar];
      if (BASS[s]) note(BASS[s] + tr, nextT, 0.22, 'triangle', 0.55);
      if (MEL[s] && (bar % 2 === 0 || s % 2 === 0)) note(MEL[s] + tr, nextT, 0.16, 'square', 0.13);
      if (DRUM[s] === 1) note(36, nextT, 0.08, 'sine', 0.5);
      if (DRUM[s] === 2) note(84, nextT, 0.03, 'square', 0.05);
      nextT += 60 / 132 / 4; step++;
    }
  }, 100);
}
function setMusic(on) { musicOn = on; if (musicGain) musicGain.gain.value = on ? 0.16 : 0; }

// =====================================================================
// RADADE EHITAMINE
// =====================================================================
const trackGroup = new THREE.Group(); scene.add(trackGroup);
let TR = null;
function newTR(kind, name) {
  return { kind, name, solids: [], movers: [], sweepers: [], tiles: [], balls: [], cps: [], wps: [], finishZ: -90,
    outY: -6, gooY: -6, spawn: raceSpawn, center: new V3(0, 0, -12) };
}
function raceSpawn(i, n, out) { out.set(-4 + (i % 5) * 2, 0.3, 1.0 + Math.floor(i / 5) * 1.3); }
function box(x, top, z, w, h, d, color, opts = {}) {
  const m = new THREE.Mesh(new THREE.BoxGeometry(w, h, d), mat(color));
  m.position.set(x, top - h / 2, z); trackGroup.add(m);
  const s = Object.assign({ type: 'box', mesh: m, hw: w / 2, hh: h / 2, hd: d / 2, vel: new V3(), delta: new V3(), active: true }, opts);
  if (s.move) { s.move.base = m.position[s.move.axis]; s.move.init = false; TR.movers.push(s); }
  if (s.bounce) { const ring = new THREE.Mesh(new THREE.TorusGeometry(Math.min(w, d) * 0.38, 0.08, 8, 24), mat(0xffffff)); ring.rotation.x = Math.PI / 2; ring.position.y = h / 2 + 0.02; m.add(ring); }
  TR.solids.push(s); return s;
}
function cyl(x, top, z, r, h, color, opts = {}) {
  const m = new THREE.Mesh(new THREE.CylinderGeometry(r, r, h, 40), mat(color));
  m.position.set(x, top - h / 2, z); trackGroup.add(m);
  const s = Object.assign({ type: 'cyl', mesh: m, r, hh: h / 2, vel: new V3(), delta: new V3(), active: true }, opts);
  TR.solids.push(s); return s;
}
function sweeper(x, z, baseTop, L, omega, bars = 1, phase = 0) {
  const g = new THREE.Group(); g.position.set(x, baseTop + 0.55, z); trackGroup.add(g);
  for (let k = 0; k < bars; k++) {
    const arm = new THREE.Group(); arm.rotation.y = k * Math.PI / bars; g.add(arm);
    const bar = new THREE.Mesh(new THREE.CylinderGeometry(0.22, 0.22, L * 2, 16), mat(0xff3b3b));
    bar.rotation.z = Math.PI / 2; arm.add(bar);
    for (let i = -3; i <= 3; i++) { if (!i) continue; const st = new THREE.Mesh(new THREE.CylinderGeometry(0.235, 0.235, 0.4, 16), mat(0xffffff)); st.rotation.z = Math.PI / 2; st.position.x = i * L / 3.5; arm.add(st); }
  }
  const sw = { x, z, y: baseTop + 0.55, baseTop, L, omega, bars, phase, a: 0, group: g, active: true, angleFn: null, activeFn: null };
  TR.sweepers.push(sw); return sw;
}
function tileGrid(x0, z0, cols, rows, top, opts = {}, size = 1.8, step = 1.9) {
  const cols4 = opts.colors || [0xff7eb6, 0x5ec8ff, 0xffd166, 0x7be495];
  for (let r = 0; r < rows; r++) for (let c = 0; c < cols; c++) {
    const x = x0 + c * step, z = z0 - r * step;
    const s = box(x, top, z, size, 0.5, size, cols4[(r + c) % cols4.length], { tile: true, state: 'idle', t: 0, idx: TR.tiles.length, noReturn: !!opts.noReturn, layerTop: top });
    s.home = s.mesh.position.clone();
    TR.tiles.push(s);
  }
}
function ball(x, zFar, zNear, top, r, speed, offset, color) {
  const g = new THREE.Group();
  const m = new THREE.Mesh(new THREE.SphereGeometry(r, 20, 14), mat(color)); g.add(m);
  const band = new THREE.Mesh(new THREE.TorusGeometry(r * 1.0, r * 0.12, 8, 24), mat(0xffffff)); band.rotation.y = Math.PI / 2; m.add(band);
  trackGroup.add(g);
  TR.balls.push({ g, m, x, zFar, zNear, top, R: r, r, speed, offset, pos: new V3() });
}
function arch(z, top, color) {
  const g = new THREE.Group();
  [-5, 5].forEach(x => { const p = new THREE.Mesh(new THREE.BoxGeometry(0.5, 4, 0.5), mat(color)); p.position.set(x, 2, 0); g.add(p); });
  const t = new THREE.Mesh(new THREE.BoxGeometry(10.5, 0.8, 0.5), mat(color)); t.position.y = 4.2; g.add(t);
  g.position.set(0, top, z); trackGroup.add(g);
}
function crownMesh(scale = 1) {
  const crown = new THREE.Group();
  const band = new THREE.Mesh(new THREE.CylinderGeometry(0.6, 0.6, 0.4, 24, 1, true), new THREE.MeshLambertMaterial({ color: 0xffc400, side: THREE.DoubleSide }));
  crown.add(band);
  for (let i = 0; i < 6; i++) { const c = new THREE.Mesh(new THREE.ConeGeometry(0.15, 0.4, 8), mat(0xffc400)); const a = i / 6 * Math.PI * 2; c.position.set(Math.cos(a) * 0.55, 0.4, Math.sin(a) * 0.55); crown.add(c); }
  crown.scale.setScalar(scale); return crown;
}
function finishDeco(z, top) {
  arch(z, top, 0xff7eb6);
  const line = new THREE.Mesh(new THREE.BoxGeometry(10, 0.02, 0.5), new THREE.MeshBasicMaterial({ color: 0xffffff }));
  line.position.set(0, top + 0.01, z); trackGroup.add(line);
  const crown = crownMesh(); crown.position.set(0, top + 2.2, z - 5); crown.userData.spin = true; crown.userData.baseY = top + 2.2; trackGroup.add(crown);
}
function textSprite(text, w = 2.4, color = '#1d1b3a', bg = 'rgba(255,255,255,.9)') {
  const cv = document.createElement('canvas'); cv.width = 512; cv.height = 128;
  const ctx = cv.getContext('2d');
  ctx.fillStyle = bg; ctx.beginPath(); ctx.roundRect(0, 0, 512, 128, 50); ctx.fill();
  ctx.fillStyle = color; ctx.font = 'bold 56px system-ui'; ctx.textAlign = 'center'; ctx.textBaseline = 'middle';
  ctx.fillText(text, 256, 68);
  const sp = new THREE.Sprite(new THREE.SpriteMaterial({ map: new THREE.CanvasTexture(cv), depthTest: false }));
  sp.scale.set(w, w / 4, 1); sp.renderOrder = 40; return sp;
}
function specPlatform() {
  box(0, 8, 16, 10, 1, 6, 0xd0bfff, { spec: true });
  box(-5.2, 8.7, 16, 0.4, 0.7, 6, 0xffffff); box(5.2, 8.7, 16, 0.4, 0.7, 6, 0xffffff);
  box(0, 8.7, 19.2, 10.8, 0.7, 0.4, 0xffffff); box(0, 8.5, 12.8, 10.8, 0.5, 0.4, 0xffffff);
  const s = textSprite('Pealtvaatajad', 2.4); s.position.set(0, 10.3, 18.8); trackGroup.add(s);
}
function cp(trig, x, y, z) { TR.cps.push({ trig, pos: new V3(x, y, z) }); }
const C = { green: 0x7be495, yellow: 0xffd166, pink: 0xff7eb6, blue: 0x5ec8ff, purple: 0xb388ff, red: 0xff5c5c, orange: 0xffa94d, ice: 0xd8f3ff };

// ---------- võidujooksu rajad ----------
const TRACKS = [
  { name: 'Takistusrada', build() {
    box(0, 0, 0, 10, 1, 10, C.green); arch(4.5, 0, C.blue);
    box(-1.5, 0, -8, 3, 1, 3, C.yellow); box(1.5, 0, -11.5, 3, 1, 3, C.pink); box(-1, 0, -15, 3, 1, 3, C.blue);
    box(0, 0, -21, 8, 1, 8, C.purple);
    cyl(0, 0, -32, 7, 1, C.yellow); cyl(0, 1.4, -32, 0.6, 1.4, C.red); sweeper(0, -32, 0, 7, 1.1);
    box(0, 0, -42.5, 4, 1, 7, C.blue);
    const m1 = box(0, 0, -48.5, 3, 0.5, 3, C.pink, { move: { axis: 'x', amp: 3, speed: 1.1, phase: 0 } });
    const m2 = box(0, 0, -52, 3, 0.5, 3, C.yellow, { move: { axis: 'x', amp: 3, speed: 1.1, phase: 2.1 } });
    const m3 = box(0, 0, -55.5, 3, 0.5, 3, C.green, { move: { axis: 'x', amp: 3, speed: 1.1, phase: 4.2 } });
    box(0, 0, -62, 8, 1, 8, C.purple);
    tileGrid(-3.8, -67, 5, 6, 0);
    box(0, 0, -84, 6, 1, 13, C.blue);
    [[-81, 0], [-84.5, 1.6], [-88, 3.2]].forEach(([z, ph]) => box(0, 1.2, z, 1.6, 1.2, 1.2, C.red, { move: { axis: 'x', amp: 2.2, speed: 1.6, phase: ph }, pusher: true }));
    box(0, 0, -96, 10, 1, 10, C.yellow);
    TR.finishZ = -93; finishDeco(-93, 0);
    cp(99, 0, 0, 1.5); cp(-17.5, 0, 0, -21); cp(-58.5, 0, 0, -62); cp(-78, 0, 0, -79.5);
    TR.wps = [
      { x: -1.5, z: -6.8 }, { x: -1.5, z: -8 }, { x: 1.5, z: -11.5 }, { x: -1, z: -15 },
      { x: 0, z: -20, wide: 2.5 }, { x: 0, z: -25.5, wide: 1 }, { side: 3.5, z: -32 }, { x: 0, z: -38.5, wide: 1.5 },
      { x: 0, z: -45.5, wide: 0.8 }, { plat: m1 }, { plat: m2 }, { plat: m3 },
      { x: 0, z: -59, wide: 2.5 }, { x: 0, z: -77, wide: 3 }, { x: 0, z: -90, wide: 2 }, { x: 0, z: -97, wide: 3 },
    ];
  } },
  { name: 'Seinte rada', build() {
    box(0, 0, 0, 10, 1, 10, C.green); arch(4.5, 0, C.orange);
    box(0, 0, -27, 10, 1, 44, C.blue);
    const walls = [-12, -21, -30, -39].map((z, k) => box(0, 2.2, z, 7, 2.2, 0.8, [C.pink, C.yellow, C.purple, C.orange][k],
      { move: { axis: 'x', amp: 3, speed: 0.7 + k * 0.18, phase: k * 1.7 }, pusher: true }));
    box(0, 0.6, -51, 4, 1.6, 3, C.pink); box(0, 1.2, -55, 4, 2.2, 3, C.yellow); box(0, 1.8, -59, 4, 2.8, 3, C.green);
    cyl(0, 1.8, -67, 6, 1, C.purple); cyl(0, 3.2, -67, 0.6, 1.4, C.red); sweeper(0, -67, 1.8, 6, 0.9, 2);
    box(0, 1.8, -77, 3, 1, 6, C.blue);
    tileGrid(-2.85, -82, 4, 4, 1.8);
    box(0, 1.8, -94, 10, 1, 10, C.yellow);
    TR.finishZ = -91.5; finishDeco(-91.5, 1.8);
    cp(99, 0, 0, 1.5); cp(-45, 0, 0, -46); cp(-57.5, 0, 1.8, -59); cp(-74.5, 0, 1.8, -77);
    TR.wps = [
      ...walls.map(w => ({ gap: w, z: w.mesh.position.z })),
      { x: 0, z: -47, wide: 2 }, { x: 0, z: -51 }, { x: 0, z: -55 }, { x: 0, z: -59 },
      { x: 0, z: -62, wide: 1 }, { x: 0, z: -72.5, wide: 1 }, { x: 0, z: -77 }, { x: 0, z: -81, wide: 1.5 },
      { x: 0, z: -89.5, wide: 2 }, { x: 0, z: -95, wide: 3 },
    ];
  } },
  { name: 'Pilverada', build() {
    box(0, 0, 0, 10, 1, 10, C.green); arch(4.5, 0, C.purple);
    const l1 = box(0, 0, -7.5, 3, 0.5, 3, C.pink, { move: { axis: 'y', amp: 1.2, speed: 0.9, phase: 0 } });
    box(0, 1.2, -12.5, 4, 1, 4, C.yellow);
    const l2 = box(0, 1.2, -17.5, 3, 0.5, 3, C.blue, { move: { axis: 'y', amp: 1.2, speed: 0.9, phase: 2 } });
    box(0, 2.4, -23, 6, 1, 6, C.purple);
    tileGrid(-0.95, -27.5, 2, 7, 2.4);
    box(0, 2.4, -44, 8, 1, 6, C.green);
    const m1 = box(0, 2.4, -50.5, 3, 0.5, 3, C.pink, { move: { axis: 'x', amp: 3, speed: 1.2, phase: 0 } });
    const m2 = box(0, 2.4, -54, 3, 0.5, 3, C.yellow, { move: { axis: 'x', amp: 3, speed: 1.2, phase: 2 } });
    cyl(0, 2.4, -62, 6, 1, C.yellow); cyl(0, 3.8, -62, 0.6, 1.4, C.red); sweeper(0, -62, 2.4, 6, -1.0, 2);
    box(0, 2.4, -74, 4, 1, 8, C.blue);
    box(0, 2.4, -84, 6, 1, 12, C.pink);
    [[-81, 0], [-84.5, 1.4], [-88, 2.8]].forEach(([z, ph]) => box(0, 3.6, z, 1.6, 1.2, 1.2, C.red, { move: { axis: 'x', amp: 2.2, speed: 1.8, phase: ph }, pusher: true }));
    box(0, 2.4, -96, 10, 1, 10, C.yellow);
    TR.finishZ = -93; finishDeco(-93, 2.4);
    cp(99, 0, 0, 1.5); cp(-20.5, 0, 2.4, -23); cp(-41.5, 0, 2.4, -44); cp(-70.5, 0, 2.4, -74);
    TR.wps = [
      { liftOn: l1 }, { liftOff: l1, top: 1.2, x: 0, z: -12.5 },
      { liftOn: l2 }, { liftOff: l2, top: 2.4, x: 0, z: -23 },
      { x: 0, z: -27, wide: 0.5 }, { x: 0, z: -39, wide: 0.5 }, { x: 0, z: -42, wide: 2 },
      { x: 0, z: -46.5, wide: 0.8 }, { plat: m1 }, { plat: m2 },
      { x: 0, z: -57, wide: 1 }, { x: 0, z: -67, wide: 1 }, { x: 0, z: -70.5 }, { x: 0, z: -77, wide: 1 },
      { x: 0, z: -90, wide: 2 }, { x: 0, z: -97, wide: 3 },
    ];
  } },
  { name: 'Jäärada', build() {
    box(0, 0, 0, 10, 1, 10, C.green); arch(4.5, 0, 0x5ec8ff);
    box(0, 0, -20, 10, 1, 30, C.ice, { ice: true });
    [-2.6, 2.6].forEach((x, k) => [0, 1].forEach(j => ball(x, -34, -5, 0, 0.9, 4, k * 7 + j * 14.5, [C.red, C.purple][k])));
    ball(0, -34, -5, 0, 0.9, 2.6, 3, C.orange);
    box(0, 0, -37.5, 6, 0.5, 4, C.orange, { bounce: true });
    box(0, 4, -44, 8, 1, 6, C.purple);
    box(0, 4, -55.5, 6, 1, 17, C.ice, { ice: true });
    sweeper(0, -55.5, 4, 3.6, 1.3, 1);
    box(-2, 4.25, -51, 1.2, 0.25, 1.2, C.orange, { bounce: true });
    box(0, 4, -70, 10, 1, 10, C.yellow);
    TR.finishZ = -67; finishDeco(-67, 4);
    cp(99, 0, 0, 1.5); cp(-30, 0, 0, -31); cp(-41.5, 0, 4, -44);
    TR.wps = [
      { x: 0, z: -6, wide: 2 }, { x: 0, z: -20, wide: 3 }, { x: 0, z: -34, wide: 2 }, { x: 0, z: -37.5, wide: 1 },
      { x: 0, z: -44, wide: 1.5 }, { x: 0, z: -50, wide: 1 }, { x: 0, z: -64, wide: 1.5 }, { x: 0, z: -71, wide: 3 },
    ];
  } },
];

// ---------- areenid ----------
function accelAngle(t, w0, acc, wmax) {
  const tc = (wmax - w0) / acc;
  if (t < tc) return w0 * t + 0.5 * acc * t * t;
  return w0 * tc + 0.5 * acc * tc * tc + wmax * (t - tc);
}
const ARENAS = {
  areen: { name: 'Keerlev areen', desc: 'Hüppa üle punase lati – ära kuku maha!', build() {
    cyl(0, 0, -12, 9, 1, C.purple); cyl(0, 1.4, -12, 0.7, 1.4, C.red);
    const a = sweeper(0, -12, 0, 9, 0, 1); a.angleFn = t => accelAngle(t * S(), 0.8, 0.035, 3.0);
    const b = sweeper(0, -12, 0, 9, 0, 1); b.angleFn = t => 1.6 - accelAngle(t * S(), 0.6, 0.03, 2.5); b.activeFn = t => t > 20;
    // alguskohad latist eemal (latt on alguses x-teljel)
    TR.spawn = (i, n, out) => { const half = i % 2, k = Math.floor(i / 2), m = Math.ceil(n / 2); const ang = 0.8 + (k + 0.5) / Math.max(1, m) * (Math.PI - 1.6) + half * Math.PI; out.set(Math.cos(ang) * 6, 0.3, -12 - Math.sin(ang) * 6); };
  } },
  plaadid: { name: 'Plaadisaar', desc: 'Plaadid kukuvad su all – püsi kõige kauem üleval!', build() {
    [0, -4, -8].forEach((top, L) => tileGrid(-6.65, -5.35, 8, 8, top, { noReturn: true, colors: [[C.pink, C.yellow], [C.blue, C.green], [C.purple, C.orange]][L] }));
    TR.outY = -12.5; TR.gooY = -15; TR.slowTiles = true;
    TR.spawn = (i, n, out) => { const s = TR.tiles[(i * 23 + 9) % 64]; out.set(s.home.x, 0.3, s.home.z); };
  } },
  saba: { name: 'Saba püüdmine', desc: 'Haara kellegi saba! Kellel lõpus saba on, pääseb edasi.', build() {
    box(0, 0, -12, 18, 1, 18, C.blue);
    box(-4.5, 1.2, -7.5, 2, 1.2, 2, C.pink); box(4.5, 1.2, -16.5, 2, 1.2, 2, C.yellow);
    box(4.5, 2, -7, 2, 2, 2, C.purple); box(-4.5, 0.6, -16.5, 3, 0.6, 3, C.green);
    box(0, 0.25, -12, 2.6, 0.5, 2.6, C.orange, { bounce: true });
    box(0, 2.2, -4.5, 3, 0.5, 3, C.pink, { move: { axis: 'x', amp: 5, speed: 0.6, phase: 0 } });
    box(-7, 0.25, -3, 2, 0.5, 2, C.orange, { bounce: true }); box(7, 0.25, -21, 2, 0.5, 2, C.orange, { bounce: true });
    TR.spawn = (i, n, out) => { const ang = i / Math.max(1, n) * Math.PI * 2; out.set(Math.cos(ang) * 6.5, 0.3, -12 + Math.sin(ang) * 6.5); };
  } },
};
function buildLobby() {
  box(0, 0, 0, 16, 1, 16, 0x9be7ff);
  TR.pad = cyl(0, 0.12, -4.5, 1.7, 0.24, 0x22c55e);
  const ps = textSprite('ALUSTA (juht)', 2.6, '#fff', 'rgba(34,197,94,.95)'); ps.position.set(0, 2.1, -4.5); trackGroup.add(ps);
  box(6, 0.25, 5, 2.4, 0.5, 2.4, C.orange, { bounce: true }); box(-6, 0.25, 5, 2.4, 0.5, 2.4, C.orange, { bounce: true });
  box(-6, 0.8, -4, 2, 0.8, 2, C.pink); box(-6, 1.6, -6.5, 2, 1.6, 2, C.yellow); box(-3.5, 2.4, -6.5, 2, 2.4, 2, C.purple);
  box(6, 0.6, -5, 3, 0.6, 3, C.green); box(6, 2.0, -5, 1, 0.6, 1, C.blue, { move: { axis: 'y', amp: 0.8, speed: 1, phase: 0 } });
  // kroonide tabel
  const cv = document.createElement('canvas'); cv.width = 768; cv.height = 420;
  const tex = new THREE.CanvasTexture(cv);
  const board = new THREE.Mesh(new THREE.PlaneGeometry(5.5, 3), new THREE.MeshBasicMaterial({ map: tex }));
  board.position.set(0, 3.3, -7.9); trackGroup.add(board);
  const post = new THREE.Mesh(new THREE.BoxGeometry(5.8, 3.3, 0.2), mat(0xff7eb6)); post.position.set(0, 3.3, -8.05); trackGroup.add(post);
  TR.board = { cv, tex, key: '' };
  TR.spawn = (i, n, out) => out.set(-4 + (i % 5) * 2, 0.3, 3.5 - Math.floor(i / 5) * 1.3);
}
function drawBoard() {
  const B = TR && TR.board; if (!B) return;
  const list = Object.entries(show.crowns).sort((a, b) => b[1] - a[1]).slice(0, 6);
  const key = JSON.stringify(list) + net.hostId + net.peers.size;
  if (key === B.key) return; B.key = key;
  const ctx = B.cv.getContext('2d');
  ctx.fillStyle = '#ffffff'; ctx.fillRect(0, 0, 768, 420);
  ctx.fillStyle = '#ff3d8b'; ctx.font = 'bold 64px system-ui'; ctx.textAlign = 'center'; ctx.fillText('Kroonid', 384, 75);
  ctx.font = 'bold 40px system-ui'; ctx.fillStyle = '#1d1b3a';
  if (!list.length) ctx.fillText('Keegi pole veel võitnud', 384, 180);
  list.forEach(([n, c], i) => { ctx.textAlign = 'left'; ctx.fillText((i + 1) + '. ' + n, 110, 140 + i * 44); ctx.textAlign = 'right'; ctx.fillText(c + ' kroon' + (c > 1 ? 'i' : ''), 660, 140 + i * 44); });
  ctx.textAlign = 'center'; ctx.fillStyle = '#22a35a'; ctx.font = 'bold 34px system-ui';
  ctx.fillText(net.on && net.peers.size ? 'Mängu alustab: ' + nameOf(net.hostId) : 'Vajuta ALUSTA või astu ringile', 384, 400);
  B.tex.needsUpdate = true;
}

function mapName(rt, ti) { return rt === 'race' ? TRACKS[ti].name : rt === 'lobby' ? 'Ootesaal' : ARENAS[rt].name; }
function buildMap(rt, ti) {
  trackGroup.traverse(o => { if (o.geometry) o.geometry.dispose(); if (o.material && o.material.map) o.material.map.dispose(); });
  trackGroup.clear();
  TR = newTR(rt, mapName(rt, ti));
  if (rt === 'lobby') buildLobby();
  else if (rt === 'race') TRACKS[ti].build();
  else ARENAS[rt].build();
  if (rt !== 'lobby') specPlatform();
  goo.position.y = TR.gooY;
  updateWorld(0.016, true);
}
function wpZ(wp) {
  if (wp.plat) return wp.plat.mesh.position.z;
  if (wp.liftOn) return wp.liftOn.mesh.position.z;
  if (wp.gap) return wp.gap.mesh.position.z;
  return wp.z;
}

// =====================================================================
// TEGELASED, MÜTSID, SABAD
// =====================================================================
function makeHat(type, y) {
  const g = new THREE.Group(); g.position.y = y;
  if (type === 1) { const c = new THREE.Mesh(new THREE.ConeGeometry(0.17, 0.34, 12), mat(0xe03131)); c.position.y = 0.15; g.add(c); const p = new THREE.Mesh(new THREE.SphereGeometry(0.06, 8, 6), mat(0xffffff)); p.position.y = 0.34; g.add(p); }
  else if (type === 2) { [-0.16, 0.16].forEach(x => { const e = new THREE.Mesh(new THREE.SphereGeometry(0.09, 10, 8), mat(0x8d5a3b)); e.position.set(x, 0.02, 0); g.add(e); const i = new THREE.Mesh(new THREE.SphereGeometry(0.05, 8, 6), mat(0xffb3c6)); i.position.set(x, 0.02, 0.05); g.add(i); }); }
  else if (type === 3) { const c = new THREE.Mesh(new THREE.ConeGeometry(0.05, 0.32, 10), mat(0xfff3b0)); c.position.set(0, 0.1, 0.12); c.rotation.x = 0.4; g.add(c); }
  else if (type === 4) { const st = new THREE.Mesh(new THREE.CylinderGeometry(0.015, 0.015, 0.2), mat(0x2f9e44)); st.position.y = 0.08; g.add(st); const ce = new THREE.Mesh(new THREE.SphereGeometry(0.05, 8, 6), mat(0xffd43b)); ce.position.y = 0.2; g.add(ce); for (let i = 0; i < 5; i++) { const a = i / 5 * Math.PI * 2; const p = new THREE.Mesh(new THREE.SphereGeometry(0.045, 8, 6), mat(0xff8fab)); p.position.set(Math.cos(a) * 0.08, 0.2, Math.sin(a) * 0.08); g.add(p); } }
  else if (type === 5) { const cap = new THREE.Mesh(new THREE.SphereGeometry(0.16, 12, 8, 0, Math.PI * 2, 0, Math.PI / 2), mat(0x4dabf7)); g.add(cap); const pr = new THREE.Group(); pr.position.y = 0.2; const bl = new THREE.Mesh(new THREE.BoxGeometry(0.45, 0.02, 0.06), mat(0xffd43b)); pr.add(bl); const ax = new THREE.Mesh(new THREE.CylinderGeometry(0.015, 0.015, 0.06), mat(0x333344)); ax.position.y = -0.03; pr.add(ax); g.add(pr); g.userData.prop = pr; }
  return g;
}
function makeTail() {
  const g = new THREE.Group();
  for (let i = 0; i < 4; i++) { const s = new THREE.Mesh(new THREE.SphereGeometry(0.1 - i * 0.012, 10, 8), mat(i % 2 ? 0xffd43b : 0xff922b)); s.position.set(0, -i * 0.05, -i * 0.13); g.add(s); }
  g.position.set(0, 0.55, -0.3); g.visible = false; return g;
}
function addExtras(g, headTop) {
  const crown = crownMesh(0.3); crown.position.y = headTop + 0.25; crown.visible = false; g.add(crown); g.userData.crown = crown;
  const tail = makeTail(); g.add(tail); g.userData.tail = tail;
  const bub = textSprite('', 1.6); bub.position.y = headTop + 0.75; bub.visible = false; g.add(bub); g.userData.bubble = bub; g.userData.bubT = 0;
}
function makeBot(color, hat) {
  const g = new THREE.Group();
  const body = new THREE.Mesh(new THREE.BoxGeometry(0.6, 0.7, 0.5), mat(color)); body.position.y = 0.53; g.add(body);
  const head = new THREE.Mesh(new THREE.BoxGeometry(0.55, 0.42, 0.48), mat(color)); head.position.y = 1.1; g.add(head);
  const visor = new THREE.Mesh(new THREE.BoxGeometry(0.44, 0.22, 0.02), mat(0x1b1b2f)); visor.position.set(0, 1.12, 0.25); g.add(visor);
  const eyeM = new THREE.MeshBasicMaterial({ color: 0x9ffcff });
  [-0.1, 0.1].forEach(x => { const e = new THREE.Mesh(new THREE.BoxGeometry(0.08, 0.08, 0.02), eyeM); e.position.set(x, 1.13, 0.265); g.add(e); });
  [-0.15, 0.15].forEach(x => { const l = new THREE.Mesh(new THREE.BoxGeometry(0.18, 0.18, 0.22), mat(0x333344)); l.position.set(x, 0.09, 0); g.add(l); });
  if (hat) { const h = makeHat(hat, 1.31); g.add(h); g.userData.hat = h; }
  else { const ant = new THREE.Mesh(new THREE.CylinderGeometry(0.02, 0.02, 0.25), mat(0x333344)); ant.position.y = 1.43; g.add(ant); }
  addExtras(g, 1.31);
  scene.add(g); return g;
}
function makeHuman(color, name, hat) {
  const g = new THREE.Group();
  const body = new THREE.Mesh(new THREE.CapsuleGeometry(0.3, 0.5, 6, 14), mat(color)); body.position.y = 0.6; g.add(body);
  const head = new THREE.Mesh(new THREE.SphereGeometry(0.27, 16, 12), mat(color)); head.position.y = 1.22; g.add(head);
  const face = new THREE.Mesh(new THREE.SphereGeometry(0.2, 14, 10), mat(0xffe0c2)); face.position.set(0, 1.22, 0.12); face.scale.set(1, 0.85, 0.6); g.add(face);
  [-0.07, 0.07].forEach(x => { const e = new THREE.Mesh(new THREE.SphereGeometry(0.03, 8, 6), mat(0x111122)); e.position.set(x, 1.25, 0.25); g.add(e); });
  if (hat) { const h = makeHat(hat, 1.47); g.add(h); g.userData.hat = h; }
  const tag = nameSprite(name, color); tag.position.y = 2.05; g.add(tag);
  addExtras(g, 1.49);
  scene.add(g); return g;
}
function nameSprite(name, color) {
  const cv = document.createElement('canvas'); cv.width = 256; cv.height = 64;
  const ctx = cv.getContext('2d');
  ctx.fillStyle = 'rgba(20,20,50,.7)'; ctx.beginPath(); ctx.roundRect(0, 0, 256, 64, 30); ctx.fill();
  ctx.fillStyle = '#' + new THREE.Color(color).getHexString(); ctx.font = 'bold 38px system-ui'; ctx.textAlign = 'center'; ctx.textBaseline = 'middle';
  ctx.fillText(name || '?', 128, 34);
  const sp = new THREE.Sprite(new THREE.SpriteMaterial({ map: new THREE.CanvasTexture(cv), depthTest: false }));
  sp.scale.set(1.0, 0.25, 1); sp.renderOrder = 50; return sp;
}
function setBubble(g, text) {
  const sp = g.userData.bubble; if (!sp) return;
  const cv = document.createElement('canvas'); cv.width = 512; cv.height = 128;
  const ctx = cv.getContext('2d');
  ctx.fillStyle = 'rgba(255,255,255,.95)'; ctx.beginPath(); ctx.roundRect(0, 0, 512, 128, 50); ctx.fill();
  ctx.fillStyle = '#1d1b3a'; ctx.font = 'bold 60px system-ui'; ctx.textAlign = 'center'; ctx.textBaseline = 'middle'; ctx.fillText(text, 256, 68);
  if (sp.material.map) sp.material.map.dispose();
  sp.material.map = new THREE.CanvasTexture(cv); sp.material.needsUpdate = true;
  sp.visible = true; g.userData.bubT = 2.5;
}
function makeGlove(color) {
  const g = new THREE.Group();
  const fist = new THREE.Mesh(new THREE.SphereGeometry(0.07, 14, 10), mat(0x5a3e2b)); fist.scale.set(1, 0.8, 1.2); g.add(fist);
  const palm = new THREE.Mesh(new THREE.SphereGeometry(0.045, 10, 8), mat(color)); palm.position.y = -0.04; g.add(palm);
  return g;
}
function makeEnt(isPlayer) {
  return { isPlayer, pos: new V3(), vel: new V3(), onGround: false, standingOn: null, r: 0.3, h: isPlayer ? 1.5 : 1.4,
    cp: 0, finished: false, knock: 0, mesh: null, wi: 0, side: 1, jit: 0, speed: 4.5, skill: 1, walk: 0, holding: false,
    speedMul: 1, grabbedBy: null, lastSafe: new V3(), visible: true, aiT: 0, aiTarget: new V3() };
}
const player = makeEnt(true);
const BOT_COLORS = [0xff6b6b, 0x4dabf7, 0xffd43b, 0x69db7c, 0xda77f2, 0xff922b, 0x22b8cf, 0xf783ac, 0xa9e34b, 0x748ffc];
const botHat = i => (i * 7 + 3) % HATS.length;
let bots = [];
let remoteBots = [];
function animateBody(mesh, vel, onGround, dt, st) {
  const sp = Math.hypot(vel.x, vel.z);
  if (sp > 0.3) mesh.rotation.y = Math.atan2(vel.x, vel.z);
  st.walk = (st.walk || 0) + sp * dt * 3;
  mesh.rotation.z = onGround ? Math.sin(st.walk) * 0.08 * Math.min(1, sp) : 0.15;
}
function tickExtras(g, id, dt) {
  const u = g.userData;
  if (u.tail) u.tail.visible = show.rt === 'saba' && show.stage !== 'lobby' && show.tails.includes(id);
  if (u.crown) u.crown.visible = !!show.winner && show.winner === id && (show.stage === 'crown' || show.stage === 'lobby');
  if (u.hat) u.hat.visible = !(u.crown && u.crown.visible);
  if (u.hat && u.hat.userData.prop) u.hat.userData.prop.rotation.y += dt * 15;
  if (u.bubT > 0) { u.bubT -= dt; if (u.bubT <= 0) u.bubble.visible = false; }
}

// VR käed
const grips = [];
for (let i = 0; i < 2; i++) {
  const grip = renderer.xr.getControllerGrip(i);
  grip.add(makeGlove(i ? 0xff7eb6 : 0x5ec8ff)); rig.add(grip); grips.push(grip);
}

// =====================================================================
// HUD (kaamera ees)
// =====================================================================
function makePanel(w, h, cw, ch, pos) {
  const cv = document.createElement('canvas'); cv.width = cw; cv.height = ch;
  const tex = new THREE.CanvasTexture(cv);
  const m = new THREE.Mesh(new THREE.PlaneGeometry(w, h), new THREE.MeshBasicMaterial({ map: tex, transparent: true, depthTest: false }));
  m.renderOrder = 999; m.position.copy(pos); camera.add(m);
  return { cv, ctx: cv.getContext('2d'), tex, mesh: m };
}
const stats = makePanel(0.7, 0.075, 1180, 125, new V3(0, 0.3, -1.1));
const msg = makePanel(0.9, 0.5, 1024, 570, new V3(0, 0.02, -1.2));
const toast = makePanel(0.45, 0.06, 760, 100, new V3(0, -0.22, -1.1));
function rr(ctx, x, y, w, h, r) { ctx.beginPath(); ctx.roundRect(x, y, w, h, r); ctx.fill(); }
let lastStats = '';
function drawStats(text) {
  if (text === lastStats) return; lastStats = text;
  const { ctx, cv } = stats; ctx.clearRect(0, 0, cv.width, cv.height);
  ctx.fillStyle = 'rgba(20,20,50,0.55)'; rr(ctx, 0, 0, cv.width, cv.height, 40);
  ctx.fillStyle = '#fff'; ctx.font = 'bold 52px system-ui'; ctx.textAlign = 'center'; ctx.textBaseline = 'middle';
  ctx.fillText(text, cv.width / 2, cv.height / 2 + 4); stats.tex.needsUpdate = true;
}
let toastT = 0;
function showToast(text, time = 1.6) {
  const { ctx, cv } = toast; ctx.clearRect(0, 0, cv.width, cv.height);
  ctx.fillStyle = 'rgba(255,255,255,0.92)'; rr(ctx, 0, 0, cv.width, cv.height, 40);
  ctx.fillStyle = '#1d1b3a'; ctx.font = 'bold 48px system-ui'; ctx.textAlign = 'center'; ctx.textBaseline = 'middle';
  ctx.fillText(text, cv.width / 2, cv.height / 2 + 3); toast.tex.needsUpdate = true; toast.mesh.visible = true; toastT = time;
}
toast.mesh.visible = false;
let msgTimer = 0, msgSticky = false, lastMsgKey = '';
function showMsg(title, lines = [], time = 2, sticky = false) {
  const key = title + '|' + lines.join('|');
  msgTimer = time; msgSticky = sticky; msg.mesh.visible = true;
  if (key === lastMsgKey) return; lastMsgKey = key;
  const { ctx, cv } = msg; ctx.clearRect(0, 0, cv.width, cv.height);
  const H = lines.length ? Math.min(cv.height, 200 + lines.length * 62) : 230;
  const y0 = (cv.height - H) / 2;
  ctx.fillStyle = 'rgba(255,255,255,0.93)'; rr(ctx, 0, y0, cv.width, H, 60);
  ctx.textAlign = 'center'; ctx.textBaseline = 'middle';
  ctx.fillStyle = '#ff3d8b'; ctx.font = 'bold ' + (title.length > 12 ? 82 : 110) + 'px system-ui';
  ctx.fillText(title, cv.width / 2, y0 + (lines.length ? 95 : H / 2));
  ctx.fillStyle = '#334'; ctx.font = 'bold 46px system-ui';
  lines.forEach((l, i) => ctx.fillText(l, cv.width / 2, y0 + 185 + i * 62));
  msg.tex.needsUpdate = true;
}
function hideMsg() { msg.mesh.visible = false; msgSticky = false; lastMsgKey = ''; }
hideMsg();

// konfetid
const CN = 400;
const cGeo = new THREE.BufferGeometry();
const cPos = new Float32Array(CN * 3), cCol = new Float32Array(CN * 3), cVel = [];
for (let i = 0; i < CN; i++) { const c = new THREE.Color().setHSL(Math.random(), 1, 0.6); cCol.set([c.r, c.g, c.b], i * 3); cVel.push(new V3()); cPos[i * 3 + 1] = -100; }
cGeo.setAttribute('position', new THREE.BufferAttribute(cPos, 3));
cGeo.setAttribute('color', new THREE.BufferAttribute(cCol, 3));
const confetti = new THREE.Points(cGeo, new THREE.PointsMaterial({ size: 0.12, vertexColors: true }));
confetti.frustumCulled = false; scene.add(confetti);
function burst(p) { for (let i = 0; i < CN; i++) { cPos.set([p.x, p.y + 2, p.z], i * 3); cVel[i].set((Math.random() - .5) * 8, 4 + Math.random() * 7, (Math.random() - .5) * 8); } }

// =====================================================================
// FÜÜSIKA
// =====================================================================
const G = 18, JUMP = 7, BOUNCE = 13;
function groundTopAt(x, z, y) {
  let best = -Infinity;
  for (const s of TR.solids) {
    if (!s.active) continue; const p = s.mesh.position, top = p.y + s.hh;
    if (top > y + 0.3) continue;
    const inside = s.type === 'box' ? (Math.abs(x - p.x) <= s.hw + 0.12 && Math.abs(z - p.z) <= s.hd + 0.12) : Math.hypot(x - p.x, z - p.z) <= s.r;
    if (inside && top > best) best = top;
  }
  return best;
}
function stepAhead(x, z, y) {
  for (const s of TR.solids) {
    if (!s.active || s.pusher) continue; const p = s.mesh.position, top = p.y + s.hh;
    if (top < y + 0.2 || top > y + 1.3) continue;
    const inside = s.type === 'box' ? (Math.abs(x - p.x) <= s.hw && Math.abs(z - p.z) <= s.hd) : Math.hypot(x - p.x, z - p.z) <= s.r;
    if (inside) return true;
  }
  return false;
}
function shakeTile(i, broadcast) {
  const s = TR.tiles[i]; if (!s || s.state !== 'idle') return;
  s.state = 'shake'; s.t = 0;
  if (broadcast && net.on && net.peers.size) net.tile.send({ i, r: show.rid });
}
function hurt(e) { if (e.isPlayer) { clearHands(); buzz(0, 0.8, 80); buzz(1, 0.8, 80); sfx('hit'); } }
function stepEntity(e, dt) {
  if (e.standingOn && e.standingOn.active && !e.holding) e.pos.add(e.standingOn.delta);
  if (!e.holding) e.vel.y -= G * dt;
  const prevY = e.pos.y;
  e.pos.addScaledVector(e.vel, dt);
  const wasGround = e.onGround;
  e.onGround = false; e.standingOn = null;
  for (const s of TR.solids) {
    if (!s.active) continue;
    const p = s.mesh.position, top = p.y + s.hh, bot = p.y - s.hh;
    const dx = e.pos.x - p.x, dz = e.pos.z - p.z;
    if (s.type === 'box') {
      const inside = Math.abs(dx) <= s.hw + 0.12 && Math.abs(dz) <= s.hd + 0.12;
      if (inside && e.vel.y <= 0 && prevY >= top - 0.15 && e.pos.y <= top) {
        e.pos.y = top; e.vel.y = 0; e.onGround = true; e.standingOn = s;
      } else if (e.pos.y < top - 0.15 && e.pos.y + e.h > bot) {
        const ox = s.hw + e.r - Math.abs(dx), oz = s.hd + e.r - Math.abs(dz);
        if (ox > 0 && oz > 0) {
          if (ox < oz) {
            e.pos.x += Math.sign(dx || 1) * ox;
            if (s.pusher) { e.vel.x = s.vel.x * 1.4 + Math.sign(dx || 1) * 1.5; e.knock = 0.35; if (e.isPlayer && Math.abs(s.vel.x) > 0.5) hurt(e); } else e.vel.x = 0;
          } else { e.pos.z += Math.sign(dz || 1) * oz; e.vel.z = 0; }
        }
      }
    } else {
      const d = Math.hypot(dx, dz);
      if (d <= s.r && e.vel.y <= 0 && prevY >= top - 0.15 && e.pos.y <= top) {
        e.pos.y = top; e.vel.y = 0; e.onGround = true; e.standingOn = s;
      } else if (e.pos.y < top - 0.15 && e.pos.y + e.h > bot && d < s.r + e.r && d > 0.0001) {
        const push = s.r + e.r - d; e.pos.x += dx / d * push; e.pos.z += dz / d * push;
      }
    }
  }
  const so = e.standingOn;
  if (so && so.bounce && !e.holding) { e.vel.y = BOUNCE; e.onGround = false; if (e.isPlayer) { sfx('bounce'); clearHands(); } }
  if (so && so.tile && show.stage !== 'intro') shakeTile(so.idx, true);
  if (e.isPlayer && e.onGround && !wasGround && !so?.bounce) sfx('tap');
  if (so && e.onGround && !so.move && !so.tile && !so.bounce && !so.spec) e.lastSafe.copy(e.pos);
  for (const sw of TR.sweepers) hitSweeper(e, sw);
  for (const b of TR.balls) hitBall(e, b);
  e.knock = Math.max(0, e.knock - dt);
}
function hitSweeper(e, sw) {
  if (!sw.active) return;
  if (!(e.pos.y < sw.y + 0.15 && e.pos.y + e.h > sw.y - 0.25)) return;
  const rx = e.pos.x - sw.x, rz = e.pos.z - sw.z;
  if (Math.hypot(rx, rz) > sw.L + 0.6) return;
  for (let k = 0; k < sw.bars; k++) {
    const a = sw.a + k * Math.PI / sw.bars, bx = Math.cos(a), bz = -Math.sin(a);
    let s = rx * bx + rz * bz; s = Math.max(-sw.L, Math.min(sw.L, s));
    const cx = sw.x + bx * s, cz = sw.z + bz * s;
    let nx = e.pos.x - cx, nz = e.pos.z - cz; const d = Math.hypot(nx, nz), min = e.r + 0.25;
    if (d < min) {
      if (d < 1e-4) { nx = -bz; nz = bx; } else { nx /= d; nz /= d; }
      e.pos.x = cx + nx * min; e.pos.z = cz + nz * min;
      const w = sw.curOmega || sw.omega * S();
      const qx = cx - sw.x, qz = cz - sw.z;
      e.vel.x = qz * w * 1.2 + nx * 3; e.vel.z = -qx * w * 1.2 + nz * 3; e.vel.y = 4;
      e.knock = 0.5; e.onGround = false; e.standingOn = null;
      hurt(e);
    }
  }
}
function hitBall(e, b) {
  if (b.r < 0.3) return;
  const dx = e.pos.x - b.pos.x, dz = e.pos.z - b.pos.z, d = Math.hypot(dx, dz);
  if (d < b.r + e.r && e.pos.y < b.pos.y + b.r && e.pos.y + e.h > b.pos.y - b.r && d > 1e-4) {
    const nx = dx / d, nz = dz / d;
    e.pos.x = b.pos.x + nx * (b.r + e.r); e.pos.z = b.pos.z + nz * (b.r + e.r);
    e.vel.set(nx * 5, 5, nz * 5 + 6 * S()); e.knock = 0.6; e.onGround = false; e.standingOn = null;
    hurt(e);
  }
}
function steer(e, tx, tz, dt) {
  const ice = e.standingOn && e.standingOn.ice;
  const k = e.knock > 0 ? 1 : (e.onGround ? (ice ? 1.3 : 10) : 2.5);
  const f = Math.min(1, k * dt), m = e.speedMul || 1;
  e.vel.x += (tx * m - e.vel.x) * f; e.vel.z += (tz * m - e.vel.z) * f;
}
function pushApart(a, bx, by, bz) {
  const dx = bx - a.pos.x, dz = bz - a.pos.z, d = Math.hypot(dx, dz);
  if (d < 0.6 && d > 1e-4 && Math.abs(a.pos.y - by) < 1) {
    const p = (0.6 - d) / 2, nx = dx / d, nz = dz / d;
    a.pos.x -= nx * p; a.pos.z -= nz * p; return [nx * p, nz * p];
  }
  return null;
}

// =====================================================================
// KÄTEGA LIIKUMINE (gorilla-stiil) + kõrguse kalibreerimine
// =====================================================================
let headStand = 1.5, groundAdj = 0;
try { groundAdj = parseFloat(localStorage.getItem('kp_ground') || '0') || 0; } catch (e) {}
function armOffset() { return loco === 'arms' ? Math.min(0.95, Math.max(0.25, headStand * 0.44 + groundAdj)) : 0; }
const HAND_R = 0.07;
const hands = [0, 1].map(() => ({ touch: false, n: new V3(), solid: null, prev: new V3(), has: false }));
const flingBuf = []; let wasHolding = false;
const hp = new V3(), mv = new V3(), hm = new V3(), hl = new V3(), tv = new V3();
function contactAt(p) {
  let best = null;
  for (const s of TR.solids) {
    if (!s.active) continue;
    const c = s.mesh.position, dx = p.x - c.x, dy = p.y - c.y, dz = p.z - c.z;
    if (s.type === 'box') {
      const ex = s.hw + HAND_R - Math.abs(dx), ey = s.hh + HAND_R - Math.abs(dy), ez = s.hd + HAND_R - Math.abs(dz);
      if (ex > 0 && ey > 0 && ez > 0) {
        let n, d;
        if (dy > 0 && ey < 0.35) { n = new V3(0, 1, 0); d = ey; }
        else if (ex < ez && ex < ey) { n = new V3(Math.sign(dx) || 1, 0, 0); d = ex; }
        else if (ez < ey) { n = new V3(0, 0, Math.sign(dz) || 1); d = ez; }
        else { n = new V3(0, Math.sign(dy) || 1, 0); d = ey; }
        if (!best || d < best.d) best = { s, n, d };
      }
    } else {
      const r = Math.hypot(dx, dz), er = s.r + HAND_R - r, ey = s.hh + HAND_R - Math.abs(dy);
      if (er > 0 && ey > 0) {
        let n, d;
        if (dy > 0 && ey < 0.35) { n = new V3(0, 1, 0); d = ey; }
        else if (er < ey) { n = new V3(dx / (r || 1), 0, dz / (r || 1)); d = er; }
        else { n = new V3(0, Math.sign(dy) || 1, 0); d = ey; }
        if (!best || d < best.d) best = { s, n, d };
      }
    }
  }
  return best;
}
function buzz(i, strength = 0.35, ms = 25) {
  try {
    const src = renderer.xr.getSession().inputSources[i];
    const a = src && src.gamepad && src.gamepad.hapticActuators && src.gamepad.hapticActuators[0];
    if (a) a.pulse(strength, ms);
  } catch (e) {}
}
function clearHands() { hands.forEach(h => { h.touch = false; h.solid = null; h.has = false; }); flingBuf.length = 0; wasHolding = false; player.holding = false; }
function armLocomotion(dt) {
  rig.updateMatrixWorld(true);
  mv.set(0, 0, 0); let cnt = 0;
  const t = now();
  grips.forEach((g, i) => {
    const h = hands[i];
    hl.copy(g.position);
    if (!h.has) { h.prev.copy(hl); h.has = true; }
    hm.copy(hl).sub(h.prev).applyQuaternion(rig.quaternion);
    h.prev.copy(hl);
    if (grabHold[i]) { h.touch = false; return; }        // see käsi hoiab kedagi kinni
    g.getWorldPosition(hp);
    if (h.touch) {
      if (h.solid && !h.solid.active) h.touch = false;
      else if (hm.dot(h.n) > 0.0008) h.touch = false;
      else {
        mv.sub(hm);
        if (h.solid) mv.add(h.solid.delta);
        const c = contactAt(hp);
        if (c && c.s === h.solid) mv.addScaledVector(c.n, c.d);
        cnt++;
      }
    }
    if (!h.touch) {
      const c = contactAt(hp);
      if (c) {
        h.touch = true; h.n.copy(c.n); h.solid = c.s; mv.addScaledVector(c.n, c.d); cnt++; buzz(i);
        if (c.s.bounce) { player.vel.set(0, BOUNCE, 0); h.touch = false; cnt--; sfx('bounce'); }
      }
    }
  });
  const holding = cnt > 0;
  if (holding) {
    mv.divideScalar(cnt);
    if (mv.length() > 0.25) mv.setLength(0.25);
    player.pos.add(mv);
    flingBuf.push({ t, v: mv.clone().divideScalar(dt) });
    while (flingBuf.length && t - flingBuf[0].t > 0.1) flingBuf.shift();
    player.vel.set(0, 0, 0);
  } else if (wasHolding) {
    tv.set(0, 0, 0); flingBuf.forEach(f => tv.add(f.v));
    if (flingBuf.length) tv.divideScalar(flingBuf.length);
    if (tv.length() > 0.5) { tv.multiplyScalar(1.3); if (tv.length() > 10) tv.setLength(10); player.vel.copy(tv); }
    flingBuf.length = 0;
  } else if (player.onGround) {
    const ice = player.standingOn && player.standingOn.ice;
    const f = Math.exp(-(ice ? 0.6 : 5) * dt); player.vel.x *= f; player.vel.z *= f;
  }
  player.holding = holding; wasHolding = holding;
}

// =====================================================================
// HAARAMINE ja EMOTIKONID
// =====================================================================
const grabHold = [null, null];   // VR käte haardesihtmärgid
let pcGrab = null, grabSendT = 0;
const EMOTES = ['Tere! 👋', 'Haha 😂', 'Vau! 😮', 'GG 👍'];
let emoteCd = 0;
function doEmote(i) {
  if (emoteCd > 0) return; emoteCd = 1.2;
  sfx('pop'); showToast('Sina: ' + EMOTES[i], 1.5);
  if (net.on && net.peers.size) net.emote.send({ e: i });
}
function bodyCenter(pos, out) { return out.set(pos.x, pos.y + 0.8, pos.z); }
function allOthers() {
  const list = [];
  if (net.isHost) bots.forEach(b => { if (b.visible) list.push({ id: b.id, pos: b.pos }); });
  else remoteBots.forEach(rb => { if (rb.mesh.visible) list.push({ id: rb.id, pos: rb.pos }); });
  for (const p of net.peers.values()) if (p.d && p.avatar.visible) list.push({ id: p.id, pos: p.pos });
  return list;
}
function posOf(id) {
  if (id === net.id) return player.pos;
  if (id.startsWith('#')) {
    if (net.isHost) { const b = bots.find(b => b.id === id); return b ? b.pos : null; }
    const rb = remoteBots.find(r => r.id === id); return rb ? rb.pos : null;
  }
  const p = net.peers.get(id); return p && p.d ? p.pos : null;
}
function nameOf(id) {
  if (id === net.id) return myName;
  if (id.startsWith('#')) return 'Robot ' + (parseInt(id.slice(2)) + 1);
  const p = net.peers.get(id); return p ? p.name : '?';
}
function applyGrab(targetId, byId, on) {
  const until = on ? now() + 0.6 : 0;
  if (targetId === net.id) { player.grabbedBy = on ? { by: byId, until } : null; if (on && !player._grabMsg) { showToast(nameOf(byId) + ' haaras sind!', 1.2); player._grabMsg = 1; } if (!on) player._grabMsg = 0; return; }
  if (net.isHost) { const b = bots.find(b => b.id === targetId); if (b) b.grabbedBy = on ? { by: byId, until } : null; }
}
function sendGrab(targetId, on) {
  if (targetId === net.id) return;
  const isLocalBot = net.isHost && targetId.startsWith('#');
  if (isLocalBot) applyGrab(targetId, net.id, on);
  else if (net.on && net.peers.size) net.grab.send({ t: targetId, g: net.id, on: on ? 1 : 0 });
}
function grabEffect(e, dt) {
  e.speedMul = 1;
  if (!e.grabbedBy) return;
  if (now() > e.grabbedBy.until) { e.grabbedBy = null; return; }
  const gp = posOf(e.grabbedBy.by); if (!gp) return;
  e.speedMul = 0.35;
  const dx = gp.x - e.pos.x, dz = gp.z - e.pos.z, d = Math.hypot(dx, dz);
  if (d > 1.1 && d < 4) { const m = Math.min(d - 1.1, 2.5 * dt); e.pos.x += dx / d * m; e.pos.z += dz / d * m; }
}
const tmpA = new V3(), tmpB = new V3();
function updateGrabs(dt, buttons) {
  grabSendT -= dt;
  const resend = grabSendT <= 0; if (resend) grabSendT = 0.25;
  const others = allOthers();
  if (renderer.xr.isPresenting) {
    for (let i = 0; i < 2; i++) {
      const pressed = buttons[i];
      grips[i].getWorldPosition(tmpA);
      if (pressed && !grabHold[i]) {
        let best = null, bd = 0.55;
        for (const o of others) { const d = bodyCenter(o.pos, tmpB).distanceTo(tmpA); if (d < bd) { bd = d; best = o; } }
        if (best) { grabHold[i] = best.id; sfx('grab'); buzz(i, 0.6, 40); sendGrab(best.id, true); }
      } else if (grabHold[i]) {
        const tp = posOf(grabHold[i]);
        if (!pressed || !tp || bodyCenter(tp, tmpB).distanceTo(tmpA) > 1.8) { sendGrab(grabHold[i], false); grabHold[i] = null; }
        else if (resend) sendGrab(grabHold[i], true);
      }
    }
  } else {
    const pressed = !!keys.KeyE || touchIn.grab;
    if (pressed && !pcGrab) {
      camera.getWorldDirection(fw); fw.y = 0; fw.normalize();
      let best = null, bd = 1.6;
      for (const o of others) {
        const dx = o.pos.x - player.pos.x, dz = o.pos.z - player.pos.z, d = Math.hypot(dx, dz);
        if (d < bd && Math.abs(o.pos.y - player.pos.y) < 1.2 && (dx * fw.x + dz * fw.z) / (d || 1) > 0.2) { bd = d; best = o; }
      }
      if (best) { pcGrab = best.id; sfx('grab'); sendGrab(best.id, true); }
    } else if (pcGrab) {
      const tp = posOf(pcGrab);
      if (!pressed || !tp || tp.distanceTo(player.pos) > 2.2) { sendGrab(pcGrab, false); pcGrab = null; }
      else if (resend) sendGrab(pcGrab, true);
    }
  }
}
// lehvitamise tuvastus
const wave = [0, 1].map(() => ({ lastX: 0, dir: 0, flips: [] }));
function detectWave(dt) {
  if (!renderer.xr.isPresenting) return;
  for (let i = 0; i < 2; i++) {
    const g = grips[i], w = wave[i];
    if (g.position.y < camera.position.y - 0.02) { w.flips.length = 0; w.lastX = g.position.x; continue; }
    const vx = (g.position.x - w.lastX) / dt; w.lastX = g.position.x;
    if (Math.abs(vx) > 0.7) { const d = Math.sign(vx); if (d !== w.dir) { w.dir = d; w.flips.push(now()); } }
    while (w.flips.length && now() - w.flips[0] > 1.2) w.flips.shift();
    if (w.flips.length >= 4) { w.flips.length = 0; doEmote(0); }
  }
}

// =====================================================================
// ROBOTI MÕISTUS
// =====================================================================
function wpTarget(b, wp) {
  if (wp.plat) return { x: wp.plat.mesh.position.x, z: wp.plat.mesh.position.z };
  if (wp.liftOn) return { x: wp.liftOn.mesh.position.x, z: wp.liftOn.mesh.position.z };
  if (wp.side) return { x: b.side * wp.side, z: wp.z };
  if (wp.gap) {
    const wx = wp.gap.mesh.position.x, hw = wp.gap.hw;
    const left = (wx - hw) - (-5), right = 5 - (wx + hw);
    return { x: left > right ? (-5 + wx - hw) / 2 : (wx + hw + 5) / 2, z: wp.z };
  }
  return { x: wp.x + (wp.wide ? b.jit * wp.wide : 0), z: wp.z };
}
function botJumpRules(b, dx, dz, dist, dt) {
  if (!b.onGround || dist < 0.01) return;
  const gx = b.pos.x + dx / dist * 0.7, gz = b.pos.z + dz / dist * 0.7;
  if (groundTopAt(gx, gz, b.pos.y) < b.pos.y - 0.5) b.vel.y = JUMP;
  else if (stepAhead(b.pos.x + dx / dist * 0.55, b.pos.z + dz / dist * 0.55, b.pos.y)) b.vel.y = JUMP;
  for (const sw of TR.sweepers) {
    if (!sw.active) continue;
    const rx = b.pos.x - sw.x, rz = b.pos.z - sw.z;
    if (Math.hypot(rx, rz) > sw.L + 0.5 || Math.abs(b.pos.y - sw.baseTop) > 0.3) continue;
    const w = sw.curOmega || sw.omega * S();
    const r = Math.hypot(rx, rz);
    if (Math.abs(w) < 0.05 || r < 0.5) continue;
    const phi = Math.atan2(-rz, rx);               // sama nurgasüsteem mis latil
    for (let k = 0; k < sw.bars * 2; k++) {
      const a = sw.a + k * Math.PI / sw.bars;
      let diff = w > 0 ? phi - a : a - phi;
      diff = ((diff % (Math.PI * 2)) + Math.PI * 2) % (Math.PI * 2);
      const tHit = diff / Math.abs(w);             // millal latt kohale jõuab
      const half = (0.55 / r) / Math.abs(w);
      if (half < 0.25 && tHit > 0.14 + half && tHit < 0.45 && Math.random() < 0.35 + 0.4 * b.skill) b.vel.y = JUMP;
    }
  }
}
function goTo(b, x, z, dt, speed) {
  const dx = x - b.pos.x, dz = z - b.pos.z, dist = Math.hypot(dx, dz);
  if (dist > 0.15) steer(b, dx / dist * speed, dz / dist * speed, dt); else steer(b, 0, 0, dt);
  botJumpRules(b, dx, dz, dist, dt);
  return dist;
}
function raceAI(b, dt) {
  if (b.finished) { steer(b, 0, 0, dt); if (b.onGround && Math.random() < dt * 0.8) b.vel.y = JUMP * 0.8; return; }
  const wp = TR.wps[Math.min(b.wi, TR.wps.length - 1)], p = wpTarget(b, wp);
  let dx = p.x - b.pos.x, dz = p.z - b.pos.z; const dist = Math.hypot(dx, dz);
  let waiting = false;
  if (wp.plat) {
    if (b.standingOn === wp.plat) { b.wi++; return; }
    waiting = b.onGround && dist < 2.5 && Math.abs(dx) > 1;
  } else if (wp.liftOn) {
    if (b.standingOn === wp.liftOn) { b.wi++; return; }
    const lt = wp.liftOn.mesh.position.y + wp.liftOn.hh;
    waiting = b.onGround && dist < 2.8 && !(lt > b.pos.y - 1.0 && lt < b.pos.y + 0.25);
  } else if (wp.liftOff) {
    const lt = wp.liftOff.mesh.position.y + wp.liftOff.hh;
    if (b.standingOn === wp.liftOff && lt < wp.top - 0.25) waiting = true;
    if (b.standingOn !== wp.liftOff && b.onGround && b.pos.y > wp.top - 0.3 && (dist < 1.2 || b.pos.z < p.z)) b.wi++;
  } else if (dist < 1.2 || b.pos.z < p.z + (wp.wide ? 0.6 : -0.3)) b.wi = Math.min(b.wi + 1, TR.wps.length - 1);
  if (waiting) { steer(b, 0, 0, dt); return; }
  if (TR.balls.length) {           // põikle veerevatest pallidest kõrvale
    const danger = x => { let m = 99; for (const bl of TR.balls) { if (bl.r < 0.3 || bl.pos.z > b.pos.z + 0.5 || b.pos.z - bl.pos.z > 8) continue; if (Math.abs(bl.pos.x - x) < bl.r + 0.6) m = Math.min(m, b.pos.z - bl.pos.z); } return m; };
    if (danger(p.x) < 8) {
      let bx = p.x, best = danger(p.x);
      for (const c of [-4.4, -1.3, 0, 1.3, 4.4]) { const d = danger(c) - Math.abs(c - b.pos.x) * 0.3; if (d > best) { best = d; bx = c; } }
      dx = bx - b.pos.x; dz = (p.z - b.pos.z) * 0.4;
    }
  }
  const dd = Math.hypot(dx, dz) || 1;
  steer(b, dx / dd * b.speed, dz / dd * b.speed, dt);
  botJumpRules(b, dx, dz, dist, dt);
}
function areenAI(b, dt) {
  b.aiT -= dt;
  if (b.aiT <= 0) { b.aiT = rnd(1.5, 3); const a = rnd(0, Math.PI * 2), r = rnd(4.5, 7.5); b.aiTarget.set(Math.cos(a) * r, 0, -12 + Math.sin(a) * r); }
  goTo(b, b.aiTarget.x, b.aiTarget.z, dt, b.speed * 0.6);
}
function tilesAI(b, dt) {
  b.aiT -= dt;
  const cur = b.standingOn;
  if (b.aiT <= 0 || (cur && cur.tile && cur.state !== 'idle')) {
    b.aiT = rnd(0.6, 1.4);
    let best = null, bd = 1e9;
    for (const s of TR.tiles) {
      if (s.state !== 'idle' || Math.abs(s.layerTop - b.pos.y) > 0.5) continue;
      const d = Math.hypot(s.home.x - b.pos.x, s.home.z - b.pos.z);
      const score = Math.abs(d - 2.2) + Math.random() * 1.5 + Math.hypot(s.home.x, s.home.z + 12) * 0.15;
      if (d > 0.8 && score < bd) { bd = score; best = s; }
    }
    if (best) b.aiTarget.set(best.home.x, 0, best.home.z);
  }
  // plaadisaarel ära hüppa üle serva: hüppa ainult üle augu, kui siht-plaat on alles
  const dx = b.aiTarget.x - b.pos.x, dz = b.aiTarget.z - b.pos.z, dist = Math.hypot(dx, dz);
  if (dist > 0.2) steer(b, dx / dist * b.speed * 0.75, dz / dist * b.speed * 0.75, dt); else steer(b, 0, 0, dt);
  if (b.onGround && dist > 0.9) {
    const gx = b.pos.x + dx / dist * 0.7, gz = b.pos.z + dz / dist * 0.7;
    if (groundTopAt(gx, gz, b.pos.y) < b.pos.y - 0.5 && groundTopAt(b.aiTarget.x, b.aiTarget.z, b.pos.y) > b.pos.y - 0.3) b.vel.y = JUMP;
  }
}
function sabaAI(b, dt) {
  const has = show.tails.includes(b.id);
  let best = null, bd = 1e9;
  for (const id of show.part) {
    if (id === b.id) continue;
    const otherHas = show.tails.includes(id);
    if (has === otherHas && has) continue;
    if (!has && !otherHas) continue;
    const p = posOf(id); if (!p) continue;
    const d = p.distanceTo(b.pos); if (d < bd) { bd = d; best = p; }
  }
  let tx = 0, tz = -12;
  if (best) {
    if (has) { const dx = b.pos.x - best.x, dz = b.pos.z - best.z, d = Math.hypot(dx, dz) || 1; tx = b.pos.x + dx / d * 4; tz = b.pos.z + dz / d * 4; }
    else { tx = best.x; tz = best.z; }
  }
  tx = Math.max(-7.5, Math.min(7.5, tx)); tz = Math.max(-19.5, Math.min(-4.5, tz));
  goTo(b, tx, tz, dt, b.speed * (has ? 0.85 : 0.95));
}
function lobbyAI(b, dt) {
  b.aiT -= dt;
  if (b.aiT <= 0) { b.aiT = rnd(2, 5); b.aiTarget.set(rnd(-6.5, 6.5), 0, rnd(-6.5, 6.5)); if (Math.random() < 0.15 && b.mesh) setBubble(b.mesh, EMOTES[Math.floor(Math.random() * 4)]); }
  goTo(b, b.aiTarget.x, b.aiTarget.z, dt, 2);
}
function botAI(b, dt) {
  if (show.stage === 'lobby') return lobbyAI(b, dt);
  if (show.stage === 'intro' || !b.visible) return steer(b, 0, 0, dt);
  if (show.rt === 'race') return raceAI(b, dt);
  if (show.rt === 'areen') return areenAI(b, dt);
  if (show.rt === 'plaadid') return tilesAI(b, dt);
  if (show.rt === 'saba') return sabaAI(b, dt);
}
function botGrabs(dt) {   // robotid haaravad vahel kinni (mitte lihtsas režiimis)
  if (show.easy || show.stage !== 'play') return;
  for (const b of bots) {
    if (!b.visible) continue;
    if (b.grabbing) {
      if (T() > b.grabbing.until) { sendFromBot(b.grabbing.t, b.id, false); b.grabbing = null; }
      else if (Math.random() < dt * 4) sendFromBot(b.grabbing.t, b.id, true);
      continue;
    }
    if (Math.random() > dt * 0.25) continue;
    for (const o of [{ id: net.id, pos: player.pos }, ...allOthers()]) {
      if (o.id === b.id) continue;
      if (o.id === net.id && spectating) continue;
      if (o.pos.distanceTo(b.pos) < 0.8) { b.grabbing = { t: o.id, until: T() + 1.2 }; sendFromBot(o.id, b.id, true); break; }
    }
  }
}
function sendFromBot(targetId, botId, on) {
  if (targetId === net.id || targetId.startsWith('#')) applyGrab(targetId, botId, on);
  else if (net.on && net.peers.size) net.grab.send({ t: targetId, g: botId, on: on ? 1 : 0 });
}

// =====================================================================
// VÕRK (Trystero, WebRTC – sama wifi)
// =====================================================================
function act(room, name) {
  const a = room.makeAction(name);
  if (Array.isArray(a)) return { send: d => a[0](d), on: cb => a[1]((d, id) => cb(d, id)) };
  return { send: d => a.send(d), on: cb => { a.onMessage = (d, meta) => cb(d, meta && typeof meta === 'object' ? meta.peerId : meta); } };
}
function onRoom(room, ev, cb) { if (typeof room[ev] === 'function') room[ev](cb); else room[ev] = cb; }
async function startOnline(code) {
  setStatus('Ühendan ruumiga ' + code + ' …');
  let mod;
  try { mod = await import(TRYSTERO_URL); }
  catch (e) { setStatus('Võrguteeki ei saanud laadida. Ava mäng GitHub Pages lingilt ja kontrolli internetti.'); return false; }
  net.id = mod.selfId; net.on = true;
  const room = mod.joinRoom({ appId: 'kukerpall-vr-emil-v2' }, 'ruum-' + code);
  net.room = room;
  for (const n of ['pose', 'host', 'tile', 'fin', 'req', 'out', 'grab', 'emote']) net[n] = act(room, n);
  onRoom(room, 'onPeerJoin', id => { ensurePeer(id); recomputeHost(true); });
  onRoom(room, 'onPeerLeave', id => { removePeer(id); recomputeHost(false); });
  net.pose.on((d, id) => { const p = ensurePeer(id); if (p.name !== d.n || p.color !== d.c || p.hat !== d.h) rebuildPeerAvatar(p, d); p.d = d; p.last = now(); });
  net.host.on((d, id) => { if (id === net.hostId && !net.isHost) onHostState(d); });
  net.tile.on(d => { if (d.r === show.rid) shakeTile(d.i, false); });
  net.fin.on((d, id) => { if (net.isHost && d.r === show.rid) recordFinish(id, d.t); });
  net.out.on((d, id) => { if (net.isHost && d.r === show.rid) hostOut(id); });
  net.req.on((d, id) => { if (!net.isHost) return; if (d.k === 'fall') hostOnly.falls[id] = (hostOnly.falls[id] || 0) + 1; });
  net.grab.on((d, id) => applyGrab(d.t, d.g || id, !!d.on));
  net.emote.on((d, id) => { const p = net.peers.get(id); if (p && p.avatar) setBubble(p.avatar, EMOTES[d.e] || '👋'); });
  recomputeHost(false);
  return true;
}
function ensurePeer(id) {
  let p = net.peers.get(id);
  if (!p) {
    p = { id, name: '…', color: 0xaaaaaa, hat: 0, d: null, last: now(), avatar: makeHuman(0xaaaaaa, '…', 0), gl: makeGlove(0x5a3e2b), gr: makeGlove(0x5a3e2b), pos: new V3(0, -50, 0) };
    scene.add(p.gl); scene.add(p.gr); p.gl.visible = p.gr.visible = false; p.avatar.visible = false;
    net.peers.set(id, p);
  }
  return p;
}
function rebuildPeerAvatar(p, d) {
  scene.remove(p.avatar); p.name = d.n; p.color = d.c; p.hat = d.h;
  p.avatar = makeHuman(d.c, d.n, d.h); p.avatar.visible = false;
}
function removePeer(id) {
  const p = net.peers.get(id); if (!p) return;
  scene.remove(p.avatar); scene.remove(p.gl); scene.remove(p.gr); net.peers.delete(id);
}
function humanIds() { return [net.id, ...[...net.peers.values()].filter(p => p.d).map(p => p.id)].sort(); }
function recomputeHost(joined) {
  const ids = [net.id, ...net.peers.keys()].sort();
  const was = net.isHost;
  net.hostId = ids[0]; net.isHost = net.hostId === net.id;
  if (net.isHost && !was) { clearRemoteBots(); show.easy = wantEasy; enterLobby(); }
  if (!net.isHost) { bots.forEach(b => scene.remove(b.mesh)); bots = []; net.hasOffset = false; }
  updateHud();
}

// --- PEREMEES: show juhtimine ---
const newId = () => Math.random().toString(36).slice(2, 8);
function makeBots(n) {
  bots.forEach(b => scene.remove(b.mesh)); bots = [];
  for (let i = 0; i < n; i++) {
    const b = makeEnt(false); b.mesh = makeBot(BOT_COLORS[i % BOT_COLORS.length], botHat(i)); b.id = '#b' + i;
    b.side = Math.random() < 0.5 ? -1 : 1; b.jit = Math.random() * 2 - 1;
    b.speed = (3.3 + Math.random() * 0.9) * (show.easy ? 0.8 : 1); b.skill = 0.6 + Math.random() * 0.6;
    bots.push(b);
  }
}
function enterLobby() {
  show.stage = 'lobby'; show.rt = 'lobby'; show.ti = 0; show.rid = newId(); show.part = []; show.alive = []; show.inR = []; show.q = []; show.fo = []; show.tails = []; show.lastOut = [];
  show.easy = wantEasy;
  const n = Math.max(3, 10 - humanIds().length);
  if (bots.length !== n) makeBots(n);
  bots.forEach((b, i) => { b.visible = true; b.mesh.visible = true; b.pos.set(rnd(-6, 6), 0.3, rnd(-6, 6)); b.vel.set(0, 0, 0); b.finished = false; b.grabbing = null; b.grabbedBy = null; });
  hostOnly.padT = 0;
}
function startShow() {
  show.sid = newId(); show.rn = 0; show.winner = ''; hostOnly.falls = {}; hostOnly.prevType = '';
  show.alive = [...humanIds(), ...bots.map(b => b.id)];
  nextRound();
}
function pickRound() {
  const n = show.alive.length, rn = show.rn;
  if (n <= 3 || rn >= 4) return { rt: Math.random() < 0.5 ? 'plaadid' : 'areen', final: true };
  let options;
  if (rn === 1) options = ['race'];
  else if (rn === 2) options = ['race', 'race', 'saba'];
  else options = ['areen', 'saba', 'plaadid'];
  options = options.filter(o => o !== hostOnly.prevType || o === 'race');
  return { rt: options[Math.floor(Math.random() * options.length)], final: false };
}
function nextRound() {
  show.rn++;
  const { rt, final } = pickRound();
  hostOnly.prevType = rt;
  show.rt = rt; show.final = final; show.ti = rt === 'race' ? Math.floor(Math.random() * TRACKS.length) : 0;
  show.rid = newId(); show.stage = 'intro'; show.sa = T() + 6; show.ea = 0;
  show.part = show.alive.slice(); show.inR = show.alive.slice(); show.q = []; show.fo = []; show.lastOut = [];
  const n = show.part.length;
  show.quota = final ? 1 : rt === 'race' ? Math.max(1, Math.ceil(n * 0.65)) : rt === 'saba' ? Math.max(1, Math.ceil(n * 0.5)) : Math.max(1, Math.ceil(n * 0.6));
  show.tails = [];
  if (rt === 'saba') {
    const sh = show.part.slice().sort(() => Math.random() - 0.5);
    show.tails = sh.slice(0, show.quota);
  }
  hostOnly.immune = {};
}
function recordFinish(id, t) {
  if (show.stage !== 'play' || show.rt !== 'race') return;
  if (show.q.includes(id)) return;
  show.q.push(id); show.fo.push([id, Math.round(t * 10) / 10]);
}
function hostOut(id) {
  if (show.stage !== 'play' || !SURVIVAL[show.rt]) return;
  const i = show.inR.indexOf(id); if (i < 0) return;
  show.inR.splice(i, 1);
}
const isHuman = id => !id.startsWith('#');
function endRound() {
  let next;
  const P = show.part;
  if (show.rt === 'race') next = P.filter(id => show.q.includes(id) || (show.easy && isHuman(id)));
  else if (show.rt === 'saba') next = P.filter(id => show.tails.includes(id) || (show.easy && isHuman(id)));
  else next = show.inR.slice();
  if (!next.length) next = P.slice(0, 1);
  if (show.final) {
    let w = next[0];
    if (next.length > 1) {
      const hs = next.filter(isHuman);
      if (hs.length) w = hs.sort((a, b) => (hostOnly.falls[a] || 0) - (hostOnly.falls[b] || 0))[0];
    }
    crownStage(w); return;
  }
  show.lastOut = P.filter(id => !next.includes(id));
  show.alive = next; show.stage = 'results'; show.ea = T();
}
function crownStage(w) {
  show.winner = w; show.stage = 'crown'; show.ea = T();
  const nm = nameOf(w); show.crowns[nm] = (show.crowns[nm] || 0) + 1;
}
function hostTick(dt) {
  const t = T();
  if (show.stage === 'lobby') {
    // alustada saab ainult juht (esimene mängija): nupp, Enter või ise ringile astumine
    const pad = TR.pad ? TR.pad.mesh.position : null;
    const onPad = pad && Math.hypot(player.pos.x - pad.x, player.pos.z - pad.z) < 1.8 && player.pos.y < 1.5;
    show.ready = onPad ? 1 : 0;
    hostOnly.padT = onPad ? hostOnly.padT + dt : 0;
    if (hostOnly.padT > 1.5) startShow();
    return;
  }
  if (show.stage === 'intro' && t > show.sa) show.stage = 'play';
  if (show.stage === 'play') {
    const tau = t - show.sa;
    if (show.rt === 'race') {
      const hs = show.part.filter(isHuman);
      const humansDone = hs.length > 0 && hs.every(id => show.q.includes(id));
      if (humansDone && !hostOnly.doneAt) hostOnly.doneAt = t;
      if (!humansDone) hostOnly.doneAt = 0;
      if (show.q.length >= show.quota || tau > (show.easy ? 200 : 150) || (hostOnly.doneAt && t > hostOnly.doneAt + (show.easy ? 3 : 8))) { hostOnly.doneAt = 0; endRound(); }
    }
    else if (show.rt === 'saba') {
      if (tau > 45) endRound();
      else tailSteals(dt);
    } else {
      const bleft = show.inR.filter(id => !isHuman(id)).length;
      const onlyHumansEasy = show.easy && bleft === 0;
      if (show.final) { if (show.inR.length <= 1 || onlyHumansEasy || tau > 150) endRound(); }
      else if (show.inR.length <= show.quota || onlyHumansEasy || tau > 70) endRound();
    }
  }
  if (show.stage === 'results' && t > show.ea + 6.5) { if (show.alive.length <= 1) crownStage(show.alive[0]); else nextRound(); }
  if (show.stage === 'crown' && t > show.ea + 9) enterLobby();
}
function tailSteals() {
  const t = T();
  const P = show.part.map(id => ({ id, p: posOf(id) })).filter(o => o.p);
  for (const a of P) {
    if (show.tails.includes(a.id) || (hostOnly.immune[a.id] || 0) > t) continue;
    for (const b of P) {
      if (!show.tails.includes(b.id) || (hostOnly.immune[b.id] || 0) > t) continue;
      if (a.p.distanceTo(b.p) < 0.9) {
        show.tails.splice(show.tails.indexOf(b.id), 1); show.tails.push(a.id);
        hostOnly.immune[a.id] = t + 2; hostOnly.immune[b.id] = t + 1;
        break;
      }
    }
  }
}
let hostSendT = 0;
const r2 = v => Math.round(v * 100) / 100;
function hostBroadcast(dt) {
  if (!net.on || net.peers.size === 0) return;
  hostSendT -= dt; if (hostSendT > 0) return; hostSendT = 1 / 15;
  net.host.send({
    ht: now(), sh: show,
    b: bots.map(b => [r2(b.pos.x), r2(b.pos.y), r2(b.pos.z), r2(b.mesh.rotation.y), r2(Math.hypot(b.vel.x, b.vel.z)), b.visible ? 1 : 0]),
  });
}
// --- KÜLALINE ---
function onHostState(d) {
  const est = d.ht - now();
  if (!net.hasOffset) { net.offset = est; net.hasOffset = true; } else net.offset += (est - net.offset) * 0.05;
  const newRid = d.sh.rid !== show.rid;
  Object.assign(show, d.sh);
  if (newRid) loadForShow();
  while (remoteBots.length < d.b.length) {
    const i = remoteBots.length;
    remoteBots.push({ id: '#b' + i, mesh: makeBot(BOT_COLORS[i % BOT_COLORS.length], botHat(i)), pos: new V3(0, -50, 0), target: new V3(), yaw: 0, sp: 0, st: {} });
  }
  while (remoteBots.length > d.b.length) scene.remove(remoteBots.pop().mesh);
  d.b.forEach((a, i) => {
    const rb = remoteBots[i]; rb.target.set(a[0], a[1], a[2]); rb.yaw = a[3]; rb.sp = a[4]; rb.mesh.visible = !!a[5];
    if (rb.pos.y < -40 || rb.pos.distanceTo(rb.target) > 4) rb.pos.copy(rb.target);
  });
}
function clearRemoteBots() { remoteBots.forEach(rb => scene.remove(rb.mesh)); remoteBots = []; }

// --- KÕIK: kaardi laadimine ja enda paigutus ---
let loadedRid = '';
function loadForShow() {
  loadedRid = show.rid;
  buildMap(show.rt, show.ti);
  placeSelf();
  if (net.isHost) placeBots();
  lastCd = -1; hideMsg();
}
function participating() {
  if (show.stage === 'lobby') return true;
  if (!show.part.includes(net.id)) return false;
  if (SURVIVAL[show.rt] && iAmOut) return false;
  return true;
}
function placeSelf() {
  clearHands(); player.vel.set(0, 0, 0); player.cp = 0; player.finished = false; player.knock = 0; player.standingOn = null;
  iAmOut = false; myFinishTime = 0; player.grabbedBy = null;
  if (show.stage === 'lobby') { const i = Math.max(0, humanIds().indexOf(net.id)); TR.spawn(i, 10, player.pos); spectating = false; }
  else if (show.part.includes(net.id)) { TR.spawn(show.part.indexOf(net.id), show.part.length, player.pos); spectating = false; }
  else { player.pos.set(rnd(-3, 3), 8.6, 16); spectating = true; }
  player.lastSafe.copy(player.pos);
  rig.rotation.y = 0; pitch = 0; camera.rotation.x = 0;
  if (SURVIVAL[show.rt] || show.rt === 'saba') {   // vaata keskele
    const dx = TR.center.x - player.pos.x, dz = TR.center.z - player.pos.z;
    rig.rotation.y = Math.atan2(-dx, -dz);
  }
}
function placeBots() {
  bots.forEach((b, i) => {
    b.vel.set(0, 0, 0); b.finished = false; b.cp = 0; b.wi = 0; b.knock = 0; b.grabbing = null; b.grabbedBy = null; b.aiT = 0; b.standingOn = null;
    if (show.stage === 'lobby') { b.visible = true; b.pos.set(rnd(-6, 6), 0.3, rnd(-6, 6)); }
    else if (show.part.includes(b.id)) { b.visible = true; TR.spawn(show.part.indexOf(b.id), show.part.length, b.pos); }
    else { b.visible = false; b.pos.set(0, -100, 0); }
    b.mesh.visible = b.visible; b.lastSafe.copy(b.pos);
  });
}
function respawn(e) {
  e.vel.set(0, 0, 0); e.knock = 0; e.standingOn = null;
  if (e.isPlayer) clearHands();
  if (e.isPlayer && spectating) { e.pos.set(rnd(-3, 3), 8.6, 16); return; }
  if (show.rt === 'lobby') { e.pos.set(rnd(-3, 3), 0.5, rnd(0, 3)); return; }
  if (show.rt === 'race') {
    if (show.easy && e.isPlayer) { e.pos.copy(e.lastSafe); e.pos.y += 0.5; return; }
    const c = TR.cps[e.cp].pos;
    e.pos.set(c.x + (Math.random() - .5) * 3, c.y + 0.5, c.z + (Math.random() - .5) * 2);
    if (!e.isPlayer) { e.wi = TR.wps.findIndex(w => wpZ(w) < c.z - 0.5); if (e.wi < 0) e.wi = TR.wps.length - 1; }
    return;
  }
  if (show.rt === 'plaadid') {
    let best = null; for (const s of TR.tiles) if (s.state === 'idle' && (!best || s.layerTop > best.layerTop || (s.layerTop === best.layerTop && Math.random() < 0.2))) best = s;
    if (best) e.pos.set(best.home.x, best.layerTop + 1, best.home.z); else e.pos.set(0, -7, -12);
    return;
  }
  TR.spawn(Math.floor(Math.random() * 10), 10, e.pos); e.pos.y += 0.5;
}
// enda kukkumine
function onPlayerFall() {
  const surv = SURVIVAL[show.rt] && show.stage === 'play' && participating();
  if (surv && !show.easy) {
    iAmOut = true; spectating = true; sfx('out'); showMsg('Väljas!', ['Sa kukkusid maha', 'Vaata teisi pealt'], 2.5);
    if (net.isHost) hostOut(net.id); else net.out.send({ r: show.rid });
    respawn(player); return;
  }
  if (surv && show.easy) { if (net.isHost) hostOnly.falls[net.id] = (hostOnly.falls[net.id] || 0) + 1; else net.req.send({ k: 'fall' }); }
  sfx('plop'); showMsg('Plärts!', ['Proovi uuesti'], 1.2);
  respawn(player);
}
let poseT = 0;
const headW = new V3(), lW = new V3(), rW = new V3(), fw = new V3(), rt = new V3();
function sendPose(dt) {
  if (!net.on || net.peers.size === 0) return;
  poseT -= dt; if (poseT > 0) return; poseT = 1 / 20;
  camera.getWorldPosition(headW); camera.getWorldDirection(fw);
  const d = { n: myName, c: myColor, h: myHat, p: [r2(player.pos.x), r2(player.pos.y), r2(player.pos.z)], y: r2(Math.atan2(fw.x, fw.z)),
    hd: [r2(headW.x), r2(headW.y), r2(headW.z)], v: renderer.xr.isPresenting ? 1 : 0, s: spectating ? 1 : 0 };
  if (renderer.xr.isPresenting) {
    grips[0].getWorldPosition(lW); grips[1].getWorldPosition(rW);
    d.l = [r2(lW.x), r2(lW.y), r2(lW.z)]; d.rr = [r2(rW.x), r2(rW.y), r2(rW.z)];
  }
  net.pose.send(d);
}
function updatePeers(dt) {
  const k = 1 - Math.exp(-15 * dt);
  for (const p of net.peers.values()) {
    const d = p.d; if (!d) continue;
    p.avatar.visible = true;
    tmpA.set(d.p[0], d.p[1], d.p[2]);
    if (p.pos.y < -40 || p.pos.distanceTo(tmpA) > 5) p.pos.copy(tmpA); else p.pos.lerp(tmpA, k);
    p.avatar.position.copy(p.pos);
    let diff = d.y - p.avatar.rotation.y; diff = ((diff + Math.PI) % (Math.PI * 2) + Math.PI * 2) % (Math.PI * 2) - Math.PI;
    p.avatar.rotation.y += diff * k;
    p.gl.visible = p.gr.visible = !!(d.v && d.l);
    if (d.v && d.l) { p.gl.position.lerp(tmpB.set(d.l[0], d.l[1], d.l[2]), k); p.gr.position.lerp(tmpB.set(d.rr[0], d.rr[1], d.rr[2]), k); }
    tickExtras(p.avatar, p.id, dt);
  }
  for (const rb of remoteBots) {
    rb.pos.lerp(rb.target, k);
    rb.mesh.position.copy(rb.pos); rb.mesh.rotation.y = rb.yaw;
    rb.st.walk = (rb.st.walk || 0) + rb.sp * dt * 3;
    rb.mesh.rotation.z = Math.sin(rb.st.walk) * 0.08 * Math.min(1, rb.sp);
    tickExtras(rb.mesh, rb.id, dt);
  }
}

// =====================================================================
// SISEND
// =====================================================================
const keys = {};
addEventListener('keydown', e => {
  if (document.activeElement && document.activeElement.tagName === 'INPUT') return;
  keys[e.code] = true;
  if (!started) return;
  if (e.code === 'KeyR' && net.isHost) enterLobby();
  if (e.code === 'Enter' && show.stage === 'lobby' && net.isHost) startShow();
  if (e.code === 'KeyM') setMusic(!musicOn);
  const em = ['Digit1', 'Digit2', 'Digit3', 'Digit4'].indexOf(e.code); if (em >= 0) doEmote(em);
});
addEventListener('keyup', e => { keys[e.code] = false; });
const isTouch = ('ontouchstart' in window) || (window.matchMedia && matchMedia('(pointer: coarse)').matches);
renderer.domElement.addEventListener('click', () => { audioInit(); if (started && !renderer.xr.isPresenting && !isTouch) renderer.domElement.requestPointerLock(); });
// ---------- telefoni juhtimine ----------
const touchIn = { mx: 0, my: 0, jump: false, grab: false };
(function setupTouch() {
  const stick = document.getElementById('stick'), knob = document.getElementById('knob');
  let stickId = null, lookId = null, lx = 0, ly = 0;
  const R = 55;
  const setKnob = (x, y) => { knob.style.left = (45 + x) + 'px'; knob.style.top = (45 + y) + 'px'; };
  stick.addEventListener('pointerdown', e => { stickId = e.pointerId; stick.setPointerCapture(e.pointerId); moveStick(e); audioInit(); e.preventDefault(); });
  stick.addEventListener('pointermove', e => { if (e.pointerId === stickId) moveStick(e); });
  const endStick = e => { if (e.pointerId !== stickId) return; stickId = null; touchIn.mx = touchIn.my = 0; setKnob(0, 0); };
  stick.addEventListener('pointerup', endStick); stick.addEventListener('pointercancel', endStick);
  function moveStick(e) {
    const r = stick.getBoundingClientRect();
    let x = e.clientX - (r.left + r.width / 2), y = e.clientY - (r.top + r.height / 2);
    const d = Math.hypot(x, y); if (d > R) { x *= R / d; y *= R / d; }
    setKnob(x, y); touchIn.mx = x / R; touchIn.my = -y / R;
  }
  const cvs = renderer.domElement;
  cvs.addEventListener('pointerdown', e => { if (e.pointerType === 'mouse' || lookId !== null) return; lookId = e.pointerId; lx = e.clientX; ly = e.clientY; });
  cvs.addEventListener('pointermove', e => {
    if (e.pointerId !== lookId) return;
    rig.rotation.y -= (e.clientX - lx) * 0.006;
    pitch = Math.max(-1.2, Math.min(1.2, pitch - (e.clientY - ly) * 0.005)); camera.rotation.x = pitch;
    lx = e.clientX; ly = e.clientY;
  });
  const endLook = e => { if (e.pointerId === lookId) lookId = null; };
  cvs.addEventListener('pointerup', endLook); cvs.addEventListener('pointercancel', endLook);
  const hold = (id, key) => {
    const b = document.getElementById(id);
    b.addEventListener('pointerdown', e => { touchIn[key] = true; audioInit(); e.preventDefault(); });
    const up = () => { touchIn[key] = false; };
    b.addEventListener('pointerup', up); b.addEventListener('pointercancel', up); b.addEventListener('pointerleave', up);
  };
  hold('tJump', 'jump'); hold('tGrab', 'grab');
  let emoteI = 0;
  document.getElementById('tEmote').addEventListener('pointerdown', e => { e.preventDefault(); if (started) doEmote(emoteI++ % EMOTES.length); });
})();
document.getElementById('startBtn').onclick = () => { audioInit(); if (started && net.isHost && show.stage === 'lobby') startShow(); };
let pitch = 0;
addEventListener('mousemove', e => {
  if (document.pointerLockElement !== renderer.domElement) return;
  rig.rotation.y -= e.movementX * 0.0025;
  pitch = Math.max(-1.3, Math.min(1.3, pitch - e.movementY * 0.0025)); camera.rotation.x = pitch;
});
let jumpWas = false, turnLatch = false, adjLatch = false, bHold = 0;
const btnWas = {};
const mvIn = new V3();
function edge(key, val) { const r = val && !btnWas[key]; btnWas[key] = val; return r; }
function handleInput(dt, canMove) {
  let mx = 0, my = 0, rx = 0, ry = 0, jump = false, bDown = false;
  const grip = [false, false];
  const session = renderer.xr.getSession();
  if (session) {
    let idx = 0;
    for (const src of session.inputSources) {
      const gp = src.gamepad; if (!gp) { idx++; continue; }
      const ax = gp.axes.length >= 4 ? gp.axes[2] : (gp.axes[0] || 0), ay = gp.axes.length >= 4 ? gp.axes[3] : (gp.axes[1] || 0);
      const left = src.handedness === 'left';
      if (left) { mx = ax; my = -ay; } else { rx = ax; ry = -ay; }
      const b = gp.buttons;
      if (b[4] && b[4].pressed) jump = true;
      const hi = left ? 0 : 1;
      if (b[1] && b[1].pressed) grip[hi] = true;
      if (edge((left ? 'L' : 'R') + '3', !!(b[3] && b[3].pressed))) doEmote(left ? 1 : 2);
      if (left && edge('Y', !!(b[5] && b[5].pressed))) doEmote(3);
      if (!left && b[5] && b[5].pressed) bDown = true;
      idx++;
    }
  }
  // grip-ide järjekord: grips[0]/[1] vastavad inputSources järjekorrale; kasutame käelisust
  const gripByIndex = [false, false];
  if (session) { let i = 0; for (const src of session.inputSources) { if (i < 2 && src.gamepad) gripByIndex[i] = grip[src.handedness === 'left' ? 0 : 1]; i++; } }
  if (bDown) { bHold += dt; if (bHold > 1.5 && net.isHost && show.stage !== 'lobby') { enterLobby(); bHold = -99; } } else bHold = 0;

  if (keys.KeyW || keys.ArrowUp) my += 1; if (keys.KeyS || keys.ArrowDown) my -= 1;
  if (keys.KeyD || keys.ArrowRight) mx += 1; if (keys.KeyA || keys.ArrowLeft) mx -= 1;
  if (keys.Space || touchIn.jump) jump = true;
  if (!session && (touchIn.mx || touchIn.my)) { mx += touchIn.mx; my += touchIn.my; }
  if (Math.abs(mx) < 0.15) mx = 0; if (Math.abs(my) < 0.15) my = 0;
  if (Math.abs(rx) > 0.7 && !turnLatch && Math.abs(rx) > Math.abs(ry)) { rig.rotation.y -= Math.sign(rx) * Math.PI / 6; turnLatch = true; }
  if (Math.abs(rx) < 0.3) turnLatch = false;
  if (session && loco === 'arms' && Math.abs(ry) > 0.7 && !adjLatch && Math.abs(ry) > Math.abs(rx)) {
    groundAdj = Math.max(-0.4, Math.min(0.4, groundAdj + Math.sign(ry) * 0.05)); adjLatch = true;
    try { localStorage.setItem('kp_ground', String(groundAdj)); } catch (e) {}
    showToast('Maapind ' + (groundAdj >= 0 ? '+' : '') + Math.round(groundAdj * 100) + ' cm', 1.2); clearHands();
  }
  if (Math.abs(ry) < 0.3) adjLatch = false;
  updateGrabs(dt, gripByIndex);
  detectWave(dt);

  camera.getWorldDirection(fw); fw.y = 0; fw.normalize(); rt.set(-fw.z, 0, fw.x);
  mvIn.set(0, 0, 0).addScaledVector(fw, my).addScaledVector(rt, mx); if (mvIn.length() > 1) mvIn.normalize();
  const arms = session && loco === 'arms';
  if (arms) {
    if (canMove) armLocomotion(dt); else { clearHands(); steer(player, 0, 0, dt); }
  } else {
    player.holding = false;
    steer(player, canMove ? mvIn.x * player.speed : 0, canMove ? mvIn.z * player.speed : 0, dt);
    if (jump && !jumpWas && player.onGround && canMove) { player.vel.y = JUMP; sfx('jump'); }
  }
  jumpWas = jump;
}

// =====================================================================
// MAAILMA LIIKUMINE (jagatud kell → kõigil sama)
// =====================================================================
function updateWorld(dt, force) {
  const t = T(), s = S(), tau = Math.max(0, t - show.sa);
  for (const so of TR.movers) {
    const m = so.move, old = so.mesh.position[m.axis];
    const nv = m.base + Math.sin(t * m.speed * s + m.phase) * m.amp;
    so.mesh.position[m.axis] = nv;
    so.delta.set(0, 0, 0); so.vel.set(0, 0, 0);
    if (m.init && !force && Math.abs(nv - old) < 1) { so.delta[m.axis] = nv - old; so.vel[m.axis] = (nv - old) / Math.max(dt, 1e-3); }
    m.init = true;
  }
  for (const sw of TR.sweepers) {
    const prev = sw.a;
    if (sw.angleFn) sw.a = (show.stage === 'play' || show.stage === 'results' || show.stage === 'crown') ? sw.angleFn(tau) : sw.angleFn(0);
    else sw.a = sw.omega * s * t + sw.phase;
    if (sw.angleFn) sw.curOmega = show.stage === 'play' ? (sw.angleFn(tau + 0.02) - sw.angleFn(tau)) / 0.02 : 0;
    else { sw.curOmega = dt > 0 ? (sw.a - prev) / Math.max(dt, 1e-3) : sw.omega; if (Math.abs(sw.curOmega) > 20) sw.curOmega = sw.omega * s; }
    sw.active = sw.activeFn ? sw.activeFn(tau) && show.stage === 'play' : true;
    sw.group.visible = sw.activeFn ? sw.active : true;
    sw.group.rotation.y = sw.a;
  }
  for (const b of TR.balls) {
    const len = b.zNear - b.zFar;
    const u = (((t * b.speed * s + b.offset) % len) + len) % len;
    const z = b.zFar + u, sc = Math.max(0.01, Math.min(1, u / 1.2, (len - u) / 1.5));
    b.r = b.R * sc; b.pos.set(b.x, b.top + b.r, z);
    b.g.position.copy(b.pos); b.g.scale.setScalar(sc); b.m.rotation.x = z / b.R;
  }
  for (const so of TR.tiles) {
    if (so.state === 'shake') {
      so.t += dt; so.mesh.position.x = so.home.x + Math.sin(so.t * 60) * 0.04;
      if (so.t > (show.easy ? 1.2 : 0.8) + (TR.slowTiles ? 0.4 : 0)) { so.state = 'fall'; so.active = false; so.t = 0; so.mesh.position.x = so.home.x; }
    } else if (so.state === 'fall') {
      so.t += dt; so.mesh.position.y = so.home.y - so.t * so.t * 6;
      if (so.t > 3.5) { if (so.noReturn) { so.state = 'gone'; so.mesh.visible = false; } else { so.state = 'idle'; so.active = true; so.mesh.position.copy(so.home); } }
    }
  }
  trackGroup.children.forEach(o => { if (o.userData.spin) { o.rotation.y += dt; o.position.y = o.userData.baseY + Math.sin(t * 2) * 0.15; } });
}

// =====================================================================
// MENÜÜ
// =====================================================================
const $ = id => document.getElementById(id);
const hud = $('hud');
function setStatus(t) { $('status').textContent = t; }
function updateHud() {
  if (!started) return;
  const vrNow = renderer.xr.isPresenting;
  hud.style.display = vrNow ? 'none' : 'block';
  $('touch').classList.toggle('on', isTouch && !vrNow);
  $('startBtn').style.display = !vrNow && net.isHost && show.stage === 'lobby' ? 'block' : 'none';
  if (isTouch) { hud.innerHTML = '<b>' + (TR ? TR.name : '') + '</b>' + (net.on ? ' · ruum ' + roomCode + ' · ' + humanIds().length + ' inimest' : ''); return; }
  const n = humanIds().length;
  hud.innerHTML = '<b>Kukerpall VR</b> · ' + (TR ? TR.name : '') + '<br>' +
    (net.on ? 'Ruum <b>' + roomCode + '</b> · inimesi ' + n + (net.isHost ? ' · sina juhid' : '') : 'Üksi robotitega') +
    (show.easy ? ' · lihtne' : '') +
    '<br><small>Klõpsa ekraanil · WASD + hiir · tühik · E = haara · 1–4 emotikon · M = muusika' + (show.stage === 'lobby' ? ' · Enter = alusta' : '') + '</small>';
}
// värvid ja mütsid
HUMAN_COLORS.forEach(c => {
  const b = document.createElement('button'); b.className = 'sw' + (c === myColor ? ' on' : ''); b.style.background = '#' + new THREE.Color(c).getHexString();
  b.onclick = () => { myColor = c; document.querySelectorAll('.sw').forEach(x => x.classList.remove('on')); b.classList.add('on'); };
  $('colors').appendChild(b);
});
HATS.forEach((h, i) => {
  const b = document.createElement('button'); b.className = 'hat' + (i === myHat ? ' on' : ''); b.textContent = h;
  b.onclick = () => { myHat = i; document.querySelectorAll('.hat').forEach(x => x.classList.remove('on')); b.classList.add('on'); };
  $('hats').appendChild(b);
});
const params = new URLSearchParams(location.search);
let savedName = ''; try { savedName = localStorage.getItem('kp_name') || ''; } catch (e) {}
$('name').value = savedName;
$('code').value = (params.get('ruum') || randomCode()).toUpperCase();
function randomCode() { const L = 'ABDEFGHKLMNPRSTUV'; let s = ''; for (let i = 0; i < 4; i++) s += L[Math.floor(Math.random() * L.length)]; return s; }
function readMenu() {
  myName = ($('name').value.trim() || 'Mängija').slice(0, 12);
  try { localStorage.setItem('kp_name', myName); } catch (e) {}
  loco = document.querySelector('input[name=loco]:checked').value;
  wantEasy = $('easy').checked; setMusic($('music').checked);
  audioInit();
}
$('solo').onclick = () => { readMenu(); startGame(false); };
$('online').onclick = async () => {
  readMenu();
  roomCode = $('code').value.trim().toUpperCase().replace(/[^A-Z0-9]/g, '') || randomCode();
  $('code').value = roomCode;
  const ok = await startOnline(roomCode);
  if (ok) startGame(true);
};
function startGame(online) {
  started = true; $('menu').style.display = 'none';
  pitch = 0; camera.rotation.set(0, 0, 0);
  if (!online) { net.on = false; net.isHost = true; }
  if (net.isHost) { show.easy = wantEasy; enterLobby(); loadForShow(); }
  updateHud();
}
renderer.xr.addEventListener('sessionstart', () => { if (!started) { readMenu(); startGame(false); } audioInit(); clearHands(); headStand = 1.5; updateHud(); });
renderer.xr.addEventListener('sessionend', updateHud);

// =====================================================================
// MÄNGU TSÜKKEL
// =====================================================================
const clock = new THREE.Clock();
let hudT = 0, lastCd = -1, menuSpin = 0, lastStageKey = '', hadTail = false;
buildMap('race', 0);
player.pos.set(0, 0.2, 1.5);
const headLocal = new V3();

function onStageChange() {
  const st = show.stage;
  if (st === 'results') {
    const me = net.id;
    if (show.part.includes(me)) {
      if (show.alive.includes(me)) { sfx('cheer'); showMsg('Pääsesid edasi!', ['Edasi pääses ' + show.alive.length + '/' + show.part.length], 3); }
      else { sfx('out'); showMsg('Langesid välja', ['Järgmisi voore saad pealt vaadata'], 3); }
    }
  }
  if (st === 'crown') {
    sfx('fanfare');
    const wp = posOf(show.winner); burst(wp || player.pos);
  }
  if (st === 'play' && show.part.includes(net.id)) { sfx('go'); showMsg('MINE!', [], 1); }
}

renderer.setAnimationLoop(() => {
  const dt = Math.min(clock.getDelta(), 0.05);
  const vr = renderer.xr.isPresenting;

  if (!started) {
    updateWorld(dt, false);
    menuSpin += dt * 0.1;
    rig.position.set(Math.sin(menuSpin) * 14, 6, 6 + Math.cos(menuSpin) * 4); rig.rotation.y = Math.sin(menuSpin) * 0.3;
    camera.rotation.x = -0.35;
    renderer.render(scene, camera); return;
  }
  if (show.rid !== loadedRid) loadForShow();
  updateWorld(dt, false);
  emoteCd -= dt;

  const t = T();
  const stageKey = show.rid + show.stage;
  if (stageKey !== lastStageKey) { lastStageKey = stageKey; onStageChange(); }

  // stardiloendus
  if (show.stage === 'intro') {
    const n = Math.ceil(show.sa - t);
    if (n !== lastCd) {
      lastCd = n;
      const nm = mapName(show.rt, show.ti);
      const desc = show.rt === 'race' ? 'Esimesed ' + show.quota + ' pääsevad edasi' : show.final ? 'Viimane püstiseisja võidab krooni!' : ARENAS[show.rt].desc;
      if (n <= 3 && n > 0) { showMsg(String(n), [], 1.1); sfx('beep'); }
      else if (n > 3) showMsg(nm, [(show.final ? 'FINAAL · ' : 'Voor ' + show.rn + ' · ') + show.part.length + ' mängijat', desc, spectating ? 'Sina vaatad pealt' : ''], 1.2);
    }
  }

  // VR: pikkuse kalibreerimine ja päris kõndimine
  if (vr) {
    headStand = Math.max(headStand - dt * 0.01, camera.position.y);
    headLocal.set(camera.position.x, 0, camera.position.z).applyQuaternion(rig.quaternion);
    const hx = rig.position.x + headLocal.x, hz = rig.position.z + headLocal.z;
    if (Math.hypot(hx - player.pos.x, hz - player.pos.z) < 0.5) { player.pos.x = hx; player.pos.z = hz; }
  }

  const canMove = show.stage !== 'intro' || spectating;
  grabEffect(player, dt);
  handleInput(dt, canMove);

  if (net.isHost) {
    for (const b of bots) if (b.visible) { grabEffect(b, dt); botAI(b, dt); }
    botGrabs(dt);
  }
  const off = vr ? armOffset() : 0;
  player.h = vr ? Math.max(0.6, Math.min(1.5, headStand - off)) : 1.5;
  stepEntity(player, dt);
  if (net.isHost) for (const b of bots) if (b.visible) stepEntity(b, dt);

  // lükkamine
  const others = allOthers();
  for (const o of others) pushApart(player, o.pos.x, o.pos.y, o.pos.z);
  if (net.isHost) {
    for (let i = 0; i < bots.length; i++) {
      if (!bots[i].visible) continue;
      for (let j = i + 1; j < bots.length; j++) { if (!bots[j].visible) continue; const m = pushApart(bots[i], bots[j].pos.x, bots[j].pos.y, bots[j].pos.z); if (m) { bots[j].pos.x += m[0]; bots[j].pos.z += m[1]; } }
      pushApart(bots[i], player.pos.x, player.pos.y, player.pos.z);
      for (const p of net.peers.values()) if (p.d) pushApart(bots[i], p.pos.x, p.pos.y, p.pos.z);
    }
  }

  // kukkumine, kontrollpunktid, finiš (enda ja peremehe robotid)
  if (player.pos.y < TR.outY) onPlayerFall();
  if (show.rt === 'race' && !spectating) {
    for (let c = player.cp + 1; c < TR.cps.length; c++) if (player.onGround && player.pos.z < TR.cps[c].trig) { player.cp = c; sfx('ding'); showToast('Kontrollpunkt!', 1.2); }
    if (!player.finished && show.stage === 'play' && player.pos.z < TR.finishZ && player.pos.y > TR.cps[0].pos.y - 1 && show.part.includes(net.id)) {
      player.finished = true; myFinishTime = t - show.sa; burst(player.pos); sfx('cheer');
      if (net.isHost) recordFinish(net.id, myFinishTime); else net.fin.send({ t: myFinishTime, r: show.rid });
    }
  }
  if (net.isHost) {
    for (const b of bots) {
      if (!b.visible) continue;
      if (b.pos.y < TR.outY) {
        if (SURVIVAL[show.rt] && show.stage === 'play') { hostOut(b.id); b.visible = false; b.mesh.visible = false; b.pos.set(0, -100, 0); continue; }
        respawn(b);
      }
      if (show.rt === 'race') {
        for (let c = b.cp + 1; c < TR.cps.length; c++) if (b.onGround && b.pos.z < TR.cps[c].trig) b.cp = c;
        if (!b.finished && show.stage === 'play' && b.pos.z < TR.finishZ) { b.finished = true; recordFinish(b.id, t - show.sa); }
      }
      b.mesh.position.copy(b.pos); animateBody(b.mesh, b.vel, b.onGround, dt, b); tickExtras(b.mesh, b.id, dt);
    }
    hostTick(dt);
    hostBroadcast(dt);
  }
  sendPose(dt);
  updatePeers(dt);
  if (show.stage === 'lobby') drawBoard();

  // saba saamise heli
  const hasTail = show.rt === 'saba' && show.tails.includes(net.id);
  if (show.rt === 'saba' && show.stage === 'play' && hasTail !== hadTail) { if (hasTail) { sfx('tail'); showToast('Said saba! Hoia seda!', 1.5); } else { sfx('hit'); showToast('Su saba võeti ära!', 1.5); } }
  hadTail = hasTail;

  // kaamera keha juurde
  if (vr) {
    headLocal.set(camera.position.x, 0, camera.position.z).applyQuaternion(rig.quaternion);
    rig.position.set(player.pos.x - headLocal.x, player.pos.y - off, player.pos.z - headLocal.z);
  } else {
    rig.position.copy(player.pos); camera.position.set(0, 1.6, 0);
  }

  // konfetid
  for (let i = 0; i < CN; i++) {
    if (cPos[i * 3 + 1] < -50) continue;
    const v = cVel[i]; v.y -= 6 * dt;
    cPos[i * 3] += v.x * dt; cPos[i * 3 + 1] += v.y * dt; cPos[i * 3 + 2] += v.z * dt;
    if (cPos[i * 3 + 1] < -20) cPos[i * 3 + 1] = -100;
  }
  cGeo.attributes.position.needsUpdate = true;

  // HUD
  hudT -= dt;
  if (hudT <= 0) {
    hudT = 0.2;
    const tau = Math.max(0, t - show.sa);
    const clock = Math.floor(tau / 60) + ':' + String(Math.floor(tau % 60)).padStart(2, '0');
    let line = TR.name;
    if (show.stage === 'lobby') {
      line = 'Ootesaal · ' + (net.isHost ? (renderer.xr.isPresenting ? 'Astu rohelisele ringile, et alustada' : 'Vajuta ALUSTA, kui kõik on kohal') : 'Ootame, kuni ' + nameOf(net.hostId) + ' alustab');
    } else if (spectating) {
      line = 'Vaatad pealt · ' + TR.name + (SURVIVAL[show.rt] ? ' · alles ' + show.inR.length : '');
    } else if (show.rt === 'race') {
      const qi = show.q.indexOf(net.id);
      line = TR.name + ' · ' + clock + ' · ' + (qi >= 0 ? (qi + 1) + '. koht ✓' : 'edasi ' + show.q.length + '/' + show.quota);
    } else if (show.rt === 'saba') {
      line = (hasTail ? 'SUL ON SABA! Põgene!' : 'Püüa saba!') + ' · ' + Math.max(0, Math.ceil(45 - tau)) + ' s';
    } else line = TR.name + ' · alles ' + show.inR.length + (show.final ? '' : ' · edasi ' + show.quota) + ' · ' + clock;
    drawStats(line);
    if (show.stage === 'results') {
      showMsg(show.alive.includes(net.id) ? 'Pääsesid edasi!' : show.part.includes(net.id) ? 'Langesid välja' : 'Voor läbi!',
        ['Edasi pääses ' + show.alive.length + '/' + show.part.length, 'Järgmine voor ' + Math.max(0, Math.ceil(show.ea + 6.5 - t)) + ' s pärast'], 1, true);
    } else if (show.stage === 'crown') {
      const nm = nameOf(show.winner);
      showMsg(show.winner === net.id ? 'SINA VÕITSID!' : 'Võitja: ' + nm, ['👑 ' + nm + ' – kroone kokku ' + (show.crowns[nm] || 1), 'Tagasi ootesaali ' + Math.max(0, Math.ceil(show.ea + 9 - t)) + ' s'], 1, true);
    } else if (show.rt === 'race' && player.finished && show.stage === 'play') {
      const qi = show.q.indexOf(net.id);
      showMsg('Finiš!', [(qi >= 0 ? (qi + 1) + '. koht · ' : '') + myFinishTime.toFixed(1) + ' s', 'Pääsesid edasi – ootame teisi…'], 1, true);
    } else if (msgSticky) hideMsg();
    updateHud();
  }
  if (!msgSticky) { msgTimer -= dt; msg.mesh.visible = msgTimer > 0; }
  if (toastT > 0) { toastT -= dt; if (toastT <= 0) toast.mesh.visible = false; }

  renderer.render(scene, camera);
});
</script>
</body>
</html>
