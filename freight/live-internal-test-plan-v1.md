# Freight Pilot V1 — Live Internal Test Plan

> **เวอร์ชัน:** 1.0.0
> **วันที่:** 2026-05-05
> **สถานะเอกสาร:** INTERNAL TEST USE ONLY — ห้ามแชร์ภายนอก
> **ผู้รับผิดชอบ:** Freight Pilot Live Internal Test Coordinator

---

## 1. สถานะปัจจุบัน (Current Status)

| รายการ | สถานะ |
|--------|-------|
| Freight Pilot Fix V1 — Code Simulation QA | ✅ PASS (12/12 test cases) |
| Production Readiness | 🔴 NO-GO |
| Carrier-Verified Rates | 🔴 NO — ทุก route ใน RateTable มี `Rate Verified = NO` |
| n8n Live Execution | 🔴 ยังไม่ได้รัน — ต้องรันใน test environment ก่อน |
| Customer Quotation Auto-Send | 🔴 ล็อค — ต้องผ่าน manual approval เท่านั้น |

**ข้อสรุป:**
Simulation ผ่านทุก TC แล้ว แต่ยังไม่ได้รัน n8n จริง เป้าหมายของเอกสารนี้คือให้ผู้ดำเนินการ (Technical Operator) สามารถรัน workflow ใน n8n test environment และบันทึก evidence ได้โดยไม่ต้องเดาขั้นตอนใดๆ

> ⚠️ **กฎห้ามละเมิด:**
> - ห้ามใช้ข้อมูลลูกค้าจริงในการทดสอบ
> - ห้าม commit secrets จริงในโค้ด
> - ห้าม mark production-ready จนกว่าจะผ่านทุกเกณฑ์ในหัวข้อ 7
> - ห้าม auto-send quotation ให้ลูกค้า — ทุก output ต้องเป็น DRAFT จนกว่า manager จะ approve ด้วยตนเอง

---

## 2. Environment Variables ที่ต้องกำหนดใน n8n Test Instance

ตั้งค่า environment variables ทั้งหมดต่อไปนี้ใน n8n **test instance เท่านั้น** (ไม่ใช่ production)

| ชื่อตัวแปร | ประเภท | ตัวอย่างค่าทดสอบ | คำอธิบาย | หมายเหตุ |
|-----------|--------|-----------------|----------|---------|
| `USD_THB` | Number | `35.50` | อัตราแลกเปลี่ยน USD→THB สำหรับคำนวณราคา | **ต้องไม่ hardcode ใน workflow** |
| `DEFAULT_MARGIN_PERCENT` | Number | `15` | % margin เหนือต้นทุน freight | ค่า test อาจตั้งเป็น 0 เพื่อตรวจ base price |
| `QUOTE_VALIDITY_DAYS` | Number | `7` | จำนวนวันที่ quotation มีผล | ใช้ใน quotation template |
| `DOC_FEE` | Number | `500` | ค่า Documentation Fee (THB) | ห้าม hardcode 500 ใน workflow |
| `COMPANY_NAME` | String | `Test Freight Co. (TEST ENV)` | ชื่อบริษัทในเอกสาร quotation | ใส่คำว่า TEST ENV เพื่อแยกจาก production |
| `SALES_EMAIL` | String | `test-sales@internal.test` | อีเมล sales สำหรับ notification | ใช้ email ภายในทีมเท่านั้น — ห้ามใช้ email ลูกค้าจริง |
| `MANAGER_EMAIL` | String | `test-manager@internal.test` | อีเมล manager สำหรับ approval notification | ต้องเป็น inbox ที่ทีมเข้าถึงได้ |
| `GOOGLE_SHEET_ID` | String | `<ID ของ test sheet>` | Google Sheets ID สำหรับ test environment | **ต้องเป็น sheet แยกจาก production** |
| `LINE_NOTIFY_TOKEN` | String | `<token ทดสอบ>` | LINE Notify token (หรือช่องทาง notification อื่น) | ดูหัวข้อ 2.1 สำหรับทางเลือกอื่น |
| `APPROVAL_SECRET_TOKEN` | String | `test-approval-secret-2026` | Token สำหรับยืนยัน approval webhook | ใช้ค่าจำลอง — ห้ามใช้ production secret |

### 2.1 ทางเลือก Notification Channel (แทน LINE Notify)

หาก LINE Notify token ไม่พร้อม ให้เลือกช่องทางใดช่องทางหนึ่ง:

| ตัวเลือก | ตัวแปรที่เพิ่ม | คำอธิบาย |
|---------|--------------|---------|
| Email (SMTP) | ใช้ `MANAGER_EMAIL` + `SALES_EMAIL` | n8n มี Email node built-in — ส่ง notification ทาง email แทน |
| Slack | `SLACK_WEBHOOK_URL` | ใช้ Slack Incoming Webhook — ง่ายและตรวจสอบได้ |
| Mock / Log Only | ไม่ต้องเพิ่ม | disable notification node ชั่วคราว แต่ต้อง log ใน n8n execution แทน |

