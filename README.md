[Uploading index (40).html…]()
<!doctype html>
<html lang="et">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Kukerpall VR</title>
<style>
:root{--ink:#1d1b3a;--card:rgba(255,255,255,.94);--pink:#ff3d8b;--blue:#2f9bff}
html,body{margin:0;height:100%;overflow:hidden;background:#8fd3ff;font-family:system-ui,sans-serif;color:var(--ink)}
#menu{position:fixed;inset:0;display:flex;align-items:center;justify-content:center;padding:16px;box-sizing:border-box;z-index:10}
#menu .card{background:var(--card);border-radius:22px;padding:22px 24px;max-width:420px;width:100%;box-shadow:0 10px 40px rgba(0,0,0,.18)}
#menu h1{margin:0 0 4px;font-size:30px;color:var(--pink)}
#menu p{margin:4px 0 14px;font-size:14px;line-height:1.45}
#menu label{display:block;font-size:13px;font-weight:700;margin:10px 0 4px}
#menu input[type=text]{width:100%;box-sizing:border-box;font-size:18px;padding:10px 12px;border-radius:12px;border:2px solid #ccd;text-transform:none}
#code{text-transform:uppercase!important;letter-spacing:3px;font-weight:700}
.row{display:flex;gap:8px;flex-wrap:wrap}
.row label{font-weight:500;display:flex;align-items:center;gap:6px;margin:4px 12px 0 0}
button.big{flex:1;min-width:150px;font-size:17px;font-weight:700;padding:13px;border:0;border-radius:14px;color:#fff;cursor:pointer;margin-top:16px}
#solo{background:var(--blue)}#online{background:var(--pink)}
#status{margin-top:12px;font-size:14px;min-height:20px}
#hud{position:fixed;left:12px;top:12px;background:var(--card);padding:8px 12px;border-radius:12px;font-size:13px;line-height:1.45;max-width:340px;display:none;z-index:5}
small{opacity:.75}
</style>
<script type="importmap">{"imports":{"three":"https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.module.js","three/addons/":"https://cdn.jsdelivr.net/npm/three@0.160.0/examples/jsm/"}}</script>
</head>
<body>
<div id="menu"><div class="card">
  <h1>Kukerpall VR</h1>
  <p>Takistusradade võidujooks robotite ja sõpradega. Arvutis ja VR-is saab mängida koos – kõik avavad sama lingi ja panevad sama ruumi koodi.</p>
  <label for="name">Sinu nimi</label>
  <input type="text" id="name" maxlength="12" placeholder="Nimi">
  <label for="code">Ruumi kood (sõpradel sama)</label>
  <input type="text" id="code" maxlength="6">
  <label>VR-is liikumine</label>
  <div class="row">
    <label><input type="radio" name="loco" value="arms" checked> Kätega (ahv)</label>
    <label><input type="radio" name="loco" value="stick"> Pulgaga</label>
  </div>
  <div class="row">
    <button class="big" id="solo">Mängi robotitega</button>
    <button class="big" id="online">Mängi sõpradega</button>
  </div>
  <div id="status"></div>
  <p><small><b>Arvutis:</b> WASD + hiir, tühik = hüppa, R = uus voor.<br>
  <b>VR kätega:</b> lükka kätega vastu maad ja seinu, vehi hooga, et hüpata. Parem pulk = pööra.<br>
  <b>VR pulgaga:</b> vasak pulk = liigu, A/X = hüppa. B/Y = uus voor.</small></p>
</div></div>
<div id="hud"></div>
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

// püsiv taust
const goo = new THREE.Mesh(new THREE.PlaneGeometry(500, 500), mat(0xff5fa2));
goo.rotation.x = -Math.PI / 2; goo.position.y = -6; scene.add(goo);
for (let i = 0; i < 30; i++) {
  const g = new THREE.Group();
  for (let k = 0; k < 3; k++) { const s = new THREE.Mesh(new THREE.SphereGeometry(2 + Math.random() * 2, 10, 8), mat(0xffffff)); s.position.set(k * 2.5, Math.random(), 0); g.add(s); }
  const side = Math.random() < 0.5 ? -1 : 1;
  g.position.set(side * (25 + Math.random() * 40), 8 + Math.random() * 22, 25 - Math.random() * 160); scene.add(g);
}

// =====================================================================
// RADADE EHITAMINE
// =====================================================================
const trackGroup = new THREE.Group(); scene.add(trackGroup);
let TR = null;          // praegune rada
let trackIndex = -1;

function newTR(name) { return { name, solids: [], movers: [], sweepers: [], tiles: [], cps: [], wps: [], finishZ: -90 }; }
function box(x, top, z, w, h, d, color, opts = {}) {
  const m = new THREE.Mesh(new THREE.BoxGeometry(w, h, d), mat(color));
  m.position.set(x, top - h / 2, z); trackGroup.add(m);
  const s = Object.assign({ type: 'box', mesh: m, hw: w / 2, hh: h / 2, hd: d / 2, vel: new V3(), delta: new V3(), active: true }, opts);
  if (s.move) { s.move.base = m.position[s.move.axis]; s.move.init = false; TR.movers.push(s); }
  TR.solids.push(s); return s;
}
function cyl(x, top, z, r, h, color) {
  const m = new THREE.Mesh(new THREE.CylinderGeometry(r, r, h, 40), mat(color));
  m.position.set(x, top - h / 2, z); trackGroup.add(m);
  const s = { type: 'cyl', mesh: m, r, hh: h / 2, vel: new V3(), delta: new V3(), active: true };
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
  const sw = { x, z, y: baseTop + 0.55, baseTop, L, omega, bars, phase, a: 0, group: g };
  TR.sweepers.push(sw); return sw;
}
function tileGrid(x0, z0, cols, rows, top, size = 1.8, step = 1.9) {
  const cols4 = [0xff7eb6, 0x5ec8ff, 0xffd166, 0x7be495];
  for (let r = 0; r < rows; r++) for (let c = 0; c < cols; c++) {
    const x = x0 + c * step, z = z0 - r * step;
    const s = box(x, top, z, size, 0.5, size, cols4[(r + c) % 4], { tile: true, state: 'idle', t: 0, idx: TR.tiles.length });
    s.home = s.mesh.position.clone();
    TR.tiles.push(s);
  }
}
function arch(z, top, color) {
  const g = new THREE.Group();
  [-5, 5].forEach(x => { const p = new THREE.Mesh(new THREE.BoxGeometry(0.5, 4, 0.5), mat(color)); p.position.set(x, 2, 0); g.add(p); });
  const t = new THREE.Mesh(new THREE.BoxGeometry(10.5, 0.8, 0.5), mat(color)); t.position.y = 4.2; g.add(t);
  g.position.set(0, top, z); trackGroup.add(g);
}
function finishDeco(z, top) {
  arch(z, top, 0xff7eb6);
  const line = new THREE.Mesh(new THREE.BoxGeometry(10, 0.02, 0.5), new THREE.MeshBasicMaterial({ color: 0xffffff }));
  line.position.set(0, top + 0.01, z); trackGroup.add(line);
  const crown = new THREE.Group();
  const band = new THREE.Mesh(new THREE.CylinderGeometry(0.6, 0.6, 0.4, 24, 1, true), new THREE.MeshLambertMaterial({ color: 0xffc400, side: THREE.DoubleSide }));
  crown.add(band);
  for (let i = 0; i < 6; i++) { const c = new THREE.Mesh(new THREE.ConeGeometry(0.15, 0.4, 8), mat(0xffc400)); const a = i / 6 * Math.PI * 2; c.position.set(Math.cos(a) * 0.55, 0.4, Math.sin(a) * 0.55); crown.add(c); }
  crown.position.set(0, top + 2.2, z - 5); crown.userData.spin = true; crown.userData.baseY = top + 2.2; trackGroup.add(crown);
}
function cp(trig, x, y, z) { TR.cps.push({ trig, pos: new V3(x, y, z) }); }

const C = { green: 0x7be495, yellow: 0xffd166, pink: 0xff7eb6, blue: 0x5ec8ff, purple: 0xb388ff, red: 0xff5c5c, orange: 0xffa94d };

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
];

function buildTrack(i) {
  trackGroup.traverse(o => { if (o.geometry) o.geometry.dispose(); });
  trackGroup.clear();
  TR = newTR(TRACKS[i].name); trackIndex = i;
  TRACKS[i].build();
  updateWorld(0.016, true);
}
function wpZ(wp) {
  if (wp.plat) return wp.plat.mesh.position.z;
  if (wp.liftOn) return wp.liftOn.mesh.position.z;
  if (wp.gap) return wp.gap.mesh.position.z;
  return wp.z;
}

// =====================================================================
// TEGELASED
// =====================================================================
function makeBot(color) {
  const g = new THREE.Group();
  const body = new THREE.Mesh(new THREE.BoxGeometry(0.6, 0.7, 0.5), mat(color)); body.position.y = 0.53; g.add(body);
  const head = new THREE.Mesh(new THREE.BoxGeometry(0.55, 0.42, 0.48), mat(color)); head.position.y = 1.1; g.add(head);
  const visor = new THREE.Mesh(new THREE.BoxGeometry(0.44, 0.22, 0.02), mat(0x1b1b2f)); visor.position.set(0, 1.12, 0.25); g.add(visor);
  const eyeM = new THREE.MeshBasicMaterial({ color: 0x9ffcff });
  [-0.1, 0.1].forEach(x => { const e = new THREE.Mesh(new THREE.BoxGeometry(0.08, 0.08, 0.02), eyeM); e.position.set(x, 1.13, 0.265); g.add(e); });
  const ant = new THREE.Mesh(new THREE.CylinderGeometry(0.02, 0.02, 0.25), mat(0x333344)); ant.position.y = 1.43; g.add(ant);
  const ball = new THREE.Mesh(new THREE.SphereGeometry(0.06, 10, 8), mat(0xffee55)); ball.position.y = 1.58; g.add(ball);
  [-0.15, 0.15].forEach(x => { const l = new THREE.Mesh(new THREE.BoxGeometry(0.18, 0.18, 0.22), mat(0x333344)); l.position.set(x, 0.09, 0); g.add(l); });
  scene.add(g); return g;
}
function makeHuman(color, name) {
  const g = new THREE.Group();
  const body = new THREE.Mesh(new THREE.CapsuleGeometry(0.3, 0.5, 6, 14), mat(color)); body.position.y = 0.6; g.add(body);
  const head = new THREE.Mesh(new THREE.SphereGeometry(0.27, 16, 12), mat(color)); head.position.y = 1.22; g.add(head);
  const face = new THREE.Mesh(new THREE.SphereGeometry(0.2, 14, 10), mat(0xffe0c2)); face.position.set(0, 1.22, 0.12); face.scale.set(1, 0.85, 0.6); g.add(face);
  const eyeM = mat(0x111122);
  [-0.07, 0.07].forEach(x => { const e = new THREE.Mesh(new THREE.SphereGeometry(0.03, 8, 6), eyeM); e.position.set(x, 1.25, 0.25); g.add(e); });
  const crownM = mat(0xffc400);
  const hat = new THREE.Mesh(new THREE.ConeGeometry(0.16, 0.3, 10), crownM); hat.position.y = 1.58; g.add(hat);
  const tag = nameSprite(name, color); tag.position.y = 2.0; g.add(tag); g.userData.tag = tag;
  g.userData.head = head; g.userData.face = face;
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
function makeGlove(color) {
  const g = new THREE.Group();
  const fist = new THREE.Mesh(new THREE.SphereGeometry(0.07, 14, 10), mat(0x5a3e2b)); fist.scale.set(1, 0.8, 1.2); g.add(fist);
  const palm = new THREE.Mesh(new THREE.SphereGeometry(0.045, 10, 8), mat(color)); palm.position.y = -0.04; g.add(palm);
  return g;
}
function makeEnt(isPlayer) {
  return { isPlayer, pos: new V3(), vel: new V3(), onGround: false, standingOn: null, r: 0.3, h: isPlayer ? 1.5 : 1.4,
    cp: 0, finished: false, knock: 0, mesh: null, wi: 0, side: 1, jit: 0, speed: 4.5, skill: 1, walk: 0, holding: false };
}
const player = makeEnt(true);
const BOT_COLORS = [0xff6b6b, 0x4dabf7, 0xffd43b, 0x69db7c, 0xda77f2, 0xff922b, 0x22b8cf, 0xf783ac];
const HUMAN_COLORS = [0xff3d8b, 0x2f9bff, 0x22c55e, 0xf59e0b, 0x8b5cf6, 0x06b6d4];
let bots = [];            // peremehe juures päris robotid
let remoteBots = [];      // külalise juures roboti pildid
function animateBody(mesh, vel, onGround, dt, stateObj) {
  const sp = Math.hypot(vel.x, vel.z);
  if (sp > 0.3) mesh.rotation.y = Math.atan2(vel.x, vel.z);
  stateObj.walk = (stateObj.walk || 0) + sp * dt * 3;
  mesh.rotation.z = onGround ? Math.sin(stateObj.walk) * 0.08 * Math.min(1, sp) : 0.15;
}

// VR käed
const grips = [];
for (let i = 0; i < 2; i++) {
  const grip = renderer.xr.getControllerGrip(i);
  grip.add(makeGlove(i ? 0xff7eb6 : 0x5ec8ff)); rig.add(grip); grips.push(grip);
}

// =====================================================================
// HUD (VR-is kaamera ees)
// =====================================================================
function makePanel(w, h, cw, ch, pos) {
  const cv = document.createElement('canvas'); cv.width = cw; cv.height = ch;
  const tex = new THREE.CanvasTexture(cv);
  const m = new THREE.Mesh(new THREE.PlaneGeometry(w, h), new THREE.MeshBasicMaterial({ map: tex, transparent: true, depthTest: false }));
  m.renderOrder = 999; m.position.copy(pos); camera.add(m);
  return { cv, ctx: cv.getContext('2d'), tex, mesh: m };
}
const stats = makePanel(0.66, 0.075, 1100, 125, new V3(0, 0.3, -1.1));
const msg = makePanel(0.9, 0.5, 1024, 570, new V3(0, 0.02, -1.2));
function rr(ctx, x, y, w, h, r) { ctx.beginPath(); ctx.roundRect(x, y, w, h, r); ctx.fill(); }
let lastStats = '';
function drawStats(text) {
  if (text === lastStats) return; lastStats = text;
  const { ctx, cv } = stats; ctx.clearRect(0, 0, cv.width, cv.height);
  ctx.fillStyle = 'rgba(20,20,50,0.55)'; rr(ctx, 0, 0, cv.width, cv.height, 40);
  ctx.fillStyle = '#fff'; ctx.font = 'bold 56px system-ui'; ctx.textAlign = 'center'; ctx.textBaseline = 'middle';
  ctx.fillText(text, cv.width / 2, cv.height / 2 + 4); stats.tex.needsUpdate = true;
}
let msgTimer = 0, msgSticky = false, lastMsgKey = '';
function showMsg(title, lines = [], time = 2, sticky = false) {
  const key = title + '|' + lines.join('|');
  msgTimer = time; msgSticky = sticky; msg.mesh.visible = true;
  if (key === lastMsgKey) return; lastMsgKey = key;
  const { ctx, cv } = msg; ctx.clearRect(0, 0, cv.width, cv.height);
  const H = lines.length ? Math.min(cv.height, 200 + lines.length * 62) : 230;
  const y0 = (cv.height - H) / 2;
  ctx.fillStyle = 'rgba(255,255,255,0.92)'; rr(ctx, 0, y0, cv.width, H, 60);
  ctx.textAlign = 'center'; ctx.textBaseline = 'middle';
  ctx.fillStyle = '#ff3d8b'; ctx.font = 'bold 110px system-ui';
  ctx.fillText(title, cv.width / 2, y0 + (lines.length ? 95 : H / 2));
  ctx.fillStyle = '#334'; ctx.font = 'bold 48px system-ui';
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
const G = 18, JUMP = 7;
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
function stepAhead(x, z, y) {   // kas ees on madal aste, mille peale hüpata
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
  if (broadcast && net.on) net.tile.send({ i, r: round.id });
}
function stepEntity(e, dt) {
  if (e.standingOn && e.standingOn.active && !e.holding) e.pos.add(e.standingOn.delta);
  if (!e.holding) e.vel.y -= G * dt;
  const prevY = e.pos.y;
  e.pos.addScaledVector(e.vel, dt);
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
            if (s.pusher) { e.vel.x = s.vel.x * 1.4 + Math.sign(dx || 1) * 1.5; e.knock = 0.35; } else e.vel.x = 0;
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
  if (e.standingOn && e.standingOn.tile) shakeTile(e.standingOn.idx, true);
  for (const sw of TR.sweepers) hitSweeper(e, sw);
  e.knock = Math.max(0, e.knock - dt);
}
function hitSweeper(e, sw) {
  if (!(e.pos.y < sw.y + 0.25 && e.pos.y + e.h > sw.y - 0.25)) return;
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
      const qx = cx - sw.x, qz = cz - sw.z;
      e.vel.x = qz * sw.omega * 1.2 + nx * 3; e.vel.z = -qx * sw.omega * 1.2 + nz * 3; e.vel.y = 4;
      e.knock = 0.5; e.onGround = false; e.standingOn = null;
      if (e.isPlayer) { clearHands(); buzz(0, 0.8, 80); buzz(1, 0.8, 80); }
    }
  }
}
function steer(e, tx, tz, dt) {
  const k = e.knock > 0 ? 1 : (e.onGround ? 10 : 2.5);
  const f = Math.min(1, k * dt);
  e.vel.x += (tx - e.vel.x) * f; e.vel.z += (tz - e.vel.z) * f;
}
function respawn(e) {
  const c = TR.cps[e.cp].pos;
  e.pos.set(c.x + (Math.random() - .5) * 3, c.y + 0.5, c.z + (Math.random() - .5) * 2);
  e.vel.set(0, 0, 0); e.knock = 0;
  if (!e.isPlayer) { e.wi = TR.wps.findIndex(w => wpZ(w) < c.z - 0.5); if (e.wi < 0) e.wi = TR.wps.length - 1; }
  if (e.isPlayer) clearHands();
}
function pushApart(a, bx, by, bz, moveB) {   // a = olem; b = positsioon
  const dx = bx - a.pos.x, dz = bz - a.pos.z, d = Math.hypot(dx, dz);
  if (d < 0.6 && d > 1e-4 && Math.abs(a.pos.y - by) < 1) {
    const p = (0.6 - d) / (moveB ? 2 : 1), nx = dx / d, nz = dz / d;
    a.pos.x -= nx * p; a.pos.z -= nz * p;
    return [nx * p, nz * p];
  }
  return null;
}

// =====================================================================
// KÄTEGA LIIKUMINE (gorilla-stiil)
// =====================================================================
const ARM_OFFSET = 0.55;   // keha madalamal, et käed ulatuksid maani
const HAND_R = 0.05;
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
    hl.copy(g.position);                       // käe asend rigi sees
    if (!h.has) { h.prev.copy(hl); h.has = true; }
    hm.copy(hl).sub(h.prev).applyQuaternion(rig.quaternion);   // käe päris liikumine maailmas
    h.prev.copy(hl);
    g.getWorldPosition(hp);
    if (h.touch) {
      if (h.solid && !h.solid.active) h.touch = false;
      else if (hm.dot(h.n) > 0.0008) h.touch = false;       // käsi tõstetakse pinnalt ära → lase lahti
      else {
        mv.sub(hm);                                          // keha liigub käele vastupidi
        if (h.solid) mv.add(h.solid.delta);
        const c = contactAt(hp);                             // hoia käsi pinna peal
        if (c && c.s === h.solid) mv.addScaledVector(c.n, c.d);
        cnt++;
      }
    }
    if (!h.touch) {
      const c = contactAt(hp);
      if (c) { h.touch = true; h.n.copy(c.n); h.solid = c.s; mv.addScaledVector(c.n, c.d); cnt++; buzz(i); }
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
    const f = Math.exp(-5 * dt); player.vel.x *= f; player.vel.z *= f;
  }
  player.holding = holding; wasHolding = holding;
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
function botAI(b, dt) {
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
    if (b.standingOn !== wp.liftOff && b.onGround && b.pos.y > wp.top - 0.3 && (dist < 1.2 || b.pos.z < p.z)) { b.wi++; }
  } else if (dist < 1 || b.pos.z < p.z - 0.3) { b.wi = Math.min(b.wi + 1, TR.wps.length - 1); }
  let tx = 0, tz = 0;
  if (!waiting && dist > 0.01) { tx = dx / dist * b.speed; tz = dz / dist * b.speed; }
  steer(b, tx, tz, dt);
  if (b.onGround && !waiting && dist > 0.01) {
    const gx = b.pos.x + dx / dist * 0.7, gz = b.pos.z + dz / dist * 0.7;
    if (groundTopAt(gx, gz, b.pos.y) < b.pos.y - 0.5) b.vel.y = JUMP;
    else if (stepAhead(b.pos.x + dx / dist * 0.55, b.pos.z + dz / dist * 0.55, b.pos.y)) b.vel.y = JUMP;
    for (const sw of TR.sweepers) {
      const rx = b.pos.x - sw.x, rz = b.pos.z - sw.z;
      if (Math.hypot(rx, rz) > sw.L + 0.5 || Math.abs(b.pos.y - sw.baseTop) > 0.3) continue;
      for (let k = 0; k < sw.bars; k++) {
        const a = sw.a + k * Math.PI / sw.bars, bx = Math.cos(a), bz = -Math.sin(a);
        if (Math.abs(rx * -bz + rz * bx) < 1.4 && Math.random() < dt * 7 * b.skill) b.vel.y = JUMP;
      }
    }
  }
}

// =====================================================================
// VÕRK (Trystero, WebRTC – sama wifi)
// =====================================================================
const net = { on: false, id: 'mina', room: null, isHost: true, hostId: 'mina', offset: 0, hasOffset: false, peers: new Map() };
const T = () => net.isHost ? now() : now() + net.offset;
let myName = 'Mängija', myColor = HUMAN_COLORS[Math.floor(Math.random() * HUMAN_COLORS.length)], loco = 'arms', roomCode = '';
let round = { id: '', track: 0, startAt: 0, endAt: 0, nb: 8, firstHumanAt: 0 };
let finishOrder = [];     // [[id, nimi, aeg]]
let myFinishTime = 0;

function act(room, name) {
  const a = room.makeAction(name);
  if (Array.isArray(a)) return { send: d => a[0](d), on: cb => a[1]((d, id) => cb(d, id)) };
  return { send: d => a.send(d), on: cb => { a.onMessage = (d, meta) => cb(d, meta && typeof meta === 'object' ? meta.peerId : meta); } };
}
function onRoom(room, ev, cb) {
  if (typeof room[ev] === 'function') room[ev](cb); else room[ev] = cb;
}
async function startOnline(code) {
  setStatus('Ühendan ruumiga ' + code + ' …');
  let mod;
  try { mod = await import(TRYSTERO_URL); }
  catch (e) { setStatus('Võrguteeki ei saanud laadida. Ava mäng GitHub Pages lingilt ja kontrolli internetti.'); return false; }
  net.id = mod.selfId; net.on = true;
  const room = mod.joinRoom({ appId: 'kukerpall-vr-emil-v1' }, 'ruum-' + code);
  net.room = room;
  net.pose = act(room, 'pose'); net.host = act(room, 'host'); net.tile = act(room, 'tile');
  net.fin = act(room, 'fin'); net.req = act(room, 'req');
  onRoom(room, 'onPeerJoin', id => { ensurePeer(id); recomputeHost(true); });
  onRoom(room, 'onPeerLeave', id => { removePeer(id); recomputeHost(false); });
  net.pose.on((d, id) => { const p = ensurePeer(id); p.d = d; p.last = now(); if (p.name !== d.n || p.color !== d.c) rebuildPeerAvatar(p, d); });
  net.host.on((d, id) => { if (id === net.hostId && !net.isHost) onHostState(d); });
  net.tile.on(d => { if (d.r === round.id) shakeTile(d.i, false); });
  net.fin.on((d, id) => { if (net.isHost) { const p = net.peers.get(id); recordFinish(id, p ? p.name : '?', d.t, d.r); } });
  net.req.on(d => { if (net.isHost && d.k === 'restart') newRound(true); });
  recomputeHost(false);
  return true;
}
function ensurePeer(id) {
  let p = net.peers.get(id);
  if (!p) {
    p = { id, name: '…', color: 0xaaaaaa, d: null, last: now(), avatar: makeHuman(0xaaaaaa, '…'), gl: makeGlove(0x5a3e2b), gr: makeGlove(0x5a3e2b), pos: new V3(0, -50, 0), head: new V3(), state: {} };
    scene.add(p.gl); scene.add(p.gr); p.gl.visible = p.gr.visible = false; p.avatar.visible = false;
    net.peers.set(id, p);
  }
  return p;
}
function rebuildPeerAvatar(p, d) {
  scene.remove(p.avatar); p.name = d.n; p.color = d.c;
  p.avatar = makeHuman(d.c, d.n); p.avatar.visible = false;
}
function removePeer(id) {
  const p = net.peers.get(id); if (!p) return;
  scene.remove(p.avatar); scene.remove(p.gl); scene.remove(p.gr); net.peers.delete(id);
}
function recomputeHost(joined) {
  const ids = [net.id, ...net.peers.keys()].sort();
  const was = net.isHost;
  net.hostId = ids[0]; net.isHost = net.hostId === net.id;
  if (net.isHost && !was) { clearRemoteBots(); newRound(false); }
  else if (net.isHost && joined) newRound(true);      // uus sõber → alusta voor koos uuesti
  if (!net.isHost) { bots.forEach(b => scene.remove(b.mesh)); bots = []; net.hasOffset = false; }
  updateHud();
}
function humanIds() { return [net.id, ...net.peers.keys()].sort(); }

// --- peremees ---
function newRound(sameTrack) {
  const tr = sameTrack && trackIndex >= 0 ? trackIndex : (trackIndex + 1) % TRACKS.length;
  const humans = net.on ? 1 + net.peers.size : 1;
  round = { id: Math.random().toString(36).slice(2, 8), track: tr, startAt: T() + 4.5, endAt: 0, nb: Math.max(2, 9 - humans), firstHumanAt: 0 };
  applyRound(round);
  bots.forEach(b => scene.remove(b.mesh)); bots = [];
  for (let i = 0; i < round.nb; i++) {
    const b = makeEnt(false); b.mesh = makeBot(BOT_COLORS[i % BOT_COLORS.length]); b.id = '#b' + i; b.name = 'Robot ' + (i + 1);
    b.pos.set(-4 + i * (8 / Math.max(1, round.nb - 1)), 0.2, 3.3);
    b.side = Math.random() < 0.5 ? -1 : 1; b.jit = Math.random() * 2 - 1;
    b.speed = 3.3 + Math.random() * 0.9; b.skill = 0.6 + Math.random() * 0.6;
    bots.push(b);
  }
}
function recordFinish(id, name, t, r) {
  if (r && r !== round.id) return;
  if (finishOrder.some(f => f[0] === id)) return;
  finishOrder.push([id, name, Math.round(t * 10) / 10]);
  if (!id.startsWith('#') && !round.firstHumanAt) round.firstHumanAt = T();
}
function hostTick() {
  const t = T();
  if (!round.endAt && t > round.startAt) {
    const humans = humanIds();
    const doneH = humans.filter(id => finishOrder.some(f => f[0] === id)).length;
    if (doneH >= humans.length) round.endAt = t + 1.5;
    else if (round.firstHumanAt && t > round.firstHumanAt + 30) round.endAt = t;
    else if (t > round.startAt + 180) round.endAt = t;
  }
  if (round.endAt && t > round.endAt + 8) newRound(false);
}
let hostSendT = 0;
function hostBroadcast(dt) {
  if (!net.on || net.peers.size === 0) return;
  hostSendT -= dt; if (hostSendT > 0) return; hostSendT = 1 / 15;
  const r2 = v => Math.round(v * 100) / 100;
  net.host.send({
    ht: now(),
    rd: { id: round.id, tr: round.track, sa: round.startAt, ea: round.endAt, nb: round.nb },
    b: bots.map(b => [r2(b.pos.x), r2(b.pos.y), r2(b.pos.z), r2(b.mesh.rotation.y), b.finished ? 1 : 0, r2(Math.hypot(b.vel.x, b.vel.z))]),
    fo: finishOrder,
  });
}
// --- külaline ---
function onHostState(d) {
  const est = d.ht - now();
  if (!net.hasOffset) { net.offset = est; net.hasOffset = true; } else net.offset += (est - net.offset) * 0.05;
  if (d.rd.id !== round.id) {
    round = { id: d.rd.id, track: d.rd.tr, startAt: d.rd.sa, endAt: d.rd.ea, nb: d.rd.nb, firstHumanAt: 0 };
    applyRound(round);
  }
  round.endAt = d.rd.ea; round.startAt = d.rd.sa;
  finishOrder = d.fo || [];
  while (remoteBots.length < d.b.length) {
    const i = remoteBots.length;
    remoteBots.push({ mesh: makeBot(BOT_COLORS[i % BOT_COLORS.length]), pos: new V3(), target: new V3(), yaw: 0, sp: 0, state: {} });
  }
  while (remoteBots.length > d.b.length) scene.remove(remoteBots.pop().mesh);
  d.b.forEach((a, i) => { const rb = remoteBots[i]; rb.target.set(a[0], a[1], a[2]); rb.yaw = a[3]; rb.sp = a[5]; if (rb.pos.y < -40 || rb.pos.distanceTo(rb.target) > 4) rb.pos.copy(rb.target); });
}
function clearRemoteBots() { remoteBots.forEach(rb => scene.remove(rb.mesh)); remoteBots = []; }

// --- kõik ---
function applyRound(rd) {
  if (rd.track !== trackIndex || !TR) buildTrack(rd.track);
  else { TR.tiles.forEach(s => { s.state = 'idle'; s.active = true; s.mesh.position.copy(s.home); }); }
  finishOrder = []; myFinishTime = 0;
  const ids = humanIds(); const slot = Math.max(0, ids.indexOf(net.id));
  player.cp = 0; player.finished = false; player.knock = 0; player.vel.set(0, 0, 0); player.standingOn = null;
  player.pos.set(-3 + (slot % 4) * 2, 0.2, 1.2 - Math.floor(slot / 4) * 1.2);
  rig.rotation.y = 0; clearHands();
  for (let i = 0; i < CN; i++) cPos[i * 3 + 1] = -100;
  lastCd = -1; hideMsg();
}
let poseT = 0;
const headW = new V3(), lW = new V3(), rW = new V3(), fw = new V3(), rt = new V3();
function sendPose(dt) {
  if (!net.on || net.peers.size === 0) return;
  poseT -= dt; if (poseT > 0) return; poseT = 1 / 20;
  const r2 = v => Math.round(v * 100) / 100;
  camera.getWorldPosition(headW);
  camera.getWorldDirection(fw);
  const d = { n: myName, c: myColor, p: [r2(player.pos.x), r2(player.pos.y), r2(player.pos.z)], y: r2(Math.atan2(fw.x, fw.z)),
    hd: [r2(headW.x), r2(headW.y), r2(headW.z)], f: player.finished ? 1 : 0, v: renderer.xr.isPresenting ? 1 : 0, r: round.id };
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
    const tgt = new V3(d.p[0], d.p[1], d.p[2]);
    if (p.pos.y < -40 || p.pos.distanceTo(tgt) > 5) p.pos.copy(tgt); else p.pos.lerp(tgt, k);
    p.avatar.position.copy(p.pos);
    if (d.v) { p.avatar.position.x += (d.hd[0] - d.p[0]); p.avatar.position.z += (d.hd[2] - d.p[2]); }
    p.avatar.rotation.y += ((((d.y - p.avatar.rotation.y) + Math.PI) % (Math.PI * 2) + Math.PI * 2) % (Math.PI * 2) - Math.PI) * k;
    p.gl.visible = p.gr.visible = !!(d.v && d.l);
    if (d.v && d.l) { p.gl.position.lerp(new V3(...d.l), k); p.gr.position.lerp(new V3(...d.rr), k); }
  }
  for (const rb of remoteBots) {
    rb.pos.lerp(rb.target, k);
    rb.mesh.position.copy(rb.pos); rb.mesh.rotation.y = rb.yaw;
    rb.state.walk = (rb.state.walk || 0) + rb.sp * dt * 3;
    rb.mesh.rotation.z = Math.sin(rb.state.walk) * 0.08 * Math.min(1, rb.sp);
  }
}

