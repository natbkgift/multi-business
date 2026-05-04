# Training Modules | รายการ Module การฝึกอบรม

## รายการ Modules ทั้งหมด

| Module | หัวข้อ | ระยะเวลา | เหมาะสำหรับ | เนื้อหา |
|--------|--------|---------|------------|--------|
| M01 | ภาพรวมธุรกิจ 5 สาขา | 30 นาที | ทุกคน | อ่าน README.md + docs/architecture.md |
| M02 | Tool Stack & Systems | 45 นาที | ทุกคน | docs/tool-stack.md + ลอง login ทุก tool |
| M03 | KPI & Reporting | 30 นาที | ทุกคน | docs/kpi-dashboard.md |
| M04 | Weekly Checklist | 20 นาที | ทุกคน | docs/weekly-checklist.md |
| M05 | Freight - Overview | 45 นาที | Freight Team | freight/README.md |
| M06 | Freight - Quote Process | 60 นาที | Freight Team | ฝึก end-to-end quote flow |
| M07 | Freight - n8n Basics | 30 นาที | Freight Team | เข้าใจ workflow ไม่ต้อง code |
| M08 | Real Estate - Lead Gen | 45 นาที | Sales Team | real-estate/README.md + HubSpot |
| M09 | Real Estate - Lead Scoring | 30 นาที | Sales Team | real-estate/lead-scoring-formula.md |
| M10 | Real Estate - Follow-up SOP | 45 นาที | Sales Team | ฝึก follow-up sequence |
| M11 | Sauna - LINE OA | 45 นาที | Sauna Staff | sauna/README.md + LINE OA |
| M12 | Sauna - Booking Approval | 30 นาที | Sauna Staff | ฝึก approve booking |
| M13 | Sauna - Coupon System | 20 นาที | Sauna Staff | sauna/coupon-generator.md |
| M14 | Content - Calendar | 30 นาที | Content Team | content/README.md |
| M15 | Content - Script Review | 30 นาที | Content Team | ฝึก review + approve script |
| M16 | Risk & Safeguards | 30 นาที | ทุกคน | docs/risks-and-safeguards.md |
| M17 | Human Approval Gates | 20 นาที | ทุกคน | ทำ Quiz Approval Gates |

---

## วิธีสร้าง Quiz (ใช้ Google Forms)

### ตัวอย่างคำถาม Quiz M01 (ภาพรวมธุรกิจ):

**Q1:** ธุรกิจมีกี่สาขา?
- a) 3  b) 4  c) 5  d) 6
- **Answer: c) 5**

**Q2:** ระบบใดมีลำดับความสำคัญสูงสุด (ROI = 95)?
- a) Sauna  b) Freight Forwarding  c) Content  d) SOP
- **Answer: b) Freight Forwarding**

**Q3:** Human Approval ต้องทำก่อนอะไรบ้าง? (ตอบทุกข้อที่ถูก)
- a) ส่ง quotation  b) Publish content  c) ตอบ Line ลูกค้า  d) Assign lead ให้ agent
- **Answer: a, b, d**

---

## Track Progress ใน Google Sheets

สร้าง sheet "Training Progress" ด้วย columns:
```
Name | Role | Module | Completion Date | Score | Status (Pass/Fail/Pending) | Notes
```

ใช้ Google Apps Script ส่ง email reminder ถ้า Module ยังไม่ complete ใน 3 วัน:
```javascript
function checkTrainingProgress() {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Training Progress");
  var data = sheet.getDataRange().getValues();
  // ตรวจ Pending modules ที่เกิน 3 วัน → ส่ง email reminder
}
// Trigger: Time-driven → every day
```
