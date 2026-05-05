# Freight Pilot QA Audit V1

**วันที่ตรวจสอบ:** 2026-05-05
**ผู้ตรวจสอบ:** Freight Automation QA Auditor
**ขอบเขต:** freight/ เท่านั้น
**Workflow Version:** 1.1.0-pilot

---

## 1. Executive Verdict (สรุปผลการตรวจสอบ)

| ประเภท | ผลการตัดสิน |
|--------|------------|
| **Internal Supervised Test** | ✅ **READY** — พร้อมทดสอบภายใน (มีเงื่อนไข: ต้องตั้ง env vars ครบก่อน) |
| **Production Go-Live** | 🔴 **NOT READY** — มี 3 blockers ที่ต้องแก้ก่อน |

> Workflow ผ่านการตรวจสอบ logic การคำนวณและ draft-only safety gate แต่ยังมีช่องโหว่ด้าน security (approval webhook ไม่มี auth) และ rate table ยังไม่ verified — ห้าม Go-Live จนกว่าจะแก้ครบ

---

## 2. Files Audited (ไฟล์ที่ตรวจสอบ)

| ไฟล์ | สถานะ |
|------|-------|
| `freight/n8n-workflow.json` | ✅ ตรวจสอบครบ |
| `freight/rate-table.csv` | ✅ ตรวจสอบครบ |
| `freight/quotation-template.md` | ✅ ตรวจสอบครบ |
| `freight/config-template.csv` | ✅ ตรวจสอบครบ |
| `freight/error-log-template.csv` | ✅ ตรวจสอบครบ |
| `freight/test-cases.md` | ✅ ตรวจสอบครบ |
| `freight/quote-log-template.csv` | ✅ ตรวจสอบครบ |
| `freight/pilot-report-v1.md` | ✅ ตรวจสอบครบ |
| `freight/README.md` | ✅ ตรวจสอบครบ |

---

## 3. JSON Validation Result

```
python -c "import json; json.load(open('freight/n8n-workflow.json')); print('VALID')"
→ VALID JSON ✅
```

**ผล:** `freight/n8n-workflow.json` เป็น JSON ที่ถูกต้อง — import เข้า n8n ได้

---

## 4. Pricing Logic Audit

### 4.1 USD/THB Hardcode — ✅ PASS

| รายการตรวจสอบ | ผล |
|--------------|-----|
| `* 36` ปรากฏใน jsCode | ❌ ไม่พบ — ถูกลบออกแล้ว |
| `$env.USD_THB` ถูกใช้แทน | ✅ พบ |
| Guard `usdThb <= 0` → `requiresManualQuote: true` | ✅ พบ |
| Error message ชัดเจน | ✅ `"USD_THB rate not configured. Set USD_THB environment variable."` |

### 4.2 Land Rate Hardcode — ✅ PASS

| รายการตรวจสอบ | ผล |
|--------------|-----|
| `chargeableWeight * 15` ปรากฏใน jsCode | ❌ ไม่พบ — ถูกลบออกแล้ว |
| Land shipment → `requiresManualQuote: true` เสมอ | ✅ พบ |
| Error message ชัดเจน | ✅ `"Land shipment: no verified rate available. Requires manual quote."` |

### 4.3 Unknown Shipment Type — ✅ PASS

| รายการตรวจสอบ | ผล |
|--------------|-----|
| มี `else` handler สำหรับ type ที่ไม่รู้จัก | ✅ พบ |
| Unknown type → `requiresManualQuote: true` | ✅ พบ |
| `totalPrice = 0` | ✅ พบ |

### 4.4 USD_THB ≤ 0 Guard — ✅ PASS

```javascript
const usdThb = parseFloat($env.USD_THB || '0');
if (usdThb <= 0) {
  return [{ json: { ...quoteData, requiresManualQuote: true, totalPrice: 0, ... } }];
}
```
✅ Confirmed: หาก USD_THB ไม่ได้ตั้งค่าหรือ ≤ 0 จะหยุดคำนวณทันที

### 4.5 Rate Verified Check ใน Calculation Logic — ⚠️ GAP พบ

| รายการตรวจสอบ | ผล |
|--------------|-----|
| `Rate Verified` column มีใน rate-table.csv | ✅ พบ |
| Workflow ตรวจสอบ `Rate Verified === 'YES'` ก่อนใช้ rate | ❌ **ไม่พบ** |
| **ความเสี่ยง:** rate ที่ยัง `NO` ถูกนำไปคำนวณโดยไม่มีการ block | 🟡 MEDIUM |