// =====================================================================
// SISEND
// =====================================================================
const keys = {};
addEventListener('keydown', e => {
  if (document.activeElement && document.activeElement.tagName === 'INPUT') return;
  keys[e.code] = true; if (e.code === 'KeyR' && started) requestRestart();
});
addEventListener('keyup', e => { keys[e.code] = false; });
renderer.domElement.addEventListener('click', () => { if (started && !renderer.xr.isPresenting) renderer.domElement.requestPointerLock(); });
let pitch = 0;
addEventListener('mousemove', e => {
  if (document.pointerLockElement !== renderer.domElement) return;
  rig.rotation.y -= e.movementX * 0.0025;
  pitch = Math.max(-1.3, Math.min(1.3, pitch - e.movementY * 0.0025)); camera.rotation.x = pitch;
});
function requestRestart() {
  if (net.isHost) newRound(true); else if (net.on) { net.req.send({ k: 'restart' }); showMsg('Palusin uut vooru', [], 1.2); }
}
let jumpWas = false, resetWas = false, turnLatch = false;
const mvIn = new V3();
function handleInput(dt, canMove) {
  let mx = 0, my = 0, rx = 0, jump = false, reset = false;
  const session = renderer.xr.getSession();
  if (session) for (const src of session.inputSources) {
    const gp = src.gamepad; if (!gp) continue;
    const ax = gp.axes.length >= 4 ? gp.axes[2] : (gp.axes[0] || 0), ay = gp.axes.length >= 4 ? gp.axes[3] : (gp.axes[1] || 0);
    if (src.handedness === 'left') { mx = ax; my = -ay; } else rx = ax;
    const b = gp.buttons;
    if (b[4] && b[4].pressed) jump = true;
    if (b[5] && b[5].pressed) reset = true;
  }
  if (keys.KeyW || keys.ArrowUp) my += 1; if (keys.KeyS || keys.ArrowDown) my -= 1;
  if (keys.KeyD || keys.ArrowRight) mx += 1; if (keys.KeyA || keys.ArrowLeft) mx -= 1;
  if (keys.Space) jump = true;
  if (Math.abs(mx) < 0.15) mx = 0; if (Math.abs(my) < 0.15) my = 0;
  if (Math.abs(rx) > 0.7 && !turnLatch) { rig.rotation.y -= Math.sign(rx) * Math.PI / 6; turnLatch = true; }
  if (Math.abs(rx) < 0.3) turnLatch = false;
  if (reset && !resetWas) requestRestart();
  resetWas = reset;
  camera.getWorldDirection(fw); fw.y = 0; fw.normalize(); rt.set(-fw.z, 0, fw.x);
  mvIn.set(0, 0, 0).addScaledVector(fw, my).addScaledVector(rt, mx); if (mvIn.length() > 1) mvIn.normalize();
  const arms = session && loco === 'arms';
  if (arms) {
    if (canMove) armLocomotion(dt); else { clearHands(); steer(player, 0, 0, dt); }
  } else {
    player.holding = false;
    steer(player, canMove ? mvIn.x * player.speed : 0, canMove ? mvIn.z * player.speed : 0, dt);
    if (jump && !jumpWas && player.onGround && canMove) player.vel.y = JUMP;
  }
  jumpWas = jump;
}

