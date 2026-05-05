# Freight Pilot Fix V1 — รายงานสถานะ

**วันที่จัดทำ:** 2026-05-05
**สถานะ Pilot:** Draft-Only Supervised Mode
**ขอบเขต:** freight/ เท่านั้น (ไม่รวม real-estate/, sauna/, content/, sop/)

---

## 1. ไฟล์ที่เปลี่ยนแปลง

| ไฟล์ | การเปลี่ยนแปลง |
|------|----------------|
| `freight/n8n-workflow.json` | แก้ไข Calculate Price, Email node, LINE Notify, Google Sheet IDs, meta |
| `freight/rate-table.csv` | เพิ่ม surcharge fields + คำเตือน pilot |
| `freight/quotation-template.md` | เพิ่ม DRAFT watermark + แก้ QUOTE_VALIDITY_DAYS |
| `freight/config-template.csv` | **สร้างใหม่** — environment variable reference |
| `freight/error-log-template.csv` | **สร้างใหม่** — error logging structure |
| `freight/test-cases.md` | **สร้างใหม่** — test case documentation |
| `freight/pilot-report-v1.md` | **สร้างใหม่** — รายงานนี้ |

---

## 2. การแก้ไขสำคัญ (Critical Fixes)

### ✅ ลบ hardcoded USD/THB = 36
- **เดิม:** `* 36` ใน Calculate Price node
- **ใหม่:** `const usdThb = parseFloat($env.USD_THB || '0');` — อ่านจาก environment variable
- หากไม่ได้ตั้งค่า `USD_THB`: `requiresManualQuote = true`, `totalPrice = 0`

### ✅ ลบ hardcoded land rate
- **เดิม:** `freightCharge = chargeableWeight * 15` (fabricated land rate)
- **ใหม่:** Land shipments → `requiresManualQuote = true` เสมอ

### ✅ เพิ่ม unknown shipment type handler
- หาก shipmentType ไม่ตรงกับ air/fcl/lcl/land → `requiresManualQuote = true`

### ✅ เปลี่ยน placeholder เป็น environment variables
| Placeholder เดิม | Environment Variable ใหม่ |
|-----------------|--------------------------|
| `YOUR_GOOGLE_SHEET_ID` | `$env.GOOGLE_SHEET_ID` |
| `your-email@gmail.com` | `$env.SALES_EMAIL` |
| `manager@yourcompany.com` | `$env.MANAGER_EMAIL` |
| `YOUR_LINE_NOTIFY_TOKEN` | `$env.LINE_NOTIFY_TOKEN` |
| hardcoded `30` (validity days) | `$env.QUOTE_VALIDITY_DAYS` |

---

## 3. Config Fields ที่เพิ่ม (freight/config-template.csv)

| Field | ประเภท | จำเป็น | คำอธิบาย |
|-------|--------|--------|----------|
| `USD_THB` | number | YES | อัตราแลกเปลี่ยน USD/THB — ต้องอัปเดตทุกวัน |
| `DEFAULT_MARGIN_PERCENT` | number | YES | เปอร์เซ็นต์กำไรอ้างอิง (ไม่ใช้ auto-calc) |
| `QUOTE_VALIDITY_DAYS` | number | YES | จำนวนวันที่ใบเสนอราคามีผล |
| `COMPANY_NAME` | string | YES | ชื่อบริษัทในใบเสนอราคา |
| `SALES_EMAIL` | string | YES | อีเมลผู้ส่ง |
| `MANAGER_EMAIL` | string | YES | อีเมลผู้อนุมัติ |
| `GOOGLE_SHEET_ID` | string | YES | ID ของ Google Sheets |
| `LINE_NOTIFY_TOKEN` | string | YES | Token LINE Notify สำหรับ approval |

---

## 4. การเปลี่ยนแปลง Pricing Logic

| รายการ | ก่อน | หลัง |
|--------|------|------|
| USD/THB rate | hardcode = 36 | อ่านจาก `$env.USD_THB` |
| Land rate | hardcode = 15 THB/kg | requiresManualQuote = true |
| Unknown route | error + manual | error + manual (ไม่เปลี่ยน) |
| Unknown shipment type | ไม่มี handler | requiresManualQuote = true |
| USD_THB = 0 หรือไม่ตั้งค่า | คำนวณผิด (× 0) | requiresManualQuote = true |
| Surcharges | ไม่มีการ apply | apply จาก numeric columns เท่านั้น |
| Validity days | hardcode = 30 | อ่านจาก `$env.QUOTE_VALIDITY_DAYS` |

---

## 5. Human Approval Gate

**การยืนยัน:** Workflow แบ่งเป็น 2 ส่วนที่แยกกัน:

### ส่วน 1 — Draft Generation (อัตโนมัติ)
```
Webhook → Normalize → Get Rate → Calculate → Log (DRAFT) → Notify Manager (LINE + Email)
```
- ทุก quote ถูก log ด้วย `Quote Status = "DRAFT — Pending Internal Approval"`
- ห้ามส่งข้อความให้ลูกค้าในส่วนนี้

