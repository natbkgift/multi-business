# 7-Day Pilot Plan — AI Content Engine V1

> แผนทดลองใช้งาน AI Content Engine V1 ใน 7 วันแรก
>
> **เป้าหมาย:** 5 Short-form videos, 2 Static posts, 1 Long-form script draft, 1 Weekly review report
>
> **ข้อสำคัญ:** ทุกอย่างทำด้วยมือ ไม่มี Automation ใดๆ

---

## ภาพรวม 7 วัน

| วัน | หัวข้อหลัก | เวลาโดยประมาณ | Output |
|-----|-----------|-------------|--------|
| Day 1 | Setup | 3-4 ชั่วโมง | Google Sheets พร้อมใช้ |
| Day 2 | Generate Ideas | 2-3 ชั่วโมง | 20+ Ideas ใน Idea Bank |
| Day 3 | Approve + Write Scripts | 4-5 ชั่วโมง | 5 Approved Ideas, 3 Scripts |
| Day 4 | Creative Briefs | 3-4 ชั่วโมง | 3 Creative Briefs |
| Day 5 | Production | 6-8 ชั่วโมง | 5 Content ชิ้น |
| Day 6 | Publish | 2-3 ชั่วโมง | 7 Content Published |
| Day 7 | Review | 3-4 ชั่วโมง | 1 Weekly Review Report |

---

## Day 1 — Setup (วันที่ 1)

**เป้าหมาย:** ทุกอย่างพร้อมก่อนเริ่มผลิต Content

**เวลาโดยประมาณ:** 3-4 ชั่วโมง

**ผู้รับผิดชอบ:** Owner + Content Strategist

**เครื่องมือที่ใช้:** Google Sheets, Google Drive

### Checklist รายวัน

**ช่วงเช้า (09:00-11:00) — Google Sheets Setup**
- [ ] สร้าง Google Sheets ใหม่ ตั้งชื่อ `AI Content Engine V1`
- [ ] สร้าง 7 Sheet tabs ตามชื่อใน `google-sheets-structure.md`
- [ ] กรอก Headers ทุกคอลัมน์ใน Content Pipeline Sheet
- [ ] กรอก Headers ทุกคอลัมน์ใน Idea Bank Sheet
- [ ] กรอก Headers ทุกคอลัมน์ใน Script Queue Sheet
- [ ] Import `content-pipeline-template.csv` (ตัวอย่าง 5 แถว)
- [ ] Import `content-calendar-template.csv` (ตัวอย่าง 7 แถว)

**ช่วงบ่าย (13:00-15:00) — Google Drive + Team Setup**
- [ ] สร้าง Google Drive Folder Structure:
  ```
  AI Content Engine V1/
  ├── Assets/
  │   ├── Harmony Sauna/
  │   ├── AMP Pattaya/
  │   ├── The World Freight/
  │   ├── ธรรมะดีดี/
  │   └── Affiliate AI Automation/
  ├── Scripts/
  ├── Creative Briefs/
  └── Ready for Review/
  ```
- [ ] Share Google Sheets กับทุกคนในทีม (กำหนด Permission ที่เหมาะสม)
- [ ] Share Google Drive Folder กับ Designer/Editor
- [ ] Assign บทบาทตาม `team-checklist.md`
- [ ] แจก `brand-playbooks/` ให้ทุกคนอ่านล่วงหน้า

**ช่วงบ่าย (15:00-17:00) — ยืนยัน Brand Info**
- [ ] Owner ยืนยัน Brand Objectives ทุกแบรนด์
- [ ] ยืนยัน CTAs ที่ถูกต้อง (LINE ID, เบอร์โทร, URL)
- [ ] รวบรวม Brand Assets: โลโก้, รูป, สี Brand ของแต่ละแบรนด์
- [ ] ยืนยันสถานะ The World Freight = PARKED (ยังไม่ Publish)

**Output ของ Day 1:**
- ✅ Google Sheets พร้อมใช้งาน
- ✅ Google Drive มีโครงสร้างครบ
- ✅ ทีมรู้บทบาทของตัวเอง

---

## Day 2 — Generate Ideas (วันที่ 2)

**เป้าหมาย:** สร้าง Ideas อย่างน้อย 20 ชิ้น สำหรับ 2-3 แบรนด์

**เวลาโดยประมาณ:** 2-3 ชั่วโมง

**ผู้รับผิดชอบ:** Content Strategist

**เครื่องมือที่ใช้:** ChatGPT หรือ Claude, Google Sheets

### Checklist รายวัน

