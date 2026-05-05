# Freight Pilot Fix V1 — Test Cases

> เอกสารนี้กำหนด test cases สำหรับ Freight Forwarding Quotation Workflow (Pilot V1)
> ทุก test case ต้องผ่านก่อน Go-Live
> ผลการ simulate ดูได้ที่ `freight/test-results-v1.md`

> ⚠️ **หมายเหตุสำหรับผู้ทดสอบ:** Rate Table ใน production ปัจจุบันมีทุก route เป็น `Rate Verified = NO`
> สำหรับ TC ที่ต้องการคำนวณราคาจริง (TC-001, TC-002, TC-003, TC-006, TC-011) ผู้ทดสอบต้องเปลี่ยน
> `Rate Verified` ใน Google Sheets test environment เป็น `YES` ก่อนรัน — **ห้ามแก้ใน production sheet**

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
| Rate Verified | YES (สำหรับ test เท่านั้น) |

**ผลที่คาดหวัง:**
- CBM = (60×40×30)/1,000,000 = 0.072
- Chargeable Weight = MAX(50, 0.072×167) = MAX(50, 12.024) = 50 kg
- Freight Charge = 50 × 180 = 9,000 THB (ใช้ Min Charge 500 เทียบแล้ว 9,000 > 500)
- Total = 9,000 + 500 (doc fee จาก env `DOC_FEE`) = 9,500 THB
- `requiresManualQuote = false`
- `quoteStatus = "DRAFT — Pending Internal Approval"`
- ห้ามส่งให้ลูกค้าอัตโนมัติ — ต้องรอ approval webhook + APPROVAL_SECRET_TOKEN

**จุดที่ต้องตรวจสอบ:**
- [x] USD_THB ไม่ได้ถูก hardcode ใน workflow (verified in code audit)
- [x] Rate Verified = YES ถูกตรวจสอบก่อนคำนวณ (added in B3 fix)
- [ ] Quote ถูก log ใน QuoteLog ด้วย status = DRAFT (ต้องรัน n8n จริง)
- [ ] LINE Notify / Email ส่ง notification ให้ manager (ต้องรัน n8n จริง)

**Run Status:** 🔵 SIMULATED — ดูผล simulate ใน test-results-v1.md TC-001

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
| Rate Verified | YES (สำหรับ test เท่านั้น) |

**ผลที่คาดหวัง:**
- CBM = (120×80×100)/1,000,000 = 0.96
- Chargeable Weight (LCL) = MAX(500/1000, 0.96) = MAX(0.5, 0.96) = 0.96
- Sea LCL Rate TH-US = 120 USD/CBM
- Freight = MAX(0.96 × 120 × 35.50, 800 min) = MAX(4,089.60, 800) = 4,090 THB (rounded up)
- Total = 4,090 + 500 = 4,590 THB
- `requiresManualQuote = false`
- `quoteStatus = "DRAFT — Pending Internal Approval"`

**จุดที่ต้องตรวจสอบ:**
- [x] ราคาใช้ USD_THB จาก environment variable เท่านั้น
- [x] ไม่มี * 36 ใน calculation
- [x] Rate Verified = YES ถูกตรวจสอบ

**Run Status:** 🔵 SIMULATED — ดูผล simulate ใน test-results-v1.md TC-002

---

## TC-003: Sea FCL Quote (ทดสอบ quotation ทะเล FCL)

**วัตถุประสงค์:** ตรวจสอบ FCL 20ft และ 40ft

| รายการ | ค่า (20ft) | ค่า (40ft) |
|--------|-----------|-----------|
| Origin | TH | TH |
| Destination | SG | SG |
| Shipment Type | Sea FCL 20ft | Sea FCL 40ft |
| USD_THB | 35.50 | 35.50 |
| Rate Verified | YES (test only) | YES (test only) |

**ผลที่คาดหวัง:**
- FCL 20ft TH-SG: MAX(600 × 35.50, 400 min) = MAX(21,300, 400) = 21,300 + 500 doc = 21,800 THB
- FCL 40ft TH-SG: MAX(900 × 35.50, 400 min) = MAX(31,950, 400) = 31,950 + 500 doc = 32,450 THB
- `requiresManualQuote = false`
- `quoteStatus = "DRAFT — Pending Internal Approval"`

**จุดที่ต้องตรวจสอบ:**
- [x] ราคาใช้ USD_THB จาก $env ไม่ใช่ hardcode 36
- [x] shipmentType matching ทำงานถูกต้อง ("20" vs "40")
- [x] DOC_FEE อ่านจาก $env.DOC_FEE ไม่ใช่ hardcode 500

**Run Status:** 🔵 SIMULATED — ดูผล simulate ใน test-results-v1.md TC-003

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
- [x] workflow ไม่ crash — return JSON ถูกต้อง
- [ ] manager ได้รับ notification ว่า manual quote required (ต้องรัน n8n จริง)
- [x] ไม่มีราคาส่งให้ลูกค้า (totalPrice = 0)

**Run Status:** 🔵 SIMULATED — ดูผล simulate ใน test-results-v1.md TC-004

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
- [x] ไม่มีการคำนวณ `* 15` หรือค่า default ใดๆ (verified in code audit)
- [ ] manager ได้รับ notification ว่าต้อง manual quote (ต้องรัน n8n จริง)
- [x] ผล totalPrice = 0 เสมอ

**Run Status:** 🔵 SIMULATED — ดูผล simulate ใน test-results-v1.md TC-005

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
| Rate Verified | YES (test only) |

**ผลที่คาดหวัง:**
- Workflow ยังทำงานต่อได้ — ไม่ crash
- Quote log บันทึกด้วย contact = ว่าง
- Notification แจ้ง manager ว่า contact ว่าง
- ไม่ส่งข้อความให้ลูกค้า (เพราะไม่มี contact)

