# Status Workflow — AI Content Engine V1

> เอกสารนี้อธิบาย 12 สถานะของคอนเทนต์ตั้งแต่ Idea จนถึงการตัดสินใจปิด/ขยาย
>
> **วิธีใช้:** ทุกครั้งที่คอนเทนต์เปลี่ยนขั้นตอน ให้อัปเดต Status ใน Google Sheets ทันที

---

## Flowchart (Text-Based)

```
┌─────────────────────────────────────────────────────────────────┐
│                    CONTENT LIFECYCLE                            │
└─────────────────────────────────────────────────────────────────┘

  [1. Idea] ──→ [2. Selected] ──→ [3. Approved] ──→ [4. Script Draft]
                    │                   │
                    ↓ (Reject)          ↓ (Reject)
                 [Archive]           [Archive]

  [4. Script Draft] ──→ [5. Script Review] ──→ [6. Creative Brief Ready]
                              │
                              ↓ (Revision Required)
                         [4. Script Draft อีกครั้ง]

  [6. Creative Brief Ready] ──→ [7. Production] ──→ [8. Final Review]
                                                          │
                                                          ↓ (Revision)
                                                     [7. Production อีกครั้ง]

  [8. Final Review] ──→ [9. Scheduled] ──→ [10. Published] ──→ [11. Analyzed]
                                                                      │
                                              ┌───────────────────────┤
                                              ↓           ↓           ↓
                                      [12a. Reuse]  [12b. Improve]  [12c. Stop]
```

---

## 12 สถานะโดยละเอียด

---

### สถานะที่ 1: Idea

| หัวข้อ | รายละเอียด |
|--------|-----------|
| **ความหมาย** | Idea ที่เพิ่งถูกสร้างขึ้น ยังไม่ผ่านการคัดกรองใดๆ อาจมาจาก AI Prompt, ทีม, หรือแรงบันดาลใจ |
| **Owner** | Content Strategist |
| **Entry Condition** | สร้าง Idea ใหม่ใน Idea Bank — ต้องมีครบ: Idea Title, Hook, Angle Type, Platform, Goal |
| **Exit Condition** | Strategist ตรวจสอบและให้ Score → ย้ายเป็น "Selected" หรือ "Rejected" |
| **Common Mistakes** | ❌ กรอก Idea โดยไม่ระบุ Platform, ❌ ไม่มี Hook ชัดเจน, ❌ Score ไม่ได้ประเมินก่อน Selected |

---

### สถานะที่ 2: Selected

| หัวข้อ | รายละเอียด |
|--------|-----------|
| **ความหมาย** | Idea ที่ผ่านการคัดเลือกเบื้องต้นจาก Strategist ว่าน่าผลิต รอรับการอนุมัติ |
| **Owner** | Content Strategist |
| **Entry Condition** | Score ≥ 6/10 และ Strategist ตัดสินใจว่าเหมาะกับ Brand, Platform, และ Goal ในช่วงนั้น |
| **Exit Condition** | Owner/ผู้บริหาร หรือ Strategist ระดับสูง Approve → ย้ายเป็น "Approved" |
| **Common Mistakes** | ❌ เลือก Idea โดยไม่ดู Risk Note, ❌ Select Idea มากกว่าที่ทีมรับได้ในสัปดาห์นั้น |

---

### สถานะที่ 3: Approved

| หัวข้อ | รายละเอียด |
|--------|-----------|
| **ความหมาย** | Idea ผ่านการอนุมัติแล้ว พร้อม Assign ให้ Writer เขียน Script |
| **Owner** | Owner / Content Strategist |
| **Entry Condition** | ผ่าน Script Production Gate (ดูใน `approval-gates.md`) — Idea ชัดเจน, Risk ระบุแล้ว, Platform ยืนยันแล้ว |
| **Exit Condition** | Assign Writer แล้ว สร้าง Script ID ใน Script Queue |
| **Common Mistakes** | ❌ Approve โดยไม่ตรวจ Risk Note, ❌ Approve แล้วไม่ Assign Writer ทันที (ทิ้งค้าง) |

---

### สถานะที่ 4: Script Draft

| หัวข้อ | รายละเอียด |
|--------|-----------|
| **ความหมาย** | Writer กำลังเขียนหรือเขียน Script เสร็จแล้ว รอส่ง Review |
| **Owner** | Script Writer |
| **Entry Condition** | Writer ได้รับ Approved Idea + Brand Playbook + Prompt 02 (Script Writer) |
| **Exit Condition** | Script Draft เสร็จสมบูรณ์ครบทุก section (Hook, Setup, Body, CTA, B-roll) → ส่ง Reviewer |
| **Common Mistakes** | ❌ ส่ง Script ที่ยังขาด CTA, ❌ ไม่ดู Brand Playbook ก่อนเขียน, ❌ เขียน claim สุขภาพ/การเงินโดยไม่แจ้ง |

