# Freight Pilot Fix V1 — Test Results (Simulation Report)

**วันที่:** 2026-05-05
**Workflow Version:** 1.1.0-pilot (หลัง Production Blocker Fix)
**วิธีทดสอบ:** Deterministic Input/Output Simulation — จำลองผลจาก jsCode logic ตรงๆ
**หมายเหตุ:** การทดสอบนี้เป็น code-level simulation ไม่ใช่การรัน n8n จริง  
  ต้องรัน TC-001–TC-011 ใน n8n test instance จริงก่อน Go-Live

---

## สรุปผล

| TC | กรณีทดสอบ | ผล | Pass/Fail |
|----|-----------|-----|-----------|
| TC-001 | Air Quote (TH-CN, Rate Verified=YES) | ✅ totalPrice=9,500 THB, DRAFT | ✅ PASS |
| TC-002 | Sea LCL Quote (TH-US, Rate Verified=YES) | ✅ totalPrice=4,590 THB, DRAFT | ✅ PASS |
| TC-003a | Sea FCL 20ft (TH-SG, Rate Verified=YES) | ✅ totalPrice=21,800 THB, DRAFT | ✅ PASS |
| TC-003b | Sea FCL 40ft (TH-SG, Rate Verified=YES) | ✅ totalPrice=32,450 THB, DRAFT | ✅ PASS |
| TC-004 | Unknown Route (TH-BR) | ✅ requiresManualQuote=true | ✅ PASS |
| TC-005 | Land Shipment (TH-MY, Rate Verified=YES) | ✅ requiresManualQuote=true | ✅ PASS |
| TC-006 | Missing Contact (TH-JP, contact='') | ✅ คำนวณต่อ, DRAFT, ไม่ crash | ✅ PASS |
| TC-007 | USD_THB Not Configured (=0) | ✅ requiresManualQuote=true | ✅ PASS |
| TC-008 | Missing Approval Token | ✅ throw 401, ลูกค้าไม่ได้รับ | ✅ PASS |
| TC-009 | Invalid Approval Token | ✅ throw 401, ลูกค้าไม่ได้รับ | ✅ PASS |
| TC-010 | Rate Verified = NO Blocked | ✅ requiresManualQuote=true, RATE_NOT_VERIFIED | ✅ PASS |
| TC-011 | Rate Verified = YES Draft Calculation | ✅ คำนวณได้, DRAFT | ✅ PASS |

**ผลรวม: 12/12 rows PASS (Simulation)**
*(TC-003 แบ่งเป็น 2 sub-cases: 20ft และ 40ft = 12 rows รวม)*

---

## TC-001: Air Quote (TH-CN, Rate Verified = YES)

### Input Payload
```json
{
  "origin": "TH",
  "destination": "CN",
  "shipment_type": "Air",
  "weight_kg": 50,
  "length_cm": 60,
  "width_cm": 40,
  "height_cm": 30,
  "customer_name": "บริษัททดสอบ จำกัด",
  "contact": "test@company.com"
}
```

### Environment Variables (Simulated)
```
USD_THB = 35.50
DOC_FEE = 500
QUOTE_VALIDITY_DAYS = 30
```

### Rate Table Row (Google Sheets — simulated with Rate Verified = YES)
```
TH-CN | Air Rate = 180 THB/kg | Min Charge = 500 THB | Rate Verified = YES
```

### Step-by-Step Calculation
```
CBM = (60 × 40 × 30) / 1,000,000 = 0.072 m³
Chargeable Weight (Air) = MAX(50 kg, 0.072 × 167) = MAX(50, 12.024) = 50 kg
Freight Charge = 50 × 180 = 9,000 THB
MIN CHECK: MAX(9,000, 500 min) = 9,000 THB
Surcharge = 0 (all columns = 0)
Doc Fee = 500 THB ($env.DOC_FEE)
Total = CEIL(9,000 + 0 + 500) = 9,500 THB
```

### Expected Output
```json
{
  "requiresManualQuote": false,
  "freightCharge": 9000,
  "docFee": 500,
  "otherCharges": 0,
  "totalPrice": 9500,
  "cbm": 0.072,
  "chargeableWeight": 50,
  "quoteStatus": "DRAFT — Pending Internal Approval"
}
```

### Actual / Simulated Output
```json
{
  "requiresManualQuote": false,
  "totalPrice": 9500,
  "freightCharge": 9000,
  "docFee": 500,
  "cbm": 0.072,
  "chargeableWeight": 50,
  "quoteStatus": "DRAFT — Pending Internal Approval"
}
```