> **แนะนำสำหรับ test:** ใช้ Email (SMTP) เพื่อให้บันทึก evidence ได้ง่าย และไม่ต้องพึ่ง LINE API ภายนอก

### 2.2 วิธีตั้งค่า Environment Variables ใน n8n

1. เปิด n8n test instance → ไปที่ **Settings → Environment Variables** (หรือแก้ไขไฟล์ `.env` ของ n8n)
2. เพิ่มตัวแปรทุกตัวตามตารางข้างต้น
3. Restart n8n instance หลังเพิ่มตัวแปร
4. ตรวจสอบโดยใช้ n8n Expression `{{ $env.USD_THB }}` ใน Execute Workflow node

---

## 3. การตั้งค่า Google Sheets (Google Sheets Setup)

ต้องสร้าง Google Sheets ใหม่สำหรับ **test environment เท่านั้น** — ห้ามใช้ production sheet

### 3.1 โครงสร้าง Sheets

#### Sheet 1: `QuoteLog`

บันทึกทุก quotation ที่ workflow ประมวลผล

| คอลัมน์ | ประเภท | ตัวอย่าง | คำอธิบาย |
|--------|--------|---------|---------|
| `timestamp` | DateTime | `2026-05-05T10:00:00Z` | เวลาที่ workflow รัน |
| `quoteId` | String | `QT-20260505-001` | รหัส quotation (generate โดย workflow) |
| `customerName` | String | `บริษัท Test จำกัด` | ชื่อลูกค้า (ห้ามใช้ชื่อจริง) |
| `contact` | String | `test@internal.test` | ข้อมูลติดต่อ (อาจว่างสำหรับ TC-006) |
| `origin` | String | `TH` | ต้นทาง |
| `destination` | String | `CN` | ปลายทาง |
| `shipmentType` | String | `Air` | ประเภทการขนส่ง |
| `weightKg` | Number | `50` | น้ำหนัก (kg) |
| `cbm` | Number | `0.072` | ปริมาตร (CBM) |
| `chargeableWeight` | Number | `50` | น้ำหนักที่ใช้คิดราคา |
| `freightCharge` | Number | `9000` | ค่าขนส่ง (THB) |
| `docFee` | Number | `500` | ค่า doc fee (THB) |
| `totalPrice` | Number | `9500` | ราคารวม (THB) |
| `quoteStatus` | String | `DRAFT — Pending Internal Approval` | สถานะ |
| `requiresManualQuote` | Boolean | `false` | ต้องทำ manual quote หรือไม่ |
| `rateVerified` | String | `YES` | สถานะ Rate Verified จาก RateTable |
| `usdThb` | Number | `35.50` | อัตรา USD_THB ที่ใช้ |
| `executionId` | String | `n8n-exec-xxxxxxxx` | n8n Execution ID |
| `logReason` | String | `RATE_NOT_VERIFIED` | เหตุผล (กรณี manual quote) |

#### Sheet 2: `RateTable`

นำเข้าจาก `freight/rate-table.csv` — **ต้องเป็น copy ของ test sheet แยกต่างหาก**

| คอลัมน์สำคัญ | คำอธิบาย |
|-------------|---------|
| `Route` | เช่น `TH-CN`, `TH-US` |
| `Rate Verified` | ค่าเริ่มต้น = `NO` — เปลี่ยนเป็น `YES` เฉพาะ TC ที่ต้องการทดสอบคำนวณราคา |
| `Air Rate (THB/kg)` | ราคา Air per kg |
| `Sea FCL 20ft (USD)` | ราคา FCL 20ft per container |
| `Sea FCL 40ft (USD)` | ราคา FCL 40ft per container |
| `Sea LCL (USD/CBM)` | ราคา LCL per CBM |
| `Min Charge (THB)` | ค่าขนส่งขั้นต่ำ |

> ⚠️ **สำคัญ:** ก่อนรัน TC ที่ต้องคำนวณราคา (TC-001, TC-002, TC-003a, TC-003b, TC-006, TC-011) ให้เปลี่ยน `Rate Verified = YES` เฉพาะ route ที่ทดสอบใน **test sheet** เท่านั้น

#### Sheet 3: `Config`

เก็บ configuration ที่อาจ override ค่าจาก environment variable (optional)

| Key | Value (ตัวอย่าง) | คำอธิบาย |
|-----|----------------|---------|
| `ENV` | `TEST` | บ่งชี้ว่าเป็น environment ทดสอบ |
| `ALLOW_AUTO_SEND` | `FALSE` | ล็อค auto-send ไว้เสมอในช่วงทดสอบ |
| `TEST_MODE` | `TRUE` | ให้ workflow รู้ว่าอยู่ใน test mode |

