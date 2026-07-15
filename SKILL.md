---
name: boom-3d-website
description: สร้างเว็บไซต์ landing page ระดับ Dribbble พร้อม 3D animation (Three.js + GSAP) แบบครบวงจร โดยเริ่มจากสัมภาษณ์ผู้ใช้เป็นภาษาไทยก่อนเสมอ — ถามธุรกิจ ธีม โทนสี ฟอนต์ และคอนเซ็ปต์ 3D แล้วจึงลงมือสร้าง ใช้ skill นี้ทุกครั้งที่ผู้ใช้อยากได้ "เว็บสวยๆ", "landing page", "เว็บ 3D", "ทำเว็บให้ธุรกิจ", "หน้าเว็บแบบ Dribbble", "stunning website", "3D animation website" หรือพูดถึงการทำหน้าเว็บที่ต้องว้าว แม้ไม่ได้พูดคำว่า 3D ตรงๆ ก็ตาม
---

# Boom 3D Website — สร้างเว็บระดับ Dribbble พร้อม 3D Animation

Skill นี้ช่วยสร้างเว็บไซต์ landing page คุณภาพระดับงานโชว์บน Dribbble
(อ้างอิงสไตล์: [Etail landing page — 3D animation](https://dribbble.com/shots/25571331-Etail-landing-page-web-design-3D-animation))
คือเว็บที่มี **hero 3D เคลื่อนไหวได้จริง** ตัวอักษรจัดวางแบบ editorial
และ animation ที่ลื่นไหลทุกการ scroll — ไม่ใช่เว็บ template ธรรมดา

## หลักคิดสำคัญ

เว็บที่ "ว้าว" เกิดจาก 3 อย่างรวมกัน ขาดอย่างใดอย่างหนึ่งไม่ได้:

1. **3D hero ที่มีชีวิต** — วัตถุ 3D ที่หมุน ลอย หรือตอบสนองเมาส์/scroll (Three.js)
2. **Motion ที่มีจังหวะ** — ตัวหนังสือ reveal ทีละบรรทัด, section เลื่อนเข้าแบบ stagger (GSAP + ScrollTrigger + Lenis)
3. **Typography + สีที่กล้า** — heading ใหญ่มาก (clamp 3-8rem), palette ชัดเจน, ที่ว่างเยอะ

## ขั้นตอนการทำงาน

### ขั้นที่ 1 — สัมภาษณ์ผู้ใช้ (ภาษาไทยเสมอ ห้ามข้าม)

ก่อนเขียนโค้ดแม้แต่บรรทัดเดียว ใช้เครื่องมือถามคำถาม (AskUserQuestion ถ้ามี
ไม่มีก็ถามในแชท) ถามเป็นภาษาไทยครบ 4 เรื่องนี้:

1. **ธุรกิจ/แบรนด์** — ทำธุรกิจอะไร ชื่อแบรนด์อะไร กลุ่มลูกค้าคือใคร
   เป้าหมายของเว็บคืออะไร (ขายของ / เก็บ lead / โชว์ผลงาน)
2. **ธีม/อารมณ์** — เลือกจาก preset ใน `references/design-system.md` เช่น
   Dark Luxury, Clean Minimal, Neon Cyber, Soft Pastel, Earth Organic
3. **โทนสี** — สีหลักของแบรนด์ หรือให้เลือกจาก palette ที่จับคู่กับธีมไว้แล้ว
4. **ฟอนต์** — เว็บภาษาไทยต้องใช้ฟอนต์ที่รองรับไทย (Prompt, Kanit, Anuphan,
   IBM Plex Sans Thai, Noto Sans Thai) — ดูคู่ฟอนต์แนะนำใน design-system.md

ถามเพิ่มถ้ายังไม่ชัด: อยากได้ 3D แบบไหน (สินค้า/รูปทรงนามธรรม/อนุภาค),
มี section อะไรบ้าง, มีโลโก้หรือรูปให้ไหม

### ขั้นที่ 2 — เลือกสูตรจาก references

อ่านไฟล์อ้างอิงตามที่ต้องใช้ (อย่าอ่านทั้งหมดถ้าไม่จำเป็น):

| ไฟล์ | เปิดเมื่อ |
|---|---|
| `references/design-system.md` | เลือกธีม สี ฟอนต์ และ layout ทุกครั้ง |
| `references/threejs-recipes.md` | ตอนเขียนฉาก 3D hero — มีโค้ดสูตรสำเร็จ 5 แบบ |
| `references/gsap-motion.md` | ตอนใส่ scroll animation, text reveal, preloader |

### ขั้นที่ 3 — สร้างไฟล์เดียวจบ

สร้าง `index.html` ไฟล์เดียว (HTML + CSS + JS รวมกัน) โหลดไลบรารีจาก CDN:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/0.160.0/three.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js"></script>
```

โครงหน้าเว็บมาตรฐาน (ปรับตามธุรกิจ):

1. **Preloader** — โลโก้/เปอร์เซ็นต์ จางหายเมื่อโหลดเสร็จ
2. **Nav** — โปร่งใส ลอยบนสุด blur เมื่อ scroll
3. **Hero** — heading ยักษ์ + canvas 3D เต็มจอด้านหลังหรือครึ่งขวา + CTA
4. **Features/บริการ** — cards เลื่อนเข้าแบบ stagger
5. **Showcase/สินค้า** — horizontal scroll หรือ grid พร้อม hover 3D tilt
6. **Social proof** — ตัวเลข นับขึ้นเมื่อเห็น (count-up)
7. **CTA + Footer**

### ขั้นที่ 4 — เกณฑ์คุณภาพ (ตรวจก่อนส่งเสมอ)

- [ ] 3D render ได้จริง มี fallback ถ้า WebGL ใช้ไม่ได้ (แสดง gradient แทน)
- [ ] `prefers-reduced-motion` → ปิด animation ใหญ่
- [ ] Responsive มือถือ: heading ใช้ `clamp()`, canvas ลดความละเอียด (pixelRatio ≤ 2)
- [ ] ข้อความไทยไม่ตัดคำกลางประโยค (`word-break: keep-all` หรือเว้นวรรคให้ถูก)
- [ ] ทุก section มี animation ตอน scroll ถึง — ไม่มี section ที่ "ตายนิ่ง"
- [ ] เปิดไฟล์ทดสอบจริง (เบราว์เซอร์/screenshot) ก่อนบอกว่าเสร็จ

## ตัวอย่างการเริ่มบทสนทนา

ผู้ใช้: "อยากได้เว็บให้ร้านกาแฟ"
→ ตอบ: ถามชุดคำถามขั้นที่ 1 เป็นภาษาไทยทันที (ห้ามเดาเองแล้วสร้างเลย)

ผู้ใช้: "ทำเว็บแบบตัวอย่าง Dribbble นี้ให้แบรนด์เสื้อผ้า ธีมมืด สีม่วง ฟอนต์ Prompt"
→ ข้อมูลครบแล้ว ยืนยันสั้นๆ แล้วเริ่มขั้นที่ 2-3 ได้เลย