---

### สถานะที่ 5: Script Review

| หัวข้อ | รายละเอียด |
|--------|-----------|
| **ความหมาย** | Script อยู่ระหว่างการ Review โดย Script Reviewer ตาม Compliance Checklist |
| **Owner** | Script Reviewer |
| **Entry Condition** | Writer ส่ง Script Draft เสร็จ + อัปเดต Status ใน Script Queue |
| **Exit Condition** | Reviewer ให้ "Approved" → ย้ายเป็น "Creative Brief Ready" หรือให้ "Revision Required" → กลับเป็น "Script Draft" |
| **Common Mistakes** | ❌ Approve Script โดยไม่อ่านทุก section, ❌ ไม่บันทึก Revision Notes เมื่อ Reject, ❌ Review เกิน 48 ชั่วโมง |

---

### สถานะที่ 6: Creative Brief Ready

| หัวข้อ | รายละเอียด |
|--------|-----------|
| **ความหมาย** | Script ผ่าน Review แล้ว Strategist สร้าง Creative Brief เพื่อส่งให้ Designer/Editor |
| **Owner** | Content Strategist / Creative Brief Owner |
| **Entry Condition** | Script Status = "Approved" + Strategist ใช้ Prompt 04 สร้าง Brief |
| **Exit Condition** | Brief ครบ (Shot List, Visual Direction, Text Overlay, Thumbnail, Assets) → ส่ง Editor/Designer |
| **Common Mistakes** | ❌ ไม่มี Shot List ชัดเจน, ❌ ไม่ระบุ Required Assets ทำให้ Editor ต้องถามเพิ่ม |

---

### สถานะที่ 7: Production

| หัวข้อ | รายละเอียด |
|--------|-----------|
| **ความหมาย** | Designer/Editor กำลังผลิตคอนเทนต์จาก Creative Brief |
| **Owner** | Designer / Video Editor |
| **Entry Condition** | ได้รับ Creative Brief + ทรัพยากรทั้งหมดที่ระบุใน Required Assets |
| **Exit Condition** | ไฟล์ผลิตเสร็จ Export แล้ว ส่งให้ Final Reviewer ผ่าน Drive/Folder |
| **Common Mistakes** | ❌ Production โดยไม่มี Brief ชัดเจน, ❌ ใช้ Music/Font ที่มี Copyright, ❌ ส่งไฟล์ความละเอียดต่ำ |

---

### สถานะที่ 8: Final Review

| หัวข้อ | รายละเอียด |
|--------|-----------|
| **ความหมาย** | ตรวจสอบไฟล์จริงว่าตรงกับ Brief, Script, Brand Guidelines ก่อน Publish |
| **Owner** | Content Strategist / Owner |
| **Entry Condition** | ได้รับไฟล์จาก Production + เปรียบเทียบกับ Script และ Creative Brief |
| **Exit Condition** | ผ่าน Public Posting Gate (ดูใน `approval-gates.md`) → ย้ายเป็น "Scheduled" |
| **Common Mistakes** | ❌ Approve โดยดูแค่ภาพนิ่ง ไม่ดูวิดีโอจริง, ❌ ไม่ตรวจ Caption ก่อน Publish |

---

### สถานะที่ 9: Scheduled

| หัวข้อ | รายละเอียด |
|--------|-----------|
| **ความหมาย** | คอนเทนต์อนุมัติแล้ว กำหนดวันและเวลา Publish ไว้เรียบร้อย |
| **Owner** | Social Media Scheduler |
| **Entry Condition** | ผ่าน Final Review + กรอกวันเวลาใน Publishing Calendar แล้ว |
| **Exit Condition** | ถึงวันและเวลาที่กำหนด → Scheduler Publish manually |
| **Common Mistakes** | ❌ Schedule ไว้แต่ไม่ได้ Publish ตรงเวลา, ❌ ไม่ตรวจ Platform ว่า caption format ถูกต้อง |

---

### สถานะที่ 10: Published

| หัวข้อ | รายละเอียด |
|--------|-----------|
| **ความหมาย** | คอนเทนต์ Publish สาธารณะแล้ว กำลังสะสม Engagement |
| **Owner** | Social Media Scheduler → Performance Reviewer |
| **Entry Condition** | Publish แล้ว + บันทึก Published URL ใน Calendar |
| **Exit Condition** | ครบ 48 ชั่วโมง (สำหรับ Short-form) หรือ 7 วัน (สำหรับ Long-form) → ย้ายเป็น "Analyzed" |
| **Common Mistakes** | ❌ Publish แล้วไม่บันทึก URL, ❌ ไม่ติดตาม Comment ภายใน 24 ชั่วโมงแรก |

---

### สถานะที่ 11: Analyzed

