# แผน 90 วัน | 90-Day Automation Roadmap

## Month 1 (Day 1-30): Foundation + Freight System

### Week 1-2: Setup Infrastructure
- [ ] สร้าง Google Workspace folder structure (`/Freight/Rate Tables`, `/Freight/Quotation Templates`, `/Real Estate/Leads`, `/Sauna/Bookings`)
- [ ] ลงทะเบียน n8n Cloud ($20/เดือน) หรือ Make.com ($9/เดือน)
- [ ] Copy `freight/rate-table.csv` เข้า Google Sheets และกรอกข้อมูล rates จริง
- [ ] สร้าง Google Form สำหรับรับ Quote Request (ดู `freight/README.md`)
- [ ] เชื่อมต่อ LINE OA กับ n8n ผ่าน LINE Messaging API

### Week 3-4: Freight Automation Launch
- [ ] Import `freight/n8n-workflow.json` เข้า n8n
- [ ] กำหนด credentials: Google Sheets, LINE Notify, Email (SMTP)
- [ ] สร้าง Google Docs template จาก `freight/quotation-template.md`
- [ ] ตั้ง Human Approval notification ผ่าน LINE Notify
- [ ] ทดสอบระบบกับ 5-10 quotes จริง
- [ ] เปิดใช้งานจริง + ตั้ง monitoring

**KPI เป้าหมาย Month 1:**
- Response time: < 2 ชั่วโมง (จากเดิม 1 วัน)
- Quote accuracy: 100%
- ลด manual work: 70%

---

## Month 2 (Day 31-60): Real Estate + Sauna

### Week 5-6: Real Estate Lead System
- [ ] สร้าง Facebook Lead Form และเชื่อมกับ Make.com webhook
- [ ] Setup HubSpot CRM (Free tier) และเพิ่ม custom fields จาก `real-estate/hubspot-fields.md`
- [ ] Import `real-estate/make-scenario.json` เข้า Make.com
- [ ] Copy `real-estate/leads-template.csv` เข้า Google Sheets
- [ ] Build lead scoring formula จาก `real-estate/lead-scoring-formula.md`
- [ ] สร้าง auto-reply template สำหรับ Messenger และ LINE
- [ ] ตั้ง Human Approval workflow ก่อน assign agent

### Week 7-8: Sauna LINE Flow
- [ ] สร้าง LINE OA Rich Menu (3 ปุ่ม: จองบริการ / ดูแพ็คเกจ / ติดต่อเรา)
- [ ] Import `sauna/line-chatbot-flow.json` เข้า LINE Official Account Manager
- [ ] Import `sauna/n8n-workflow.json` เข้า n8n
- [ ] Copy `sauna/booking-template.csv` เข้า Google Sheets
- [ ] ตั้ง coupon generation logic จาก `sauna/coupon-generator.md`
- [ ] ตั้ง Reminder อัตโนมัติ 24 ชั่วโมงก่อนนัด

**KPI เป้าหมาย Month 2:**
- Real Estate: Lead response < 5 นาที
- Sauna: Conversion จาก LINE 20%+
- Booking confirmation rate 90%+

---

## Month 3 (Day 61-90): Content + SOP + Optimization

### Week 9-10: AI Content Pipeline
- [ ] Copy `content/content-calendar.csv` เข้า Google Sheets
- [ ] Import `content/n8n-workflow.json` เข้า n8n
- [ ] ตั้ง OpenAI API key ใน n8n credentials
- [ ] สร้าง Human Approval step ก่อน publish ทุกชิ้น
- [ ] เชื่อม YouTube Data API / TikTok scheduling

### Week 11-12: SOP + Dashboard
- [ ] Copy `sop/onboarding-checklist.md` เข้า Notion workspace
- [ ] สร้าง training modules ตาม `sop/training-modules/README.md`
- [ ] สร้าง Google Looker Studio Dashboard เชื่อมกับ Google Sheets ทุก sheet
- [ ] Review และ optimize ทุกระบบจาก feedback 60 วันที่ผ่านมา

**KPI เป้าหมาย Month 3:**
- Manual hours saved: 25+ ชั่วโมง/สัปดาห์
- ทุกระบบมี uptime > 99%
- Team adoption rate: 100%

---

## สิ่งสำคัญที่ต้องจำ

> **"อย่าสร้างทุกอย่างพร้อมกัน"**
> ระบบแรกที่ทำงานได้จริงมีค่ามากกว่า 5 ระบบที่ยังไม่เสร็จ
> ทำ Freight ให้ดีก่อน แล้วค่อยขยายไประบบถัดไป

**สรุปการลงทุน:**
- เวลา: 5-10 ชม./สัปดาห์ ใน 4 สัปดาห์แรก
- เงิน: 1,500-4,000 ฿/เดือน
- ROI คาดการณ์: ประหยัดเวลา 25+ ชม./สัปดาห์ และเพิ่ม conversion 20-35% ภายใน 90 วัน
