# boom-3d-website — Claude Skill (ภาษาไทย)

Skill สำหรับ Claude ที่ช่วยสร้างเว็บไซต์ landing page ระดับ Dribbble พร้อม 3D animation
(Three.js + GSAP + Lenis) โดยเริ่มจากสัมภาษณ์ผู้ใช้เป็นภาษาไทย — ถามธุรกิจ ธีม โทนสี ฟอนต์
ก่อนลงมือสร้างเสมอ

อ้างอิงสไตล์: [Etail landing page — 3D animation (Dribbble)](https://dribbble.com/shots/25571331-Etail-landing-page-web-design-3D-animation)

## โครงสร้าง

- `SKILL.md` — ตัว skill หลัก (workflow สัมภาษณ์ → สร้าง → ตรวจคุณภาพ)
- `references/design-system.md` — 5 ธีม preset + คู่ฟอนต์ไทย + กติกา typography
- `references/threejs-recipes.md` — สูตรฉาก 3D hero 5 แบบ
- `references/gsap-motion.md` — จังหวะ animation, text reveal, scroll trigger
- `demo/index.html` — เว็บตัวอย่างที่สร้างจาก skill นี้ (ธีม Neon Cyber, BoomBigNose)
- `boom-3d-website.skill` — ไฟล์แพ็กเกจ พร้อมติดตั้งใน Claude

## วิธีติดตั้ง

ดาวน์โหลด `boom-3d-website.skill` แล้วเพิ่มใน Claude (Settings → Capabilities → Skills)
หรือคัดลอกทั้งโฟลเดอร์ไปไว้ในโฟลเดอร์ skills ของคุณ

## วิธีใช้

บอก Claude ว่า "อยากได้เว็บสวยๆ มี 3D ให้ธุรกิจ..." แล้ว skill จะถามธีม สี ฟอนต์
เป็นภาษาไทย ก่อนสร้าง `index.html` ไฟล์เดียวจบ เปิดได้ทันที

---
สร้างโดย BoomBigNose 🎈