// =====================================================================
// MAAILMA LIIKUMINE (jagatud kell → kõigil sama)
// =====================================================================
function updateWorld(dt, force) {
  const t = T();
  for (const s of TR.movers) {
    const m = s.move, old = s.mesh.position[m.axis];
    const nv = m.base + Math.sin(t * m.speed + m.phase) * m.amp;
    s.mesh.position[m.axis] = nv;
    s.delta.set(0, 0, 0); s.vel.set(0, 0, 0);
    if (m.init && !force) { s.delta[m.axis] = nv - old; s.vel[m.axis] = (nv - old) / Math.max(dt, 1e-3); }
    if (Math.abs(nv - old) > 1) s.delta.set(0, 0, 0);
    m.init = true;
  }
  for (const sw of TR.sweepers) { sw.a = sw.omega * t + sw.phase; sw.group.rotation.y = sw.a; }
  for (const s of TR.tiles) {
    if (s.state === 'shake') {
      s.t += dt; s.mesh.position.x = s.home.x + Math.sin(s.t * 60) * 0.04;
      if (s.t > 0.8) { s.state = 'fall'; s.active = false; s.t = 0; s.mesh.position.x = s.home.x; }
    } else if (s.state === 'fall') {
      s.t += dt; s.mesh.position.y = s.home.y - s.t * s.t * 6;
      if (s.t > 3.5) { s.state = 'idle'; s.active = true; s.mesh.position.copy(s.home); }
    }
  }
  trackGroup.children.forEach(o => { if (o.userData.spin) { o.rotation.y += dt; o.position.y = o.userData.baseY + Math.sin(t * 2) * 0.15; } });
}