**ช่วงเช้า (09:00-10:30) — Generate Ideas แบรนด์ที่ 1 (Harmony Sauna)**
- [ ] เปิด `prompts/01-idea-generator.md`
- [ ] Copy Prompt Template
- [ ] กรอก Input: Brand = Harmony Sauna, Topic = [เลือก 1 Topic จาก Playbook], Platform = TikTok
- [ ] Paste ใน ChatGPT/Claude → รับ 10 Ideas
- [ ] ตรวจทุก Idea ว่ามี Hook + Risk Note + Score
- [ ] คัดเลือก Ideas ที่ Score ≥ 6
- [ ] กรอกลง Idea Bank Sheet (อย่างน้อย 10 Ideas)

**ช่วงเช้า (10:30-12:00) — Generate Ideas แบรนด์ที่ 2 (ธรรมะดีดี หรือ Affiliate)**
- [ ] ทำซ้ำกระบวนการเดียวกันสำหรับแบรนด์ที่ 2
- [ ] กรอก Ideas เพิ่มอีก 10 ชิ้นใน Idea Bank

**ช่วงบ่าย (13:00-14:30) — ตรวจและ Score Ideas**
- [ ] ตรวจ Ideas ทั้งหมดที่กรอก
- [ ] ให้ Score สุดท้าย (1-10) สำหรับแต่ละ Idea
- [ ] Mark Top 5 Ideas ที่จะนำเสนอ Owner (Status = "Selected")
- [ ] เตรียม Summary 1 หน้าสำหรับ Owner Review

**Output ของ Day 2:**
- ✅ 20+ Ideas ใน Idea Bank
- ✅ 5 Ideas ที่ "Selected" รอ Owner Approve

---

## Day 3 — Approve Ideas + Write Scripts (วันที่ 3)

**เป้าหมาย:** Approve 5 Ideas, เขียน Script 3 ชิ้น

**เวลาโดยประมาณ:** 4-5 ชั่วโมง

**ผู้รับผิดชอบ:** Owner (Approve) + Content Strategist + Script Writer

**เครื่องมือที่ใช้:** Google Sheets, ChatGPT/Claude, `brand-playbooks/`

### Checklist รายวัน

**ช่วงเช้า (09:00-10:00) — Owner Approves Ideas**
- [ ] Owner อ่าน 5 Selected Ideas
- [ ] ผ่าน Script Production Gate (ดู `approval-gates.md` Gate 1)
- [ ] Approve หรือ Reject แต่ละ Idea พร้อมเหตุผล
- [ ] อัปเดต Status เป็น "Approved" ใน Idea Bank
- [ ] Assign Script Writer สำหรับแต่ละ Idea ที่ Approved

**ช่วงเช้า (10:00-12:00) — สร้าง Scripts ชิ้นที่ 1-2**
- [ ] เปิด `prompts/02-script-writer.md`
- [ ] Copy Prompt + กรอก Input สำหรับ Idea ที่ 1
- [ ] Generate Script Draft ด้วย AI
- [ ] ตรวจ Compliance Checklist ท้าย Script
- [ ] กรอก Script ลง Script Queue Sheet (สร้าง Script ID)
- [ ] ทำซ้ำสำหรับ Idea ที่ 2

**ช่วงบ่าย (13:00-15:00) — Script ชิ้นที่ 3 + Assign Reviewer**
- [ ] สร้าง Script ที่ 3
- [ ] อัปเดต Script Status = "Draft Ready"
- [ ] Assign Script Reviewer ใน Script Queue
- [ ] แจ้ง Script Reviewer ว่ามี 3 Scripts รอ Review

**ช่วงบ่าย (15:00-17:00) — Script Review**
- [ ] Script Reviewer ตรวจ Script ทั้ง 3 ชิ้น
- [ ] ใช้ Compliance Checklist ใน `team-checklist.md`
- [ ] Approve หรือ Reject + Revision Notes
- [ ] อัปเดต Script Queue

**Output ของ Day 3:**
- ✅ 5 Approved Ideas
- ✅ 3 Script Drafts ใน Queue
- ✅ อย่างน้อย 2 Scripts ผ่าน Review

---

## Day 4 — Create Creative Briefs (วันที่ 4)

**เป้าหมาย:** สร้าง Creative Brief 3 ชิ้นจาก Scripts ที่อนุมัติ

**เวลาโดยประมาณ:** 3-4 ชั่วโมง

**ผู้รับผิดชอบ:** Content Strategist

**เครื่องมือที่ใช้:** ChatGPT/Claude, Google Sheets, Google Drive

### Checklist รายวัน

