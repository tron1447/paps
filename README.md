[Uploading index (54).html…]()
<!doctype html>
<html lang="et">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover">
<title>Kukerpall VR</title>
<style>
:root{--ink:#1d1b3a;--card:rgba(255,255,255,.95);--pink:#ff3d8b;--blue:#2f9bff}
html,body{margin:0;width:100%;height:100%;overflow:hidden;overscroll-behavior:none;background:#1d1b3a;font-family:system-ui,sans-serif;color:var(--ink)}
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
canvas{touch-action:none;display:block;position:fixed;left:0;top:0;width:100vw!important;height:100vh!important;height:100dvh!important}
#menu{background:#8fd3ff}

#touch{display:none;position:fixed;left:0;top:0;width:100vw;height:100vh;height:100dvh;pointer-events:none;z-index:6;user-select:none;-webkit-user-select:none}
#touch.on{display:block}
#stick{position:absolute;left:calc(24px + env(safe-area-inset-left));bottom:calc(28px + env(safe-area-inset-bottom));width:140px;height:140px;border-radius:50%;background:rgba(255,255,255,.28);border:3px solid rgba(255,255,255,.7);pointer-events:auto;touch-action:none}
#knob{position:absolute;left:45px;top:45px;width:50px;height:50px;border-radius:50%;background:rgba(255,255,255,.9)}
.tb{position:absolute;pointer-events:auto;touch-action:none;border:0;border-radius:50%;font-weight:800;color:#fff;font-size:16px;box-shadow:0 4px 12px rgba(0,0,0,.2)}
#tJump{right:calc(28px + env(safe-area-inset-right));bottom:calc(36px + env(safe-area-inset-bottom));width:96px;height:96px;background:var(--pink)}
#tGrab{right:calc(136px + env(safe-area-inset-right));bottom:calc(30px + env(safe-area-inset-bottom));width:72px;height:72px;background:var(--blue)}
#tEmote{right:calc(40px + env(safe-area-inset-right));bottom:calc(146px + env(safe-area-inset-bottom));width:60px;height:60px;background:#f59e0b;font-size:24px}
#tDuck{right:calc(220px + env(safe-area-inset-right));bottom:calc(30px + env(safe-area-inset-bottom));width:72px;height:72px;background:#8b5cf6;font-size:13px;display:none}
#exitBtn{display:none;position:fixed;left:12px;top:160px;z-index:9;font-size:15px;font-weight:800;padding:9px 13px;border:0;border-radius:12px;background:rgba(29,27,58,.7);color:#fff;cursor:pointer}
#exitBtn.warn{background:#e03131}
#feed{position:fixed;right:12px;top:62px;z-index:7;display:flex;flex-direction:column;align-items:flex-end;gap:4px;pointer-events:none;max-width:60vw}
#feed div{background:rgba(29,27,58,.78);color:#fff;font:700 14px system-ui;padding:5px 10px;border-radius:10px;transition:opacity .5s}
#coopStart{display:none;position:fixed;left:50%;top:76px;transform:translateX(-50%);z-index:8;font-size:16px;font-weight:800;padding:10px 20px;border:0;border-radius:14px;background:#0ea5e9;color:#fff;cursor:pointer;box-shadow:0 6px 18px rgba(0,0,0,.2)}
#padBtn{display:none;position:fixed;left:12px;top:110px;z-index:9;width:44px;height:44px;font-size:22px;border:0;border-radius:12px;background:rgba(29,27,58,.6);color:#fff}
#fsBtn{display:none;position:fixed;right:12px;bottom:200px;z-index:8;width:48px;height:48px;font-size:24px;border:0;border-radius:12px;background:rgba(29,27,58,.6);color:#fff}
#lvMenuBtn{display:none;position:fixed;right:12px;top:12px;z-index:8;font-size:15px;font-weight:800;padding:9px 14px;border:0;border-radius:14px;background:#8b5cf6;color:#fff}
#levelSel,#lvDone{display:none;position:fixed;inset:0;z-index:31;align-items:center;justify-content:center;padding:12px;box-sizing:border-box;background:rgba(0,0,0,.3);overflow:auto}
#levelSel.on,#lvDone.on{display:flex}#levelSel .card,#lvDone .card{background:#fff;border-radius:22px;padding:16px 18px;max-width:560px;width:100%;margin:auto}
#lvGrid{display:grid;grid-template-columns:repeat(6,1fr);gap:7px}
.lvb{border:0;border-radius:12px;padding:7px 2px;background:#eef0ff;font-size:12px;line-height:1.3;cursor:pointer;color:#1d1b3a}
.lvb b{font-size:17px}.lvb.lock{background:#e9ecef;color:#999}
#lvDone .big{min-width:120px;flex:0 1 auto}
#shopBtn{display:none;position:fixed;right:12px;top:12px;z-index:8;font-size:16px;font-weight:800;padding:10px 16px;border:0;border-radius:14px;background:#f59e0b;color:#fff;cursor:pointer}
#shop{display:none;position:fixed;inset:0;z-index:30;align-items:center;justify-content:center;padding:16px;box-sizing:border-box;background:rgba(0,0,0,.25);overflow:auto}
#shop.on{display:flex}#shop .card{background:var(--card);border-radius:22px;padding:18px 20px;max-width:460px;width:100%;margin:auto}
.it{display:flex;align-items:center;justify-content:space-between;gap:8px;padding:8px 10px;border-radius:12px;background:#f1f3f8;margin:6px 0;font-size:15px}
.it button{border:0;border-radius:10px;padding:8px 12px;font-weight:700;color:#fff;cursor:pointer;min-width:96px}
#dropBtn{display:none;position:fixed;left:50%;transform:translateX(-50%);bottom:14px;z-index:7;border:0;border-radius:14px;padding:12px 18px;font-weight:800;color:#fff;background:#ff3d8b;font-size:15px}
#shieldGlow{display:none;position:fixed;inset:0;pointer-events:none;z-index:4;box-shadow:inset 0 0 60px 20px rgba(80,200,255,.55)}
#buildBar{display:none;position:fixed;left:50%;transform:translateX(-50%);bottom:12px;z-index:7;gap:8px}
#buildBar.on{display:flex}.tb2{border:0;border-radius:14px;padding:12px 14px;font-weight:800;color:#fff;background:#495057;font-size:14px}
#bBuild{background:#22c55e}#bDel{background:#ef4444}
#startBtn{display:none;position:fixed;left:50%;top:14px;transform:translateX(-50%);z-index:8;font-size:20px;font-weight:800;padding:14px 26px;border:0;border-radius:16px;background:#22c55e;color:#fff;box-shadow:0 6px 18px rgba(0,0,0,.2);cursor:pointer}
small{opacity:.8}
@media (max-height:480px){
#menu{align-items:flex-start}#menu .card{display:flex;flex-direction:column;padding:12px 16px}#menu h1{font-size:22px}#menu p{font-size:12px;margin:2px 0 6px}#playRow{order:-1;margin-bottom:4px}#playRow button.big{margin-top:6px;padding:10px;font-size:15px}#menu label.t{margin:6px 0 2px}
#stick{width:110px;height:110px;left:calc(14px + env(safe-area-inset-left));bottom:calc(14px + env(safe-area-inset-bottom))}#knob{left:32px;top:32px;width:46px;height:46px}
#tJump{width:76px;height:76px;right:calc(16px + env(safe-area-inset-right));bottom:calc(16px + env(safe-area-inset-bottom))}
#tGrab{width:58px;height:58px;right:calc(102px + env(safe-area-inset-right));bottom:calc(12px + env(safe-area-inset-bottom))}
#tDuck{width:58px;height:58px;right:calc(170px + env(safe-area-inset-right));bottom:calc(12px + env(safe-area-inset-bottom))}
#tEmote{width:48px;height:48px;right:calc(28px + env(safe-area-inset-right));bottom:calc(102px + env(safe-area-inset-bottom))}
#fsBtn{bottom:auto;top:56px;width:40px;height:40px;font-size:20px}
#padBtn{top:40px;left:6px;width:38px;height:38px;font-size:19px}#exitBtn{top:84px;left:6px;font-size:12px;padding:6px 9px}
#shopBtn{font-size:13px;padding:7px 11px;top:6px}
#startBtn{font-size:16px;padding:10px 18px;top:8px}#coopStart{font-size:13px;padding:7px 14px;top:52px}#feed{top:46px}#feed div{font-size:11px;padding:3px 7px}#hud{font-size:12px;padding:5px 9px;top:6px;left:6px;max-width:45vw}
#buildBar{bottom:8px}.tb2{padding:9px 10px;font-size:12px}
}
</style>
<script type="importmap">{"imports":{"three":"https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.module.js","three/addons/":"https://cdn.jsdelivr.net/npm/three@0.160.0/examples/jsm/"}}</script>
</head>
<body>
<div id="loadErr" style="display:none;position:fixed;inset:0;z-index:99;background:#1d1b3a;color:#fff;font:16px system-ui;padding:24px;box-sizing:border-box;overflow:auto"></div>
<script>
(function () {
  function show(msg) { var e = document.getElementById('loadErr'); if (!e) return; e.style.display = 'block'; e.innerHTML = '<h2 style="color:#ff3d8b;margin-top:0">Mäng ei laadinud</h2><p>' + msg + '</p><p><b>Proovi nii:</b><br>1. Ava mäng <b>Chrome\'is</b> GitHub Pages lingilt (https://...github.io/...), mitte failina telefonist.<br>2. Kontrolli, et internet töötab (mäng laadib 3D-mootori internetist).<br>3. Lae leht uuesti.</p><p style="opacity:.7;font-size:13px">Aadress: ' + location.href + '</p>'; }
  if (location.protocol === 'file:' || location.protocol === 'content:') setTimeout(function () { if (!window.__kpLoaded) show('Fail on avatud otse seadmest (' + location.protocol + '). Nii ei saa mäng 3D-mootorit laadida.'); }, 2500);
  setTimeout(function () { if (!window.__kpLoaded) show('Mängu kood ei käivitunud 10 sekundi jooksul.'); }, 10000);
  window.addEventListener('error', function (ev) { if (!window.__kpLoaded) show('Viga: ' + (ev.message || 'skript ei laadinud')); }, true);
})();
</script>
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
    <label><input type="checkbox" id="bots" checked> Robotid ka (sõpradega)</label>
  </div>
  <label class="t">Graafika</label>
  <div class="row">
    <label><input type="radio" name="gfx" value="ilus"> Ilus (varjud)</label>
    <label><input type="radio" name="gfx" value="kiire"> Kiire (kui mäng hakib)</label>
  </div>
  <div class="row" id="playRow">
    <button class="big" id="solo">Mängi robotitega</button>
    <button class="big" id="online">Mängi sõpradega</button>
    <button class="big" id="practiceBtn" style="background:#22c55e">Harjuta 🎓</button>
    <button class="big" id="levelsBtn" style="background:#8b5cf6">Tasemed ⭐ (30)</button>
    <button class="big" id="coopBtn" style="background:#0ea5e9">Koostöö 🤝</button>
    <button class="big" id="dailyBtn" style="background:#e8590c">📅 Päeva väljakutse</button>
  </div>
  <div id="status"></div>
  <p id="statLine" style="font-size:13px;margin:8px 0 0"></p>
  <button class="big" id="menuShop" style="background:#f59e0b;width:100%;margin-top:8px">🛒 Pood (kostüümid, mütsid, värvid)</button>
  <details><summary><b>Juhtimine</b></summary>
  <p><b>Arvutis:</b> klõpsa ekraanil või lohista hiirega, WASD liigu, nooled pööra, tühik = hüppa, C = kükita, E (hoia) = haara, lase lahti = viska sõber ette, 1–4 = emotikonid, P = pood, Enter = alusta, R = tagasi ootesaali (ainult juht). Rajaehitajas: F ehita, G kustuta, T tüüp.<br>
  <b>VR kätega:</b> lükka kätega vastu maad ja seinu, vehi hooga, et hüpata. Parem pulk vasak/parem = pööra, <b>üles/alla = maapinna kõrgus</b>. Grip = haara, <b>vehi ja lase lahti = viska sõber</b> (aitab üle aukude). Batuudil saab liikuda ka pulgaga. Käsi üle pea ja lehvita = „Tere!”. Pulgavajutus = emotikon. <b>Y = VR-menüü</b> (nimi, värv, müts, seaded). B (hoia) = tagasi ootesaali.<br>
  <b>Tassimine:</b> hoia haaramist kauem – sõber tõstetakse üles (VR-is järgib ta su kätt). Lase hooga lahti = viska.<br>
  <b>Pealtvaatajana:</b> F / nupp / päästik = viska pall rajale.<br>
  <b>VR pulgaga:</b> vasak pulk = liigu, A/X = hüppa, muu sama.</p></details>
  <p><small><b>Telefonis:</b> vasak ring = liigu, lohista ekraani = vaata ringi, nupud = hüppa / haara / emotikon.<br>
  <b>Alustamine:</b> üksi mängides vajuta „Alusta” või astu rohelisele ringile. Sõpradega mängides alustab <b>esimene ruumi tulnud mängija</b>. Ootesaalis saab juht valida ka <b>🤝 Koostöö</b> (K) – kõik peavad koos finišisse jõudma.<br>
  <b>Liigad:</b> edasi pääsedes ja võites saad karikaid 🏆 – Pronksist Meistrini. Iga päev järjest mängides kasvab päevaboonus 🔥.</small></p>
</div></div>
<div id="hud"></div>
<div id="shieldGlow"></div>
<button id="shopBtn">🛒 Pood</button>
<button id="lvMenuBtn">☰ Tasemed</button>
<div id="levelSel"><div class="card">
  <h2 style="margin:0 0 4px;color:#8b5cf6">Tasemed</h2>
  <p id="lvTotal" style="margin:0 0 8px;font-weight:700"></p>
  <button class="big" id="lvDaily" style="background:#e8590c;width:100%;margin:0 0 8px">📅 Päeva väljakutse – iga päev uus rada</button>
  <div id="lvGrid"></div>
  <p style="font-size:13px;margin:8px 0 0">Iga tase avab järgmise. Kiire aeg = 3 tähte. Arvutis: R = uuesti, Enter = järgmine.</p>
  <button class="big" id="lvBack" style="background:#495057;width:100%">Tagasi menüüsse</button>
</div></div>
<div id="lvDone"><div class="card" style="text-align:center">
  <h2 id="lvTitle" style="margin:0;color:#8b5cf6"></h2>
  <div id="lvStars" style="font-size:44px;margin:6px 0"></div>
  <p id="lvInfo" style="margin:0 0 10px"></p>
  <div class="row" style="justify-content:center">
    <button class="big" id="lvNext" style="background:#22c55e">Järgmine ▶</button>
    <button class="big" id="lvRetry" style="background:#f59e0b">Uuesti ↻</button>
    <button class="big" id="lvList" style="background:#8b5cf6">Tasemed ☰</button>
  </div>
</div></div>
<button id="fsBtn" title="Täisekraan">⛶</button>
<button id="exitBtn" title="Peamenüüsse (teise ruumi)">⏏ Menüü</button>
<button id="padBtn" title="Puutenupud sisse/välja">🎮</button>
<div id="shop"><div class="card">
  <h2 style="margin:0 0 4px;color:var(--pink)">Pood</h2>
  <p id="coinsLbl" style="margin:0 0 10px;font-weight:700"></p>
  <div id="shopItems"></div>
  <p style="font-size:13px;margin:10px 0 0">Münte saad rajalt korjates, finišis, edasi pääsedes ja krooni võites.</p>
  <button class="big" id="shopClose" style="background:#22c55e;width:100%">Sulge</button>
</div></div>
<button id="dropBtn">🎳 VISKA PALL RAJALE</button>
<div id="buildBar"><button class="tb2" id="bBuild">EHITA</button><button class="tb2" id="bDel">KUSTUTA</button><button class="tb2" id="bType">TÜÜP: Kast</button></div>
<button id="startBtn">▶ ALUSTA MÄNGU</button>
<button id="coopStart">🤝 KOOSTÖÖ (kõik koos)</button>
<div id="feed"></div>
<div id="touch">
  <div id="stick"><div id="knob"></div></div>
  <button class="tb" id="tJump">HÜPPA</button>
  <button class="tb" id="tGrab">HAARA</button>
  <button class="tb" id="tEmote">😊</button>
  <button class="tb" id="tDuck">KÜKITA</button>
</div>
<script type="module">
import * as THREE from 'three';
import { VRButton } from 'three/addons/webxr/VRButton.js';
import { RoundedBoxGeometry } from 'three/addons/geometries/RoundedBoxGeometry.js';
window.__kpLoaded = true;
const TRYSTERO_URL = 'https://cdn.jsdelivr.net/npm/trystero@0.25.0/+esm';

// =====================================================================
// PÕHI
// =====================================================================
let gfx = 'ilus';
try { gfx = localStorage.getItem('kp_gfx') || 'ilus'; } catch (e) {}
const HEADSET = typeof navigator !== 'undefined' && /OculusBrowser|Quest|Pico|Wolvic|VR/i.test(navigator.userAgent || '');
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setPixelRatio(gfx === 'ilus' ? Math.min(devicePixelRatio, 2) : 1);
renderer.setSize(innerWidth, innerHeight, false);
renderer.shadowMap.enabled = gfx === 'ilus';
renderer.shadowMap.type = THREE.PCFSoftShadowMap;
renderer.xr.enabled = true;
renderer.xr.setReferenceSpaceType('local-floor');
document.body.appendChild(renderer.domElement);
const vrBtn = VRButton.createButton(renderer);
vrBtn.style.zIndex = 20;
if (!HEADSET && (('ontouchstart' in window) || (window.matchMedia && matchMedia('(pointer: coarse)').matches))) vrBtn.style.display = 'none';
document.body.appendChild(vrBtn);

const scene = new THREE.Scene();
const HORIZON = 0xa8e0ff;
scene.background = new THREE.Color(HORIZON);
scene.fog = new THREE.Fog(HORIZON, 55, 190);
const camera = new THREE.PerspectiveCamera(70, innerWidth / innerHeight, 0.05, 320);
const rig = new THREE.Group();
scene.add(rig); rig.add(camera);
camera.position.set(0, 1.6, 0);
let fitW = 0, fitH = 0;
function fitScreen() {   // telefoni pööramisel kohanda pilt kogu ekraanile
  if (renderer.xr.isPresenting) return;
  // võta suurus sellest, kui suur lõuend ekraanil päriselt on (CSS venitab selle kogu ekraanile)
  const c = renderer.domElement;
  const w = Math.round(c.clientWidth || innerWidth), h = Math.round(c.clientHeight || innerHeight);
  if (!w || !h || (Math.abs(w - fitW) < 1 && Math.abs(h - fitH) < 1)) return;
  fitW = w; fitH = h;
  camera.aspect = w / h; camera.updateProjectionMatrix(); renderer.setSize(w, h, false);
}
addEventListener('resize', fitScreen);
addEventListener('orientationchange', () => { setTimeout(fitScreen, 150); setTimeout(fitScreen, 600); });

const hemi = new THREE.HemisphereLight(0xeaf6ff, 0xb48ad6, 1.0); scene.add(hemi);
const sun = new THREE.DirectionalLight(0xfff4e0, 1.7); sun.position.set(14, 32, 10); scene.add(sun); scene.add(sun.target);
sun.castShadow = true;
sun.shadow.mapSize.set(HEADSET ? 1024 : 2048, HEADSET ? 1024 : 2048);
{ const sc = sun.shadow.camera; sc.left = -24; sc.right = 24; sc.top = 24; sc.bottom = -24; sc.near = 1; sc.far = 90; }
sun.shadow.bias = -0.0006; sun.shadow.normalBias = 0.03;

const V3 = THREE.Vector3;
const now = () => performance.now() / 1000;
const rnd = (a, b) => a + Math.random() * (b - a);

// multikalik "toon" varjutus: 3 selget tooni
const gradTex = (() => { const t = new THREE.DataTexture(new Uint8Array([110, 185, 255]), 3, 1, THREE.RedFormat); t.minFilter = t.magFilter = THREE.NearestFilter; t.needsUpdate = true; return t; })();
const mats = {};
const mat = c => mats[c] || (mats[c] = new THREE.MeshToonMaterial({ color: c, gradientMap: gradTex }));
// ruuduline muster suurtele põrandatele
const checkerImg = (() => { const cv = document.createElement('canvas'); cv.width = cv.height = 64; const x = cv.getContext('2d'); x.fillStyle = '#ffffff'; x.fillRect(0, 0, 64, 64); x.fillStyle = '#e4e4ee'; x.fillRect(0, 0, 32, 32); x.fillRect(32, 32, 32, 32); return cv; })();
const checkMats = {};
function conveyorMat(w, d) {   // liikuva lindi nooled
  const cv = document.createElement('canvas'); cv.width = 64; cv.height = 64; const x = cv.getContext('2d');
  x.fillStyle = '#7d8794'; x.fillRect(0, 0, 64, 64); x.fillStyle = '#ffd43b';
  x.beginPath(); x.moveTo(32, 50); x.lineTo(10, 28); x.lineTo(22, 28); x.lineTo(22, 10); x.lineTo(42, 10); x.lineTo(42, 28); x.lineTo(54, 28); x.closePath(); x.fill();
  const tex = new THREE.CanvasTexture(cv); tex.wrapS = tex.wrapT = THREE.RepeatWrapping; tex.repeat.set(Math.round(w / 2), Math.round(d / 2)); tex.colorSpace = THREE.SRGBColorSpace;
  return new THREE.MeshToonMaterial({ map: tex, gradientMap: gradTex });
}
const neonMats = {};
function neonMat(color) { return neonMats[color] || (neonMats[color] = new THREE.MeshToonMaterial({ color, emissive: color, emissiveIntensity: 0.55, gradientMap: gradTex })); }
const discMats = {};
function checkerDiscMat(color) {   // ketastele kiirjas muster
  if (discMats[color]) return discMats[color];
  const cv = document.createElement('canvas'); cv.width = cv.height = 128; const x = cv.getContext('2d');
  x.fillStyle = '#ffffff'; x.fillRect(0, 0, 128, 128); x.fillStyle = '#e4e4ee';
  for (let i = 0; i < 12; i++) { x.beginPath(); x.moveTo(64, 64); x.arc(64, 64, 90, i / 12 * 6.283, (i + 0.5) / 12 * 6.283); x.fill(); }
  const tex = new THREE.CanvasTexture(cv); tex.colorSpace = THREE.SRGBColorSpace;
  return (discMats[color] = new THREE.MeshToonMaterial({ color, map: tex, gradientMap: gradTex }));
}
function checkerMat(color, w, d) {
  const key = color + ':' + Math.round(w) + 'x' + Math.round(d);
  if (checkMats[key]) return checkMats[key];
  const tex = new THREE.CanvasTexture(checkerImg); tex.wrapS = tex.wrapT = THREE.RepeatWrapping; tex.repeat.set(Math.max(1, Math.round(w / 2)), Math.max(1, Math.round(d / 2)));
  tex.colorSpace = THREE.SRGBColorSpace; tex.magFilter = THREE.NearestFilter;
  return (checkMats[key] = new THREE.MeshToonMaterial({ color, map: tex, gradientMap: gradTex }));
}
const bounceMat = new THREE.MeshToonMaterial({ color: 0xffa94d, emissive: 0xff6a00, emissiveIntensity: 0.3, gradientMap: gradTex });
const iceMat = new THREE.MeshToonMaterial({ color: 0xd8f3ff, emissive: 0x9fdcff, emissiveIntensity: 0.25, gradientMap: gradTex });
function shadowAll(o, cast = true, recv = true) { o.traverse(c => { if (c.isMesh && !c.isSprite) { c.castShadow = cast; c.receiveShadow = recv; } }); }

// taevas: värviüleminek + päike
const sky = new THREE.Mesh(new THREE.SphereGeometry(280, 32, 16), new THREE.ShaderMaterial({
  side: THREE.BackSide, depthWrite: false, fog: false,
  uniforms: { top: { value: new THREE.Color(0x2f7dff) }, mid: { value: new THREE.Color(HORIZON) }, bot: { value: new THREE.Color(0xffc8e6) }, sunDir: { value: new V3(14, 32, 10).normalize() } },
  vertexShader: 'varying vec3 vP; void main(){ vP = normalize(position); gl_Position = projectionMatrix * modelViewMatrix * vec4(position, 1.0); }',
  fragmentShader: 'uniform vec3 top; uniform vec3 mid; uniform vec3 bot; uniform vec3 sunDir; varying vec3 vP;\nvoid main(){ float h = vP.y; vec3 c = h > 0.0 ? mix(mid, top, pow(h, 0.55)) : mix(mid, bot, pow(-h, 0.6));\n float s = max(dot(normalize(vP), sunDir), 0.0); c += vec3(1.0, 0.92, 0.7) * (pow(s, 600.0) * 2.0 + pow(s, 12.0) * 0.18);\n gl_FragColor = vec4(c, 1.0);\n #include <colorspace_fragment>\n}',
}));
sky.renderOrder = -10; sky.frustumCulled = false; scene.add(sky);

// lainetav ja sätendav lima
const gooMat = new THREE.ShaderMaterial({
  uniforms: { t: { value: 0 }, fogC: { value: new THREE.Color(HORIZON) }, camPos: { value: new V3() }, nightF: { value: 0 } },
  vertexShader: 'uniform float t; varying vec3 vW; void main(){ vec4 w = modelMatrix * vec4(position, 1.0); w.y += sin(w.x * 0.35 + t * 1.3) * 0.22 + cos(w.z * 0.3 + t * 1.1) * 0.22; vW = w.xyz; gl_Position = projectionMatrix * viewMatrix * w; }',
  fragmentShader: 'uniform float t; uniform vec3 fogC; uniform vec3 camPos; uniform float nightF; varying vec3 vW;\nvoid main(){ float b = sin(vW.x * 0.35 + t * 1.3) + cos(vW.z * 0.3 + t * 1.1);\n vec3 c = mix(vec3(0.75, 0.08, 0.38), vec3(1.0, 0.38, 0.7), smoothstep(-0.6, 1.7, b));\n vec2 cell = floor(vW.xz * 1.2); float r = fract(sin(dot(cell, vec2(12.9898, 78.233))) * 43758.5453);\n c = mix(c, c * 0.3 + vec3(0.12, 0.0, 0.22), nightF);\n c += step(0.986, r) * (0.5 + 0.5 * sin(t * 4.0 + r * 40.0)) * (0.6 + nightF * 0.8);\n float d = length(vW.xz - camPos.xz); c = mix(c, fogC, smoothstep(45.0, 170.0, d));\n gl_FragColor = vec4(c, 1.0);\n #include <colorspace_fragment>\n}',
});
const goo = new THREE.Mesh(new THREE.PlaneGeometry(360, 360, 90, 90), gooMat);
goo.rotation.x = -Math.PI / 2; goo.position.y = -6; scene.add(goo);

// pilved ja hõljuvad saared
const cloudMat = new THREE.MeshToonMaterial({ color: 0xffffff, gradientMap: gradTex, emissive: 0xffffff, emissiveIntensity: 0.25 });
for (let i = 0; i < 30; i++) {
  const g = new THREE.Group();
  for (let k = 0; k < 4; k++) { const s = new THREE.Mesh(new THREE.SphereGeometry(1.6 + Math.random() * 2.2, 12, 9), cloudMat); s.position.set(k * 2.3 - 3, Math.random() * 1.2, Math.random() * 1.5); s.scale.y = 0.7; g.add(s); }
  const side = Math.random() < 0.5 ? -1 : 1;
  g.position.set(side * (28 + Math.random() * 50), 10 + Math.random() * 25, 30 - Math.random() * 170); g.userData.drift = 0.3 + Math.random() * 0.6; scene.add(g);
  g.userData.cloud = true;
}
for (let i = 0; i < 12; i++) {
  const g = new THREE.Group();
  const r = 4 + Math.random() * 5;
  const rock = new THREE.Mesh(new THREE.ConeGeometry(r, r * 1.6, 7), mat(0xa47551)); rock.rotation.x = Math.PI; rock.position.y = -r * 0.8; g.add(rock);
  const grass = new THREE.Mesh(new THREE.CylinderGeometry(r * 1.02, r, 0.8, 7), mat(0x6bd36b)); g.add(grass);
  for (let k = 0; k < 3; k++) {
    const tr = new THREE.Group(); const a = Math.random() * 6.28, rr = Math.random() * r * 0.6; tr.position.set(Math.cos(a) * rr, 0.4, Math.sin(a) * rr);
    const trunk = new THREE.Mesh(new THREE.CylinderGeometry(0.25, 0.3, 1.6, 6), mat(0x8b5a3c)); trunk.position.y = 0.8; tr.add(trunk);
    const crown = new THREE.Mesh(new THREE.ConeGeometry(1.3, 2.6, 7), mat([0x40c057, 0x2f9e44, 0x94d82d][k])); crown.position.y = 2.6; tr.add(crown);
    g.add(tr);
  }
  const side = i % 2 ? -1 : 1;
  g.position.set(side * (45 + Math.random() * 40), -2 + Math.random() * 16, 20 - i * 12 - Math.random() * 6); g.rotation.y = Math.random() * 6;
  g.userData.bobY = g.position.y; g.userData.bobP = Math.random() * 6; g.userData.island = true; scene.add(g);
}
const floaters = scene.children.filter(o => o.userData.cloud || o.userData.island);
function updateScenery(dt, t) {
  for (const o of floaters) {
    if (o.userData.cloud) { o.position.x += o.userData.drift * dt * Math.sign(o.position.x); if (Math.abs(o.position.x) > 110) o.position.x = Math.sign(o.position.x) * 28; }
    else o.position.y = o.userData.bobY + Math.sin(t * 0.4 + o.userData.bobP) * 0.6;
  }
}

// osakesed: tolmupahvakud ja limapritsmed
const puffTex = (() => { const cv = document.createElement('canvas'); cv.width = cv.height = 64; const x = cv.getContext('2d'); const gr = x.createRadialGradient(32, 32, 2, 32, 32, 30); gr.addColorStop(0, 'rgba(255,255,255,1)'); gr.addColorStop(1, 'rgba(255,255,255,0)'); x.fillStyle = gr; x.fillRect(0, 0, 64, 64); return new THREE.CanvasTexture(cv); })();
const puffs = [];
for (let i = 0; i < 48; i++) {
  const sp = new THREE.Sprite(new THREE.SpriteMaterial({ map: puffTex, transparent: true, depthWrite: false, opacity: 0 }));
  sp.visible = false; scene.add(sp); puffs.push({ sp, life: 0, max: 1, vel: new V3(), grow: 1 });
}
let puffI = 0;
function puff(pos, color, n = 6, speed = 1.2, size = 0.5, up = 0.6) {
  if (gfx !== 'ilus' && n > 3) n = 3;
  for (let i = 0; i < n; i++) {
    const p = puffs[puffI++ % puffs.length];
    p.sp.position.set(pos.x + rnd(-0.2, 0.2), pos.y + 0.1, pos.z + rnd(-0.2, 0.2));
    const a = Math.random() * 6.28;
    p.vel.set(Math.cos(a) * speed, up + Math.random() * up, Math.sin(a) * speed);
    p.life = p.max = rnd(0.4, 0.8); p.grow = size; p.sp.material.color.setHex(color); p.sp.scale.setScalar(size * 0.4); p.sp.visible = true;
  }
}
function updatePuffs(dt) {
  for (const p of puffs) {
    if (p.life <= 0) continue;
    p.life -= dt; p.sp.position.addScaledVector(p.vel, dt); p.vel.multiplyScalar(Math.exp(-3 * dt)); p.vel.y -= dt * 0.5;
    const k = 1 - p.life / p.max; p.sp.scale.setScalar(p.grow * (0.4 + k * 1.2)); p.sp.material.opacity = Math.max(0, 0.8 * (1 - k));
    if (p.life <= 0) p.sp.visible = false;
  }
}
function setGfx(q) {
  gfx = q; try { localStorage.setItem('kp_gfx', q); } catch (e) {}
  renderer.shadowMap.enabled = q === 'ilus';
  renderer.setPixelRatio(q === 'ilus' ? Math.min(devicePixelRatio, 2) : 1);
  scene.traverse(o => { if (o.material) { if (Array.isArray(o.material)) o.material.forEach(m => m.needsUpdate = true); else o.material.needsUpdate = true; } });
}

// =====================================================================
// MÄNGU OLEK (jagatud; peremees otsustab)
// =====================================================================
const net = { on: false, id: 'mina', room: null, isHost: true, hostId: 'mina', offset: 0, hasOffset: false, peers: new Map() };
const T = () => net.isHost ? now() : now() + net.offset;
const show = { sid: '', stage: 'lobby', rn: 0, rt: 'lobby', ti: 0, rid: '', sa: 0, ea: 0, part: [], alive: [], inR: [], q: [], fo: [],
  tails: [], winner: '', easy: false, crowns: {}, final: false, lastOut: [], quota: 0, ready: 0 };
const hostOnly = { padT: 0, falls: {}, immune: {}, prevType: '', tailTimer: 0, grabbedAt: {}, ballPos: new THREE.Vector3(), ballV: new THREE.Vector3(), goalT: 0, eggs: [], carry: {}, stealCd: {} };
const S = () => show.easy ? 0.65 : (show.mode === 'levels' || show.mode === 'coop' || show.stage === 'trenn' || show.stage === 'lobby') ? 1 : (show.rt === 'race' ? 1.08 + Math.min(0.08, Math.max(0, (show.rn || 1) - 1) * 0.04) : 1.1 + Math.min(0.21, Math.max(0, (show.rn || 1) - 1) * 0.07)) * (show.tw === 'turbo' ? 1.25 : 1);
// vooru keerdkäigud (kõigil sama, tuleb juhilt show.tw kaudu)
const TWISTS = { kuu: '🌙 Kuugravitatsioon – hüppad kõrgele!', turbo: '⚡ Turbo – takistused on eriti kiired!', libe: '🧊 Libe põrand – kõik libiseb!', hiid: '🦍 Hiiglased – kõik on suured!', tuul: '🌪️ Tuuletorm – tuul lükkab külje pealt!', hupe: '🦘 Superhüpe – hüppa üle kõige!' };
const twOn = k => show.tw === k && show.mode !== 'levels' && show.mode !== 'coop' && show.stage !== 'lobby' && show.stage !== 'trenn';
const twG = () => twOn('kuu') ? 0.5 : 1, twJ = () => twOn('hupe') ? 1.4 : 1;
function twistLine() { return show.tw && TWISTS[show.tw] ? 'Üllatus: ' + TWISTS[show.tw] : ''; }
const SURVIVAL = { areen: 1, plaadid: 1, noor: 1, boss: 1 };
const TEAM = { pall: 1, munad: 1 };
const TEAM_COLORS = [0x2f9bff, 0xff3b3b], TEAM_NAMES = ['Sinine', 'Punane'];
let myName = 'Mängija', loco = 'arms', roomCode = '', musicOn = true, wantEasy = false, wantBots = true;
const HUMAN_COLORS = [0xff3d8b, 0x2f9bff, 0x22c55e, 0xf59e0b, 0x8b5cf6, 0x06b6d4, 0xef4444, 0xfacc15];
const HATS = ['Puudub', 'Müts', 'Kõrvad', 'Sarv', 'Lill', 'Propeller', 'Võlurimüts', 'Kokamüts', 'Viikingisarved'];
const FREE_HATS = 6;
const COSTUMES = ['Puudub', 'Dinosaurus', 'Astronaut', 'Kass', 'Pingviin'];
const SHOP = [
  { id: 'h6', type: 'hat', val: 6, name: 'Võlurimüts', price: 30 }, { id: 'h7', type: 'hat', val: 7, name: 'Kokamüts', price: 30 }, { id: 'h8', type: 'hat', val: 8, name: 'Viikingisarved', price: 40 },
  { id: 'cGold', type: 'color', val: 0xffc400, name: 'Kuldne värv', price: 25 }, { id: 'cBlack', type: 'color', val: 0x2b2b3a, name: 'Must värv', price: 20 }, { id: 'cWhite', type: 'color', val: 0xf1f3f5, name: 'Valge värv', price: 20 },
  { id: 'k1', type: 'costume', val: 1, name: 'Dinosaurus', price: 80 }, { id: 'k2', type: 'costume', val: 2, name: 'Astronaut', price: 80 },
  { id: 'k3', type: 'costume', val: 3, name: 'Kass', price: 60 }, { id: 'k4', type: 'costume', val: 4, name: 'Pingviin', price: 100 },
];
let myColor = HUMAN_COLORS[Math.floor(Math.random() * HUMAN_COLORS.length)], myHat = 1, myCostume = 0;
let coins = 0, owned = [], prog = { shows: 0, crowns: 0, rounds: 0, qual: 0, best: {} };
try {
  coins = parseInt(localStorage.getItem('kp_coins') || '0') || 0;
  owned = JSON.parse(localStorage.getItem('kp_owned') || '[]') || [];
  myCostume = parseInt(localStorage.getItem('kp_costume') || '0') || 0;
  prog = Object.assign(prog, JSON.parse(localStorage.getItem('kp_stats') || '{}'));
  const h = localStorage.getItem('kp_hat'); if (h !== null) myHat = parseInt(h) || 0;
  const c = localStorage.getItem('kp_color'); if (c !== null) myColor = parseInt(c) || myColor;
} catch (e) {}
const ownsHat = i => i < FREE_HATS || owned.includes('h' + i);
const PETS = ['Puudub', 'Kutsikas', 'Kassipoeg', 'Robotike', 'Tibu'];
let myPet = 0;
try { myPet = parseInt(localStorage.getItem('kp_pet') || '0') || 0; } catch (e) {}
SHOP.push(
  { id: 'p1', type: 'pet', val: 1, name: 'Lemmik: Kutsikas', price: 60, lv: 2 }, { id: 'p2', type: 'pet', val: 2, name: 'Lemmik: Kassipoeg', price: 60, lv: 2 },
  { id: 'p3', type: 'pet', val: 3, name: 'Lemmik: Robotike', price: 80, lv: 3 }, { id: 'p4', type: 'pet', val: 4, name: 'Lemmik: Tibu', price: 50, lv: 2 });
SHOP.forEach(it => { if (it.type === 'costume') it.lv = it.lv || 3; if (it.id === 'h8') it.lv = 4; });
if (myPet && !owned.includes('p' + myPet)) myPet = 0;
prog.xp = prog.xp || 0;
const levelOf = xp => 1 + Math.floor(Math.sqrt(xp / 40));
const myLevel = () => levelOf(prog.xp);
function addXp(n) {
  const before = myLevel(); prog.xp += n; saveProg();
  if (myLevel() > before) { setTimeout(() => { sfx('fanfare'); showToast('TASE ' + myLevel() + '! 🎉 Uued asjad poes', 2.5); }, 400); }
}
const LEAGUES = [['Pronks', '🥉', 0], ['Hõbe', '🥈', 60], ['Kuld', '🥇', 150], ['Plaatina', '💠', 300], ['Teemant', '💎', 500], ['Meister', '👑', 800]];
function leagueOf(tr) { let k = 0; LEAGUES.forEach((L, i) => { if (tr >= L[2]) k = i; }); return k; }
function myLeague() { return leagueOf(prog.tr || 0); }
function leagueText() { const k = myLeague(), L = LEAGUES[k], nx = LEAGUES[k + 1]; return L[1] + ' ' + L[0] + ' liiga · 🏆 ' + (prog.tr || 0) + (nx ? ' (' + nx[0] + ' ' + nx[2] + ')' : ''); }
function addTrophies(n) {
  const before = myLeague(), floor = LEAGUES[before][2];
  prog.tr = Math.max(floor, (prog.tr || 0) + n); saveProg();   // liigast alla ei kuku
  if (n > 0) setTimeout(() => showToast('+' + n + ' 🏆 karikat', 1.4), 1600);
  const after = myLeague();
  if (after > before) setTimeout(() => { sfx('fanfare'); burst(player.pos); showMsg(LEAGUES[after][1] + ' ' + LEAGUES[after][0] + ' liiga!', ['Tõusid uude liigasse!', '+' + after * 50 + ' 🪙 boonust'], 4); addCoins(after * 50, 'uus liiga!'); }, 2500);
}
const dayKey = () => new Date().toDateString();
let dailyMsg = '';
function dailyLogin() {   // iga päev järjest mängides kasvab boonus
  const today = dayKey(); if (prog.lastDay === today) return;
  const y = new Date(Date.now() - 864e5).toDateString();
  prog.streak = prog.lastDay === y ? (prog.streak || 0) + 1 : 1; prog.lastDay = today;
  const bonus = prog.streak % 7 === 0 ? 120 : 10 + Math.min(prog.streak, 6) * 10;
  coins += bonus; saveProg();
  dailyMsg = '🔥 ' + prog.streak + '. päev järjest! +' + bonus + ' 🪙' + (prog.streak % 7 === 0 ? ' (nädala SUURBOONUS!)' : ' · homme rohkem');
}
function showDailyMsg() { if (!dailyMsg) return; const m = dailyMsg; dailyMsg = ''; setTimeout(() => { sfx('fanfare'); showToast(m, 3.5); }, 1200); }
function dailyDesc() { const d = prog.daily && prog.daily.day === dayKey() ? prog.daily : null; return d ? 'Tänane rekord ' + d.t + ' s – löö see üle!' : 'Iga päev uus rada · esimene läbimine +40 🪙'; }
const QUEST_POOL = [
  { id: 'coin', text: 'Korja 20 münti', goal: 20 }, { id: 'qual', text: 'Pääse 3 korda edasi', goal: 3 }, { id: 'fin', text: 'Jõua 2 korda finišisse', goal: 2 },
  { id: 'throw', text: 'Viska kedagi 3 korda', goal: 3 }, { id: 'crown', text: 'Võida kroon', goal: 1 },
  { id: 'carry', text: 'Tassi kedagi 2 korda', goal: 2 }, { id: 'pad', text: 'Vajuta 2 nuppu (boss/värav)', goal: 2 },
];
function ensureQuests() {
  const today = new Date().toDateString();
  if (prog.day === today && Array.isArray(prog.quests) && prog.quests.length && prog.quests.every(q => QUEST_POOL.some(x => x.id === q.id))) return;
  const pool = QUEST_POOL.slice().sort(() => Math.random() - 0.5).slice(0, 3);
  prog.day = today; prog.quests = pool.map(q => ({ id: q.id, n: 0, done: false })); saveProg();
}
ensureQuests();
try { dailyLogin(); } catch (e) {}
function questText(q) { const d = QUEST_POOL.find(x => x.id === q.id); return d ? (q.done ? '✓ ' : '') + d.text + ' (' + Math.min(q.n, d.goal) + '/' + d.goal + ')' : ''; }
function questTick(id, n = 1) {
  ensureQuests();
  const q = prog.quests.find(x => x.id === id && !x.done); if (!q) return;
  const d = QUEST_POOL.find(x => x.id === id); q.n += n;
  if (q.n >= d.goal) { q.done = true; setTimeout(() => { addCoins(20, 'ülesanne tehtud!'); addXp(50); showToast('✅ ' + d.text + ' – tehtud! +20 🪙', 2.5); }, 700); }
  saveProg();
}
if (!ownsHat(myHat)) myHat = 1;
if (myCostume && !owned.includes('k' + myCostume)) myCostume = 0;
function saveProg() {
  try {
    localStorage.setItem('kp_coins', String(coins)); localStorage.setItem('kp_owned', JSON.stringify(owned));
    localStorage.setItem('kp_costume', String(myCostume)); localStorage.setItem('kp_stats', JSON.stringify(prog));
    localStorage.setItem('kp_hat', String(myHat)); localStorage.setItem('kp_color', String(myColor)); localStorage.setItem('kp_pet', String(typeof myPet === 'number' ? myPet : 0));
  } catch (e) {}
}
function addCoins(n, why) { coins += n; saveProg(); showToast('+' + n + ' 🪙 ' + (why || ''), 1.6); sfx('coin'); }
function shopState(it) {
  const has = owned.includes(it.id);
  const using = has && ((it.type === 'hat' && myHat === it.val) || (it.type === 'color' && myColor === it.val) || (it.type === 'costume' && myCostume === it.val) || (it.type === 'pet' && myPet === it.val));
  return { has, using };
}
function shopClick(it) {
  const st = shopState(it);
  if (!st.has) {
    if (it.lv && myLevel() < it.lv) { showToast('Vaja taset ' + it.lv + ' (sul ' + myLevel() + ')', 1.6); sfx('hit'); return false; }
    if (coins < it.price) { showToast('Vaja ' + it.price + ' 🪙 – sul on ' + coins, 1.6); sfx('hit'); return false; }
    coins -= it.price; owned.push(it.id); sfx('fanfare');
  }
  if (it.type === 'hat') myHat = st.using ? 1 : it.val;
  else if (it.type === 'color') myColor = st.using ? HUMAN_COLORS[0] : it.val;
  else if (it.type === 'pet') myPet = st.using ? 0 : it.val;
  else myCostume = st.using ? 0 : it.val;
  saveProg(); if (typeof renderPickers === 'function') renderPickers(); if (typeof renderShop === 'function') renderShop();
  return true;
}
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
  coin: () => { tone(988, 988, 0.08, 'square', 0.12); tone(1319, 1319, 0.18, 'square', 0.12, 0.07); },
  goal: () => { noise(1.2, 0.25, 2000); [523, 784, 1047].forEach((f, i) => tone(f, f, 0.25, 'square', 0.16, i * 0.12)); },
  build: () => tone(320, 200, 0.07, 'square', 0.14),
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
  return { kind, name, solids: [], movers: [], sweepers: [], tiles: [], balls: [], pendulums: [], spinners: [], convs: [], blinks: [], rollers: [], winds: [], fans: [], coins: [], boxes: [], gates: [], pads: [], cps: [], wps: [], finishZ: -90,
    outY: -6, gooY: -6, spawn: raceSpawn, center: new V3(0, 0, -12) };
}
function raceSpawn(i, n, out) { out.set(-4 + (i % 5) * 2, 0.3, 1.0 + Math.floor(i / 5) * 1.3); }
let geoCache = new Map();
function rbox(w, h, d) {   // ümarate servadega kast (jagatud geomeetria)
  const k = w + ',' + h + ',' + d; let g = geoCache.get(k);
  if (!g) { const r = Math.min(0.16, Math.min(w, h, d) * 0.22); g = new RoundedBoxGeometry(w, h, d, 2, r); geoCache.set(k, g); }
  return g;
}
function box(x, top, z, w, h, d, color, opts = {}) {
  let material = opts.bounce ? bounceMat : opts.ice ? iceMat : (w >= 5.5 && d >= 5.5 ? checkerMat(color, w, d) : mat(color));
  if (opts.conveyor) material = conveyorMat(w, d);
  if (TR.night && !opts.bounce && !opts.conveyor) material = neonMat(color);
  const m = new THREE.Mesh(opts.flat ? new THREE.BoxGeometry(w, h, d) : rbox(w, h, d), material);
  m.position.set(x, top - h / 2, z); trackGroup.add(m);
  m.castShadow = true; m.receiveShadow = true;
  const s = Object.assign({ type: 'box', mesh: m, hw: w / 2, hh: h / 2, hd: d / 2, vel: new V3(), delta: new V3(), active: true }, opts);
  if (s.move) { s.move.base = m.position[s.move.axis]; s.move.init = false; TR.movers.push(s); }
  if (s.conveyor && !s.roller) { TR.convs.push({ s, tex: material.map, len: d }); }
  if (s.blink) { s.blink.left = 9; TR.blinks.push(s); }
  if (s.roller) {   // pöörlev rull: kokkupõrkekast on nähtamatu, näha on silinder
    m.material = new THREE.MeshBasicMaterial({ visible: false });
    const rl = new THREE.Mesh(new THREE.CylinderGeometry(h / 2, h / 2, w, 24), mat(color)); rl.rotation.z = Math.PI / 2; m.add(rl);
    for (let i = 0; i < 6; i++) { const st = new THREE.Mesh(new THREE.BoxGeometry(w * 1.01, 0.08, 0.12), mat(0xffffff)); const a = i / 6 * Math.PI * 2; st.position.set(0, Math.cos(a) * h / 2, Math.sin(a) * h / 2); st.rotation.x = a; rl.add(st); }
    rl.castShadow = true; TR.rollers.push({ s, rl });
  }
  if (s.bounce) {
    const ring = new THREE.Mesh(new THREE.TorusGeometry(Math.min(w, d) * 0.36, 0.09, 8, 28), mat(0xffffff)); ring.rotation.x = Math.PI / 2; ring.position.y = h / 2 + 0.03; m.add(ring);
    const dot = new THREE.Mesh(new THREE.CylinderGeometry(Math.min(w, d) * 0.16, Math.min(w, d) * 0.16, 0.04, 20), mat(0xffffff)); dot.position.y = h / 2 + 0.02; m.add(dot);
  }
  if (s.pusher) {   // hoiatustriibud
    for (let i = -1; i <= 1; i += 2) { const st = new THREE.Mesh(new THREE.BoxGeometry(w * 1.01, h * 0.12, d * 1.01), mat(0xffd43b)); st.position.y = i * h * 0.28; m.add(st); }
  }
  TR.solids.push(s); return s;
}
function cyl(x, top, z, r, h, color, opts = {}) {
  const m = new THREE.Mesh(new THREE.CylinderGeometry(r, r, h, 48), TR.night ? neonMat(color) : r > 3 ? checkerDiscMat(color) : mat(color));
  m.position.set(x, top - h / 2, z); trackGroup.add(m);
  m.castShadow = true; m.receiveShadow = true;
  if (r > 2) { const rim = new THREE.Mesh(new THREE.TorusGeometry(r - 0.05, 0.13, 8, 64), mat(0xffffff)); rim.rotation.x = Math.PI / 2; rim.position.y = h / 2; m.add(rim); }
  const s = Object.assign({ type: 'cyl', mesh: m, r, hh: h / 2, vel: new V3(), delta: new V3(), active: true, ang: 0, spinD: 0 }, opts);
  if (s.spin) { TR.spinners.push(s); for (let i = 0; i < 4; i++) { const a = i / 4 * Math.PI * 2; const ar = new THREE.Mesh(new THREE.ConeGeometry(0.3, 0.6, 3), mat(0xffffff)); ar.rotation.set(-Math.PI / 2, 0, (s.spin > 0 ? 1 : -1) * Math.PI / 2 - a); ar.position.set(Math.cos(a) * r * 0.62, h / 2 + 0.03, -Math.sin(a) * r * 0.62); ar.scale.set(1, 1, 0.15); m.add(ar); } }
  TR.solids.push(s); return s;
}
function ramp(x, w, zTop, zBot, yTop, yBot, color) {   // liumägi: kõrgem ots zTop juures
  const len = Math.hypot(zTop - zBot, yTop - yBot), ang = Math.atan2(yTop - yBot, zTop - zBot);
  const m = new THREE.Mesh(new THREE.BoxGeometry(w, 0.4, len), iceMat);
  m.position.set(x, (yTop + yBot) / 2 - 0.2, (zTop + zBot) / 2); m.rotation.x = ang; m.receiveShadow = true; trackGroup.add(m);
  [-1, 1].forEach(sd => { const rail = new THREE.Mesh(new THREE.BoxGeometry(0.2, 0.5, len), mat(color)); rail.position.set(sd * (w / 2 + 0.1), 0.15, 0); m.add(rail); });
  const s = { type: 'ramp', mesh: m, x, hw: w / 2, zTop, zBot, yTop, yBot, ice: true, ramp: true, active: true, vel: new V3(), delta: new V3(), hh: 0 };
  TR.solids.push(s); return s;
}
function rampTop(s, z) { const t = (z - s.zBot) / (s.zTop - s.zBot); return s.yBot + (s.yTop - s.yBot) * t; }
function inRamp(s, x, z, pad = 0.12) { return Math.abs(x - s.x) <= s.hw + pad && z <= Math.max(s.zTop, s.zBot) && z >= Math.min(s.zTop, s.zBot); }
function fan(x, z, dir) {
  const g = new THREE.Group(); g.position.set(x, 1.6, z); g.rotation.y = dir > 0 ? Math.PI / 2 : -Math.PI / 2;
  const ring = new THREE.Mesh(new THREE.TorusGeometry(1.1, 0.15, 8, 24), mat(0xdee2e6)); g.add(ring);
  const blades = new THREE.Group(); g.add(blades);
  for (let i = 0; i < 3; i++) { const b = new THREE.Mesh(new THREE.BoxGeometry(0.25, 1.9, 0.05), mat(0x74c0fc)); b.rotation.z = i / 3 * Math.PI * 2; blades.add(b); }
  const post = new THREE.Mesh(new THREE.CylinderGeometry(0.12, 0.12, 1.6, 8), mat(0xadb5bd)); post.position.y = -1.2; g.add(post);
  shadowAll(g, true, false); trackGroup.add(g); TR.fans.push(blades);
}
function sweeper(x, z, baseTop, L, omega, bars = 1, phase = 0, opts = {}) {
  const height = opts.height || 0.55;
  const g = new THREE.Group(); g.position.set(x, baseTop + height, z); trackGroup.add(g);
  if (opts.high) TR.hasHigh = true;
  for (let k = 0; k < bars; k++) {
    const arm = new THREE.Group(); arm.rotation.y = k * Math.PI / bars; g.add(arm);
    const bar = new THREE.Mesh(new THREE.CylinderGeometry(0.22, 0.22, L * 2, 16), mat(opts.high ? 0xffd43b : 0xff3b3b));
    bar.rotation.z = Math.PI / 2; arm.add(bar);
    for (let i = -3; i <= 3; i++) { if (!i) continue; const st = new THREE.Mesh(new THREE.CylinderGeometry(0.235, 0.235, 0.4, 16), mat(0xffffff)); st.rotation.z = Math.PI / 2; st.position.x = i * L / 3.5; arm.add(st); }
  }
  const sw = { x, z, y: baseTop + height, baseTop, L, omega, bars, phase, a: 0, group: g, active: true, angleFn: null, activeFn: null, high: !!opts.high };
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
const FLAG_COLORS = [0xff3d8b, 0xffd43b, 0x2f9bff, 0x22c55e, 0xff922b, 0x8b5cf6];
function bunting(g, x0, x1, y, sag = 0.5) {   // lipuvanik
  const n = Math.round((x1 - x0) / 0.6);
  for (let i = 0; i <= n; i++) {
    const u = i / n, x = x0 + (x1 - x0) * u, yy = y - Math.sin(u * Math.PI) * sag;
    const f = new THREE.Mesh(new THREE.ConeGeometry(0.2, 0.42, 3), mat(FLAG_COLORS[i % FLAG_COLORS.length]));
    f.rotation.z = Math.PI; f.rotation.y = Math.PI / 6; f.position.set(x, yy - 0.2, 0); g.add(f);
  }
}
function arch(z, top, color) {
  const g = new THREE.Group();
  [-5, 5].forEach(x => {
    const p = new THREE.Mesh(new THREE.CylinderGeometry(0.32, 0.4, 4, 14), mat(color)); p.position.set(x, 2, 0); g.add(p);
    const b = new THREE.Mesh(new THREE.SphereGeometry(0.5, 14, 10), mat(0xffffff)); b.position.set(x, 4.6, 0); g.add(b);
  });
  const t = new THREE.Mesh(new RoundedBoxGeometry(10.5, 0.8, 0.6, 2, 0.2), mat(color)); t.position.y = 4.2; g.add(t);
  bunting(g, -4.6, 4.6, 3.6, 0.5);
  shadowAll(g, true, false);
  g.position.set(0, top, z); trackGroup.add(g);
}
const BALLOON_COLORS = [0xff3d8b, 0xffd43b, 0x2f9bff, 0x22c55e, 0xff922b, 0x8b5cf6, 0xffffff];
function balloon(x, y, z) {
  const g = new THREE.Group();
  const c = BALLOON_COLORS[Math.floor(Math.random() * BALLOON_COLORS.length)];
  const b = new THREE.Mesh(new THREE.SphereGeometry(0.5, 16, 12), mat(c)); b.scale.y = 1.18; g.add(b);
  const knot = new THREE.Mesh(new THREE.ConeGeometry(0.09, 0.14, 8), mat(c)); knot.position.y = -0.62; g.add(knot);
  const str = new THREE.Mesh(new THREE.CylinderGeometry(0.01, 0.01, 1.6, 4), mat(0xffffff)); str.position.y = -1.45; g.add(str);
  g.position.set(x, y, z); g.userData.bob = Math.random() * 6; g.userData.baseY = y; trackGroup.add(g);
}
function decorate(rt) {
  let z0 = 6, z1 = -100, w = 8;
  if (rt === 'race') z1 = TR.finishZ - 6;
  else if (rt === 'lobby') { z0 = 9; z1 = -9; w = 9.5; }
  else { z0 = 2; z1 = -26; w = 11; }
  const n = rt === 'race' ? 18 : 10;
  for (let i = 0; i < n; i++) {
    const side = i % 2 ? -1 : 1;
    balloon(side * (w + Math.random() * 4), 2 + Math.random() * 5, z0 + (z1 - z0) * (i / n) + rnd(-2, 2));
  }
  if (rt === 'lobby') {
    const g = new THREE.Group(); bunting(g, -7.8, 7.8, 4.5, 0.8); g.position.z = 7.8; trackGroup.add(g);
    const g2 = new THREE.Group(); bunting(g2, -7.8, 7.8, 5.2, 0.8); g2.position.z = -8.2; trackGroup.add(g2);
    [[-7.8, -7.8], [7.8, -7.8], [-7.8, 7.8], [7.8, 7.8]].forEach(([x, z]) => { const p = new THREE.Mesh(new THREE.CylinderGeometry(0.15, 0.15, 5.4, 8), mat(0xffffff)); p.position.set(x, 2.7, z); trackGroup.add(p); });
  }
}
function crownMesh(scale = 1) {
  const crown = new THREE.Group();
  const band = new THREE.Mesh(new THREE.CylinderGeometry(0.6, 0.6, 0.4, 24, 1, true), new THREE.MeshToonMaterial({ color: 0xffc400, side: THREE.DoubleSide, gradientMap: gradTex, emissive: 0x996600, emissiveIntensity: 0.3 }));
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
  { name: 'Pendlirada', build() {
    box(0, 0, 0, 10, 1, 10, C.green); arch(4.5, 0, C.red);
    box(0, 0, -14, 3, 1, 18, C.blue);
    pendulum(-9, 1.6, 0); pendulum(-14, 1.9, 2.1); pendulum(-19, 1.7, 4.0);
    box(0, 0, -27, 8, 1, 6, C.purple);
    cyl(0, 0, -35, 3.5, 1, C.yellow, { spin: 0.8 }); cyl(0, 0, -42.5, 3.5, 1, C.pink, { spin: -0.9 }); cyl(0, 0, -50, 3.5, 1, C.green, { spin: 1.0 });
    box(0, 0, -57, 8, 1, 4, C.purple);
    box(0, 0, -68, 6, 1, 18, 0x9aa5b1, { conveyor: 2.0, flat: true });
    box(0, 1.2, -64, 1.6, 1.2, 1.2, C.red, { move: { axis: 'x', amp: 2, speed: 1.5, phase: 0 }, pusher: true });
    box(0, 1.2, -71, 1.6, 1.2, 1.2, C.red, { move: { axis: 'x', amp: 2, speed: 1.5, phase: 2 }, pusher: true });
    box(0, 0, -82, 10, 1, 8, C.yellow);
    TR.finishZ = -79; finishDeco(-79, 0);
    cp(99, 0, 0, 1.5); cp(-24.5, 0, 0, -27); cp(-55.5, 0, 0, -57);
    TR.wps = [
      { x: 0, z: -6 }, { x: 0, z: -11.5 }, { x: 0, z: -16.5 }, { x: 0, z: -22 }, { x: 0, z: -27, wide: 2 },
      { x: 0, z: -33 }, { x: 0, z: -40 }, { x: 0, z: -48 }, { x: 0, z: -55.5, wide: 2 },
      { x: 0, z: -62, wide: 1.5 }, { x: 0, z: -75, wide: 1.5 }, { x: 0, z: -83, wide: 3 },
    ];
  } },
  { name: 'Taevatrepp', build() {
    box(0, 0, 0, 10, 1, 10, C.green); arch(4.5, 0, C.yellow);
    box(0, 0, -7, 4, 0.5, 4, C.orange, { bounce: true });
    box(0, 3, -12, 6, 3.2, 5, C.purple);
    cyl(0, 3, -18.5, 3.5, 1, C.yellow, { spin: 0.9 });
    box(0, 3, -27, 1.6, 1, 8, C.blue);
    sweeper(0, -27, 3, 3, 1.2, 1);
    tileGrid(-0.95, -33.5, 2, 3, 3.6); tileGrid(-0.95, -39.2, 2, 3, 4.2);
    box(0, 4.8, -47, 6, 1, 6, C.purple);
    box(0, 4.8, -52, 3, 0.5, 3, C.orange, { bounce: true });
    box(0, 7.5, -59, 10, 3, 10, C.yellow);
    TR.finishZ = -56.5; finishDeco(-56.5, 7.5);
    cp(99, 0, 0, 1.5); cp(-10, 0, 3, -12.5); cp(-23.5, 0, 3, -24); cp(-44.5, 0, 4.8, -47);
    TR.wps = [
      { x: 0, z: -6 }, { x: 0, z: -7.5 }, { x: 0, z: -12, wide: 1.5 }, { x: 0, z: -18.5 }, { x: 0, z: -23.5 }, { x: 0, z: -31 },
      { x: 0, z: -34 }, { x: 0, z: -42 }, { x: 0, z: -47, wide: 1.5 }, { x: 0, z: -52 }, { x: 0, z: -60, wide: 3 },
    ];
  } },
];
TRACKS.push(
  { name: 'Tuulerada', build() {
    box(0, 0, 0, 10, 1, 10, C.green); arch(4.5, 0, 0x74c0fc);
    box(0, 0, -14, 8, 1, 18, C.blue);
    TR.winds.push({ x: 0, hw: 4.2, z0: -5.5, z1: -22.5, amp: 2.6, speed: 0.7 });
    fan(-5.6, -9, 1); fan(5.6, -14, -1); fan(-5.6, -19, 1);
    box(0, 0, -26.5, 8, 1, 6, C.purple);
    const bl = [[-1.5, -33, 0], [1.5, -36.5, -0.8], [-1.5, -40, -1.6], [1.5, -43.5, -2.4], [0, -47, -3.2]].map(([x, z, ph], i) => box(x, 0, z, 3, 1, 3, [C.pink, C.yellow, C.green, C.orange, C.purple][i], { blink: { period: 3.2, phase: ph } }));
    box(0, 0, -58, 6, 1, 18, 0x9aa5b1);
    [-52.5, -58, -63.5].forEach(z => box(0, 1.0, z, 6, 1.0, 1.0, C.orange, { conveyor: 2, flat: true, roller: true }));
    box(-3.4, 1.4, -55.2, 1.6, 1.2, 1.0, C.red, { move: { axis: 'x', amp: 1.8, speed: 2.2, phase: 0 }, pusher: true });
    box(3.4, 1.4, -60.8, 1.6, 1.2, 1.0, C.red, { move: { axis: 'x', amp: 1.8, speed: 2.2, phase: 1.5 }, pusher: true });
    box(0, 0, -69, 4, 0.5, 4, C.orange, { bounce: true });
    box(0, 4, -73.5, 6, 1, 5, C.purple);
    ramp(0, 5, -76, -88, 4, 0, C.pink);
    box(0, 0, -93, 10, 1, 10, C.yellow);
    TR.finishZ = -90; finishDeco(-90, 0);
    cp(99, 0, 0, 1.5); cp(-24, 0, 0, -26.5); cp(-49.2, 0, 0, -50.5); cp(-71.5, 0, 4, -73.5);
    TR.wps = [
      { x: 0, z: -6 }, { x: 0, z: -14, wide: 1 }, { x: 0, z: -22 }, { x: 0, z: -27, wide: 2 },
      { blink: bl[0], x: -1.5, z: -33 }, { blink: bl[1], x: 1.5, z: -36.5 }, { blink: bl[2], x: -1.5, z: -40 }, { blink: bl[3], x: 1.5, z: -43.5 }, { blink: bl[4], x: 0, z: -47 },
      { x: 0, z: -51, wide: 1 }, { x: 0, z: -66, wide: 1 }, { x: 0, z: -69 }, { x: 0, z: -73.5 }, { x: 0, z: -86 }, { x: 0, z: -94, wide: 3 },
    ];
  } },
  { name: 'Öörada', build() {
    TR.night = true;
    box(0, 0, 0, 10, 1, 10, 0x22d3ee); arch(4.5, 0, 0xff3dd6);
    box(-1.5, 0, -8, 3, 1, 3, 0xff3dd6); box(1.5, 0, -11.5, 3, 1, 3, 0x7c3aed); box(-1.5, 0, -15, 3, 1, 3, 0x22d3ee); box(1.5, 0, -18.5, 3, 1, 3, 0xfacc15);
    box(0, 0, -24, 6, 1, 5, 0xa3e635);
    cyl(0, 0, -31.5, 4, 1, 0xff3dd6, { spin: 1.1 }); sweeper(0, -31.5, 0, 4, -1.4);
    tileGrid(-0.95, -37, 2, 6, 0, { colors: [0x22d3ee, 0x7c3aed] });
    box(0, 0, -51, 8, 1, 6, 0xfacc15);
    const n1 = box(0, 0, -57, 3, 1, 3, 0x22d3ee, { blink: { period: 3.0, phase: 0 } });
    const n2 = box(-2, 0, -60.5, 3, 1, 3, 0xff3dd6, { blink: { period: 3.0, phase: -0.9 } });
    const n3 = box(2, 0, -64, 3, 1, 3, 0xa3e635, { blink: { period: 3.0, phase: -1.8 } });
    box(0, 0, -71, 10, 1, 8, 0xff3dd6);
    TR.finishZ = -69; finishDeco(-69, 0);
    cp(99, 0, 0, 1.5); cp(-22, 0, 0, -24); cp(-48.5, 0, 0, -51);
    TR.wps = [
      { x: -1.5, z: -6.8 }, { x: -1.5, z: -8 }, { x: 1.5, z: -11.5 }, { x: -1.5, z: -15 }, { x: 1.5, z: -18.5 }, { x: 0, z: -24, wide: 1.5 },
      { x: 0, z: -31.5 }, { x: 0, z: -36.5, wide: 0.5 }, { x: 0, z: -47, wide: 0.5 }, { x: 0, z: -51, wide: 2 },
      { blink: n1, x: 0, z: -57 }, { blink: n2, x: -2, z: -60.5 }, { blink: n3, x: 2, z: -64 }, { x: 0, z: -71, wide: 3 },
    ];
  } },
);
TRACKS.push({ name: 'Koostöö 1: Kaks väravat', coop: true, build() {
  box(0, 0, 0, 10, 1, 10, C.green); arch(4.5, 0, 0x22c55e);
  box(0, 0, -14, 10, 1, 18, C.blue);
  gate(0, -20, [[-3.5, -12], [3.5, -12]]);
  box(0, 0, -33, 10, 1, 20, C.purple);
  box(0, 1.2, -26.5, 1.6, 1.2, 1.2, C.red, { move: { axis: 'x', amp: 3.5, speed: 1.4, phase: 0 }, pusher: true });
  gate(1, -40, [[-3.5, -32], [3.5, -32]]);
  box(0, 0, -48, 10, 1, 10, C.yellow);
  TR.finishZ = -46; finishDeco(-46, 0);
  cp(99, 0, 0, 1.5); cp(-21, 0, 0, -24);
  TR.wps = [{ x: 0, z: -6, wide: 2 }, { gate: 0, x: 0, z: -18 }, { x: 0, z: -24, wide: 3 }, { gate: 1, x: 0, z: -38 }, { x: 0, z: -44, wide: 3 }, { x: 0, z: -49, wide: 3 }];
  const sg = textSprite('Seiske KAHEKESI nuppudel!', 3.4, '#fff', 'rgba(255,92,92,.95)'); sg.position.set(0, 4.2, -19.6); trackGroup.add(sg);
} });
TRACKS.push({ name: 'Koostöö 2: Kolm nuppu', coop: true, build() {
  box(0, 0, 0, 10, 1, 10, C.green); arch(4.5, 0, 0x0ea5e9);
  box(0, 0, -16, 10, 1, 22, C.blue);
  sweeper(0, -11, 0, 4.6, 1.1, 1);
  gate(0, -24, [[-3.5, -17.5], [0, -20], [3.5, -17.5]]);
  box(0, 0, -34, 10, 1, 18, C.purple);
  pendulum(-30, 1.6, 0); pendulum(-36, 1.6, 2.2);
  gate(1, -41.5, [[-3.8, -38.5], [3.8, -38.5]]);
  box(0, 0, -50, 10, 1, 16, C.yellow);
  TR.finishZ = -50; finishDeco(-50, 0);
  cp(99, 0, 0, 1.5); cp(-25, 0, 0, -27); cp(-42.5, 0, 0, -45);
  TR.wps = [{ x: 0, z: -6, wide: 2 }, { x: 0, z: -11, wide: 2 }, { x: 0, z: -16, wide: 2 }, { gate: 0, x: 0, z: -22 }, { x: 0, z: -27, wide: 2 }, { x: 0, z: -30.8 }, { x: 0, z: -35 }, { gate: 1, x: 0, z: -40 }, { x: 0, z: -46, wide: 2 }, { x: 0, z: -52, wide: 2 }];
  const sg = textSprite('KOLM nuppu korraga!', 3.2, '#fff', 'rgba(14,165,233,.95)'); sg.position.set(0, 4.2, -23.6); trackGroup.add(sg);
} });
TRACKS.push({ name: 'Koostöö 3: Hüppesild', coop: true, build() {
  box(0, 0, 0, 10, 1, 10, C.green); arch(4.5, 0, 0xff922b);
  box(0, 0, -10, 10, 1, 10, C.blue);
  gate(0, -14, [[-3.5, -8], [3.5, -8]]);
  const b1 = box(-1.5, 0, -16.5, 3, 1, 3, C.pink, { blink: { period: 3.8, phase: 0 } });
  const b2 = box(1.5, 0, -20, 3, 1, 3, C.yellow, { blink: { period: 3.8, phase: -0.8 } });
  const b3 = box(-1.5, 0, -23.5, 3, 1, 3, C.green, { blink: { period: 3.8, phase: -1.6 } });
  box(0, 0, -32, 10, 1, 12, C.purple);
  gate(1, -38, [[-3.8, -30], [-1.3, -32], [1.3, -32], [3.8, -30]]);
  box(0, 0, -40.5, 4, 0.5, 4, C.orange, { bounce: true });
  box(0, 3, -47, 8, 3.5, 8, C.blue);
  box(0, 3, -56, 10, 1, 10, C.yellow);
  TR.finishZ = -54; finishDeco(-54, 3);
  cp(99, 0, 0, 1.5); cp(-27, 0, 0, -29); cp(-45, 0, 3, -47);
  TR.wps = [{ x: 0, z: -6, wide: 2 }, { gate: 0, x: 0, z: -12 }, { x: -1.5, z: -14 }, { blink: b1, x: -1.5, z: -16.5 }, { blink: b2, x: 1.5, z: -20 }, { blink: b3, x: -1.5, z: -23.5 }, { x: 0, z: -28.5, wide: 2 }, { gate: 1, x: 0, z: -36 }, { x: 0, z: -39.5 }, { x: 0, z: -40.5 }, { x: 0, z: -47, wide: 2 }, { x: 0, z: -58, wide: 2 }];
  const sg = textSprite('NELI nuppu – kõik appi!', 3.4, '#fff', 'rgba(255,146,43,.95)'); sg.position.set(0, 4.2, -37.6); trackGroup.add(sg);
} });
TRACKS.push({ name: 'Koostöö 4: Suur väljakutse', coop: true, build() {
  box(0, 0, 0, 10, 1, 10, C.green); arch(4.5, 0, 0x8b5cf6);
  box(0, 0, -12, 10, 1, 14, C.blue);
  box(0, 1.2, -10, 1.6, 1.2, 1.2, C.red, { move: { axis: 'x', amp: 3.5, speed: 1.5, phase: 0 }, pusher: true });
  gate(0, -18.5, [[-3.8, -14], [3.8, -14]]);
  box(0, 0, -27, 10, 1, 16, C.pink, { conveyor: 1.3, flat: true });
  sweeper(0, -27, 0, 4.6, -1.0, 2);
  gate(1, -34, [[-3.8, -21.5], [0, -20.5], [3.8, -21.5]], 15);
  box(0, 0, -41, 10, 1, 12, C.purple);
  pendulum(-40, 1.7, 1);
  gate(2, -46.5, [[-3.8, -43], [-1.3, -44.5], [1.3, -44.5], [3.8, -43]]);
  box(0, 0, -55, 10, 1, 16, C.yellow);
  TR.finishZ = -54; finishDeco(-54, 0);
  cp(99, 0, 0, 1.5); cp(-19.5, 0, 0, -21); cp(-35, 0, 0, -37); cp(-47.5, 0, 0, -50);
  TR.wps = [{ x: 0, z: -6, wide: 2 }, { x: 0, z: -12, wide: 2 }, { gate: 0, x: 0, z: -16.5 }, { x: 0, z: -21, wide: 2 }, { gate: 1, x: 0, z: -32 }, { x: 0, z: -37, wide: 2 }, { x: 0, z: -39 }, { gate: 2, x: 0, z: -45.5 }, { x: 0, z: -50, wide: 2 }, { x: 0, z: -57, wide: 2 }];
  const sg = textSprite('Nupud on TAGA – jookske kiiresti!', 3.6, '#fff', 'rgba(139,92,246,.95)'); sg.position.set(0, 4.2, -33.6); trackGroup.add(sg);
} });
function gate(k, z, plates, hold = 6) {
  const w = box(0, 3, z, 10, 3, 0.6, C.red); w.gateBase = w.mesh.position.y;
  const pl = plates.map(([x, pz]) => ({ x, z: pz, s: cyl(x, 0.1, pz, 1.0, 0.2, C.yellow) }));
  TR.gates.push({ k, wall: w, plates: pl, z, hold });
}
function gateOpen(k) { return T() < ((show.gates || [])[k] || 0); }
function gateVisual(dt, t) {
  const all = [{ p: player.pos }, ...allOthers().map(o => ({ p: o.pos }))];
  for (const gt of TR.gates) {
    const open = gateOpen(gt.k);
    gt.wall.active = !open;
    const target = open ? gt.wall.gateBase - 3.2 : gt.wall.gateBase;
    gt.wall.mesh.position.y += (target - gt.wall.mesh.position.y) * Math.min(1, dt * 5);
    gt.plates.forEach(pl => { const occ = all.some(o => Math.hypot(o.p.x - pl.x, o.p.z - pl.z) < 1.05 && o.p.y < 1); pl.s.mesh.material = mat(occ ? 0x22c55e : C.yellow); if (occ && !pl.was && Math.hypot(player.pos.x - pl.x, player.pos.z - pl.z) < 1.05) questTick('pad'); pl.was = occ; });
    if (open && !gt.wasOpen) { sfx('ding'); showToast('Värav avanes! 🚪', 1.3); }
    gt.wasOpen = open;
  }
}
function gateHostTick(t) {
  if (!TR.gates.length || show.stage !== 'play') return;
  for (const gt of TR.gates) {
    const n = gt.plates.filter(pl => show.part.some(id => { const p = posOf(id); return p && Math.hypot(p.x - pl.x, p.z - pl.z) < 1.05 && p.y < 1; })).length;
    const need = Math.min(gt.plates.length, show.part.length);   // vähem mängijaid kui nuppe → piisab kõigist
    if (n >= need || show.part.length < 2) show.gates[gt.k] = Math.round((t + gt.hold) * 10) / 10;
  }
}
function startCoop(cl) {   // koostöörežiim: kõik koos läbi mitme koostöörajaa
  if (!net.isHost) return;
  if (cl === 1) { show.ct = 0; if (net.on) pushFeed('🤝 Koostöö algab! Kõik peavad finišisse jõudma'); }
  show.mode = 'coop'; show.cl = cl; show.tw = ''; show.weather = ''; show.rt = 'race'; show.ti = COOPS[cl - 1]; show.rn = cl; show.final = false;
  show.boss = null; show.teams = {}; show.gates = [0, 0, 0, 0]; show.ok = 0; show.lt = 0;
  const hs = humanIds(), helpers = bots.slice(0, Math.max(0, 4 - hs.length));
  show.part = [...hs, ...helpers.map(b => b.id)]; show.alive = show.part.slice(); show.inR = show.part.slice(); show.q = []; show.fo = []; show.quota = 99; show.lastOut = [];
  show.rid = newId(); show.stage = 'intro'; show.sa = T() + 5; show.ea = 0; hostOnly.claims = {};
}
function coopTick(t) {
  if (show.stage === 'intro' && t > show.sa) show.stage = 'play';
  if (show.stage === 'play') {
    gateHostTick(t);
    const tau = t - show.sa, hs = show.part.filter(isHuman).filter(id => id === net.id || net.peers.has(id));
    const done = hs.length > 0 && hs.every(id => show.q.includes(id));
    if (done || tau > 240) {
      show.ok = done ? 1 : 0; show.lt = Math.round(tau * 10) / 10; if (done) show.ct = Math.round(((show.ct || 0) + tau) * 10) / 10;
      if (done && net.on) pushFeed('✅ Tiim läbis raja ' + show.cl + ' ajaga ' + show.lt + ' s');
      show.stage = 'cdone'; show.ea = t;
    }
  }
  if (show.stage === 'cdone' && t > show.ea + 6) { if (!show.ok) startCoop(show.cl); else if (show.cl < COOPS.length) startCoop(show.cl + 1); else { show.stage = 'cwin'; show.ea = t; } }
  if (show.stage === 'cwin' && t > show.ea + 9) enterLobby();
}
const PRACTICE = [
  { id: 'move', text: '1. Liigu edasi (WASD / joystick / VR-is lükka kätega maad)' },
  { id: 'jump', text: '2. Hüppa üle madala müüri (tühik / HÜPPA / VR: lükka hooga üles)' },
  { id: 'duck', text: '3. Kükita kollase lati alt läbi (C / KÜKITA / vasak päästik)' },
  { id: 'grab', text: '4. Haara robotist kinni (E / HAARA / grip)' },
  { id: 'throw', text: '5. Viska robot: hoia kinni ja lase hooga lahti' },
  { id: 'bounce', text: '6. Hüppa batuudil' },
  { id: 'coin', text: '7. Korja münt' },
];
let practice = {};
function practiceDone(id) {
  if (show.stage !== 'trenn' || practice[id]) return;
  practice[id] = true; sfx('ding'); puff(player.pos, 0x22c55e, 10, 2, 0.5, 1);
  const left = PRACTICE.filter(p => !practice[p.id]);
  showToast(left.length ? '✓ Tubli! Järgmine: ' + left[0].text.slice(3, 40) : '🎉 Kõik harjutused tehtud!', 2);
  if (!left.length) { addXp(30); setTimeout(() => showMsg('Valmis!', ['Oskad nüüd kõike!', renderer.xr.isPresenting ? 'Y-menüüst: Mängi robotitega' : 'Vajuta R, et minna päris mängu'], 5), 800); }
}
function practiceLine() { const n = PRACTICE.find(p => !practice[p.id]); return n ? 'Harjutus ' + PRACTICE.filter(p => practice[p.id]).length + '/' + PRACTICE.length + ' · ' + n.text : '🎉 Kõik tehtud! ' + (renderer.xr.isPresenting ? 'Y-menüü → Mängi' : 'R = päris mäng'); }
function practiceTick() {
  if (player.pos.z < -4) practiceDone('move');
  if (player.pos.z < -8.6 && practice.move) practiceDone('jump');
  if (player.pos.z < -14.6 && practice.jump) practiceDone('duck');
  if ((pcGrab && pcGrab.startsWith('#')) || grabHold.some(g => g && g.startsWith('#'))) practiceDone('grab');
  if (player.bounceT > 0 && player.pos.z < -24) practiceDone('bounce');
}
function buildTrenn() {
  box(0, 0, -20, 8, 1, 50, 0xb3e5fc);
  [-4.2, 4.2].forEach(x => box(x, 0.5, -20, 0.4, 0.5, 50, 0xffffff));
  const sign = (t, z, c) => { const sp = textSprite(t, 3.4, '#fff', c); sp.position.set(0, 3.2, z); trackGroup.add(sp); };
  sign('1. Liigu edasi', -1, 'rgba(47,155,255,.95)');
  box(0, 0.6, -8, 7.6, 0.6, 0.4, C.orange); sign('2. Hüppa üle', -6.5, 'rgba(255,169,77,.95)');
  box(0, 1.6, -14, 7.6, 0.4, 0.4, 0xffd43b); sign('3. Kükita alt läbi', -12.5, 'rgba(250,204,21,.95)');
  TR.dummyPos = new V3(0, 0.3, -20); sign('4.–5. Haara ja viska robotit', -18, 'rgba(139,92,246,.95)');
  box(0, 0.25, -27, 3, 0.5, 3, C.orange, { bounce: true }); sign('6. Batuut', -25.5, 'rgba(255,61,139,.95)');
  addCoin(0, 1.2, -32); addCoin(-1.5, 1.2, -32.5); addCoin(1.5, 1.2, -32.5); sign('7. Mündid', -30.5, 'rgba(34,197,94,.95)');
  sign('Valmis? Vajuta R', -42, 'rgba(29,27,58,.9)');
  TR.spawn = (i, n, out) => out.set(0, 0.3, 2);
  TR.outY = -6;
}
function levelIndex(n) { return TRACKS.findIndex(t => t.gen && t.level === n); }
prog.levels = prog.levels || {};
function levelStars(n) { return prog.levels[n] ? prog.levels[n].s : 0; }
function levelUnlocked(n) { return n === 1 || levelStars(n - 1) > 0; }
function startLevel(n) {
  if (n > LEVEL_COUNT) { openLevelSelect(); return; }
  readMenu(); started = true; $('menu').style.display = 'none'; $('levelSel').classList.remove('on'); $('lvDone').classList.remove('on'); goFullscreen();
  pitch = 0; camera.rotation.set(0, 0, 0);
  net.on = false; net.isHost = true;
  makeBots(3); bots.forEach(b => { b.speed *= 0.85; });
  show.mode = 'levels'; show.level = n; show.weather = ''; show.gates = [0, 0]; show.boss = null; show.teams = {};
  show.rt = 'race'; show.ti = levelIndex(n); show.stage = 'intro'; show.sa = T() + 4; show.ea = 0;
  show.part = [net.id, ...bots.map(b => b.id)]; show.alive = show.part.slice(); show.inR = show.part.slice(); show.q = []; show.fo = []; show.quota = 99; show.final = false;
  show.rid = newId(); closeVrMenu(); show.tw = '';
  updateHud(); showDailyMsg();
}
function levelComplete() {
  if (show.stage !== 'play') return;
  show.stage = 'lvdone'; show.ea = T();
  const n = show.level, t = myFinishTime, par = TR.par || 40;
  if (n === 0) {   // päeva väljakutse
    const today = dayKey(), d = prog.daily && prog.daily.day === today ? prog.daily : null, first = !d, rec = !d || t < d.t;
    prog.daily = { day: today, t: d ? Math.min(d.t, Math.round(t * 10) / 10) : Math.round(t * 10) / 10 }; prog.dailyN = (prog.dailyN || 0) + (first ? 1 : 0); saveProg();
    setTimeout(() => addCoins(first ? 40 : rec ? 10 : 2, 'päeva väljakutse'), 700); addXp(first ? 40 : 5); if (first) addTrophies(5);
    sfx('fanfare'); burst(player.pos);
    $('lvTitle').textContent = 'Päeva väljakutse läbitud!'; $('lvStars').textContent = rec ? '🏆 REKORD' : '✅';
    $('lvInfo').textContent = 'Aeg ' + t.toFixed(1) + ' s · tänane rekord ' + prog.daily.t + ' s · homme tuleb uus rada!';
    $('lvNext').style.display = 'none';
    if (renderer.xr.isPresenting) { vrMenu.open = true; vrMenu.inGame = true; vrMenu.screen = 'lvdone'; vrMenu.place = true; drawMenu(); }
    else { $('lvDone').classList.add('on'); if (document.pointerLockElement) document.exitPointerLock(); }
    showMsg(rec ? '🏆 Uus rekord!' : '✅ Läbitud!', ['Päeva väljakutse', 'Aeg ' + t.toFixed(1) + ' s'], 4);
    return;
  }
  const st = t <= par ? 3 : t <= par * 1.5 ? 2 : 1;
  const old = prog.levels[n] || { s: 0, t: 999 };
  const firstTime = !old.s;
  prog.levels[n] = { s: Math.max(old.s, st), t: Math.min(old.t, Math.round(t * 10) / 10) };
  saveProg();
  const reward = (firstTime ? 10 : 2) + (st > old.s ? (st - old.s) * 5 : 0);
  setTimeout(() => addCoins(reward, 'tase ' + n), 700); addXp(firstTime ? 25 : 5);
  sfx(st === 3 ? 'fanfare' : 'cheer'); burst(player.pos);
  const stars = '⭐'.repeat(st) + '☆'.repeat(3 - st);
  $('lvTitle').textContent = 'Tase ' + n + ' läbitud!'; $('lvStars').textContent = stars;
  $('lvInfo').textContent = 'Aeg ' + t.toFixed(1) + ' s · 3 tähe jaoks ' + par + ' s · rekord ' + prog.levels[n].t + ' s';
  $('lvNext').style.display = n < LEVEL_COUNT ? 'inline-block' : 'none';
  if (renderer.xr.isPresenting) { vrMenu.open = true; vrMenu.inGame = true; vrMenu.screen = 'lvdone'; vrMenu.place = true; drawMenu(); }
  else { $('lvDone').classList.add('on'); if (document.pointerLockElement) document.exitPointerLock(); }
  showMsg(stars, ['Tase ' + n + ' läbitud!', 'Aeg ' + t.toFixed(1) + ' s'], 4);
}
function openLevelSelect() {
  if (renderer.xr.isPresenting) { vrMenu.open = true; vrMenu.screen = 'levels'; vrMenu.place = true; drawMenu(); return; }
  const g = $('lvGrid'); g.innerHTML = '';
  let total = 0;
  for (let n = 1; n <= LEVEL_COUNT; n++) {
    const b = document.createElement('button'), st = levelStars(n), un = levelUnlocked(n); total += st;
    b.className = 'lvb' + (un ? '' : ' lock'); b.innerHTML = '<b>' + n + '</b><br>' + (un ? '⭐'.repeat(st) + '☆'.repeat(3 - st) : '🔒');
    b.onclick = () => { if (un) startLevel(n); else showToast('Läbi enne tase ' + (n - 1), 1.2); };
    g.appendChild(b);
  }
  $('lvTotal').textContent = 'Tähti kokku: ' + total + ' / ' + LEVEL_COUNT * 3;
  $('levelSel').classList.add('on');
}
function startPractice() {
  readMenu(); started = true; $('menu').style.display = 'none'; goFullscreen();
  pitch = 0; camera.rotation.set(0, 0, 0);
  net.on = false; net.isHost = true; practice = {};
  makeBots(1); bots[0].speed = 0;
  show.stage = 'trenn'; show.rt = 'trenn'; show.ti = 0; show.rid = newId(); show.part = []; show.alive = [];
  updateHud();
  setTimeout(() => showMsg('Harjutusväljak', ['Tee harjutused läbi järjest', 'Ülal on alati järgmine ülesanne'], 4), 300);
}
// =====================================================================
// TASEMED: rajad genereeritakse juppidest (sama number = alati sama rada)
// =====================================================================
function rng(seed) { let a = seed >>> 0; return () => { a = (a + 0x6D2B79F5) | 0; let t = Math.imul(a ^ (a >>> 15), 1 | a); t = (t + Math.imul(t ^ (t >>> 7), 61 | t)) ^ t; return ((t ^ (t >>> 14)) >>> 0) / 4294967296; }; }
const GPAL = [C.pink, C.yellow, C.blue, C.green, C.purple, C.orange, 0x63e6be, 0xff8787];
const SEG = {
  stones(c) { const sd = c.R() < 0.5 ? 1 : -1; for (let i = 0; i < 4; i++) { const x = (i % 2 ? 1.5 : -1.5) * sd, zc = c.z - 2 - i * 3.5; box(x, c.y, zc, 3, 1, 3, c.col()); c.w({ x, z: zc }); } c.z = c.z - 2 - 3 * 3.5 - 1.5; },
  tiles(c) { tileGrid(-2.85, c.z - 1.4, 4, 5, c.y); c.w({ x: 0, z: c.z - 1.4, wide: 1.5 }); c.w({ x: 0, z: c.z - 9, wide: 1.5 }); c.z -= 10; },
  disc(c) { const zc = c.z - 5.5; cyl(0, c.y, zc, 5, 1, c.col()); cyl(0, c.y + 1.4, zc, 0.6, 1.4, C.red); sweeper(0, zc, c.y, 5, (c.R() < 0.5 ? 1 : -1) * (0.8 + c.R() * 0.5)); c.w({ x: 0, z: zc + 4.4, wide: 1 }); c.w({ side: 2.6, z: zc }); c.w({ x: 0, z: zc - 4.4, wide: 1 }); c.z = zc - 5; },
  movers(c) { const ms = []; for (let i = 0; i < 3; i++) ms.push(box(0, c.y, c.z - 2 - i * 3.5, 3, 0.5, 3, c.col(), { move: { axis: 'x', amp: 3, speed: 1 + c.R() * 0.3, phase: i * 2.1 } })); c.w({ x: 0, z: c.z + 0.6, wide: 0.8 }); ms.forEach(m => c.w({ plat: m })); c.z = c.z - 2 - 2 * 3.5 - 1.5; },
  pend(c) { box(0, c.y, c.z - 7.5, 3, 1, 15, c.col()); [-3, -7.5, -12].forEach((dz, i) => pendulum(c.z + dz, 1.5 + c.R() * 0.5, i * 2.1, c.y)); [-1, -5.2, -9.7, -14].forEach(dz => c.w({ x: 0, z: c.z + dz })); c.z -= 15; },
  walls(c) { box(0, c.y, c.z - 9, 10, 1, 18, c.col()); const ws = [-5, -12].map((dz, k) => box(0, c.y + 2.2, c.z + dz, 7, 2.2, 0.8, c.col(), { move: { axis: 'x', amp: 3, speed: 0.7 + c.R() * 0.5, phase: k * 1.7 }, pusher: true })); ws.forEach(w => c.w({ gap: w, z: w.mesh.position.z })); c.w({ x: 0, z: c.z - 17, wide: 2 }); c.z -= 18; },
  blink(c) { const per = 3.6 + c.R() * 0.6; [[-1.5, -2], [1.5, -5.5], [-1.5, -9], [1.5, -12.5]].forEach(([x, dz], i) => { const b = box(x, c.y, c.z + dz, 3, 1, 3, c.col(), { blink: { period: per, phase: -i * 0.7 } }); c.w({ blink: b, x, z: c.z + dz }); }); c.z -= 14; },
  rollers(c) { box(0, c.y, c.z - 7, 6, 1, 14, 0x9aa5b1); [-3.5, -8.5].forEach(dz => box(0, c.y + 1, c.z + dz, 6, 1, 1, C.orange, { conveyor: 1.6, flat: true, roller: true })); box(-3.4, c.y + 1.4, c.z - 6, 1.6, 1.2, 1, C.red, { move: { axis: 'x', amp: 1.8, speed: 2, phase: c.R() * 3 }, pusher: true }); c.w({ x: 0, z: c.z - 1, wide: 1 }); c.w({ x: 0, z: c.z - 13, wide: 1 }); c.z -= 14; },
  stairs(c) { for (let i = 0; i < 3; i++) { c.y += 0.6; const zc = c.z - 1.5 - i * 3.6; box(0, c.y, zc, 4, 1 + c.y, 3, c.col()); c.w({ x: 0, z: zc }); } c.z = c.z - 1.5 - 2 * 3.6 - 1.5; },
  bounce(c) { box(0, c.y, c.z - 2, 4, 0.5, 4, C.orange, { bounce: true }); c.y += 3; box(0, c.y, c.z - 6.5, 6, 3.2, 5, c.col()); c.w({ x: 0, z: c.z - 1 }); c.w({ x: 0, z: c.z - 2 }); c.w({ x: 0, z: c.z - 6.5, wide: 1.5 }); c.z -= 9; },
  conv(c) { box(0, c.y, c.z - 7, 6, 1, 14, 0x9aa5b1, { conveyor: 1.2 + c.R() * 0.6, flat: true }); box(0, c.y + 1.2, c.z - 5, 1.6, 1.2, 1.2, C.red, { move: { axis: 'x', amp: 2, speed: 1.5, phase: 0 }, pusher: true }); box(0, c.y + 1.2, c.z - 10, 1.6, 1.2, 1.2, C.red, { move: { axis: 'x', amp: 2, speed: 1.5, phase: 2 }, pusher: true }); c.w({ x: 0, z: c.z - 1, wide: 1.5 }); c.w({ x: 0, z: c.z - 13, wide: 1.5 }); c.z -= 14; },
  spin(c) { const a = c.z - 4, b = c.z - 11.5, sg = c.R() < 0.5 ? 1 : -1; cyl(0, c.y, a, 3.5, 1, c.col(), { spin: sg * (0.7 + c.R() * 0.4) }); cyl(0, c.y, b, 3.5, 1, c.col(), { spin: -sg * (0.7 + c.R() * 0.4) }); c.w({ x: 0, z: a }); c.w({ x: 0, z: b }); c.z = b - 3.5; },
  lift(c) { const l = box(0, c.y, c.z - 2.5, 3, 0.5, 3, c.col(), { move: { axis: 'y', amp: 1.2, speed: 0.9, phase: c.R() * 6 } }); c.w({ liftOn: l }); c.w({ liftOff: l, top: c.y + 1.2, x: 0, z: c.z - 7 }); c.y += 1.2; box(0, c.y, c.z - 7, 4, 1, 4, c.col()); c.z -= 9; },
};
const SEG_EASY = ['stones', 'tiles', 'movers', 'spin', 'stairs'];
const SEG_MID = ['disc', 'conv', 'lift', 'bounce'];
const SEG_HARD = ['walls', 'pend', 'blink', 'rollers'];
const LV_ADJ = ['Rõõmus', 'Kiire', 'Vingerdav', 'Hüplev', 'Keerlev', 'Salapärane', 'Vahune', 'Hull', 'Libe', 'Tormine', 'Kuldne', 'Pilvine', 'Kõikuv', 'Suur', 'Viimane'];
const LV_NOUN = ['sild', 'rada', 'mägi', 'labürint', 'saar', 'tee', 'torn', 'jõgi', 'park', 'linn'];
function genTrack(seed, nSeg, pool) {
  return function () {
    const R = rng(seed);
    const c = { z: -5, y: 0, R, w: wp => TR.wps.push(wp), col: () => GPAL[Math.floor(R() * GPAL.length)] };
    box(0, 0, 0, 10, 1, 10, C.green); arch(4.5, 0, c.col()); cp(99, 0, 0, 1.5);
    let last = '';
    for (let i = 0; i < nSeg; i++) {
      let name; do { name = pool[Math.floor(R() * pool.length)]; } while (name === last && pool.length > 1);
      last = name; const z0 = c.z, y0 = c.y; SEG[name](c); (TR.segs = TR.segs || []).push([name, Math.round(z0), Math.round(c.z), y0, c.y]);
      const zc = c.z - 2.5; box(0, c.y, zc, 8, 1, 5, c.col());
      if (i % 2 === 0 && i < nSeg - 1) cp(c.z - 0.6, 0, c.y, zc);
      c.w({ x: 0, z: zc, wide: 2 }); c.z -= 5;
    }
    box(0, c.y, c.z - 5, 10, 1, 10, C.yellow);
    TR.finishZ = c.z - 2; finishDeco(c.z - 2, c.y); c.w({ x: 0, z: c.z - 6, wide: 3 });
    TR.par = Math.round(Math.abs(TR.finishZ) / 3.4 + nSeg * 1.5);
  };
}
const LEVEL_COUNT = 30;
const LEVEL_BASE = TRACKS.length;   // tasemed tulevad TRACKS massiivi lõppu (pärast käsitsi tehtud radu)
for (let n = 1; n <= LEVEL_COUNT; n++) {
  const R = rng(4242 + n * 97);
  const pool = n <= 5 ? SEG_EASY : n <= 12 ? [...SEG_EASY, ...SEG_MID] : n <= 20 ? [...SEG_EASY.slice(2), ...SEG_MID, ...SEG_HARD] : [...SEG_MID, ...SEG_HARD];
  const nSeg = Math.min(9, 2 + Math.floor(n / 3));
  TRACKS.push({ name: 'Tase ' + n + ': ' + LV_ADJ[Math.floor(R() * LV_ADJ.length)] + ' ' + LV_NOUN[Math.floor(R() * LV_NOUN.length)], gen: true, level: n, build: genTrack(9000 + n * 7919, nSeg, pool) });
}
function pendulum(z, speed, phase, by = 0) {   // kiikuv kuul üle silla
  const piv = new V3(0, 6 + by, z), len = 5.2, R = 0.8;
  const frame = new THREE.Group(); frame.position.set(0, by, z);
  [-4.6, 4.6].forEach(x => { const p = new THREE.Mesh(new THREE.CylinderGeometry(0.18, 0.22, 6.2, 10), mat(0xffffff)); p.position.set(x, 3.0, 0); frame.add(p); });
  const top = new THREE.Mesh(new THREE.CylinderGeometry(0.15, 0.15, 9.4, 10), mat(0xffffff)); top.rotation.z = Math.PI / 2; top.position.y = 6.05; frame.add(top);
  shadowAll(frame, true, false); trackGroup.add(frame);
  const g = new THREE.Group(); g.position.copy(piv);
  const rope = new THREE.Mesh(new THREE.CylinderGeometry(0.05, 0.05, len, 6), mat(0x8b5a3c)); rope.position.y = -len / 2; g.add(rope);
  const b = new THREE.Mesh(new THREE.SphereGeometry(R, 20, 14), mat(0xff3b3b)); b.position.y = -len; g.add(b);
  const st = new THREE.Mesh(new THREE.TorusGeometry(R * 1.0, R * 0.12, 8, 24), mat(0xffffff)); st.position.y = -len; g.add(st);
  shadowAll(g, true, false); trackGroup.add(g);
  TR.pendulums.push({ g, piv, len, r: R, speed, phase, amp: 1.0, pos: new V3(), vx: 0 });
}

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
  noor: { name: 'Hüppenöör', desc: 'Hüppa üle punase lati, KÜKITA kollase all (C / KÜKITA / vasak päästik)!', build() {
    cyl(0, 0, -12, 9, 1, C.blue); cyl(0, 2.4, -12, 0.7, 2.4, C.red);
    const a = sweeper(0, -12, 0, 9, 0, 1); a.angleFn = t => accelAngle(t * S(), 0.9, 0.03, 2.8);
    const b = sweeper(0, -12, 0, 9, 0, 1, 0, { height: 1.15, high: true }); b.angleFn = t => 2.2 - accelAngle(t * S(), 0.6, 0.02, 2.0); b.activeFn = t => t > 10;
    TR.spawn = (i, n, out) => { const half = i % 2, k = Math.floor(i / 2), m = Math.ceil(n / 2); const ang = 0.8 + (k + 0.5) / Math.max(1, m) * (Math.PI - 1.6) + half * Math.PI; out.set(Math.cos(ang) * 6, 0.3, -12 - Math.sin(ang) * 6); };
  } },
  boss: { name: 'Bossivoor', desc: 'Hiidrobot ründab! Seiske kõigil 4 nupul, et ta limasse lükata. Hoidu meteooridest!', build() {
    cyl(0, 0, -12, 12, 1, C.purple); cyl(0, 2, -12, 2.2, 3, 0x495057);
    const bm = makeBot(0x7c3aed, 0); bm.scale.setScalar(4); bm.position.set(0, 2, -12); trackGroup.add(bm); TR.bossMesh = bm;
    const sw = sweeper(0, -12, 0, 12, 0, 1); sw.angleFn = t => accelAngle(t * S(), 0.45, 0.015, 1.6); sw.activeFn = t => t > 5;
    [0.785, 2.356, 3.927, 5.498].forEach((a, i) => {
      const x = Math.cos(a) * 9, z = -12 + Math.sin(a) * 9;
      const pd = cyl(x, 0.12, z, 1.3, 0.24, 0xff5c5c); TR.pads.push({ s: pd, x, z, i });
      const lb = textSprite('NUPP ' + (i + 1), 1.4, '#fff', 'rgba(255,92,92,.9)'); lb.position.set(x, 1.6, z); trackGroup.add(lb);
    });
    TR.meteors = {};
    TR.spawn = (i, n, out) => { const ang = i / Math.max(1, n) * Math.PI * 2 + 0.4; out.set(Math.cos(ang) * 6, 0.3, -12 + Math.sin(ang) * 6); };
  } },
  magi: { name: 'Mäe tipp', desc: 'Ronige tippu ja püsige seal – iga sekund tipus annab punkti!', build() {
    cyl(0, 0, -12, 9.5, 1, C.green); cyl(0, 0.6, -12, 6.5, 1.6, C.yellow); cyl(0, 1.2, -12, 4.5, 2.2, C.orange); cyl(0, 1.8, -12, 2.6, 2.8, C.pink);
    const glow = new THREE.Mesh(new THREE.TorusGeometry(2.6, 0.12, 8, 48), new THREE.MeshBasicMaterial({ color: 0xffd43b })); glow.rotation.x = Math.PI / 2; glow.position.set(0, 1.85, -12); trackGroup.add(glow);
    const cr = crownMesh(0.8); cr.position.set(0, 4.2, -12); cr.userData.spin = true; cr.userData.baseY = 4.2; trackGroup.add(cr);
    box(-7.6, 0.25, -12, 2, 0.5, 2, C.orange, { bounce: true }); box(7.6, 0.25, -12, 2, 0.5, 2, C.orange, { bounce: true });
    TR.spawn = (i, n, out) => { const ang = i / Math.max(1, n) * Math.PI * 2; out.set(Math.cos(ang) * 8, 0.3, -12 + Math.sin(ang) * 8); };
  } },
  pall: { name: 'Pallimäng', desc: 'Võistkonnad! Lükake suur pall vastaste väravasse.', build() {
    box(0, 0, -12, 16, 1, 26, C.green);
    [-8.3, 8.3].forEach(x => box(x, 0.8, -12, 0.6, 0.8, 26.6, 0xffffff));
    [1.3, -25.3].forEach(z => [-5.65, 5.65].forEach(x => box(x, 1.2, z, 4.7, 1.2, 0.6, 0xffffff)));
    [[2.8, 0], [-26.8, 1]].forEach(([z, tm]) => {
      box(0, 0, z, 6.4, 1, 3, TEAM_COLORS[tm]);
      box(0, 1.6, z + (tm ? -1.7 : 1.7), 6.8, 1.6, 0.4, TEAM_COLORS[tm]);
      [-3.2, 3.2].forEach(x => box(x, 1.6, z, 0.4, 1.6, 3, TEAM_COLORS[tm]));
      const bar = new THREE.Mesh(new THREE.CylinderGeometry(0.12, 0.12, 6.4, 10), mat(0xffffff)); bar.rotation.z = Math.PI / 2; bar.position.set(0, 2.4, tm ? -25.3 : 1.3); trackGroup.add(bar);
    });
    const line = new THREE.Mesh(new THREE.BoxGeometry(16, 0.02, 0.25), new THREE.MeshBasicMaterial({ color: 0xffffff })); line.position.set(0, 0.02, -12); trackGroup.add(line);
    const bm = new THREE.Group();
    const bs = new THREE.Mesh(new THREE.SphereGeometry(1.1, 24, 16), mat(0xffffff)); bm.add(bs);
    for (let i = 0; i < 3; i++) { const band = new THREE.Mesh(new THREE.TorusGeometry(1.1, 0.07, 6, 32), mat(0xff3d8b)); band.rotation.set(i * 1.05, i * 0.7, 0); bm.add(band); }
    shadowAll(bm, true, false); bm.position.set(0, 1.1, -12); trackGroup.add(bm); TR.ballMesh = bm;
    TR.teamSpawn = (tm, k, out) => out.set(-5 + (k % 6) * 2, 0.3, tm === 0 ? -4 - Math.floor(k / 6) * 1.5 : -20 + Math.floor(k / 6) * 1.5);
    TR.spawn = (i, n, out) => out.set(rnd(-5, 5), 0.3, rnd(-18, -6));
  } },
  munad: { name: 'Munajaht', desc: 'Võistkonnad! Viige munad oma pessa – võib ka vastastelt varastada.', build() {
    box(0, 0, -12, 18, 1, 22, C.yellow);
    box(-5, 0.6, -12, 2, 0.6, 2, C.pink); box(5, 0.6, -12, 2, 0.6, 2, C.purple); box(0, 0.25, -12, 2.4, 0.5, 2.4, C.orange, { bounce: true });
    [[-2.5, 0], [-21.5, 1]].forEach(([z, tm]) => {
      const n = new THREE.Mesh(new THREE.CylinderGeometry(2, 2.1, 0.25, 32), mat(TEAM_COLORS[tm])); n.position.set(0, 0.12, z); trackGroup.add(n);
      const rim = new THREE.Mesh(new THREE.TorusGeometry(2.05, 0.18, 8, 40), mat(0xc9a36b)); rim.rotation.x = Math.PI / 2; rim.position.set(0, 0.25, z); trackGroup.add(rim);
    });
    TR.eggMeshes = [];
    const ec = [0xfff3bf, 0xd0ebff, 0xffdeeb, 0xd3f9d8, 0xfff0f6];
    for (let i = 0; i < 14; i++) { const e = new THREE.Mesh(new THREE.SphereGeometry(0.28, 14, 10), mat(ec[i % ec.length])); e.scale.y = 1.3; e.castShadow = true; e.position.set(0, -50, 0); trackGroup.add(e); TR.eggMeshes.push(e); }
    TR.teamSpawn = (tm, k, out) => out.set(-5 + (k % 6) * 2, 0.3, tm === 0 ? -4.8 - Math.floor(k / 6) * 1.2 : -19.2 + Math.floor(k / 6) * 1.2);
    TR.spawn = (i, n, out) => out.set(rnd(-6, 6), 0.3, rnd(-18, -6));
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
  const cv2 = document.createElement('canvas'); cv2.width = 640; cv2.height = 480;
  const tex2 = new THREE.CanvasTexture(cv2);
  const b2 = new THREE.Mesh(new THREE.PlaneGeometry(4, 3), new THREE.MeshBasicMaterial({ map: tex2 }));
  b2.position.set(-7.85, 3.0, 1.5); b2.rotation.y = Math.PI / 2; trackGroup.add(b2);
  const post2 = new THREE.Mesh(new THREE.BoxGeometry(0.2, 3.3, 4.3), mat(0x2f9bff)); post2.position.set(-8.0, 3.0, 1.5); trackGroup.add(post2);
  TR.board2 = { cv: cv2, tex: tex2, key: '' };
  TR.spawn = (i, n, out) => out.set(-4 + (i % 5) * 2, 0.3, 3.5 - Math.floor(i / 5) * 1.3);
  // rajaehitaja plats
  box(11, 0, 0, 6.2, 1, 3, 0xffffff);
  const zone = box(19, 0, 0, 10, 1, 24, 0xdee2e6);
  zone.mesh.material = gridMat;
  box(19, 0.06, 10, 10, 0.12, 4, C.green); box(19, 0.06, -10, 10, 0.12, 4, C.yellow);
  const zs = textSprite('RAJAEHITAJA', 3.2, '#fff', 'rgba(47,155,255,.95)'); zs.position.set(19, 3.4, 12); trackGroup.add(zs);
  const zs2 = textSprite('START', 1.6, '#fff', 'rgba(34,197,94,.95)'); zs2.position.set(19, 1.4, 10.5); trackGroup.add(zs2);
  const zs3 = textSprite('FINIŠ', 1.6, '#1d1b3a', 'rgba(255,209,102,.95)'); zs3.position.set(19, 1.4, -10.5); trackGroup.add(zs3);
  TR.builderSolids = []; TR.builderKey = null;
  const gm = new THREE.Mesh(new THREE.BoxGeometry(1, 1, 1), new THREE.MeshBasicMaterial({ color: 0x22c55e, transparent: true, opacity: 0.35, depthWrite: false }));
  gm.visible = false; trackGroup.add(gm); TR.ghost = gm;
}
const gridMat = (() => {
  const cv = document.createElement('canvas'); cv.width = cv.height = 64; const x = cv.getContext('2d');
  x.fillStyle = '#e9ecef'; x.fillRect(0, 0, 64, 64); x.strokeStyle = '#adb5bd'; x.lineWidth = 3; x.strokeRect(1.5, 1.5, 61, 61);
  const t = new THREE.CanvasTexture(cv); t.wrapS = t.wrapT = THREE.RepeatWrapping; t.repeat.set(10, 24); t.colorSpace = THREE.SRGBColorSpace;
  return new THREE.MeshToonMaterial({ map: t, gradientMap: gradTex });
})();
const BUILD_TYPES = ['Kast', 'Batuut', 'Auk'];
const STACK_COLORS = [0xff7eb6, 0x5ec8ff, 0xffd166, 0x7be495];
let buildType = 0, buildAimCache = null;
show.blocks = [];
function cellWorld(cx, cz) { return { x: 14.5 + cx, z: 7.5 - cz }; }
function rebuildBuilder(force) {
  if (!TR || TR.kind !== 'lobby') return;
  const key = JSON.stringify(show.blocks || []);
  if (!force && key === TR.builderKey) return;
  TR.builderKey = key;
  for (const sd of TR.builderSolids) { const i = TR.solids.indexOf(sd); if (i >= 0) TR.solids.splice(i, 1); trackGroup.remove(sd.mesh); }
  TR.builderSolids = [];
  for (const [cx, cz, n, bo, ho] of show.blocks || []) {
    const w = cellWorld(cx, cz);
    if (ho) { const hm = new THREE.Mesh(new THREE.PlaneGeometry(0.95, 0.95), new THREE.MeshBasicMaterial({ color: 0x1d1b3a })); hm.rotation.x = -Math.PI / 2; hm.position.set(w.x, 0.015, w.z); trackGroup.add(hm); TR.builderSolids.push({ mesh: hm }); continue; }
    for (let k = 0; k < n; k++) TR.builderSolids.push(box(w.x, k + 1, w.z, 1, 1, 1, STACK_COLORS[k % 4]));
    if (bo) TR.builderSolids.push(box(w.x, n + 0.25, w.z, 0.9, 0.25, 0.9, C.orange, { bounce: true }));
  }
}
function stackAt(cx, cz) { const e = (show.blocks || []).find(b => b[0] === cx && b[1] === cz); return e ? e[2] + (e[3] ? 0.25 : 0) : 0; }
function buildAim() {
  if (!TR || TR.kind !== 'lobby' || show.stage !== 'lobby' || vrMenu.open) return null;
  let o, d;
  if (renderer.xr.isPresenting) { const r = rays[1]; r.c.updateMatrixWorld(true); o = tmpA.setFromMatrixPosition(r.c.matrixWorld).clone(); d = tmpB.set(0, 0, -1).transformDirection(r.c.matrixWorld).clone(); }
  else { o = camera.getWorldPosition(tmpA).clone(); d = camera.getWorldDirection(tmpB).clone(); }
  if (o.x < 10 && Math.abs(player.pos.x - 19) > 6) return null;
  if (d.y > -0.05) return null;
  const tt = -o.y / d.y; if (tt > 12) return null;
  const hx = o.x + d.x * tt, hz = o.z + d.z * tt;
  if (hx < 14 || hx >= 24 || hz > 8 || hz <= -8) return null;
  const cx = Math.floor(hx - 14), cz = Math.floor(8 - hz);
  return { cx, cz };
}
function applyBuild(cx, cz, t, del) {
  if (cx < 0 || cx > 9 || cz < 0 || cz > 15) return;
  const B = show.blocks; let e = B.find(b => b[0] === cx && b[1] === cz);
  if (del) { if (!e) return; if (e[3]) e[3] = 0; else if (e[2] > 0) e[2]--; else e[4] = 0; if (!e[2] && !e[3] && !e[4]) B.splice(B.indexOf(e), 1); return; }
  if (!e) { if (B.length >= 160) return; e = [cx, cz, 0, 0, 0]; B.push(e); }
  if (t === 0) { e[4] = 0; if (e[2] < 4 && !e[3]) e[2]++; }
  else if (t === 1) { e[4] = 0; e[3] = 1; }
  else { e[2] = 0; e[3] = 0; e[4] = 1; }
}
function doBuild(del) {
  const a = buildAimCache; if (!a) return;
  sfx('build');
  if (net.isHost) applyBuild(a.cx, a.cz, buildType, del); else if (net.on) net.req.send({ k: del ? 'del' : 'build', cx: a.cx, cz: a.cz, t: buildType });
}
function cycleBuildType() { buildType = (buildType + 1) % BUILD_TYPES.length; showToast('Ehitad: ' + BUILD_TYPES[buildType], 1.2); const b = document.getElementById('bType'); if (b) b.textContent = 'TÜÜP: ' + BUILD_TYPES[buildType]; }
function updateBuilder() {
  buildAimCache = buildAim();
  if (!TR || !TR.ghost) return;
  if (!buildAimCache) { TR.ghost.visible = false; return; }
  const w = cellWorld(buildAimCache.cx, buildAimCache.cz), h = stackAt(buildAimCache.cx, buildAimCache.cz);
  TR.ghost.visible = true;
  TR.ghost.material.color.setHex(buildType === 0 ? 0x22c55e : buildType === 1 ? 0xffa94d : 0x1d1b3a);
  TR.ghost.scale.set(1, buildType === 2 ? 0.05 : buildType === 1 ? 0.25 : 1, 1);
  TR.ghost.position.set(w.x, buildType === 2 ? 0.03 : h + (buildType === 1 ? 0.13 : 0.5), w.z);
}
function buildCustom() {
  box(0, 0, 0, 10, 1, 6, C.green); arch(3.5, 0, C.blue);
  const blocks = show.blocks || [], holes = new Set(blocks.filter(b => b[4]).map(b => b[0] + ',' + b[1]));
  for (let cz = 0; cz < 16; cz++) {
    let cx = 0;
    while (cx < 10) {
      if (holes.has(cx + ',' + cz)) { cx++; continue; }
      let e = cx; while (e < 10 && !holes.has(e + ',' + cz)) e++;
      box(-5 + (cx + e) / 2, 0, -3.5 - cz, e - cx, 1, 1, cz % 2 ? 0x9be7ff : 0xb3e5fc);
      cx = e;
    }
  }
  for (const [cx, cz, n, bo, ho] of blocks) {
    if (ho) continue;
    const x = -4.5 + cx, z = -3.5 - cz;
    for (let k = 0; k < n; k++) box(x, k + 1, z, 1, 1, 1, STACK_COLORS[k % 4]);
    if (bo) box(x, n + 0.25, z, 0.9, 0.25, 0.9, C.orange, { bounce: true });
  }
  box(0, 0, -24, 10, 1, 10, C.yellow);
  TR.finishZ = -21; finishDeco(-21, 0);
  cp(99, 0, 0, 1);
  TR.wps = [{ x: 0, z: -4, wide: 3 }, { x: 0, z: -12, wide: 4 }, { x: 0, z: -19.5, wide: 3 }, { x: 0, z: -25, wide: 3 }];
}
TRACKS.push({ name: 'Teie rada', custom: true, build: buildCustom });
{ // päeva väljakutse: iga päev uus juhuslik (kuid kõigil sama) rada
  let h = 7; for (const ch of new Date().toDateString()) h = (h * 31 + ch.charCodeAt(0)) >>> 0;
  const R = rng(h);
  TRACKS.push({ name: 'Päeva väljakutse: ' + LV_ADJ[Math.floor(R() * LV_ADJ.length)] + ' ' + LV_NOUN[Math.floor(R() * LV_NOUN.length)], gen: true, level: 0, daily: true, build: genTrack(h % 1000000 + 77, 6 + Math.floor(R() * 3), [...SEG_EASY.slice(2), ...SEG_MID, ...SEG_HARD]) });
}
const COOPS = TRACKS.map((t, i) => i).filter(i => TRACKS[i].coop);
function drawStatBoard() {
  const B = TR && TR.board2; if (!B) return;
  const key = coins + JSON.stringify(prog) + myName + myPet;
  if (key === B.key) return; B.key = key;
  const x = B.cv.getContext('2d');
  x.fillStyle = '#ffffff'; x.fillRect(0, 0, 640, 480);
  x.fillStyle = '#2f9bff'; x.font = 'bold 50px system-ui'; x.textAlign = 'center'; x.fillText('Sinu statistika', 320, 62);
  x.fillStyle = '#1d1b3a'; x.font = 'bold 32px system-ui'; x.textAlign = 'left';
  const lv = myLevel(), x0 = 40 * (lv - 1) * (lv - 1), x1 = 40 * lv * lv;
  const rows = ['⭐ Tase ' + lv + '   🪙 ' + coins + '   👑 ' + prog.crowns + '   🔥 ' + (prog.streak || 1), leagueText()];
  rows.forEach((r, i) => x.fillText(r, 40, 118 + i * 44));
  x.fillStyle = '#e9ecef'; x.fillRect(40, 196, 560, 18); x.fillStyle = '#22c55e'; x.fillRect(40, 196, 560 * Math.min(1, (prog.xp - x0) / Math.max(1, x1 - x0)), 18);
  ensureQuests();
  x.fillStyle = '#ff3d8b'; x.font = 'bold 28px system-ui'; x.fillText('Tänased ülesanded (+20 🪙):', 40, 258);
  x.fillStyle = '#1d1b3a'; x.font = 'bold 25px system-ui';
  prog.quests.forEach((q, i) => x.fillText(questText(q), 50, 296 + i * 34));
  const best = Object.entries(prog.best || {}).sort((a, b) => a[1] - b[1]).slice(0, 2);
  x.fillStyle = '#555'; x.font = 'bold 22px system-ui';
  if (best.length) x.fillText('Rekordid: ' + best.map(([k, v]) => k + ' ' + v.toFixed(1) + 's').join(' · '), 40, 420);
  B.tex.needsUpdate = true;
}
function drawBoard() {
  const B = TR && TR.board; if (!B) return;
  const names = new Set([...Object.keys(show.crowns || {}), ...Object.keys(show.pts || {})]);
  const list = [...names].map(n => [n, (show.crowns || {})[n] || 0, (show.pts || {})[n] || 0]).sort((a, b) => b[1] - a[1] || b[2] - a[2]).slice(0, 6);
  const key = JSON.stringify(list) + net.hostId + net.peers.size;
  if (key === B.key) return; B.key = key;
  const ctx = B.cv.getContext('2d');
  ctx.fillStyle = '#ffffff'; ctx.fillRect(0, 0, 768, 420);
  ctx.fillStyle = '#ff3d8b'; ctx.font = 'bold 58px system-ui'; ctx.textAlign = 'center'; ctx.fillText(net.on ? 'Ruumi edetabel' : 'Kroonid', 384, 70);
  ctx.font = 'bold 38px system-ui'; ctx.fillStyle = '#1d1b3a';
  if (!list.length) ctx.fillText('Keegi pole veel võitnud', 384, 180);
  list.forEach(([n, c, pt], i) => { ctx.fillStyle = i === 0 ? '#e8590c' : '#1d1b3a'; ctx.textAlign = 'left'; ctx.fillText((i + 1) + '. ' + n, 80, 136 + i * 43); ctx.textAlign = 'right'; ctx.fillText('👑' + c + '   ⭐' + pt, 690, 136 + i * 43); });
  ctx.textAlign = 'center'; ctx.fillStyle = '#22a35a'; ctx.font = 'bold 34px system-ui';
  ctx.fillText(net.on && net.peers.size ? 'Mängijaid ' + humanIds().length + '/' + MAX_HUMANS + ' · alustab: ' + nameOf(net.hostId) : 'Vajuta ALUSTA või astu ringile', 384, 400);
  B.tex.needsUpdate = true;
}

const stars = (() => {
  const n = 700, g = new THREE.BufferGeometry(), pos = new Float32Array(n * 3);
  for (let i = 0; i < n; i++) { const u = Math.random() * 2 - 1, a = Math.random() * 6.283, r = 240, y = Math.abs(u) * 0.9 + 0.08; const c = Math.sqrt(1 - y * y); pos.set([Math.cos(a) * c * r, y * r, Math.sin(a) * c * r], i * 3); }
  g.setAttribute('position', new THREE.BufferAttribute(pos, 3));
  const p = new THREE.Points(g, new THREE.PointsMaterial({ color: 0xffffff, size: 1.4, sizeAttenuation: false, fog: false }));
  p.visible = false; p.frustumCulled = false; scene.add(p); return p;
})();
let envNight = null;
function applyEnv(night) {
  if (envNight === night) return; envNight = night;
  const hz = night ? 0x141a4d : HORIZON;
  scene.background = new THREE.Color(hz); scene.fog.color.setHex(hz); scene.fog.near = night ? 30 : 55; scene.fog.far = night ? 150 : 190;
  sky.material.uniforms.top.value.setHex(night ? 0x02031a : 0x2f7dff);
  sky.material.uniforms.mid.value.setHex(hz);
  sky.material.uniforms.bot.value.setHex(night ? 0x3b0f5a : 0xffc8e6);
  gooMat.uniforms.fogC.value.setHex(hz); gooMat.uniforms.nightF.value = night ? 1 : 0;
  hemi.intensity = night ? 0.45 : 1.0; hemi.color.setHex(night ? 0x8899ff : 0xeaf6ff);
  sun.intensity = night ? 0.55 : 1.7; sun.color.setHex(night ? 0xaabbff : 0xfff4e0);
  cloudMat.emissiveIntensity = night ? 0.0 : 0.25; cloudMat.color.setHex(night ? 0x4a4f8a : 0xffffff);
  stars.visible = night;
}
// ---------- boonuskastid ----------
const POWERS = [
  { id: 'kiirus', name: '⚡ Kiirus', t: 4 }, { id: 'hupe', name: '🦘 Superhüpe', t: 6 }, { id: 'kilp', name: '🛡️ Kilp', t: 6 },
  { id: 'magnet', name: '🧲 Magnet', t: 8 }, { id: 'banaan', name: '🍌 Banaan', t: 0 },
];
const boxTex = (() => { const cv = document.createElement('canvas'); cv.width = cv.height = 64; const x = cv.getContext('2d'); x.fillStyle = '#ffd43b'; x.fillRect(0, 0, 64, 64); x.strokeStyle = '#f08c00'; x.lineWidth = 6; x.strokeRect(3, 3, 58, 58); x.fillStyle = '#ffffff'; x.font = 'bold 46px system-ui'; x.textAlign = 'center'; x.textBaseline = 'middle'; x.fillText('?', 32, 35); const t = new THREE.CanvasTexture(cv); t.colorSpace = THREE.SRGBColorSpace; return t; })();
const boxMat = new THREE.MeshToonMaterial({ map: boxTex, gradientMap: gradTex, emissive: 0x664400, emissiveIntensity: 0.3 });
const boxGeo = new RoundedBoxGeometry(0.8, 0.8, 0.8, 2, 0.12);
function addBox(x, y, z) { const m = new THREE.Mesh(boxGeo, boxMat); m.position.set(x, y, z); m.castShadow = true; trackGroup.add(m); TR.boxes.push({ m, x, y, z, back: 0, botBack: 0 }); }
function placeBoxes(rt) {
  if (rt !== 'race' || !TR.wps.length) return;
  [0.3, 0.55, 0.8].forEach(f => {
    const w = TR.wps[Math.floor(TR.wps.length * f)]; if (!w || w.x === undefined) return;
    const gt = groundTopAt(w.x, w.z, 50); if (gt > -5) [-1.6, 1.6].forEach(dx => addBox(w.x + dx, gt + 1.1, w.z));
  });
}
function givePower(e, isMe) {
  const pool = isMe ? POWERS : POWERS.filter(p => p.id !== 'banaan' && p.id !== 'magnet');
  const pw = pool[Math.floor(Math.random() * pool.length)];
  if (pw.id === 'banaan') { dropBanana(e); }
  else { e.pw = pw.id; e.pwT = now() + pw.t; if (pw.id === 'kilp') e.shieldT = now() + pw.t; }
  if (isMe) { sfx('fanfare'); showToast(pw.name + (pw.t ? ' ' + pw.t + ' s!' : ' – kukkus su taha!'), 1.8); questTick('box'); if (show.stage === 'trenn') practiceDone('box'); }
}
const bananas = [];
let banId = 0;
const banMat = mat(0xffe066);
function addBanana(id, x, y, z) {
  const g = new THREE.Group();
  const b = new THREE.Mesh(new THREE.TorusGeometry(0.22, 0.07, 6, 14, Math.PI), banMat); b.rotation.x = -Math.PI / 2; g.add(b);
  const tip = new THREE.Mesh(new THREE.SphereGeometry(0.05, 6, 5), mat(0x6b4f2a)); tip.position.set(0.22, 0, 0); g.add(tip);
  g.position.set(x, y + 0.06, z); trackGroup.add(g);
  bananas.push({ id, x, y, z, g, gone: false, t: now() });
}
function dropBanana(e) {
  camera.getWorldDirection(fw); fw.y = 0; fw.normalize();
  const id = net.id + '-' + (banId++), x = r2(e.pos.x - fw.x * 1.3), z = r2(e.pos.z - fw.z * 1.3), y = r2(e.pos.y);
  addBanana(id, x, y, z);
  if (net.on && net.peers.size) net.item.send({ k: 'ban', id, x, y, z, r: show.rid });
}
function slipOn(e, bn) {
  bn.gone = true; trackGroup.remove(bn.g);
  const a = Math.random() * 6.28; e.vel.set(Math.cos(a) * 5, 4, Math.sin(a) * 5); e.knock = 0.9; e.onGround = false; e.standingOn = null;
  if (e.isPlayer) { hurt(e); showToast('🍌 Libisesid banaanikoorel!', 1.4); }
  if (net.on && net.peers.size) net.item.send({ k: 'gone', id: bn.id, r: show.rid });
}
// ---------- pealtvaatajate pallid (ja bossi meteoorid kasutavad sama loogikat) ----------
const drops = [];
let dropCd = 0;
const dropGeo = new THREE.SphereGeometry(0.8, 18, 12);
function addDrop(x, y, z, vx, vz, color) {
  const m = new THREE.Mesh(dropGeo, mat(color || 0xff3d8b)); m.position.set(x, y, z); m.castShadow = true; trackGroup.add(m);
  drops.push({ m, pos: new V3(x, y, z), vel: new V3(vx || 0, 0, vz || 0), r: 0.8, born: now() });
}
function updateDrops(dt) {
  for (let i = drops.length - 1; i >= 0; i--) {
    const d = drops[i];
    d.vel.y -= G * dt; d.pos.addScaledVector(d.vel, dt);
    const gt = groundTopAt(d.pos.x, d.pos.z, d.pos.y);
    if (gt > -50 && d.pos.y - d.r < gt) { d.pos.y = gt + d.r; if (d.vel.y < -3) { puff({ x: d.pos.x, y: gt, z: d.pos.z }, 0xffffff, 5, 2, 0.6, 0.5); sfx('tap'); } d.vel.y = Math.abs(d.vel.y) > 3 ? -d.vel.y * 0.4 : 0; d.vel.x *= 0.995; d.vel.z *= 0.995; }
    d.m.position.copy(d.pos); d.m.rotation.x += d.vel.z * dt; d.m.rotation.z -= d.vel.x * dt;
    if (now() - d.born > 7 || d.pos.y < -20) { trackGroup.remove(d.m); drops.splice(i, 1); }
  }
}
function hitDrop(e, d) {
  if (e.shieldT > now()) return;
  const dx = e.pos.x - d.pos.x, dz = e.pos.z - d.pos.z, dd = Math.hypot(dx, dz);
  if (dd < d.r + e.r && e.pos.y < d.pos.y + d.r && e.pos.y + e.h > d.pos.y - d.r && dd > 1e-4) {
    const nx = dx / dd, nz = dz / dd;
    e.pos.x = d.pos.x + nx * (d.r + e.r); e.pos.z = d.pos.z + nz * (d.r + e.r);
    e.vel.set(nx * 6, 5, nz * 6); e.knock = 0.6; e.onGround = false; e.standingOn = null; hurt(e);
  }
}
function aimGround(o, d) {   // kiir maapinnani (pealtvaataja sihib)
  for (let t = 2; t < 90; t += 0.6) {
    const x = o.x + d.x * t, y = o.y + d.y * t, z = o.z + d.z * t;
    const gt = groundTopAt(x, z, y + 0.3);
    if (gt > -50 && y <= gt + 0.3) return { x, y: gt, z };
    if (y < -8) return null;
  }
  return null;
}
function spectatorDrop(i) {
  if (!spectating || show.stage !== 'play' || dropCd > 0) return;
  let o, d;
  if (renderer.xr.isPresenting) { const r = rays[i == null ? 1 : i]; r.c.updateMatrixWorld(true); o = tmpA.setFromMatrixPosition(r.c.matrixWorld).clone(); d = tmpB.set(0, 0, -1).transformDirection(r.c.matrixWorld).clone(); }
  else { o = camera.getWorldPosition(tmpA).clone(); d = camera.getWorldDirection(tmpB).clone(); }
  const hit = aimGround(o, d); if (!hit) { showToast('Siht ei ole rajal', 1); return; }
  dropCd = 3.5; sfx('pop');
  const vx = rnd(-1, 1), vz = rnd(-1, 1), col = [0xff3d8b, 0x2f9bff, 0xffd43b, 0x22c55e][Math.floor(Math.random() * 4)];
  addDrop(hit.x, hit.y + 9, hit.z, vx, vz, col);
  if (net.on && net.peers.size) net.item.send({ k: 'drop', x: r2(hit.x), y: r2(hit.y + 9), z: r2(hit.z), vx: r2(vx), vz: r2(vz), c: col, r: show.rid });
}
const RAIN_N = 900;
const rainGeo = new THREE.BufferGeometry(), rainPos = new Float32Array(RAIN_N * 3);
for (let i = 0; i < RAIN_N; i++) rainPos.set([rnd(-15, 15), rnd(0, 18), rnd(-15, 15)], i * 3);
rainGeo.setAttribute('position', new THREE.BufferAttribute(rainPos, 3));
const rain = new THREE.Points(rainGeo, new THREE.PointsMaterial({ color: 0xa5d8ff, size: 0.09, transparent: true, opacity: 0.8 }));
rain.visible = false; rain.frustumCulled = false; scene.add(rain);
let raining = false;
function setRain(on) {
  raining = on; rain.visible = on;
  envNight = null; applyEnv(!!(TR && TR.night));
  if (on) {
    const g = 0x8fa3b8; scene.background = new THREE.Color(g); scene.fog.color.setHex(g); scene.fog.near = 30; scene.fog.far = 140;
    sky.material.uniforms.top.value.setHex(0x5b6b80); sky.material.uniforms.mid.value.setHex(g); sky.material.uniforms.bot.value.setHex(0x9aa5b1);
    gooMat.uniforms.fogC.value.setHex(g); sun.intensity = 0.9; hemi.intensity = 0.85; cloudMat.color.setHex(0x9aa5b1);
  }
}
function updateRain(dt) {
  if (!raining) return;
  rain.position.set(player.pos.x, player.pos.y - 2, player.pos.z);
  for (let i = 0; i < RAIN_N; i++) { let y = rainPos[i * 3 + 1] - dt * 16; if (y < 0) y += 18; rainPos[i * 3 + 1] = y; }
  rainGeo.attributes.position.needsUpdate = true;
}
const coinGeo = new THREE.CylinderGeometry(0.28, 0.28, 0.07, 20);
const coinMat = new THREE.MeshToonMaterial({ color: 0xffd43b, emissive: 0xffa400, emissiveIntensity: 0.35, gradientMap: gradTex });
function addCoin(x, y, z) { const m = new THREE.Mesh(coinGeo, coinMat); m.rotation.x = Math.PI / 2; m.position.set(x, y, z); m.castShadow = true; trackGroup.add(m); TR.coins.push({ m, taken: false }); }
function placeCoins(rt) {
  if (rt === 'race') {
    TR.wps.forEach((w, i) => { if (w.x === undefined || w.side) return; const gt = groundTopAt(w.x, w.z, 50); if (gt > -5) addCoin(w.x + (i % 2 ? 0.8 : -0.8), gt + 1.0, w.z); });
  } else if (rt !== 'plaadid') {
    for (let i = 0; i < 6; i++) { const a = i / 6 * 6.283, x = Math.cos(a) * 4.5, z = -12 + Math.sin(a) * 4.5, gt = groundTopAt(x, z, 50); if (gt > -5) addCoin(x, gt + 1.0, z); }
  }
}
function mapName(rt, ti) { return rt === 'race' ? TRACKS[ti].name : rt === 'lobby' ? 'Ootesaal' : rt === 'trenn' ? 'Harjutusväljak' : ARENAS[rt].name; }
function buildMap(rt, ti) {
  trackGroup.traverse(o => { if (o.geometry) o.geometry.dispose(); if (o.isSprite && o.material.map) o.material.map.dispose(); });
  trackGroup.clear();
  geoCache = new Map();
  TR = newTR(rt, mapName(rt, ti));
  if (rt === 'lobby') buildLobby();
  else if (rt === 'trenn') buildTrenn();
  else if (rt === 'race') TRACKS[ti].build();
  else ARENAS[rt].build();
  if (rt !== 'lobby' && rt !== 'trenn') specPlatform();
  decorate(rt);
  applyEnv(!!TR.night);
  if (rt !== 'lobby' && rt !== 'trenn') { placeCoins(rt); }
  drops.length = 0; bananas.length = 0;
  setRain(show.weather === 'vihm' && rt === 'race' && !TR.night);
  if (rt === 'lobby') rebuildBuilder(true);
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
  else if (type === 6) { const br = new THREE.Mesh(new THREE.CylinderGeometry(0.3, 0.3, 0.03, 20), mat(0x5f3dc4)); g.add(br); const c = new THREE.Mesh(new THREE.ConeGeometry(0.17, 0.5, 14), mat(0x5f3dc4)); c.position.y = 0.26; c.rotation.z = 0.15; g.add(c); for (let i = 0; i < 4; i++) { const st = new THREE.Mesh(new THREE.SphereGeometry(0.03, 6, 5), mat(0xffd43b)); st.position.set(Math.cos(i * 1.6) * 0.1, 0.12 + i * 0.08, Math.sin(i * 1.6) * 0.1); g.add(st); } }
  else if (type === 7) { const c = new THREE.Mesh(new THREE.CylinderGeometry(0.17, 0.17, 0.2, 16), mat(0xffffff)); c.position.y = 0.1; g.add(c); [[0, 0.26, 0], [0.1, 0.24, 0.05], [-0.1, 0.24, -0.04], [0.02, 0.25, -0.1]].forEach(([x, y, z]) => { const p = new THREE.Mesh(new THREE.SphereGeometry(0.12, 10, 8), mat(0xffffff)); p.position.set(x, y, z); g.add(p); }); }
  else if (type === 8) { const h = new THREE.Mesh(new THREE.SphereGeometry(0.22, 14, 8, 0, Math.PI * 2, 0, Math.PI / 2), mat(0x868e96)); h.position.y = -0.05; g.add(h); [-1, 1].forEach(sd => { const hr = new THREE.Mesh(new THREE.ConeGeometry(0.05, 0.3, 10), mat(0xfff9db)); hr.position.set(sd * 0.22, 0.12, 0); hr.rotation.z = -sd * 0.9; g.add(hr); }); }
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
  const ring = new THREE.Mesh(new THREE.TorusGeometry(0.34, 0.06, 8, 24), new THREE.MeshBasicMaterial({ color: 0xffffff })); ring.rotation.x = Math.PI / 2; ring.position.y = headTop + 0.42; ring.visible = false; g.add(ring); g.userData.teamRing = ring;
}
const BOT_GEO = { body: new RoundedBoxGeometry(0.6, 0.7, 0.5, 3, 0.14), head: new RoundedBoxGeometry(0.55, 0.42, 0.48, 3, 0.12), visor: new RoundedBoxGeometry(0.44, 0.22, 0.04, 2, 0.02), leg: new RoundedBoxGeometry(0.18, 0.18, 0.22, 2, 0.05) };
const eyeM = new THREE.MeshBasicMaterial({ color: 0x9ffcff });
function makeBot(color, hat) {
  const g = new THREE.Group();
  const body = new THREE.Mesh(BOT_GEO.body, mat(color)); body.position.y = 0.53; g.add(body);
  const belly = new THREE.Mesh(new THREE.CircleGeometry(0.16, 16), mat(0xffffff)); belly.position.set(0, 0.5, 0.252); g.add(belly);
  const head = new THREE.Mesh(BOT_GEO.head, mat(color)); head.position.y = 1.1; g.add(head);
  const visor = new THREE.Mesh(BOT_GEO.visor, mat(0x1b1b2f)); visor.position.set(0, 1.12, 0.23); g.add(visor);
  [-0.1, 0.1].forEach(x => { const e = new THREE.Mesh(new THREE.CircleGeometry(0.045, 12), eyeM); e.position.set(x, 1.13, 0.255); g.add(e); });
  [-0.15, 0.15].forEach(x => { const l = new THREE.Mesh(BOT_GEO.leg, mat(0x333344)); l.position.set(x, 0.09, 0); g.add(l); });
  if (hat) { const h = makeHat(hat, 1.31); g.add(h); g.userData.hat = h; }
  else { const ant = new THREE.Mesh(new THREE.CylinderGeometry(0.02, 0.02, 0.25), mat(0x333344)); ant.position.y = 1.43; g.add(ant); }
  addExtras(g, 1.31);
  shadowAll(g, true, false);
  scene.add(g); return g;
}
function makeHuman(color, name, hat, costume = 0) {
  const g = new THREE.Group();
  const bodyC = costume === 2 ? 0xf1f3f5 : costume === 4 ? 0x222831 : color;
  const body = new THREE.Mesh(new THREE.CapsuleGeometry(0.3, 0.5, 6, 14), mat(bodyC)); body.position.y = 0.6; g.add(body);
  addCostume(g, costume, color);
  const head = new THREE.Mesh(new THREE.SphereGeometry(0.27, 16, 12), mat(color)); head.position.y = 1.22; g.add(head);
  const face = new THREE.Mesh(new THREE.SphereGeometry(0.2, 14, 10), mat(0xffe0c2)); face.position.set(0, 1.22, 0.12); face.scale.set(1, 0.85, 0.6); g.add(face);
  [-0.07, 0.07].forEach(x => { const e = new THREE.Mesh(new THREE.SphereGeometry(0.03, 8, 6), mat(0x111122)); e.position.set(x, 1.25, 0.25); g.add(e); });
  if (hat) { const h = makeHat(hat, 1.47); g.add(h); g.userData.hat = h; }
  [-0.1, 0.1].forEach(x => { const c = new THREE.Mesh(new THREE.CircleGeometry(0.035, 10), mat(0xff9fb5)); c.position.set(x * 1.5, 1.17, 0.235); g.add(c); });
  const tag = nameSprite(name, color); tag.position.y = 2.05; g.add(tag);
  addExtras(g, 1.49);
  shadowAll(g, true, false);
  scene.add(g); return g;
}
function addCostume(g, k, color) {
  if (k === 1) {   // dinosaurus
    for (let i = 0; i < 5; i++) { const sp = new THREE.Mesh(new THREE.ConeGeometry(0.07, 0.18, 6), mat(0xffd43b)); sp.position.set(0, 0.45 + i * 0.22, -0.27 - (i === 4 ? 0.02 : 0)); sp.rotation.x = -0.5; g.add(sp); }
    const sn = new THREE.Mesh(new RoundedBoxGeometry(0.3, 0.16, 0.2, 2, 0.06), mat(color)); sn.position.set(0, 1.12, 0.27); g.add(sn);
    const tl = new THREE.Mesh(new THREE.ConeGeometry(0.17, 0.7, 10), mat(color)); tl.position.set(0, 0.32, -0.55); tl.rotation.x = -1.25; g.add(tl);
  } else if (k === 2) {   // astronaut
    const band = new THREE.Mesh(new THREE.TorusGeometry(0.31, 0.05, 8, 24), mat(color)); band.rotation.x = Math.PI / 2; band.position.y = 0.62; g.add(band);
    const hel = new THREE.Mesh(new THREE.SphereGeometry(0.34, 18, 14), new THREE.MeshBasicMaterial({ color: 0x99ddff, transparent: true, opacity: 0.28, depthWrite: false })); hel.position.y = 1.22; g.add(hel);
    const bp = new THREE.Mesh(new RoundedBoxGeometry(0.42, 0.48, 0.2, 2, 0.06), mat(0xadb5bd)); bp.position.set(0, 0.75, -0.33); g.add(bp);
  } else if (k === 3) {   // kass
    [-0.14, 0.14].forEach(x => { const e = new THREE.Mesh(new THREE.ConeGeometry(0.09, 0.2, 4), mat(color)); e.position.set(x, 1.5, 0); g.add(e); });
    [-1, 1].forEach(sd => [0.02, -0.02].forEach(dy => { const w = new THREE.Mesh(new THREE.BoxGeometry(0.22, 0.008, 0.008), mat(0x222222)); w.position.set(sd * 0.17, 1.17 + dy, 0.24); w.rotation.z = sd * dy * 8; g.add(w); }));
    const tl = new THREE.Mesh(new THREE.CylinderGeometry(0.045, 0.03, 0.75, 8), mat(color)); tl.position.set(0, 0.6, -0.42); tl.rotation.x = -0.6; g.add(tl);
  } else if (k === 4) {   // pingviin
    const bel = new THREE.Mesh(new THREE.SphereGeometry(0.26, 14, 10), mat(0xffffff)); bel.scale.set(0.9, 1.4, 0.5); bel.position.set(0, 0.6, 0.15); g.add(bel);
    const bk = new THREE.Mesh(new THREE.ConeGeometry(0.06, 0.16, 8), mat(0xff922b)); bk.rotation.x = Math.PI / 2; bk.position.set(0, 1.16, 0.3); g.add(bk);
    [-1, 1].forEach(sd => { const f = new THREE.Mesh(new RoundedBoxGeometry(0.08, 0.4, 0.18, 2, 0.03), mat(0x222831)); f.position.set(sd * 0.33, 0.7, 0); f.rotation.z = sd * 0.25; g.add(f); const ft = new THREE.Mesh(new RoundedBoxGeometry(0.14, 0.05, 0.22, 2, 0.02), mat(0xff922b)); ft.position.set(sd * 0.12, 0.03, 0.08); g.add(ft); });
  }
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
function makePet(k) {
  const g = new THREE.Group();
  const c = [0, 0xb07040, 0x9aa5b1, 0x74c0fc, 0xffd43b][k] || 0xffffff;
  if (k === 4) {
    const b = new THREE.Mesh(new THREE.SphereGeometry(0.16, 12, 10), mat(c)); b.position.y = 0.17; g.add(b);
    const bk = new THREE.Mesh(new THREE.ConeGeometry(0.04, 0.08, 6), mat(0xff922b)); bk.rotation.x = Math.PI / 2; bk.position.set(0, 0.2, 0.16); g.add(bk);
  } else if (k === 3) {
    const b = new THREE.Mesh(new RoundedBoxGeometry(0.28, 0.26, 0.26, 2, 0.05), mat(c)); b.position.y = 0.2; g.add(b);
    const v = new THREE.Mesh(new THREE.BoxGeometry(0.2, 0.08, 0.02), new THREE.MeshBasicMaterial({ color: 0x9ffcff })); v.position.set(0, 0.24, 0.135); g.add(v);
    const a = new THREE.Mesh(new THREE.SphereGeometry(0.03, 6, 5), new THREE.MeshBasicMaterial({ color: 0xff3d8b })); a.position.y = 0.42; g.add(a);
  } else {
    const b = new THREE.Mesh(new THREE.CapsuleGeometry(0.1, 0.18, 4, 10), mat(c)); b.rotation.x = Math.PI / 2; b.position.y = 0.16; g.add(b);
    const h = new THREE.Mesh(new THREE.SphereGeometry(0.11, 12, 10), mat(c)); h.position.set(0, 0.28, 0.17); g.add(h);
    [-1, 1].forEach(sd => { const e = new THREE.Mesh(k === 2 ? new THREE.ConeGeometry(0.04, 0.09, 4) : new THREE.SphereGeometry(0.045, 6, 5), mat(k === 1 ? 0x6b4226 : c)); e.position.set(sd * 0.07, 0.38, 0.15); if (k === 1) e.scale.set(0.7, 1.4, 0.7); g.add(e); });
    const tl = new THREE.Mesh(new THREE.CylinderGeometry(0.02, 0.015, 0.2, 6), mat(c)); tl.position.set(0, 0.22, -0.2); tl.rotation.x = -0.8; g.add(tl);
  }
  shadowAll(g, true, false); scene.add(g); return g;
}
const petMeshes = new Map();   // id -> { k, g, ph }
function petFor(id, k, pos, yaw, dt) {
  let pm = petMeshes.get(id);
  if (!k) { if (pm) { scene.remove(pm.g); petMeshes.delete(id); } return; }
  if (!pm || pm.k !== k) { if (pm) scene.remove(pm.g); pm = { k, g: makePet(k), ph: Math.random() * 6 }; pm.g.position.set(pos.x, pos.y, pos.z); petMeshes.set(id, pm); }
  const tx = pos.x + Math.sin(yaw + 2.4) * 0.9, tz = pos.z + Math.cos(yaw + 2.4) * 0.9;
  const g = pm.g, dx = tx - g.position.x, dz = tz - g.position.z, d = Math.hypot(dx, dz);
  if (d > 6) g.position.set(tx, pos.y, tz);
  else if (d > 0.15) { g.position.x += dx * Math.min(1, dt * 4); g.position.z += dz * Math.min(1, dt * 4); g.rotation.y = Math.atan2(dx, dz); }
  pm.ph += dt * (d > 0.3 ? 14 : 3);
  g.position.y += ((pos.y + Math.abs(Math.sin(pm.ph)) * (d > 0.3 ? 0.12 : 0.03)) - g.position.y) * Math.min(1, dt * 10);
}
function updatePets(dt) {
  camera.getWorldDirection(fw);
  petFor(net.id, spectating && show.stage === 'play' ? 0 : myPet, player.pos, Math.atan2(fw.x, fw.z), dt);
  for (const p of net.peers.values()) if (p.d) petFor(p.id, p.d.pt || 0, p.pos, p.avatar.rotation.y, dt);
  for (const [id] of petMeshes) if (id !== net.id && !net.peers.has(id)) { scene.remove(petMeshes.get(id).g); petMeshes.delete(id); }
}
const shieldBubble = new THREE.Mesh(new THREE.SphereGeometry(0.9, 16, 12), new THREE.MeshBasicMaterial({ color: 0x74c0fc, transparent: true, opacity: 0.25, depthWrite: false }));
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
const botHat = i => (i * 7 + 3) % FREE_HATS;
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
  if (u.teamRing) { const tm = TEAM[show.rt] && show.stage !== 'lobby' && show.teams ? show.teams[id] : undefined; u.teamRing.visible = tm !== undefined; if (tm !== undefined) u.teamRing.material.color.setHex(TEAM_COLORS[tm]); }
  const duckT = u.duck ? 0.62 : 1; g.scale.y += (duckT - g.scale.y) * Math.min(1, dt * 12);
  if (show.stage === 'crown' && show.winner === id) { g.rotation.y += dt * 5; g.position.y += Math.abs(Math.sin(now() * 6)) * 0.45; }
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
    if (s.type === 'ramp') { if (inRamp(s, x, z)) { const rt0 = rampTop(s, z); if (rt0 <= y + 0.3 && rt0 > best) best = rt0; } continue; }
    const inside = s.type === 'box' ? (Math.abs(x - p.x) <= s.hw + 0.12 && Math.abs(z - p.z) <= s.hd + 0.12) : Math.hypot(x - p.x, z - p.z) <= s.r;
    if (inside && top > best) best = top;
  }
  return best;
}
function stepAhead(x, z, y) {
  for (const s of TR.solids) {
    if (!s.active || s.pusher || s.type === 'ramp') continue; const p = s.mesh.position, top = p.y + s.hh;
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
  if (e.standingOn && e.standingOn.active && !e.holding) {
    const so0 = e.standingOn;
    e.pos.add(so0.delta);
    if (so0.spin && so0.spinD) {   // pöörlev ketas kannab kaasa
      const c = so0.mesh.position, dx = e.pos.x - c.x, dz = e.pos.z - c.z, ca = Math.cos(so0.spinD), sa = Math.sin(so0.spinD);
      e.pos.x = c.x + dx * ca + dz * sa; e.pos.z = c.z - dx * sa + dz * ca;
    }
    if (so0.conveyor) e.pos.z += so0.conveyor * S() * dt;
  }
  for (const w of TR.winds) if (Math.abs(e.pos.x - w.x) < w.hw + 1 && e.pos.z < w.z0 && e.pos.z > w.z1 && e.pos.y > -1) e.pos.x += w.amp * S() * Math.sin(T() * w.speed) * dt;
  if (!e.holding && !e.carried) e.vel.y -= G * twG() * dt;
  if (twOn('tuul') && show.stage === 'play' && e.pos.y > -1) e.pos.x += Math.sin(T() * 0.55) * 1.7 * dt;
  const prevY = e.pos.y;
  e.pos.addScaledVector(e.vel, dt);
  const wasGround = e.onGround;
  e.onGround = false; e.standingOn = null;
  for (const s of TR.solids) {
    if (!s.active) continue;
    if (s.type === 'ramp') {
      if (inRamp(s, e.pos.x, e.pos.z)) {
        const rt0 = rampTop(s, e.pos.z);
        if (e.vel.y <= 0.5 && prevY >= rt0 - 0.45 && e.pos.y <= rt0 + 0.05) { e.pos.y = rt0; e.vel.y = 0; e.onGround = true; e.standingOn = s; }
      }
      continue;
    }
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
            if (s.pusher && !(e.shieldT > now())) { e.vel.x = s.vel.x * 1.4 + Math.sign(dx || 1) * 1.5; e.knock = 0.35; if (e.isPlayer && Math.abs(s.vel.x) > 0.5) hurt(e); } else e.vel.x = 0;
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
  if (so && so.ramp) { const dir = Math.sign(so.zBot - so.zTop); e.vel.z += dir * 7 * dt; }
  if (so && so.bounce && !e.holding) { e.vel.y = BOUNCE; e.onGround = false; if (e.isPlayer) { sfx('bounce'); clearHands(); e.bounceT = 2.5; } if (nearPlayer(e)) puff(e.pos, 0xffd43b, 8, 2.2, 0.6, 0.3); }
  if (e.onGround && !wasGround && prevY - e.pos.y > -0.01 && nearPlayer(e) && !(so && so.bounce)) puff(e.pos, 0xffffff, e.isPlayer ? 5 : 3, 1.0, 0.45, 0.4);
  if (so && so.tile && show.stage !== 'intro') shakeTile(so.idx, true);
  if (e.isPlayer && e.onGround && !wasGround && !so?.bounce) sfx('tap');
  if (so && e.onGround && !so.move && !so.tile && !so.bounce && !so.spec) e.lastSafe.copy(e.pos);
  for (const sw of TR.sweepers) hitSweeper(e, sw);
  for (const b of TR.balls) hitBall(e, b);
  for (const p of TR.pendulums) {
    if (e.shieldT > now()) break;
    const dx = e.pos.x - p.pos.x, dz = e.pos.z - p.pos.z, d = Math.hypot(dx, dz);
    if (d < p.r + e.r && e.pos.y < p.pos.y + p.r && e.pos.y + e.h > p.pos.y - p.r) {
      const nx = d > 1e-4 ? dx / d : Math.sign(p.vx) || 1, nz = d > 1e-4 ? dz / d : 0;
      e.pos.x = p.pos.x + nx * (p.r + e.r); e.pos.z = p.pos.z + nz * (p.r + e.r);
      e.vel.set(nx * 3 + Math.sign(p.vx) * Math.min(8, Math.abs(p.vx) * 1.3 + 2), 5, nz * 3); e.knock = 0.6; e.onGround = false; e.standingOn = null;
      hurt(e);
    }
  }
  for (const b of drops) hitDrop(e, b);
  for (const bn of bananas) if (!bn.gone && Math.hypot(e.pos.x - bn.x, e.pos.z - bn.z) < 0.6 && Math.abs(e.pos.y - bn.y) < 0.6 && !(e.shieldT > now())) slipOn(e, bn);
  e.knock = Math.max(0, e.knock - dt);
}
function nearPlayer(e) { return e.isPlayer || (Math.abs(e.pos.x - player.pos.x) < 30 && Math.abs(e.pos.z - player.pos.z) < 30); }
function splash(e) { if (nearPlayer(e)) puff({ x: e.pos.x, y: goo.position.y + 0.2, z: e.pos.z }, 0xff5fa2, 12, 2.5, 0.9, 2.5); }
function hitSweeper(e, sw) {
  if (!sw.active || e.shieldT > now()) return;
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
  if (b.r < 0.3 || e.shieldT > now()) return;
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
  const k = e.knock > 0 ? 1 : (e.onGround ? (ice ? 1.3 : twOn('libe') ? 2.2 : raining ? 3.5 : 10) : 2.5);
  const f = Math.min(1, k * dt), m = (e.speedMul || 1) * (e.pw === 'kiirus' && e.pwT > now() ? 1.6 : 1);
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
    if (!s.active || s.type === 'ramp') continue;
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
        if (c.s.bounce) { player.vel.set(0, BOUNCE, 0); player.bounceT = 2.5; h.touch = false; cnt--; sfx('bounce'); }
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
    if (tv.length() > 0.5) { const boost = player.pw === 'hupe' && player.pwT > now() ? 1.5 : player.pw === 'kiirus' && player.pwT > now() ? 1.3 : 1; tv.multiplyScalar(1.3 * boost * (twOn('hupe') ? 1.25 : 1)); if (tv.length() > 10 * boost) tv.setLength(10 * boost); player.vel.copy(tv); }
    flingBuf.length = 0;
  } else if (player.onGround) {
    const ice = player.standingOn && player.standingOn.ice;
    const f = Math.exp(-(ice ? 0.6 : raining ? 1.6 : 5) * dt); player.vel.x *= f; player.vel.z *= f;
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
function bodyDist(pos, hand) {   // käe kaugus tegelase kehast (püstine lõik jalgadest peani)
  const y = Math.max(pos.y + 0.2, Math.min(pos.y + 1.1, hand.y));
  return Math.hypot(hand.x - pos.x, hand.y - y, hand.z - pos.z);
}
let grabMark = null;
function showGrabMark(p) {   // ✋ märk selle kohal, keda saab haarata
  if (!grabMark) { grabMark = new THREE.Mesh(new THREE.RingGeometry(0.16, 0.24, 20), new THREE.MeshBasicMaterial({ color: 0xffe14d, side: THREE.DoubleSide, transparent: true, opacity: 0.9, depthTest: false })); grabMark.renderOrder = 10; grabMark.rotation.x = -Math.PI / 2; scene.add(grabMark); }
  grabMark.visible = !!p; if (p) grabMark.position.set(p.x, p.y + 1.45 + Math.sin(now() * 6) * 0.05, p.z);
}
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
function applyGrab(targetId, byId, on, th, hp) {
  if (net.isHost && !on && th && th.length === 3 && isHuman(byId)) { hostOnly.thr = hostOnly.thr || {}; hostOnly.thr[byId] = (hostOnly.thr[byId] || 0) + 1; pushFeed('🚀 ' + nameOf(byId) + ' viskas: ' + nameOf(targetId)); }
  const until = on ? now() + 0.6 : 0;
  if (on) markGrab(targetId);
  const tgt = targetId === net.id ? player : (net.isHost ? bots.find(b => b.id === targetId) : null);
  if (tgt) {
    if (on && hp && hp.length === 3 && !(tgt.carryBlock > now())) { if (!tgt.carry) tgt.carryStart = now(); tgt.carry = { p: new V3(hp[0], hp[1], hp[2]), until: now() + 0.35, by: byId }; }
    else if (!on) tgt.carry = null;
  }
  const thr = th && th.length === 3 ? new V3(th[0], th[1], th[2]) : null;
  if (thr && thr.length() > 10) thr.setLength(10);
  if (targetId === net.id) {
    player.grabbedBy = on ? { by: byId, until } : null;
    if (on && !player._grabMsg) { showToast(nameOf(byId) + ' haaras sind!', 1.2); player._grabMsg = 1; }
    if (!on) player._grabMsg = 0;
    if (!on && thr) { player.vel.copy(thr); player.onGround = false; player.standingOn = null; player.knock = 0.25; player.bounceT = 2; clearHands(); sfx('bounce'); showToast(nameOf(byId) + ' viskas sind! 🚀', 1.4); }
    return;
  }
  if (net.isHost) { const b = bots.find(b => b.id === targetId); if (b) { b.grabbedBy = on ? { by: byId, until } : null; if (!on && thr) { b.vel.copy(thr); b.onGround = false; b.standingOn = null; b.knock = 0.4; } } }
}
function sendGrab(targetId, on, th, hp) {
  if (targetId === net.id) return;
  const isLocalBot = net.isHost && targetId.startsWith('#');
  if (th) { sfx('jump'); showToast('Viskasid: ' + nameOf(targetId) + ' 🚀', 1.2); questTick('throw'); if (show.stage === 'trenn' && targetId.startsWith('#')) practiceDone('throw'); }
  if (isLocalBot) applyGrab(targetId, net.id, on, th, hp);
  else if (net.on && net.peers.size) net.grab.send({ t: targetId, g: net.id, on: on ? 1 : 0, th: th || 0, hp: hp || 0 });
}
let carryCounted = {};
function holdPoint(i) {   // kuhu tassitav sõber tõstetakse
  if (renderer.xr.isPresenting) { grips[i].getWorldPosition(tmpC); return [r2(tmpC.x), r2(tmpC.y - 0.85), r2(tmpC.z)]; }
  camera.getWorldDirection(fw); fw.y = 0; fw.normalize();
  return [r2(player.pos.x + fw.x * 0.75), r2(player.pos.y + 1.45), r2(player.pos.z + fw.z * 0.75)];
}
const handVel = [new V3(), new V3()], handPrev = [new V3(), new V3()];
let pcGrabT = 0, grabMissT = 0;
function grabEffect(e, dt) {
  e.speedMul = 1;
  e.carried = false;
  if (e.carry && now() < e.carry.until && !(e.carryBlock > now())) {
    if (now() - (e.carryStart || now()) > 6) { e.carry = null; e.carryBlock = now() + 1.5; if (e.isPlayer) showToast('Rabelesid lahti! 💪', 1.4); return; }
    e.pos.lerp(e.carry.p, Math.min(1, dt * 18)); e.vel.set(0, 0, 0); e.speedMul = 0; e.carried = true; e.onGround = false; e.standingOn = null;
    if (e.isPlayer) clearHands();
    return;
  }
  if (!e.grabbedBy) return;
  if (now() > e.grabbedBy.until) { e.grabbedBy = null; return; }
  const gp = posOf(e.grabbedBy.by); if (!gp) return;
  e.speedMul = 0.35;
  const dx = gp.x - e.pos.x, dz = gp.z - e.pos.z, d = Math.hypot(dx, dz);
  if (d > 1.1 && d < 4) { const m = Math.min(d - 1.1, 2.5 * dt); e.pos.x += dx / d * m; e.pos.z += dz / d * m; }
}
const tmpA = new V3(), tmpB = new V3(), tmpC = new V3();
function updateGrabs(dt, buttons) {
  grabSendT -= dt;
  const resend = grabSendT <= 0; if (resend) grabSendT = 0.1;
  const others = allOthers();
  if (renderer.xr.isPresenting) {
    for (let i = 0; i < 2; i++) {
      const pressed = buttons[i];
      grips[i].getWorldPosition(tmpA);
      handVel[i].lerp(tmpC.copy(tmpA).sub(handPrev[i]).divideScalar(Math.max(dt, 1e-3)), 0.5); handPrev[i].copy(tmpA);
      if (pressed && !grabHold[i]) {
        let best = null, bd = 0.85;
        for (const o of others) { const d = bodyDist(o.pos, tmpA); if (d < bd) { bd = d; best = o; } }
        if (best) { grabHold[i] = best.id; sfx('grab'); buzz(i, 0.6, 40); sendGrab(best.id, true); }
      } else if (grabHold[i]) {
        const tp = posOf(grabHold[i]);
        if (!pressed || !tp || bodyDist(tp, tmpA) > 2.4) {
          // vehkimisega lahti lastes viskad sõbra käe liikumise suunas
          const v = handVel[i], th = !pressed && v.length() > 1.3 ? [r2(v.x * 2.2), r2(Math.max(3, v.y * 2.2 + 3)), r2(v.z * 2.2)] : null;
          sendGrab(grabHold[i], false, th); grabHold[i] = null;
        }
        else if (resend) { sendGrab(grabHold[i], true, null, holdPoint(i)); if (!carryCounted[grabHold[i] + show.rid]) { carryCounted[grabHold[i] + show.rid] = 1; questTick('carry'); } }
      }
    }
    let mk = null, md = 1.0;   // näita ✋ märki, kui käsi on kellelegi piisavalt lähedal
    if (!grabHold[0] && !grabHold[1]) for (let i = 0; i < 2; i++) { grips[i].getWorldPosition(tmpA); for (const o of others) { const d = bodyDist(o.pos, tmpA); if (d < md) { md = d; mk = o; } } }
    showGrabMark(mk ? mk.pos : null);
  } else {
    const pressed = !!keys.KeyE || touchIn.grab;
    if (!pressed) grabMissT = 0;
    let cand = null;
    if (!pcGrab) {
      camera.getWorldDirection(fw); fw.y = 0; fw.normalize();
      let bs = 1e9;
      for (const o of others) {
        const dx = o.pos.x - player.pos.x, dz = o.pos.z - player.pos.z, d = Math.hypot(dx, dz);
        const face = (dx * fw.x + dz * fw.z) / (d || 1);
        if (d < 2.4 && Math.abs(o.pos.y - player.pos.y) < 1.6 && (face > -0.3 || d < 1.1)) { const sc = d - face * 0.6; if (sc < bs) { bs = sc; cand = o; } }
      }
    }
    showGrabMark(!pcGrab && cand ? cand.pos : null);
    if (pressed && !pcGrab) {
      if (cand) { pcGrab = cand.id; pcGrabT = 0; sfx('grab'); sendGrab(cand.id, true); }
      else if (!grabMissT) { showToast('Kedagi pole lähedal – mine robotile ligi', 1); grabMissT = 1; }
    } else if (pcGrab) {
      const tp = posOf(pcGrab);
      pcGrabT += dt;
      if (!pressed || !tp || tp.distanceTo(player.pos) > 3.6) {
        // lahti lastes viskad sõbra ette ja üles (kui hoidsid vähemalt pool sekundit)
        camera.getWorldDirection(fw); fw.y = 0; fw.normalize();
        const th = !pressed && pcGrabT > 0.5 ? [r2(fw.x * 6.5), 6, r2(fw.z * 6.5)] : null;
        sendGrab(pcGrab, false, th); pcGrab = null;
      }
      else if (resend) {
        const carrying = pcGrabT > 0.35;   // hoia kauem = tõstad sõbra üles ja tassid
        sendGrab(pcGrab, true, null, carrying ? holdPoint(0) : null);
        if (carrying && !carryCounted[pcGrab + show.rid]) { carryCounted[pcGrab + show.rid] = 1; questTick('carry'); }
      }
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
      if (sw.high) { if (tHit < 0.3 + half && Math.random() < 0.4 + 0.4 * b.skill) b.duckT = 0.35 + half * 2; }
      else if (half < 0.25 && tHit > 0.14 + half && tHit < 0.45 && Math.random() < 0.35 + 0.4 * b.skill) b.vel.y = JUMP;
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
  b.stuckT = (b.stuckT || 0) + dt;
  if (b.stuckT > 2.5) { if (!TR.balls.length && b.pos.z > (b.lastZ ?? 99) - 0.6) { b.jit = rnd(-1, 1); b.side *= -1; } b.lastZ = b.pos.z; b.stuckT = 0; }
  if (b.finished) { steer(b, 0, 0, dt); if (b.onGround && Math.random() < dt * 0.8) b.vel.y = JUMP * 0.8; return; }
  if (TR.gates.length) for (let i = 0; i < b.wi && i < TR.wps.length; i++) {
    const w = TR.wps[i]; if (w.gate === undefined) continue;
    const gt = TR.gates.find(g => g.k === w.gate);
    if (gt && b.pos.z > gt.z && !gateOpen(w.gate)) { b.wi = i; b.claim = null; break; }
  }
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
  } else if (wp.gate === undefined && (dist < 1.2 || b.pos.z < p.z + (wp.wide ? 0.6 : -0.3))) b.wi = Math.min(b.wi + 1, TR.wps.length - 1);
  if (waiting) { steer(b, 0, 0, dt); return; }
  if (wp.gate !== undefined) {
    if (gateOpen(wp.gate)) { b.wi++; b.claim = null; return; }
    const gt = TR.gates.find(g => g.k === wp.gate);
    if (gt) {
      hostOnly.claims = hostOnly.claims || {};
      if (!b.claim || (hostOnly.claims[b.claim.key] || {}).id !== b.id) {
        b.claim = null;
        for (const pl of gt.plates) {
          const key = wp.gate + ':' + pl.x;
          const c = hostOnly.claims[key];
          const humanOn = show.part.some(id => isHuman(id) && (() => { const p = posOf(id); return p && Math.hypot(p.x - pl.x, p.z - pl.z) < 1.05; })());
          if (!humanOn && (!c || c.t < now() - 1)) { hostOnly.claims[key] = { id: b.id, t: now() }; b.claim = { key, pl }; break; }
        }
      }
      if (b.claim) { hostOnly.claims[b.claim.key].t = now(); goTo(b, b.claim.pl.x, b.claim.pl.z, dt, b.speed); return; }
      goTo(b, b.jit * 3, wp.z + 1.5, dt, b.speed * 0.6); return;
    }
  }
  if (b.onGround && b.standingOn && b.standingOn.roller) b.vel.y = JUMP;   // rullilt hüppa kohe edasi
  if (wp.blink && b.standingOn !== wp.blink && !(b.standingOn && b.standingOn.blink) && dist < 3.2 && (!wp.blink.active || wp.blink.blink.left < 0.7)) { steer(b, 0, 0, dt); return; }
  for (const pd of TR.pendulums) {   // oota, kuni kiikuv kuul on eest läinud
    if (b.pos.z > pd.piv.z + 0.9 && b.pos.z - pd.piv.z < 2.6 && Math.abs(pd.pos.x - b.pos.x) < 2.6 && b.onGround) { steer(b, 0, 0, dt); return; }
  }
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
function bossAI(b, dt) {
  const B = show.boss, t = T();
  if (B) for (const [, x, z, t0] of B.m) if (t0 > t && t0 - t < 1.4 && Math.hypot(b.pos.x - x, b.pos.z - z) < 2.8) { const dx = b.pos.x - x, dz = b.pos.z - z, d = Math.hypot(dx, dz) || 1; goTo(b, b.pos.x + dx / d * 3, b.pos.z + dz / d * 3, dt, b.speed); return; }
  let best = null, bd = 1e9;
  if (B) TR.pads.forEach(pd => { if (B.pads[pd.i]) return; const d = Math.hypot(b.pos.x - pd.x, b.pos.z - pd.z); if (d < bd) { bd = d; best = pd; } });
  if (best && t - show.sa > 13) goTo(b, best.x, best.z, dt, b.speed * 0.85); else areenAI(b, dt);
}
function magiAI(b, dt) {
  const d = Math.hypot(b.pos.x, b.pos.z + 12);
  if (d < 2.2 && b.pos.y > 1.7) { b.aiT -= dt; if (b.aiT <= 0) { b.aiT = rnd(0.5, 1.2); const a = rnd(0, 6.28); b.aiTarget.set(Math.cos(a) * 1.2, 0, -12 + Math.sin(a) * 1.2); } goTo(b, b.aiTarget.x, b.aiTarget.z, dt, b.speed * 0.5); }
  else goTo(b, 0, -12, dt, b.speed * 0.8);
}
function pallAI(b, dt) {
  const tm = show.teams[b.id]; const bp = hostOnly.ballPos;
  const gz = tm === 0 ? -26 : 2;                       // ründatav värav
  let gx = -bp.x * 0.8, gzv = gz - bp.z; const gl = Math.hypot(gx, gzv) || 1; gx /= gl; gzv /= gl;
  const rx = b.pos.x - bp.x, rz = b.pos.z - bp.z, rd = Math.hypot(rx, rz);
  const ahead = (rx * gx + rz * gzv) / (rd || 1);
  const def = parseInt(b.id.slice(2)) % 3 === 0;
  const ownZ = tm === 0 ? -1.5 : -22.5;
  if (def && Math.abs(bp.z - ownZ) > 9) { goTo(b, bp.x * 0.5, ownZ, dt, b.speed * 0.7); return; }
  if (ahead > -0.2) {                                  // vale pool: mine ringiga palli taha
    const px = -gzv, pz = gx, sd = (rx * px + rz * pz) >= 0 ? 1 : -1;
    if (rd < 2.2) goTo(b, b.pos.x + px * sd * 2, b.pos.z + pz * sd * 2 - gzv * 1.5, dt, b.speed);
    else goTo(b, bp.x + px * sd * 2.6 - gx * 2.2, bp.z + pz * sd * 2.6 - gzv * 2.2, dt, b.speed);
    return;
  }
  const ax = bp.x - gx * 2.0, az = bp.z - gzv * 2.0;
  hostOnly.kickCd = hostOnly.kickCd || {};
  if (rd < 1.9 && ahead < -0.55 && (hostOnly.kickCd[b.id] || 0) < now() && !hostOnly.goalT) {   // löök väravasse
    hostOnly.kickCd[b.id] = now() + rnd(0.8, 1.6);
    const err = rnd(-0.35, 0.35) * (1.3 - b.skill * 0.6);
    const kx = gx * Math.cos(err) - gzv * Math.sin(err), kz = gx * Math.sin(err) + gzv * Math.cos(err);
    hostOnly.ballV.set(kx * rnd(7, 10), 2.5, kz * rnd(7, 10));
  }
  if (Math.hypot(b.pos.x - ax, b.pos.z - az) > 0.8 && rd > 1.7) goTo(b, ax, az, dt, b.speed);
  else goTo(b, bp.x + gx * 1.5, bp.z + gzv * 1.5, dt, b.speed);
}
function munadAI(b, dt) {
  const tm = show.teams[b.id], myNest = tm === 0 ? -2.5 : -21.5, enNest = tm === 0 ? -21.5 : -2.5;
  if (hostOnly.carry[b.id] !== undefined) { goTo(b, 0, myNest, dt, b.speed * 0.85); return; }
  let best = null, bd = 1e9;
  for (const e of hostOnly.eggs) if (e.s === -1) { const d = Math.hypot(e.pos.x - b.pos.x, e.pos.z - b.pos.z); if (d < bd) { bd = d; best = e; } }
  if (best) { goTo(b, best.pos.x, best.pos.z, dt, b.speed); return; }
  if (hostOnly.eggs.some(e => e.s === 1 - tm)) { goTo(b, 0, enNest, dt, b.speed); return; }
  areenAI(b, dt);
}
function lobbyAI(b, dt) {
  b.aiT -= dt;
  if (b.aiT <= 0) { b.aiT = rnd(2, 5); b.aiTarget.set(rnd(-6.5, 6.5), 0, rnd(-6.5, 6.5)); if (Math.random() < 0.15 && b.mesh) setBubble(b.mesh, EMOTES[Math.floor(Math.random() * 4)]); }
  goTo(b, b.aiTarget.x, b.aiTarget.z, dt, 2);
}
function botAI(b, dt) {
  if (show.stage === 'lobby') return lobbyAI(b, dt);
  if (show.stage === 'trenn') { if (TR.dummyPos && b.pos.distanceTo(TR.dummyPos) > 0.6 && b.onGround && !b.carried) goTo(b, TR.dummyPos.x, TR.dummyPos.z, dt, 1.5); else steer(b, 0, 0, dt); return; }
  if (show.stage === 'intro' || !b.visible) return steer(b, 0, 0, dt);
  if (show.rt === 'race') return raceAI(b, dt);
  if (show.rt === 'areen') return areenAI(b, dt);
  if (show.rt === 'plaadid') return tilesAI(b, dt);
  if (show.rt === 'saba') return sabaAI(b, dt);
  if (show.rt === 'noor') return areenAI(b, dt);
  if (show.rt === 'magi') return magiAI(b, dt);
  if (show.rt === 'boss') return bossAI(b, dt);
  if (show.rt === 'pall') return pallAI(b, dt);
  if (show.rt === 'munad') return munadAI(b, dt);
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
  net.id = mod.selfId; net.on = true; net.isHost = true; net.hostId = net.id; net.hostSince = Date.now(); net.hostHs = 0;
  const room = mod.joinRoom({ appId: 'kukerpall-vr-emil-v2' }, 'ruum-' + code);
  net.room = room;
  for (const n of ['pose', 'host', 'tile', 'fin', 'req', 'out', 'grab', 'emote', 'item']) net[n] = act(room, n);
  net.item.on(d => {
    if (d.r !== show.rid) return;
    if (d.k === 'ban') addBanana(d.id, d.x, d.y, d.z);
    else if (d.k === 'gone') { const bn = bananas.find(b => b.id === d.id); if (bn && !bn.gone) { bn.gone = true; trackGroup.remove(bn.g); } }
    else if (d.k === 'drop') addDrop(d.x, d.y, d.z, d.vx, d.vz, d.c);
  });
  onRoom(room, 'onPeerJoin', id => { ensurePeer(id); updateHud(); });
  onRoom(room, 'onPeerLeave', id => onPeerGone(id));
  net.pose.on((d, id) => { const p = ensurePeer(id); if (p.name !== d.n || p.color !== d.c || p.hat !== d.h || p.costume !== (d.u || 0) || p.lv !== (d.lv || 1) || (p.lg || 0) !== (d.lg || 0)) rebuildPeerAvatar(p, d); p.d = d; p.last = now(); });
  net.host.on((d, id) => {
    const hs = d.hs || Infinity;
    if (net.isHost) {
      // kaks juhti: juhiks jääb see, kes on kauem juht olnud (liitus varem)
      const older = hs < net.hostSince || (hs === net.hostSince && id < net.id);
      if (!older) return;
      net.isHost = false; net.hostId = id; bots.forEach(b => scene.remove(b.mesh)); bots = []; net.hasOffset = false;
    } else if (id !== net.hostId) {
      if (!(hs < (net.hostHs || Infinity))) return;
      net.hostId = id; net.hasOffset = false;
    }
    net.hostHs = hs;
    if (d.sh.members && d.sh.members.length >= MAX_HUMANS && !d.sh.members.includes(net.id)) { roomFull(); return; }
    onHostState(d);
  });
  net.tile.on(d => { if (d.r === show.rid) shakeTile(d.i, false); });
  net.fin.on((d, id) => { if (net.isHost && d.r === show.rid) recordFinish(id, d.t); });
  net.out.on((d, id) => { if (net.isHost && d.r === show.rid) hostOut(id); });
  net.req.on((d, id) => {
    if (!net.isHost) return;
    if (d.k === 'fall') hostOnly.falls[id] = (hostOnly.falls[id] || 0) + 1;
    if ((d.k === 'build' || d.k === 'del') && show.stage === 'lobby') applyBuild(d.cx | 0, d.cz | 0, d.t | 0, d.k === 'del');
  });
  net.grab.on((d, id) => { if (d.on) markGrab(d.t); applyGrab(d.t, d.g || id, !!d.on, d.th || null, d.hp || null); });
  net.emote.on((d, id) => { const p = net.peers.get(id); if (p && p.avatar) setBubble(p.avatar, EMOTES[d.e] || '👋'); });
  updateHud();
  return true;
}
function ensurePeer(id) {
  let p = net.peers.get(id);
  if (!p) {
    p = { id, name: '…', color: 0xaaaaaa, hat: 0, costume: 0, d: null, last: now(), joinT: now(), avatar: makeHuman(0xaaaaaa, '…', 0), gl: makeGlove(0x5a3e2b), gr: makeGlove(0x5a3e2b), pos: new V3(0, -50, 0) };
    scene.add(p.gl); scene.add(p.gr); p.gl.visible = p.gr.visible = false; p.avatar.visible = false;
    net.peers.set(id, p);
  }
  return p;
}
function rebuildPeerAvatar(p, d) {
  if (p.name === '…' && net.isHost) pushFeed('👋 ' + d.n + ' tuli ruumi');
  scene.remove(p.avatar); p.name = d.n; p.color = d.c; p.hat = d.h; p.costume = d.u || 0; p.lv = d.lv || 1;
  p.lg = d.lg || 0; p.avatar = makeHuman(d.c, (LEAGUES[p.lg] ? LEAGUES[p.lg][1] + ' ' : '') + d.n + ' ⭐' + p.lv, d.h, d.u || 0); p.avatar.visible = false;
  const sb = shieldBubble.clone(); sb.position.y = 0.8; sb.visible = false; p.avatar.add(sb); p.avatar.userData.shield = sb;
}
function removePeer(id) {
  const p = net.peers.get(id); if (!p) return;
  scene.remove(p.avatar); scene.remove(p.gl); scene.remove(p.gr); net.peers.delete(id);
}
const MAX_HUMANS = 8;
function membersNow() {   // juht: esimesed 8 liitujat (liitumise järjekorras)
  return [net.id, ...[...net.peers.values()].filter(p => p.d).sort((a, b) => a.joinT - b.joinT).map(p => p.id)].slice(0, MAX_HUMANS);
}
function humanIds() {
  if (!net.on) return [net.id];
  if (net.isHost) return membersNow().sort();
  const m = show.members && show.members.length ? show.members.filter(id => id === net.id || net.peers.has(id)) : [net.id, ...[...net.peers.values()].filter(p => p.d).map(p => p.id)];
  return m.slice().sort();
}
function onPeerGone(id) {
  { const pp = net.peers.get(id); if (pp && pp.name && pp.name !== '…' && (net.isHost || id === net.hostId)) { const nm = pp.name; setTimeout(() => pushFeed('🚪 ' + nm + ' lahkus'), 50); } }
  removePeer(id);
  if (net.on && id === net.hostId && !net.isHost) {
    // juht lahkus: uueks juhiks saab järgmine liitunu (kõigil sama nimekiri)
    const order = (show.members || []).filter(x => x !== id && (x === net.id || net.peers.has(x)));
    const next = order[0] || net.id;
    if (next === net.id) becomeHost(); else { net.hostId = next; net.hostHs = Infinity; net.hasOffset = false; }
  }
  updateHud();
}
function becomeHost() { net.isHost = true; net.hostId = net.id; net.hostSince = Date.now(); clearRemoteBots(); show.easy = wantEasy; enterLobby(); }
function roomFull() {
  try { net.room && net.room.leave && net.room.leave(); } catch (e) {}
  net.on = false;
  const e = document.getElementById('loadErr');
  if (e) { e.style.display = 'block'; e.innerHTML = '<h2 style="color:#ff3d8b;margin-top:0">Ruum on täis</h2><p>Selles ruumis on juba ' + MAX_HUMANS + ' mängijat.</p><p>Vali teine ruumi kood ja proovi uuesti.</p><p><button onclick="location.reload()" style="font-size:18px;padding:12px 20px;border:0;border-radius:12px;background:#22c55e;color:#fff;font-weight:800">Tagasi menüüsse</button></p>'; }
  showMsg('Ruum on täis', [MAX_HUMANS + ' mängijat on juba sees', 'Vali teine kood'], 99, true);
}


// --- PEREMEES: show juhtimine ---
const newId = () => Math.random().toString(36).slice(2, 8);
function makeBots(n) {
  bots.forEach(b => scene.remove(b.mesh)); bots = [];
  for (let i = 0; i < n; i++) {
    const b = makeEnt(false); b.mesh = makeBot(BOT_COLORS[i % BOT_COLORS.length], botHat(i)); b.id = '#b' + i;
    b.side = Math.random() < 0.5 ? -1 : 1; b.jit = Math.random() * 2 - 1;
    b.speed = show.easy ? (3.3 + Math.random() * 0.9) * 0.8 : 3.65 + Math.random() * 0.85; b.skill = show.easy ? 0.6 + Math.random() * 0.6 : 0.8 + Math.random() * 0.55;
    bots.push(b);
  }
}
function enterLobby() {
  show.mode = ''; show.tw = '';
  show.stage = 'lobby'; show.rt = 'lobby'; show.ti = 0; show.rid = newId(); show.part = []; show.alive = []; show.inR = []; show.q = []; show.fo = []; show.tails = []; show.lastOut = [];
  show.easy = wantEasy;
  show.bots = !net.on || wantBots;     // üksi mängides on robotid alati
  const n = show.bots ? Math.max(3, 10 - humanIds().length) : 0;
  if (bots.length !== n) makeBots(n);
  bots.forEach((b, i) => { b.visible = true; b.mesh.visible = true; b.pos.set(rnd(-6, 6), 0.3, rnd(-6, 6)); b.vel.set(0, 0, 0); b.finished = false; b.grabbing = null; b.grabbedBy = null; });
  hostOnly.padT = 0;
}
function startShow() {
  show.sid = newId(); show.rn = 0; show.winner = ''; hostOnly.falls = {}; hostOnly.prevType = ''; hostOnly.thr = {}; hostOnly.fast = null; show.aw = []; show.mode = '';
  if (net.on) pushFeed('🎬 Show algab! ' + humanIds().length + ' inimest' + (bots.length ? ' + ' + bots.length + ' robotit' : ''));
  show.alive = [...humanIds(), ...bots.map(b => b.id)];
  nextRound();
}
function pickRound() {
  const n = show.alive.length, rn = show.rn;
  // ainult sõpradega: keegi ei lange välja, 3 vooru + finaal
  if ((show.bots && n <= 3) || rn >= 4 || n <= 1 && show.bots) return { rt: ['plaadid', 'areen', 'noor'][Math.floor(Math.random() * 3)], final: true };
  let options;
  if (rn === 1) options = ['race'];
  else if (rn === 2) options = ['race', 'race', 'saba', 'pall', 'munad', 'magi', 'boss'];
  else options = ['areen', 'saba', 'plaadid', 'noor', 'magi', 'pall', 'munad', 'boss'];
  options = options.filter(o => o !== hostOnly.prevType || o === 'race');
  return { rt: options[Math.floor(Math.random() * options.length)], final: false };
}
function nextRound() {
  show.rn++;
  const { rt, final } = pickRound();
  hostOnly.prevType = rt;
  const NT = TRACKS.filter(t => !t.custom && !t.coop).length, CI = TRACKS.findIndex(t => t.custom), COOP = TRACKS.findIndex(t => t.coop);
  const REG = TRACKS.map((tr, i) => i).filter(i => !TRACKS[i].custom && !TRACKS[i].coop && !TRACKS[i].gen);
  const GENS = TRACKS.map((tr, i) => i).filter(i => TRACKS[i].gen && TRACKS[i].level >= 4);
  show.rt = rt; show.final = final; show.ti = rt === 'race' ? REG[Math.floor(Math.random() * REG.length)] : 0;
  if (rt === 'race' && Math.random() < 0.35 && GENS.length) show.ti = GENS[Math.floor(Math.random() * GENS.length)];
  if (rt === 'race' && (show.blocks || []).length >= 4 && show.rn <= 2 && Math.random() < 0.4) show.ti = CI;
  show.weather = rt === 'race' && TRACKS[show.ti] && TRACKS[show.ti].name !== 'Öörada' && Math.random() < 0.25 ? 'vihm' : '';
  show.gates = [0, 0, 0, 0]; show.boss = null;
  // üllatus-keerdkäik: alates 2. voorust umbes pooltel kordadel
  show.tw = '';
  if (show.rn >= 2 && Math.random() < (final ? 0.4 : 0.5)) { const pool = Object.keys(TWISTS).filter(k => !(show.easy && k === 'turbo') && !(TEAM[rt] && k === 'tuul')); show.tw = pool[Math.floor(Math.random() * pool.length)]; }
  if (rt === 'boss') { show.boss = { pads: [0, 0, 0, 0], m: [], won: 0 }; hostOnly.padHold = [0, 0, 0, 0]; hostOnly.metT = 0; hostOnly.metId = 0; }
  show.rid = newId(); show.stage = 'intro'; show.sa = T() + 6; show.ea = 0;
  show.part = show.alive.slice(); show.inR = show.alive.slice(); show.q = []; show.fo = []; show.lastOut = [];
  const n = show.part.length;
  show.quota = final ? 1 : rt === 'race' ? Math.max(1, Math.ceil(n * (show.easy ? 0.65 : 0.55))) : rt === 'saba' ? Math.max(1, Math.ceil(n * 0.5)) : Math.max(1, Math.ceil(n * 0.6));
  if (!show.bots && !final) show.quota = rt === 'race' ? n : rt === 'saba' ? Math.max(1, Math.ceil(n * 0.5)) : 1;
  show.tails = []; show.teams = {}; show.score = [0, 0]; show.winTeam = -1; show.hill = {}; show.ball = null; show.eggs = null;
  if (TEAM[rt]) {
    show.part.slice().sort(() => Math.random() - 0.5).forEach((id, i) => { show.teams[id] = i % 2; });
    if (rt === 'pall') { hostOnly.ballPos.set(0, 1.1, -12); hostOnly.ballV.set(0, 0, 0); hostOnly.goalT = 0; show.ball = [0, 1.1, -12]; }
    if (rt === 'munad') {
      hostOnly.eggs = []; hostOnly.carry = {}; hostOnly.stealCd = {};
      for (let i = 0; i < 14; i++) hostOnly.eggs.push({ pos: new THREE.Vector3(rnd(-7, 7), 0.35, rnd(-17.5, -6.5)), s: -1, c: null });
      syncEggs();
    }
  }
  if (rt === 'saba') {
    const sh = show.part.slice().sort(() => Math.random() - 0.5);
    show.tails = sh.slice(0, show.quota);
  }
  hostOnly.immune = {};
}
function pushFeed(text) {   // sündmuste teade kõigile (juht saadab show kaudu)
  if (!net.isHost) return;
  show.fid = (show.fid || 0) + 1; show.feed = [...(show.feed || []).slice(-4), [show.fid, text]];
}
let feedSeen = -1;
function feedTick() {
  const f = show.feed || []; const maxId = f.length ? f[f.length - 1][0] : 0;
  if (feedSeen < 0) { feedSeen = net.isHost ? 0 : maxId; }
  for (const [id, tx] of f) if (id > feedSeen) { feedSeen = id; addFeedLine(tx); }
  if (maxId < feedSeen) feedSeen = maxId;
}
function addFeedLine(tx) {
  if (renderer.xr.isPresenting) { showToast(tx, 2.2); return; }
  const box = $('feed'); const d = document.createElement('div'); d.textContent = tx; box.appendChild(d);
  while (box.children.length > 4) box.removeChild(box.firstChild);
  setTimeout(() => { d.style.opacity = '0'; setTimeout(() => d.remove(), 600); }, 4500);
}
function recordFinish(id, t) {
  if (show.stage !== 'play' || show.rt !== 'race') return;
  if (show.q.includes(id)) return;
  show.q.push(id); show.fo.push([id, Math.round(t * 10) / 10]);
  if (show.q.length === 1 && show.mode !== 'coop') pushFeed('🏁 ' + nameOf(id) + ' jõudis esimesena finišisse!');
  if (isHuman(id) && (!hostOnly.fast || t < hostOnly.fast[1])) hostOnly.fast = [id, Math.round(t * 10) / 10];
}
function hostOut(id) {
  if (show.stage !== 'play' || !SURVIVAL[show.rt]) return;
  const i = show.inR.indexOf(id); if (i < 0) return;
  show.inR.splice(i, 1);
  if (isHuman(id)) pushFeed('💥 ' + nameOf(id) + ' kukkus välja!');
}
const isHuman = id => !id.startsWith('#');
function endRound() {
  let next;
  const P = show.part;
  if (!show.bots && !show.final) next = P.slice();       // ainult sõbrad: kõik jätkavad finaalini
  else if (show.rt === 'magi') {
    next = P.slice().sort((a, b) => (show.hill[b] || 0) - (show.hill[a] || 0)).slice(0, show.quota);
    if (show.easy) P.forEach(id => { if (isHuman(id) && !next.includes(id)) next.push(id); });
  }
  else if (TEAM[show.rt]) {
    const [a, b] = show.score;
    if (a === b) next = P.slice();
    else { const w = a > b ? 0 : 1; show.winTeam = w; next = P.filter(id => show.teams[id] === w || (show.easy && isHuman(id))); }
  }
  else if (show.rt === 'race') {
    next = P.filter(id => show.q.includes(id) || (show.easy && isHuman(id)));
    const fin = P.filter(id => show.q.includes(id)).length;
    if (fin < show.quota && show.bots) {   // aeg sai otsa: edasi pääsevad ka kõige kaugemale jõudnud
      const rest = P.filter(id => !next.includes(id)).map(id => [id, posOf(id)]).filter(([, p]) => p && p.y > -20).sort((a, b) => a[1].z - b[1].z);
      rest.slice(0, show.quota - fin).forEach(([id]) => next.push(id));
      if (rest.length) pushFeed('⏱️ Aeg läbi! Edasi pääsesid ka kõige kaugemale jõudnud');
    }
  }
  else if (show.rt === 'saba' && show.bots) next = P.filter(id => show.tails.includes(id) || (show.easy && isHuman(id)));
  else next = show.inR.slice();
  if (!next.length && show.rt === 'boss') next = P.slice();   // kõik kukkusid: boss võitis, kõik jätkavad
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
  show.pts = show.pts || {};
  next.filter(isHuman).forEach(id => { const nm = nameOf(id); show.pts[nm] = (show.pts[nm] || 0) + 1; });
  const outH = show.lastOut.filter(isHuman).map(nameOf);
  if (outH.length) pushFeed('❌ Välja langes: ' + outH.join(', '));
  show.alive = next; show.stage = 'results'; show.ea = T();
}
function crownStage(w) {
  show.winner = w; show.stage = 'crown'; show.ea = T();
  const nm = nameOf(w); show.crowns[nm] = (show.crowns[nm] || 0) + 1;
  show.pts = show.pts || {}; show.pts[nm] = (show.pts[nm] || 0) + 5;
  pushFeed('👑 ' + nm + ' võitis krooni!');
  show.aw = [];   // show auhinnad
  const thr = Object.entries(hostOnly.thr || {}).sort((a, b) => b[1] - a[1])[0];
  if (thr && thr[1] >= 2) show.aw.push(['🚀 Suurim viskaja', nameOf(thr[0]) + ' (' + thr[1] + ' viset)']);
  if (hostOnly.fast) show.aw.push(['⚡ Kiireim jooksja', nameOf(hostOnly.fast[0]) + ' (' + hostOnly.fast[1] + ' s)']);
  const fl = Object.entries(hostOnly.falls || {}).filter(([id]) => isHuman(id)).sort((a, b) => b[1] - a[1])[0];
  if (fl && fl[1] >= 3) show.aw.push(['🤸 Kõige rohkem kukkumisi', nameOf(fl[0]) + ' (' + fl[1] + ')']);
}
function hostTick(dt) {
  const t = T();
  if (show.stage === 'trenn') return;
  if (show.mode === 'levels') { if (show.stage === 'intro' && t > show.sa) show.stage = 'play'; return; }
  if (show.mode === 'coop' && show.stage !== 'lobby') { coopTick(t); return; }
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
      gateHostTick(t);
      const hs = show.part.filter(isHuman);
      const humansDone = hs.length > 0 && hs.every(id => show.q.includes(id));
      if (humansDone && !hostOnly.doneAt) hostOnly.doneAt = t;
      if (!humansDone) hostOnly.doneAt = 0;
      if (show.q.length >= show.quota || tau > (show.easy ? 200 : 150) || (hostOnly.doneAt && t > hostOnly.doneAt + (show.easy ? 3 : 8))) { hostOnly.doneAt = 0; endRound(); }
    }
    else if (show.rt === 'saba') {
      if (tau > 45) endRound();
      else tailSteals(dt);
    } else if (show.rt === 'magi') {
      if (tau > 50) endRound();
      else for (const id of show.part) { const p = posOf(id); if (p && Math.hypot(p.x, p.z + 12) < 2.7 && p.y > 1.7) show.hill[id] = Math.round(((show.hill[id] || 0) + dt) * 100) / 100; }
    } else if (show.rt === 'pall') {
      if (tau > 75) endRound(); else ballTick(dt, t);
    } else if (show.rt === 'boss') {
      const B = show.boss;
      if (B.won) { if (t > B.won + 3.5) endRound(); }
      else if (tau > 70 || show.inR.length === 0) endRound();
      else {
        hostOnly.metT -= dt;
        if (hostOnly.metT <= 0 && tau > 2) {
          hostOnly.metT = Math.max(0.8, 1.8 - tau * 0.012) / S();
          const onPad = TR.pads.filter(pd => !B.pads[pd.i] && show.inR.some(id => { const p = posOf(id); return p && Math.hypot(p.x - pd.x, p.z - pd.z) < 1.4; }));
          const tg = onPad.length && Math.random() < 0.35 ? onPad[Math.floor(Math.random() * onPad.length)] : show.inR.length && Math.random() < 0.75 ? posOf(show.inR[Math.floor(Math.random() * show.inR.length)]) : null;
          const ang = Math.random() * 6.28, rr = Math.random() * 10;
          const mx = tg ? tg.x + rnd(-1.2, 1.2) : Math.cos(ang) * rr, mz = tg ? tg.z + rnd(-1.2, 1.2) : -12 + Math.sin(ang) * rr;
          B.m.push([hostOnly.metId++, r2(mx), r2(mz), Math.round((t + 1.3) * 100) / 100]); if (B.m.length > 10) B.m.shift();
        }
        if (tau > 15) TR.pads.forEach(pd => {
          if (B.pads[pd.i]) return;
          const on = show.inR.some(id => { const p = posOf(id); return p && Math.hypot(p.x - pd.x, p.z - pd.z) < 1.3 && p.y < 1.2; });
          hostOnly.padHold[pd.i] = on ? hostOnly.padHold[pd.i] + dt : 0;
          if (hostOnly.padHold[pd.i] > 3) B.pads[pd.i] = 1;
        });
        if (B.pads.every(x => x)) B.won = t;
      }
    } else if (show.rt === 'munad') {
      if (tau > 60) endRound(); else eggTick(dt, t);
    } else {
      const bleft = show.inR.filter(id => !isHuman(id)).length;
      const onlyHumansEasy = show.easy && show.bots && bleft === 0;
      if (show.final) { if (show.inR.length <= 1 || onlyHumansEasy || tau > (show.easy && !show.bots ? 60 : 150)) endRound(); }
      else if (show.inR.length <= show.quota || onlyHumansEasy || tau > 70) endRound();
    }
  }
  if (show.stage === 'results' && t > show.ea + 6.5) { if (show.alive.length <= 1) crownStage(show.alive[0]); else nextRound(); }
  if (show.stage === 'crown' && t > show.ea + 9) enterLobby();
}
function ballTick(dt, t) {
  const B = hostOnly.ballPos, V = hostOnly.ballV, R = 1.1;
  if (hostOnly.goalT) { if (t > hostOnly.goalT) { hostOnly.goalT = 0; B.set(0, R + 2, -12); V.set(0, 0, 0); } }
  V.y -= 18 * dt; B.addScaledVector(V, dt);
  const inGoalZ = B.z > 1.3 || B.z < -25.3;
  const floorTop = 0;
  if (B.y < floorTop + R) { B.y = floorTop + R; V.y = Math.abs(V.y) > 2 ? -V.y * 0.35 : 0; }
  const f = Math.exp(-0.55 * dt); V.x *= f; V.z *= f;
  const xLim = inGoalZ ? 3.0 - R : 8 - R;
  if (Math.abs(B.x) > xLim) { B.x = Math.sign(B.x) * xLim; V.x = -V.x * 0.7; }
  if (Math.abs(B.x) > 2.6 && !inGoalZ) {
    if (B.z > 1.3 - R) { B.z = 1.3 - R; V.z = -Math.abs(V.z) * 0.7; }
    if (B.z < -25.3 + R) { B.z = -25.3 + R; V.z = Math.abs(V.z) * 0.7; }
  }
  if (B.z > 4.2 - R) { B.z = 4.2 - R; V.z = -Math.abs(V.z) * 0.3; }
  if (B.z < -28.4 + R) { B.z = -28.4 + R; V.z = Math.abs(V.z) * 0.3; }
  if (!hostOnly.goalT && (B.z > 1.9 || B.z < -25.9) && Math.abs(B.x) < 3) {
    const scorer = B.z > 1.9 ? 1 : 0; show.score[scorer]++; show.lastGoal = [scorer, Math.round(t * 10) / 10]; hostOnly.goalT = t + 1.8;
  }
  for (const id of show.part) {
    const p = posOf(id); if (!p) continue;
    const dx = B.x - p.x, dz = B.z - p.z, d = Math.hypot(dx, dz);
    if (d < R + 0.38 && p.y < B.y + R && p.y + 1.4 > B.y - R && d > 1e-4) {
      const nx = dx / d, nz = dz / d;
      B.x = p.x + nx * (R + 0.38); B.z = p.z + nz * (R + 0.38);
      const ev = velOf(id), push = Math.max(0, ev.x * nx + ev.z * nz) * 1.5 + 1.2;   // lükkab nii kõvasti, kui kiiresti jooksed
      const vn = V.x * nx + V.z * nz;
      if (vn < push) { V.x += nx * (push - vn); V.z += nz * (push - vn); if (push > 4) V.y = Math.max(V.y, 1.2); }
    }
  }
  show.ball = [r2(B.x), r2(B.y), r2(B.z)];
}
const zeroV = new THREE.Vector3();
function velOf(id) {
  if (id === net.id) return player.vel;
  if (id.startsWith('#')) { const b = bots.find(b => b.id === id); return b ? b.vel : zeroV; }
  const p = net.peers.get(id); return p && p.vel ? p.vel : zeroV;
}
function bossVisual(dt, t) {
  const B = show.boss; if (!B || !TR.pads.length) return;
  TR.pads.forEach(pd => {
    const lit = !!B.pads[pd.i];
    pd.s.mesh.material = mat(lit ? 0x22c55e : 0xff5c5c);
    if (lit && !pd.wasLit) { pd.wasLit = true; sfx('ding'); puff({ x: pd.x, y: 0.3, z: pd.z }, 0x22c55e, 12, 2, 0.7, 1.5); if (Math.hypot(player.pos.x - pd.x, player.pos.z - pd.z) < 2) questTick('pad'); }
  });
  if (TR.bossMesh) {
    const bm = TR.bossMesh;
    if (B.won) { const k = t - B.won; bm.position.y = 2 - k * k * 3; bm.rotation.z = Math.min(1.5, k); if (!TR.bossFell && k > 0.1) { TR.bossFell = true; sfx('fanfare'); showToast('BOSS ON LIMAS! 🎉', 3); burst(bm.position); } }
    else { bm.rotation.y = Math.sin(t * 0.8) * 0.6; bm.position.y = 2 + Math.abs(Math.sin(t * 3)) * 0.15; }
  }
  for (const m of B.m) {
    const [id, x, z, t0] = m; let o = TR.meteors[id];
    if (!o) {
      if (t > t0 + 0.5) continue;
      const ring = new THREE.Mesh(new THREE.RingGeometry(1.9, 2.2, 32), new THREE.MeshBasicMaterial({ color: 0xff3b3b, transparent: true, opacity: 0.8, side: THREE.DoubleSide }));
      ring.rotation.x = -Math.PI / 2; ring.position.set(x, 0.05, z); trackGroup.add(ring);
      const ball = new THREE.Mesh(new THREE.SphereGeometry(0.7, 14, 10), mat(0xff6b00)); trackGroup.add(ball);
      o = TR.meteors[id] = { ring, ball, done: false };
    }
    if (o.done) continue;
    const left = t0 - t;
    o.ring.scale.setScalar(0.6 + 0.4 * Math.max(0, Math.min(1, 1 - left / 1.3)));
    o.ball.position.set(x, Math.max(0.7, 0.7 + left * left * 8), z);
    if (left <= 0) {
      o.done = true; trackGroup.remove(o.ring); trackGroup.remove(o.ball);
      puff({ x, y: 0.4, z }, 0xff922b, 14, 4, 0.9, 1.5); if (Math.hypot(player.pos.x - x, player.pos.z - z) < 14) sfx('hit');
      const hitE = e => { if (e.shieldT > now()) return; const dx = e.pos.x - x, dz = e.pos.z - z, d = Math.hypot(dx, dz); if (d < 2.2 && e.pos.y < 2) { const nx = d > 0.01 ? dx / d : 1, nz = d > 0.01 ? dz / d : 0; e.vel.set(nx * 4.5, 4.5, nz * 4.5); e.knock = 0.7; e.onGround = false; e.standingOn = null; hurt(e); } };
      if (!spectating) hitE(player);
      if (net.isHost) bots.forEach(b => { if (b.visible) hitE(b); });
    }
  }
}
function nestZ(tm) { return tm === 0 ? -2.5 : -21.5; }
function syncEggs() {
  const inNest = [0, 0];
  show.eggs = hostOnly.eggs.map(e => {
    if (e.s === 0 || e.s === 1) { const k = inNest[e.s]++; const a = k * 0.9, rr = 0.5 + 0.35 * Math.floor(k / 7); e.pos.set(Math.cos(a) * rr, 0.55, nestZ(e.s) + Math.sin(a) * rr); }
    return [r2(e.pos.x), r2(e.pos.y), r2(e.pos.z), e.s];
  });
  show.score = inNest;
}
function eggTick(dt, t) {
  const P = show.part.map(id => ({ id, p: posOf(id) })).filter(o => o.p);
  for (const e of hostOnly.eggs) {
    if (e.s !== 2) continue;
    const cp = posOf(e.c); const ent = e.c.startsWith('#') ? bots.find(b => b.id === e.c) : e.c === net.id ? player : null;
    const grabbed = (hostOnly.grabbedAt[e.c] || 0) > now() - 0.15;
    if (!cp || cp.y < -1 || grabbed || (ent && ent.knock > 0.3)) { e.s = -1; delete hostOnly.carry[e.c]; if (cp) e.pos.set(cp.x + rnd(-0.5, 0.5), 0.35, cp.z + rnd(-0.5, 0.5)); e.c = null; continue; }
    e.pos.set(cp.x, cp.y + 1.95, cp.z);
    const tm = show.teams[e.c];
    if (Math.hypot(cp.x, cp.z - nestZ(tm)) < 2) { e.s = tm; delete hostOnly.carry[e.c]; e.c = null; }
  }
  for (const o of P) {
    if (hostOnly.carry[o.id] !== undefined) continue;
    const tm = show.teams[o.id];
    let taken = false;
    for (let i = 0; i < hostOnly.eggs.length && !taken; i++) {
      const e = hostOnly.eggs[i];
      if (e.s === -1 && Math.hypot(e.pos.x - o.p.x, e.pos.z - o.p.z) < 0.9 && o.p.y < 1.5) { e.s = 2; e.c = o.id; hostOnly.carry[o.id] = i; taken = true; }
    }
    if (!taken && Math.hypot(o.p.x, o.p.z - nestZ(1 - tm)) < 2 && (hostOnly.stealCd[o.id] || 0) < t) {
      const i = hostOnly.eggs.findIndex(e => e.s === 1 - tm);
      if (i >= 0) { const e = hostOnly.eggs[i]; e.s = 2; e.c = o.id; hostOnly.carry[o.id] = i; hostOnly.stealCd[o.id] = t + 1.5; }
    }
  }
  syncEggs();
}
function markGrab(id) { if (net.isHost) hostOnly.grabbedAt[id] = now(); }
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
    ht: now(), hs: net.hostSince, sh: (show.members = membersNow(), show),
    b: bots.map(b => [r2(b.pos.x), r2(b.pos.y), r2(b.pos.z), r2(b.mesh.rotation.y), r2(Math.hypot(b.vel.x, b.vel.z)), b.visible ? 1 : 0, b.duckT > 0 ? 1 : 0]),
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
    const rb = remoteBots[i]; rb.target.set(a[0], a[1], a[2]); rb.yaw = a[3]; rb.sp = a[4]; rb.mesh.visible = !!a[5]; rb.mesh.userData.duck = !!a[6];
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
  if (show.stage === 'lobby' || show.stage === 'trenn') return true;
  if (!show.part.includes(net.id)) return false;
  if (SURVIVAL[show.rt] && iAmOut) return false;
  return true;
}
function placeSelf() {
  clearHands(); player.vel.set(0, 0, 0); player.cp = 0; player.finished = false; player.knock = 0; player.standingOn = null;
  iAmOut = false; myFinishTime = 0; player.grabbedBy = null;
  if (show.stage === 'lobby') { const i = Math.max(0, humanIds().indexOf(net.id)); TR.spawn(i, 10, player.pos); spectating = false; }
  else if (show.stage === 'trenn') { TR.spawn(0, 1, player.pos); spectating = false; }
  else if (show.part.includes(net.id)) { spawnFor(net.id, player.pos); spectating = false; }
  else { player.pos.set(rnd(-3, 3), 8.6, 16); spectating = true; }
  player.lastSafe.copy(player.pos);
  rig.rotation.y = 0; pitch = 0; camera.rotation.x = 0;
  if (SURVIVAL[show.rt] || show.rt === 'saba' || show.rt === 'magi' || TEAM[show.rt]) {   // vaata keskele
    const dx = TR.center.x - player.pos.x, dz = TR.center.z - player.pos.z;
    rig.rotation.y = Math.atan2(-dx, -dz);
  }
}
function spawnFor(id, out) {
  if (TR.teamSpawn && show.teams && show.teams[id] !== undefined) {
    const tm = show.teams[id], k = show.part.filter(x => show.teams[x] === tm).indexOf(id);
    TR.teamSpawn(tm, Math.max(0, k), out); return;
  }
  TR.spawn(Math.max(0, show.part.indexOf(id)), show.part.length, out);
}
function placeBots() {
  bots.forEach((b, i) => {
    b.vel.set(0, 0, 0); b.finished = false; b.cp = 0; b.wi = 0; b.knock = 0; b.grabbing = null; b.grabbedBy = null; b.aiT = 0; b.standingOn = null;
    if (show.stage === 'lobby') { b.visible = true; b.pos.set(rnd(-6, 6), 0.3, rnd(-6, 6)); }
    else if (show.stage === 'trenn') { b.visible = true; b.pos.copy(TR.dummyPos || new V3(0, 0.3, -20)); }
    else if (show.part.includes(b.id)) { b.visible = true; spawnFor(b.id, b.pos); }
    else { b.visible = false; b.pos.set(0, -100, 0); }
    b.mesh.visible = b.visible; b.lastSafe.copy(b.pos);
  });
}
function respawn(e) {
  e.vel.set(0, 0, 0); e.knock = 0; e.standingOn = null;
  if (e.isPlayer) clearHands();
  if (e.isPlayer && spectating) { e.pos.set(rnd(-3, 3), 8.6, 16); return; }
  if (show.rt === 'lobby') { e.pos.set(rnd(-3, 3), 0.5, rnd(0, 3)); return; }
  if (show.rt === 'trenn') { if (e.isPlayer) e.pos.copy(e.lastSafe).add(tmpC.set(0, 0.5, 0)); else e.pos.copy(TR.dummyPos); return; }
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
  if (TEAM[show.rt]) { spawnFor(e.isPlayer ? net.id : e.id, e.pos); e.pos.y += 0.5; return; }
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
    hd: [r2(headW.x), r2(headW.y), r2(headW.z)], v: renderer.xr.isPresenting ? 1 : 0, s: spectating ? 1 : 0, k: player.ducking ? 1 : 0, u: myCostume, pt: myPet, sh: player.shieldT > now() ? 1 : 0, lv: myLevel(), lg: myLeague() };
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
    if (!p.vel) { p.vel = new V3(); p.prevP = new V3().copy(tmpA); p.velT = now(); }
    if (now() - p.velT > 0.04) { const dtv = now() - p.velT; p.vel.set((tmpA.x - p.prevP.x) / dtv, 0, (tmpA.z - p.prevP.z) / dtv); if (p.vel.length() > 12) p.vel.set(0, 0, 0); p.prevP.copy(tmpA); p.velT = now(); }
    if (p.pos.y < -40 || p.pos.distanceTo(tmpA) > 5) p.pos.copy(tmpA); else p.pos.lerp(tmpA, k);
    p.avatar.position.copy(p.pos);
    let diff = d.y - p.avatar.rotation.y; diff = ((diff + Math.PI) % (Math.PI * 2) + Math.PI * 2) % (Math.PI * 2) - Math.PI;
    p.avatar.rotation.y += diff * k;
    p.gl.visible = p.gr.visible = !!(d.v && d.l);
    if (d.v && d.l) { p.gl.position.lerp(tmpB.set(d.l[0], d.l[1], d.l[2]), k); p.gr.position.lerp(tmpB.set(d.rr[0], d.rr[1], d.rr[2]), k); }
    p.avatar.userData.duck = !!d.k;
    if (p.avatar.userData.shield) p.avatar.userData.shield.visible = !!d.sh;
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
  if (e.code === 'KeyR' && show.mode === 'levels') { startLevel(show.level); return; }
  if (e.code === 'Enter' && show.mode === 'levels' && show.stage === 'lvdone') { startLevel(show.level + 1); return; }
  if (e.code === 'KeyR' && net.isHost) { if (show.stage === 'trenn') { makeBots(0); } enterLobby(); }
  if (e.code === 'Enter' && show.stage === 'lobby' && net.isHost) startShow();
  if (e.code === 'KeyM') setMusic(!musicOn);
  if (e.code === 'KeyP' && show.stage === 'lobby') openShop();
  if (e.code === 'KeyK' && started && net.isHost && show.stage === 'lobby') startCoop(1);
  if (e.code === 'KeyF') { if (spectating) spectatorDrop(); else doBuild(false); }
  if (e.code === 'KeyG') doBuild(true);
  if (e.code === 'KeyT' && buildAimCache) cycleBuildType();
  const em = ['Digit1', 'Digit2', 'Digit3', 'Digit4'].indexOf(e.code); if (em >= 0) doEmote(em);
});
addEventListener('keyup', e => { keys[e.code] = false; });
// VR-prillide brauser (Quest, Pico jt) – seal telefoninuppe ei näidata kunagi
const isHeadset = HEADSET;
// puuteekraaniga arvuti (hiir olemas) EI ole telefon
const finePointer = !!(window.matchMedia && matchMedia('(any-pointer: fine)').matches);
const hasTouch = ('ontouchstart' in window) || (navigator.maxTouchPoints || 0) > 0;
const coarse = !!(window.matchMedia && matchMedia('(pointer: coarse)').matches);
const smallScreen = typeof screen !== 'undefined' && Math.min(screen.width || 9999, screen.height || 9999) < 820;
// telefon = puuteekraan ja (jäme osuti või väike ekraan). Kindluse mõttes: iga puudutus lülitab nupud sisse.
let isTouch = !isHeadset && hasTouch && (coarse || smallScreen || !finePointer);
let touchManual = null;   // 🎮 nupuga: true/false
try { const tm = localStorage.getItem('kp_touch'); if (tm !== null) touchManual = tm === '1'; } catch (e) {}
if (touchManual !== null && !isHeadset) isTouch = touchManual;
addEventListener('pointerdown', e => { if (e.pointerType === 'touch' && !isHeadset && touchManual !== false && !isTouch) { isTouch = true; updateHud(); } }, true);
addEventListener('touchstart', () => { if (!isHeadset && touchManual !== false && !isTouch) { isTouch = true; updateHud(); } }, { capture: true, passive: true });
renderer.domElement.addEventListener('click', () => { audioInit(); if (started && !renderer.xr.isPresenting && !isTouch) renderer.domElement.requestPointerLock(); });
// ---------- telefoni juhtimine ----------
const touchIn = { mx: 0, my: 0, jump: false, grab: false, duck: false };
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
  let mouseDrag = false;   // hiirega lohistamine keerab kaamerat ka ilma hiire lukustuseta
  cvs.addEventListener('pointerdown', e => { if (e.pointerType === 'mouse') { mouseDrag = true; return; } if (lookId !== null) return; lookId = e.pointerId; lx = e.clientX; ly = e.clientY; });
  addEventListener('pointerup', e => { if (e.pointerType === 'mouse') mouseDrag = false; });
  cvs.addEventListener('pointermove', e => {
    if (e.pointerType === 'mouse') {
      if (mouseDrag && started && document.pointerLockElement !== cvs && !renderer.xr.isPresenting) { rig.rotation.y -= (e.movementX || 0) * 0.005; pitch = Math.max(-1.3, Math.min(1.3, pitch - (e.movementY || 0) * 0.004)); camera.rotation.x = pitch; }
      return;
    }
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
  hold('tJump', 'jump'); hold('tGrab', 'grab'); hold('tDuck', 'duck');
  let emoteI = 0;
  document.getElementById('tEmote').addEventListener('pointerdown', e => { e.preventDefault(); if (started) doEmote(emoteI++ % EMOTES.length); });
})();
document.getElementById('bBuild').onclick = () => doBuild(false);
document.getElementById('dropBtn').onclick = () => spectatorDrop();
document.getElementById('bDel').onclick = () => doBuild(true);
document.getElementById('bType').onclick = () => cycleBuildType();
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
  let mx = 0, my = 0, rx = 0, ry = 0, jump = false, bDown = false, leftTrig = false;
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
      if (left && b[0] && b[0].pressed && !vrMenu.open) leftTrig = true;
      if (edge((left ? 'L' : 'R') + '3', !!(b[3] && b[3].pressed))) doEmote(left ? 1 : 2);
      if (left && edge('Y', !!(b[5] && b[5].pressed))) { if (vrMenu.open) closeVrMenu(); else if (started) openVrMenu(true); }
      if (!left && b[5] && b[5].pressed) bDown = true;
      if (!left && edge('Bshort', !!(b[5] && b[5].pressed)) && buildAimCache) cycleBuildType();
      idx++;
    }
  }
  // grip-ide järjekord: grips[0]/[1] vastavad inputSources järjekorrale; kasutame käelisust
  const gripByIndex = [false, false];
  if (session) { let i = 0; for (const src of session.inputSources) { if (i < 2 && src.gamepad) gripByIndex[i] = grip[src.handedness === 'left' ? 0 : 1]; i++; } }
  if (bDown) { bHold += dt; if (bHold > 1.5 && show.mode === 'levels') { startLevel(show.level); bHold = -99; } else if (bHold > 1.5 && net.isHost && show.stage !== 'lobby') { if (show.stage === 'trenn') makeBots(0); enterLobby(); bHold = -99; } } else bHold = 0;

  if (keys.KeyW || keys.ArrowUp) my += 1; if (keys.KeyS || keys.ArrowDown) my -= 1;
  if (keys.KeyD) mx += 1; if (keys.KeyA) mx -= 1;
  if (keys.ArrowLeft) rig.rotation.y += 2.4 * dt; if (keys.ArrowRight) rig.rotation.y -= 2.4 * dt;   // nooled keeravad
  player.ducking = canMove && !!(keys.KeyC || keys.ControlLeft || keys.ControlRight || touchIn.duck || leftTrig || (session && camera.position.y < headStand - 0.38));
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
    if (canMove) {
      armLocomotion(dt);
      // batuudil ja batuudihüppe/viske ajal saab liikuda ka pulgaga
      const onBounce = (player.standingOn && player.standingOn.bounce) || (player.bounceT || 0) > 0;
      if (onBounce && !player.holding && (mx || my)) steer(player, mvIn.x * player.speed, mvIn.z * player.speed, dt);
    } else { clearHands(); steer(player, 0, 0, dt); }
  } else {
    player.holding = false;
    steer(player, canMove ? mvIn.x * player.speed : 0, canMove ? mvIn.z * player.speed : 0, dt);
    if (jump && !jumpWas && player.onGround && canMove) { player.vel.y = JUMP * twJ() * (player.pw === 'hupe' && player.pwT > now() ? 1.45 : 1); sfx('jump'); puff(player.pos, 0xffffff, 4, 0.8, 0.4, 0.3); }
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
  for (const p of TR.pendulums) {
    const a = p.amp * Math.sin(t * p.speed * s + p.phase);
    p.vx = p.len * Math.cos(a) * p.amp * p.speed * s * Math.cos(t * p.speed * s + p.phase);
    p.g.rotation.z = a;
    p.pos.set(p.piv.x + p.len * Math.sin(a), p.piv.y - p.len * Math.cos(a), p.piv.z);
  }
  for (const sp of TR.spinners) {
    const na = sp.spin * s * t;
    sp.spinD = force ? 0 : na - sp.ang; if (Math.abs(sp.spinD) > 0.5) sp.spinD = 0;
    sp.ang = na; sp.mesh.rotation.y = na;
  }
  for (const c of TR.convs) if (c.tex) c.tex.offset.y += (c.s.conveyor * s * dt) / 2;
  for (const bk of TR.blinks) {
    const B = bk.blink, tt = (((t * s + B.phase) % B.period) + B.period) % B.period, onT = B.period * 0.7;
    bk.active = tt < onT; B.left = bk.active ? onT - tt : 0;
    bk.mesh.visible = bk.active && !(B.left < 0.7 && Math.floor(t * 12) % 2);
  }
  for (const r of TR.rollers) r.rl.rotation.x -= r.s.conveyor * s * dt / (r.s.hh || 0.5);
  for (const f of TR.fans) f.rotation.z += dt * 9;
  for (const w of TR.winds) {
    w.acc = (w.acc || 0) + dt;
    if (w.acc > 0.12 && Math.abs(player.pos.z - (w.z0 + w.z1) / 2) < 20) {
      w.acc = 0; const dir = Math.sign(Math.sin(t * w.speed)) || 1;
      const p = puffs[puffI++ % puffs.length]; p.sp.position.set(-dir * 4, rnd(0.4, 2.2), rnd(w.z1, w.z0));
      p.vel.set(dir * 7, 0, 0); p.life = p.max = 0.9; p.grow = 0.35; p.sp.material.color.setHex(0xe7f5ff); p.sp.scale.setScalar(0.2); p.sp.visible = true;
    }
  }
  if (TR.ballMesh) {
    const bp = net.isHost ? hostOnly.ballPos : (show.ball ? tmpC.set(show.ball[0], show.ball[1], show.ball[2]) : null);
    if (bp) { const o = TR.ballMesh.position; const ox = o.x, oz = o.z; o.lerp(bp, net.isHost ? 1 : Math.min(1, dt * 12)); TR.ballMesh.rotation.x -= (o.z - oz) / 1.1; TR.ballMesh.rotation.z += (o.x - ox) / 1.1; }
  }
  for (const c of TR.coins) if (!c.taken) { c.m.rotation.z += dt * 3; }
  if (TR.eggMeshes && show.eggs) TR.eggMeshes.forEach((m, i) => { const e = show.eggs[i]; if (e) m.position.lerp(tmpC.set(e[0], e[1], e[2]), Math.min(1, dt * 15)); });
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
      if (so.t > (show.easy ? 1.2 : 0.65) + (TR.slowTiles ? 0.4 : 0)) { so.state = 'fall'; so.active = false; so.t = 0; so.mesh.position.x = so.home.x; }
    } else if (so.state === 'fall') {
      so.t += dt; so.mesh.position.y = so.home.y - so.t * so.t * 6;
      if (so.t > 3.5) { if (so.noReturn) { so.state = 'gone'; so.mesh.visible = false; } else { so.state = 'idle'; so.active = true; so.mesh.position.copy(so.home); } }
    }
  }
  trackGroup.children.forEach(o => {
    if (o.userData.spin) { o.rotation.y += dt; o.position.y = o.userData.baseY + Math.sin(t * 2) * 0.15; }
    else if (o.userData.bob !== undefined) { o.position.y = o.userData.baseY + Math.sin(t * 1.3 + o.userData.bob) * 0.3; o.rotation.z = Math.sin(t * 0.9 + o.userData.bob) * 0.08; }
  });
  gooMat.uniforms.t.value = t;
  bounceMat.emissiveIntensity = 0.25 + 0.2 * Math.sin(t * 5);
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
  $('tDuck').style.display = TR && TR.hasHigh ? 'block' : 'none';
  $('startBtn').style.display = !vrNow && net.isHost && show.stage === 'lobby' ? 'block' : 'none';
  $('coopStart').style.display = !vrNow && net.isHost && show.stage === 'lobby' ? 'block' : 'none';
  $('shopBtn').style.display = !vrNow && show.stage === 'lobby' ? 'block' : 'none';
  $('lvMenuBtn').style.display = !vrNow && show.mode === 'levels' ? 'block' : 'none';
  $('fsBtn').style.display = isTouch && !vrNow && !document.fullscreenElement ? 'block' : 'none';
  $('exitBtn').style.display = !vrNow ? 'block' : 'none';
  $('padBtn').style.display = !vrNow && !isHeadset && (hasTouch || touchManual !== null) ? 'block' : 'none';
  $('buildBar').classList.toggle('on', isTouch && !vrNow && !!buildAimCache);
  $('dropBtn').style.display = isTouch && !vrNow && spectating && show.stage === 'play' ? 'block' : 'none';
  $('shieldGlow').style.display = !vrNow && player.shieldT > now() ? 'block' : 'none';
  if (isTouch) { hud.innerHTML = '<b>' + (TR ? TR.name : '') + '</b>' + (net.on ? ' · ruum ' + roomCode + ' · ' + humanIds().length + ' inimest' : ''); return; }
  const n = humanIds().length;
  hud.innerHTML = '<b>Kukerpall VR</b> · ' + (TR ? TR.name : '') + '<br>' +
    (net.on ? 'Ruum <b>' + roomCode + '</b> · inimesi ' + n + '/' + MAX_HUMANS + (net.isHost ? ' · sina juhid' : '') : 'Üksi robotitega') +
    (show.easy ? ' · lihtne' : '') +
    '<br><small>Klõpsa ekraanil · WASD + hiir · tühik · E = haara · 1–4 emotikon · M = muusika' + (show.stage === 'lobby' ? ' · Enter = alusta' : '') + '</small>';
}
// värvid ja mütsid
{ const gr = document.querySelector('input[name=gfx][value=' + gfx + ']'); if (gr) gr.checked = true; }
function myColors() { return [...HUMAN_COLORS, ...SHOP.filter(it => it.type === 'color' && owned.includes(it.id)).map(it => it.val)]; }
function myHats() { return HATS.map((h, i) => i).filter(ownsHat); }
function renderPickers() {
  $('colors').innerHTML = ''; $('hats').innerHTML = '';
  myColors().forEach(c => {
    const b = document.createElement('button'); b.className = 'sw' + (c === myColor ? ' on' : ''); b.style.background = '#' + new THREE.Color(c).getHexString();
    b.onclick = () => { myColor = c; saveProg(); renderPickers(); };
    $('colors').appendChild(b);
  });
  myHats().forEach(i => {
    const b = document.createElement('button'); b.className = 'hat' + (i === myHat ? ' on' : ''); b.textContent = HATS[i];
    b.onclick = () => { myHat = i; saveProg(); renderPickers(); };
    $('hats').appendChild(b);
  });
  if (myCostume) { const b = document.createElement('button'); b.className = 'hat on'; b.textContent = '🎭 ' + COSTUMES[myCostume]; b.onclick = () => { myCostume = 0; saveProg(); renderPickers(); }; $('hats').appendChild(b); }
  const best = Object.entries(prog.best || {}).sort((a, b) => a[1] - b[1]).slice(0, 3).map(([k, v]) => k + ' ' + v.toFixed(1) + ' s').join(', ');
  ensureQuests();
  $('statLine').innerHTML = '<b>' + leagueText() + '</b> · 🔥 ' + (prog.streak || 1) + ' päeva järjest<br>⭐ Tase <b>' + myLevel() + '</b> · 🪙 <b>' + coins + '</b> münti · 👑 ' + prog.crowns + ' krooni · mänge ' + prog.shows + ' · edasipääse ' + prog.qual + '/' + prog.rounds + (best ? '<br>Parimad ajad: ' + best : '') + '<br><b>Tänased ülesanded:</b> ' + prog.quests.map(questText).join(' · ');
}
function renderShop() {
  $('coinsLbl').textContent = 'Sul on ' + coins + ' 🪙 · tase ' + myLevel();
  $('shopItems').innerHTML = '';
  SHOP.forEach(it => {
    const st = shopState(it), row = document.createElement('div'); row.className = 'it';
    const lab = document.createElement('span'); lab.textContent = (it.type === 'costume' ? '🎭 ' : it.type === 'hat' ? '🎩 ' : it.type === 'pet' ? '🐾 ' : '🎨 ') + it.name; row.appendChild(lab);
    const b = document.createElement('button'), locked = !st.has && it.lv && myLevel() < it.lv;
    b.textContent = st.using ? 'Kasutusel ✓' : st.has ? 'Kasuta' : locked ? '🔒 Tase ' + it.lv : it.price + ' 🪙';
    b.style.background = st.using ? '#1d1b3a' : st.has ? '#22c55e' : !locked && coins >= it.price ? '#f59e0b' : '#adb5bd';
    b.onclick = () => shopClick(it);
    row.appendChild(b); $('shopItems').appendChild(row);
  });
}
function openShop() { renderShop(); $('shop').classList.add('on'); if (document.pointerLockElement) document.exitPointerLock(); }
$('shopClose').onclick = () => { $('shop').classList.remove('on'); renderPickers(); };
$('padBtn').onclick = () => { isTouch = !isTouch; touchManual = isTouch; try { localStorage.setItem('kp_touch', isTouch ? '1' : '0'); } catch (e) {} updateHud(); };
$('shopBtn').onclick = openShop; $('fsBtn').onclick = goFullscreen; $('menuShop').onclick = openShop;
renderPickers();
const params = new URLSearchParams(location.search);
let savedName = ''; try { savedName = localStorage.getItem('kp_name') || ''; } catch (e) {}
$('name').value = savedName;
$('code').value = (params.get('ruum') || randomCode()).toUpperCase();
function randomCode() { const L = 'ABDEFGHKLMNPRSTUV'; let s = ''; for (let i = 0; i < 4; i++) s += L[Math.floor(Math.random() * L.length)]; return s; }
function readMenu() {
  myName = ($('name').value.trim() || 'Mängija').slice(0, 12);
  try { localStorage.setItem('kp_name', myName); } catch (e) {}
  loco = document.querySelector('input[name=loco]:checked').value;
  wantEasy = $('easy').checked; wantBots = $('bots').checked; setMusic($('music').checked);
  const gsel = document.querySelector('input[name=gfx]:checked'); if (gsel && gsel.value !== gfx) setGfx(gsel.value);
  audioInit();
}
$('solo').onclick = () => { readMenu(); startGame(false); };
$('practiceBtn').onclick = () => startPractice();
$('coopBtn').onclick = () => { readMenu(); startGame(false); startCoop(1); };
$('dailyBtn').onclick = () => { readMenu(); startLevel(0); };
$('lvDaily').onclick = () => startLevel(0);
$('coopStart').onclick = () => { audioInit(); if (started && net.isHost && show.stage === 'lobby') startCoop(1); };
$('levelsBtn').onclick = () => { readMenu(); openLevelSelect(); };
$('lvBack').onclick = () => { $('levelSel').classList.remove('on'); if (started) location.reload(); };
$('lvNext').onclick = () => startLevel(show.level + 1);
$('lvRetry').onclick = () => startLevel(show.level);
$('lvList').onclick = () => { $('lvDone').classList.remove('on'); openLevelSelect(); };
$('lvMenuBtn').onclick = () => openLevelSelect();
$('online').onclick = async () => {
  readMenu();
  roomCode = $('code').value.trim().toUpperCase().replace(/[^A-Z0-9]/g, '') || randomCode();
  $('code').value = roomCode;
  const ok = await startOnline(roomCode);
  if (ok) startGame(true);
};
function goFullscreen() {   // telefonis: täisekraan + horisontaalne, et brauseri ribad kaoksid
  if (!isTouch) return;
  const el = document.documentElement;
  try {
    const r = el.requestFullscreen ? el.requestFullscreen({ navigationUI: 'hide' }) : el.webkitRequestFullscreen ? el.webkitRequestFullscreen() : null;
    if (r && r.then) r.then(() => { try { screen.orientation && screen.orientation.lock && screen.orientation.lock('landscape').catch(() => {}); } catch (e) {} setTimeout(fitScreen, 300); }).catch(() => {});
  } catch (e) {}
  setTimeout(fitScreen, 300); setTimeout(fitScreen, 900);
}
document.addEventListener('fullscreenchange', () => { fitW = 0; setTimeout(fitScreen, 100); setTimeout(fitScreen, 500); });
let exitArm = 0;
function exitToMenu() {   // lahku ruumist ja mine peamenüüsse (saab valida uue ruumi koodi)
  try { net.room && net.room.leave && net.room.leave(); } catch (e) {}
  try { saveProg(); } catch (e) {}
  setTimeout(() => location.reload(), 150);
}
$('exitBtn').onclick = () => {
  audioInit();
  if (now() < exitArm) { exitToMenu(); return; }
  exitArm = now() + 3; $('exitBtn').classList.add('warn'); $('exitBtn').textContent = 'Kindel? Vajuta uuesti';
  setTimeout(() => { $('exitBtn').classList.remove('warn'); $('exitBtn').textContent = '⏏ Menüü'; }, 3000);
};
function startGame(online) {
  goFullscreen();
  started = true; $('menu').style.display = 'none';
  pitch = 0; camera.rotation.set(0, 0, 0);
  if (!online) { net.on = false; net.isHost = true; }
  if (net.isHost) { show.easy = wantEasy; enterLobby(); loadForShow(); }
  updateHud(); showDailyMsg();
}
renderer.xr.addEventListener('sessionstart', () => {
  audioInit(); clearHands(); headStand = 1.5;
  if (!started) { readMenu(); openVrMenu(false); }
  updateHud();
});

// =====================================================================
// VR MENÜÜ (laserkiirega, kontrolleri päästik = vali)
// =====================================================================
const vrMenu = { open: false, screen: 'main', edit: '', text: '', hover: null, items: [], status: '', inGame: false, place: true };
const MW = 1024, MH = 820;
const menuCv = document.createElement('canvas'); menuCv.width = MW; menuCv.height = MH;
const menuCtx = menuCv.getContext('2d');
const menuTex = new THREE.CanvasTexture(menuCv); menuTex.colorSpace = THREE.SRGBColorSpace;
const menuMesh = new THREE.Mesh(new THREE.PlaneGeometry(1.25, 1.0), new THREE.MeshBasicMaterial({ map: menuTex, transparent: true, depthTest: false, fog: false }));
menuMesh.renderOrder = 1000; menuMesh.visible = false; scene.add(menuMesh);
const raycaster = new THREE.Raycaster();
const rays = [];
for (let i = 0; i < 2; i++) {
  const c = renderer.xr.getController(i); rig.add(c);
  const line = new THREE.Line(new THREE.BufferGeometry().setFromPoints([new V3(0, 0, 0), new V3(0, 0, -1)]), new THREE.LineBasicMaterial({ color: 0xff3d8b }));
  line.scale.z = 3; line.visible = false; c.add(line);
  const dot = new THREE.Mesh(new THREE.SphereGeometry(0.012, 8, 6), new THREE.MeshBasicMaterial({ color: 0xff3d8b, depthTest: false })); dot.renderOrder = 1001; dot.visible = false; scene.add(dot);
  rays.push({ c, line, dot });
  c.addEventListener('selectstart', () => {
    if (vrMenu.open) return menuSelect(i);
    if (spectating && show.stage === 'play') { spectatorDrop(i); return; }
    if (buildAimCache) { const src = renderer.xr.getSession() && renderer.xr.getSession().inputSources[i]; doBuild(!!(src && src.handedness === 'left')); }
  });
}
function openVrMenu(inGame) {
  vrMenu.open = true; vrMenu.inGame = inGame; vrMenu.screen = 'main'; vrMenu.status = ''; vrMenu.place = true;
  if (!roomCode) roomCode = ($('code').value || randomCode()).toUpperCase();
  drawMenu();
}
function closeVrMenu() { vrMenu.open = false; menuMesh.visible = false; rays.forEach(r => { r.line.visible = false; r.dot.visible = false; }); clearHands(); }
function placeMenu() {
  camera.getWorldPosition(tmpA); camera.getWorldDirection(fw); fw.y = 0; if (fw.length() < 0.01) fw.set(0, 0, -1); fw.normalize();
  menuMesh.position.copy(tmpA).addScaledVector(fw, 1.3); menuMesh.position.y = tmpA.y - 0.1;
  menuMesh.lookAt(tmpA.x, menuMesh.position.y, tmpA.z);
  menuMesh.visible = true; menuMesh.updateMatrixWorld(true);
}
function mBtn(x, y, w, h, label, action, opts = {}) { vrMenu.items.push({ x, y, w, h, label, action, ...opts }); }
function syncHtml() {
  $('name').value = myName; $('code').value = roomCode; $('easy').checked = wantEasy; $('bots').checked = wantBots; $('music').checked = musicOn;
  const lr = document.querySelector('input[name=loco][value=' + loco + ']'); if (lr) lr.checked = true;
  const gr = document.querySelector('input[name=gfx][value=' + gfx + ']'); if (gr) gr.checked = true;
  try { localStorage.setItem('kp_name', myName); } catch (e) {}
}
function drawMenu() {
  const x = menuCtx, M = vrMenu; M.items = [];
  x.clearRect(0, 0, MW, MH);
  x.fillStyle = 'rgba(255,255,255,0.96)'; x.beginPath(); x.roundRect(0, 0, MW, MH, 50); x.fill();
  x.textBaseline = 'middle';
  if (M.screen === 'main') {
    if (M.inGame) { x.fillStyle = '#ff3d8b'; x.font = 'bold 64px system-ui'; x.textAlign = 'left'; x.fillText('Kukerpall VR', 40, 60); }
    x.fillStyle = '#1d1b3a'; x.font = 'bold 34px system-ui';
    const row = (t, y) => { x.fillStyle = '#1d1b3a'; x.textAlign = 'left'; x.font = 'bold 34px system-ui'; x.fillText(t, 40, y + 30); };
    row('Nimi', 120); mBtn(230, 120, 420, 62, myName + '  ✎', () => openKeyboard('name'));
    row('Värv', 205);
    HUMAN_COLORS.forEach((c, i) => mBtn(230 + i * 88, 205, 70, 62, '', () => { myColor = c; }, { color: c, on: c === myColor }));
    row('Müts', 290);
    const hs = myHats().slice(0, 5);
    hs.forEach((i, k) => mBtn(230 + (k % 3) * 255, 290 + Math.floor(k / 3) * 70, 240, 60, HATS[i], () => { myHat = i; saveProg(); }, { on: i === myHat }));
    mBtn(230 + 2 * 255, 360, 240, 60, '🛒 Pood', () => { M.screen = 'shop'; }, { big: '#f59e0b' });
    row('Liikumine', 450);
    mBtn(230, 450, 250, 60, 'Kätega (ahv)', () => { loco = 'arms'; clearHands(); }, { on: loco === 'arms' });
    mBtn(495, 450, 250, 60, 'Pulgaga', () => { loco = 'stick'; clearHands(); }, { on: loco === 'stick' });
    x.fillStyle = '#b8860b'; x.font = 'bold 30px system-ui'; x.textAlign = 'right'; x.fillText(LEAGUES[myLeague()][1] + ' ⭐' + myLevel() + '  🪙 ' + coins, 984, 60);
    mBtn(40, 530, 300, 60, 'Lihtne režiim', () => { wantEasy = !wantEasy; }, { on: wantEasy });
    mBtn(355, 530, 300, 60, 'Muusika', () => setMusic(!musicOn), { on: musicOn });
    mBtn(670, 530, 314, 60, 'Graafika: ' + (gfx === 'ilus' ? 'Ilus' : 'Kiire'), () => setGfx(gfx === 'ilus' ? 'kiire' : 'ilus'));
    if (!M.inGame) {
      row('Kood', 610); mBtn(230, 610, 250, 62, roomCode + '  ✎', () => openKeyboard('code'));
      mBtn(495, 610, 489, 62, 'Robotid ka (sõpradega)', () => { wantBots = !wantBots; }, { on: wantBots });
      mBtn(40, 695, 460, 86, 'Mängi robotitega', () => { syncHtml(); closeVrMenu(); startGame(false); }, { big: '#2f9bff' });
      mBtn(524, 695, 460, 86, 'Mängi sõpradega', () => vrOnline(), { big: '#ff3d8b' });
      mBtn(40, 20, 230, 64, 'Harjuta 🎓', () => { syncHtml(); closeVrMenu(); startPractice(); }, { big: '#22c55e' });
      mBtn(282, 20, 230, 64, 'Koostöö 🤝', () => { syncHtml(); closeVrMenu(); startGame(false); startCoop(1); }, { big: '#0ea5e9' });
      mBtn(524, 20, 230, 64, 'Tasemed ⭐', () => { syncHtml(); M.screen = 'levels'; }, { big: '#8b5cf6' });
      mBtn(670, 120, 314, 62, '📅 Päeva rada', () => { syncHtml(); closeVrMenu(); startLevel(0); }, { big: '#e8590c' });
    } else {
      x.fillStyle = '#556'; x.font = '28px system-ui'; x.textAlign = 'left'; x.fillText(net.on ? 'Ruum ' + roomCode + ' · ' + humanIds().length + ' inimest' : 'Üksi robotitega', 40, 640);
      mBtn(40, 695, 460, 86, 'Sulge menüü', () => closeVrMenu(), { big: '#22c55e' });
      mBtn(524, 695, 460, 86, '⏏ Peamenüüsse', () => { if (now() < exitArm) exitToMenu(); else { exitArm = now() + 3; M.status = 'Vajuta uuesti, et lahkuda ruumist'; } }, { big: '#e03131' });
      if (net.isHost && show.stage === 'lobby') mBtn(524, 610, 460, 70, '🤝 Alusta koostööd', () => { closeVrMenu(); startCoop(1); }, { big: '#0ea5e9' });
      if (show.mode === 'levels') { mBtn(40, 610, 460, 70, 'Tasemete valik', () => { M.screen = 'levels'; }, { big: '#8b5cf6' }); mBtn(524, 610, 460, 70, 'Alusta uuesti ↻', () => startLevel(show.level), { big: '#f59e0b' }); }
    }
    if (M.status) { x.fillStyle = '#d6336c'; x.font = 'bold 28px system-ui'; x.textAlign = 'center'; x.fillText(M.status, MW / 2, 800); }
  } else if (M.screen === 'levels') {
    x.fillStyle = '#8b5cf6'; x.font = 'bold 54px system-ui'; x.textAlign = 'left'; x.fillText('Tasemed', 40, 58);
    for (let n = 1; n <= LEVEL_COUNT; n++) {
      const i = n - 1, st = levelStars(n), un = levelUnlocked(n);
      mBtn(40 + (i % 6) * 158, 100 + Math.floor(i / 6) * 112, 146, 100, n + (un ? ' ' + '★'.repeat(st) : ' 🔒'), () => { if (un) startLevel(n); }, { on: st === 3 });
    }
    mBtn(40, 668, 460, 80, '📅 Päeva väljakutse', () => startLevel(0), { big: '#e8590c' });
    mBtn(524, 668, 460, 80, 'Tagasi', () => { M.screen = 'main'; }, { big: '#495057' });
  } else if (M.screen === 'lvdone') {
    const n = show.level, L = n ? (prog.levels[n] || { s: 0, t: 0 }) : { s: 3, t: prog.daily ? prog.daily.t : 0 };
    x.fillStyle = '#8b5cf6'; x.font = 'bold 64px system-ui'; x.textAlign = 'center'; x.fillText(n ? 'Tase ' + n + ' läbitud!' : 'Päeva väljakutse läbitud!', MW / 2, 110);
    x.fillStyle = '#f59e0b'; x.font = 'bold 120px system-ui'; x.fillText('★'.repeat(L.s) + '☆'.repeat(3 - L.s), MW / 2, 260);
    x.fillStyle = '#1d1b3a'; x.font = 'bold 38px system-ui'; x.fillText('Aeg ' + myFinishTime.toFixed(1) + ' s · rekord ' + L.t + ' s', MW / 2, 370);
    if (n && n < LEVEL_COUNT) mBtn(40, 460, 300, 110, 'Järgmine ▶', () => startLevel(n + 1), { big: '#22c55e' });
    mBtn(362, 460, 300, 110, 'Uuesti ↻', () => startLevel(n), { big: '#f59e0b' });
    mBtn(684, 460, 300, 110, 'Tasemed ☰', () => { M.screen = 'levels'; }, { big: '#8b5cf6' });
  } else if (M.screen === 'shop') {
    x.fillStyle = '#ff3d8b'; x.font = 'bold 56px system-ui'; x.textAlign = 'left'; x.fillText('Pood', 40, 60);
    x.fillStyle = '#b8860b'; x.font = 'bold 36px system-ui'; x.textAlign = 'right'; x.fillText('🪙 ' + coins, 984, 60);
    SHOP.forEach((it, i) => {
      const st = shopState(it);
      mBtn(40 + (i % 2) * 482, 115 + Math.floor(i / 2) * 112, 462, 96, it.name + ' – ' + (st.using ? 'kasutusel ✓' : st.has ? 'kasuta' : it.price + ' 🪙'), () => shopClick(it), { on: st.using });
    });
    mBtn(262, 700, 500, 86, 'Tagasi', () => { M.screen = 'main'; }, { big: '#22c55e' });
  } else {
    const isCode = M.edit === 'code';
    x.fillStyle = '#ff3d8b'; x.font = 'bold 54px system-ui'; x.textAlign = 'left'; x.fillText(isCode ? 'Ruumi kood' : 'Sinu nimi', 40, 60);
    x.fillStyle = '#eef'; x.beginPath(); x.roundRect(40, 105, 944, 80, 20); x.fill();
    x.fillStyle = '#1d1b3a'; x.font = 'bold 50px system-ui'; x.fillText((M.text || '') + '|', 64, 147);
    const keys = isCode ? 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789' : 'ABCDEFGHIJKLMNOPQRSTUVWXYZÕÄÖÜ-';
    const max = isCode ? 6 : 12;
    [...keys].forEach((k, i) => mBtn(40 + (i % 10) * 95, 210 + Math.floor(i / 10) * 92, 85, 80, k, () => {
      if (M.text.length >= max) return;
      M.text += isCode ? k : (M.text.length === 0 || M.text.endsWith(' ') ? k : k.toLowerCase());
    }));
    const by = 210 + Math.ceil(keys.length / 10) * 92 + 20;
    if (!isCode) mBtn(40, by, 300, 80, 'Tühik', () => { if (M.text.length < max && M.text && !M.text.endsWith(' ')) M.text += ' '; });
    mBtn(360, by, 200, 80, '⌫', () => { M.text = M.text.slice(0, -1); });
    mBtn(580, by, 404, 80, 'Valmis', () => {
      const v = M.text.trim();
      if (isCode) roomCode = v.replace(/[^A-Z0-9]/g, '') || roomCode; else myName = v || myName;
      syncHtml(); M.screen = 'main';
    }, { big: '#22c55e' });
  }
  for (const it of M.items) {
    const hov = M.hover && M.hover.x === it.x && M.hover.y === it.y;
    if (it.color !== undefined) {
      x.fillStyle = '#' + new THREE.Color(it.color).getHexString(); x.beginPath(); x.arc(it.x + it.w / 2, it.y + it.h / 2, 28, 0, 6.283); x.fill();
      if (it.on || hov) { x.strokeStyle = it.on ? '#1d1b3a' : '#999'; x.lineWidth = 6; x.beginPath(); x.arc(it.x + it.w / 2, it.y + it.h / 2, 34, 0, 6.283); x.stroke(); }
      continue;
    }
    x.fillStyle = it.big ? it.big : it.on ? '#1d1b3a' : hov ? '#dde3ff' : '#eef0f6';
    if (hov && it.big) x.globalAlpha = 0.8;
    x.beginPath(); x.roundRect(it.x, it.y, it.w, it.h, 18); x.fill(); x.globalAlpha = 1;
    x.fillStyle = it.big || it.on ? '#fff' : '#1d1b3a'; x.font = 'bold ' + (it.big ? 38 : 30) + 'px system-ui'; x.textAlign = 'center';
    x.fillText((it.on && !it.big && it.color === undefined && it.label ? '✓ ' : '') + it.label, it.x + it.w / 2, it.y + it.h / 2 + 2);
  }
  menuTex.needsUpdate = true;
}
function openKeyboard(which) { vrMenu.screen = 'keys'; vrMenu.edit = which; vrMenu.text = which === 'code' ? '' : ''; }
async function vrOnline() {
  syncHtml(); vrMenu.status = 'Ühendan ruumiga ' + roomCode + ' …'; drawMenu();
  const ok = await startOnline(roomCode);
  if (ok) { closeVrMenu(); startGame(true); }
  else { vrMenu.status = 'Ei saanud ühendust – ava mäng GitHubi lingilt'; drawMenu(); }
}
function menuHit(i) {
  const r = rays[i]; r.c.updateMatrixWorld(true);
  tmpA.setFromMatrixPosition(r.c.matrixWorld);
  tmpB.set(0, 0, -1).transformDirection(r.c.matrixWorld);
  raycaster.set(tmpA, tmpB);
  const hit = raycaster.intersectObject(menuMesh, false)[0];
  if (!hit || !hit.uv) return null;
  const px = hit.uv.x * MW, py = (1 - hit.uv.y) * MH;
  const item = vrMenu.items.find(it => px >= it.x && px <= it.x + it.w && py >= it.y && py <= it.y + it.h) || null;
  return { hit, item };
}
let lastRay = 1;
function menuSelect(i) {
  if (!vrMenu.open) return;
  lastRay = i;
  const h = menuHit(i); if (!h || !h.item) return;
  sfx('tap'); buzz(i, 0.4, 20);
  h.item.action(); drawMenu();
}
function updateVrMenu() {
  if (!vrMenu.open || !renderer.xr.isPresenting) { if (menuMesh.visible && !renderer.xr.isPresenting) menuMesh.visible = false; return; }
  if (vrMenu.place && camera.position.y > 0.3) { placeMenu(); vrMenu.place = false; }
  if (vrMenu.place) return;
  let hov = null;
  for (let i = 0; i < 2; i++) {
    const r = rays[i], h = menuHit(i);
    r.line.visible = true;
    if (h) { r.dot.visible = true; r.dot.position.copy(h.hit.point); r.line.scale.z = h.hit.distance; if (h.item && (i === lastRay || !hov)) hov = h.item; }
    else { r.dot.visible = false; r.line.scale.z = 3; }
  }
  const key = hov ? hov.x + ',' + hov.y : '';
  const old = vrMenu.hover ? vrMenu.hover.x + ',' + vrMenu.hover.y : '';
  if (key !== old) { vrMenu.hover = hov; drawMenu(); }
}
renderer.xr.addEventListener('sessionend', updateHud);

// =====================================================================
// MÄNGU TSÜKKEL
// =====================================================================
const clock = new THREE.Clock();
let lastTwScale = 1;
let hudT = 0, lastCd = -1, menuSpin = 0, lastStageKey = '', hadTail = false, camY = 1.6, lastGoalKey = '', fwT = 0;
buildMap('race', 0);
player.pos.set(0, 0.2, 1.5);
const headLocal = new V3();

const recBuf = new Map(); let recT = 0, replay = null, replayGhost = null;
function recordPositions(dt, t) {
  recT -= dt; if (recT > 0) return; recT = 0.1;
  const add = (id, p) => { if (!p) return; let a = recBuf.get(id); if (!a) recBuf.set(id, a = []); a.push([t, p.x, p.y, p.z]); while (a.length && t - a[0][0] > 7) a.shift(); };
  add(net.id, player.pos);
  for (const pr of net.peers.values()) if (pr.d) add(pr.id, pr.pos);
  if (net.isHost) bots.forEach(b => { if (b.visible) add(b.id, b.pos); }); else remoteBots.forEach(rb => { if (rb.mesh.visible) add(rb.id, rb.pos); });
}
function replayTick(dt, t, vr) {
  if (show.rt === 'race' && show.stage === 'play') {
    recordPositions(dt, t);
    if (show.q.length && (!replay || replay.rid !== show.rid)) {
      const id = show.q[0], path = (recBuf.get(id) || []).filter(e => e[0] > t - 5.5).map(e => e.slice());
      if (path.length > 5) replay = { rid: show.rid, id, name: nameOf(id), path, t0: path[0][0], played: false };
    }
  }
  if (show.stage !== 'results' || !replay || replay.rid !== show.rid || vr) { if (replayGhost) { scene.remove(replayGhost); replayGhost = null; } return false; }
  if (!replay.started) { replay.started = true; replay.clock = 0; showToast('KORDUS: ' + replay.name + ' finišisse!', 3); }
  replay.clock += dt;
  const P = replay.path, tt = replay.t0 + replay.clock;
  if (tt > P[P.length - 1][0] + 0.8) { if (replayGhost) { scene.remove(replayGhost); replayGhost = null; } replay.done = true; return false; }
  let i = 0; while (i < P.length - 2 && P[i + 1][0] < tt) i++;
  const a = P[i], b = P[Math.min(i + 1, P.length - 1)], u = Math.max(0, Math.min(1, (tt - a[0]) / ((b[0] - a[0]) || 1)));
  const gx = a[1] + (b[1] - a[1]) * u, gy = a[2] + (b[2] - a[2]) * u, gz = a[3] + (b[3] - a[3]) * u;
  if (!replayGhost) {
    const pr = net.peers.get(replay.id);
    replayGhost = replay.id.startsWith('#') ? makeBot(0xffffff, 0) : replay.id === net.id ? makeHuman(myColor, myName, myHat, myCostume) : makeHuman(pr ? pr.color : 0xffffff, replay.name, pr ? pr.hat : 0, pr ? pr.costume : 0);
  }
  let dx = b[1] - a[1], dz = b[3] - a[3]; const dl = Math.hypot(dx, dz);
  if (dl > 0.01) { replayGhost.rotation.y = Math.atan2(dx, dz); replay.dir = [dx / dl, dz / dl]; }
  replayGhost.position.set(gx, gy, gz);
  const d = replay.dir || [0, -1];
  rig.position.set(gx - d[0] * 4.5, gy + 0.9, gz - d[1] * 4.5); rig.rotation.y = Math.atan2(-d[0], -d[1]); camera.position.set(0, 1.6, 0); camera.rotation.x = -0.28;
  replay.active = true;
  return true;
}
function onStageChange() {
  const st = show.stage;
  if (st === 'intro' && show.rn === 1) { prog.shows++; saveProg(); }
  if (st === 'results') {
    const me = net.id;
    if (show.part.includes(me)) {
      prog.rounds++;
      if (show.alive.includes(me)) { prog.qual++; sfx('cheer'); showMsg('Pääsesid edasi!', ['Edasi pääses ' + show.alive.length + '/' + show.part.length], 3); setTimeout(() => addCoins(5, 'edasipääsu eest'), 600); }
      else { sfx('out'); showMsg('Langesid välja', ['Järgmisi voore saad pealt vaadata'], 3); }
      if (show.rt === 'boss' && show.boss && show.boss.won && show.alive.includes(me)) setTimeout(() => addCoins(10, 'bossi võitmise eest!'), 1400);
      if (show.alive.includes(me)) { questTick('qual'); addXp(20); addTrophies(show.easy ? 2 : 3 + Math.min(3, show.rn)); }
      else if (!show.easy) addTrophies(-2);
      saveProg();
    }
  }
  if (st === 'crown') {
    sfx('fanfare');
    const wp = posOf(show.winner); burst(wp || player.pos);
    if (show.winner === net.id) { prog.crowns++; saveProg(); setTimeout(() => addCoins(50, 'KROONI eest!'), 900); questTick('crown'); addXp(100); addTrophies(show.easy ? 10 : 25); }
    fwT = 0;
  }
  if (st === 'play' && show.part.includes(net.id)) { sfx('go'); showMsg('MINE!', [twistLine()].filter(Boolean), 1.2); }
  if (st === 'cdone' && show.part.includes(net.id) && show.ok) { sfx('cheer'); burst(player.pos); setTimeout(() => addCoins(8, 'koostöö!'), 600); addXp(15); addTrophies(2); }
  if (st === 'cwin') {
    sfx('fanfare'); burst(player.pos); fwT = 0;
    if (show.part.includes(net.id)) { const old = prog.coopBest || 0; if (!old || show.ct < old) prog.coopBest = show.ct; saveProg(); setTimeout(() => addCoins(30, 'kõik koostöörajad!'), 900); addXp(60); addTrophies(6); }
  }
}

renderer.setAnimationLoop(() => {
  const dt = Math.min(clock.getDelta(), 0.05);
  const vr = renderer.xr.isPresenting;

  fitScreen();
  if (!started) {
    updateWorld(dt, false);
    if (vr) { rig.position.set(0, 0, 9); rig.rotation.y = 0; updateVrMenu(); }
    else {
      menuSpin += dt * 0.1;
      rig.position.set(Math.sin(menuSpin) * 14, 6, 6 + Math.cos(menuSpin) * 4); rig.rotation.y = Math.sin(menuSpin) * 0.3;
      camera.rotation.x = -0.35;
    }
    updateScenery(dt, now()); sky.position.copy(rig.position); gooMat.uniforms.camPos.value.copy(rig.position);
    renderer.render(scene, camera); return;
  }
  if (show.rid !== loadedRid) loadForShow();
  updateWorld(dt, false);
  emoteCd -= dt;
  player.bounceT = Math.max(0, (player.bounceT || 0) - dt);
  if (player.onGround && !(player.standingOn && player.standingOn.bounce)) player.bounceT = Math.min(player.bounceT, 0.2);

  const t = T();
  const stageKey = show.rid + show.stage;
  if (stageKey !== lastStageKey) { lastStageKey = stageKey; onStageChange(); }

  // stardiloendus
  if (show.stage === 'intro') {
    const n = Math.ceil(show.sa - t);
    if (n !== lastCd) {
      lastCd = n;
      const nm = mapName(show.rt, show.ti);
      const desc = show.mode === 'coop' ? 'Kõik peavad finišisse jõudma · seiske koos kollastel nuppudel!' : show.mode === 'levels' && show.level === 0 ? dailyDesc() : show.mode === 'levels' ? (levelStars(show.level) ? 'Rekord ' + prog.levels[show.level].t + ' s · 3 tähte alla ' + (TR.par || 40) + ' s' : '3 tähte, kui jõuad alla ' + (TR.par || 40) + ' s') : show.rt === 'race' && TRACKS[show.ti] && TRACKS[show.ti].custom ? 'Teie enda ehitatud rada! Esimesed ' + show.quota + ' edasi' : show.rt === 'race' ? 'Esimesed ' + show.quota + ' pääsevad edasi' : show.final ? 'Viimane püstiseisja võidab krooni!' : ARENAS[show.rt].desc;
      if (n <= 3 && n > 0) { showMsg(String(n), [], 1.1); sfx('beep'); }
      else if (n > 3) showMsg(nm, [show.mode === 'coop' ? '🤝 Koostöö · rada ' + show.cl + '/' + COOPS.length : show.mode === 'levels' ? (show.level ? 'Tasemete režiim' : '📅 Päeva väljakutse') : (show.final ? 'FINAAL · ' : 'Voor ' + show.rn + ' · ') + show.part.length + ' mängijat', desc, twistLine(), spectating ? 'Sina vaatad pealt' : ''].filter(Boolean), 1.2);
    }
  }

  // VR: pikkuse kalibreerimine ja päris kõndimine
  if (vr) {
    headStand = Math.max(headStand - dt * 0.01, camera.position.y);
    headLocal.set(camera.position.x, 0, camera.position.z).applyQuaternion(rig.quaternion);
    const hx = rig.position.x + headLocal.x, hz = rig.position.z + headLocal.z;
    if (Math.hypot(hx - player.pos.x, hz - player.pos.z) < 0.5) { player.pos.x = hx; player.pos.z = hz; }
  }

  updateVrMenu();
  updateBuilder();
  if (vr && !vrMenu.open) { const r = rays[1]; r.line.visible = !!buildAimCache; r.dot.visible = false; if (buildAimCache) r.line.scale.z = 6; if (!buildAimCache) rays[0].line.visible = false; }
  const canMove = (show.stage !== 'intro' || spectating) && !vrMenu.open;
  grabEffect(player, dt);
  handleInput(dt, canMove);

  if (net.isHost) {
    for (const b of bots) if (b.visible) { grabEffect(b, dt); if (!b.carried) botAI(b, dt); }
    botGrabs(dt);
  }
  const off = vr ? armOffset() : 0;
  player.h = vr ? Math.max(0.6, Math.min(1.5, headStand - off)) : 1.5;
  if (player.ducking) player.h = Math.min(player.h, 0.75);
  if (net.isHost) for (const b of bots) { b.duckT = (b.duckT || 0) - dt; b.h = b.duckT > 0 ? 0.7 : 1.4; if (b.mesh) b.mesh.userData.duck = b.duckT > 0; }
  stepEntity(player, dt);
  if (net.isHost) for (const b of bots) if (b.visible) stepEntity(b, dt);

  // lükkamine
  const others = allOthers();
  if (!player.carried) for (const o of others) { if (o.id === pcGrab || grabHold.includes(o.id)) continue; pushApart(player, o.pos.x, o.pos.y, o.pos.z); }
  if (net.isHost) {
    for (let i = 0; i < bots.length; i++) {
      if (!bots[i].visible) continue;
      for (let j = i + 1; j < bots.length; j++) { if (!bots[j].visible) continue; const m = pushApart(bots[i], bots[j].pos.x, bots[j].pos.y, bots[j].pos.z); if (m) { bots[j].pos.x += m[0]; bots[j].pos.z += m[1]; } }
      if (!bots[i].carried) pushApart(bots[i], player.pos.x, player.pos.y, player.pos.z);
      for (const p of net.peers.values()) if (p.d) pushApart(bots[i], p.pos.x, p.pos.y, p.pos.z);
    }
  }

  // kukkumine, kontrollpunktid, finiš (enda ja peremehe robotid)
  if (player.pos.y < TR.outY) { splash(player); onPlayerFall(); }
  if (show.rt === 'race' && !spectating) {
    for (let c = player.cp + 1; c < TR.cps.length; c++) if (player.onGround && player.pos.z < TR.cps[c].trig) { player.cp = c; sfx('ding'); showToast('Kontrollpunkt!', 1.2); }
    if (!player.finished && show.stage === 'play' && player.pos.z < TR.finishZ && player.pos.y > TR.cps[0].pos.y - 1 && show.part.includes(net.id)) {
      player.finished = true; myFinishTime = t - show.sa; burst(player.pos); sfx('cheer');
      if (!(prog.best[TR.name] < myFinishTime)) { prog.best[TR.name] = Math.round(myFinishTime * 10) / 10; showToast('Uus rekord: ' + myFinishTime.toFixed(1) + ' s!', 2); }
      saveProg(); setTimeout(() => addCoins(3, 'finiš'), 300); questTick('fin'); addXp(10);
      if (net.isHost) recordFinish(net.id, myFinishTime); else net.fin.send({ t: myFinishTime, r: show.rid });
      if (show.mode === 'levels') levelComplete();
    }
  }
  if (net.isHost) {
    for (const b of bots) {
      if (!b.visible) continue;
      if (b.pos.y < TR.outY) {
        splash(b);
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
  if (show.stage === 'lobby') { drawBoard(); drawStatBoard(); rebuildBuilder(false); }
  // mündid
  const tnow = now();
  for (const bx of TR.boxes) {
    const show1 = bx.back < tnow; bx.m.visible = show1; bx.m.rotation.y += dt * 1.5; bx.m.position.y = bx.y + Math.sin(tnow * 2 + bx.x) * 0.12;
    if (show1 && !spectating && Math.hypot(player.pos.x - bx.x, player.pos.z - bx.z) < 1.0 && Math.abs(player.pos.y + 0.8 - bx.y) < 1.3) { bx.back = tnow + 5; puff(bx.m.position, 0xffd43b, 10, 2, 0.5, 1); givePower(player, true); }
    if (net.isHost && bx.botBack < tnow) for (const b of bots) if (b.visible && Math.hypot(b.pos.x - bx.x, b.pos.z - bx.z) < 1.0 && Math.abs(b.pos.y + 0.8 - bx.y) < 1.3) { bx.botBack = tnow + 5; givePower(b, false); break; }
  }
  const magnet = player.pw === 'magnet' && player.pwT > tnow;
  if (TR.coins.length && !spectating) for (const c of TR.coins) {
    if (c.taken) continue;
    if (magnet) { const mx = player.pos.x - c.m.position.x, my = player.pos.y + 0.8 - c.m.position.y, mz = player.pos.z - c.m.position.z, md = Math.hypot(mx, my, mz); if (md < 7 && md > 0.01) c.m.position.addScaledVector(tmpC.set(mx, my, mz).normalize(), Math.min(md, dt * 12)); }
    const dx = c.m.position.x - player.pos.x, dy = c.m.position.y - (player.pos.y + 0.8), dz = c.m.position.z - player.pos.z;
    if (dx * dx + dy * dy + dz * dz < 0.8) { c.taken = true; c.m.visible = false; coins++; saveProg(); sfx('coin'); questTick('coin'); addXp(1); if (show.stage === 'trenn') practiceDone('coin'); puff(c.m.position, 0xffd43b, 6, 1.2, 0.4, 0.8); }
  }
  // ilutulestik kroonimisel
  if (show.stage === 'crown' || show.stage === 'cwin') {
    fwT -= dt;
    if (fwT <= 0) {
      fwT = 0.4; const c = posOf(show.winner) || player.pos;
      const pt = { x: c.x + rnd(-7, 7), y: c.y + rnd(6, 11), z: c.z + rnd(-7, 7) };
      puff(pt, [0xff3d8b, 0xffd43b, 0x2f9bff, 0x22c55e, 0xb388ff][Math.floor(Math.random() * 5)], 14, 4.5, 0.9, 1.2); sfx('pop');
    }
  }
  { const sc = twOn('hiid') ? 1.45 : 1;
    if (sc !== lastTwScale || (sc !== 1 && Math.random() < 0.05)) { lastTwScale = sc; bots.forEach(b => b.mesh && b.mesh.scale.setScalar(sc)); remoteBots.forEach(rb => rb.mesh.scale.setScalar(sc)); for (const pp of net.peers.values()) if (pp.avatar) pp.avatar.scale.setScalar(sc); } }
  feedTick();
  replayTick(dt, t, vr);
  updateDrops(dt);
  dropCd = Math.max(0, dropCd - dt);
  if (player.pwT && player.pwT < tnow && player.pw) { player.pw = null; }
  shieldBubble.visible = !vr && false;
  updateRain(dt);
  updatePets(dt);
  if (show.rt === 'boss') bossVisual(dt, t);
  if (TR.gates.length) gateVisual(dt, t);
  if (show.stage === 'trenn') practiceTick();

  // värav
  if (show.rt === 'pall' && show.lastGoal) { const k = show.rid + show.lastGoal.join(); if (k !== lastGoalKey) { lastGoalKey = k; sfx('goal'); showToast('VÄRAV! ' + TEAM_NAMES[show.lastGoal[0]] + ' · ' + show.score[0] + ':' + show.score[1], 2); if (TR.ballMesh) puff(TR.ballMesh.position, TEAM_COLORS[show.lastGoal[0]], 14, 3, 0.9, 2); } }
  // pall on tahke ka mängijale
  if (TR.ballMesh && show.rt === 'pall') { const bp = TR.ballMesh.position, dx = player.pos.x - bp.x, dz = player.pos.z - bp.z, d = Math.hypot(dx, dz); if (d < 1.4 && d > 1e-4 && player.pos.y < bp.y + 1.1) { player.pos.x = bp.x + dx / d * 1.4; player.pos.z = bp.z + dz / d * 1.4; } }
  // saba saamise heli
  const hasTail = show.rt === 'saba' && show.tails.includes(net.id);
  if (show.rt === 'saba' && show.stage === 'play' && hasTail !== hadTail) { if (hasTail) { sfx('tail'); showToast('Said saba! Hoia seda!', 1.5); } else { sfx('hit'); showToast('Su saba võeti ära!', 1.5); } }
  hadTail = hasTail;

  // kaamera keha juurde
  if (vr) {
    headLocal.set(camera.position.x, 0, camera.position.z).applyQuaternion(rig.quaternion);
    rig.position.set(player.pos.x - headLocal.x, player.pos.y - off, player.pos.z - headLocal.z);
  } else {
    if (!(replayGhost && show.stage === 'results')) { rig.position.copy(player.pos); camY += ((player.ducking ? 0.95 : 1.6) - camY) * Math.min(1, dt * 12); camera.position.set(0, camY, 0); if (replay && replay.active && !replayGhost) { replay.active = false; camera.rotation.x = pitch; } }
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
      if (buildAimCache) line = 'Rajaehitaja · ' + (renderer.xr.isPresenting ? 'parem päästik = ehita, vasak = kustuta, B = tüüp' : isTouch ? 'nupud all' : 'F = ehita, G = kustuta, T = tüüp') + ' (' + BUILD_TYPES[buildType] + ')';
      else line = 'Ootesaal · ' + (net.isHost ? (renderer.xr.isPresenting ? 'Astu rohelisele ringile, et alustada' : 'Vajuta ALUSTA, kui kõik on kohal') : 'Ootame, kuni ' + nameOf(net.hostId) + ' alustab');
    } else if (show.mode === 'levels') {
      const rec = show.level ? prog.levels[show.level] : (prog.daily && prog.daily.day === dayKey() ? prog.daily : null);
      line = (show.level ? 'Tase ' + show.level : '📅 Päeva väljakutse') + ' · ' + (show.stage === 'lvdone' ? myFinishTime.toFixed(1) : tau.toFixed(1)) + ' s' + (rec ? ' · rekord ' + rec.t + ' s' : '') + ' · 3⭐ alla ' + (TR.par || 40) + ' s';
    } else if (show.mode === 'coop') {
      const hs = show.part.filter(isHuman);
      line = '🤝 Koostöö ' + show.cl + '/' + COOPS.length + ' · ' + clock + ' · finišis ' + hs.filter(id => show.q.includes(id)).length + '/' + hs.length + (show.ct ? ' · kokku ' + Math.round(show.ct) + ' s' : '');
    } else if (show.stage === 'trenn') {
      line = practiceLine();
    } else if (spectating) {
      line = 'Vaatad pealt · ' + (show.stage === 'play' ? (renderer.xr.isPresenting ? 'päästik' : isTouch ? 'nupp all' : 'F') + ' = viska pall rajale' + (dropCd > 0 ? ' (' + Math.ceil(dropCd) + ')' : '') + ' · ' : '') + TR.name + (SURVIVAL[show.rt] ? ' · alles ' + show.inR.length : '');
    } else if (show.rt === 'race') {
      const qi = show.q.indexOf(net.id);
      line = TR.name + ' · ' + clock + ' · ' + (qi >= 0 ? (qi + 1) + '. koht ✓' : 'edasi ' + show.q.length + '/' + show.quota);
    } else if (show.rt === 'saba') {
      line = (hasTail ? 'SUL ON SABA! Põgene!' : 'Püüa saba!') + ' · ' + Math.max(0, Math.ceil(45 - tau)) + ' s';
    } else if (TEAM[show.rt]) {
      const tm = show.teams ? show.teams[net.id] : undefined, sc = show.score || [0, 0];
      line = 'Sinine ' + sc[0] + ' : ' + sc[1] + ' Punane' + (tm !== undefined ? ' · sina: ' + TEAM_NAMES[tm].toUpperCase() : '') + ' · ' + Math.max(0, Math.ceil((show.rt === 'pall' ? 75 : 60) - tau)) + ' s';
    } else if (show.rt === 'boss' && show.boss) {
      line = 'Bossivoor · ' + (tau < 15 ? 'boss on kaitstud veel ' + Math.ceil(15 - tau) + ' s – hoia meteooridest eemale' : 'nupud ' + show.boss.pads.filter(x => x).length + '/4 (seisa 3 s)') + (show.boss.won ? ' · VÕIT! 🎉' : '') + ' · alles ' + show.inR.length + ' · ' + Math.max(0, Math.ceil(70 - tau)) + ' s';
    } else if (show.rt === 'magi') {
      const list = Object.entries(show.hill || {}).sort((a, b) => b[1] - a[1]);
      line = 'Mäe tipp · sinu punktid ' + Math.floor((show.hill || {})[net.id] || 0) + (list[0] ? ' · liider ' + nameOf(list[0][0]) + ' ' + Math.floor(list[0][1]) : '') + ' · ' + Math.max(0, Math.ceil(50 - tau)) + ' s';
    } else line = TR.name + ' · alles ' + show.inR.length + (show.final ? '' : ' · edasi ' + show.quota) + ' · ' + clock;
    if (player.pw && player.pwT > now() && !spectating) { const pw = POWERS.find(p => p.id === player.pw); if (pw) line = pw.name + ' ' + Math.ceil(player.pwT - now()) + 's · ' + line; }
    if (raining) line = '🌧️ ' + line;
    if (show.tw && twOn(show.tw)) line = TWISTS[show.tw].split(' ')[0] + ' ' + line;
    drawStats(line);
    if (show.stage === 'cdone') {
      showMsg(show.ok ? 'Rada läbitud! 🤝' : 'Aeg sai otsa…', [show.ok ? 'Tiimi aeg ' + show.lt + ' s' : 'Proovime sama rada uuesti', show.ok && show.cl < COOPS.length ? 'Järgmine rada ' + Math.max(0, Math.ceil(show.ea + 6 - t)) + ' s pärast' : show.ok ? 'See oli viimane rada!' : 'Uus katse ' + Math.max(0, Math.ceil(show.ea + 6 - t)) + ' s pärast'], 1, true);
    } else if (show.stage === 'cwin') {
      showMsg('KOOSTÖÖ VÕIDETUD! 🏆', ['Kõik ' + COOPS.length + ' rada läbitud', 'Kogu aeg ' + Math.round(show.ct) + ' s' + (prog.coopBest ? ' · rekord ' + Math.round(prog.coopBest) + ' s' : ''), 'Tagasi ootesaali ' + Math.max(0, Math.ceil(show.ea + 9 - t)) + ' s'], 1, true);
    } else if (show.stage === 'results') {
      showMsg(show.alive.includes(net.id) ? 'Pääsesid edasi!' : show.part.includes(net.id) ? 'Langesid välja' : 'Voor läbi!',
        [...(TEAM[show.rt] ? [show.winTeam >= 0 ? 'Võitis ' + TEAM_NAMES[show.winTeam] + ' (' + show.score[0] + ':' + show.score[1] + ')' : 'Viik ' + show.score[0] + ':' + show.score[1]] : []),
         'Edasi pääses ' + show.alive.length + '/' + show.part.length, 'Järgmine voor ' + Math.max(0, Math.ceil(show.ea + 6.5 - t)) + ' s pärast'], 1, true);
    } else if (show.stage === 'crown') {
      const nm = nameOf(show.winner);
      showMsg(show.winner === net.id ? 'SINA VÕITSID!' : 'Võitja: ' + nm, ['👑 ' + nm + ' – kroone kokku ' + (show.crowns[nm] || 1), ...(show.aw || []).slice(0, 2).map(a => a[0] + ': ' + a[1]), 'Tagasi ootesaali ' + Math.max(0, Math.ceil(show.ea + 9 - t)) + ' s'], 1, true);
    } else if (show.rt === 'race' && player.finished && show.stage === 'play') {
      const qi = show.q.indexOf(net.id);
      showMsg('Finiš!', [(qi >= 0 ? (qi + 1) + '. koht · ' : '') + myFinishTime.toFixed(1) + ' s', show.mode === 'coop' ? 'Ootame tiimikaaslasi – mine aita neid nuppudel!' : 'Pääsesid edasi – ootame teisi…'], 1, true);
    } else if (msgSticky) hideMsg();
    updateHud();
  }
  if (!msgSticky) { msgTimer -= dt; msg.mesh.visible = msgTimer > 0; }
  if (toastT > 0) { toastT -= dt; if (toastT <= 0) toast.mesh.visible = false; }

  // taust ja valgus järgivad mängijat
  updatePuffs(dt); updateScenery(dt, t);
  sky.position.copy(rig.position);
  goo.position.x = Math.round(player.pos.x / 10) * 10; goo.position.z = Math.round(player.pos.z / 10) * 10;
  gooMat.uniforms.camPos.value.copy(player.pos);
  sun.target.position.copy(player.pos); sun.position.set(player.pos.x + 14, player.pos.y + 32, player.pos.z + 10);

  renderer.render(scene, camera);
});
</script>
</body>
</html>
