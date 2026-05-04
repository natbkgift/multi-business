# 🥈 Real Estate Lead Generation Automation

ระบบ #2 — รับ lead อัตโนมัติจาก Facebook / LINE / Website ตอบภายใน 5 นาที

## ภาพรวม

ระบบนี้จะช่วยให้คุณ:
- รับ lead จาก Facebook Lead Ads / LINE OA / Website Form
- ส่ง auto-reply ให้ลูกค้าทันที (< 5 นาที)
- บันทึก lead เข้า HubSpot CRM + Google Sheets
- คำนวณ Lead Score อัตโนมัติ
- แจ้งทีม Sales **หลังจาก Manager อนุมัติ** (Human Approval Gate)
- ส่ง follow-up sequence วัน 1, 3, 7

## ไฟล์ในโฟลเดอร์นี้

| ไฟล์ | วัตถุประสงค์ |
|------|------------|
| `make-scenario.json` | Import เข้า Make.com ได้ทันที |
| `hubspot-fields.md` | Custom fields สำหรับ HubSpot CRM |
| `lead-scoring-formula.md` | สูตรคำนวณ Lead Score |
| `leads-template.csv` | โครงสร้าง Google Sheets Lead Log |

## วิธี Setup

### Step 1: HubSpot CRM
1. สร้างบัญชี HubSpot ฟรีที่ [hubspot.com](https://app.hubspot.com/signup)
2. ไปที่ Settings → Properties → Create custom properties ตาม `hubspot-fields.md`
3. สร้าง Pipeline: `New Lead → Contacted → Viewing Scheduled → Offer → Closed Won / Lost`
4. สร้าง Private App token: Settings → Integrations → Private Apps

### Step 2: Facebook Lead Ads
1. สร้าง Lead Ad Form ใน Facebook Ads Manager
2. Fields: ชื่อ, เบอร์โทร, งบประมาณ, ทำเลที่ต้องการ, ประเภทอสังหาฯ
3. Setup Facebook → Make.com webhook ใน Make.com

### Step 3: Make.com
1. Import `make-scenario.json` เข้า Make.com
2. กำหนด connections: Facebook, HubSpot, Google Sheets, LINE, Gmail
3. อัปเดต HubSpot API token
4. ทดสอบด้วย test lead

## Lead Score คำนวณอย่างไร?

ดูรายละเอียดที่ `lead-scoring-formula.md`

คะแนนรวม:
- 80-100: Hot Lead 🔥 — ติดต่อภายใน 1 ชั่วโมง
- 60-79: Warm Lead 🌡️ — ติดต่อภายใน 24 ชั่วโมง
- 40-59: Cool Lead ❄️ — เข้า nurture sequence
- < 40: Cold Lead 🧊 — เก็บไว้ใน database

---

> ⚠️ **Reminder:** ต้องมี Human Approval ก่อน assign lead ให้ agent เสมอ