// =====================================================================
// MENÜÜ
// =====================================================================
const $ = id => document.getElementById(id);
const hud = $('hud');
function setStatus(t) { $('status').textContent = t; hud.innerHTML = t ? '<b>Kukerpall VR</b><br>' + t : ''; }
function updateHud() {
  if (!started) return;
  hud.style.display = 'block';
  const n = net.on ? 1 + net.peers.size : 1;
  hud.innerHTML = '<b>Kukerpall VR</b> · ' + (TR ? TR.name : '') + '<br>' +
    (net.on ? 'Ruum <b>' + roomCode + '</b> · inimesi ' + n + (net.isHost ? ' · sina juhid roboteid' : '') : 'Üksi robotitega') +
    '<br><small>' + (renderer.xr.isPresenting ? '' : 'Klõpsa ekraanil, WASD + hiir, tühik, R = uus voor') + '</small>';
}
const params = new URLSearchParams(location.search);
let savedName = ''; try { savedName = localStorage.getItem('kp_name') || ''; } catch (e) {}
$('name').value = savedName;
$('code').value = (params.get('ruum') || randomCode()).toUpperCase();
function randomCode() { const L = 'ABDEFGHKLMNPRSTUV'; let s = ''; for (let i = 0; i < 4; i++) s += L[Math.floor(Math.random() * L.length)]; return s; }
let started = false;
function readMenu() {
  myName = ($('name').value.trim() || 'Mängija').slice(0, 12);
  try { localStorage.setItem('kp_name', myName); } catch (e) {}
  loco = document.querySelector('input[name=loco]:checked').value;
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
  if (!online || net.isHost) newRound(false);
  updateHud();
}
renderer.xr.addEventListener('sessionstart', () => { if (!started) { readMenu(); startGame(false); } clearHands(); updateHud(); });
renderer.xr.addEventListener('sessionend', updateHud);