> ⚠️ แม้ rate-table.csv จะมี column `Rate Verified` แต่ jsCode ใน `Calculate Price` node ไม่ได้ตรวจสอบ field นี้ก่อนคำนวณ ในทาง logic จึงสามารถใช้ rate ที่ยังไม่ verified ในการออก DRAFT quote ได้ สำหรับ internal test ยังยอมรับได้ แต่ต้องแก้ก่อน production

### 4.6 Surcharge Application — ✅ PASS

```javascript
const fuelSurcharge = parseFloat(rateRow['Fuel Surcharge (USD/CBM)'] || '0');
const baf = parseFloat(rateRow['BAF (USD/CBM)'] || '0');
const otherSurcharge = parseFloat(rateRow['Other Surcharge (USD)'] || '0');
if (fuelSurcharge > 0) surchargeTotal += fuelSurcharge * cbm * usdThb;
```
✅ Confirmed: surcharge ถูก apply เฉพาะจาก numeric column และเฉพาะเมื่อ > 0 เท่านั้น

### 4.7 DOC_FEE — ✅ PASS (แก้ไขแล้วใน audit review)

```javascript
const docFee = parseFloat($env.DOC_FEE || '500');
```
✅ ย้ายออกจาก hardcode เป็น environment variable แล้ว (fallback 500 THB)

---

## 5. Draft-Only / Human Approval Audit

### 5.1 DRAFT Status Logged — ✅ PASS

| รายการตรวจสอบ | ผล |
|--------------|-----|
| `quoteStatus = "DRAFT — Pending Internal Approval"` ใน Calculate Price | ✅ ทุก return path |
| `Quote Status` column ถูก log ใน QuoteLog sheet | ✅ พบใน Log to Quote Sheet node |
| `Approval Status = "Pending"` ถูก log | ✅ พบ |

### 5.2 Quotation Template — ✅ PASS

- ✅ DRAFT watermark ที่ต้น template: `"DRAFT QUOTATION — PENDING INTERNAL APPROVAL"`
- ✅ คำเตือนภาษาไทย + อังกฤษ
- ✅ ช่อง `Approved by:` และ `Date:` ต้องกรอกด้วยมือ
- ✅ คำเตือนซ้ำท้าย template

### 5.3 Approval Flow Architecture — ✅ PASS (with caveat)

```
[Draft Path — Auto]
Webhook(quote-request) → Normalize → Get Rate → Calculate → Log(DRAFT) → Notify Manager

[Send Path — Manual trigger only]
Webhook(quote-approved) → Build Customer Msg → Send to Customer (LINE) → Update Sheet
```

✅ การส่งข้อความหาลูกค้า **แยก webhook** ออกจาก draft path
✅ ลูกค้าไม่ได้รับข้อความโดยอัตโนมัติจาก draft path
⚠️ แต่ approval webhook **ไม่มี authentication** (ดูหัวข้อ 6)

### 5.4 Missing Contact Handling — ⚠️ PARTIAL

| รายการตรวจสอบ | ผล |
|--------------|-----|
| Workflow ไม่ crash เมื่อ contact ว่าง | ✅ ไม่พบ explicit crash handler แต่ค่าเริ่มต้นเป็น `''` |
| Quote ยังถูก log เมื่อ contact ว่าง | ✅ ไม่มีการ validate contact ก่อน log |
| Manager notification แจ้งว่า contact ว่าง | ❌ **ไม่พบ** explicit warning ใน notification message |

> ⚠️ TC-006 (Missing Contact) จะ log ค่าว่างได้โดยไม่ crash แต่ manager notification ไม่แจ้งเตือนว่า contact ว่าง เป็น LOW risk สำหรับ internal test

---

## 6. Approval Webhook Security Audit

### ผลการตรวจสอบ — 🔴 BLOCKING

```json
"node-approval-webhook": {
  "parameters": {
    "httpMethod": "POST",
    "path": "freight-quote-approved",
    "responseMode": "onReceived",
    "options": {}
  }
}
```

| รายการตรวจสอบ | ผล |
|--------------|-----|
| มี `authentication` field | ❌ ไม่พบ |
| มี header validation | ❌ ไม่พบ |
| มี IP whitelist | ❌ ไม่พบ |
| URL คาดเดาได้ | 🔴 `/freight-quote-approved` — path ตรงไปตรงมา |

### ความเสี่ยง

> 🔴 **CRITICAL:** ใครก็ตามที่รู้ webhook URL สามารถ POST ข้อมูลปลอมและ trigger การส่ง quote หาลูกค้าโดยไม่ผ่าน approval จริง — นี่คือ blocker ที่ต้องแก้ก่อน Go-Live ทั้งหมด