#### Sheet 4: `ErrorLog`

บันทึก error ทุกอย่างที่เกิดใน workflow

| คอลัมน์ | ประเภท | คำอธิบาย |
|--------|--------|---------|
| `timestamp` | DateTime | เวลาที่เกิด error |
| `executionId` | String | n8n Execution ID |
| `errorCode` | String | เช่น `RATE_NOT_VERIFIED`, `USD_THB_MISSING`, `401_UNAUTHORIZED` |
| `errorMessage` | String | ข้อความ error เต็ม |
| `inputPayload` | String (JSON) | input ที่ส่งมา |
| `tcId` | String | Test Case ID (เช่น TC-001) |
| `resolvedBy` | String | ผู้แก้ไข (กรอกด้วยมือหลังทดสอบ) |

### 3.2 การตั้งค่า Google Sheets API ใน n8n

1. ไปที่ Google Cloud Console → สร้าง Service Account สำหรับ test environment
2. Share test sheet กับ Service Account email (Editor permission)
3. ใน n8n → **Credentials → Google Sheets OAuth2 / Service Account** → เพิ่ม credentials ใหม่
4. ตั้งค่า `GOOGLE_SHEET_ID` ใน environment variable ให้ตรงกับ test sheet
5. ทดสอบ connection ก่อนรัน TC จริง

---

## 4. Pre-Test Checklist (รายการตรวจสอบก่อนเริ่มทดสอบ)

ผู้ดำเนินการต้องตรวจสอบทุกรายการต่อไปนี้และทำเครื่องหมาย ✅ ก่อนเริ่ม TC ใดๆ

### 4.1 Environment Setup

- [ ] **n8n test instance แยกจาก production** — ยืนยัน URL ต่างกัน
- [ ] **Environment variables ครบทุกตัว** ตามหัวข้อ 2 — ตรวจสอบด้วย n8n Expression
- [ ] **`USD_THB` = 35.50** (ค่าทดสอบ) — verify ใน n8n console
- [ ] **`APPROVAL_SECRET_TOKEN`** ตั้งค่าแล้ว — จำค่าไว้สำหรับ TC-008, TC-009, TC-010a
- [ ] **`DOC_FEE` = 500** — verify ใน n8n console
- [ ] **`COMPANY_NAME`** มีคำว่า `TEST` เพื่อแยกจาก production document

### 4.2 Google Sheets

- [ ] **Test Google Sheet สร้างแล้ว** — Sheet ID บันทึกไว้ใน `GOOGLE_SHEET_ID`
- [ ] **Sheets ครบ 4 แผ่น:** QuoteLog, RateTable, Config, ErrorLog
- [ ] **RateTable นำเข้าจาก rate-table.csv แล้ว** — Rate Verified ทุก row = `NO` (ค่าเริ่มต้น)
- [ ] **n8n เชื่อมต่อ Google Sheets ได้** — ทดสอบด้วย Read node
- [ ] **ไม่มีข้อมูลลูกค้าจริงใน sheet ใดๆ**

### 4.3 Workflow Import

- [ ] **Import `freight/n8n-workflow.json`** เข้า test instance แล้ว
- [ ] **Workflow version = 1.1.0-pilot** — ตรวจสอบใน workflow header node
- [ ] **Webhook URL บันทึกไว้** สำหรับส่ง test payload
- [ ] **Approval webhook URL บันทึกไว้** สำหรับ TC-008, TC-009, TC-010a

### 4.4 Notification Channel

- [ ] **Email หรือ LINE Notify พร้อมใช้งาน** — ส่ง test notification ก่อนเริ่ม
- [ ] **`MANAGER_EMAIL` และ `SALES_EMAIL`** เป็น inbox ที่ทีมเข้าถึงได้
- [ ] **ยืนยันว่า notification ไม่ส่งออกนอกทีม**

### 4.5 Test Data

- [ ] **ใช้ข้อมูลลูกค้าจำลองทั้งหมด** — ห้ามใช้ชื่อ/email ลูกค้าจริง
- [ ] **บันทึก test payload ทุกชุดไว้** ก่อนส่ง (ดูตาราง Evidence หัวข้อ 6)
- [ ] **เตรียม HTTP client** (เช่น Postman, curl, Insomnia) สำหรับส่ง webhook request

### 4.6 Evidence Recording

- [ ] **เปิด n8n Execution Log ไว้** เพื่อบันทึก Execution ID
- [ ] **เปิด QuoteLog Sheet** พร้อมตรวจสอบหลังแต่ละ TC
- [ ] **เปิด ErrorLog Sheet** พร้อมตรวจสอบสำหรับ TC ที่คาดว่า error

---

## 5. Test Execution Matrix (ขั้นตอนการทดสอบแต่ละ TC)

