# Weekly Execution Checklist | งานรายสัปดาห์

## ทุกวันจันทร์ (ใช้เวลา ~30 นาที)

- [ ] เปิด KPI Dashboard ใน Google Looker Studio — ตรวจสัปดาห์ที่ผ่านมา
- [ ] ตรวจ Freight Quote Log — quotes ที่ยังค้าง (Status = Draft / Pending)
- [ ] ตรวจ Real Estate Lead List — leads ที่ยังไม่ได้ติดตามใน 48 ชั่วโมง
- [ ] ตรวจ Sauna Booking Sheet — bookings สัปดาห์นี้ + reminder ส่งครบ?
- [ ] Assign งานทีมตามลำดับความสำคัญ

---

## ทุกวัน (ใช้เวลา ~15 นาที)

### เช้า (09:00)
- [ ] ตรวจ LINE Notify / Email — มี Approval requests รออนุมัติ?
- [ ] Approve หรือ Reject quotation drafts ที่รออนุมัติ
- [ ] ตรวจ n8n / Make.com Dashboard — มี workflow ล้มเหลว?

### บ่าย (14:00)
- [ ] ตอบ escalated queries ที่ chatbot ไม่สามารถจัดการได้
- [ ] ตรวจ Real Estate leads ใหม่ที่เข้ามาตั้งแต่เช้า

---

## ทุกวันศุกร์ (ใช้เวลา ~45 นาที)

- [ ] Review ผลประจำสัปดาห์ + บันทึก lessons learned
- [ ] อัปเดต Rate Table (Freight) ถ้ามีการเปลี่ยนแปลงจากซัพพลายเออร์
- [ ] ตรวจ Content Pipeline — content สำหรับสัปดาห์หน้าพร้อมหรือยัง?
- [ ] Backup Google Sheets (File → Download → .xlsx)
- [ ] ส่ง Weekly Summary ให้ทีมผ่าน LINE Group

---

## ทุกสิ้นเดือน (ใช้เวลา ~2 ชั่วโมง)

- [ ] สรุป KPI เปรียบเทียบกับเป้าหมาย
- [ ] Review n8n / Make.com — workflows ไหนมี error บ่อย?
- [ ] อัปเดต SOP ถ้ามี process เปลี่ยน
- [ ] ประเมิน tool costs vs ROI — ยกเลิก tools ที่ไม่ได้ใช้
- [ ] วางแผนเดือนถัดไปตามแผน 90-day roadmap

---

## Quick Reference: Approval SLA

| ระบบ | Approval Type | กำหนด |
|------|-------------|-------|
| Freight | Quote Approval | ภายใน 2 ชั่วโมงหลังแจ้ง |
| Real Estate | Lead Assignment | ภายใน 30 นาทีหลังรับ lead |
| Sauna | Booking Confirmation | ภายใน 15 นาทีในเวลาทำการ |
| Content | Script/Video Approval | ภายใน 24 ชั่วโมง |
| Rate Table | Rate Change Approval | Manual (ก่อน update) |

---

## ถ้า Automation ล้มเหลว (Fallback Procedure)

1. **n8n / Make.com ล่ม:**
   - ตรวจที่ [status.n8n.io](https://status.n8n.io) หรือ [status.make.com](https://status.make.com)
   - ดำเนิน manual process ชั่วคราว (ดู manual checklist ใน `/freight/README.md`)
   - แจ้งทีมใน LINE Group

2. **LINE OA ไม่ส่งข้อความ:**
   - ตรวจ Channel Access Token หมดอายุหรือไม่
   - Fallback: ใช้ Email แจ้งลูกค้าแทน

3. **Google Sheets ไม่ตอบสนอง:**
   - ตรวจ Google Workspace Status
   - ใช้ local Excel backup ชั่วคราว

4. **API Quota เต็ม (OpenAI / Google):**
   - ตรวจ usage ใน dashboard
   - สร้าง content manually สำหรับสัปดาห์นั้น