### คำแนะนำ: Minimal Token-Based Protection

เพิ่ม Header Authentication ใน n8n webhook node:

```
Authentication: Header Auth
Header Name: X-Approval-Secret
Header Value: {{ $env.APPROVAL_SECRET_TOKEN }}
```

และเพิ่ม env var ใหม่:

| Env Var | ประเภท | คำอธิบาย |
|---------|--------|----------|
| `APPROVAL_SECRET_TOKEN` | string | Secret token สำหรับ approval webhook — ต้องไม่ commit ลง git |

หลังเพิ่ม auth แล้ว approval script ต้องส่ง header:
```
POST /freight-quote-approved
X-Approval-Secret: <APPROVAL_SECRET_TOKEN>
```

---

## 7. Rate Verification Audit

### ผลการตรวจสอบ — 🔴 BLOCKING (สำหรับ Production)

| รายการตรวจสอบ | ผล |
|--------------|-----|
| `Rate Verified` column มีใน rate-table.csv | ✅ พบ |
| จำนวน route ทั้งหมด | 12 routes |
| Routes ที่ `Rate Verified = YES` | 0 (ศูนย์) |
| Routes ที่ `Rate Verified = NO` | 12 ทั้งหมด |
| Workflow block ไม่ให้ใช้ rate ที่ `NO` | ❌ **ไม่มี** |

### สรุป

> 🔴 Rate Table ทั้ง 12 route ยังไม่ได้รับการยืนยันจาก carrier จริง (`Rate Verified = NO` ทุกแถว) — ห้ามใช้ rate ชุดนี้ในการออก quote จริงหาลูกค้า

### คำแนะนำ

1. ก่อน production: verify rate กับ carrier จริงและเปลี่ยน `Rate Verified` เป็น `YES`
2. ใน V2: เพิ่ม check ใน `Calculate Price` jsCode:
   ```javascript
   if ((rateRow['Rate Verified'] || '').toString().trim().toUpperCase() !== 'YES') {
     return [{ json: { ...quoteData, requiresManualQuote: true,
       error: `Rate for ${route} not verified. Manual quote required.` } }];
   }
   ```

---

## 8. Environment Variable Audit

### รายการ Env Vars ที่จำเป็นทั้งหมด

| Env Var | ใช้ใน node | จำเป็น | ค่า Default | หมายเหตุ |
|---------|-----------|--------|------------|----------|
| `USD_THB` | Calculate Price | YES | ไม่มี (block ถ้า 0) | อัปเดตทุกวัน |
| `QUOTE_VALIDITY_DAYS` | Calculate Price | YES | fallback 30 | |
| `DOC_FEE` | Calculate Price | YES | fallback 500 THB | |
| `GOOGLE_SHEET_ID` | Get Rate Table, Log, Update, Follow-up | YES | ไม่มี | |
| `SALES_EMAIL` | Email - Human Approval Backup | YES | ไม่มี | |
| `MANAGER_EMAIL` | Email - Human Approval Backup | YES | ไม่มี | |
| `LINE_NOTIFY_TOKEN` | LINE Notify nodes | YES | ไม่มี | ตั้งผ่าน n8n Credentials |
| `APPROVAL_SECRET_TOKEN` | Approval Webhook (แนะนำ) | YES | **ยังไม่มี** | ต้องเพิ่ม |

### ผลการตรวจสอบ

| รายการ | ผล |
|--------|-----|
| ไม่มี secret ถูก commit ลงใน git | ✅ ไม่พบ credential จริงใน repository |
| ไม่มี placeholder เหลือใน workflow JSON | ✅ ไม่พบ `YOUR_*` |
| Env vars ทั้งหมดมีเอกสารใน config-template.csv | ✅ (ยกเว้น APPROVAL_SECRET_TOKEN ที่ยังไม่ได้เพิ่ม) |
| APPROVAL_SECRET_TOKEN ถูก document | ❌ ยังไม่มีใน config-template.csv |

---

## 9. Test Case Coverage