> **คำแนะนำ:** รัน TC ตามลำดับ TC-001 → TC-011 เพื่อให้ตรวจสอบได้เป็นระบบ
> หลังแต่ละ TC: บันทึก Execution ID + ผลลัพธ์ใน Evidence Table (หัวข้อ 6)

---

### TC-001: Air Quote (TH-CN)

**วัตถุประสงค์:** ตรวจสอบการคำนวณ Air Quote end-to-end ใน n8n จริง

**การเตรียมก่อนรัน:**
1. เปิด test RateTable → row `TH-CN` → เปลี่ยน `Rate Verified` เป็น `YES`
2. ยืนยัน `USD_THB = 35.50` และ `DOC_FEE = 500` ใน n8n environment

**ขั้นตอนการรัน:**
1. ส่ง POST request ไปที่ Freight Quote Webhook URL พร้อม payload:
```json
{
  "origin": "TH",
  "destination": "CN",
  "shipment_type": "Air",
  "weight_kg": 50,
  "length_cm": 60,
  "width_cm": 40,
  "height_cm": 30,
  "customer_name": "บริษัท Test ABC จำกัด",
  "contact": "test-tc001@internal.test"
}
```
2. บันทึก n8n Execution ID จาก n8n execution log
3. ตรวจสอบ response JSON
4. ตรวจสอบ QuoteLog ใน Google Sheets
5. ตรวจสอบ notification ที่ MANAGER_EMAIL

**ผลที่คาดหวัง:**

| รายการ | ค่าที่คาดหวัง |
|--------|-------------|
| `totalPrice` | 9,500 THB |
| `freightCharge` | 9,000 THB |
| `docFee` | 500 THB |
| `quoteStatus` | `DRAFT — Pending Internal Approval` |
| `requiresManualQuote` | `false` |
| QuoteLog — status | `DRAFT` |
| Customer ได้รับ quote | ❌ ไม่ — ต้องรอ approval |
| Manager notification | ✅ ส่งแล้ว |

**การคืนค่า Rate Verified:** หลัง TC-001 เสร็จ → เปลี่ยน `TH-CN Rate Verified` กลับเป็น `NO`

---

### TC-002: Sea LCL Quote (TH-US)

**วัตถุประสงค์:** ตรวจสอบการคำนวณ LCL โดยใช้ USD_THB จาก env

**การเตรียมก่อนรัน:**
1. เปิด test RateTable → row `TH-US` → เปลี่ยน `Rate Verified` เป็น `YES`

**ขั้นตอนการรัน:**
1. ส่ง POST request:
```json
{
  "origin": "TH",
  "destination": "US",
  "shipment_type": "Sea LCL",
  "weight_kg": 500,
  "length_cm": 120,
  "width_cm": 80,
  "height_cm": 100,
  "customer_name": "บริษัท Test XYZ จำกัด",
  "contact": "test-tc002@internal.test"
}
```
2. บันทึก Execution ID
3. ตรวจสอบ response
4. ตรวจสอบ QuoteLog

**ผลที่คาดหวัง:**

| รายการ | ค่าที่คาดหวัง |
|--------|-------------|
| CBM | 0.96 |
| Chargeable Weight (LCL logic) | 0.96 CBM |
| `freightCharge` | 4,090 THB (0.96 × 120 × 35.50 = 4,089.60 → round up) |
| `totalPrice` | 4,590 THB |
| `quoteStatus` | `DRAFT — Pending Internal Approval` |
| `requiresManualQuote` | `false` |

**การคืนค่า Rate Verified:** เปลี่ยน `TH-US Rate Verified` กลับเป็น `NO`

---

### TC-003a: Sea FCL 20ft Quote (TH-SG)

**วัตถุประสงค์:** ตรวจสอบ FCL 20ft end-to-end

**การเตรียมก่อนรัน:**
1. เปิด test RateTable → row `TH-SG` → เปลี่ยน `Rate Verified` เป็น `YES`

**ขั้นตอนการรัน:**
1. ส่ง POST request:
```json
{
  "origin": "TH",
  "destination": "SG",
  "shipment_type": "Sea FCL 20ft",
  "customer_name": "บริษัท Test FCL จำกัด",
  "contact": "test-tc003a@internal.test"
}
```
2. บันทึก Execution ID

**ผลที่คาดหวัง:**

| รายการ | ค่าที่คาดหวัง |
|--------|-------------|
| `freightCharge` | 21,300 THB (600 USD × 35.50) |
| `totalPrice` | 21,800 THB |
| `quoteStatus` | `DRAFT — Pending Internal Approval` |
| `requiresManualQuote` | `false` |

---

### TC-003b: Sea FCL 40ft with Surcharge (TH-SG)

**วัตถุประสงค์:** ตรวจสอบ FCL 40ft และการคิด surcharge

