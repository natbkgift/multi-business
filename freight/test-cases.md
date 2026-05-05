# Freight Pilot Fix V1 — Test Cases

> เอกสารนี้กำหนด test cases สำหรับ Freight Forwarding Quotation Workflow (Pilot V1)
> ทุก test case ต้องผ่านก่อน Go-Live

---

## TC-001: Air Quote (ทดสอบ quotation ทางอากาศ)

**วัตถุประสงค์:** ตรวจสอบว่าระบบคำนวณราคาส่งทางอากาศถูกต้อง

| รายการ | ค่า |
|--------|-----|
| Origin | TH |
| Destination | CN |
| Shipment Type | Air |
| Weight (kg) | 50 |
| Dimensions (cm) | 60 × 40 × 30 |
| USD_THB | 35.50 (env var) |

**ผลที่คาดหวัง:**
- CBM = (60×40×30)/1,000,000 = 0.072
- Chargeable Weight = MAX(50, 0.072×167) = MAX(50, 12.024) = 50 kg
- Freight Charge = 50 × 180 = 9,000 THB
- Total = 9,000 + 500 (doc fee) = 9,500 THB
- `requiresManualQuote = false`
- `quoteStatus = "DRAFT — Pending Internal Approval"`
- ห้ามส่งให้ลูกค้าอัตโนมัติ — ต้องรอ approval webhook

**จุดที่ต้องตรวจสอบ:**
- [ ] USD_THB ไม่ได้ถูก hardcode ใน workflow
- [ ] Quote ถูก log ใน QuoteLog ด้วย status = DRAFT
- [ ] LINE Notify / Email ส่ง notification ให้ manager

---

## TC-002: Sea LCL Quote (ทดสอบ quotation ทะเล LCL)

**วัตถุประสงค์:** ตรวจสอบว่าระบบคำนวณราคา LCL ถูกต้องโดยใช้ USD_THB จาก env

| รายการ | ค่า |
|--------|-----|
| Origin | TH |
| Destination | US |
| Shipment Type | Sea LCL |
| Weight (kg) | 500 |
| Dimensions (cm) | 120 × 80 × 100 |
| USD_THB | 35.50 (env var) |

**ผลที่คาดหวัง:**
- CBM = (120×80×100)/1,000,000 = 0.96
- Chargeable Weight (LCL) = MAX(500/1000, 0.96) = MAX(0.5, 0.96) = 0.96
- Sea LCL Rate TH-US = 120 USD/CBM
- Freight = 0.96 × 120 × 35.50 = 4,089.60 THB → rounded up
- `requiresManualQuote = false`
- `quoteStatus = "DRAFT — Pending Internal Approval"`

**จุดที่ต้องตรวจสอบ:**
- [ ] ราคาใช้ USD_THB จาก environment variable เท่านั้น
- [ ] ไม่มี * 36 ใน calculation

---

## TC-003: Sea FCL Quote (ทดสอบ quotation ทะเล FCL)

**วัตถุประสงค์:** ตรวจสอบ FCL 20ft และ 40ft

| รายการ | ค่า (20ft) | ค่า (40ft) |
|--------|-----------|-----------|
| Origin | TH | TH |
| Destination | SG | SG |
| Shipment Type | Sea FCL 20ft | Sea FCL 40ft |
| USD_THB | 35.50 | 35.50 |

**ผลที่คาดหวัง:**
- FCL 20ft TH-SG: 600 USD × 35.50 = 21,300 THB + 500 doc = 21,800 THB
- FCL 40ft TH-SG: 900 USD × 35.50 = 31,950 THB + 500 doc = 32,450 THB
- `requiresManualQuote = false`
- `quoteStatus = "DRAFT — Pending Internal Approval"`

**จุดที่ต้องตรวจสอบ:**
- [ ] ราคาใช้ USD_THB จาก $env ไม่ใช่ hardcode 36
- [ ] shipmentType matching ทำงานถูกต้อง ("20" vs "40")

---

## TC-004: Unknown Route (ทดสอบเส้นทางที่ไม่มีใน Rate Table)