| TC | กรณีทดสอบ | มีใน test-cases.md | ผล Expected ถูกต้อง | สถานะ |
|----|-----------|------------------|--------------------|-------|
| TC-001 | Air Quote | ✅ | ✅ คำนวณถูก + DRAFT | ⬜ ยังไม่รัน |
| TC-002 | Sea LCL Quote | ✅ | ✅ ใช้ USD_THB จาก env | ⬜ ยังไม่รัน |
| TC-003 | Sea FCL 20ft / 40ft | ✅ | ✅ ตรวจ "20" vs "40" | ⬜ ยังไม่รัน |
| TC-004 | Unknown Route | ✅ | ✅ requiresManualQuote = true | ⬜ ยังไม่รัน |
| TC-005 | Land Without Rate | ✅ | ✅ ไม่มี * 15 | ⬜ ยังไม่รัน |
| TC-006 | Missing Contact | ✅ | ⚠️ ไม่ครอบคลุม missing contact warning ใน notification | ⬜ ยังไม่รัน |
| TC-007 | USD_THB Not Configured | ✅ | ✅ usdThb ≤ 0 guard | ⬜ ยังไม่รัน |

**ข้อสังเกต TC-003:** ผลที่คาดหวังใน test-cases.md ยังคำนวณ doc fee เป็น 500 THB แบบ hardcode แต่จริงๆ ตอนนี้อ่านจาก `$env.DOC_FEE || '500'` — ต้องอัปเดตหมายเหตุใน TC-003 ให้ระบุว่า doc fee มาจาก env var

---

## 10. Critical Blockers (ต้องแก้ก่อน Production ทุกข้อ)

| # | Blocker | ประเภท | วิธีแก้ |
|---|---------|--------|--------|
| **B1** | Approval webhook (`freight-quote-approved`) ไม่มี authentication | 🔴 SECURITY | เพิ่ม Header Auth + `APPROVAL_SECRET_TOKEN` env var |
| **B2** | Rate Table ทั้ง 12 route มี `Rate Verified = NO` | 🔴 DATA INTEGRITY | Verify rate กับ carrier จริง ก่อนใช้งาน |
| **B3** | `Calculate Price` ไม่ตรวจสอบ `Rate Verified` field ก่อนคำนวณ | 🔴 LOGIC GAP | เพิ่ม check `Rate Verified !== 'YES'` → `requiresManualQuote: true` |
| **B4** | Test cases TC-001 ถึง TC-007 ยังไม่ได้รันจริง | 🔴 VALIDATION | รัน test ครบ 7 cases ใน n8n test instance ก่อน Go-Live |

---

## 11. High / Medium / Low Issues

### 🟠 HIGH

| # | รายการ | คำอธิบาย |
|---|--------|----------|
| H1 | `APPROVAL_SECRET_TOKEN` ไม่มีใน config-template.csv | ผู้ติดตั้งไม่ทราบว่าต้องตั้ง env var นี้ |
| H2 | Follow-up reminder ส่งผ่าน LINE Notify เท่านั้น (ไม่มี Email fallback) | LINE Notify อาจ deprecate |

### 🟡 MEDIUM

| # | รายการ | คำอธิบาย |
|---|--------|----------|
| M1 | USD_THB ต้องอัปเดตด้วยมือ — ไม่มี alert หากลืม | quote ราคาผิดหากค่าเงินเคลื่อนไหวมาก |
| M2 | Missing contact ไม่ทำให้ manager notification แสดงคำเตือน | manager อาจส่ง approved ไปโดยไม่มีช่องทางติดต่อลูกค้า |
| M3 | ไม่มี retry logic เมื่อ Google Sheets หรือ LINE ล้มเหลว | quote อาจหาย log |
| M4 | TC-003 expected result ยังอ้าง doc fee 500 แบบ hardcode | อาจทำให้ผู้ทดสอบสับสน |

### 🟢 LOW

| # | รายการ | คำอธิบาย |
|---|--------|----------|
| L1 | quotation-template.md ยังมี `[ชื่อธนาคาร]` placeholder ที่ไม่ใช่ env var | เป็น template — OK สำหรับ manual fill |
| L2 | `transitDays` ใน quotation template ไม่ถูก inject อัตโนมัติ | ต้องกรอกมือ |
| L3 | `COMPANY_NAME` อยู่ใน config-template.csv แต่ไม่ถูกใช้ใน n8n workflow | ใช้แค่ใน template manual |

---

## 12. Internal Test Go / No-Go

### ✅ INTERNAL TEST: **GO** (มีเงื่อนไข)

**เงื่อนไขก่อน Start Internal Test:**
- [ ] ตั้ง environment variables ครบทุกตัวใน n8n test instance (`USD_THB`, `QUOTE_VALIDITY_DAYS`, `DOC_FEE`, `GOOGLE_SHEET_ID`, `SALES_EMAIL`, `MANAGER_EMAIL`, `LINE_NOTIFY_TOKEN`)
- [ ] ใช้ Google Sheets สำหรับ test เท่านั้น (ไม่ใช่ production sheet)
- [ ] ใช้ LINE Notify channel สำหรับ test เท่านั้น
- [ ] **ห้ามใช้ข้อมูลลูกค้าจริงในการทดสอบ**
- [ ] ทีมเข้าใจว่า rate ยังไม่ verified — output เป็น DRAFT เพื่อ training เท่านั้น

