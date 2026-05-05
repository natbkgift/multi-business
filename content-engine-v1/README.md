# AI Content Engine V1 — Manual-First Content Production System

> ระบบผลิตคอนเทนต์แบบ Manual-First สำหรับหลายแบรนด์ ขับเคลื่อนด้วย AI Prompts และ Google Sheets

---

## วัตถุประสงค์ (Mission)

AI Content Engine V1 คือระบบบริหารจัดการคอนเทนต์แบบครบวงจร ที่ออกแบบมาเพื่อ:

- **ผลิตคอนเทนต์สม่ำเสมอ** ในหลายแบรนด์พร้อมกัน โดยทีมเล็กๆ
- **ใช้ AI เป็นเครื่องมือช่วย** ไม่ใช่เครื่องมือแทนมนุษย์ ทุกอย่างผ่านการตรวจสอบก่อน publish
- **มีระบบ Approval Gate** ชัดเจน เพื่อลดความเสี่ยงด้าน compliance และ brand safety
- **วัดผลได้จริง** ด้วย KPI Tracker และ Weekly Review

ระบบนี้ทำงานแบบ **Manual-First** คือมนุษย์เป็นคนตัดสินใจทุกขั้นตอน AI เป็นแค่ผู้ช่วย

---

## 5 แบรนด์เป้าหมาย

| แบรนด์ | ประเภท | Platform หลัก | สถานะ |
|--------|--------|----------------|--------|
| **Harmony Sauna** | Wellness / Lifestyle | TikTok, Instagram, Facebook | ✅ Active |
| **AMP Pattaya** | Real Estate | Facebook, YouTube, Instagram | ✅ Active |
| **The World Freight** | B2B Logistics | Facebook, LinkedIn, YouTube | ✅ Active |
| **ธรรมะดีดี** | Spiritual / YouTube | YouTube, Facebook | ✅ Active |
| **Affiliate / AI Automation** | Education / Affiliate | TikTok, YouTube Shorts, Facebook | ✅ Active |

> ⚠️ **Freight Pilot Note:** The World Freight อยู่ในช่วง Park (ระงับการผลิตเต็มรูปแบบชั่วคราว) ให้สร้าง Script Draft และ Idea ได้ แต่ยังไม่ต้อง Publish จนกว่าจะได้รับการยืนยัน

---

## โครงสร้างโฟลเดอร์ทั้งหมด

```
content-engine-v1/
│
├── README.md                          ← ไฟล์นี้ — ภาพรวมระบบทั้งหมด
├── google-sheets-structure.md         ← โครงสร้าง Google Sheets 7 Sheets
├── status-workflow.md                 ← สถานะคอนเทนต์ 12 ขั้นตอน + Flowchart
├── approval-gates.md                  ← ประตูอนุมัติ 6 ประเภท
├── kpi-tracking.md                    ← ระบบวัดผล + สูตร Content Score
├── team-checklist.md                  ← Checklist รายบทบาท 6 ตำแหน่ง
├── seven-day-pilot-plan.md            ← แผน 7 วันเริ่มต้นใช้งาน
├── content-pipeline-template.csv     ← Template CSV สำหรับ Content Pipeline
├── content-calendar-template.csv     ← Template CSV สำหรับ Publishing Calendar
│
├── prompts/
│   ├── 01-idea-generator.md           ← Prompt สร้าง Idea 10 ชิ้น
│   ├── 02-script-writer.md            ← Prompt เขียน Script
│   ├── 03-caption-generator.md        ← Prompt สร้าง Caption
│   ├── 04-creative-brief-generator.md ← Prompt สร้าง Creative Brief
│   └── 05-weekly-performance-review.md← Prompt วิเคราะห์ผลรายสัปดาห์
│
└── brand-playbooks/
    ├── harmony-sauna.md               ← Playbook: Harmony Sauna
    ├── amp-pattaya.md                 ← Playbook: AMP Pattaya
    ├── the-world-freight.md           ← Playbook: The World Freight
    ├── dhamma-dee-dee.md              ← Playbook: ธรรมะดีดี
    └── affiliate-ai-automation.md    ← Playbook: Affiliate / AI Automation
```

---

