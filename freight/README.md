# 🥇 Freight Forwarding Quotation Automation

ระบบ #1 ที่ควรทำก่อน — ROI สูงสุด สามารถ implement ได้ใน 10-14 วัน

## ภาพรวม

ระบบนี้จะช่วยให้คุณ:
- รับ quote request จาก Form / LINE / Messenger
- คำนวณราคาอัตโนมัติจาก Rate Table
- สร้าง PDF Quotation draft
- ส่งให้ผู้รับผิดชอบ **อนุมัติก่อนส่ง** (Human Approval Gate)
- ติดตาม status จนถึง Booking Confirmed

## ไฟล์ในโฟลเดอร์นี้

| ไฟล์ | วัตถุประสงค์ |
|------|------------|
| `n8n-workflow.json` | Import เข้า n8n ได้ทันที |
| `rate-table.csv` | ตัวอย่าง Rate Table — copy เข้า Google Sheets |
| `quote-log-template.csv` | โครงสร้าง Quote Log Sheet |
| `quotation-template.md` | Template เนื้อหาใบเสนอราคา |

## วิธี Setup (Step by Step)

### Step 1: Google Sheets
1. สร้าง Google Sheets ใหม่ชื่อ **"Freight Management"**
2. สร้าง 3 sheets: `QuoteLog`, `RateTable`, `KPIDashboard`
3. Copy header row จาก `quote-log-template.csv` เข้า sheet `QuoteLog`
4. Copy ข้อมูลจาก `rate-table.csv` เข้า sheet `RateTable`
5. กรอก rates จริงของคุณใน RateTable
6. แชร์ Google Sheet กับ Service Account email (จาก Google Cloud Console)

### Step 2: Google Form
สร้าง Google Form ด้วย fields เหล่านี้:
- ชื่อลูกค้า (Short answer)
- LINE ID หรือ Email (Short answer)
- ต้นทาง / Origin (Short answer)
- ปลายทาง / Destination (Short answer)
- ประเภทสินค้า (Short answer)
- น้ำหนัก / Weight kg (Number)
- ขนาด L×W×H cm (Short answer)
- ประเภทการขนส่ง (Multiple choice: Air / Sea FCL / Sea LCL / Land)
- หมายเหตุเพิ่มเติม (Paragraph)

เชื่อม Form Responses เข้า Google Sheet โดยอัตโนมัติ (Form → Responses → Link to Sheets)

### Step 3: n8n Setup
1. ไปที่ n8n instance ของคุณ
2. Import `n8n-workflow.json` (Menu → Import from file)
3. กำหนด Credentials:
   - **Google Sheets**: เพิ่ม Service Account credentials
   - **LINE Notify**: ใส่ LINE Notify Token
   - **Gmail/SMTP**: ใส่ Email credentials
4. อัปเดต Webhook URL ของ Google Form ใน workflow
5. Activate workflow

### Step 4: ทดสอบ
1. กรอก Google Form ด้วยข้อมูลทดสอบ
2. ตรวจดูว่า n8n ดึงข้อมูลได้ถูกต้อง
3. ตรวจ Quote Log ว่าบันทึกข้อมูลครบ
4. ตรวจ LINE Notify ว่าแจ้งเตือนมา
5. อนุมัติ → ตรวจว่าระบบส่ง quote ให้ลูกค้า

## Manual Fallback (ถ้าระบบล้มเหลว)

1. รับข้อมูลจากลูกค้าทาง LINE / โทรศัพท์
2. เปิด Google Sheet `QuoteLog` — เพิ่ม row ใหม่ด้วยตนเอง
3. ดู Rate Table — คำนวณ CBM และราคาด้วยตนเอง
4. Copy `quotation-template.md` — กรอกข้อมูล — ส่งเป็น PDF
5. อัปเดต Status ใน QuoteLog

## สูตรคำนวณ CBM

```
CBM = (Length cm × Width cm × Height cm) / 1,000,000

Chargeable Weight (Air) = MAX(Actual Weight kg, CBM × 167)
Chargeable Weight (Sea LCL) = MAX(Actual Weight kg / 1000, CBM)
```

## KPI ที่ต้องติดตาม

- Response Time (Received → Sent): เป้าหมาย < 2 ชั่วโมง
- Conversion Rate (Quote → Booking): เป้าหมาย 20%+
- Quotes ต่อสัปดาห์: เป้าหมาย +50% จาก baseline

---

> ⚠️ **Reminder:** ทุก quotation ต้องผ่าน Human Approval ก่อนส่งให้ลูกค้าเสมอ