### ✅ PASS
**หมายเหตุ:** ต้องยืนยันว่า Quote Log บันทึกใน Google Sheets และ LINE Notify ส่งถึง manager ในการทดสอบจริง

---

## TC-002: Sea LCL Quote (TH-US, Rate Verified = YES)

### Input Payload
```json
{
  "origin": "TH",
  "destination": "US",
  "shipment_type": "Sea LCL",
  "weight_kg": 500,
  "length_cm": 120,
  "width_cm": 80,
  "height_cm": 100,
  "customer_name": "Test Exporter Co.",
  "contact": "export@test.co.th"
}
```

### Environment Variables
```
USD_THB = 35.50
DOC_FEE = 500
```

### Rate Table Row
```
TH-US | Sea LCL = 120 USD/CBM | Min Charge = 800 THB | Rate Verified = YES
```

### Step-by-Step Calculation
```
CBM = (120 × 80 × 100) / 1,000,000 = 0.96 m³
Chargeable Weight (LCL) = MAX(500/1000, 0.96) = MAX(0.5, 0.96) = 0.96
Freight Charge = MAX(0.96, 0.96) × 120 × 35.50 = 0.96 × 120 × 35.50 = 4,089.60 THB
MIN CHECK: MAX(4,089.60, 800) = 4,089.60 → CEIL = 4,090 THB
Doc Fee = 500 THB
Total = CEIL(4,090 + 500) = 4,590 THB
```

### Expected Output
```json
{
  "requiresManualQuote": false,
  "freightCharge": 4090,
  "docFee": 500,
  "totalPrice": 4590,
  "cbm": 0.96,
  "chargeableWeight": 0.96,
  "quoteStatus": "DRAFT — Pending Internal Approval"
}
```

### ✅ PASS

---

## TC-003a: Sea FCL 20ft (TH-SG, Rate Verified = YES)

### Input Payload
```json
{
  "origin": "TH",
  "destination": "SG",
  "shipment_type": "Sea FCL 20ft",
  "customer_name": "Container Test Co."
}
```

### Calculation
```
FCL 20ft TH-SG Rate = 600 USD
Freight = 600 × 35.50 = 21,300 THB
MIN CHECK: MAX(21,300, 400) = 21,300 THB
Doc Fee = 500 THB
Total = 21,300 + 500 = 21,800 THB
```

### Expected / Simulated Output
```json
{
  "requiresManualQuote": false,
  "freightCharge": 21300,
  "docFee": 500,
  "totalPrice": 21800,
  "quoteStatus": "DRAFT — Pending Internal Approval"
}
```

### ✅ PASS

---

## TC-003b: Sea FCL 40ft (TH-SG, Rate Verified = YES)

### Input Payload
```json
{
  "origin": "TH",
  "destination": "SG",
  "shipment_type": "Sea FCL 40ft"
}
```

### Calculation
```
FCL 40ft TH-SG Rate = 900 USD
Freight = 900 × 35.50 = 31,950 THB
MIN CHECK: MAX(31,950, 400) = 31,950 THB
Doc Fee = 500 THB
Total = 31,950 + 500 = 32,450 THB
```

### Expected / Simulated Output
```json
{
  "requiresManualQuote": false,
  "freightCharge": 31950,
  "docFee": 500,
  "totalPrice": 32450,
  "quoteStatus": "DRAFT — Pending Internal Approval"
}
```

### ✅ PASS

---

## TC-004: Unknown Route (TH-BR)

### Input Payload
```json
{
  "origin": "TH",
  "destination": "BR",
  "shipment_type": "Air",
  "weight_kg": 10
}
```

### Code Path
```
route = "TH-BR"
rateRow = rateRows.find(r => r['Route'] === 'TH-BR') → undefined
→ early return: No rate found
```

### Expected / Simulated Output
```json
{
  "requiresManualQuote": true,
  "totalPrice": 0,
  "error": "No rate found for route: TH-BR",
  "quoteStatus": "DRAFT — Pending Internal Approval"
}
```

### ✅ PASS
**หมายเหตุ:** ลูกค้าไม่ได้รับ quote, manager ได้รับ notification ว่า manual required (ต้องยืนยันใน n8n จริง)

---

## TC-005: Land Shipment (TH-MY)

### Input Payload
```json
{
  "origin": "TH",
  "destination": "MY",
  "shipment_type": "Land",
  "weight_kg": 200
}
```

### Code Path (Rate Verified = YES scenario)
```
route = "TH-MY"
rateRow found: Rate Verified = YES (passed)
shipmentType.includes('land') = true
→ early return: Land shipment no verified rate
```

