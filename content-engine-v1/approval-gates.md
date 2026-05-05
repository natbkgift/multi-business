# Approval Gates — AI Content Engine V1

> เอกสารนี้กำหนดประตูอนุมัติ 6 ประเภทที่ต้องผ่านก่อนคอนเทนต์จะดำเนินการต่อ
>
> **หลักการ:** ทุก Gate คือจุดหยุด ถ้าไม่ผ่าน Gate จะไม่สามารถก้าวไปขั้นตอนต่อไปได้

---

## ภาพรวม Approval Gates

```
[Idea] → [Gate 1: Script Production] → [Script Draft] → [Gate 2: Public Posting]
                                                               ↓
                                                     [Gate 3: Ads Usage]
                                                     [Gate 4: Customer-Facing]
                                                     [Gate 5: Sensitive Claims]
                                                     [Gate 6: Medical/Financial/Freight]
```

---

## Gate 1: Script Production Gate

**จุดประสงค์:** ตรวจสอบให้แน่ใจว่า Idea พร้อมและเหมาะสมก่อนลงทุนเวลาเขียน Script

**ตำแหน่งใน Workflow:** ระหว่าง "Approved" → "Script Draft"

### ผู้อนุมัติ (Who Approves)
- Content Strategist (Primary)
- Owner เฉพาะกรณีที่ Risk Note ระดับ High

### สิ่งที่ต้องตรวจ (What to Check)

| รายการตรวจ | วิธีตรวจ |
|-----------|---------|
| ✅ Idea มี Hook ชัดเจน | อ่าน Hook แล้วเข้าใจใน 3 วินาทีหรือไม่ |
| ✅ Platform ระบุชัด | ดูใน Idea Bank ว่ากรอกแล้ว |
| ✅ Risk Note ตรวจแล้ว | ไม่มี claim ที่ยังไม่ได้รับการอนุมัติ |
| ✅ ไม่ซ้ำกับคอนเทนต์ที่ทำไปแล้วใน 30 วัน | ตรวจ Content Pipeline |
| ✅ ทรัพยากร (คน/เวลา) พร้อม | Strategist ยืนยันว่า Writer และ Editor พร้อม |

### เกณฑ์ผ่าน (Pass Criteria)
- ✅ ตรวจครบทุกรายการ
- ✅ Risk Note = "Low" หรือ "Medium" และมีแผนจัดการ
- ✅ Idea Score ≥ 6/10

### เกณฑ์ Reject (Reject Criteria)
- ❌ ไม่มี Hook ชัดเจน
- ❌ Risk Note = "High" โดยไม่มีแผนจัดการ
- ❌ ซ้ำกับคอนเทนต์ที่เพิ่ง Publish ไปใน 7 วัน
- ❌ ทรัพยากรไม่พร้อม

### หมายเหตุที่ต้องบันทึก (Required Notes)
- ผู้ที่ให้ Approve: ชื่อ + วันที่
- Risk Note สุดท้าย
- ถ้า Reject: เหตุผลชัดเจน + suggestion

---

## Gate 2: Public Posting Gate

**จุดประสงค์:** ตรวจสอบคอนเทนต์ final ก่อน Publish สาธารณะ

**ตำแหน่งใน Workflow:** ระหว่าง "Final Review" → "Scheduled"

### ผู้อนุมัติ (Who Approves)
- Owner (Primary สำหรับ Harmony Sauna, AMP Pattaya)
- Content Strategist (สำหรับ ธรรมะดีดี, Affiliate/AI Automation)
- Owner ต้อง Approve ทุกชิ้นสำหรับ The World Freight

### สิ่งที่ต้องตรวจ (What to Check)

| รายการตรวจ | วิธีตรวจ |
|-----------|---------|
| ✅ เนื้อหาตรงกับ Script ที่ Approved | เปรียบเทียบวิดีโอกับ Script |
| ✅ Brand Guidelines ถูกต้อง | โลโก้, สี, Font ตาม Brand Playbook |
| ✅ ไม่มีข้อมูลผิดพลาด | ตรวจ Facts, ราคา, ชื่อ, วันที่ |
| ✅ Caption ถูกต้องตาม Platform | ความยาว, Hashtag, Format |
| ✅ CTA ชัดเจนและถูกต้อง | Link ทำงาน, เบอร์โทรถูก |
| ✅ ไม่มี Copyright issues | Music, Image, Footage |
| ✅ ไม่มีข้อมูลส่วนตัวที่ไม่ได้รับอนุญาต | หน้าคน, ชื่อ, เบอร์โทร |

