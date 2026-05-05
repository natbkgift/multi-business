# Google Sheets Structure — AI Content Engine V1

> เอกสารนี้อธิบายโครงสร้าง Google Sheets ทั้ง 7 sheets ที่ใช้ในระบบ AI Content Engine V1
>
> **วิธีใช้:** สร้าง Google Sheets ใหม่ ตั้งชื่อ `AI Content Engine V1` แล้วสร้าง tabs ตามด้านล่าง

---

## Sheet 1: Content Pipeline

**วัตถุประสงค์:** ติดตามสถานะคอนเทนต์ทุกชิ้นตั้งแต่ Idea จนถึง Published

| คอลัมน์ | Field Type | ตัวอย่างค่า | Required/Optional | ผู้อัปเดต | เวลาอัปเดต |
|---------|-----------|-------------|-------------------|-----------|------------|
| Content ID | Text (Auto) | `HS-2026-001` | Required | ระบบ / Strategist | ตอนสร้าง |
| Brand | Dropdown | `Harmony Sauna` | Required | Strategist | ตอนสร้าง |
| Platform | Dropdown | `TikTok` | Required | Strategist | ตอนสร้าง |
| Topic | Text | `5 เหตุผลที่ต้องลอง Sauna ครั้งแรก` | Required | Strategist | ตอนสร้าง |
| Hook | Text | `คุณรู้ไหมว่าแค่ 15 นาทีใน Sauna...` | Required | Strategist | ตอนสร้าง |
| Status | Dropdown | `Script Draft` | Required | ทุกคน | ทุกครั้งที่เปลี่ยนสถานะ |
| Script Writer | Text | `@สมชาย` | Required | Strategist | ตอน Assign |
| Script Reviewer | Text | `@สมหญิง` | Required | Strategist | ตอน Assign |
| Creative Brief Owner | Text | `@ดีไซเนอร์` | Optional | Strategist | ตอน Assign |
| Scheduled Date | Date | `2026-05-10` | Optional | Scheduler | ตอนวางแผน |
| Published Date | Date | `2026-05-10` | Optional | Scheduler | หลัง Publish |
| Priority | Dropdown | `High` / `Medium` / `Low` | Required | Strategist | ตอนสร้าง |
| Notes | Text | `รอรูปจาก photographer` | Optional | ทุกคน | ตลอดเวลา |

**Dropdown Values:**
- Brand: `Harmony Sauna`, `AMP Pattaya`, `The World Freight`, `ธรรมะดีดี`, `Affiliate/AI Automation`
- Platform: `TikTok`, `Facebook`, `YouTube`, `YouTube Shorts`, `Instagram`, `LinkedIn`, `X (Twitter)`
- Status: ดูได้ที่ `status-workflow.md`
- Priority: `High`, `Medium`, `Low`

**การตั้งค่า Conditional Formatting:**
- Status = `Published` → แถวสีเขียวอ่อน
- Status = `Stop` → แถวสีแดงอ่อน
- Priority = `High` → ตัวหนา

---

## Sheet 2: Idea Bank

**วัตถุประสงค์:** เก็บ Idea ทุกชิ้นที่สร้างโดย AI หรือทีม พร้อม Score และสถานะ

