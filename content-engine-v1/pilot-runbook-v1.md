# Pilot Runbook V1 — AI Content Engine V1

> **เอกสารนี้คือ Runbook จริงสำหรับ Pilot 7 วันแรก หลังจาก AI Content Engine V1 ได้รับการอนุมัติ**
>
> **Pilot Brands:** Harmony Sauna และ ธรรมะดีดี
>
> **ข้อสำคัญ:** ทุกอย่างทำด้วยมือ — ไม่มี Automation, ไม่มี Auto-Publish, ไม่มี API Integration

---

## 1. Pilot Objective (วัตถุประสงค์ของ Pilot)

Pilot นี้มีวัตถุประสงค์เพื่อ:

1. **ทดสอบระบบ AI Content Engine V1 ในสภาพแวดล้อมจริง** — ตรวจสอบว่า Google Sheets, Approval Gate, และ Workflow ใช้งานได้จริงหรือไม่
2. **ผลิต Content จริงที่ Publish ได้** — ไม่ใช่แค่ Draft ในระบบ แต่ต้อง Publish จริงบน Platform จริง
3. **เก็บ KPI จริง** — ไม่สามารถอ้างว่า Pilot สำเร็จได้ หากยังไม่มีตัวเลข KPI จาก Platform Analytics จริง
4. **ค้นหา Bottleneck** — ขั้นตอนไหนที่ใช้เวลานานเกิน? ขั้นตอนไหนที่สับสน? บันทึกไว้เพื่อปรับปรุงก่อน V2
5. **ตัดสินใจ Go/No-Go สำหรับ V2 Automation** — ข้อมูลจาก Pilot นี้จะเป็นพื้นฐานในการตัดสินใจ

### เงื่อนไขสำเร็จ (Definition of Success)

| เงื่อนไข | เกณฑ์ขั้นต่ำ |
|---------|-----------|
| Content Published | อย่างน้อย **5 ชิ้น** Publish จริงบน Platform |
| KPI บันทึกครบ | ทุก Published Content มีข้อมูล KPI ใน Tracker |
| Approval Gate ผ่าน | ทุก Content ผ่าน Gate 1 และ Gate 2 ก่อน Publish |
| Weekly Review เสร็จ | มี Weekly Review Report 1 ฉบับพร้อม Insights |

---

## 2. Pilot Scope (ขอบเขตของ Pilot)

### แบรนด์ที่เข้าร่วม Pilot

| แบรนด์ | Platform | Content Type | เป้าหมาย Content |
|--------|---------|-------------|----------------|
| **Harmony Sauna** | TikTok (หลัก), Facebook | Short-form Video 60s, Static Post | 3 ชิ้น |
| **ธรรมะดีดี** | YouTube Shorts (หลัก), Facebook | YouTube Shorts, Static Post | 2 ชิ้น |

### แบรนด์ที่ไม่รวมอยู่ใน Pilot นี้

| แบรนด์ | เหตุผล |
|--------|--------|
| AMP Pattaya | รอรอบถัดไปหลัง Pilot สำเร็จ |
| The World Freight | สถานะ PARKED — ห้าม Publish |
| Affiliate/AI Automation | รอรอบถัดไปหลัง Pilot สำเร็จ |

### ขอบเขตที่ชัดเจน (Hard Scope)

| สิ่งที่ทำ | สิ่งที่ไม่ทำ |
|---------|-----------|
| ✅ ผลิต Content ด้วยมือผ่าน AI Prompts | ❌ ไม่มี Make.com / n8n / Zapier |
| ✅ Publish ด้วยมือบน Platform | ❌ ไม่มี Auto-Publish ใดๆ |
| ✅ กรอก KPI ด้วยมือจาก Analytics | ❌ ไม่มี API Integration |
| ✅ Review ผ่าน Google Sheets | ❌ ไม่แก้ไขไฟล์ในโฟลเดอร์ freight/ |
| ✅ Approve ผ่าน Approval Gates | ❌ ไม่อ้างว่าสำเร็จก่อนมี KPI จริง |

---

## 3. Day 0 Setup (การเตรียมความพร้อมก่อนเริ่ม)

> Day 0 คือวันเตรียมความพร้อมทั้งหมด ต้องเสร็จก่อน Day 1 เริ่ม

### 3.1 ตั้งค่า Google Sheets

**ขั้นตอนที่ 1 — สร้าง Google Sheets หลัก**
1. เปิด Google Sheets ใหม่ ตั้งชื่อว่า `AI Content Engine V1 — Pilot`
2. สร้าง 7 Sheet Tabs ตามนี้:
   - `Content Pipeline`
   - `Idea Bank`
   - `Script Queue`
   - `Creative Briefs`
   - `Publishing Calendar`
   - `KPI Tracker`
   - `Weekly Review`
