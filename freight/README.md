# 🥇 Freight Forwarding Quotation Automation

> ⚠️ **PILOT V1 — Draft-Only Supervised Mode**
> ทุก quotation ต้องผ่าน Human Approval ก่อนส่งให้ลูกค้าเสมอ
> ห้ามใช้ rate จาก rate-table.csv โดยไม่ verify กับ carrier จริงก่อน

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
| `n8n-workflow.json` | Import เข้า n8n ได้ทันที (Pilot V1 — draft-only mode) |
| `rate-table.csv` | Rate Table — **ต้อง verify กับ carrier จริงก่อนใช้** |
| `quote-log-template.csv` | โครงสร้าง Quote Log Sheet |
| `quotation-template.md` | Template ใบเสนอราคา (มี DRAFT watermark) |
| `config-template.csv` | Environment variable reference — ตั้งใน n8n |
| `error-log-template.csv` | โครงสร้าง Error Log สำหรับ pilot |
| `test-cases.md` | Test cases ก่อน Go-Live (TC-001–TC-011) |
| `test-results-v1.md` | Simulation results สำหรับ TC-001–TC-011 |
| `pilot-report-v1.md` | รายงาน Pilot Fix V1 |
| `pilot-qa-audit-v1.md` | QA Audit report |

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
3. ตั้ง **Environment Variables** ใน n8n (Settings → Variables):
   - `GOOGLE_SHEET_ID` — ID ของ Google Sheet
   - `USD_THB` — อัตราแลกเปลี่ยนปัจจุบัน เช่น `35.50` (อัปเดตทุกวัน)
   - `QUOTE_VALIDITY_DAYS` — เช่น `30`
   - `DOC_FEE` — ค่าธรรมเนียมเอกสาร (THB) เช่น `500`
   - `SALES_EMAIL` — อีเมลผู้ส่ง
   - `MANAGER_EMAIL` — อีเมลผู้อนุมัติ
   - `LINE_NOTIFY_TOKEN` — Token จาก notify-bot.line.me/my
   - `APPROVAL_SECRET_TOKEN` — **[SECURITY REQUIRED]** Secret token สำหรับ approval webhook  
     สร้างด้วย random string ≥ 32 ตัวอักษร ใช้ตัวอักษร alphanumeric และสัญลักษณ์ `-_` เท่านั้น  
     (เช่น `openssl rand -hex 32` หรือ `openssl rand -base64 32 | tr -d '=+/'`)  
     **ห้ามใช้อักขระพิเศษ เช่น `&`, `?`, `#` เพราะอาจมีปัญหา URL encoding**  
     **ห้าม commit ค่าจริงลงใน git**  
     ผู้ที่ trigger approval webhook ต้องส่ง token นี้ใน header `x-approval-token` หรือ body field `approvalToken`
4. กำหนด Credentials:
   - **Google Sheets**: เพิ่ม Service Account credentials
   - **LINE Notify**: ใส่ LINE Notify credentials
   - **Gmail/SMTP**: ใส่ Email credentials
5. อัปเดต Webhook URL ของ Google Form ใน workflow
6. **ใส่ authentication บน approval webhook** ก่อน activate
7. Activate workflow

### Step 4: ทดสอบ Approval Webhook

ก่อนส่ง approval request ต้องใส่ token เสมอ:

```bash
# ตัวอย่าง curl พร้อม APPROVAL_SECRET_TOKEN
curl -X POST https://your-n8n-instance/webhook/freight-quote-approved \
  -H "Content-Type: application/json" \
  -H "x-approval-token: <YOUR_APPROVAL_SECRET_TOKEN>" \
  -d '{"quoteId":"QT-20260505-1234","approvedBy":"manager","customerContact":"@line_id","totalPrice":9500,"customerName":"Test Co."}'
```

หาก token ขาดหรือผิด → workflow จะ throw error และ **ลูกค้าจะไม่ได้รับข้อความใดๆ**

### Step 5: ทดสอบ End-to-End
1. กรอก Google Form ด้วยข้อมูลทดสอบ
2. ตรวจดูว่า n8n ดึงข้อมูลได้ถูกต้อง
3. ตรวจ Quote Log ว่าบันทึกข้อมูลครบ (status = DRAFT)
4. ตรวจ LINE Notify ว่าแจ้งเตือน manager มา
5. ทดสอบ approval webhook พร้อม valid token — ตรวจว่าระบบส่ง quote ให้ลูกค้า
6. ทดสอบ approval webhook **ไม่มี token** — ตรวจว่า workflow halt และลูกค้าไม่ได้รับ

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