| คอลัมน์ | Field Type | ตัวอย่างค่า | Required/Optional | ผู้อัปเดต | เวลาอัปเดต |
|---------|-----------|-------------|-------------------|-----------|------------|
| Idea ID | Text (Auto) | `IDEA-HS-001` | Required | Strategist | ตอนสร้าง |
| Brand | Dropdown | `Harmony Sauna` | Required | Strategist | ตอนสร้าง |
| Platform | Dropdown | `TikTok` | Required | Strategist | ตอนสร้าง |
| Idea Title | Text | `ทำไมคนญี่ปุ่นถึงใช้ Sauna ทุกวัน` | Required | Strategist | ตอนสร้าง |
| Hook | Text | `ความลับที่คนญี่ปุ่นรู้แต่คนไทยไม่รู้...` | Required | Strategist | ตอนสร้าง |
| Angle Type | Dropdown | `Education` | Required | Strategist | ตอนสร้าง |
| Target Audience | Text | `ผู้หญิงอายุ 25-35 ที่ต้องการผ่อนคลาย` | Required | Strategist | ตอนสร้าง |
| Suggested Format | Dropdown | `Short-form video 60s` | Required | Strategist | ตอนสร้าง |
| Goal | Dropdown | `Awareness` | Required | Strategist | ตอนสร้าง |
| Source | Text | `AI Generated / จาก Prompt 01` | Optional | Strategist | ตอนสร้าง |
| CTA | Text | `เพิ่ม LINE เพื่อจอง` | Required | Strategist | ตอนสร้าง |
| Risk Note | Text | `ห้ามอ้างสรรพคุณสุขภาพโดยไม่มีหลักฐาน` | Optional | Reviewer | ตอนตรวจ |
| Score | Number (1-10) | `8` | Required | Strategist | ตอนประเมิน |
| Status | Dropdown | `Selected` | Required | Strategist | ทุกครั้งที่เปลี่ยน |
| Date Added | Date | `2026-05-06` | Required | ระบบ | ตอนสร้าง |
| Added By | Text | `@สมชาย` | Required | ผู้สร้าง | ตอนสร้าง |

**Dropdown Values:**
- Angle Type: `Education`, `Entertainment`, `Testimonial`, `Problem-Solution`, `Behind the Scene`, `Promotion`, `Trend`, `Comparison`, `How-to`, `Storytelling`
- Suggested Format: `Short-form video 30s`, `Short-form video 60s`, `Short-form video 90s`, `Reel`, `Static Post`, `Carousel`, `Story`, `Long-form video`, `YouTube Shorts`
- Goal: `Awareness`, `Engagement`, `Leads`, `Sales`, `Retention`, `Education`
- Status: `New`, `Selected`, `Approved`, `Rejected`, `On Hold`

---

## Sheet 3: Script Queue

**วัตถุประสงค์:** ติดตามกระบวนการเขียนและอนุมัติ Script

| คอลัมน์ | Field Type | ตัวอย่างค่า | Required/Optional | ผู้อัปเดต | เวลาอัปเดต |
|---------|-----------|-------------|-------------------|-----------|------------|
| Script ID | Text (Auto) | `SCR-HS-001` | Required | Strategist | ตอนสร้าง |
| Linked Idea ID | Text | `IDEA-HS-001` | Required | Strategist | ตอนสร้าง |
| Brand | Dropdown | `Harmony Sauna` | Required | Strategist | ตอนสร้าง |
| Platform | Dropdown | `TikTok` | Required | Strategist | ตอนสร้าง |
| Draft Version | Number | `v1`, `v2`, `v3` | Required | Writer | ทุกครั้งแก้ไข |
| Script Status | Dropdown | `In Review` | Required | Writer / Reviewer | ทุกครั้งที่เปลี่ยน |
| Writer | Text | `@สมชาย` | Required | Strategist | ตอน Assign |
| Reviewer | Text | `@สมหญิง` | Required | Strategist | ตอน Assign |
| Review Date | Date | `2026-05-08` | Optional | Reviewer | ตอน Review |
| Approved By | Text | `@เจ้าของ` | Optional | Approver | ตอน Approve |
| Revision Notes | Text | `แก้ Hook ให้ตื่นเต้นขึ้น ลด claim สุขภาพ` | Optional | Reviewer | ตอน Review |

**Dropdown Values:**
- Script Status: `Not Started`, `Writing`, `Draft Ready`, `In Review`, `Revision Required`, `Approved`, `Rejected`

---

## Sheet 4: Creative Briefs

**วัตถุประสงค์:** เก็บ Brief ให้ Designer/Editor เพื่อผลิตคอนเทนต์