**จุดที่ต้องตรวจสอบ:**
- [x] ไม่มี crash — normalize node ใช้ `''` เป็น default
- [ ] Quote ยังถูก log (ต้องรัน n8n จริง)
- [ ] Manager notification ระบุว่า contact ว่าง (ต้องรัน n8n จริง)

**Run Status:** 🔵 SIMULATED — ดูผล simulate ใน test-results-v1.md TC-006

---

## TC-007: USD_THB Not Configured (env var ไม่ได้ตั้งค่า)

**วัตถุประสงค์:** ตรวจสอบว่าระบบไม่ทำงานโดยไม่มี USD_THB

**ผลที่คาดหวัง:**
- `requiresManualQuote = true`
- `error = "USD_THB rate not configured. Set USD_THB environment variable."`
- `totalPrice = 0`
- ห้ามคำนวณราคาเลย

**จุดที่ต้องตรวจสอบ:**
- [x] ตรวจสอบว่าค่า usdThb > 0 ก่อนคำนวณ (verified in code audit)
- [x] Error message ชัดเจน

**Run Status:** 🔵 SIMULATED — ดูผล simulate ใน test-results-v1.md TC-007

---

## TC-008: Missing Approval Token (ทดสอบ approval webhook ไม่มี token)

**วัตถุประสงค์:** ตรวจสอบว่า approval webhook ปฏิเสธ request ที่ไม่มี token

| รายการ | ค่า |
|--------|-----|
| Endpoint | POST /freight-quote-approved |
| x-approval-token header | (ว่าง / ไม่ส่ง) |
| approvalToken in body | (ว่าง / ไม่ส่ง) |

**ผลที่คาดหวัง:**
- Workflow หยุด — throw error
- `error = "401 Unauthorized: Invalid or missing approval token. Request rejected — customer quote NOT sent."`
- ลูกค้าไม่ได้รับข้อความใดๆ
- Customer send node ไม่ถูก execute

**Run Status:** 🔵 SIMULATED — ดูผล simulate ใน test-results-v1.md TC-008

---

## TC-009: Invalid Approval Token (ทดสอบ approval webhook token ผิด)

**วัตถุประสงค์:** ตรวจสอบว่า approval webhook ปฏิเสธ token ที่ผิด

| รายการ | ค่า |
|--------|-----|
| Endpoint | POST /freight-quote-approved |
| x-approval-token header | `wrong-token-12345` |
| APPROVAL_SECRET_TOKEN env | `correct-secret-abc` |

**ผลที่คาดหวัง:**
- Workflow หยุด — throw error "401 Unauthorized"
- ลูกค้าไม่ได้รับข้อความ
- Customer send node ไม่ถูก execute

**Run Status:** 🔵 SIMULATED — ดูผล simulate ใน test-results-v1.md TC-009

---

## TC-010: Rate Verified = NO Blocked (ทดสอบการ block rate ที่ยังไม่ verified)

**วัตถุประสงค์:** ตรวจสอบว่า workflow ไม่คำนวณราคาเมื่อ Rate Verified ≠ YES

| รายการ | ค่า |
|--------|-----|
| Origin | TH |
| Destination | CN |
| Shipment Type | Air |
| Rate Verified (in sheet) | NO (สถานะปัจจุบัน) |

**ผลที่คาดหวัง:**
- `requiresManualQuote = true`
- `logReason = "RATE_NOT_VERIFIED"`
- `error` ระบุ route และสถานะ Rate Verified
- `totalPrice = 0`
- ลูกค้าไม่ได้รับ quote

**Run Status:** 🔵 SIMULATED — ดูผล simulate ใน test-results-v1.md TC-010

---

## TC-011: Rate Verified = YES Draft Calculation (ทดสอบเมื่อ rate ถูก verify แล้ว)

**วัตถุประสงค์:** ตรวจสอบว่าระบบคำนวณและออก DRAFT ได้เมื่อ Rate Verified = YES

| รายการ | ค่า |
|--------|-----|
| Origin | TH |
| Destination | JP |
| Shipment Type | Air |
| Weight (kg) | 30 |
| Dimensions (cm) | 50 × 40 × 30 |
| Rate Verified (in sheet) | YES |
| USD_THB | 35.50 |

**ผลที่คาดหวัง:**
- `requiresManualQuote = false`
- `quoteStatus = "DRAFT — Pending Internal Approval"`
- ราคาคำนวณได้ (ไม่ block)
- ยังไม่ส่งให้ลูกค้า — ต้องรอ approval

**Run Status:** 🔵 SIMULATED — ดูผล simulate ใน test-results-v1.md TC-011

---

## วิธีการทดสอบ

1. ตั้งค่า n8n test environment (แยกจาก production)
2. ตั้ง environment variables ทั้งหมดใน n8n รวมถึง `APPROVAL_SECRET_TOKEN`
3. Import `n8n-workflow.json` เข้า test environment
4. ส่ง POST request ไปที่ webhook URL พร้อมข้อมูลทดสอบแต่ละ TC
5. สำหรับ TC-001–TC-003 และ TC-006, TC-011: ต้องตั้ง Rate Verified = YES ใน Google Sheets ก่อนรัน
6. สำหรับ TC-008–TC-009: ทดสอบ approval webhook path แยกต่างหาก
7. ตรวจสอบ QuoteLog ใน Google Sheets
8. ตรวจสอบ LINE Notify / Email notification
9. **ห้ามทดสอบโดยใช้ข้อมูลลูกค้าจริง**

---

> ✅ ทุก test case ต้องผ่านก่อนยืนยัน Go-Live สำหรับ Pilot V1
> 📄 ดูผล simulation ทั้งหมดได้ที่ `freight/test-results-v1.md`