### เกณฑ์ผ่าน (Pass Criteria)
- ✅ ผ่านทุกรายการตรวจ
- ✅ ไม่มี claim ที่อาจทำให้เกิดปัญหา legal
- ✅ Visual Quality ผ่านมาตรฐาน (ไม่พร่ามัว, เสียงไม่แตก)

### เกณฑ์ Reject (Reject Criteria)
- ❌ เนื้อหาผิดพลาด (ราคา, วันที่, ข้อมูล)
- ❌ Claim ที่ไม่ได้รับการ Approve (ดู Gate 5 และ 6)
- ❌ Visual Quality ต่ำกว่ามาตรฐาน
- ❌ ไม่มี CTA หรือ CTA ผิด

### หมายเหตุที่ต้องบันทึก (Required Notes)
- Approved By: ชื่อ + วันที่ + เวลา
- Platform ที่จะ Publish
- ถ้า Reject: Screenshot + คำอธิบายชัดเจน

---

## Gate 3: Ads Usage Gate

**จุดประสงค์:** ตรวจสอบคอนเทนต์ก่อนนำไปใช้เป็นโฆษณา (Paid Ads)

**ตำแหน่งใน Workflow:** แยกจาก Organic Content Pipeline — ต้องผ่าน Gate เพิ่มเติม

### ผู้อนุมัติ (Who Approves)
- Owner (ทุกแบรนด์ ต้อง Owner Approve เท่านั้น)

### สิ่งที่ต้องตรวจ (What to Check)

| รายการตรวจ | วิธีตรวจ |
|-----------|---------|
| ✅ ไม่มี claim สุขภาพที่ไม่มีหลักฐาน | ตรวจ Script + Visual |
| ✅ ไม่มี claim การเงิน/การลงทุนที่รับประกัน | ตรวจ Script + Caption |
| ✅ Target Audience ใน Ads Manager ถูกต้อง | Owner ยืนยัน |
| ✅ Budget Approved | Owner อนุมัติ Budget |
| ✅ Landing Page / CTA พร้อม | ทดสอบ Link |
| ✅ Disclosure ตามกฎหมายกำกับโฆษณา | เช่น "#โฆษณา" สำหรับ Influencer content |

### เกณฑ์ผ่าน (Pass Criteria)
- ✅ Owner Approve เป็นลายลักษณ์อักษร (ใน Google Sheets หรือ Chat)
- ✅ Budget กำหนดแล้ว
- ✅ ไม่มี Sensitive Claims

### เกณฑ์ Reject (Reject Criteria)
- ❌ ไม่ได้รับ Owner Approve
- ❌ มี Medical/Financial claim
- ❌ Landing Page ยังไม่พร้อม

### หมายเหตุที่ต้องบันทึก (Required Notes)
- Budget: จำนวน + ระยะเวลา
- Target Audience: รายละเอียด
- Owner Approval: ชื่อ + วันที่

---

## Gate 4: Customer-Facing Content Gate

**จุดประสงค์:** คอนเทนต์ที่ลูกค้าเจาะจงจะเห็น (เช่น DM auto-reply, email, brochure ดิจิทัล)

**ตำแหน่งใน Workflow:** ก่อน publish/ส่ง ทุกชิ้น

### ผู้อนุมัติ (Who Approves)
- Owner หรือ Designated Approver ที่ Owner มอบหมาย

### สิ่งที่ต้องตรวจ (What to Check)