### ส่วน 2 — Customer Send (ต้อง trigger ด้วยมือ)
```
Approval Webhook (POST manual) → Build Customer Message → Send to Customer → Update Sheet
```
- Webhook `freight-quote-approved` **ต้อง trigger ด้วยมือ** โดย manager
- Manager ต้องส่ง POST พร้อม `{ quoteId, approvedBy, customerContact, totalPrice, customerName }`

**ข้อควรระวัง:**
> ⚠️ Approval webhook (`freight-quote-approved`) ยังคงใช้งานอยู่ในไฟล์นี้ หากมีคนส่ง POST ไปโดยตรงระบบจะส่งข้อความให้ลูกค้าทันที ต้องควบคุม URL นี้ให้มี authentication ก่อน Go-Live

---

## 6. Test Cases ที่เพิ่ม

ดู `freight/test-cases.md` สำหรับรายละเอียดทั้งหมด

| TC | กรณีทดสอบ | สถานะ |
|----|-----------|-------|
| TC-001 | Air Quote (TH-CN) | ⬜ ยังไม่ทดสอบ |
| TC-002 | Sea LCL Quote (TH-US) | ⬜ ยังไม่ทดสอบ |
| TC-003 | Sea FCL 20ft / 40ft (TH-SG) | ⬜ ยังไม่ทดสอบ |
| TC-004 | Unknown Route (TH-BR) | ⬜ ยังไม่ทดสอบ |
| TC-005 | Land Shipment Without Rate | ⬜ ยังไม่ทดสอบ |
| TC-006 | Missing Customer Contact | ⬜ ยังไม่ทดสอบ |
| TC-007 | USD_THB Not Configured | ⬜ ยังไม่ทดสอบ |

---

## 7. ความเสี่ยงที่เหลือ (Remaining Risks)

| # | ความเสี่ยง | ระดับ | วิธี Mitigate |
|---|-----------|-------|---------------|
| R1 | Approval webhook URL ไม่มี authentication — ใครก็ POST ได้ | 🔴 HIGH | ใส่ secret header หรือ basic auth ก่อน Go-Live |
| R2 | Rate Table ยังไม่ verified (Rate Verified = NO ทุกแถว) | 🔴 HIGH | ต้อง verify rate กับ carrier จริงก่อน pilot |
| R3 | USD_THB ต้องอัปเดตด้วยมือทุกวัน — risk ราคาผิด | 🟡 MEDIUM | พิจารณา auto-fetch rate หรือตั้ง alert เตือน |
| R4 | LINE Notify อาจถูก deprecate — ใช้เป็น channel เดียวไม่ได้ | 🟡 MEDIUM | Email backup ถูกเพิ่มแล้ว ควรทดสอบ Email path ด้วย |
| R5 | ไม่มี retry logic เมื่อ Google Sheets หรือ LINE ล้มเหลว | 🟡 MEDIUM | เพิ่ม error handling node ใน V2 |
| R6 | Test cases ยังไม่ได้รัน — ยืนยันไม่ได้ว่าทำงานถูกต้อง | 🟡 MEDIUM | รัน TC-001 ถึง TC-007 ก่อน Go-Live |
| R7 | Doc fee = 500 THB ยัง hardcode ใน jsCode | 🟢 LOW | ย้ายเข้า config ใน V2 |
| R8 | Follow-up reminder ยังส่งทาง LINE Notify เท่านั้น | 🟢 LOW | เพิ่ม Email path ใน V2 |

---

## 8. Pilot Go / No-Go Recommendation

### ✅ สิ่งที่พร้อมแล้ว
- [x] ลบ hardcoded USD/THB = 36 แล้ว
- [x] ลบ fabricated land rate แล้ว
- [x] เพิ่ม environment variable pattern แล้ว
- [x] Quote ทุกใบถูก log ด้วย DRAFT status
- [x] DRAFT watermark ใน quotation template
- [x] Dual approval channel (LINE + Email)
- [x] config-template.csv พร้อม
- [x] error-log-template.csv พร้อม
- [x] test cases documentation พร้อม

### ❌ ต้องทำก่อน Go-Live
- [ ] **[BLOCKING]** ใส่ authentication บน approval webhook URL
- [ ] **[BLOCKING]** Verify rate ใน Rate Table กับ carrier จริง (ทุก route ที่จะใช้)
- [ ] **[BLOCKING]** ตั้งค่า environment variables ครบทุกตัวใน n8n production
- [ ] รัน test cases TC-001 ถึง TC-007 ให้ผ่านทั้งหมด
- [ ] ทดสอบ Email backup path

### คำแนะนำ

> **🟡 CONDITIONAL GO** — Workflow พร้อมสำหรับ internal test environment แต่ยัง **NO-GO** สำหรับ production จนกว่า blocking items ทั้ง 3 ข้อด้านบนจะเสร็จสิ้น

---

*รายงานนี้จัดทำโดย Freight Pilot Fix V1 Implementation — 2026-05-05*