**การเตรียมก่อนรัน:**
1. TH-SG ยังคง `Rate Verified = YES` จาก TC-003a (หรือตั้งใหม่)
2. (Optional) เพิ่ม `Fuel Surcharge` ใน TH-SG row เพื่อทดสอบ surcharge logic

**ขั้นตอนการรัน:**
1. ส่ง POST request:
```json
{
  "origin": "TH",
  "destination": "SG",
  "shipment_type": "Sea FCL 40ft",
  "customer_name": "บริษัท Test FCL Large จำกัด",
  "contact": "test-tc003b@internal.test"
}
```
2. บันทึก Execution ID

**ผลที่คาดหวัง:**

| รายการ | ค่าที่คาดหวัง |
|--------|-------------|
| `freightCharge` | 31,950 THB (900 USD × 35.50) |
| `totalPrice` | 32,450 THB (+ 500 doc fee) |
| `quoteStatus` | `DRAFT — Pending Internal Approval` |
| Surcharge (ถ้ามี) | คิดแยกจาก base rate |

**การคืนค่า Rate Verified:** เปลี่ยน `TH-SG Rate Verified` กลับเป็น `NO`

---

### TC-004: Unknown Route (TH-BR)

**วัตถุประสงค์:** ตรวจสอบว่า workflow handle เส้นทางที่ไม่มีใน RateTable อย่างปลอดภัย

**การเตรียมก่อนรัน:** ไม่ต้องเปลี่ยน Rate Verified — route TH-BR ไม่มีใน table

**ขั้นตอนการรัน:**
1. ส่ง POST request:
```json
{
  "origin": "TH",
  "destination": "BR",
  "shipment_type": "Air",
  "weight_kg": 100,
  "length_cm": 50,
  "width_cm": 50,
  "height_cm": 50,
  "customer_name": "บริษัท Test Unknown จำกัด",
  "contact": "test-tc004@internal.test"
}
```
2. บันทึก Execution ID

**ผลที่คาดหวัง:**

| รายการ | ค่าที่คาดหวัง |
|--------|-------------|
| `requiresManualQuote` | `true` |
| `totalPrice` | `0` |
| `error` | `"No rate found for route: TH-BR"` |
| QuoteLog — status | `DRAFT` + manual required |
| Workflow crash | ❌ ไม่ crash |
| Customer ได้รับราคา | ❌ ไม่ |
| Manager notification | ✅ แจ้ง manual quote required |

---

### TC-005: Land Shipment Without Rate (TH-MY)

**วัตถุประสงค์:** ตรวจสอบว่าไม่มีการใช้ hardcoded land rate

**การเตรียมก่อนรัน:** ไม่ต้องเปลี่ยน Rate Verified

**ขั้นตอนการรัน:**
1. ส่ง POST request:
```json
{
  "origin": "TH",
  "destination": "MY",
  "shipment_type": "Land",
  "weight_kg": 200,
  "customer_name": "บริษัท Test Land จำกัด",
  "contact": "test-tc005@internal.test"
}
```
2. บันทึก Execution ID

**ผลที่คาดหวัง:**

| รายการ | ค่าที่คาดหวัง |
|--------|-------------|
| `requiresManualQuote` | `true` |
| `totalPrice` | `0` |
| `error` | `"Land shipment: no verified rate available. Requires manual quote."` |
| ไม่มีการคำนวณ `* 15` หรือค่า default | ✅ ต้องยืนยัน |

---

### TC-006: Missing Customer Contact (TH-JP)

**วัตถุประสงค์:** ตรวจสอบว่า workflow ไม่ crash เมื่อ contact ว่าง และ log ได้ถูกต้อง

**การเตรียมก่อนรัน:**
1. เปิด test RateTable → row `TH-JP` → เปลี่ยน `Rate Verified` เป็น `YES`

**ขั้นตอนการรัน:**
1. ส่ง POST request (contact ว่าง):
```json
{
  "origin": "TH",
  "destination": "JP",
  "shipment_type": "Air",
  "weight_kg": 30,
  "length_cm": 40,
  "width_cm": 30,
  "height_cm": 25,
  "customer_name": "บริษัท Test NoContact จำกัด",
  "contact": ""
}
```
2. บันทึก Execution ID

**ผลที่คาดหวัง:**

| รายการ | ค่าที่คาดหวัง |
|--------|-------------|
| Workflow crash | ❌ ไม่ crash |
| `quoteStatus` | `DRAFT — Pending Internal Approval` |
| QuoteLog contact field | ว่าง / empty string |
| Manager notification | ✅ ระบุว่า contact ว่าง |
| ส่ง quote ให้ลูกค้า | ❌ ไม่ (ไม่มี contact) |

**การคืนค่า Rate Verified:** เปลี่ยน `TH-JP Rate Verified` กลับเป็น `NO`

---

### TC-007: USD_THB Invalid / Not Configured