**ข้อกำหนดระหว่าง Internal Test:**
- รัน TC-001 ถึง TC-007 และบันทึกผล
- บันทึก error ทุกข้อใน `error-log-template.csv`
- ห้าม promote ผล quote ใดๆ เป็น final ก่อน production blockers ทั้งหมดถูกปิด

---

## 13. Production Go / No-Go

### 🔴 PRODUCTION: **NO-GO**

**ยังไม่สามารถ Go-Live ในระดับ production ได้ เนื่องจาก:**

| Blocker | สถานะ |
|---------|-------|
| B1: Approval webhook ไม่มี authentication | ❌ ยังไม่แก้ |
| B2: Rate Table ทั้งหมด Rate Verified = NO | ❌ ยังไม่แก้ |
| B3: Workflow ไม่ block unverified rates | ❌ ยังไม่แก้ |
| B4: Test cases ยังไม่ผ่านการรัน | ❌ ยังไม่แก้ |

---

## 14. Required Fixes Before Production

### [BLOCKING] ต้องทำก่อน Production ทุกข้อ

#### Fix 1: เพิ่ม Auth บน Approval Webhook
เปลี่ยน `node-approval-webhook` ใน `n8n-workflow.json`:
```json
"parameters": {
  "httpMethod": "POST",
  "path": "freight-quote-approved",
  "authentication": "headerAuth",
  "responseMode": "onReceived"
}
```
และเพิ่ม credential สำหรับ Header Auth ใน n8n โดยใช้ `$env.APPROVAL_SECRET_TOKEN`

#### Fix 2: เพิ่ม Rate Verified Check ใน Calculate Price Node
ใส่ code ต่อไปนี้ใน `Calculate Price` jsCode หลังจากหา `rateRow`:
```javascript
if ((rateRow['Rate Verified'] || '').toString().trim().toUpperCase() !== 'YES') {
  return [{
    json: {
      ...quoteData,
      error: `Rate for route ${route} is not verified. Manual quote required.`,
      requiresManualQuote: true,
      freightCharge: 0,
      docFee: 0,
      otherCharges: 0,
      totalPrice: 0,
      quoteStatus: 'DRAFT — Pending Internal Approval'
    }
  }];
}
```

#### Fix 3: Verify Rate Table กับ Carrier
- ติดต่อ carrier สำหรับทุก route ที่จะใช้จริง
- อัปเดต rate ใน rate-table.csv (หรือ Google Sheets `RateTable`)
- เปลี่ยน `Rate Verified` เป็น `YES` พร้อมวันที่ verify
- Rate ต้องมีอายุไม่เกิน 30 วัน

#### Fix 4: เพิ่ม APPROVAL_SECRET_TOKEN ใน config-template.csv
เพิ่มบรรทัด:
```
APPROVAL_SECRET_TOKEN,,string,YES,Secret token สำหรับ approval webhook header auth — ห้าม commit ลง git,<generate-random-32-char-string>
```

#### Fix 5: รัน Test Cases ครบทุกข้อ
- รัน TC-001 ถึง TC-007 ใน n8n test environment
- บันทึกผลจริงเทียบกับ expected ใน `test-cases.md`
- ผ่านทุก TC ก่อนยืนยัน Go-Live

---

## สรุปตัวชี้วัด Pilot Readiness

| ด้าน | คะแนน | หมายเหตุ |
|------|-------|----------|
| JSON Validity | 5/5 | ✅ |
| Pricing Logic Safety | 4/5 | ⚠️ ไม่ block unverified rate |
| Draft-Only Enforcement | 5/5 | ✅ |
| Approval Architecture | 3/5 | 🔴 webhook ไม่มี auth |
| Environment Variable Design | 4/5 | ⚠️ APPROVAL_SECRET_TOKEN ยังไม่ document |
| Rate Table Safety | 2/5 | 🔴 ทั้งหมด NO |
| Test Coverage (documentation) | 5/5 | ✅ |
| Test Coverage (executed) | 0/5 | ❌ ยังไม่ได้รัน |
| **รวม** | **28/40** | — |

---

*รายงานนี้จัดทำโดย Freight Automation QA Auditor — 2026-05-05*
*ตรวจสอบโดยอ้างอิง: freight/n8n-workflow.json v1.1.0-pilot, freight/pilot-report-v1.md*
