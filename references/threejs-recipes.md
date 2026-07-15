# Three.js Recipes — สูตรฉาก 3D Hero 5 แบบ

ทุกสูตรใช้โครงเดียวกัน: สร้าง scene → ใส่วัตถุ → animate ใน loop →
ตอบสนองเมาส์/scroll → จัดการ resize + fallback

## โครงพื้นฐาน (ใช้ทุกสูตร)

```js
const canvas = document.querySelector('#hero-canvas');
let renderer, scene, camera;
try {
  renderer = new THREE.WebGLRenderer({ canvas, alpha: true, antialias: true });
} catch(e) {
  canvas.style.display = 'none';           // fallback: ซ่อน canvas
  document.body.classList.add('no-webgl'); // CSS แสดง gradient แทน
}
renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
renderer.setSize(innerWidth, innerHeight);
scene = new THREE.Scene();
camera = new THREE.PerspectiveCamera(45, innerWidth/innerHeight, 0.1, 100);
camera.position.z = 6;

// เมาส์ parallax — ใช้ lerp ให้นุ่ม
const mouse = { x: 0, y: 0 }, target = { x: 0, y: 0 };
addEventListener('mousemove', e => {
  target.x = (e.clientX / innerWidth - 0.5) * 2;
  target.y = (e.clientY / innerHeight - 0.5) * 2;
});

function tick(t) {
  mouse.x += (target.x - mouse.x) * 0.05;
  mouse.y += (target.y - mouse.y) * 0.05;
  // ...อัปเดตวัตถุตามสูตรที่เลือก...
  renderer.render(scene, camera);
  requestAnimationFrame(tick);
}
requestAnimationFrame(tick);

addEventListener('resize', () => {
  camera.aspect = innerWidth/innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(innerWidth, innerHeight);
});
```

แสงมาตรฐานที่สวยเกือบทุกกรณี:
```js
scene.add(new THREE.AmbientLight(0xffffff, 0.4));
const key = new THREE.DirectionalLight(0xffffff, 1.2); key.position.set(3,4,5);
const rim = new THREE.DirectionalLight(0x6d5dfc, 2);  rim.position.set(-4,-2,-3);
scene.add(key, rim);
```

## สูตร 1 — Floating Torus Knot (นามธรรม หรู — default ที่ดีที่สุด)

```js
const geo = new THREE.TorusKnotGeometry(1.1, 0.38, 220, 40);
const mat = new THREE.MeshStandardMaterial({
  color: 0x6d5dfc, metalness: 0.85, roughness: 0.15,
});
const knot = new THREE.Mesh(geo, mat); scene.add(knot);
// ใน tick():
knot.rotation.x = t*0.0002 + mouse.y*0.3;
knot.rotation.y = t*0.0003 + mouse.x*0.5;
knot.position.y = Math.sin(t*0.001)*0.15; // ลอยขึ้นลง
```

## สูตร 2 — Particle Field (เทค/AI/cyber)

```js
const N = 2500, pos = new Float32Array(N*3);
for (let i=0;i<N*3;i++) pos[i] = (Math.random()-0.5)*12;
const g = new THREE.BufferGeometry();
g.setAttribute('position', new THREE.BufferAttribute(pos,3));
const p = new THREE.Points(g, new THREE.PointsMaterial({
  color:0x00e5ff, size:0.03, transparent:true, opacity:0.85 }));
scene.add(p);
// tick: p.rotation.y = t*0.00006 + mouse.x*0.15;
```

## สูตร 3 — Morphing Blob (นุ่ม อินทรีย์ — pastel/organic)

ใช้ IcosahedronGeometry ละเอียดสูง แล้วขยับ vertex ด้วย sine หลายคลื่น:
```js
const geo = new THREE.IcosahedronGeometry(1.4, 48);
const base = geo.attributes.position.array.slice();
const blob = new THREE.Mesh(geo, new THREE.MeshStandardMaterial({
  color:0xff8fab, metalness:0.1, roughness:0.6 }));
scene.add(blob);
// ใน tick():
const pos = geo.attributes.position;
for (let i=0;i<pos.count;i++){
  const ox=base[i*3], oy=base[i*3+1], oz=base[i*3+2];
  const n = 0.12*Math.sin(ox*2.1+t*0.0012) + 0.09*Math.sin(oy*3.3+t*0.0009);
  const s = 1+n;
  pos.setXYZ(i, ox*s, oy*s, oz*s);
}
pos.needsUpdate = true; geo.computeVertexNormals();
```

## สูตร 4 — Product Cards ลอย 3 มิติ (ecommerce แบบ Etail)

กล่อง/แผ่นหลายใบลอยเป็นวง แต่ละใบหมุนเอียงเล็กน้อย:
```js
const group = new THREE.Group(); scene.add(group);
for (let i=0;i<6;i++){
  const card = new THREE.Mesh(
    new THREE.BoxGeometry(1.1, 1.5, 0.06),
    new THREE.MeshStandardMaterial({ color:0xffffff, metalness:0.2, roughness:0.4 }));
  const a = i/6*Math.PI*2;
  card.position.set(Math.cos(a)*2.4, Math.sin(a*2)*0.4, Math.sin(a)*2.4);
  card.lookAt(0,0,0);
  group.add(card);
}
// tick: group.rotation.y = t*0.00025 + mouse.x*0.4;
```

## สูตร 5 — Scroll-driven Camera (เล่าเรื่องตาม scroll)

ผูกตำแหน่งกล้องกับ ScrollTrigger — hero ค้างจอ (pin) แล้วกล้องบินรอบวัตถุ:
```js
gsap.to(camera.position, {
  z: 3.2, y: 1.1,
  scrollTrigger: { trigger:'.hero', start:'top top', end:'+=120%',
                   scrub: 1, pin: true }
});
```

## เกณฑ์ประสิทธิภาพ

- pixelRatio จำกัดที่ 2 เสมอ
- มือถือ (`innerWidth < 768`): ลดจำนวน particle/segment ลงครึ่งหนึ่ง
- หยุด render loop เมื่อ tab ไม่ active (`document.hidden`)
- `prefers-reduced-motion` → หมุนช้าลง 90% หรือหยุดนิ่งเป็นภาพสวย