| คอลัมน์ | Field Type | ตัวอย่างค่า | Required/Optional | ผู้อัปเดต | เวลาอัปเดต |
|---------|-----------|-------------|-------------------|-----------|------------|
| Brief ID | Text (Auto) | `BRIEF-HS-001` | Required | Strategist | ตอนสร้าง |
| Linked Script ID | Text | `SCR-HS-001` | Required | Strategist | ตอนสร้าง |
| Brand | Dropdown | `Harmony Sauna` | Required | Strategist | ตอนสร้าง |
| Visual Style | Text | `อบอุ่น, สีน้ำตาลอ่อน, Minimal` | Required | Strategist | ตอนสร้าง |
| Shot List | Long Text | `Shot 1: ประตู Sauna, Shot 2: ไอน้ำ...` | Required | Strategist | ตอนสร้าง |
| Text Overlay | Long Text | `"15 นาที เปลี่ยนวันของคุณ"` | Required | Strategist | ตอนสร้าง |
| Thumbnail Concept | Text | `ผู้หญิงยิ้มในห้อง Sauna, ข้อความ "ลองแล้วติดใจ"` | Required | Strategist | ตอนสร้าง |
| Required Assets | Text | `โลโก้ Harmony, รูป Sauna Room 3 รูป` | Required | Strategist | ตอนสร้าง |
| Editor Notes | Long Text | `ใช้ bgm เบาๆ, ห้ามใส่เพลงดังเกิน, ตัด caption ไทย` | Optional | Strategist | ตอนสร้าง |
| Status | Dropdown | `Sent to Editor` | Required | Strategist | ทุกครั้งที่เปลี่ยน |
| Designer | Text | `@ช่างภาพ` | Required | Strategist | ตอน Assign |

**Dropdown Values:**
- Status: `Not Started`, `Briefing`, `Sent to Editor`, `In Production`, `Review Ready`, `Approved`, `Revision Required`

---

## Sheet 5: Publishing Calendar

**วัตถุประสงค์:** วางแผนและติดตามการ Publish คอนเทนต์ประจำวัน/สัปดาห์

| คอลัมน์ | Field Type | ตัวอย่างค่า | Required/Optional | ผู้อัปเดต | เวลาอัปเดต |
|---------|-----------|-------------|-------------------|-----------|------------|
| Date | Date | `2026-05-10` | Required | Scheduler | ตอนวางแผน |
| Time | Time | `18:00` | Required | Scheduler | ตอนวางแผน |
| Brand | Dropdown | `Harmony Sauna` | Required | Scheduler | ตอนวางแผน |
| Platform | Dropdown | `TikTok` | Required | Scheduler | ตอนวางแผน |
| Content ID | Text | `HS-2026-001` | Required | Scheduler | ตอนวางแผน |
| Caption Preview | Long Text | `วันนี้ขอแนะนำ 5 เหตุผล...` | Required | Strategist | ตอนสร้าง Caption |
| Status | Dropdown | `Scheduled` | Required | Scheduler | ทุกครั้งที่เปลี่ยน |
| Scheduler | Text | `@สมชาย` | Required | Strategist | ตอน Assign |
| Approved By | Text | `@เจ้าของ` | Required | Approver | ตอน Approve |
| Published URL | URL | `https://www.tiktok.com/@...` | Optional | Scheduler | หลัง Publish |
| Notes | Text | `ต้องลง TikTok ก่อน Facebook 1 ชั่วโมง` | Optional | ทุกคน | ตลอดเวลา |

**Dropdown Values:**
- Status: `Draft`, `Pending Approval`, `Scheduled`, `Published`, `Failed`, `Cancelled`

---

## Sheet 6: KPI Tracker

**วัตถุประสงค์:** เก็บข้อมูลประสิทธิภาพคอนเทนต์หลัง Publish

