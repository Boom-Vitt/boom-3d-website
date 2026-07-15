# Design System — ธีม สี ฟอนต์ สำหรับเว็บ 3D

เลือก 1 ธีมจากตารางนี้ตามคำตอบสัมภาษณ์ แล้วประกาศเป็น CSS variables ทุกครั้ง

## ธีม preset

### 1. Dark Luxury (หรู มืด พรีเมียม)
เหมาะกับ: แบรนด์แฟชั่น, อสังหา, รถ, จิวเวลรี่, เอเจนซี่
```css
:root{
  --bg:#0a0a0f; --surface:#14141c; --text:#f5f3ee; --muted:#8a8794;
  --accent:#c9a962; /* ทองแชมเปญ */ --accent-2:#6d5dfc;
}
```
3D ที่เข้ากัน: วัตถุโลหะ/แก้ว (metalness 0.9, roughness 0.1) + แสง rim สีทอง

### 2. Clean Minimal (ขาว สะอาด แบบ Etail/Dribbble)
เหมาะกับ: SaaS, สตาร์ทอัพ, ecommerce, คลินิก
```css
:root{
  --bg:#f7f6f3; --surface:#ffffff; --text:#111114; --muted:#6b6b70;
  --accent:#6d5dfc; /* ม่วงไล่ฟ้า */ --accent-2:#b48fff;
}
```
3D ที่เข้ากัน: วัตถุ pastel gradient ลอยนุ่มๆ เงา soft shadow

### 3. Neon Cyber (มืด + เรืองแสง)
เหมาะกับ: เกม, เทค, AI, คริปโต, อีสปอร์ต
```css
:root{
  --bg:#05060e; --surface:#0d1020; --text:#eaf6ff; --muted:#7d89a3;
  --accent:#00e5ff; --accent-2:#ff2d78;
}
```
3D ที่เข้ากัน: wireframe, particles, bloom effect, เส้น grid เรืองแสง

### 4. Soft Pastel (สดใส เป็นมิตร)
เหมาะกับ: คาเฟ่, ขนม, เด็ก, ความงาม, ไลฟ์สไตล์
```css
:root{
  --bg:#fff8f2; --surface:#ffffff; --text:#3a2e2a; --muted:#9c8d86;
  --accent:#ff8fab; --accent-2:#ffd166;
}
```
3D ที่เข้ากัน: รูปทรงกลมมน matte สีพาสเทล เด้งนุ่มๆ

### 5. Earth Organic (ธรรมชาติ อบอุ่น)
เหมาะกับ: ร้านกาแฟ, ออร์แกนิก, สปา, งานคราฟต์
```css
:root{
  --bg:#f2ede4; --surface:#faf7f0; --text:#2d2a24; --muted:#87816f;
  --accent:#4a6741; --accent-2:#c07b4d;
}
```
3D ที่เข้ากัน: รูปทรงหิน/ใบไม้ นามธรรม texture ด้าน หมุนช้าๆ

## คู่ฟอนต์ (รองรับภาษาไทยทั้งหมด — โหลดจาก Google Fonts)

| คู่ | Heading | Body | อารมณ์ |
|---|---|---|---|
| A | Prompt (700/800) | Prompt (300/400) | โมเดิร์น สากล — ปลอดภัยสุด |
| B | Kanit (600/700) | Sarabun (400) | เทค กีฬา พลังเยอะ |
| C | Anuphan (600) | Anuphan (300) | มินิมอล หรู นุ่ม |
| D | IBM Plex Sans Thai (700) | IBM Plex Sans Thai (400) | องค์กร น่าเชื่อถือ |
| E | Chonburi (400, display) | Noto Sans Thai (400) | คาแรกเตอร์จัด งานคราฟต์ |

ตัวอย่างโหลด:
```html
<link href="https://fonts.googleapis.com/css2?family=Prompt:wght@300;400;700;800&display=swap" rel="stylesheet">
```

## กติกา Typography

- Hero heading: `font-size: clamp(2.8rem, 8vw, 7rem); line-height: 1.05;`
- ภาษาไทยต้องเพิ่ม line-height เป็น ~1.2-1.3 สำหรับ heading (สระบน/ล่างโดนตัด)
- ความกว้างข้อความ body ไม่เกิน `65ch`
- ใช้ตัวหนา 2 น้ำหนักพอ อย่าใส่ทุกน้ำหนัก

## Layout & Spacing

- Container: `max-width: 1200px` padding ข้าง `clamp(1.25rem, 5vw, 4rem)`
- ระยะห่างระหว่าง section: `clamp(5rem, 12vh, 10rem)` — ที่ว่างคือความหรู
- Border-radius สม่ำเสมอทั้งเว็บ: เลือกค่าเดียว (เช่น 20px) ใช้ทุก card/ปุ่ม