**วัตถุประสงค์:** ตรวจสอบว่าระบบหยุดทำงานเมื่อ USD_THB = 0 หรือไม่ได้ตั้งค่า

**การเตรียมก่อนรัน:**
1. เปลี่ยน `USD_THB` ใน n8n environment เป็น `0` (หรือลบออกชั่วคราว)
2. Restart n8n instance

**ขั้นตอนการรัน:**
1. ส่ง POST request:
```json
{
  "origin": "TH",
  "destination": "CN",
  "shipment_type": "Air",
  "weight_kg": 50,
  "length_cm": 60,
  "width_cm": 40,
  "height_cm": 30,
  "customer_name": "บริษัท Test NoRate จำกัด",
  "contact": "test-tc007@internal.test"
}
```
2. บันทึก Execution ID

**ผลที่คาดหวัง:**

| รายการ | ค่าที่คาดหวัง |
|--------|-------------|
| `requiresManualQuote` | `true` |
| `totalPrice` | `0` |
| `error` | `"USD_THB rate not configured. Set USD_THB environment variable."` |
| การคำนวณราคา | ❌ ไม่เกิดขึ้น |

**หลัง TC-007:** คืนค่า `USD_THB = 35.50` และ restart n8n

---

### TC-008: Approval Token Missing (Approval Webhook)

**วัตถุประสงค์:** ตรวจสอบว่า approval webhook ปฏิเสธ request ที่ไม่มี token

**การเตรียมก่อนรัน:** ไม่ต้องเปลี่ยนค่าใดๆ — ใช้ workflow ปกติ

**ขั้นตอนการรัน:**
1. ส่ง POST request ไปที่ **Approval Webhook URL** (ไม่ใช่ Quote Webhook):
```bash
curl -X POST https://<n8n-test-url>/webhook/freight-quote-approved \
  -H "Content-Type: application/json" \
  -d '{"quoteId": "QT-20260505-TEST"}'
```
*(ไม่ส่ง `x-approval-token` header และไม่มี `approvalToken` ใน body)*

2. บันทึก Execution ID

**ผลที่คาดหวัง:**

| รายการ | ค่าที่คาดหวัง |
|--------|-------------|
| HTTP Status | 401 (หรือ workflow error) |
| `error` | `"401 Unauthorized: Invalid or missing approval token. Request rejected — customer quote NOT sent."` |
| Customer ได้รับ quote | ❌ ไม่ |
| Customer send node execute | ❌ ไม่ถูก execute |

---

### TC-009: Approval Token Invalid

**วัตถุประสงค์:** ตรวจสอบว่า approval webhook ปฏิเสธ token ที่ผิด

**ขั้นตอนการรัน:**
1. ส่ง POST request ไปที่ Approval Webhook URL พร้อม token ผิด:
```bash
curl -X POST https://<n8n-test-url>/webhook/freight-quote-approved \
  -H "Content-Type: application/json" \
  -H "x-approval-token: wrong-token-99999" \
  -d '{"quoteId": "QT-20260505-TEST"}'
```
2. บันทึก Execution ID

**ผลที่คาดหวัง:**

| รายการ | ค่าที่คาดหวัง |
|--------|-------------|
| `error` | `"401 Unauthorized"` |
| Customer ได้รับ quote | ❌ ไม่ |
| Customer send node execute | ❌ ไม่ถูก execute |

---

### TC-010a: Approval Token Valid (Rate Verified = NO → Blocked)

**วัตถุประสงค์:** ตรวจสอบว่าแม้ token ถูกต้อง แต่ถ้า Rate Verified = NO ยังบล็อกอยู่

**การเตรียมก่อนรัน:** ตรวจสอบว่า TH-CN ใน RateTable ยัง `Rate Verified = NO`

**ขั้นตอนการรัน:**
1. ส่ง POST request สร้าง quote ก่อน (TH-CN, Rate Verified = NO) — ได้ DRAFT ที่ manual quote
2. จากนั้น ส่ง approval request พร้อม token ถูกต้อง:
```bash
curl -X POST https://<n8n-test-url>/webhook/freight-quote-approved \
  -H "Content-Type: application/json" \
  -H "x-approval-token: test-approval-secret-2026" \
  -d '{"quoteId": "<quoteId จาก step 1>"}'
```
3. บันทึก Execution ID

**ผลที่คาดหวัง:**

| รายการ | ค่าที่คาดหวัง |
|--------|-------------|
| `logReason` | `RATE_NOT_VERIFIED` |
| Customer ได้รับ quote | ❌ ไม่ |
| `requiresManualQuote` | `true` |

---

### TC-010b: Approval Token Valid (Rate Verified = YES → Draft Only)

**วัตถุประสงค์:** ตรวจสอบว่าเมื่อ token ถูก + Rate Verified = YES ได้ DRAFT (ไม่ส่งลูกค้าอัตโนมัติ)

