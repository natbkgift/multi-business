# System Architecture | สถาปัตยกรรมระบบ

## ภาพรวม Integration

```
                        ┌─────────────────┐
                        │   n8n / Make.com │
                        │  (Automation Hub) │
                        └────────┬────────┘
              ┌──────────────────┼──────────────────┐
              │                  │                  │
    ┌─────────▼──────┐  ┌───────▼───────┐  ┌──────▼───────┐
    │  Google Sheets │  │  LINE OA API  │  │  HubSpot CRM │
    │  (Database)    │  │  (Messaging)  │  │  (Real Estate)│
    └────────────────┘  └───────────────┘  └──────────────┘
              │                  │
    ┌─────────▼──────┐  ┌───────▼───────┐
    │  Google Docs   │  │ FB Messenger  │
    │  (PDF/Quotes)  │  │  (Lead Gen)   │
    └────────────────┘  └───────────────┘
```

---

## ระบบที่ 1: Freight Forwarding Quotation

```
TRIGGER: ลูกค้ากรอก Google Form / LINE / Messenger
         │
         ▼
    [n8n Webhook]
    รับข้อมูล: Origin, Destination, Weight, Dimensions, Type
         │
         ▼
    [Google Sheets - Rate Table]
    ดึง Rate → คำนวณ CBM, Weight Charge, Surcharge, Total
         │
         ▼
    [Google Docs Template]
    สร้าง PDF Quotation Draft โดยอัตโนมัติ
         │
         ▼
    ⚠️ [LINE Notify / Email → ผู้รับผิดชอบ]
    HUMAN APPROVAL REQUIRED
    (อนุมัติ / แก้ไข / ปฏิเสธ)
         │
         ▼ (อนุมัติแล้ว)
    [ส่ง Quote ให้ลูกค้า ผ่าน LINE / Email]
         │
         ▼
    [Google Sheets - Quote Log]
    Update Status → ติดตาม Response
         │
         ▼
    [Booking Confirmed?]
    ├── YES → Auto-generate Booking Summary → Update Revenue Sheet
    └── NO  → Follow-up Reminder ใน 3 วัน
```

---

## ระบบที่ 2: Real Estate Lead Generation

```
TRIGGER: Facebook Lead Ad / LINE OA / Website Form
         │
         ▼
    [Make.com Webhook]
    รับ Lead: ชื่อ, เบอร์, งบ, ทำเล, Timeline
         │
         ▼
    [Auto-Reply Messenger/LINE]
    "ขอบคุณครับ ทีมงานจะติดต่อกลับภายใน 5 นาที"
         │
         ▼
    [HubSpot CRM - Create Contact]
    บันทึก Lead + คำนวณ Lead Score
         │
         ▼
    ⚠️ [LINE Notify → Manager]
    HUMAN APPROVAL: ยืนยัน Lead Assignment
         │
         ▼ (อนุมัติแล้ว)
    [Assign to Agent + Send Welcome Message]
         │
         ▼
    [Follow-up Sequence: Day 1, Day 3, Day 7]
    (Email + LINE ตาม preference ลูกค้า)
```

---

## ระบบที่ 3: Harmony Sauna LINE Flow

```
TRIGGER: ลูกค้าส่งข้อความใน LINE OA
         │
         ▼
    [n8n - Intent Detection]
    วิเคราะห์ intent: จอง / สอบถาม / ราคา / อื่นๆ
         │
         ├── จอง ──────────────────────────────────┐
         ├── ราคา → ส่ง Package Menu               │
         └── อื่นๆ → Fallback to Human             │
                                                   ▼
                                         [แนะนำ Package]
                                         ลูกค้าเลือก Package
                                                   │
                                                   ▼
                                         [เลือกวัน/เวลา]
                                                   │
                                                   ▼
                                         ⚠️ [Human Approval - Staff]
                                         ยืนยันการจองและ Slot
                                                   │
                                                   ▼
                                         [Generate Coupon Code]
                                         บันทึกใน Google Sheets
                                                   │
                                                   ▼
                                         [Booking Confirmation LINE]
                                                   │
                                                   ▼
                                         [Reminder -24h ก่อนนัด]
```

---

## ระบบที่ 4: AI Content Pipeline

```
TRIGGER: วันจันทร์ 08:00 (Scheduled) หรือ Manual
         │
         ▼
    [n8n - Trend Research]
    ดึงข้อมูลจาก Google Trends / YouTube API
         │
         ▼
    [OpenAI GPT-4]
    สร้าง Script Draft + Hook + CTA
         │
         ▼
    ⚠️ [Google Sheets - Content Queue]
    HUMAN APPROVAL: ตรวจ Script ก่อน Produce
         │
         ▼ (อนุมัติแล้ว)
    [Production Checklist Assigned]
    ถ่าย → ตัดต่อ → Thumbnail → Description
         │
         ▼
    ⚠️ [Final Approval ก่อน Publish]
         │
         ▼
    [Publish + Schedule YouTube/TikTok]
    [Track Performance → Google Sheets Dashboard]
```

---

## ระบบที่ 5: Intern / SOP Management

```
TRIGGER: Intern เริ่มงานวันแรก
         │
         ▼
    [Notion - Onboarding Checklist Auto-Create]
    กำหนด tasks ตามตำแหน่ง
         │
         ▼
    [Training Modules Assigned]
    Video + Reading + Quiz
         │
         ▼
    [Auto-Grade Quiz]
    Pass ≥ 80% → Next Module
    Fail → Retry + Notify Supervisor
         │
         ▼
    [Completion Certificate]
    บันทึกใน HR Sheet
```

---

## Human Approval Gates (จุดที่ต้องมนุษย์อนุมัติ)

| # | ระบบ | เหตุการณ์ | ช่องทางแจ้ง | SLA |
|---|------|----------|------------|-----|
| 1 | Freight | ก่อนส่ง Quotation | LINE Notify + Email | 2 ชม. |
| 2 | Real Estate | ก่อน Assign Lead ให้ Agent | LINE Notify | 30 นาที |
| 3 | Sauna | ยืนยัน Booking Slot | LINE OA (Staff) | 15 นาที |
| 4 | Content | ก่อน Publish ทุกชิ้น | Google Sheets Queue | 24 ชม. |
| 5 | Freight | ก่อนแก้ไข Rate Table | Email + LINE | Manual |
