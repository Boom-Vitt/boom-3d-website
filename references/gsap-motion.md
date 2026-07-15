# GSAP Motion — จังหวะ animation ที่ทำให้เว็บดู "แพง"

หลักการ: ทุกอย่างเข้าฉากด้วย ease เดียวกันทั้งเว็บ (แนะนำ `power3.out`)
ระยะเวลา 0.8-1.2s และ stagger 0.08-0.12s — ความสม่ำเสมอ = ความโปร

## 1. Preloader → Hero sequence (เปิดเว็บ)

```js
const tl = gsap.timeline({ defaults:{ ease:'power3.out' } });
tl.to('.preloader', { yPercent:-100, duration:0.9, delay:0.6 })
  .from('.nav',        { y:-30, opacity:0, duration:0.8 }, '-=0.3')
  .from('.hero-title .line', { yPercent:110, duration:1, stagger:0.1 }, '-=0.5')
  .from('.hero-sub',   { y:24, opacity:0, duration:0.8 }, '-=0.6')
  .from('.hero-cta',   { y:24, opacity:0, duration:0.8 }, '-=0.65');
```

## 2. Text reveal ทีละบรรทัด (สำคัญมาก — คือลายเซ็นของเว็บสวย)

HTML: ห่อแต่ละบรรทัดด้วย mask
```html
<h1 class="hero-title">
  <span class="mask"><span class="line">สร้างเว็บที่</span></span>
  <span class="mask"><span class="line">โลกต้องหันมอง</span></span>
</h1>
```
```css
.mask { display:block; overflow:hidden; }
.line { display:block; }
```
ภาษาไทยระวังสระบน/ล่างโดน mask ตัด → ใส่ `padding: 0.12em 0` ที่ `.line`
แล้วชดเชยด้วย `margin: -0.12em 0` ที่ `.mask`

## 3. Section เข้าฉากตอน scroll

```js
gsap.registerPlugin(ScrollTrigger);
gsap.utils.toArray('.reveal').forEach(el => {
  gsap.from(el, {
    y: 60, opacity: 0, duration: 1, ease: 'power3.out',
    scrollTrigger: { trigger: el, start: 'top 82%' }
  });
});
// cards แบบ stagger:
gsap.from('.card', {
  y: 80, opacity: 0, stagger: 0.1, duration: 0.9, ease: 'power3.out',
  scrollTrigger: { trigger: '.cards', start: 'top 75%' }
});
```

## 4. ตัวเลขนับขึ้น (social proof)

```js
gsap.utils.toArray('.stat-num').forEach(el => {
  const end = +el.dataset.value;
  gsap.fromTo(el, { innerText: 0 }, {
    innerText: end, duration: 1.6, ease: 'power1.out', snap: { innerText: 1 },
    scrollTrigger: { trigger: el, start: 'top 85%' }
  });
});
```

## 5. Hover 3D tilt บน card (ไม่ต้องใช้ Three.js)

```js
document.querySelectorAll('.tilt').forEach(card => {
  card.addEventListener('mousemove', e => {
    const r = card.getBoundingClientRect();
    const x = (e.clientX - r.left) / r.width - 0.5;
    const y = (e.clientY - r.top) / r.height - 0.5;
    gsap.to(card, { rotateY: x*12, rotateX: -y*12, duration:0.4, ease:'power2.out',
                    transformPerspective: 800 });
  });
  card.addEventListener('mouseleave', () =>
    gsap.to(card, { rotateX:0, rotateY:0, duration:0.6, ease:'power3.out' }));
});
```

## 6. Nav เปลี่ยนสถานะเมื่อ scroll

```js
ScrollTrigger.create({
  start: 'top -80',
  onUpdate: self => document.querySelector('.nav')
    .classList.toggle('scrolled', self.progress > 0)
});
```
```css
.nav { transition: background .4s, backdrop-filter .4s; }
.nav.scrolled { background: rgba(10,10,15,.6); backdrop-filter: blur(14px); }
```

## 7. Smooth scroll (ทางเลือก)

ถ้าอยากได้ scroll หนืดแบบเว็บ awwwards ใช้ Lenis:
```html
<script src="https://unpkg.com/lenis@1.1.13/dist/lenis.min.js"></script>
```
```js
const lenis = new Lenis({ lerp: 0.1 });
lenis.on('scroll', ScrollTrigger.update);
gsap.ticker.add(t => lenis.raf(t*1000));
gsap.ticker.lagSmoothing(0);
```

## Reduced motion (ห้ามลืม)

```js
if (matchMedia('(prefers-reduced-motion: reduce)').matches) {
  gsap.globalTimeline.timeScale(100); // ทุกอย่างจบทันที
  ScrollTrigger.getAll().forEach(s => s.disable(false));
}
```
