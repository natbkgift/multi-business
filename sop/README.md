# 📚 Intern / SOP Management System

ระบบ #5 — จัดการการฝึกอบรมพนักงานและเอกสาร SOP

## ภาพรวม

ระบบนี้จะช่วยให้คุณ:
- สร้าง Onboarding checklist อัตโนมัติสำหรับ Intern ใหม่
- Track progress การเรียนรู้
- จัดการเอกสาร SOP ที่อัปเดตเสมอ
- ส่ง reminder ถ้า training ไม่เสร็จตามกำหนด

## โครงสร้างโฟลเดอร์

```
sop/
├── README.md                  ← คุณอยู่ที่นี่
├── onboarding-checklist.md    ← Checklist สำหรับ Intern ใหม่
└── training-modules/
    └── README.md              ← รายการ training modules ทั้งหมด
```

## วิธีใช้งาน

### Notion Setup (แนะนำ)
1. สร้าง Notion workspace
2. สร้าง Database "Team Members" with properties: Name, Role, Start Date, Onboarding Status
3. สร้าง Database "Training Modules" with properties: Module Name, Role, Duration, Quiz Link, Completed By (relation)
4. สร้าง Template page สำหรับ Onboarding ตาม `onboarding-checklist.md`
5. ทุกครั้ง Intern เริ่มงาน: Duplicate template และ assign ให้ Intern

### Alternative: Google Sheets
1. สร้าง sheet "Intern Progress" พร้อม columns: Name, Role, Start Date, Module, Status, Completed Date, Score
2. ใช้ n8n ส่ง email reminder ถ้า module ยังไม่ complete ใน 3 วัน

## SOP Update Process

1. แก้ไข SOP ใน Notion หรือ Google Docs
2. Update "Last Updated" date
3. แจ้ง LINE Group เมื่อมีการเปลี่ยนแปลง SOP สำคัญ
4. ทีมทุกคนต้อง acknowledge การเปลี่ยนแปลง (ใช้ Notion comment หรือ Google Form)