3. Freeze Row 1 ทุก Sheet
4. ตั้งค่า Conditional Formatting:
   - Status = `Published` → แถวสีเขียวอ่อน (#d9ead3)
   - Status = `Stop` → แถวสีแดงอ่อน (#fce8e6)
   - Priority = `High` → ตัวหนา

**ขั้นตอนที่ 2 — กรอก Headers (Content Pipeline)**

```
Content ID | Brand | Platform | Topic | Hook | Status | Script Writer | Script Reviewer | Creative Brief Owner | Scheduled Date | Published Date | Priority | Notes
```

**ขั้นตอนที่ 3 — กรอก Headers (Idea Bank)**

```
Idea ID | Brand | Platform | Idea Title | Hook | Angle Type | Target Audience | Suggested Format | Goal | Source | CTA | Risk Note | Score | Status | Date Added | Added By
```

**ขั้นตอนที่ 4 — กรอก Headers (KPI Tracker)**

```
Content ID | Brand | Platform | Publish Date | Views | 3s Retention | Watch Time (avg) | Completion Rate | Likes | Comments | Shares | Saves | Link Clicks | DM/Inquiries | Leads | Conversion Signal | Content Score | Repurpose Decision
```

**ขั้นตอนที่ 5 — ตั้งค่า Dropdown Values**

| Sheet | คอลัมน์ | ค่า Dropdown |
|-------|---------|------------|
| ทุก Sheet | Brand | `Harmony Sauna`, `ธรรมะดีดี` |
| ทุก Sheet | Platform | `TikTok`, `Facebook`, `YouTube`, `YouTube Shorts`, `Instagram` |
| Idea Bank | Status | `New`, `Selected`, `Approved`, `Rejected`, `On Hold` |
| Script Queue | Script Status | `Not Started`, `Writing`, `Draft Ready`, `In Review`, `Revision Required`, `Approved`, `Rejected` |
| KPI Tracker | Conversion Signal | `None`, `Weak`, `Moderate`, `Strong`, `Very Strong` |
| KPI Tracker | Repurpose Decision | `Repurpose + Scale`, `Repeat`, `Improve`, `Review`, `Stop` |

### 3.2 ตั้งค่า Google Drive

สร้าง Folder Structure ใน Google Drive:

```
AI Content Engine V1 — Pilot/
├── Assets/
│   ├── Harmony Sauna/
│   │   ├── Logos/
│   │   ├── Photos/
│   │   └── Brand Colors/
│   └── ธรรมะดีดี/
│       ├── Logos/
│       ├── Photos/
│       └── Brand Colors/
├── Scripts/
│   ├── Harmony Sauna/
│   └── ธรรมะดีดี/
├── Creative Briefs/
│   ├── Harmony Sauna/
│   └── ธรรมะดีดี/
└── Ready for Review/
    ├── Harmony Sauna/
    └── ธรรมะดีดี/
```

### 3.3 รวบรวม Brand Assets

**Harmony Sauna:**
- [ ] โลโก้ (PNG บนพื้นโปร่งใส ขนาด ≥ 500px)
- [ ] รูปห้อง Sauna อย่างน้อย 5 รูป (ความละเอียดสูง)
- [ ] สี Brand (Hex code): น้ำตาลอ่อน, ครีม, ขาว
- [ ] LINE ID / เบอร์โทรสำหรับ CTA
- [ ] URL เว็บไซต์หรือ Booking Link

**ธรรมะดีดี:**
- [ ] โลโก้ (PNG บนพื้นโปร่งใส)
- [ ] รูปประกอบธรรมะ / พระพุทธรูป / ธรรมชาติ อย่างน้อย 5 รูป
- [ ] สี Brand: เหลืองทอง, ส้มอ่อน, ขาว
- [ ] ช่อง YouTube / Facebook Page Link
- [ ] เพลง Background ที่ได้รับอนุญาต (Royalty-free)

### 3.4 Assign บทบาทและแจ้งทีม

| บทบาท | ผู้รับผิดชอบ | งานใน Pilot |
|------|-----------|-----------|
| **Owner** | [ชื่อเจ้าของ] | Approve Ideas, Approve Pre-Publish, Final Review |
| **Content Strategist** | [ชื่อ] | Generate Ideas, ดูแล Workflow, สร้าง Captions |
| **Script Writer** | [ชื่อ] | เขียน Script Draft ด้วย AI |
| **Script Reviewer** | [ชื่อ] | ตรวจ Script Compliance |
| **Designer/Editor** | [ชื่อ] | ผลิต Video/Static Post |
| **Social Scheduler** | [ชื่อ] | Publish บน Platform + บันทึก URL |

> ⚠️ **หมายเหตุ:** บุคคลหนึ่งคนสามารถรับหลายบทบาทได้ในทีมเล็ก แต่ Owner และ Script Reviewer ต้องแยกกัน

### 3.5 Checklist Day 0

- [ ] Google Sheets สร้างครบ 7 Sheets + Headers + Dropdowns
- [ ] Google Drive Folder Structure พร้อม
- [ ] Brand Assets รวบรวมครบทั้ง 2 แบรนด์
- [ ] ทีมทุกคนได้รับ Permission เข้า Google Sheets + Google Drive
- [ ] ทุกคนอ่าน Brand Playbook ของแบรนด์ที่ตัวเองรับผิดชอบ
- [ ] Owner ยืนยัน CTA (LINE ID, เบอร์โทร, URL) ของทั้ง 2 แบรนด์
- [ ] ยืนยันสถานะ: The World Freight = PARKED (ไม่ใช้ใน Pilot นี้)

---

## 4. Day-by-Day Runbook (แผนปฏิบัติงานรายวัน)

### Day 1 — Idea Generation & Approval

**เป้าหมาย:** สร้าง Ideas 10 ชิ้น (5 Harmony Sauna + 5 ธรรมะดีดี) และให้ Owner Approve

**ผู้รับผิดชอบหลัก:** Content Strategist + Owner

---

**ช่วงเช้า 09:00–11:00 — Generate Ideas: Harmony Sauna**

1. เปิด `prompts/01-idea-generator.md`
2. Copy Prompt Template
3. กรอก Input:
   - Brand = Harmony Sauna
   - Platform = TikTok
   - Topic = [เลือกจาก Content Pillars ใน brand-playbooks/harmony-sauna.md]
   - Format = Short-form video 60s
4. Paste ใน ChatGPT หรือ Claude → รับ Ideas
5. คัดเลือก 5 Ideas ที่ดีที่สุด (Score ≥ 6/10)
6. กรอกใน **Idea Bank Sheet**:
   - กรอก Idea ID: `IDEA-HS-001` ถึง `IDEA-HS-005`
   - กรอกทุก Field ให้ครบ (อย่าเว้น Risk Note)
   - ตั้ง Status = `New`

**ช่วงเช้า 11:00–12:00 — Generate Ideas: ธรรมะดีดี**

1. ทำซ้ำกระบวนการเดียวกันสำหรับ ธรรมะดีดี
2. Platform = YouTube Shorts
3. กรอก Idea ID: `IDEA-DD-001` ถึง `IDEA-DD-005`
4. ตั้ง Status = `New`

**ช่วงบ่าย 13:00–14:00 — Score และ Select Ideas**

1. Strategist ตรวจ Ideas ทั้ง 10 ชิ้น
2. ให้ Score สุดท้าย (1-10)
3. เปลี่ยน Status ของ Ideas ที่ดีที่สุดจากแต่ละแบรนด์เป็น `Selected`
4. เตรียม Summary 1 หน้าสำหรับ Owner (ชื่อ Idea + Hook + Score + Risk Note)

**ช่วงบ่าย 14:00–16:00 — Owner Reviews + Approves**

1. Owner อ่าน Ideas ที่ `Selected`
2. ตรวจตาม **Gate 1: Script Production Gate** (ดู `approval-gates.md`)
3. Approve หรือ Reject แต่ละ Idea พร้อมเหตุผล
4. อัปเดต Status = `Approved` หรือ `Rejected` + บันทึกชื่อ Owner + วันที่

**Output Day 1:**
- ✅ Idea Bank มี 10 Ideas
- ✅ อย่างน้อย 5 Ideas ที่ `Approved`

---

### Day 2 — Script Writing

**เป้าหมาย:** เขียน Script Draft 3 ชิ้น (2 Harmony Sauna + 1 ธรรมะดีดี)

**ผู้รับผิดชอบหลัก:** Script Writer + Script Reviewer

---

**ช่วงเช้า 09:00–10:30 — Script ชิ้นที่ 1 (Harmony Sauna)**

1. เปิด `prompts/02-script-writer.md`
2. Copy Prompt + กรอก Idea ที่ Approved ของ Harmony Sauna
3. Generate Script Draft ด้วย AI
4. Writer ตรวจ:
   - Hook แรงพอไหม?
   - Claim ใดๆ ที่ต้องระวัง?
   - CTA ชัดเจนไหม?
5. กรอก Script ลง **Script Queue Sheet**:
   - Script ID: `SCR-HS-001`
   - Status = `Draft Ready`
   - Assign Reviewer

**ช่วงเช้า 10:30–12:00 — Script ชิ้นที่ 2 (Harmony Sauna)**

1. ทำซ้ำสำหรับ Idea ที่ 2 ของ Harmony Sauna
2. Script ID: `SCR-HS-002`

**ช่วงบ่าย 13:00–14:30 — Script ชิ้นที่ 3 (ธรรมะดีดี)**

1. ทำซ้ำสำหรับ Idea แรกของ ธรรมะดีดี
2. Script ID: `SCR-DD-001`

**ช่วงบ่าย 14:30–17:00 — Script Review**

1. Script Reviewer ตรวจ Scripts ทั้ง 3 ชิ้น
2. ใช้ Compliance Checklist ใน `team-checklist.md`
3. ตรวจ **Gate 5: Sensitive Claims Gate** ทุก Script
4. ตรวจ **Gate 6: Medical Gate** สำหรับ Harmony Sauna
5. Approve → Status = `Approved` | Reject → Status = `Revision Required` + Revision Notes
6. Writer แก้ไข Script ที่ถูก Reject + ส่ง Review ใหม่

**Output Day 2:**
- ✅ 3 Script Drafts ใน Queue
- ✅ อย่างน้อย 2 Scripts ผ่าน Review (Status = `Approved`)

---

### Day 3 — Creative Briefs + Assign Editor

**เป้าหมาย:** สร้าง Creative Brief 3 ชิ้น ส่งให้ Editor

**ผู้รับผิดชอบหลัก:** Content Strategist

---

**ช่วงเช้า 09:00–11:00 — Brief ชิ้นที่ 1–2 (Harmony Sauna)**

1. เปิด `prompts/04-creative-brief-generator.md`
2. Copy Prompt + Paste Script `SCR-HS-001` ที่ Approved
3. Generate Creative Brief ด้วย AI
4. ตรวจ Brief ครบ:
   - Shot List (3–5 Shot)
   - Visual Direction
   - Text Overlay
   - Thumbnail Concept
   - Required Assets
5. กรอก Brief ใน **Creative Briefs Sheet**:
   - Brief ID: `BRIEF-HS-001`
   - Status = `Briefing`
6. รวบรวม Assets ที่ต้องใช้ → Upload ขึ้น Google Drive
7. ทำซ้ำสำหรับ `SCR-HS-002` → `BRIEF-HS-002`

**ช่วงบ่าย 13:00–15:00 — Brief ชิ้นที่ 3 (ธรรมะดีดี)**

1. สร้าง Brief สำหรับ `SCR-DD-001` → `BRIEF-DD-001`
2. ระบุ Background Music ที่ได้รับอนุญาต (Royalty-free)

**ช่วงบ่าย 15:00–17:00 — ส่งให้ Editor**

1. Assign Designer/Editor + แจ้ง Deadline (Day 4 เย็น)
2. ส่ง Brief พร้อม Assets ผ่าน Google Drive
3. อัปเดต Status = `Sent to Editor`
4. ยืนยัน Export Spec กับ Editor:
   - TikTok/Shorts: 9:16, 1080×1920, MP4
   - Facebook: 1:1 หรือ 16:9, 1080p
   - Static Post: PNG/JPEG ≥ 1080×1080

**Output Day 3:**
- ✅ 3 Creative Briefs พร้อม
- ✅ Assets ใน Google Drive ครบ
- ✅ Editor รับงานแล้ว

---

### Day 4 — Production Day

**เป้าหมาย:** Editor ผลิต Content 3–5 ชิ้นให้เสร็จ

**ผู้รับผิดชอบหลัก:** Designer/Video Editor

---

**ตลอดวัน — ผลิต Content**

| ชิ้น | Brand | Type | Brief ID | Deadline |
|-----|-------|------|---------|---------|
| 1 | Harmony Sauna | Short-form Video 60s | BRIEF-HS-001 | 15:00 Day 4 |
| 2 | Harmony Sauna | Short-form Video 60s | BRIEF-HS-002 | 17:00 Day 4 |
| 3 | ธรรมะดีดี | YouTube Shorts | BRIEF-DD-001 | 17:00 Day 4 |

**Checklist สำหรับ Editor (ทุกชิ้น):**
- [ ] เปิด Creative Brief + ตรวจ Assets ครบ
- [ ] ผลิตตาม Shot List ในBrief
- [ ] ใส่ Text Overlay ตาม Brief (ตรวจ Spelling ภาษาไทย)
- [ ] ตรวจ Audio Level (ไม่แตก, ไม่เบาเกิน)
- [ ] ใส่โลโก้ Brand ตามตำแหน่งที่กำหนด
- [ ] Export ตาม Spec + ตั้งชื่อไฟล์: `[BriefID]-v1.mp4`
- [ ] Upload ขึ้น `Ready for Review/[Brand]/` ใน Google Drive
- [ ] อัปเดต Brief Status = `Review Ready`
- [ ] แจ้ง Strategist ว่างานเสร็จ

**Output Day 4:**
- ✅ 3 ชิ้น (อย่างน้อย) Upload ขึ้น Ready for Review
- ✅ Brief Status = `Review Ready` ทุกชิ้น

---

### Day 5 — Final Review + Captions + Pre-Publish

**เป้าหมาย:** ตรวจ Content Final, สร้าง Captions, เตรียม Publish

**ผู้รับผิดชอบหลัก:** Strategist + Owner

---

**ช่วงเช้า 09:00–11:00 — Final Review (Gate 2)**

1. Strategist เปิดไฟล์ทุกชิ้นบน Mobile (ดูจากมุมมองผู้ใช้งานจริง)
2. ตรวจตาม **Gate 2: Public Posting Gate** ทุกรายการ
3. ตรวจ **Gate 6: Medical Gate** สำหรับ Harmony Sauna อีกครั้ง
4. หากมีปัญหา → ส่งกลับ Editor พร้อม Revision Notes
5. หากผ่าน → Owner Sign-off สำหรับ Harmony Sauna

**ช่วงเช้า 11:00–13:00 — สร้าง Captions**

1. เปิด `prompts/03-caption-generator.md`
2. สร้าง Caption สำหรับทุก Content
3. กรอก Caption ใน **Publishing Calendar**:
   - Content ID
   - Platform
   - Scheduled Date + Time
   - Caption Preview
   - Status = `Pending Approval`
4. ตรวจ CTA, Hashtags, Links ทุกชิ้น
5. ตรวจ: Link ทำงานจริงไหม? LINE ID ถูกต้องไหม?

**ช่วงบ่าย 13:00–15:00 — Owner Final Approval**

1. Owner ตรวจ Publishing Calendar
2. อ่าน Caption ทุกชิ้น
3. Approve → อัปเดต `Approved By` + วันที่ใน Calendar
4. อัปเดต Status = `Scheduled`

**Output Day 5:**
- ✅ ทุก Content ผ่าน Gate 2
- ✅ Captions พร้อมทุกชิ้น
- ✅ Publishing Calendar มีวันที่ Publish ชัดเจน

---

### Day 6 — Publish Day

**เป้าหมาย:** Publish Content ทุกชิ้น บันทึก URL

**ผู้รับผิดชอบหลัก:** Social Scheduler

---

**ตารางการ Publish (แนะนำ)**

| เวลา | Brand | Platform | Content ID |
|-----|-------|---------|-----------|
| 17:00 | Harmony Sauna | TikTok | HS-2026-001 |
| 18:00 | Harmony Sauna | Facebook | HS-2026-002 |
| 19:00 | ธรรมะดีดี | YouTube Shorts | DD-2026-001 |

**Checklist Publish (ทุกชิ้น):**
- [ ] ดาวน์โหลดไฟล์จาก Ready for Review
- [ ] เปิด App/Web ของ Platform
- [ ] Upload ไฟล์
- [ ] วาง Caption ที่เตรียมไว้ใน Publishing Calendar
- [ ] ตรวจ Hashtags + Links + CTA อีกครั้ง
- [ ] กด Publish (ด้วยมือ — ห้าม Schedule อัตโนมัติ)
- [ ] คัดลอก Published URL
- [ ] บันทึก URL ใน **Publishing Calendar** (คอลัมน์ Published URL)
- [ ] เปลี่ยน Status = `Published`
- [ ] แจ้งทีมใน Group Chat: "✅ [Brand] [Platform] Published: [URL]"

**หลัง Publish:**
- [ ] ติดตาม Comment ทุก Post ภายใน 2 ชั่วโมง
- [ ] Reply Comment หรือคำถามทันที

**Output Day 6:**
- ✅ Content ทุกชิ้น Published
- ✅ Published URLs บันทึกครบ
- ✅ Status = `Published` ทุกชิ้น

---

### Day 7 — KPI Collection + Weekly Review

**เป้าหมาย:** เก็บ KPI, วิเคราะห์ผล, ทำ Weekly Review Report

**ผู้รับผิดชอบหลัก:** Strategist + Performance Reviewer + Owner

---

**ช่วงเช้า 09:00–11:00 — เก็บ KPI**

1. เข้า Platform Analytics ของทุก Account
2. กรอก KPI ทุกชิ้นใน **KPI Tracker Sheet** (ดูรายการ KPI ในหัวข้อ 9)
3. คำนวณ Content Score ตามสูตรใน `kpi-tracking.md`
4. บันทึก Repurpose Decision เบื้องต้น

**ช่วงเช้า 11:00–12:00 — Weekly Review ด้วย AI**

1. Export ข้อมูล KPI จาก Sheet (Copy ตาราง)
2. เปิด `prompts/05-weekly-performance-review.md`
3. Paste ข้อมูล KPI + ให้ AI วิเคราะห์
4. Copy ผลลัพธ์ลง **Weekly Review Sheet**

**ช่วงบ่าย 13:00–15:00 — กรอก Weekly Review Template**

1. กรอกตาม **Weekly Review Template** (ดูหัวข้อ 10)
2. สรุป Top Performers + Failed Content
3. บันทึก Patterns ที่พบ
4. สร้าง Recommendation สำหรับสัปดาห์ถัดไป

**ช่วงบ่าย 15:00–16:00 — ประชุม Go/No-Go**

1. Strategist + Owner ประชุมทบทวนผลลัพธ์
2. ตรวจสอบ Go/No-Go Criteria (ดูหัวข้อ 11)
3. บันทึกผลการตัดสินใจในไฟล์นี้หรือ Weekly Review

**Output Day 7:**
- ✅ KPI ครบทุกชิ้น
- ✅ Content Score คำนวณแล้ว
- ✅ Weekly Review Report เสร็จ 1 ฉบับ
- ✅ Go/No-Go ตัดสินใจแล้ว

---

## 5. Team Roles (บทบาทและความรับผิดชอบ)

| บทบาท | ความรับผิดชอบหลัก | สิทธิ์ Approve | ไฟล์ที่ใช้ |
|------|-----------------|--------------|-----------|
| **Owner** | Final Approve ทุกชิ้น, ยืนยัน Brand Info, ตัดสินใจ Go/No-Go | Gate 1 (High Risk), Gate 2, Gate 3, Gate 6 | ทุกไฟล์ |
| **Content Strategist** | Manage Workflow, Generate Ideas, สร้าง Briefs, สร้าง Captions | Gate 1 (Low/Medium), Gate 2 (ธรรมะดีดี) | prompts/, brand-playbooks/, Google Sheets |
| **Script Writer** | เขียน Script Draft ด้วย AI Prompts | - | prompts/02-script-writer.md |
| **Script Reviewer** | ตรวจ Script Compliance, Approve/Reject Scripts | Gate 5 | approval-gates.md, team-checklist.md |
| **Designer/Video Editor** | ผลิต Video + Static Post ตาม Creative Brief | - | Creative Briefs Sheet, Google Drive |
| **Social Scheduler** | Publish บน Platform (ด้วยมือ), บันทึก URL, Reply Comments | - | Publishing Calendar Sheet |
| **Performance Reviewer** | กรอก KPI, คำนวณ Content Score, ทำ Weekly Review | - | kpi-tracking.md, KPI Tracker Sheet |

> 💡 **Note:** ใน Pilot แรก บุคคลหนึ่งอาจรับหลายบทบาท เช่น Strategist + Performance Reviewer ถือเป็นบทบาทเดียวกันได้

---

## 6. First 10 Content Ideas (10 ไอเดียเริ่มต้น)

### Harmony Sauna — 5 Ideas แรก

| # | Idea ID | Title | Hook | Format | Platform | Angle | Goal | CTA | Risk Note | Score |
|---|---------|-------|------|--------|---------|-------|------|-----|---------|-------|
| 1 | IDEA-HS-001 | **ทำไมคนญี่ปุ่นถึงเข้า Sauna ทุกวัน — ความจริงที่คนไทยอาจไม่รู้** | "คนญี่ปุ่นอายุยืน เพราะนิสัยนี้..." | Short-form Video 60s | TikTok | Education | Awareness | เพิ่ม LINE เพื่อจองทดลอง | ห้ามอ้างสรรพคุณทางการแพทย์ | 8 |
| 2 | IDEA-HS-002 | **15 นาทีแรกใน Sauna — คุณจะรู้สึกยังไง? (สำหรับมือใหม่)** | "ครั้งแรกใน Sauna ทุกคนถามว่า..." | Short-form Video 60s | TikTok | How-to / Education | Awareness + Leads | จองผ่าน LINE [ID] | Risk ต่ำ ระวัง claim สุขภาพ | 9 |
| 3 | IDEA-HS-003 | **ก่อน VS หลัง Sauna 4 สัปดาห์ — เรื่องจริงจากลูกค้า** | "4 สัปดาห์ที่ผ่านมา ชีวิตเปลี่ยนอะไรบ้าง?" | Short-form Video 60s | Facebook + TikTok | Testimonial | Engagement + Leads | ทักมาคุย LINE [ID] | ต้องได้รับอนุญาตจากลูกค้าจริงก่อนใช้ | 7 |
| 4 | IDEA-HS-004 | **เปิดห้อง Sauna ให้ดู — ทำไมถึงร้อนสบาย ไม่ใช่ร้อนทรมาน** | "หลายคนกลัว Sauna เพราะคิดว่าร้อนแบบนี้..." | Short-form Video 60s | TikTok | Behind the Scene | Awareness | ดู Package ที่ [Link] | Risk ต่ำ ระวัง claim เรื่องสุขภาพ | 8 |
| 5 | IDEA-HS-005 | **5 เรื่องที่ควรรู้ก่อนเข้า Sauna ครั้งแรก** | "ถ้าไม่รู้ 5 ข้อนี้ อาจไม่สนุกเท่าที่ควร..." | Static Post (Carousel) | Instagram + Facebook | Education | Awareness + Save | เพิ่ม LINE [ID] รับข้อมูลเพิ่มเติม | Risk ต่ำ | 7 |

### ธรรมะดีดี — 5 Ideas แรก

| # | Idea ID | Title | Hook | Format | Platform | Angle | Goal | CTA | Risk Note | Score |
|---|---------|-------|------|--------|---------|-------|------|-----|---------|-------|
| 1 | IDEA-DD-001 | **เมื่อวานเจ็บปวด วันนี้ปล่อยวาง — ธรรมะสอนว่าอย่างไร** | "ถ้าคุณเจ็บปวดอยู่ตอนนี้... ลองฟังนี้" | YouTube Shorts | YouTube Shorts | Storytelling | Awareness + Retention | Subscribe + กด Bell | ห้ามอ้างว่าธรรมะ "รักษา" ปัญหาจิตใจ | 9 |
| 2 | IDEA-DD-002 | **1 นาทีกับสติ — ลองทำดูตอนนี้เลย** | "1 นาทีนี้ อาจเปลี่ยนอารมณ์ทั้งวันของคุณ" | YouTube Shorts | YouTube Shorts | How-to | Engagement + Retention | กด Save ไว้ดูอีกครั้ง | Risk ต่ำ | 8 |
| 3 | IDEA-DD-003 | **พระพุทธเจ้าสอนเรื่องความเครียด — สั้น ชัด ใช้ได้จริง** | "2,500 ปีที่แล้ว พระองค์บอกว่าความเครียดเกิดจาก..." | YouTube Shorts | YouTube Shorts + Facebook | Education | Awareness | ดู Full Video ที่ Channel | ห้ามตีความธรรมะผิด ต้องมีความถูกต้องทางพุทธศาสนา | 8 |
| 4 | IDEA-DD-004 | **เช้านี้เริ่มวันใหม่ด้วยธรรมะ — 60 วินาที** | "ก่อนจะเริ่มวันที่ยุ่งวิ่ง ลองหยุด 60 วินาที..." | Short YouTube Shorts | YouTube Shorts | Storytelling / How-to | Retention + Habit building | ตั้ง Reminder ดูทุกเช้า | Risk ต่ำ | 7 |
| 5 | IDEA-DD-005 | **ธรรมะสอนอะไรเรื่องเงิน — มุมมองที่อาจเปลี่ยนชีวิต** | "คนส่วนใหญ่คิดว่าธรรมะสอนให้ไม่สนใจเงิน... แต่จริงๆ แล้ว..." | Static Post | Facebook | Education | Engagement + Shares | แชร์ให้เพื่อนที่ต้องการ | ระวัง: ห้ามเชื่อมธรรมะกับการลงทุน/การเงินแบบ promise | 7 |

---

## 7. Sample Sheet Rows (ตัวอย่างแถวใน Google Sheets)

### ตัวอย่าง: Content Pipeline Sheet

| Content ID | Brand | Platform | Topic | Hook | Status | Script Writer | Script Reviewer | Scheduled Date | Priority | Notes |
|-----------|-------|---------|-------|------|--------|-------------|----------------|--------------|---------|-------|
| HS-2026-001 | Harmony Sauna | TikTok | ทำไมคนญี่ปุ่นถึงเข้า Sauna ทุกวัน | คนญี่ปุ่นอายุยืน เพราะนิสัยนี้... | Script Draft | @Writer | @Reviewer | 2026-05-13 | High | ระวัง health claim |
| HS-2026-002 | Harmony Sauna | TikTok | 15 นาทีแรกใน Sauna สำหรับมือใหม่ | ครั้งแรกใน Sauna ทุกคนถามว่า... | Approved | @Writer | @Reviewer | 2026-05-14 | High | - |
| DD-2026-001 | ธรรมะดีดี | YouTube Shorts | เมื่อวานเจ็บปวด วันนี้ปล่อยวาง | ถ้าคุณเจ็บปวดอยู่ตอนนี้... | Script Draft | @Writer | @Reviewer | 2026-05-15 | Medium | ห้ามอ้าง "รักษา" |

### ตัวอย่าง: Idea Bank Sheet

| Idea ID | Brand | Platform | Idea Title | Hook | Angle Type | Target Audience | Format | Goal | CTA | Risk Note | Score | Status | Date Added |
|---------|-------|---------|-----------|------|-----------|---------------|--------|------|-----|---------|-------|--------|-----------|
| IDEA-HS-001 | Harmony Sauna | TikTok | ทำไมคนญี่ปุ่นถึงเข้า Sauna ทุกวัน | คนญี่ปุ่นอายุยืน เพราะนิสัยนี้... | Education | ผู้หญิงอายุ 25-40 ที่ต้องการดูแลสุขภาพ | Short-form video 60s | Awareness | เพิ่ม LINE เพื่อจอง | ห้ามอ้างสรรพคุณทางการแพทย์ | 8 | Approved | 2026-05-07 |
| IDEA-DD-001 | ธรรมะดีดี | YouTube Shorts | เมื่อวานเจ็บปวด วันนี้ปล่อยวาง | ถ้าคุณเจ็บปวดอยู่ตอนนี้... | Storytelling | ผู้ใหญ่อายุ 30-55 ที่ต้องการความสงบใจ | YouTube Shorts | Awareness + Retention | Subscribe + กด Bell | ห้ามอ้างว่าธรรมะ "รักษา" | 9 | Approved | 2026-05-07 |

### ตัวอย่าง: KPI Tracker Sheet

| Content ID | Brand | Platform | Publish Date | Views | 3s Retention | Watch Time | Completion Rate | Likes | Comments | Shares | Saves | DM | Leads | Conversion Signal | Content Score | Repurpose Decision |
|-----------|-------|---------|-------------|-------|------------|-----------|----------------|-------|---------|-------|-------|-----|-------|-----------------|-------------|------------------|
| HS-2026-001 | Harmony Sauna | TikTok | 2026-05-13 | 8,420 | 64% | 38s | 44% | 310 | 45 | 22 | 67 | 9 | 2 | Strong | 74 | Repeat |
| DD-2026-001 | ธรรมะดีดี | YouTube Shorts | 2026-05-15 | 3,200 | 58% | 42s | 51% | 180 | 28 | 15 | 55 | 3 | 0 | Weak | 61 | Repeat |

### ตัวอย่าง: Publishing Calendar Sheet

| Date | Time | Brand | Platform | Content ID | Caption Preview | Status | Scheduler | Approved By | Published URL | Notes |
|------|------|-------|---------|-----------|----------------|--------|---------|-----------|-------------|-------|
| 2026-05-13 | 17:00 | Harmony Sauna | TikTok | HS-2026-001 | "คนญี่ปุ่นอายุยืนเพราะนิสัยนี้... 🇯🇵 #Sauna #ผ่อนคลาย" | Published | @Scheduler | @Owner | https://tiktok.com/... | ลงก่อน Facebook 1 ชั่วโมง |
| 2026-05-15 | 19:00 | ธรรมะดีดี | YouTube Shorts | DD-2026-001 | "ถ้าคุณเจ็บปวดอยู่ตอนนี้... #ธรรมะ #สติ" | Published | @Scheduler | @Strategist | https://youtube.com/... | - |

---

## 8. Approval Checklist (Checklist อนุมัติคอนเทนต์)

> ใช้ Checklist นี้สำหรับทุก Content ก่อน Publish

### Checklist ก่อน Script Approved (Gate 1)

- [ ] Idea มี Hook ชัดเจน — อ่านแล้วเข้าใจใน 3 วินาที
- [ ] Platform ระบุชัดเจน
- [ ] Risk Note ตรวจแล้ว — ไม่มี High Risk Claims ที่ไม่มีแผนจัดการ
- [ ] ไม่ซ้ำกับ Content ที่ Publish ใน 30 วัน
- [ ] Idea Score ≥ 6/10
- [ ] ทรัพยากร (Writer, Editor) พร้อม
- [ ] **สำหรับ Harmony Sauna:** ไม่มีสรรพคุณทางการแพทย์
- [ ] **ผู้ Approve:** _____________________ วันที่: _____________

### Checklist ก่อน Publish (Gate 2)

- [ ] เนื้อหาตรงกับ Script ที่ Approved
- [ ] Brand Guidelines ถูกต้อง (โลโก้, สี, Font)
- [ ] ไม่มีข้อมูลผิดพลาด (ราคา, วันที่, ชื่อ)
- [ ] Caption ถูกต้องตาม Platform (ความยาว, Hashtags, Format)
- [ ] CTA ชัดเจนและถูกต้อง — Link ทำงาน, LINE ID ถูก
- [ ] ไม่มี Copyright issues (เพลง, ภาพ, Footage)
- [ ] ไม่มีข้อมูลส่วนตัวที่ไม่ได้รับอนุญาต
- [ ] Visual Quality ผ่านมาตรฐาน (ไม่พร่ามัว, เสียงไม่แตก)
- [ ] **สำหรับ Harmony Sauna:** Gate 6 Medical — ไม่มีสรรพคุณทางการแพทย์
- [ ] **สำหรับ ธรรมะดีดี:** ไม่มีการอ้างว่าธรรมะ "รักษา" โรค
- [ ] **ผู้ Approve:** _____________________ วันที่: _____________ เวลา: _____________

### Checklist สำหรับ Harmony Sauna (Medical Gate — Gate 6)

**คำที่ห้ามใช้:**
- [ ] ❌ "รักษา", "บำบัด", "หาย" (โรค/อาการ)
- [ ] ❌ "ลดความดัน", "ลดน้ำตาล", "ดีต่อหัวใจ" (โดยไม่มีหลักฐาน)
- [ ] ❌ "พิสูจน์แล้วทางการแพทย์"
- [ ] ❌ Claim ที่อ้างอิงสถิติทางการแพทย์โดยไม่มี Source

**คำที่ใช้ได้:**
- [ ] ✅ "ผ่อนคลาย", "รู้สึกสดชื่น", "รู้สึกดีขึ้น"
- [ ] ✅ "ช่วยให้นอนหลับได้ง่ายขึ้น" (ประสบการณ์ส่วนตัว)
- [ ] ✅ "หลายคนบอกว่า..." (แยกชัดว่าเป็นประสบการณ์ ไม่ใช่ fact)

---

## 9. KPI Collection Method (วิธีเก็บ KPI)

### วิธีเข้าถึง Analytics

| Platform | วิธีเข้า Analytics |
|---------|-----------------|
| **TikTok** | TikTok App → Profile → Analytics → แตะ Content → เลือก Video |
| **YouTube Shorts** | YouTube Studio (studio.youtube.com) → Analytics → เลือก Video |
| **Facebook Video** | Meta Business Suite → Insights → Content → เลือก Post |
| **Instagram Reel** | Instagram App → Profile → Reel ที่ต้องการ → View Insights |

### ตารางการกรอก KPI

| Platform | กรอกครั้งแรก | กรอกครั้งที่ 2 |
|---------|------------|--------------|
| TikTok | หลัง 24 ชั่วโมง | หลัง 7 วัน |
| YouTube Shorts | หลัง 48 ชั่วโมง | หลัง 14 วัน |
| Facebook | หลัง 48 ชั่วโมง | หลัง 7 วัน |

### KPI ที่ต้องกรอกทุกชิ้น

**กลุ่มที่ 1 — Reach:**
- Views หรือ Reach (แล้วแต่ Platform)

**กลุ่มที่ 2 — Retention:**
- 3-Second Retention (%) — ดูจาก Platform Analytics
- Average Watch Time (วินาที)
- Completion Rate (%)

**กลุ่มที่ 3 — Engagement:**
- Likes, Comments, Shares, Saves

**กลุ่มที่ 4 — Conversion:**
- Link Clicks (ถ้ามี)
- DM/Inquiries (นับเองจาก Inbox)
- Leads (ถ้ามี)
- Conversion Signal (ประเมินตารางใน `kpi-tracking.md`)

### การคำนวณ Content Score

```
Content Score = (Reach Score × 25%) + (Engagement Score × 25%) + 
                (Conversion Signal × 30%) + (Reuse Potential × 20%)
```

> ดูวิธีคำนวณแต่ละ Component อย่างละเอียดใน `kpi-tracking.md`

### วิธีนับ DM/Inquiries

DM/Inquiries ไม่สามารถดูได้จาก Analytics โดยตรง ให้นับเองดังนี้:

1. เปิด Inbox ของแต่ละ Platform หลัง Publish
2. นับข้อความที่เข้ามาใหม่ที่เกี่ยวกับ Content นั้น (ภายใน 7 วัน)
3. กรอกตัวเลขใน KPI Tracker ทุกวัน หรืออย่างน้อยวันที่ 1, 3, 7

---

## 10. Weekly Review Template (Template สรุปรายสัปดาห์)

> กรอก Template นี้ใน **Weekly Review Sheet** ใน Google Sheets ทุก Day 7

---

### Weekly Review — สัปดาห์ที่: _______ | วันที่: _______ ถึง _______

**ผู้จัดทำ:** _____________________ | **วันที่จัดทำ:** _____________

---

**ส่วนที่ 1 — ภาพรวมสัปดาห์**

| รายการ | Harmony Sauna | ธรรมะดีดี | รวม |
|-------|------------|---------|-----|
| Content ที่ Publish | | | |
| Content Score เฉลี่ย | | | |
| Views รวม | | | |
| Engagement รวม (Likes+Comments+Shares) | | | |
| DM/Leads รวม | | | |
| Content ที่ Score ≥ 60 | | | |

---

**ส่วนที่ 2 — Top Performers (Content ที่ทำได้ดีที่สุด)**

| Content ID | แบรนด์ | Platform | Score | เหตุผลที่ทำได้ดี |
|-----------|------|---------|-------|----------------|
| | | | | |
| | | | | |

---

**ส่วนที่ 3 — Failed Content (Content ที่ทำได้ไม่ดี)**

| Content ID | แบรนด์ | Platform | Score | เหตุผลที่ทำได้ไม่ดี |
|-----------|------|---------|-------|------------------|
| | | | | |

---

**ส่วนที่ 4 — Patterns ที่พบ**

- **Pattern ที่ 1:** _____________________________________________________________
- **Pattern ที่ 2:** _____________________________________________________________
- **Pattern ที่ 3:** _____________________________________________________________

---

**ส่วนที่ 5 — สิ่งที่ควรทำซ้ำสัปดาห์หน้า**

- Hook Style ที่ได้ผล: ___________________________________________________________
- Format ที่ได้ผล: _______________________________________________________________
- Topic ที่ได้ผล: ________________________________________________________________

---

**ส่วนที่ 6 — สิ่งที่ควรหยุดทำ**

- Hook Style ที่ไม่ได้ผล: ________________________________________________________
- Format ที่ไม่ได้ผล: ____________________________________________________________
- Topic ที่ไม่ได้ผล: ______________________________________________________________

---

**ส่วนที่ 7 — Recommendations สำหรับสัปดาห์หน้า**

1. ___________________________________________________________________________
2. ___________________________________________________________________________
3. ___________________________________________________________________________

---

**ส่วนที่ 8 — Process Issues (ปัญหาใน Workflow ที่พบ)**

| ขั้นตอน | ปัญหา | แนวทางแก้ไข |
|--------|------|-----------|
| | | |
| | | |

---

**ส่วนที่ 9 — Go/No-Go Preliminary Assessment**

> (กรอกวันแรก เพื่อเตรียมพร้อมสำหรับการตัดสินใจ Go/No-Go ใน Day 7)

| เกณฑ์ | เป้าหมาย | ผลจริง | ผ่าน/ไม่ผ่าน |
|------|---------|-------|-----------|
| Content Published ≥ 5 ชิ้น | 5 | | |
| Content Score เฉลี่ย ≥ 50 | 50 | | |
| ทุก Content ผ่าน Approval Gate | 100% | | |
| KPI บันทึกครบ | 100% | | |

---

## 11. Automation Go/No-Go Criteria (เกณฑ์ตัดสินใจสู่ V2)

> ใช้เกณฑ์นี้หลังจาก Pilot 7 วันเสร็จสมบูรณ์ เพื่อตัดสินใจว่าจะเดินหน้าสู่ V2 Automation หรือไม่

### เกณฑ์ GO (ผ่านทั้งหมด = สามารถเดินหน้า V2 ได้)

| # | เกณฑ์ | วิธีวัด | เป้าหมายขั้นต่ำ |
|---|------|--------|--------------|
| G1 | **Content Published จริง** | นับจาก Published URLs ใน Calendar | ≥ 5 ชิ้น |
| G2 | **KPI บันทึกครบ** | ตรวจ KPI Tracker Sheet — ทุก Column กรอกแล้ว | 100% ของ Published Content |
| G3 | **Content Score เฉลี่ย** | ค่าเฉลี่ย Content Score ทุกชิ้น | ≥ 50/100 |
| G4 | **Approval Gate ไม่มีการข้าม** | ไม่มี Content ใดที่ Publish โดยไม่ผ่าน Gate 1 และ Gate 2 | 0 violations |
| G5 | **ทีมเข้าใจ Workflow** | Strategist และ Owner ยืนยันว่าเข้าใจขั้นตอนทั้งหมด | ยืนยันเป็นลายลักษณ์อักษร |
| G6 | **Process Issues บันทึกไว้** | Weekly Review มี Process Issues Section กรอกครบ | บันทึกอย่างน้อย 3 รายการ (ถ้าพบ) |
| G7 | **Weekly Review เสร็จ** | Weekly Review Sheet กรอกครบทุก Section | 1 ฉบับ Complete |

### เกณฑ์ NO-GO (ถ้ามีข้อใดข้อหนึ่ง = ต้อง Re-Pilot ก่อน)

| # | เกณฑ์ NO-GO | เหตุผล | การแก้ไข |
|---|-----------|------|---------|
| N1 | **ไม่มี Content ที่ Publish จริงเลย** | Automation จะไม่มีประโยชน์ถ้าขั้นตอน Manual ยังไม่ผ่าน | Re-run Pilot สัปดาห์ถัดไป |
| N2 | **KPI ไม่ครบ (< 80%)** | ไม่มีข้อมูลพื้นฐานสำหรับออกแบบ V2 | กรอก KPI ให้ครบก่อน |
| N3 | **มีการ Publish โดยข้าม Approval Gate** | ความเสี่ยง Brand Safety ยังไม่ได้รับการจัดการ | Retrain ทีม + Re-audit ทุก Content |
| N4 | **ทีมไม่เข้าใจ Workflow** | Automation จะทำให้ปัญหาแย่ลง ไม่ใช่ดีขึ้น | ทำ Training ก่อนก้าวไป V2 |
| N5 | **Content Score เฉลี่ย < 30** | Content Quality ยังต่ำเกินไป ต้องแก้ไขก่อน Scale | วิเคราะห์ Root Cause + ปรับ Prompt/Brief |
| N6 | **Owner ไม่ยืนยัน GO** | ต้องได้รับความเห็นชอบจาก Owner ก่อนเสมอ | ประชุมทบทวนผลและรับ Feedback |

### ผลการตัดสินใจ

| ผล | ความหมาย | ขั้นตอนถัดไป |
|----|---------|-----------|
| **✅ GO** | ผ่านทุกเกณฑ์ G1–G7 และไม่มีเกณฑ์ N1–N6 | เริ่มออกแบบ V2 Automation Plan |
| **⚠️ CONDITIONAL GO** | ผ่านเกณฑ์ G1–G4 แต่ G5–G7 ยังไม่สมบูรณ์ | แก้ไข G5–G7 ภายใน 1 สัปดาห์ แล้วค่อยเริ่ม V2 |
| **❌ NO-GO** | มีเกณฑ์ N ข้อใดข้อหนึ่ง | แก้ไขให้ครบ + Re-Pilot อีกรอบ |

### บันทึกผลการตัดสินใจ Go/No-Go

> กรอกหลัง Day 7 Review เสร็จ

| รายการ | ค่า |
|-------|-----|
| **วันที่ตัดสินใจ** | |
| **ผลการตัดสินใจ** | ✅ GO / ⚠️ CONDITIONAL GO / ❌ NO-GO |
| **เหตุผล** | |
| **ขั้นตอนถัดไป** | |
| **ผู้ตัดสินใจ (Owner)** | |

---

## หมายเหตุสำคัญ (Important Disclaimers)

1. **Pilot ยังไม่สำเร็จ** จนกว่าจะมี Published Content จริง + KPI จริง บันทึกใน Google Sheets ครบ
2. **ห้าม Claim Pilot Success** หากยังไม่มีข้อมูลจริงจาก Platform Analytics
3. **The World Freight** ไม่ใช่ส่วนหนึ่งของ Pilot นี้ และยังอยู่ในสถานะ PARKED
4. **ไม่มี Automation ใดๆ** ใน V1 ทั้งหมดเป็น Manual — AI ช่วยแค่ Generate Content ผ่าน Copy-Paste เท่านั้น
5. เอกสารนี้เป็น **Living Document** ให้อัปเดตทุกครั้งที่พบปัญหาหรือข้อปรับปรุง

---

*Pilot Runbook V1 | AI Content Engine V1 | สร้าง: 2026-05-05 | สำหรับทีม Harmony Sauna & ธรรมะดีดี*
