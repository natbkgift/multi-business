# Risks and Safeguards | ความเสี่ยงและการป้องกัน

## Risk Register

| # | ความเสี่ยง | ระดับ | ความน่าจะเป็น | ผลกระทบ | การป้องกัน | Plan B |
|---|-----------|-------|-------------|---------|-----------|--------|
| 1 | ส่ง quote ผิดราคาอัตโนมัติ | 🔴 สูง | ต่ำ | สูงมาก | **Human Approval บังคับ** ก่อนส่งทุกครั้ง | Manual review ทั้งหมด |
| 2 | LINE OA ตอบข้อมูลผิดพลาด | 🟡 กลาง | กลาง | กลาง | Confidence threshold < 80% → Fallback to human | Staff online ตลอดเวลาทำการ |
| 3 | ข้อมูล lead รั่วไหล | 🔴 สูง | ต่ำ | สูงมาก | ใช้ Google Workspace (ไม่เก็บข้อมูลใน 3rd party tools) | PDPA compliance review |
| 4 | n8n / Make.com ล่ม | 🟡 กลาง | ต่ำ-กลาง | กลาง | Email backup notification + Manual checklist พร้อม | ทำ manual แล้ว reprocess |
| 5 | Rate Table ไม่อัปเดต | 🔴 สูง | กลาง | สูง | Alert อัตโนมัติถ้าไม่ได้ update ใน 7 วัน | Double-check กับ supplier ก่อน approve |
| 6 | ทีมไม่ยอมใช้ระบบใหม่ | 🟡 กลาง | กลาง | กลาง | Training + ทำให้ง่ายกว่า manual workflow เดิม | 1-on-1 coaching + feedback loop |
| 7 | API costs เกินงบ | 🟢 ต่ำ | ต่ำ | ต่ำ | ตั้ง spending limit ใน OpenAI / Anthropic | ลด API calls, ใช้ cheaper model |
| 8 | Google Sheets Performance | 🟡 กลาง | ต่ำ | กลาง | ไม่เก็บ data > 10,000 rows ใน sheet เดียว | Archive monthly data |
| 9 | LINE OA Message Quota | 🟡 กลาง | กลาง | กลาง | Monitor monthly usage, upgrade plan ถ้าจำเป็น | ใช้ Email แทน |
| 10 | ลูกค้าได้รับ automation แล้วรู้สึกไม่ personalized | 🟢 ต่ำ | กลาง | ต่ำ-กลาง | เพิ่ม personalization ด้วยชื่อลูกค้า + context | Human follow-up ทุก high-value lead |

---

## Human Approval Gates (จุดบังคับก่อนส่งออก)

### ✋ Gate 1 — Freight Quotation
- **เมื่อไร:** ทุกครั้งก่อนส่ง quote ให้ลูกค้า
- **ใครอนุมัติ:** ผู้จัดการหรือ Operation Lead
- **ช่องทาง:** LINE Notify + Link ไป Google Sheets row
- **SLA:** ต้องอนุมัติภายใน 2 ชั่วโมง
- **ถ้าไม่อนุมัติใน SLA:** ระบบ escalate ไปยัง backup approver

### ✋ Gate 2 — Real Estate Lead Assignment
- **เมื่อไร:** ก่อน assign lead ให้ agent และส่ง welcome message
- **ใครอนุมัติ:** Sales Manager
- **ช่องทาง:** LINE Notify + HubSpot notification
- **SLA:** 30 นาที (ในเวลาทำการ)

### ✋ Gate 3 — Sauna Booking Confirmation
- **เมื่อไร:** ก่อนยืนยัน slot และส่ง coupon
- **ใครอนุมัติ:** Staff on duty
- **ช่องทาง:** LINE OA internal notification
- **SLA:** 15 นาที (ในเวลาเปิด)

### ✋ Gate 4 — Content Publishing
- **เมื่อไร:** ก่อน publish ทุก video / post
- **ใครอนุมัติ:** Content Manager / เจ้าของ
- **ช่องทาง:** Google Sheets queue + Email notification
- **SLA:** 24 ชั่วโมง

### ✋ Gate 5 — Rate Table Changes
- **เมื่อไร:** ก่อนแก้ไข rate ใดๆ ใน Rate Table
- **ใครอนุมัติ:** เจ้าของธุรกิจ
- **ช่องทาง:** Manual confirmation via LINE/Email
- **หมายเหตุ:** ต้องมี version history (Google Sheets version control)

---

## Data Privacy Safeguards

1. **ไม่เก็บข้อมูลลูกค้าใน n8n / Make.com** — ส่งผ่านแล้วเก็บใน Google Sheets เท่านั้น
2. **Google Sheets Access Control** — Share กับเฉพาะคนที่จำเป็น (ไม่ใช้ "Anyone with link")
3. **LINE User ID** — ใช้เป็น identifier เท่านั้น ไม่เก็บ personal data เกินความจำเป็น
4. **HubSpot** — ตั้ง Data Retention policy ตาม PDPA (ลบ data หลัง 2 ปีถ้าไม่มีการติดต่อ)
5. **API Keys** — เก็บใน n8n Credentials (ไม่ hardcode ใน workflow JSON)

---

## Monitoring และ Alerting

```
n8n Error Monitoring:
- ตั้ง Error Workflow ที่แจ้ง LINE Notify ทุกครั้ง workflow fail
- ตรวจ Execution Log ทุกเช้า

Make.com:
- เปิด Email notification เมื่อ scenario error
- ตั้ง incomplete execution alert

Google Sheets:
- ใช้ Google Apps Script ส่ง alert ถ้า Rate Table ไม่ได้ update ใน 7 วัน
- Script ตัวอย่าง:
  function checkRateTableUpdate() {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("RateTable");
    var lastUpdated = sheet.getRange("A1").getValue(); // ใส่วันที่ update ใน A1
    var daysSince = (new Date() - new Date(lastUpdated)) / (1000 * 60 * 60 * 24);
    if (daysSince > 7) {
      // Send LINE Notify alert
    }
  }
```