**ช่วงเช้า (09:00-11:00) — สร้าง Brief ชิ้นที่ 1-2**
- [ ] เปิด `prompts/04-creative-brief-generator.md`
- [ ] Copy Prompt + Paste Script ที่ Approved
- [ ] Generate Creative Brief ด้วย AI
- [ ] ตรวจ Brief ครบ: Shot List, Visual Direction, Text Overlay, Thumbnail, Assets
- [ ] กรอก Brief ลง Creative Briefs Sheet (สร้าง Brief ID)
- [ ] รวบรวม Assets ที่ต้องใช้ Upload ขึ้น Google Drive
- [ ] ทำซ้ำสำหรับ Brief ที่ 2

**ช่วงบ่าย (13:00-15:00) — Brief ชิ้นที่ 3 + ส่งให้ Editor**
- [ ] สร้าง Brief ที่ 3
- [ ] Assign Designer/Editor + แจ้ง Deadline
- [ ] ส่ง Brief พร้อม Assets ผ่าน Google Drive
- [ ] อัปเดต Brief Status = "Sent to Editor"
- [ ] ยืนยัน Export Spec กับ Editor

**Output ของ Day 4:**
- ✅ 3 Creative Briefs พร้อมใช้
- ✅ Assets ใน Google Drive ครบ
- ✅ Editor รับงานแล้ว

---

## Day 5 — Produce / Prepare Assets (วันที่ 5)

**เป้าหมาย:** Editor/Designer ผลิต Content 5 ชิ้น

**เวลาโดยประมาณ:** 6-8 ชั่วโมง (สำหรับ Editor)

**ผู้รับผิดชอบ:** Designer / Video Editor

**เครื่องมือที่ใช้:** Video editing software, Canva, Design tools

### Checklist รายวัน

**ช่วงเช้า (09:00-12:00) — ผลิต Short-form Videos (3 ชิ้น)**
- [ ] เปิด Creative Brief ชิ้นที่ 1
- [ ] ตรวจ Assets ครบในโฟลเดอร์
- [ ] ผลิตวิดีโอตาม Shot List
- [ ] ใส่ Text Overlay ตาม Brief
- [ ] ตรวจ Audio Level
- [ ] Export ตาม Spec + ตั้งชื่อไฟล์ถูกต้อง
- [ ] ทำซ้ำสำหรับชิ้นที่ 2 และ 3

**ช่วงบ่าย (13:00-16:00) — Static Posts (2 ชิ้น)**
- [ ] ผลิต Static Post ใน Canva ตาม Brief
- [ ] ตรวจ Visual Brand Guidelines
- [ ] Export ความละเอียดสูง
- [ ] ตั้งชื่อไฟล์ถูกต้อง
- [ ] Upload ทั้งหมดขึ้น "Ready for Review" folder

**ช่วงบ่าย (16:00-17:00) — แจ้งและส่ง Review**
- [ ] แจ้ง Strategist ว่างานเสร็จทั้ง 5 ชิ้น
- [ ] อัปเดต Brief Status = "Review Ready"
- [ ] Strategist + Owner ทำ Final Review ด่วนสำหรับ Day 5

**Output ของ Day 5:**
- ✅ 3 Short-form Videos พร้อม
- ✅ 2 Static Posts พร้อม
- ✅ ไฟล์ทั้งหมดใน Google Drive

---

## Day 6 — Publish Manually (วันที่ 6)

**เป้าหมาย:** Publish 5 Short-form, 2 Static Posts

**เวลาโดยประมาณ:** 2-3 ชั่วโมง

**ผู้รับผิดชอบ:** Strategist (Final Review) + Social Media Scheduler (Publish)

**เครื่องมือที่ใช้:** TikTok, Facebook, Instagram, YouTube — App และ Web

### Checklist รายวัน

**ช่วงเช้า (09:00-10:00) — Final Review**
- [ ] Strategist ดู Video/Image ทุกชิ้นจริงๆ บน Mobile
- [ ] ผ่าน Public Posting Gate (Gate 2 ใน `approval-gates.md`)
- [ ] Owner Sign off ชิ้นที่ต้องการ Owner Approval
- [ ] อัปเดต Status = "Scheduled" ใน Publishing Calendar

**ช่วงเช้า (10:00-12:00) — สร้าง Captions**
- [ ] ใช้ Prompt 03 สร้าง Caption สำหรับทุก Content
- [ ] กรอก Caption ใน Publishing Calendar
- [ ] ตรวจ CTA, Hashtags, Links ทุกชิ้น

