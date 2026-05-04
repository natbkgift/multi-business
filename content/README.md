# 🎬 AI Content / YouTube Shorts Automation

ระบบ #4 — AI-assisted content pipeline จาก trend research ถึง publish

## ภาพรวม

ระบบนี้จะช่วยให้คุณ:
- Research trends อัตโนมัติทุกต้นสัปดาห์
- สร้าง Script Draft ด้วย OpenAI GPT-4
- จัดการ Content Calendar ใน Google Sheets
- Human Approval ก่อน produce และก่อน publish ทุกชิ้น
- Track performance ใน dashboard

## ไฟล์ในโฟลเดอร์นี้

| ไฟล์ | วัตถุประสงค์ |
|------|------------|
| `n8n-workflow.json` | Content pipeline workflow |
| `content-calendar.csv` | Content Calendar template |

## วิธี Setup

### Step 1: OpenAI API
1. สร้างบัญชีที่ [platform.openai.com](https://platform.openai.com)
2. สร้าง API Key
3. ตั้ง spending limit $20-50/เดือน
4. เพิ่ม credentials ใน n8n

### Step 2: Google Sheets
1. Copy `content-calendar.csv` เข้า Google Sheets ชื่อ "Content Calendar"
2. แชร์กับ Service Account email

### Step 3: n8n Workflow
1. Import `n8n-workflow.json`
2. กำหนด credentials: Google Sheets, OpenAI
3. Activate scheduled trigger (ทุกวันจันทร์ 08:00)

## Content Calendar Fields

ดูโครงสร้างทั้งหมดใน `content-calendar.csv`

## Human Approval Process

1. **Script Approval:** GPT สร้าง draft → ทีมตรวจใน Google Sheets → เปลี่ยน Status = Script Approved
2. **Pre-Publish Approval:** Video/Post พร้อม → Manager ตรวจ → เปลี่ยน Status = Published

> ⚠️ ห้าม publish ใดๆ โดยไม่ผ่าน Human Approval