| คอลัมน์ | Field Type | ตัวอย่างค่า | Required/Optional | ผู้อัปเดต | เวลาอัปเดต |
|---------|-----------|-------------|-------------------|-----------|------------|
| Content ID | Text | `HS-2026-001` | Required | Performance Reviewer | หลัง 48 ชั่วโมง |
| Brand | Dropdown | `Harmony Sauna` | Required | Performance Reviewer | ตอนกรอก |
| Platform | Dropdown | `TikTok` | Required | Performance Reviewer | ตอนกรอก |
| Publish Date | Date | `2026-05-10` | Required | Performance Reviewer | ตอนกรอก |
| Views | Number | `12,540` | Required | Performance Reviewer | หลัง 48 ชั่วโมง |
| 3-sec Retention | Percentage | `68%` | Required | Performance Reviewer | หลัง 48 ชั่วโมง |
| Watch Time (avg) | Number (วินาที) | `34` | Required | Performance Reviewer | หลัง 48 ชั่วโมง |
| Completion Rate | Percentage | `42%` | Required | Performance Reviewer | หลัง 48 ชั่วโมง |
| Likes | Number | `380` | Required | Performance Reviewer | หลัง 48 ชั่วโมง |
| Comments | Number | `54` | Required | Performance Reviewer | หลัง 48 ชั่วโมง |
| Shares | Number | `27` | Required | Performance Reviewer | หลัง 48 ชั่วโมง |
| Saves | Number | `89` | Optional | Performance Reviewer | หลัง 48 ชั่วโมง |
| Link Clicks | Number | `210` | Optional | Performance Reviewer | หลัง 48 ชั่วโมง |
| DM/Inquiries | Number | `15` | Optional | Performance Reviewer | หลัง 48 ชั่วโมง |
| Leads | Number | `5` | Optional | Performance Reviewer | หลัง 7 วัน |
| Conversion Signal | Dropdown | `Strong` | Required | Performance Reviewer | หลัง 7 วัน |
| Content Score | Number (1-100) | `78` | Required | Performance Reviewer | หลัง 7 วัน |
| Repurpose Decision | Dropdown | `Repeat` | Required | Strategist | ตอน Weekly Review |

**Dropdown Values:**
- Conversion Signal: `None`, `Weak`, `Moderate`, `Strong`, `Very Strong`
- Repurpose Decision: `Repurpose + Scale`, `Repeat`, `Improve`, `Review`, `Stop`

---

## Sheet 7: Weekly Review

**วัตถุประสงค์:** บันทึกผลการวิเคราะห์ประจำสัปดาห์เพื่อปรับปรุง Strategy

| คอลัมน์ | Field Type | ตัวอย่างค่า | Required/Optional | ผู้อัปเดต | เวลาอัปเดต |
|---------|-----------|-------------|-------------------|-----------|------------|
| Week | Text | `Week 19 (6-12 พ.ค. 2026)` | Required | Performance Reviewer | ทุกจันทร์ |
| Brand | Dropdown | `Harmony Sauna` | Required | Performance Reviewer | ตอนกรอก |
| Top Performers | Long Text | `HS-001 (Score 85), HS-003 (Score 79)` | Required | Performance Reviewer | ทุกจันทร์ |
| Failed Content | Long Text | `HS-002 (Score 28) — Hook ไม่แข็งแรง` | Required | Performance Reviewer | ทุกจันทร์ |
| Patterns Found | Long Text | `วิดีโอที่มี Before/After ทำได้ดีกว่า 2x` | Required | Performance Reviewer | ทุกจันทร์ |
| What to Repeat | Long Text | `Hook แบบ "คุณรู้ไหม...", Format 60s` | Required | Strategist | ทุกจันทร์ |
| What to Stop | Long Text | `Hook แบบตั้งคำถามนานเกิน 5 วินาที` | Required | Strategist | ทุกจันทร์ |
| Next Week Recommendations | Long Text | `เพิ่ม Testimonial 2 ชิ้น ลด Promotion 1 ชิ้น` | Required | Strategist | ทุกจันทร์ |
| Reviewed By | Text | `@สมชาย` | Required | ผู้ Review | ทุกจันทร์ |

---

## การตั้งค่าแนะนำ

### การ Freeze Rows
- Freeze แถวที่ 1 ทุก Sheet เพื่อให้ Header ไม่เลื่อน

### การ Sort และ Filter
- Content Pipeline: Sort ตาม Priority (High → Low) และ Scheduled Date
- Idea Bank: Sort ตาม Score (สูง → ต่ำ)
- KPI Tracker: Sort ตาม Content Score (สูง → ต่ำ)

### สีประจำแต่ละแบรนด์ (แนะนำ)
- Harmony Sauna: 🟤 น้ำตาลอ่อน
- AMP Pattaya: 🔵 น้ำเงิน
- The World Freight: 🟠 ส้ม
- ธรรมะดีดี: 🟡 เหลืองทอง
- Affiliate/AI Automation: 🟢 เขียว

---

*อัปเดตล่าสุด: 2026-05-06 | AI Content Engine V1*
