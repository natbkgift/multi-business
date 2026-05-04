# Multi-Business Automation System 🚀

ระบบอัตโนมัติสำหรับธุรกิจหลายสาขา | 90-Day Automation Execution Plan

---

## ภาพรวมโครงการ

ระบบนี้ออกแบบมาเพื่อให้ทีมขนาดเล็กสามารถบริหารธุรกิจ 5 สาขาได้อย่างมีประสิทธิภาพ โดยใช้ระบบอัตโนมัติที่มี ROI สูงสุดก่อน เน้นการสร้างรายได้และลดเวลาซ้ำซ้อน ทุก workflow มีจุด **Human Approval** ก่อนการส่งข้อมูลออกสู่ภายนอก

## ลำดับความสำคัญของระบบ

| ลำดับ | โฟลเดอร์ | ระบบ | คะแนน ROI |
|-------|---------|------|-----------|
| 🥇 1 | [`/freight`](./freight/) | Freight Forwarding Quotation | 95 |
| 🥈 2 | [`/real-estate`](./real-estate/) | Real Estate Lead Generation | 88 |
| 🥉 3 | [`/sauna`](./sauna/) | Harmony Sauna LINE Flow | 82 |
| 4 | [`/content`](./content/) | AI Content / YouTube Shorts | 74 |
| 5 | [`/sop`](./sop/) | Intern / SOP Management | 58 |

## โครงสร้างโฟลเดอร์

```
multi-business/
├── README.md                    ← คุณอยู่ที่นี่
├── docs/
│   ├── 90-day-roadmap.md
│   ├── architecture.md
│   ├── kpi-dashboard.md
│   ├── tool-stack.md
│   ├── risks-and-safeguards.md
│   └── weekly-checklist.md
├── freight/                     ← 🥇 เริ่มที่นี่ก่อน
│   ├── README.md
│   ├── n8n-workflow.json
│   ├── rate-table.csv
│   ├── quote-log-template.csv
│   └── quotation-template.md
├── real-estate/
│   ├── README.md
│   ├── make-scenario.json
│   ├── hubspot-fields.md
│   ├── lead-scoring-formula.md
│   └── leads-template.csv
├── sauna/
│   ├── README.md
│   ├── line-chatbot-flow.json
│   ├── n8n-workflow.json
│   ├── booking-template.csv
│   └── coupon-generator.md
├── content/
│   ├── README.md
│   ├── n8n-workflow.json
│   └── content-calendar.csv
└── sop/
    ├── README.md
    ├── onboarding-checklist.md
    └── training-modules/
        └── README.md
```

## เริ่มต้นใช้งาน

1. อ่าน [`docs/90-day-roadmap.md`](./docs/90-day-roadmap.md) เพื่อดูแผนรวม
2. เริ่มที่ [`freight/README.md`](./freight/README.md) — ระบบที่ควรทำก่อนสุด
3. Import `freight/n8n-workflow.json` เข้า n8n instance ของคุณ
4. Copy `freight/rate-table.csv` และ `freight/quote-log-template.csv` เข้า Google Sheets
5. ติดตาม KPI ด้วย [`docs/kpi-dashboard.md`](./docs/kpi-dashboard.md)

## เป้าหมาย 90 วัน

- ✅ ลดงาน manual **60-70%**
- ✅ เพิ่มความเร็วตอบ lead **3-5 เท่า**
- ✅ ประหยัดเวลา **25+ ชั่วโมง/สัปดาห์**
- ✅ เพิ่ม conversion **20-35%**
- ✅ ค่าใช้จ่ายระบบ **1,500-4,000 ฿/เดือน**

---

> ⚠️ **หมายเหตุสำคัญ:** ทุก automation มีจุด **Human Approval** ก่อนส่งข้อมูลออกสู่ภายนอก
> ไม่มีระบบใดส่ง quote / assign lead / publish content โดยอัตโนมัติโดยไม่ผ่านการอนุมัติจากมนุษย์
