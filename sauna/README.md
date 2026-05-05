# 🥉 Harmony Sauna - LINE OA Automation

ระบบ #3 — LINE chatbot สำหรับรับจอง + ออก Coupon + Reminder

## ภาพรวม

ระบบนี้จะช่วยให้คุณ:
- รับข้อความจากลูกค้าผ่าน LINE OA 24/7
- ตอบกลับอัตโนมัติตาม intent (จอง/ราคา/สอบถาม)
- แนะนำ package ที่เหมาะสม
- รับการจองและส่ง Staff **อนุมัติ slot** (Human Approval)
- ออก Coupon Code อัตโนมัติหลังจองสำเร็จ
- ส่ง Reminder 24 ชั่วโมงก่อนนัด

## ไฟล์ในโฟลเดอร์นี้

| ไฟล์ | วัตถุประสงค์ |
|------|------------|
| `line-chatbot-flow.json` | LINE OA Chatbot flow config |
| `n8n-workflow.json` | n8n workflow สำหรับ backend logic |
| `booking-template.csv` | โครงสร้าง Booking Sheet |
| `coupon-generator.md` | ตรรกะการสร้าง Coupon |

## วิธี Setup

### Step 1: LINE Official Account
1. สร้าง / Login LINE Official Account ที่ [manager.line.biz](https://manager.line.biz)
2. ไปที่ Settings → Messaging API → Enable
3. สร้าง Channel (Messaging API type)
4. บันทึก Channel Access Token และ Channel Secret

### Step 2: Rich Menu
สร้าง Rich Menu ด้วย 3 ปุ่ม:
- **จองบริการ** → ส่ง postback action: `action=book`
- **ดูแพ็คเกจ & ราคา** → ส่ง postback action: `action=packages`
- **ติดต่อเจ้าหน้าที่** → ส่ง postback action: `action=contact`

### Step 3: Webhook
1. ไปที่ n8n → Import `n8n-workflow.json`
2. Copy Webhook URL จาก n8n
3. วาง URL ใน LINE Developers Console → Webhook URL
4. Enable "Use webhook" ✓

### Step 4: Google Sheets
1. Copy `booking-template.csv` เข้า Google Sheets ชื่อ "Sauna Management"
2. สร้าง sheet: `Bookings`, `Packages`, `Coupons`
3. กรอก Packages ที่ offer ใน sheet `Packages`

## แพ็คเกจตัวอย่าง (แก้ไขได้)

| Package | ราคา | ระยะเวลา | รายละเอียด |
|---------|------|---------|-----------|
| Basic Relax | 499 ฿ | 45 นาที | สวน + ฝักบัว |
| Premium | 899 ฿ | 90 นาที | สวน + ฝักบัว + น้ำผลไม้ |
| Couple Spa | 1,499 ฿ | 90 นาที | 2 ท่าน + ขนม + น้ำผลไม้ |
| Monthly Pass | 2,499 ฿ | ไม่จำกัด (1 เดือน) | สูงสุด 8 ครั้ง |

---

> ⚠️ **Reminder:** Staff ต้องยืนยัน booking slot ก่อนที่ระบบจะส่ง coupon ให้ลูกค้า