| รายการตรวจ | วิธีตรวจ |
|-----------|---------|
| ✅ ข้อมูลการติดต่อถูกต้อง | เบอร์โทร, LINE, Email, ที่อยู่ |
| ✅ ราคา/โปรโมชั่นเป็นปัจจุบัน | ยืนยันกับ Owner ก่อนทุกครั้ง |
| ✅ Tone ตาม Brand Voice | ตรวจกับ Brand Playbook |
| ✅ ไม่ก่อให้เกิดความเข้าใจผิดเรื่องสินค้า/บริการ | อ่านจากมุมมองลูกค้า |
| ✅ มีช่องทางติดต่อกลับ | LINE OA, เบอร์โทร, etc. |

### เกณฑ์ผ่าน (Pass Criteria)
- ✅ ข้อมูลทุกอย่างเป็นปัจจุบันและถูกต้อง
- ✅ Tone ตรงกับ Brand Playbook

### เกณฑ์ Reject (Reject Criteria)
- ❌ ราคาผิดหรือล้าสมัย
- ❌ ข้อมูลติดต่อผิด
- ❌ สร้างความเข้าใจผิดเรื่องสินค้า/บริการ

### หมายเหตุที่ต้องบันทึก (Required Notes)
- วันที่ตรวจสอบ + ผู้ตรวจสอบ
- เวอร์ชันเอกสาร

---

## Gate 5: Sensitive Claims Gate

**จุดประสงค์:** ตรวจสอบคำกล่าวอ้างที่อาจก่อให้เกิดความเสี่ยง legal หรือ brand damage

**ใช้กับ:** ทุกแบรนด์ ทุกคอนเทนต์ที่มีคำกล่าวอ้าง

### ผู้อนุมัติ (Who Approves)
- Script Reviewer (Primary)
- Owner (กรณี Risk ระดับ High)

### Sensitive Claims ที่ต้องระวัง

| ประเภท Claim | ตัวอย่าง | Risk Level |
|-------------|---------|-----------|
| สรรพคุณสุขภาพ | "ช่วยรักษาโรค...", "ดีต่อสุขภาพอย่างพิสูจน์แล้ว" | High |
| ผลตอบแทนการลงทุน | "ROI 15% ต่อปีรับประกัน", "ราคาต้องขึ้นแน่ๆ" | High |
| Testimonial ที่ไม่ได้รับอนุญาต | ใช้ชื่อลูกค้าจริงโดยไม่ได้ขออนุญาต | High |
| เปรียบเทียบคู่แข่ง | "ดีกว่า [ชื่อบริษัทคู่แข่ง]" | Medium |
| ราคาที่ไม่แน่นอน | ราคาสินค้า/บริการที่อาจเปลี่ยนแปลง | Medium |
| Scarcity ปลอม | "เหลือแค่ 3 ที่!" โดยไม่เป็นความจริง | Medium |

### เกณฑ์ผ่าน (Pass Criteria)
- ✅ ไม่มี High Risk Claims
- ✅ Medium Risk Claims มีการ Disclaimer ชัดเจน
- ✅ Testimonials มีการขออนุญาตเป็นลายลักษณ์อักษร

### เกณฑ์ Reject (Reject Criteria)
- ❌ มี High Risk Claims โดยไม่มี evidence
- ❌ Testimonials ที่ไม่ได้รับอนุญาต
- ❌ Claim ที่อาจละเมิด พ.ร.บ. คุ้มครองผู้บริโภค

### หมายเหตุที่ต้องบันทึก (Required Notes)
- Claims ที่แก้ไข + วิธีแก้
- ถ้ายังคงไว้: เหตุผล + Evidence

---

## Gate 6: Medical / Financial / Investment / Freight Pricing Gate

**จุดประสงค์:** ตรวจสอบคำกล่าวอ้างเฉพาะทางที่อาจมีผลทางกฎหมาย

**ใช้กับ:** แบรนด์เฉพาะ — บังคับ ห้ามข้าม

### ผู้อนุมัติ (Who Approves)
- **Owner เท่านั้น** — ไม่มีการ delegate

### รายละเอียดตาม Brand

