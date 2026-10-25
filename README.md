[Uploading Kukerpall VR.html…]()
<!doctype html><html><head><meta charset=utf8><meta name=viewport content="width=device-width,initial-scale=1"><style>:root{color-scheme:light}body{margin:0;padding:0;font:14px -apple-system,BlinkMacSystemFont,sans-serif;background:#fff;color:#000}img{max-width:100%}[hidden]:not([hidden=until-found i]){display:none!important}</style></head><body>
<!doctype html>
<html lang="et">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Kukerpall VR</title>
<style>
html,body{margin:0;height:100%;overflow:hidden;background:#8fd3ff;font-family:system-ui,sans-serif}
#info{position:fixed;left:12px;top:12px;color:#123;background:rgba(255,255,255,.85);padding:10px 14px;border-radius:12px;font-size:14px;max-width:360px;line-height:1.45}
</style>
<script type="importmap">{"imports":{"three":"https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.module.js","three/addons/":"https://cdn.jsdelivr.net/npm/three@0.160.0/examples/jsm/"}}</script>
</head>
<body>
<div id="info"><b>Kukerpall VR</b> – takistusraja võidujooks<br>
<b>VR:</b> liigu KÄTEGA – lükka kätega vastu maad ja seinu, vehi kiiresti, et hooga hüpata. Parem pulk = pööra, B/Y = uus mäng<br>
<b>Arvutis:</b> klõpsa ekraanil, WASD + hiir, tühik = hüppa, R = uus mäng</div>
<script type="module">
import * as THREE from 'three';
import { VRButton } from 'three/addons/webxr/VRButton.js';

// ---------- põhi ----------
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setPixelRatio(Math.min(devicePixelRatio, 2));
renderer.setSize(innerWidth, innerHeight);
renderer.xr.enabled = true;
renderer.xr.setReferenceSpaceType('local-floor');
document.body.appendChild(renderer.domElement);
document.body.appendChild(VRButton.createButton(renderer));

const scene = new THREE.Scene();
scene.background = new THREE.Color(0x8fd3ff);
scene.fog = new THREE.Fog(0x8fd3ff, 45, 160);
const camera = new THREE.PerspectiveCamera(70, innerWidth / innerHeight, 0.05, 300);
const rig = new THREE.Group();
scene.add(rig); rig.add(camera);
camera.position.set(0, 1.6, 0);
addEventListener('resize', () => { camera.aspect = innerWidth / innerHeight; camera.updateProjectionMatrix(); renderer.setSize(innerWidth, innerHeight); });

scene.add(new THREE.HemisphereLight(0xffffff, 0x9977bb, 1.15));
const sun = new THREE.DirectionalLight(0xffffff, 1.3); sun.position.set(10, 30, 12); scene.add(sun);

const mats = {};
const mat = c => mats[c] || (mats[c] = new THREE.MeshLambertMaterial({ color: c }));

// ---------- maailm ----------
const solids = [];
function addBox(x, top, z, w, h, d, color, opts = {}) {
  const m = new THREE.Mesh(new THREE.BoxGeometry(w, h, d), mat(color));
  m.position.set(x, top - h / 2, z); scene.add(m);
  const s = Object.assign({ type: 'box', mesh: m, hw: w / 2, hh: h / 2, hd: d / 2, vel: new THREE.Vector3(), delta: new THREE.Vector3(), active: true }, opts);
  solids.push(s); return s;
}
function addCyl(x, top, z, r, h, color) {
  const m = new THREE.Mesh(new THREE.CylinderGeometry(r, r, h, 40), mat(color));
  m.position.set(x, top - h / 2, z); scene.add(m);
  const s = { type: 'cyl', mesh: m, r, hh: h / 2, vel: new THREE.Vector3(), delta: new THREE.Vector3(), active: true };
  solids.push(s); return s;
}

// start
addBox(0, 0, 0, 10, 1, 10, 0x7be495);
// hüppekivid
addBox(-1.5, 0, -8, 3, 1, 3, 0xffd166);
addBox(1.5, 0, -11.5, 3, 1, 3, 0xff7eb6);
addBox(-1, 0, -15, 3, 1, 3, 0x5ec8ff);
// kontrollpunkt 1
addBox(0, 0, -21, 8, 1, 8, 0xb388ff);
// pöörlev latt
const SW = { x: 0, z: -32, L: 7, omega: 1.1, a: 0 };
addCyl(0, 0, -32, 7, 1, 0xffd166);
addCyl(0, 1.4, -32, 0.6, 1.4, 0xff5c5c);
const sweeper = new THREE.Group(); sweeper.position.set(0, 0.55, -32); scene.add(sweeper);
const bar = new THREE.Mesh(new THREE.CylinderGeometry(0.22, 0.22, SW.L * 2, 16), mat(0xff3b3b));
bar.rotation.z = Math.PI / 2; sweeper.add(bar);
for (let i = -3; i <= 3; i++) { if (!i) continue; const st = new THREE.Mesh(new THREE.CylinderGeometry(0.235, 0.235, 0.4, 16), mat(0xffffff)); st.rotation.z = Math.PI / 2; st.position.x = i * 2; sweeper.add(st); }
// sild
addBox(0, 0, -42.5, 4, 1, 7, 0x5ec8ff);
// liikuvad platvormid
const movers = [];
function addMover(z, phase, color) { const s = addBox(0, 0, z, 3, 0.5, 3, color, { mover: { base: 0, amp: 3, speed: 1.1, phase } }); movers.push(s); return s; }
const m1 = addMover(-48.5, 0, 0xff7eb6), m2 = addMover(-52, 2.1, 0xffd166), m3 = addMover(-55.5, 4.2, 0x7be495);
// kontrollpunkt 2
addBox(0, 0, -62, 8, 1, 8, 0xb388ff);
// kukkuvad plaadid
const tiles = [];
const tileCols = [0xff7eb6, 0x5ec8ff, 0xffd166, 0x7be495];
for (let r = 0; r < 6; r++) for (let c = 0; c < 5; c++) {
  const x = -3.8 + c * 1.9, z = -67 - r * 1.9;
  const s = addBox(x, 0, z, 1.8, 0.5, 1.8, tileCols[(r + c) % 4], { tile: true, state: 'idle', t: 0, home: new THREE.Vector3(x, -0.25, z) });
  tiles.push(s);
}
// presside koridor
addBox(0, 0, -84, 6, 1, 13, 0x5ec8ff);
const pushers = [];
[[-81, 0], [-84.5, 1.6], [-88, 3.2]].forEach(([z, ph]) => {
  const s = addBox(0, 1.2, z, 1.6, 1.2, 1.2, 0xff5c5c, { pusher: { phase: ph } }); pushers.push(s);
});
// finiš
addBox(0, 0, -96, 10, 1, 10, 0xffd166);

// dekoratsioonid
const goo = new THREE.Mesh(new THREE.PlaneGeometry(400, 400), mat(0xff5fa2));
goo.rotation.x = -Math.PI / 2; goo.position.y = -6; scene.add(goo);
const line = new THREE.Mesh(new THREE.BoxGeometry(10, 0.02, 0.5), new THREE.MeshBasicMaterial({ color: 0xffffff }));
line.position.set(0, 0.01, -93); scene.add(line);
function arch(z, color) {
  const g = new THREE.Group();
  [-5, 5].forEach(x => { const p = new THREE.Mesh(new THREE.BoxGeometry(0.5, 4, 0.5), mat(color)); p.position.set(x, 2, 0); g.add(p); });
  const top = new THREE.Mesh(new THREE.BoxGeometry(10.5, 0.8, 0.5), mat(color)); top.position.y = 4.2; g.add(top);
  g.position.z = z; scene.add(g);
}
arch(4.5, 0x5ec8ff); arch(-93, 0xff7eb6);
const crown = new THREE.Group();
const band = new THREE.Mesh(new THREE.CylinderGeometry(0.6, 0.6, 0.4, 24, 1, true), new THREE.MeshLambertMaterial({ color: 0xffc400, side: THREE.DoubleSide }));
crown.add(band);
for (let i = 0; i < 6; i++) { const c = new THREE.Mesh(new THREE.ConeGeometry(0.15, 0.4, 8), mat(0xffc400)); const a = i / 6 * Math.PI * 2; c.position.set(Math.cos(a) * 0.55, 0.4, Math.sin(a) * 0.55); crown.add(c); }
crown.position.set(0, 2.2, -98); scene.add(crown);
for (let i = 0; i < 26; i++) {
  const g = new THREE.Group();
  for (let k = 0; k < 3; k++) { const s = new THREE.Mesh(new THREE.SphereGeometry(2 + Math.random() * 2, 10, 8), mat(0xffffff)); s.position.set(k * 2.5, Math.random(), 0); g.add(s); }
  const side = Math.random() < 0.5 ? -1 : 1;
  g.position.set(side * (25 + Math.random() * 40), 8 + Math.random() * 20, 20 - Math.random() * 140); scene.add(g);
}

// ---------- kontrollpunktid ja teekond ----------
const CPS = [
  { trig: 99, pos: new THREE.Vector3(0, 0, 1.5), wi: 0 },
  { trig: -17.5, pos: new THREE.Vector3(0, 0, -21), wi: 5 },
  { trig: -58.5, pos: new THREE.Vector3(0, 0, -62), wi: 12 },
  { trig: -78, pos: new THREE.Vector3(0, 0, -79.5), wi: 14 },
];
const WPS = [
  { x: -1.5, z: -6.8 }, { x: -1.5, z: -8 }, { x: 1.5, z: -11.5 }, { x: -1, z: -15 },
  { x: 0, z: -20, wide: 2.5 }, { x: 0, z: -25.5, wide: 1 }, { side: true, z: -32 }, { x: 0, z: -38.5, wide: 1.5 },
  { x: 0, z: -45.5, wide: 0.8 }, { plat: m1 }, { plat: m2 }, { plat: m3 },
  { x: 0, z: -59, wide: 2.5 }, { x: 0, z: -77, wide: 3 }, { x: 0, z: -90, wide: 2 }, { x: 0, z: -97, wide: 3 },
];
const FINISH_Z = -93;

// ---------- tegelased ----------
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
function makeEnt(isPlayer, color) {
  return { isPlayer, pos: new THREE.Vector3(), vel: new THREE.Vector3(), onGround: false, standingOn: null, r: 0.3, h: isPlayer ? 1.5 : 1.4,
    cp: 0, finished: false, place: 0, knock: 0, mesh: isPlayer ? null : makeBot(color), wi: 0, side: 1, jit: 0, speed: 4.5, skill: 1, walk: 0 };
}
const player = makeEnt(true);
const botColors = [0xff6b6b, 0x4dabf7, 0xffd43b, 0x69db7c, 0xda77f2, 0xff922b, 0x22b8cf, 0xf783ac];
const bots = botColors.map(c => makeEnt(false, c));
const ents = [player, ...bots];

// käed VR-is
const grips = [];
for (let i = 0; i < 2; i++) {
  const grip = renderer.xr.getControllerGrip(i);
  const glove = new THREE.Mesh(new THREE.SphereGeometry(0.07, 14, 10), mat(0x5a3e2b));
  glove.scale.set(1, 0.8, 1.2); grip.add(glove);
  const palm = new THREE.Mesh(new THREE.SphereGeometry(0.045, 10, 8), mat(i ? 0xff7eb6 : 0x5ec8ff));
  palm.position.y = -0.04; grip.add(palm);
  rig.add(grip); grips.push(grip);
}

// ---------- kätega liikumine (VR) ----------
const ARM_OFFSET = 0.6;          // keha on madalam, et käed ulataksid maani
const HAND_R = 0.05;
const hands = [0, 1].map(() => ({ anchor: null, n: new THREE.Vector3(), solid: null }));
const vSamples = []; let wasHolding = false;
const hp = new THREE.Vector3(), mv = new THREE.Vector3(), tv = new THREE.Vector3();
function contactAt(p) {
  let best = null;
  for (const s of solids) {
    if (!s.active) continue;
    const c = s.mesh.position, dx = p.x - c.x, dy = p.y - c.y, dz = p.z - c.z;
    if (s.type === 'box') {
      const ex = s.hw + HAND_R - Math.abs(dx), ey = s.hh + HAND_R - Math.abs(dy), ez = s.hd + HAND_R - Math.abs(dz);
      if (ex > 0 && ey > 0 && ez > 0) {
        let n, d;
        if (dy > 0 && ey < 0.35) { n = new THREE.Vector3(0, 1, 0); d = ey; }
        else if (ex < ez && ex < ey) { n = new THREE.Vector3(Math.sign(dx) || 1, 0, 0); d = ex; }
        else if (ez < ey) { n = new THREE.Vector3(0, 0, Math.sign(dz) || 1); d = ez; }
        else { n = new THREE.Vector3(0, Math.sign(dy) || 1, 0); d = ey; }
        if (!best || d > best.d) best = { s, n, d };
      }
    } else {
      const r = Math.hypot(dx, dz), er = s.r + HAND_R - r, ey = s.hh + HAND_R - Math.abs(dy);
      if (er > 0 && ey > 0) {
        let n, d;
        if (dy > 0 && ey < 0.35) { n = new THREE.Vector3(0, 1, 0); d = ey; }
        else if (er < ey) { n = new THREE.Vector3(dx / (r || 1), 0, dz / (r || 1)); d = er; }
        else { n = new THREE.Vector3(0, Math.sign(dy) || 1, 0); d = ey; }
        if (!best || d > best.d) best = { s, n, d };
      }
    }
  }
  return best;
}
function buzz(i) {
  try {
    const src = renderer.xr.getSession().inputSources[i];
    const a = src && src.gamepad && src.gamepad.hapticActuators && src.gamepad.hapticActuators[0];
    if (a) a.pulse(0.35, 25);
  } catch (e) {}
}
function clearHands() { hands.forEach(h => { h.anchor = null; h.solid = null; }); vSamples.length = 0; wasHolding = false; player.holding = false; }
function armLocomotion(dt) {
  rig.updateMatrixWorld(true);
  mv.set(0, 0, 0); let cnt = 0;
  grips.forEach((g, i) => {
    const h = hands[i]; g.getWorldPosition(hp);
    if (h.anchor) {
      if (h.solid && !h.solid.active) h.anchor = null;
      else {
        if (h.solid) h.anchor.add(h.solid.delta);
        tv.copy(hp).sub(h.anchor);
        if (tv.dot(h.n) > 0.03) h.anchor = null;        // käsi tõmmati pinnalt ära
      }
    }
    if (!h.anchor) {
      const c = contactAt(hp);
      if (c) { h.anchor = hp.clone().addScaledVector(c.n, c.d); h.n.copy(c.n); h.solid = c.s; buzz(i); }
    }
    if (h.anchor) { tv.copy(h.anchor).sub(hp); if (tv.length() > 0.3) tv.setLength(0.3); mv.add(tv); cnt++; }
  });
  const holding = cnt > 0;
  if (holding) {
    mv.divideScalar(cnt); player.pos.add(mv);
    vSamples.push(mv.clone().divideScalar(dt)); if (vSamples.length > 5) vSamples.shift();
    player.vel.set(0, 0, 0);
  } else if (wasHolding) {
    tv.set(0, 0, 0); vSamples.forEach(v => tv.add(v));
    if (vSamples.length) tv.divideScalar(vSamples.length);
    tv.multiplyScalar(1.2); if (tv.length() > 9) tv.setLength(9);
    player.vel.copy(tv); vSamples.length = 0;
  } else if (player.onGround) {
    const f = Math.exp(-6 * dt); player.vel.x *= f; player.vel.z *= f;
  }
  player.holding = holding; wasHolding = holding;
}

// ---------- HUD ----------
function makePanel(w, h, cw, ch, pos) {
  const cv = document.createElement('canvas'); cv.width = cw; cv.height = ch;
  const tex = new THREE.CanvasTexture(cv);
  const m = new THREE.Mesh(new THREE.PlaneGeometry(w, h), new THREE.MeshBasicMaterial({ map: tex, transparent: true, depthTest: false }));
  m.renderOrder = 999; m.position.copy(pos); camera.add(m);
  return { cv, ctx: cv.getContext('2d'), tex, mesh: m };
}
const stats = makePanel(0.6, 0.075, 1024, 128, new THREE.Vector3(0, 0.3, -1.1));
const msg = makePanel(0.9, 0.34, 1024, 384, new THREE.Vector3(0, 0.05, -1.2));
function rr(ctx, x, y, w, h, r) { ctx.beginPath(); ctx.roundRect(x, y, w, h, r); ctx.fill(); }
function drawStats(text) {
  const { ctx, cv } = stats; ctx.clearRect(0, 0, cv.width, cv.height);
  ctx.fillStyle = 'rgba(20,20,50,0.55)'; rr(ctx, 0, 0, cv.width, cv.height, 40);
  ctx.fillStyle = '#fff'; ctx.font = 'bold 64px system-ui'; ctx.textAlign = 'center'; ctx.textBaseline = 'middle';
  ctx.fillText(text, cv.width / 2, cv.height / 2 + 4); stats.tex.needsUpdate = true;
}
let msgTimer = 0, msgSticky = false;
function showMsg(lines, time = 2, sticky = false) {
  const { ctx, cv } = msg; ctx.clearRect(0, 0, cv.width, cv.height);
  ctx.fillStyle = 'rgba(255,255,255,0.9)'; rr(ctx, 0, 0, cv.width, cv.height, 60);
  ctx.textAlign = 'center'; ctx.textBaseline = 'middle';
  const arr = Array.isArray(lines) ? lines : [lines];
  arr.forEach((l, i) => {
    ctx.fillStyle = i ? '#334' : '#ff3d8b'; ctx.font = i ? 'bold 58px system-ui' : 'bold 120px system-ui';
    ctx.fillText(l, cv.width / 2, arr.length === 1 ? cv.height / 2 : 110 + (i ? 70 + (i - 1) * 75 : 0));
  });
  msg.tex.needsUpdate = true; msg.mesh.visible = true; msgTimer = time; msgSticky = sticky;
}

// ---------- konfetid ----------
const CN = 400;
const cGeo = new THREE.BufferGeometry();
const cPos = new Float32Array(CN * 3), cCol = new Float32Array(CN * 3), cVel = [];
for (let i = 0; i < CN; i++) { const c = new THREE.Color().setHSL(Math.random(), 1, 0.6); cCol.set([c.r, c.g, c.b], i * 3); cVel.push(new THREE.Vector3()); cPos[i * 3 + 1] = -100; }
cGeo.setAttribute('position', new THREE.BufferAttribute(cPos, 3));
cGeo.setAttribute('color', new THREE.BufferAttribute(cCol, 3));
const confetti = new THREE.Points(cGeo, new THREE.PointsMaterial({ size: 0.12, vertexColors: true }));
confetti.frustumCulled = false; scene.add(confetti);
function burst(p) { for (let i = 0; i < CN; i++) { cPos.set([p.x, p.y + 2, p.z], i * 3); cVel[i].set((Math.random() - .5) * 8, 4 + Math.random() * 7, (Math.random() - .5) * 8); } }

// ---------- füüsika ----------
const G = 18, JUMP = 7, tmp = new THREE.Vector3();
function groundTopAt(x, z, y) {
  let best = -Infinity;
  for (const s of solids) {
    if (!s.active) continue; const p = s.mesh.position, top = p.y + s.hh;
    if (top > y + 0.3) continue;
    const inside = s.type === 'box' ? (Math.abs(x - p.x) <= s.hw + 0.12 && Math.abs(z - p.z) <= s.hd + 0.12) : Math.hypot(x - p.x, z - p.z) <= s.r;
    if (inside && top > best) best = top;
  }
  return best;
}
function stepEntity(e, dt) {
  if (e.standingOn && e.standingOn.active && !e.holding) e.pos.add(e.standingOn.delta);
  if (!e.holding) e.vel.y -= G * dt;
  const prevY = e.pos.y;
  e.pos.addScaledVector(e.vel, dt);
  e.onGround = false; e.standingOn = null;
  for (const s of solids) {
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
  if (e.standingOn && e.standingOn.tile && e.standingOn.state === 'idle') { e.standingOn.state = 'shake'; e.standingOn.t = 0; }
  // pöörlev latt
  const bx = Math.cos(SW.a), bz = -Math.sin(SW.a);
  if (e.pos.y < 0.8 && e.pos.y + e.h > 0.3) {
    const rx = e.pos.x - SW.x, rz = e.pos.z - SW.z;
    let s = rx * bx + rz * bz; s = Math.max(-SW.L, Math.min(SW.L, s));
    const cx = SW.x + bx * s, cz = SW.z + bz * s;
    let nx = e.pos.x - cx, nz = e.pos.z - cz; const d = Math.hypot(nx, nz), min = e.r + 0.25;
    if (d < min) {
      if (d < 1e-4) { nx = -bz; nz = bx; } else { nx /= d; nz /= d; }
      e.pos.x = cx + nx * min; e.pos.z = cz + nz * min;
      const qx = cx - SW.x, qz = cz - SW.z;
      e.vel.x = qz * SW.omega * 1.2 + nx * 3; e.vel.z = -qx * SW.omega * 1.2 + nz * 3; e.vel.y = 4;
      e.knock = 0.5; e.onGround = false; e.standingOn = null;
    }
  }
  e.knock = Math.max(0, e.knock - dt);
}
function steer(e, tx, tz, dt) {
  const k = e.knock > 0 ? 1 : (e.onGround ? 10 : 2.5);
  const f = Math.min(1, k * dt);
  e.vel.x += (tx - e.vel.x) * f; e.vel.z += (tz - e.vel.z) * f;
}
function respawn(e) {
  const c = CPS[e.cp].pos;
  e.pos.set(c.x + (Math.random() - .5) * 3, c.y + 0.5, c.z + (Math.random() - .5) * 2);
  e.vel.set(0, 0, 0); e.knock = 0; e.wi = CPS[e.cp].wi;
  if (e.isPlayer) clearHands();
}

// ---------- roboti mõistus ----------
function wpPos(b, wp) {
  if (wp.plat) return { x: wp.plat.mesh.position.x, z: wp.plat.mesh.position.z };
  if (wp.side) return { x: b.side * 3.5, z: wp.z };
  return { x: wp.x + (wp.wide ? b.jit * wp.wide : 0), z: wp.z };
}
function botAI(b, dt) {
  if (b.finished) { steer(b, 0, 0, dt); if (b.onGround && Math.random() < dt * 0.8) b.vel.y = JUMP * 0.8; return; }
  const wp = WPS[Math.min(b.wi, WPS.length - 1)], p = wpPos(b, wp);
  let dx = p.x - b.pos.x, dz = p.z - b.pos.z; const dist = Math.hypot(dx, dz);
  if (wp.plat) { if (b.standingOn === wp.plat) { b.wi++; return; } }
  else if (dist < 1 || b.pos.z < p.z - 0.3) { b.wi = Math.min(b.wi + 1, WPS.length - 1); }
  let tx = 0, tz = 0;
  const waiting = wp.plat && b.onGround && b.standingOn !== wp.plat && Math.abs(p.x - b.pos.x) > 1;
  if (!waiting && dist > 0.01) { tx = dx / dist * b.speed; tz = dz / dist * b.speed; }
  steer(b, tx, tz, dt);
  if (b.onGround && !waiting && dist > 0.01) {
    const gx = b.pos.x + dx / dist * 0.7, gz = b.pos.z + dz / dist * 0.7;
    if (groundTopAt(gx, gz, b.pos.y) < b.pos.y - 0.5) b.vel.y = JUMP;
    // latist üle hüppamine
    const rx = b.pos.x - SW.x, rz = b.pos.z - SW.z;
    if (Math.hypot(rx, rz) < 7.5) {
      const bx = Math.cos(SW.a), bz = -Math.sin(SW.a);
      const perp = Math.abs(rx * -bz + rz * bx);
      if (perp < 1.4 && Math.random() < dt * 7 * b.skill) b.vel.y = JUMP;
    }
  }
}

// ---------- mängu olek ----------
let state = 'countdown', cd = 3.5, raceTime = 0, finishCount = 0, lastCdShown = -1;
function resetGame() {
  state = 'countdown'; cd = 3.5; raceTime = 0; finishCount = 0; lastCdShown = -1;
  ents.forEach((e, i) => {
    e.cp = 0; e.finished = false; e.place = 0; e.knock = 0; e.wi = 0;
    e.vel.set(0, 0, 0); e.standingOn = null;
    if (e.isPlayer) e.pos.set(0, 0.2, 1.5);
    else {
      e.pos.set(-4 + (i - 1) * (8 / 7), 0.2, 3.3);
      e.side = Math.random() < 0.5 ? -1 : 1; e.jit = Math.random() * 2 - 1;
      e.speed = 3.3 + Math.random() * 0.9; e.skill = 0.6 + Math.random() * 0.6;
    }
  });
  tiles.forEach(s => { s.state = 'idle'; s.active = true; s.mesh.position.copy(s.home); });
  rig.rotation.y = 0; clearHands();
  for (let i = 0; i < CN; i++) cPos[i * 3 + 1] = -100;
}
function fmt(t) { const m = Math.floor(t / 60), s = Math.floor(t % 60); return m + ':' + String(s).padStart(2, '0'); }

// ---------- sisend ----------
const keys = {};
addEventListener('keydown', e => { keys[e.code] = true; if (e.code === 'KeyR') resetGame(); });
addEventListener('keyup', e => { keys[e.code] = false; });
renderer.domElement.addEventListener('click', () => { if (!renderer.xr.isPresenting) renderer.domElement.requestPointerLock(); });
let pitch = 0;
addEventListener('mousemove', e => {
  if (document.pointerLockElement !== renderer.domElement) return;
  rig.rotation.y -= e.movementX * 0.0025;
  pitch = Math.max(-1.3, Math.min(1.3, pitch - e.movementY * 0.0025)); camera.rotation.x = pitch;
});
let jumpWas = false, resetWas = false, turnLatch = false;
const fw = new THREE.Vector3(), rt = new THREE.Vector3();
function handleInput(dt) {
  let mx = 0, my = 0, rx = 0, jump = false, reset = false;
  const session = renderer.xr.getSession();
  if (session) for (const src of session.inputSources) {
    const gp = src.gamepad; if (!gp) continue;
    const ax = gp.axes.length >= 4 ? gp.axes[2] : (gp.axes[0] || 0), ay = gp.axes.length >= 4 ? gp.axes[3] : (gp.axes[1] || 0);
    if (src.handedness === 'left') { mx = ax; my = -ay; } else rx = ax;
    const b = gp.buttons;
    if ((b[0] && b[0].pressed) || (b[4] && b[4].pressed)) jump = true;
    if (b[5] && b[5].pressed) reset = true;
  }
  if (keys.KeyW || keys.ArrowUp) my += 1; if (keys.KeyS || keys.ArrowDown) my -= 1;
  if (keys.KeyD || keys.ArrowRight) mx += 1; if (keys.KeyA || keys.ArrowLeft) mx -= 1;
  if (keys.Space) jump = true;
  if (Math.abs(mx) < 0.15) mx = 0; if (Math.abs(my) < 0.15) my = 0;
  if (Math.abs(rx) > 0.7 && !turnLatch) { rig.rotation.y -= Math.sign(rx) * Math.PI / 6; turnLatch = true; }
  if (Math.abs(rx) < 0.3) turnLatch = false;
  if (reset && !resetWas) resetGame();
  resetWas = reset;
  camera.getWorldDirection(fw); fw.y = 0; fw.normalize(); rt.set(-fw.z, 0, fw.x);
  tmp.set(0, 0, 0).addScaledVector(fw, my).addScaledVector(rt, mx); if (tmp.length() > 1) tmp.normalize();
  const canMove = state !== 'countdown';
  if (session) {
    // VR: liigud ainult kätega, nagu ahv
    if (canMove) armLocomotion(dt); else { clearHands(); steer(player, 0, 0, dt); }
  } else {
    steer(player, canMove ? tmp.x * player.speed : 0, canMove ? tmp.z * player.speed : 0, dt);
    if (jump && !jumpWas && player.onGround && canMove) player.vel.y = JUMP;
  }
  jumpWas = jump;
}

// ---------- tsükkel ----------
const clock = new THREE.Clock();
let time = 0, hudT = 0;
resetGame();
renderer.setAnimationLoop(() => {
  const dt = Math.min(clock.getDelta(), 0.05); time += dt;

  // maailm liigub
  SW.a += SW.omega * dt; sweeper.rotation.y = SW.a;
  for (const s of movers) {
    const old = s.mesh.position.x, nx = s.mover.base + Math.sin(time * s.mover.speed + s.mover.phase) * s.mover.amp;
    s.mesh.position.x = nx; s.delta.set(nx - old, 0, 0); s.vel.set((nx - old) / dt, 0, 0);
  }
  for (const s of pushers) {
    const old = s.mesh.position.x, nx = Math.sin(time * 1.6 + s.pusher.phase) * 2.2;
    s.mesh.position.x = nx; s.delta.set(nx - old, 0, 0); s.vel.set((nx - old) / dt, 0, 0);
  }
  for (const s of tiles) {
    if (s.state === 'shake') {
      s.t += dt; s.mesh.position.x = s.home.x + Math.sin(s.t * 60) * 0.04;
      if (s.t > 0.8) { s.state = 'fall'; s.active = false; s.t = 0; s.mesh.position.x = s.home.x; }
    } else if (s.state === 'fall') {
      s.t += dt; s.mesh.position.y = s.home.y - s.t * s.t * 6;
      if (s.t > 3.5) { s.state = 'idle'; s.active = true; s.mesh.position.copy(s.home); }
    }
  }
  crown.rotation.y += dt; crown.position.y = 2.2 + Math.sin(time * 2) * 0.15;

  // olek
  if (state === 'countdown') {
    cd -= dt; const n = Math.ceil(cd);
    if (n !== lastCdShown && n > 0 && n <= 3) { showMsg(String(n), 1.1); lastCdShown = n; }
    if (cd <= 0) { state = 'race'; showMsg('MINE!', 1); }
  } else if (state === 'race') raceTime += dt;

  handleInput(dt);
  if (state !== 'countdown') for (const b of bots) botAI(b, dt); else for (const b of bots) steer(b, 0, 0, dt);
  for (const e of ents) stepEntity(e, dt);

  // tegelased lükkavad üksteist
  for (let i = 0; i < ents.length; i++) for (let j = i + 1; j < ents.length; j++) {
    const a = ents[i], b = ents[j], dx = b.pos.x - a.pos.x, dz = b.pos.z - a.pos.z, d = Math.hypot(dx, dz);
    if (d < 0.6 && d > 1e-4 && Math.abs(a.pos.y - b.pos.y) < 1) {
      const p = (0.6 - d) / 2, nx = dx / d, nz = dz / d;
      a.pos.x -= nx * p; a.pos.z -= nz * p; b.pos.x += nx * p; b.pos.z += nz * p;
    }
  }

  for (const e of ents) {
    if (e.pos.y < -6) { respawn(e); if (e.isPlayer) showMsg(['Plärts!', 'Proovi uuesti'], 1.2); }
    for (let c = e.cp + 1; c < CPS.length; c++) if (e.onGround && e.pos.z < CPS[c].trig) {
      e.cp = c; if (e.isPlayer) showMsg('Kontrollpunkt!', 1.3);
    }
    if (!e.finished && e.pos.z < FINISH_Z && e.pos.y > -1) {
      e.finished = true; e.place = ++finishCount;
      if (e.isPlayer) {
        state = 'done'; burst(e.pos);
        showMsg([e.place === 1 ? 'VÕITSID!' : 'Finiš!', 'Koht ' + e.place + '/' + ents.length + '  ·  aeg ' + fmt(raceTime), 'Uus mäng: B/Y või R'], 999, true);
      }
    }
    if (e.mesh) {
      e.mesh.position.copy(e.pos);
      const sp = Math.hypot(e.vel.x, e.vel.z);
      if (sp > 0.3) e.mesh.rotation.y = Math.atan2(e.vel.x, e.vel.z);
      e.walk += sp * dt * 3; e.mesh.rotation.z = e.onGround ? Math.sin(e.walk) * 0.08 * Math.min(1, sp) : 0.15;
    }
  }
  rig.position.copy(player.pos);
  const vr = renderer.xr.isPresenting;
  if (vr) rig.position.y -= ARM_OFFSET;
  player.h = vr ? 1.0 : 1.5;

  // konfetid
  for (let i = 0; i < CN; i++) {
    if (cPos[i * 3 + 1] < -50) continue;
    const v = cVel[i]; v.y -= 6 * dt;
    cPos[i * 3] += v.x * dt; cPos[i * 3 + 1] += v.y * dt; cPos[i * 3 + 2] += v.z * dt;
    if (cPos[i * 3 + 1] < -5) cPos[i * 3 + 1] = -100;
  }
  cGeo.attributes.position.needsUpdate = true;

  // HUD
  hudT -= dt;
  if (hudT <= 0) {
    hudT = 0.2;
    const prog = e => e.finished ? 1000 - e.place : -e.pos.z;
    const rank = player.finished ? player.place : 1 + ents.filter(e => e !== player && prog(e) > prog(player)).length;
    drawStats('Aeg ' + fmt(raceTime) + '   ·   Koht ' + rank + '/' + ents.length);
  }
  if (!msgSticky) { msgTimer -= dt; msg.mesh.visible = msgTimer > 0; }

  renderer.render(scene, camera);
});
</script>
</body>

</body></html>