| หัวข้อ | รายละเอียด |
|--------|-----------|
| **ความหมาย** | กรอก KPI ครบแล้ว คำนวณ Content Score แล้ว รอการตัดสินใจ |
| **Owner** | Performance Reviewer |
| **Entry Condition** | กรอก KPI ใน KPI Tracker ครบทุก Required Fields + คำนวณ Content Score |
| **Exit Condition** | Strategist ตัดสินใจ Repurpose Decision → ย้ายเป็น "Reuse", "Improve", หรือ "Stop" |
| **Common Mistakes** | ❌ กรอก KPI ไม่ครบ ทำให้ Score ผิดพลาด, ❌ ไม่ทำ Weekly Review ภายใน 7 วันหลัง Publish |

---

### สถานะที่ 12a: Reuse (ทำซ้ำ / ขยาย)

| หัวข้อ | รายละเอียด |
|--------|-----------|
| **ความหมาย** | Content Score ≥ 60 — คอนเทนต์นี้ทำงานได้ดี ควรผลิตซ้ำหรือ Repurpose |
| **Owner** | Content Strategist |
| **Entry Condition** | Content Score 60-100 + Strategist ตัดสินใจ |
| **Exit Condition** | สร้าง Idea ใหม่ต่อยอดจากคอนเทนต์นี้ใน Idea Bank |
| **Common Mistakes** | ❌ Copy เนื้อหาเดิม 100% โดยไม่ปรับ Hook ใหม่ |

---

### สถานะที่ 12b: Improve (ปรับปรุง)

| หัวข้อ | รายละเอียด |
|--------|-----------|
| **ความหมาย** | Content Score 40-59 — ทำได้ปานกลาง ต้องวิเคราะห์และปรับปรุงก่อนทำซ้ำ |
| **Owner** | Content Strategist |
| **Entry Condition** | Content Score 40-59 + บันทึก Patterns Found ใน Weekly Review |
| **Exit Condition** | สร้าง Revised Idea ใน Idea Bank พร้อม Revision Notes ชัดเจน |
| **Common Mistakes** | ❌ Improve โดยไม่ระบุว่า "ปรับตรงไหน" อย่างชัดเจน |

---

### สถานะที่ 12c: Stop (หยุด)

| หัวข้อ | รายละเอียด |
|--------|-----------|
| **ความหมาย** | Content Score < 40 — คอนเทนต์รูปแบบนี้ไม่ได้ผล ให้หยุดและ Archive |
| **Owner** | Content Strategist |
| **Entry Condition** | Content Score < 40 + Strategist ยืนยัน |
| **Exit Condition** | บันทึกใน Weekly Review ว่า "What to Stop" + อย่าทำ Angle/Hook แบบนี้ซ้ำ |
| **Common Mistakes** | ❌ Stop โดยไม่บันทึกเหตุผล ทำให้ทีมทำผิดซ้ำในอนาคต |

---

## Status → Action Matrix

| Status ปัจจุบัน | Action ที่ต้องทำ | ผู้รับผิดชอบ | เวลาที่ควรใช้ |
|----------------|-----------------|-------------|--------------|
| Idea | ประเมิน Score + ตรวจ Risk | Strategist | ภายใน 24 ชั่วโมง |
| Selected | รอ Approval จาก Owner | Strategist → Owner | ภายใน 48 ชั่วโมง |
| Approved | Assign Writer + สร้าง Script ID | Strategist | ทันที |
| Script Draft | เขียน Script ตาม Prompt 02 | Writer | 1-2 วัน |
| Script Review | Review ตาม Compliance Checklist | Reviewer | ภายใน 48 ชั่วโมง |
| Creative Brief Ready | สร้าง Brief ตาม Prompt 04 | Strategist | ภายใน 24 ชั่วโมง |
| Production | ผลิตคอนเทนต์ตาม Brief | Designer/Editor | 2-3 วัน |
| Final Review | ตรวจ + ผ่าน Public Posting Gate | Strategist/Owner | ภายใน 24 ชั่วโมง |
| Scheduled | เตรียม Platform + Caption | Scheduler | วันก่อน Publish |
| Published | ติดตาม Comment 24 ชั่วโมงแรก | Scheduler | ทันที |
| Analyzed | กรอก KPI + คำนวณ Score | Performance Reviewer | ภายใน 72 ชั่วโมง |
| Reuse/Improve/Stop | สร้าง Idea ใหม่หรือ Archive | Strategist | ใน Weekly Review |

---

## Escalation Rules (กรณีคอนเทนต์ติด)

1. **Script Review เกิน 48 ชั่วโมง** → Strategist แจ้ง Reviewer โดยตรง
2. **Final Review เกิน 24 ชั่วโมง** → Escalate ไปที่ Owner
3. **Production เกิน 3 วัน** → ตรวจสอบ Assets ว่าครบหรือไม่
4. **ไม่มีใคร Assign** → Strategist รับผิดชอบ default

---

*อัปเดตล่าสุด: 2026-05-06 | AI Content Engine V1*