**การเตรียมก่อนรัน:**
1. เปลี่ยน TH-CN `Rate Verified = YES` ใน test sheet
2. ส่ง quote request สร้าง DRAFT ก่อน — บันทึก quoteId

**ขั้นตอนการรัน:**
1. ส่ง approval request พร้อม token ถูกต้อง:
```bash
curl -X POST https://<n8n-test-url>/webhook/freight-quote-approved \
  -H "Content-Type: application/json" \
  -H "x-approval-token: test-approval-secret-2026" \
  -d '{"quoteId": "<quoteId จาก DRAFT>"}'
```

**ผลที่คาดหวัง:**

| รายการ | ค่าที่คาดหวัง |
|--------|-------------|
| Quote status update | `APPROVED — Pending Send` หรือ manager-triggered send |
| Customer auto-send | ❌ ต้องไม่ส่งอัตโนมัติ — ต้องมี manual trigger |
| `quoteStatus` | ยังคงเป็น DRAFT หรือ pending ตามที่ workflow กำหนด |

---

## 6. Evidence Table (ตารางบันทึก Evidence)

กรอกตารางนี้ทันทีหลังรันแต่ละ TC — ห้ามกรอกล่วงหน้าหรือเดา

| Test ID | Input Payload (สรุป) | Expected Result | Actual Result | n8n Execution ID | Pass/Fail | Notes |
|---------|---------------------|-----------------|---------------|-----------------|-----------|-------|
| TC-001 | TH-CN, Air, 50kg, Rate Verified=YES | totalPrice=9,500 THB, DRAFT | _(กรอกหลังรัน)_ | _(กรอกหลังรัน)_ | _(กรอกหลังรัน)_ | |
| TC-002 | TH-US, Sea LCL, 500kg, 0.96 CBM, Rate Verified=YES | totalPrice=4,590 THB, DRAFT | | | | |
| TC-003a | TH-SG, FCL 20ft, Rate Verified=YES | totalPrice=21,800 THB, DRAFT | | | | |
| TC-003b | TH-SG, FCL 40ft, Rate Verified=YES | totalPrice=32,450 THB, DRAFT | | | | |
| TC-004 | TH-BR, Air, ไม่มีใน RateTable | requiresManualQuote=true, totalPrice=0 | | | | |
| TC-005 | TH-MY, Land, 200kg | requiresManualQuote=true, totalPrice=0 | | | | |
| TC-006 | TH-JP, Air, contact='', Rate Verified=YES | DRAFT, ไม่ crash, manager notified | | | | |
| TC-007 | TH-CN, Air, USD_THB=0 | requiresManualQuote=true, totalPrice=0, error | | | | |
| TC-008 | Approval webhook, ไม่มี token | 401 Unauthorized, customer ไม่ได้รับ | | | | |
| TC-009 | Approval webhook, token ผิด | 401 Unauthorized, customer ไม่ได้รับ | | | | |
| TC-010a | Approval webhook, token ถูก, Rate Verified=NO | Blocked — RATE_NOT_VERIFIED | | | | |
| TC-010b | Approval webhook, token ถูก, Rate Verified=YES | DRAFT only — ไม่ auto-send | | | | |

**คำแนะนำการกรอก:**
- **n8n Execution ID:** คัดลอกจาก n8n → Executions → รายการล่าสุด
- **Actual Result:** ระบุค่าที่ได้จริง เช่น `totalPrice=9,500 THB, quoteStatus=DRAFT`
- **Pass/Fail:** ✅ PASS หรือ ❌ FAIL
- **Notes:** ระบุความแตกต่างจาก expected result หรือข้อสังเกตพิเศษ

---

## 7. เกณฑ์ Go/No-Go สำหรับ Internal Pilot (Internal Pilot Go/No-Go Criteria)

### เกณฑ์ GO ✅ (ต้องผ่านทุกข้อ)

- [ ] **TC-001 ถึง TC-007 ทุก case: Pass** — ผล Actual Result ตรงกับ Expected Result ทุกข้อ
- [ ] **TC-008, TC-009: Approval webhook ปฏิเสธ unauthorized request** — ได้ 401 จริงใน n8n execution
- [ ] **TC-010a: Rate Verified = NO ถูก block** — ยืนยันใน n8n execution log
- [ ] **TC-010b: Rate Verified = YES ได้ DRAFT — ไม่ auto-send** — ยืนยันว่า customer ไม่ได้รับ
- [ ] **QuoteLog บันทึกทุก TC** — ครบทุก field ตามโครงสร้าง
- [ ] **ErrorLog บันทึก error cases** — TC-004, TC-005, TC-007, TC-008, TC-009, TC-010a
- [ ] **Manager notification ส่งสำหรับทุก case** ที่ต้อง manual หรือ approval
- [ ] **ไม่มี customer auto-send เกิดขึ้น** ตลอดการทดสอบ
- [ ] **Evidence Table กรอกครบทุก row** พร้อม n8n Execution ID จริง

