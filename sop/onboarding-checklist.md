# Onboarding Checklist | รายการสำหรับพนักงานและ Intern ใหม่

> **วิธีใช้:** Copy checklist นี้เข้า Notion หรือ Google Docs สำหรับ Intern/พนักงานใหม่แต่ละคน
> กำหนด due date และ assign ผู้รับผิดชอบในแต่ละข้อ

---

## ข้อมูลพนักงานใหม่

- **ชื่อ-นามสกุล:**
- **ตำแหน่ง:**
- **วันเริ่มงาน:**
- **Buddy/Mentor:**
- **วันครบ Probation:**

---

## Day 1: Welcome & Setup

- [ ] รับ computer / อุปกรณ์ทำงาน
- [ ] สร้าง Email บริษัท
- [ ] เพิ่มเข้า LINE Group ทีม
- [ ] แนะนำทีมและทัวร์ office (หรือ virtual intro ถ้า remote)
- [ ] ติดตั้ง tools ที่จำเป็น (Google Workspace, n8n, Notion, LINE)
- [ ] อ่าน Company Overview และ Business Context (30 นาที)
- [ ] อ่าน `docs/architecture.md` — ภาพรวมระบบ

---

## Week 1: Core Training

### Day 2-3: Business Overview
- [ ] **Module 1:** ภาพรวมธุรกิจและ 5 สาขา (ดู `docs/architecture.md`)
- [ ] **Module 2:** Tool Stack และวิธีใช้งาน (ดู `docs/tool-stack.md`)
- [ ] **Module 3:** KPI Dashboard และวิธีอ่านผล (ดู `docs/kpi-dashboard.md`)
- [ ] Quiz Module 1-3: pass ≥ 80%

### Day 4-5: ระบบที่รับผิดชอบ (เลือกตามตำแหน่ง)

#### ถ้าดูแลระบบ Freight:
- [ ] อ่าน `freight/README.md` ทั้งหมด
- [ ] ดู n8n workflow: `freight/n8n-workflow.json` (import + explore)
- [ ] ทดสอบส่ง quote request ผ่าน Google Form
- [ ] ฝึก approve quote ใน Google Sheets
- [ ] Quiz Freight: pass ≥ 80%

#### ถ้าดูแลระบบ Real Estate:
- [ ] อ่าน `real-estate/README.md`
- [ ] ดู `real-estate/lead-scoring-formula.md`
- [ ] Login HubSpot CRM และ explore
- [ ] ฝึก qualify lead + update pipeline stage
- [ ] Quiz Real Estate: pass ≥ 80%

#### ถ้าดูแลระบบ Sauna:
- [ ] อ่าน `sauna/README.md`
- [ ] ฝึก approve booking ผ่าน LINE Notify
- [ ] ทำความเข้าใจ coupon system (`sauna/coupon-generator.md`)
- [ ] Quiz Sauna: pass ≥ 80%

#### ถ้าดูแล Content:
- [ ] อ่าน `content/README.md`
- [ ] ดู Content Calendar ใน Google Sheets
- [ ] ฝึก review และ approve script draft
- [ ] Quiz Content: pass ≥ 80%

---

## Week 2: Hands-on Practice

- [ ] ทำงานจริงภายใต้การดูแลของ Buddy
- [ ] Shadow Buddy ใน 5 tasks จริง
- [ ] ทำ 3 tasks แรกด้วยตนเอง (Buddy ตรวจ)
- [ ] Weekly Check-in กับ Supervisor
- [ ] Feedback session กับ Buddy

---

## Week 3-4: Independent Work

- [ ] ทำงานอิสระ ± มี Buddy พร้อม assist
- [ ] ทำ daily checklist ด้วยตนเอง (ดู `docs/weekly-checklist.md`)
- [ ] Monthly Review กับ Manager

---

## Completion Criteria

ถือว่า Onboarding สำเร็จเมื่อ:
- [ ] ผ่าน Quiz ทุก Module ≥ 80%
- [ ] ทำ 10 tasks จริงโดยไม่มี error
- [ ] รู้จัก Human Approval Gates ทุกจุด
- [ ] Manager sign-off

---

## กฎ Human Approval (ต้องจำ!)

พนักงานทุกคนต้องรู้จุดที่ **ห้ามทำโดยอัตโนมัติ:**
1. ❌ ห้ามส่ง quotation ให้ลูกค้าโดยไม่ผ่าน Manager
2. ❌ ห้าม assign lead ให้ agent โดยไม่ผ่าน Sales Manager
3. ❌ ห้าม publish content ใดๆ โดยไม่ผ่าน Content Manager
4. ❌ ห้ามออก coupon/promo โดยไม่ผ่าน Staff on duty
5. ❌ ห้ามแก้ Rate Table โดยไม่ผ่าน เจ้าของธุรกิจ

---

*Template นี้ปรับปรุงล่าสุด: 2024-01 | อัปเดตเมื่อมีการเปลี่ยนแปลง workflow*