**วัตถุประสงค์:** ตรวจสอบว่าระบบ handle เส้นทางที่ไม่รู้จักอย่างปลอดภัย

| รายการ | ค่า |
|--------|-----|
| Origin | TH |
| Destination | BR (Brazil — ไม่มีใน table) |
| Shipment Type | Air |

**ผลที่คาดหวัง:**
- `requiresManualQuote = true`
- `error = "No rate found for route: TH-BR"`
- `totalPrice = 0`
- Quote ถูก log ใน QuoteLog ด้วย status = DRAFT + manual required
- ห้ามคำนวณราคาด้วยค่าสุ่มหรือ default

**จุดที่ต้องตรวจสอบ:**
- [ ] workflow ไม่ crash — ทำงานต่อจนถึง notification
- [ ] manager ได้รับ notification ว่า manual quote required
- [ ] ไม่มีราคาส่งให้ลูกค้า

---

## TC-005: Land Shipment Without Rate (ขนส่งทางบกไม่มี rate)

**วัตถุประสงค์:** ตรวจสอบว่าระบบไม่ใช้ hardcoded land rate

| รายการ | ค่า |
|--------|-----|
| Origin | TH |
| Destination | MY |
| Shipment Type | Land |
| Weight (kg) | 200 |

**ผลที่คาดหวัง:**
- `requiresManualQuote = true`
- `error = "Land shipment: no verified rate available. Requires manual quote."`
- `totalPrice = 0`
- ห้ามใช้ `chargeableWeight * 15` หรือค่าอื่นที่ hardcode

**จุดที่ต้องตรวจสอบ:**
- [ ] ไม่มีการคำนวณ `* 15` หรือค่า default ใดๆ
- [ ] manager ได้รับ notification ว่าต้อง manual quote
- [ ] Quote log บันทึกว่า requires manual

---

## TC-006: Missing Customer Contact (ข้อมูลติดต่อลูกค้าไม่ครบ)

**วัตถุประสงค์:** ตรวจสอบว่าระบบ handle ข้อมูลติดต่อว่างได้

| รายการ | ค่า |
|--------|-----|
| Customer Name | บริษัททดสอบ |
| Contact | (ว่าง) |
| Origin | TH |
| Destination | JP |
| Shipment Type | Air |

**ผลที่คาดหวัง:**
- Workflow ยังทำงานต่อได้ — ไม่ crash
- Quote log บันทึกด้วย contact = ว่าง
- Notification แจ้ง manager ว่า contact ว่าง
- ไม่ส่งข้อความให้ลูกค้า (เพราะไม่มี contact)

**จุดที่ต้องตรวจสอบ:**
- [ ] ไม่มี crash/error ที่ทำให้ workflow หยุด
- [ ] Quote ยังถูก log
- [ ] Manager notification ระบุว่า contact ว่าง

---

## TC-007: USD_THB Not Configured (env var ไม่ได้ตั้งค่า)

**วัตถุประสงค์:** ตรวจสอบว่าระบบไม่ทำงานโดยไม่มี USD_THB

**ผลที่คาดหวัง:**
- `requiresManualQuote = true`
- `error = "USD_THB rate not configured. Set USD_THB environment variable."`
- `totalPrice = 0`
- ห้ามคำนวณราคาเลย

**จุดที่ต้องตรวจสอบ:**
- [ ] ตรวจสอบว่าค่า usdThb > 0 ก่อนคำนวณ
- [ ] Error message ชัดเจน

---

## วิธีการทดสอบ

1. ตั้งค่า n8n test environment (แยกจาก production)
2. ตั้ง environment variables ทั้งหมดใน n8n
3. Import `n8n-workflow.json` เข้า test environment
4. ส่ง POST request ไปที่ webhook URL พร้อมข้อมูลทดสอบแต่ละ TC
5. ตรวจสอบ QuoteLog ใน Google Sheets
6. ตรวจสอบ LINE Notify / Email notification
7. **ห้ามทดสอบโดยใช้ข้อมูลลูกค้าจริง**

---

> ✅ ทุก test case ต้องผ่านก่อนยืนยัน Go-Live สำหรับ Pilot V1