### Expected / Simulated Output
```json
{
  "requiresManualQuote": true,
  "totalPrice": 0,
  "error": "Land shipment: no verified rate available. Requires manual quote.",
  "quoteStatus": "DRAFT — Pending Internal Approval"
}
```

### ✅ PASS
**หมายเหตุ:** ไม่มีการคำนวณ `chargeableWeight * 15` หรือค่า default ใดๆ

---

## TC-006: Missing Customer Contact (TH-JP, contact = '')

### Input Payload
```json
{
  "origin": "TH",
  "destination": "JP",
  "shipment_type": "Air",
  "weight_kg": 30,
  "length_cm": 50,
  "width_cm": 40,
  "height_cm": 30,
  "customer_name": "บริษัททดสอบ",
  "contact": ""
}
```

### Code Path
```
Normalize: contact = '' (ใช้ default '' — ไม่ crash)
Route TH-JP found: Rate Verified = YES
Air calculation:
  CBM = (50×40×30)/1,000,000 = 0.06
  CW = MAX(30, 0.06×167) = MAX(30, 10.02) = 30 kg
  Freight = 30 × 200 = 6,000 THB
  MIN CHECK: MAX(6,000, 500) = 6,000 THB
  Total = 6,000 + 500 = 6,500 THB
quoteStatus = DRAFT
```

### Expected / Simulated Output
```json
{
  "requiresManualQuote": false,
  "contact": "",
  "freightCharge": 6000,
  "docFee": 500,
  "totalPrice": 6500,
  "quoteStatus": "DRAFT — Pending Internal Approval"
}
```

### ✅ PASS
**หมายเหตุ:** Workflow ไม่ crash แม้ contact ว่าง — แต่ manager notification ยังไม่แสดง warning เมื่อ contact ว่าง (medium risk — แก้ใน V2)

---

## TC-007: USD_THB Not Configured (usdThb = 0)

### Input Payload
```json
{
  "origin": "TH",
  "destination": "CN",
  "shipment_type": "Air",
  "weight_kg": 50
}
```

### Environment Variables
```
USD_THB = (ไม่ได้ตั้งค่า — default = 0)
```

### Code Path
```
const usdThb = parseFloat($env.USD_THB || '0') = 0
if (usdThb <= 0) → early return
```

### Expected / Simulated Output
```json
{
  "requiresManualQuote": true,
  "totalPrice": 0,
  "error": "USD_THB rate not configured. Set USD_THB environment variable.",
  "quoteStatus": "DRAFT — Pending Internal Approval"
}
```

### ✅ PASS

---

## TC-008: Missing Approval Token

### Input Payload (POST to `/freight-quote-approved`)
```json
{
  "quoteId": "QT-20260505-1234",
  "approvedBy": "manager",
  "customerContact": "@line_user",
  "totalPrice": 9500,
  "customerName": "Test Customer"
}
```

### Headers
```
(ไม่มี x-approval-token header)
(ไม่มี approvalToken ใน body)
```

### Environment Variables
```
APPROVAL_SECRET_TOKEN = "my-secret-token-abc"
```

### Code Path (Validate Approval Token node)
```javascript
providedToken = '' (empty)
expectedToken = 'my-secret-token-abc'
providedToken !== expectedToken → throw Error('401 Unauthorized...')
// Execution HALTS here — Build Customer Message node never runs
// Customer quote NOT sent
```

### Expected Output (Error thrown — workflow halts)
```
Error: 401 Unauthorized: Invalid or missing approval token.
       Request rejected — customer quote NOT sent.
       Provide the correct token in header 'x-approval-token' or body field 'approvalToken'.
```

### Simulated Result
```json
{
  "halted": true,
  "reason": "Missing token",
  "error": "401 Unauthorized: Invalid or missing approval token. Request rejected — customer quote NOT sent.",
  "customerSent": false
}
```

### ✅ PASS

---

## TC-009: Invalid Approval Token

### Input Payload
```json
{
  "quoteId": "QT-20260505-1234",
  "approvedBy": "attacker",
  "customerContact": "@victim",
  "totalPrice": 0,
  "approvalToken": "wrong-token-12345"
}
```

### Headers
```
x-approval-token: wrong-token-12345
```

### Code Path
```javascript
providedToken = 'wrong-token-12345'
expectedToken = 'my-secret-token-abc'
'wrong-token-12345' !== 'my-secret-token-abc' → throw Error('401 Unauthorized...')
// Execution HALTS — no customer quote sent
```

