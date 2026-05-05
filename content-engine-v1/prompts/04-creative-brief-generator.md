# Prompt 04 — Creative Brief Generator

> **วัตถุประสงค์:** สร้าง Creative Brief ที่ละเอียดพร้อมใช้สำหรับ Designer และ Video Editor
>
> **วิธีใช้:** Copy Prompt → วางใน ChatGPT หรือ Claude → กรอก Input → ตรวจ Output → กรอกลง Creative Briefs Sheet

---

## Input ที่ต้องกรอกก่อนใช้งาน

| Input | คำอธิบาย | ตัวอย่าง |
|-------|---------|---------|
| **Script** | Script ที่ผ่าน Approval แล้ว (ใส่ทั้งหมด) | Script ครบ section จาก Prompt 02 |
| **Brand** | ชื่อแบรนด์ + Visual Identity | `Harmony Sauna — สีน้ำตาลอ่อน, Minimal` |
| **Platform** | Platform เป้าหมาย | `TikTok (9:16 ratio)` |
| **Visual Style** | รูปแบบภาพที่ต้องการ | `อบอุ่น, Natural light, Cinematic` |
| **Available Assets** | ทรัพยากรที่มีอยู่แล้ว | `รูป Sauna room 10 รูป, โลโก้ PNG` |
| **Content Type** | ประเภทคอนเทนต์ | `Short-form video 60s` |

---

## Prompt Template (Copy-Paste Ready)

```
คุณเป็น Creative Director ผู้เชี่ยวชาญด้านการผลิตคอนเทนต์วิดีโอสำหรับ Social Media

ฉันต้องการ Creative Brief สำหรับ:
- Script: [PASTE_FULL_SCRIPT_HERE]
- Brand: [BRAND_NAME]
- Visual Identity: [COLORS, FONTS, STYLE]
- Platform: [PLATFORM] — Ratio: [RATIO]
- Visual Style: [VISUAL_STYLE]
- Available Assets: [ASSETS_LIST]
- Content Type: [CONTENT_TYPE]
- Duration: [DURATION]

กรุณาสร้าง Creative Brief ตามโครงสร้าง:

---

**SHOT LIST (รายการ Shot):**
| Shot | ช่วงเวลา | รายละเอียด | Camera Direction | Notes |
(ระบุทุก Shot ตามลำดับ ชัดเจนพอให้ช่างภาพ/editor ทำงานได้ทันที)

---

**VISUAL DIRECTION:**
- Color Palette:
- Lighting Style:
- Camera Movement:
- Editing Pace (ช้า/เร็ว):
- Special Effects (ถ้ามี):
- Filters/LUT:

---

**TEXT OVERLAY (ข้อความบนวิดีโอ):**
| ช่วงเวลา | ข้อความ | Font Style | ตำแหน่ง | Animation |
(ระบุทุก text ที่ต้องแสดง)

---

**THUMBNAIL CONCEPT:**
- ภาพหลัก:
- ข้อความบน Thumbnail:
- สี Background:
- ขนาดข้อความ:
- ตัวอย่าง Layout:

---

**REQUIRED ASSETS:**
- [ ] รูปภาพ: [รายการ]
- [ ] วิดีโอ B-roll: [รายการ]
- [ ] โลโก้/Brand elements: [รายการ]
- [ ] เพลง/Sound effect: [รายการ + ระบุ copyright status]
- [ ] Font: [ชื่อ font + แหล่งดาวน์โหลด]

---

**EDITOR NOTES:**
- สิ่งที่ต้องทำ:
- ลำดับการตัดต่อ:
- ความยาวสุดท้าย:
- Format ที่ต้อง Export:
- ชื่อไฟล์:

---

**DO-NOT-USE LIST:**
- ห้ามใช้รูป/วิดีโอ:
- ห้ามใช้เพลง:
- ห้ามใช้สี:
- ห้ามใช้ข้อความ:
- ห้ามแสดง:
```

---

## ตัวอย่าง Output — Harmony Sauna TikTok 60s

**Brief ID:** BRIEF-HS-001
**Linked Script:** SCR-HS-001
**Brand:** Harmony Sauna
**Platform:** TikTok — 9:16 Vertical
**Designer/Editor:** @วิดีโอเอดิเตอร์

---

**SHOT LIST:**