### เกณฑ์ NO-GO ❌ (ถ้าพบข้อใดข้อหนึ่ง → หยุดทันที)

- ❌ TC ใดๆ FAIL — ต้องแก้ไข workflow และรันใหม่
- ❌ Customer ได้รับ quote โดยไม่มี manual approval
- ❌ USD_THB hardcoded ใน workflow (ไม่ใช่ env var)
- ❌ Approval webhook ยอมรับ request ที่ไม่มี token หรือ token ผิด
- ❌ Rate Verified = NO แต่ workflow ยังคำนวณราคา
- ❌ Workflow crash (unhandled exception) ใน TC ใดๆ
- ❌ n8n Execution ID ว่าง (ไม่สามารถ audit ได้)
- ❌ Evidence Table ไม่ครบ หรือกรอกโดยไม่ได้รัน n8n จริง

---

## 8. Production Blockers ที่ยังค้างอยู่ (Production Blockers)

เหตุผลเหล่านี้ทำให้ **ไม่สามารถ mark production-ready** ได้แม้ว่า internal test จะ pass

| # | Blocker | รายละเอียด | วิธีแก้ |
|---|---------|-----------|--------|
| B1 | **Rate ยังไม่ได้ Carrier-Verified** | ทุก route ใน RateTable มี `Rate Verified = NO` — ราคายังไม่ได้ confirm จาก carrier จริง | ติดต่อ carrier แต่ละเจ้า รับ quote sheet อย่างเป็นทางการ แล้วกรอก Verified By + Verified Date ใน RateTable |
| B2 | **ยังไม่ได้รัน n8n จริง** | ผ่านเฉพาะ simulation — ยังไม่มี evidence จาก n8n execution จริง | รัน TC-001 ถึง TC-010b ตามแผนนี้และบันทึก Evidence Table |
| B3 | **LINE Notify Token** | ถ้าใช้ LINE Notify ต้องทดสอบ token จริงในสภาพแวดล้อม production | เตรียม token จาก LINE Notify สำหรับ production channel |
| B4 | **APPROVAL_SECRET_TOKEN ยังเป็นค่า test** | ค่าใน env ปัจจุบันเป็น placeholder | Generate secret ที่แข็งแกร่ง (min 32 chars) สำหรับ production — อย่า commit ใน Git |
| B5 | **Google Sheet ID สำหรับ production** | ยังไม่ได้สร้าง production sheet ที่ผ่านการทดสอบ | สร้าง production sheet พร้อม access control ที่เหมาะสม |
| B6 | **Customer Notification Channel** | ยังไม่ได้ confirm channel ที่ใช้ส่ง quote ให้ลูกค้าจริง | กำหนด channel (Email, LINE, WhatsApp) + template ที่ approved |
| B7 | **Rate Validity Period** | Rate ที่จะ verify จะมีวันหมดอายุ — ต้องมีกระบวนการ re-verify | กำหนด SOP re-verification (เช่น ทุก 30 วัน) |

---

## 9. ข้อเสนอแนะสุดท้าย (Final Recommendation)

### สรุปสถานะ

```
Freight Pilot Fix V1
├── Code Simulation QA:    ✅ PASS (12/12)
├── n8n Live Execution:    🔵 PENDING — ต้องรันตามแผนนี้
├── Carrier-Verified Rates:🔴 NOT DONE — Blocker B1
└── Production Readiness:  🔴 NO-GO
```

### ลำดับขั้นตอนต่อไป

1. **ทันที:** ตั้งค่า n8n test environment ตามหัวข้อ 2 และ 3
2. **ทันที:** รัน TC-001 ถึง TC-010b ตามลำดับ — กรอก Evidence Table ทุก row
3. **ถ้า Internal Test Pass ทุก TC:** ประชุม Go/No-Go กับ manager
4. **ขนาน:** เริ่มกระบวนการ carrier rate verification (Blocker B1)
5. **หลัง B1 แก้ไขแล้ว:** รัน Live Test รอบที่ 2 พร้อม rate จริง
6. **เมื่อผ่านทุก blocker:** อัปเดตเอกสาร `freight/pilot-report-v1.md` และขอ production approval

> **คำเตือนสำคัญ:** ห้าม mark production-ready, ห้ามส่ง quotation ให้ลูกค้า, และห้ามใช้ข้อมูลลูกค้าจริงจนกว่า Blocker B1 และ B2 จะได้รับการแก้ไขและ Evidence Table จะกรอกครบถ้วนด้วย n8n Execution ID จริง

---

*เอกสารนี้จัดทำโดย Freight Pilot Live Internal Test Coordinator*
*เวอร์ชัน: 1.0.0 | วันที่: 2026-05-05 | สถานะ: INTERNAL TEST USE ONLY*