## วิธีเริ่มต้นใช้งาน (Quick Start 5 ขั้นตอน)

### ขั้นตอนที่ 1 — เตรียม Google Sheets
1. เปิด Google Sheets ใหม่ ตั้งชื่อว่า `AI Content Engine V1`
2. สร้าง 7 sheets ตามโครงสร้างใน `google-sheets-structure.md`
3. Import `content-pipeline-template.csv` และ `content-calendar-template.csv`

### ขั้นตอนที่ 2 — อ่าน Brand Playbook ของแบรนด์ที่จะเริ่ม
1. เปิด `brand-playbooks/[ชื่อแบรนด์].md`
2. ทำความเข้าใจ Tone, Do/Don't, และ Content Pillars
3. นำไปประกอบการใช้งาน Prompt

### ขั้นตอนที่ 3 — สร้าง Idea ด้วย AI
1. เปิด `prompts/01-idea-generator.md`
2. Copy prompt ไปวางใน ChatGPT / Claude
3. กรอก Input ตามแบรนด์และ Platform ที่ต้องการ
4. นำผลลัพธ์กรอกลง Idea Bank ใน Google Sheets

### ขั้นตอนที่ 4 — เขียน Script และผ่าน Approval Gate
1. เลือก Idea ที่ดีที่สุด → เปลี่ยน Status เป็น "Selected"
2. ใช้ `prompts/02-script-writer.md` สร้าง Script Draft
3. ส่ง Script ให้ Script Reviewer ตรวจสอบตาม `approval-gates.md`
4. หาก Approve → สร้าง Creative Brief ด้วย `prompts/04-creative-brief-generator.md`

### ขั้นตอนที่ 5 — Publish และวัดผล
1. Publish ตามตารางใน Publishing Calendar
2. กรอก KPI ใน KPI Tracker หลัง 24-48 ชั่วโมง
3. ทุกสัปดาห์ใช้ `prompts/05-weekly-performance-review.md` วิเคราะห์ผล
4. อัปเดต Weekly Review sheet

---

## สิ่งที่ระบบนี้ไม่ทำ (Hard Boundaries)

| สิ่งที่ระบบนี้ไม่ทำ | เหตุผล |
|---------------------|--------|
| ❌ ไม่มี Automation ใดๆ (Make.com, n8n, Zapier) | Manual-First คือหัวใจของระบบ |
| ❌ ไม่มี API Calls อัตโนมัติ | ทุกการใช้ AI ต้องผ่านมนุษย์ copy-paste |
| ❌ ไม่มี Auto-Publish | ทุก post ต้องมีมนุษย์กดปุ่ม Publish |
| ❌ ไม่มี Auto-Scheduling | ใช้ Calendar เป็น reference ไม่ใช่ trigger |
| ❌ ไม่มีการส่ง Credentials ผ่านระบบ | ห้ามเก็บ password หรือ API Key ในไฟล์ |
| ❌ ไม่รับประกัน ROI หรือผลลัพธ์ทางการแพทย์ | ทุกแบรนด์มี Risk Control ชัดเจน |

---

## Disclaimer

> **The World Freight — Freight Pilot Status: PARKED**
>
> The World Freight อยู่ในสถานะ "Park" ชั่วคราว หมายความว่า:
> - ✅ สามารถสร้าง Idea และ Script Draft ได้ตามปกติ
> - ✅ สามารถวางแผนคอนเทนต์ใน Calendar ได้
> - ❌ ยังไม่ให้ Publish คอนเทนต์ใดๆ ต่อสาธารณะ
> - ❌ ยังไม่ให้แสดงราคา หรือ quotation ในคอนเทนต์
>
> สถานะนี้จะเปลี่ยนแปลงเมื่อได้รับการยืนยันจาก Owner เท่านั้น

---

## เวอร์ชันและการอัปเดต

| เวอร์ชัน | วันที่ | การเปลี่ยนแปลง |
|---------|--------|----------------|
| V1.0 | 2026-05-06 | สร้างระบบครั้งแรก |

---

*AI Content Engine V1 — สร้างสำหรับทีมเล็กๆ ที่ต้องการผลิตคอนเทนต์ที่มีคุณภาพ สม่ำเสมอ และปลอดภัย*