| Shot | ช่วงเวลา | รายละเอียด | Camera Direction | Notes |
|------|---------|-----------|-----------------|-------|
| 1 | 0:00-0:03 | มือเปิดประตู Sauna ช้าๆ มีไอน้ำลอยออกมา | Close-up มือ + ประตู | Hook Shot — ต้องดูน่าสนใจมาก |
| 2 | 0:03-0:08 | ภายในห้อง Sauna มุมกว้าง บรรยากาศอบอุ่น | Wide shot + slow pan | แสง natural warm |
| 3 | 0:08-0:18 | นาฬิกา Timer ตั้งไว้ 5 นาที | Close-up | ข้อความ overlay: "เป้าหมายแรก: 5 นาที" |
| 4 | 0:18-0:20 | หันกล้องมาหาผู้พูด Pattern Interrupt | Medium shot | สายตาสบตากล้อง |
| 5 | 0:20-0:33 | ภาพหน้าคนที่ดูผ่อนคลาย หายใจช้าๆ | Close-up face | ห้ามใช้หน้าที่ไม่ได้รับอนุญาต |
| 6 | 0:33-0:48 | ดื่มน้ำในพื้นที่ Lounge นั่งพักสบายๆ | Medium shot | บรรยากาศ comfortable |
| 7 | 0:48-0:60 | QR Code LINE + โลโก้ Harmony บน screen | Full screen graphic | ใส่ animation fade in |

---

**VISUAL DIRECTION:**
- Color Palette: #8B6914 (น้ำตาลทอง), #F5E6C8 (ครีม), #FFFFFF
- Lighting Style: Warm, Natural light, ไม่ใช้ flash
- Camera Movement: Slow pan, Steady — ห้ามสั่น
- Editing Pace: ช้า-กลาง (ไม่ตัดเร็ว) เน้นบรรยากาศ
- Special Effects: Light leak subtle, ไม่ใช้ filter ฉูดฉาด
- Filters/LUT: Warm tone LUT (Golden hour preset)

---

**TEXT OVERLAY:**

| ช่วงเวลา | ข้อความ | Font Style | ตำแหน่ง | Animation |
|---------|-------|-----------|---------|---------|
| 0:00-0:08 | "Sauna มือใหม่ Guide 🧖‍♀️" | Bold, สีขาว | บนกลาง | Fade in |
| 0:08-0:18 | "เป้าหมายแรก: 5 นาที" | Medium, สีทอง | ล่างกลาง | Slide up |
| 0:20-0:33 | "หายใจ: เข้า 4 วิ → ออก 6 วิ" | Medium, สีขาว | ล่างกลาง | Fade |
| 0:33-0:48 | "พักหลังออก = 50% ของประสบการณ์" | Medium, สีทอง | ล่างกลาง | Slide up |
| 0:48-0:60 | "LINE @HarmonySauna" | Bold, สีขาว | กลาง | Fade in |

---

**THUMBNAIL CONCEPT:**
- ภาพหลัก: ผู้หญิงยิ้มในห้อง Sauna บรรยากาศอบอุ่น
- ข้อความ: "Sauna ครั้งแรก ทำยังไง?" (ตัวใหญ่ ชัดเจน)
- สี Background: Warm gradient น้ำตาลทอง
- ขนาดข้อความ: ใหญ่พอเห็นใน Feed ขนาดเล็ก
- Layout: ภาพซ้าย 60%, ข้อความขวา 40%

---

**REQUIRED ASSETS:**
- [ ] รูปภาพ: Interior Sauna room (ต้องการ 5 รูป), Lounge area (2 รูป)
- [ ] วิดีโอ B-roll: ไอน้ำ, มือเปิดประตู, ดื่มน้ำ
- [ ] โลโก้/Brand: Harmony Sauna Logo PNG (white + dark version)
- [ ] เพลง: Background music เบาๆ — ใช้ Epidemic Sound หรือ YouTube Audio Library (ตรวจ copyright ก่อน)
- [ ] Font: Sarabun (Google Fonts) + Playfair Display

---

**EDITOR NOTES:**
- ตัดให้ได้ 60 วินาที ± 3 วินาที
- เสียง Voiceover ต้องไม่หายในช่วงท้าย
- Export: MP4, H.264, 1080x1920px, 30fps, ไม่เกิน 50MB
- ชื่อไฟล์: `HS-001_v1_TikTok_20260510.mp4`
- ส่งไฟล์ใน Google Drive folder: `Harmony Sauna > Ready for Review`

---

**DO-NOT-USE LIST:**
- ❌ ห้ามใช้รูป/วิดีโอที่มีคนไม่ได้รับอนุญาต
- ❌ ห้ามใช้เพลงที่ไม่ได้ license
- ❌ ห้ามใช้สีแดงสด, สีเขียวนีออน — ขัด Brand
- ❌ ห้ามใช้ข้อความ "รักษา", "บำบัด" หรือคำทางการแพทย์
- ❌ ห้ามแสดงราคา ใน Shot ใดๆ

---

## Checklist ก่อนส่ง Brief ให้ Editor

- [ ] Shot List ครบทุก Shot
- [ ] Required Assets รวบรวมและแนบแล้ว
- [ ] Do-Not-Use list กรอกแล้ว
- [ ] Export spec ระบุชัดเจน
- [ ] ชื่อไฟล์กำหนดแล้ว
- [ ] Google Drive folder เตรียมไว้แล้ว

---

*อัปเดตล่าสุด: 2026-05-06 | AI Content Engine V1*