// =====================================================================
// MÄNGU TSÜKKEL
// =====================================================================
const clock = new THREE.Clock();
let hudT = 0, lastCd = -1, menuSpin = 0;
buildTrack(0);
player.pos.set(0, 0.2, 1.5);
const headLocal = new V3();

renderer.setAnimationLoop(() => {
  const dt = Math.min(clock.getDelta(), 0.05);
  updateWorld(dt, false);
  const vr = renderer.xr.isPresenting;

  if (!started) {
    menuSpin += dt * 0.1;
    rig.position.set(Math.sin(menuSpin) * 14, 6, 6 + Math.cos(menuSpin) * 4); rig.rotation.y = Math.sin(menuSpin) * 0.3;
    camera.rotation.x = -0.35;
    renderer.render(scene, camera); return;
  }

  const t = T();
  const phase = t < round.startAt ? 'countdown' : (round.endAt && t > round.endAt ? 'results' : 'race');
  const raceTime = Math.max(0, t - round.startAt);

  if (phase === 'countdown') {
    const n = Math.ceil(round.startAt - t);
    if (n !== lastCd) { lastCd = n; showMsg(n <= 3 ? String(n) : TR.name, n <= 3 ? [] : ['Rada ' + (trackIndex + 1) + '/' + TRACKS.length], 1.1); }
  } else if (lastCd > 0 && phase === 'race') { lastCd = 0; showMsg('MINE!', [], 1); }

  // VR: päris kõndimine liigutab keha
  if (vr) {
    headLocal.set(camera.position.x, 0, camera.position.z).applyQuaternion(rig.quaternion);
    const hx = rig.position.x + headLocal.x, hz = rig.position.z + headLocal.z;
    const ddx = hx - player.pos.x, ddz = hz - player.pos.z;
    if (Math.hypot(ddx, ddz) < 0.5) { player.pos.x = hx; player.pos.z = hz; }
  }

  const canMove = phase !== 'countdown';
  handleInput(dt, canMove);

  if (net.isHost) {
    if (phase !== 'countdown') for (const b of bots) botAI(b, dt); else for (const b of bots) steer(b, 0, 0, dt);
  }
  player.h = vr ? 1.05 : 1.5;
  stepEntity(player, dt);
  if (net.isHost) for (const b of bots) stepEntity(b, dt);

  // lükkamine
  const others = [];
  if (net.isHost) bots.forEach(b => others.push(b.pos)); else remoteBots.forEach(rb => others.push(rb.pos));
  for (const p of net.peers.values()) if (p.d) others.push(p.pos);
  for (const o of others) pushApart(player, o.x, o.y, o.z, false);
  if (net.isHost) {
    for (let i = 0; i < bots.length; i++) {
      for (let j = i + 1; j < bots.length; j++) { const m = pushApart(bots[i], bots[j].pos.x, bots[j].pos.y, bots[j].pos.z, true); if (m) { bots[j].pos.x += m[0]; bots[j].pos.z += m[1]; } }
      pushApart(bots[i], player.pos.x, player.pos.y, player.pos.z, false);
      for (const p of net.peers.values()) if (p.d) pushApart(bots[i], p.pos.x, p.pos.y, p.pos.z, false);
    }
  }

  // kukkumine, kontrollpunktid, finiš
  const mine = [player, ...(net.isHost ? bots : [])];
  for (const e of mine) {
    if (e.pos.y < -6) { respawn(e); if (e.isPlayer) showMsg('Plärts!', ['Proovi uuesti'], 1.2); }
    for (let c = e.cp + 1; c < TR.cps.length; c++) if (e.onGround && e.pos.z < TR.cps[c].trig) {
      e.cp = c; if (e.isPlayer) showMsg('Kontrollpunkt!', [], 1.3);
    }
    if (!e.finished && phase === 'race' && e.pos.z < TR.finishZ && e.pos.y > -1) {
      e.finished = true;
      if (e.isPlayer) {
        myFinishTime = raceTime; burst(e.pos);
        if (net.isHost) recordFinish(net.id, myName, raceTime);
        else net.fin.send({ t: raceTime, r: round.id });
      } else recordFinish(e.id, e.name, raceTime);
    }
    if (e.mesh) { e.mesh.position.copy(e.pos); animateBody(e.mesh, e.vel, e.onGround, dt, e); }
  }

  if (net.isHost) { hostTick(); hostBroadcast(dt); }
  sendPose(dt);
  updatePeers(dt);

  // kaamera rig keha juurde
  if (vr) {
    headLocal.set(camera.position.x, 0, camera.position.z).applyQuaternion(rig.quaternion);
    rig.position.set(player.pos.x - headLocal.x, player.pos.y - ARM_OFFSET, player.pos.z - headLocal.z);
  } else {
    rig.position.copy(player.pos); camera.position.set(0, 1.6, 0);
  }

  // konfetid
  for (let i = 0; i < CN; i++) {
    if (cPos[i * 3 + 1] < -50) continue;
    const v = cVel[i]; v.y -= 6 * dt;
    cPos[i * 3] += v.x * dt; cPos[i * 3 + 1] += v.y * dt; cPos[i * 3 + 2] += v.z * dt;
    if (cPos[i * 3 + 1] < -5) cPos[i * 3 + 1] = -100;
  }
  cGeo.attributes.position.needsUpdate = true;

  // HUD ja tulemused
  hudT -= dt;
  if (hudT <= 0) {
    hudT = 0.2;
    const placeIdx = finishOrder.findIndex(f => f[0] === net.id);
    const total = 1 + (net.on ? net.peers.size : 0) + (net.isHost ? bots.length : remoteBots.length);
    let rank;
    if (placeIdx >= 0) rank = placeIdx + 1;
    else {
      const zs = [];
      if (net.isHost) bots.forEach(b => { if (!b.finished) zs.push(b.pos.z); }); else remoteBots.forEach(rb => zs.push(rb.pos.z));
      for (const p of net.peers.values()) if (p.d && !p.d.f) zs.push(p.pos.z);
      rank = finishOrder.length + 1 + zs.filter(z => z < player.pos.z).length;
    }
    drawStats(TR.name + '  ·  ' + Math.floor(raceTime / 60) + ':' + String(Math.floor(raceTime % 60)).padStart(2, '0') + '  ·  Koht ' + rank + '/' + total);
    if (phase === 'results') {
      const lines = finishOrder.slice(0, 5).map((f, i) => (i + 1) + '. ' + f[1] + '  ' + f[2] + ' s');
      if (placeIdx >= 5) lines.push('Sina: ' + (placeIdx + 1) + '. koht');
      if (placeIdx < 0) lines.push('Sa ei jõudnud finišisse');
      lines.push('Järgmine: ' + TRACKS[(trackIndex + 1) % TRACKS.length].name + ' ' + Math.max(0, Math.ceil(round.endAt + 8 - t)) + ' s');
      showMsg('Voor läbi!', lines, 1, true);
    } else if (player.finished) {
      showMsg(placeIdx === 0 ? 'VÕITSID!' : 'Finiš!', [placeIdx >= 0 ? (placeIdx + 1) + '. koht · ' + myFinishTime.toFixed(1) + ' s' : 'Aeg ' + myFinishTime.toFixed(1) + ' s', 'Ootame teisi…'], 1, true);
    } else if (msgSticky) hideMsg();
    updateHud();
  }
  if (!msgSticky) { msgTimer -= dt; msg.mesh.visible = msgTimer > 0; }

  renderer.render(scene, camera);
});
</script>
</body>
</html>