### Simulated Result
```json
{
  "halted": true,
  "reason": "Wrong token",
  "error": "401 Unauthorized: Invalid or missing approval token. Request rejected — customer quote NOT sent.",
  "customerSent": false
}
```

### ✅ PASS

---

## TC-010: Rate Verified = NO Blocked

### Input Payload
```json
{
  "origin": "TH",
  "destination": "CN",
  "shipment_type": "Air",
  "weight_kg": 50,
  "length_cm": 60,
  "width_cm": 40,
  "height_cm": 30
}
```

### Rate Table Row (current production state)
```
TH-CN | Rate Verified = NO
```

### Code Path
```javascript
rateVerified = 'NO'
rateVerified !== 'YES' → early return with RATE_NOT_VERIFIED
```

### Expected / Simulated Output
```json
{
  "requiresManualQuote": true,
  "totalPrice": 0,
  "logReason": "RATE_NOT_VERIFIED",
  "error": "Rate for route TH-CN is not verified (Rate Verified = NO). Manual quote required.",
  "quoteStatus": "DRAFT — Pending Internal Approval"
}
```

### ✅ PASS
**หมายเหตุ:** นี่คือสถานะปัจจุบันของ production rate table — ทุก quote จะถูก block จนกว่าจะ verify กับ carrier จริง

---

## TC-011: Rate Verified = YES — Draft Calculation

### Input Payload
```json
{
  "origin": "TH",
  "destination": "JP",
  "shipment_type": "Air",
  "weight_kg": 30,
  "length_cm": 50,
  "width_cm": 40,
  "height_cm": 30,
  "customer_name": "Valid Test Co."
}
```

### Rate Table Row (simulated as verified)
```
TH-JP | Air Rate = 200 THB/kg | Min Charge = 500 THB | Rate Verified = YES
```

### Calculation
```
CBM = (50×40×30)/1,000,000 = 0.06
CW = MAX(30, 0.06×167) = MAX(30, 10.02) = 30 kg
Freight = 30 × 200 = 6,000 THB
MIN CHECK: MAX(6,000, 500) = 6,000 THB
Total = 6,000 + 500 = 6,500 THB
quoteStatus = DRAFT — ยังไม่ส่งลูกค้า
```

### Expected / Simulated Output
```json
{
  "requiresManualQuote": false,
  "freightCharge": 6000,
  "docFee": 500,
  "totalPrice": 6500,
  "cbm": 0.06,
  "chargeableWeight": 30,
  "quoteStatus": "DRAFT — Pending Internal Approval"
}
```

### ✅ PASS
**หมายเหตุ:** Quote ยังคงเป็น DRAFT — ลูกค้าไม่ได้รับจนกว่า manager จะ trigger approval webhook พร้อม valid token

---

## การตรวจสอบที่ต้องทำใน n8n จริง

ตารางด้านล่างนี้แสดงสิ่งที่ simulation ยืนยันไม่ได้ — ต้องรัน n8n จริงก่อน Go-Live:

| รายการ | TC | ต้องยืนยันด้วย |
|--------|-----|----------------|
| Quote log บันทึกใน Google Sheets | TC-001–TC-006, TC-011 | Google Sheets manual check |
| LINE Notify ส่งถึง manager | TC-001–TC-007 | LINE message received |
| Email backup ส่งสำเร็จ | TC-001–TC-007 | Email inbox check |
| n8n error log เมื่อ workflow halt | TC-008, TC-009 | n8n execution log |
| Customer LINE message received after valid approval | TC-011 | LINE message to test account |
| Google Sheet status updated to Approved | TC-011 | Google Sheets manual check |

---

## Validation Summary

| รายการ | ผล |
|--------|-----|
| ไม่มี `* 36` hardcode | ✅ Confirmed (code audit) |
| ไม่มี `chargeableWeight * 15` | ✅ Confirmed (code audit) |
| USD_THB ≤ 0 blocks calculation | ✅ TC-007 PASS |
| Rate Verified = NO blocks calculation | ✅ TC-010 PASS |
| Approval webhook requires token | ✅ TC-008, TC-009 PASS |
| Invalid token halts execution | ✅ TC-009 PASS |
| Customer quote never sent from draft path | ✅ Architecture confirmed |
| All quotes logged as DRAFT | ✅ All TC output confirmed |
| No real secrets in repository | ✅ Confirmed (grep scan) |

---

*รายงานนี้สร้างโดย Deterministic Simulation — 2026-05-05*  
*ต้องรัน TC ทั้งหมดใน n8n instance จริงก่อนยืนยัน Production Go-Live*