#### Harmony Sauna — Medical Gate
| รายการตรวจ | ตัวอย่างที่ห้าม | ตัวอย่างที่อนุญาต |
|-----------|--------------|----------------|
| สรรพคุณทางการแพทย์ | "รักษาโรค...", "ช่วยลดความดัน" | "ผ่อนคลาย", "รู้สึกสดชื่น" |
| Claims ที่ต้องอ้างอิง | ตัวเลขสถิติทางการแพทย์ | ประสบการณ์ทั่วไป |
| การเปรียบเทียบกับยา | ทุกกรณี | ไม่แนะนำ |

#### AMP Pattaya — Financial/Investment Gate
| รายการตรวจ | ตัวอย่างที่ห้าม | ตัวอย่างที่อนุญาต |
|-----------|--------------|----------------|
| รับประกัน ROI | "รับประกัน 15% ต่อปี" | "โอกาสสร้างรายได้จากการปล่อยเช่า" |
| ราคาที่แน่นอน | "ราคาจะขึ้น 20% ปีหน้า" | "ราคาเริ่มต้น ณ วันที่..." |
| ผลตอบแทนในอดีต | ใช้เป็นการรับประกันอนาคต | ระบุ "ผลในอดีตไม่รับประกันอนาคต" |

#### The World Freight — Freight Pricing Gate
| รายการตรวจ | ตัวอย่างที่ห้าม | ตัวอย่างที่อนุญาต |
|-----------|--------------|----------------|
| ราคา Freight จริง | ระบุตัวเลขที่แน่นอน | "ติดต่อขอ quotation" |
| Transit Time รับประกัน | "ถึงใน 7 วันแน่นอน" | "โดยทั่วไป 7-14 วัน ขึ้นกับปัจจัย..." |
| Customs Clearance | "รับประกัน clearance" | "ช่วยดำเนินการ clearance" |

#### ธรรมะดีดี — ไม่มี Medical/Financial Gate
- ตรวจตาม Gate 5 เท่านั้น
- ระวัง: ห้ามอ้างว่าธรรมะ "รักษา" โรคทางกายหรือจิตใจ

#### Affiliate/AI Automation — Financial Gate
| รายการตรวจ | ตัวอย่างที่ห้าม | ตัวอย่างที่อนุญาต |
|-----------|--------------|----------------|
| Get-rich-quick | "รวยง่ายๆ", "เงินออนไลน์ไม่ต้องทำอะไร" | "เรียนรู้การสร้างรายได้เพิ่ม" |
| รับประกันรายได้ | "รับประกัน 50,000/เดือน" | "บางคนสร้างรายได้ถึง..." |
| Affiliate Disclosure | ไม่แจ้งว่าเป็น affiliate link | ระบุ "#ad" หรือ "ลิงก์ affiliate" |

### เกณฑ์ผ่าน (Pass Criteria)
- ✅ Owner ตรวจและ Approve เป็นลายลักษณ์อักษร
- ✅ ไม่มีคำกล่าวอ้างที่ผิดกฎหมาย
- ✅ มี Disclaimer ที่เหมาะสม

### เกณฑ์ Reject (Reject Criteria)
- ❌ Claims ที่ละเมิดกฎหมาย พ.ร.บ. ที่เกี่ยวข้อง
- ❌ ไม่มี Owner Approval

### หมายเหตุที่ต้องบันทึก (Required Notes)
- Owner Name + วันที่ + เวลา
- Claims ที่ตรวจ + ผล (Pass/Reject)
- ถ้ามี Disclaimer: ข้อความ Disclaimer จริง

---

## Quick Reference — Approval Gate Matrix

| Content Type | Gate 1 | Gate 2 | Gate 3 | Gate 4 | Gate 5 | Gate 6 |
|-------------|--------|--------|--------|--------|--------|--------|
| Organic Short-form | ✅ | ✅ | - | - | ✅ | ตามแบรนด์ |
| Organic Long-form | ✅ | ✅ | - | - | ✅ | ตามแบรนด์ |
| Paid Ads | ✅ | ✅ | ✅ | - | ✅ | ตามแบรนด์ |
| Customer DM/Email | ✅ | ✅ | - | ✅ | ✅ | ตามแบรนด์ |
| Promotional Content | ✅ | ✅ | - | ✅ | ✅ | ตามแบรนด์ |

---

*อัปเดตล่าสุด: 2026-05-06 | AI Content Engine V1*