**ช่วงบ่าย (14:00-17:00) — Publish (สลับเวลาตาม Platform Best Practice)**
- [ ] Publish TikTok ชิ้นที่ 1 (แนะนำ: 17:00-19:00)
- [ ] Publish Facebook Video ชิ้นที่ 1
- [ ] Publish Instagram Reel/Post
- [ ] บันทึก Published URL ทุกชิ้นใน Calendar
- [ ] เปลี่ยน Status = "Published"
- [ ] แจ้งทีมใน Group Chat

**หลัง Publish (เย็น)**
- [ ] ติดตาม Comment ของทุก Post
- [ ] Reply Comment คำถามภายใน 2 ชั่วโมง

**Output ของ Day 6:**
- ✅ 5 Short-form Videos Published
- ✅ 2 Static Posts Published
- ✅ Published URLs บันทึกไว้ครบ

---

## Day 7 — Track Metrics + Review (วันที่ 7)

**เป้าหมาย:** กรอก KPI, วิเคราะห์, วางแผนสัปดาห์หน้า

**เวลาโดยประมาณ:** 3-4 ชั่วโมง

**ผู้รับผิดชอบ:** Performance Reviewer + Content Strategist + Owner

**เครื่องมือที่ใช้:** Platform Analytics, Google Sheets, ChatGPT/Claude

### Checklist รายวัน

**ช่วงเช้า (09:00-11:00) — กรอก KPI**
- [ ] เข้า Platform Analytics ของทุก Account
- [ ] กรอก KPI ทุกชิ้นใน KPI Tracker Sheet
  - Views, 3s Retention, Avg Watch Time, Completion Rate
  - Likes, Comments, Shares, Saves
  - Link Clicks, DMs, Leads
  - Conversion Signal
- [ ] คำนวณ Content Score ตามสูตรใน `kpi-tracking.md`
- [ ] อัปเดต Repurpose Decision เบื้องต้น

**ช่วงเช้า (11:00-12:00) — Weekly Review ด้วย AI**
- [ ] Export KPI Data จาก Sheet
- [ ] ใช้ Prompt 05 วิเคราะห์ผลสำหรับแต่ละแบรนด์
- [ ] Copy ผลลัพธ์ลง Weekly Review Sheet

**ช่วงบ่าย (13:00-15:00) — วางแผนสัปดาห์หน้า**
- [ ] สรุป Top Performers และ Failed Content
- [ ] สรุป Patterns ที่พบ
- [ ] สร้าง Ideas สำหรับสัปดาห์หน้าจาก Patterns
- [ ] วางแผน Content Calendar สัปดาห์ที่ 2
- [ ] Assign งานสำหรับสัปดาห์หน้า

**ช่วงบ่าย (15:00-16:00) — Report ให้ Owner**
- [ ] สรุป 1-Page Report ให้ Owner
  - ผลรวมสัปดาห์แรก
  - Top Performer 1 ชิ้น
  - สิ่งที่เรียนรู้ได้ 3 อย่าง
  - แผนสัปดาห์หน้า
- [ ] ส่ง Report ให้ Owner

**Output ของ Day 7:**
- ✅ KPI ครบทุกชิ้น
- ✅ Content Score คำนวณแล้ว
- ✅ Weekly Review Report
- ✅ แผนสัปดาห์หน้าพร้อม

---

## เป้าหมายรวม 7 วัน

| Output | เป้าหมาย | ผู้รับผิดชอบ |
|--------|---------|-----------|
| Short-form Videos Published | 5 ชิ้น | Editor + Scheduler |
| Static Posts Published | 2 ชิ้น | Designer + Scheduler |
| Long-form Script Draft | 1 ชิ้น | Writer (สำหรับ YouTube/Long-form) |
| Weekly Review Report | 1 ฉบับ | Performance Reviewer |
| Ideas ใน Idea Bank | 20+ Ideas | Strategist |

---

## Tips สำหรับ Pilot Week

1. **อย่าสมบูรณ์แบบในสัปดาห์แรก** — เป้าหมายคือเรียนรู้ระบบ ไม่ใช่ผลิต Content ที่สมบูรณ์แบบ
2. **กรอก Google Sheets ทุกครั้ง** — ถ้าไม่กรอก ระบบทั้งหมดจะล้มเหลว
3. **ตรวจ Risk ก่อนเสมอ** — ดีกว่าแก้ปัญหาหลัง Publish
4. **เลือกแบรนด์ที่ง่ายที่สุดก่อน** — แนะนำ Harmony Sauna หรือ ธรรมะดีดี
5. **ทำ Weekly Review จริงๆ** — นี่คือขั้นตอนที่สำคัญที่สุดของระบบ

---

*อัปเดตล่าสุด: 2026-05-06 | AI Content Engine V1*
