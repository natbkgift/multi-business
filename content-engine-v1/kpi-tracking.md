# KPI Tracking — AI Content Engine V1

> เอกสารนี้อธิบายระบบวัดผลคอนเทนต์ทั้งหมด รวมถึงสูตร Content Score และวิธีตีความผลลัพธ์
>
> **วิธีใช้:** กรอก KPI ใน KPI Tracker Sheet ภายใน 48 ชั่วโมงหลัง Publish ทุกครั้ง

---

## KPI Fields ทั้งหมด

### กลุ่มที่ 1: Reach & Visibility

| KPI | คำอธิบาย | หน่วย | วิธีดู |
|-----|---------|------|-------|
| **Views** | จำนวนครั้งที่วิดีโอ/โพสต์ถูกแสดง | ตัวเลข | Platform Analytics |
| **Reach** (Facebook/Instagram) | จำนวนคนที่เห็นโพสต์ | ตัวเลข | Meta Business Suite |
| **Impressions** | จำนวนครั้งที่แสดงทั้งหมด (นับซ้ำได้) | ตัวเลข | Platform Analytics |

### กลุ่มที่ 2: Video Retention

| KPI | คำอธิบาย | หน่วย | Benchmark ดี |
|-----|---------|------|------------|
| **3-Second Retention** | % คนที่ดูผ่าน 3 วินาทีแรก | % | TikTok: >60%, YouTube: >70% |
| **Average Watch Time** | เวลาเฉลี่ยที่ดู | วินาที | ≥ 50% ของความยาวทั้งหมด |
| **Completion Rate** | % คนที่ดูจนจบ | % | >35% ถือว่าดี |

### กลุ่มที่ 3: Engagement

| KPI | คำอธิบาย | หน่วย | ความสำคัญ |
|-----|---------|------|----------|
| **Likes** | จำนวน Like | ตัวเลข | Signal พื้นฐาน |
| **Comments** | จำนวน Comment | ตัวเลข | Strong engagement signal |
| **Shares** | จำนวนแชร์ | ตัวเลข | Organic amplification |
| **Saves** | จำนวน Save (Instagram/TikTok) | ตัวเลข | High-intent signal |
| **Engagement Rate** | (Likes+Comments+Shares+Saves)/Views × 100 | % | >3% ถือว่าดี |

### กลุ่มที่ 4: Conversion

| KPI | คำอธิบาย | หน่วย | ความสำคัญ |
|-----|---------|------|----------|
| **Link Clicks** | คลิก Link ใน Bio/Caption/Story | ตัวเลข | Intent signal |
| **DM/Inquiries** | ข้อความถามซื้อ/สอบถามโดยตรง | ตัวเลข | Strong conversion signal |
| **Leads** | คนที่ลงทะเบียน/ขอ consultation จริง | ตัวเลข | Hard conversion |
| **Conversion Signal** | ประเมินโดยรวม | Dropdown | None/Weak/Moderate/Strong/Very Strong |

---

## สูตร Content Score

### สูตรหลัก

```
Content Score = (Reach Score × 25%) + (Engagement Score × 25%) + 
                (Conversion Signal × 30%) + (Reuse Potential × 20%)
```

**ช่วงคะแนน: 0-100**

---

### วิธีคำนวณแต่ละ Component

#### Component 1: Reach Score (25%)

ประเมินจาก Views/Reach เทียบกับ Benchmark ของ Platform และขนาด Audience

| ระดับ | เกณฑ์ (TikTok) | เกณฑ์ (Facebook) | เกณฑ์ (YouTube) | คะแนน |
|------|---------------|-----------------|----------------|------|
| Excellent | >50,000 Views | >20,000 Reach | >10,000 Views | 90-100 |
| Good | 10,000-50,000 | 5,000-20,000 | 2,000-10,000 | 70-89 |
| Average | 3,000-10,000 | 1,500-5,000 | 500-2,000 | 50-69 |
| Below Average | 1,000-3,000 | 500-1,500 | 100-500 | 30-49 |
| Poor | <1,000 | <500 | <100 | 0-29 |

> หมายเหตุ: สำหรับ Account ใหม่ (< 3 เดือน) ให้ลด Benchmark ลง 50%

#### Component 2: Engagement Score (25%)

ใช้ Engagement Rate และคุณภาพ Engagement

| ระดับ | Engagement Rate | Comment Quality | คะแนน |
|------|----------------|----------------|------|
| Excellent | >8% | มี Meaningful comments | 90-100 |
| Good | 5-8% | Comments ที่เกี่ยวข้อง | 70-89 |
| Average | 3-5% | Mix ของ Comments | 50-69 |
| Below Average | 1-3% | Comments น้อย หรือ Spam | 30-49 |
| Poor | <1% | แทบไม่มี Engagement | 0-29 |

#### Component 3: Conversion Signal (30%)

| Conversion Signal Level | คำอธิบาย | คะแนน |
|------------------------|---------|------|
| Very Strong | มี Leads จริง + DM สูง + Link Clicks | 90-100 |
| Strong | DM/Inquiries สูง + Link Clicks | 70-89 |
| Moderate | มี Link Clicks + บ้าง DM | 50-69 |
| Weak | Engagement แต่ไม่มี Conversion | 30-49 |
| None | ไม่มี Conversion signal เลย | 0-29 |

#### Component 4: Reuse Potential (20%)

ประเมินว่าคอนเทนต์นี้ Repurpose ต่อได้ไหม

| ระดับ | เกณฑ์ | คะแนน |
|------|------|------|
| High | ทำได้ดี + Topic ยังใหม่ + ขยายได้อีก | 80-100 |
| Medium | ทำได้ดีพอสมควร + ปรับเล็กน้อยได้ | 50-79 |
| Low | ทำได้แย่ หรือ Topic หมดอายุแล้ว | 0-49 |

---

### ตาราง Content Score และการตัดสินใจ

| คะแนน | ระดับ | สัญลักษณ์ | การตัดสินใจ (Repurpose Decision) |
|------|------|---------|--------------------------------|
| **80-100** | Excellent | ⭐ | **Repurpose + Scale** — ทำซ้ำทันที + ขยายไป Platform อื่น + เพิ่ม Paid |
| **60-79** | Good | ✅ | **Repeat** — ทำซ้ำใน Format/Topic เดียวกัน |
| **40-59** | Average | 🔄 | **Improve** — วิเคราะห์ว่าอะไรไม่ได้ผล แล้วปรับก่อนทำซ้ำ |
| **20-39** | Below Average | ⚠️ | **Review** — ทบทวนอย่างละเอียด อาจไม่ควรทำซ้ำ |
| **0-19** | Poor | 🛑 | **Stop** — หยุดทำ Format/Topic นี้ บันทึกใน "What to Stop" |

---

## ตัวอย่างการคำนวณจริง

### ตัวอย่าง 1: Harmony Sauna — TikTok 60s

| KPI | ค่าที่วัดได้ |
|-----|-----------|
| Views | 15,400 |
| 3s Retention | 72% |
| Avg Watch Time | 38s (จากทั้งหมด 60s) |
| Completion Rate | 48% |
| Likes | 520 |
| Comments | 67 |
| Shares | 35 |
| Saves | 89 |
| Link Clicks | 210 |
| DMs | 15 |
| Leads | 3 |
| Conversion Signal | Strong |

**การคำนวณ:**
- Reach Score: 15,400 Views = Good → คะแนน 75
- Engagement Score: (520+67+35+89)/15,400 = 4.7% → Good → คะแนน 72
- Conversion Signal: Strong (DM 15 + Leads 3) → คะแนน 80
- Reuse Potential: Topic ดี + Completion 48% + ขยายได้ → High → คะแนน 85

**Content Score = (75 × 25%) + (72 × 25%) + (80 × 30%) + (85 × 20%)**
= 18.75 + 18 + 24 + 17
= **77.75 → ปัดเป็น 78**

**ผล:** ✅ **Repeat** — ทำซ้ำ Format นี้ ปรับ Hook และ Topic ใหม่

---

### ตัวอย่าง 2: AMP Pattaya — Facebook Video

| KPI | ค่าที่วัดได้ |
|-----|-----------|
| Reach | 4,200 |
| Avg Watch Time | 45s (จากทั้งหมด 120s) |
| Likes | 89 |
| Comments | 34 |
| Shares | 18 |
| Link Clicks | 95 |
| DMs | 8 |
| Leads | 2 |
| Conversion Signal | Moderate |

**Content Score = ประมาณ 64** → ✅ Repeat

---

### ตัวอย่าง 3: ธรรมะดีดี — YouTube Shorts (ต่ำกว่าคาด)

| KPI | ค่าที่วัดได้ |
|-----|-----------|
| Views | 850 |
| 3s Retention | 35% |
| Completion | 22% |
| Likes | 12 |
| Comments | 3 |
| Conversion Signal | None |

**Content Score = ประมาณ 18** → 🛑 Stop — Hook ไม่แข็งแรง, Topic ไม่ตรงกลุ่ม

---

## คู่มือการกรอก KPI

### เมื่อไหร่ต้องกรอก KPI?

| Platform | กรอกครั้งแรก | กรอกครั้งสุดท้าย |
|---------|------------|----------------|
| TikTok | หลัง 24 ชั่วโมง | หลัง 7 วัน |
| Facebook | หลัง 48 ชั่วโมง | หลัง 7 วัน |
| Instagram | หลัง 24 ชั่วโมง | หลัง 7 วัน |
| YouTube Shorts | หลัง 48 ชั่วโมง | หลัง 14 วัน |
| YouTube Long-form | หลัง 72 ชั่วโมง | หลัง 30 วัน |

### วิธีกรอก Conversion Signal

| สถานการณ์ | Conversion Signal |
|---------|-----------------|
| ไม่มี DM ไม่มี Link Click เลย | None |
| มี Link Click แต่ไม่มี DM | Weak |
| มี DM 1-5 ราย หรือ Link Clicks สูง | Moderate |
| มี DM 6-15 ราย หรือมี Leads | Strong |
| มี Leads 3+ ราย หรือ Sales จริง | Very Strong |

---

## การใช้ KPI Data ใน Weekly Review

1. Export KPI Data จาก KPI Tracker Sheet
2. ใช้ Prompt 05 วิเคราะห์
3. บันทึกผลใน Weekly Review Sheet
4. อัปเดต Repurpose Decision
5. สร้าง Idea ใหม่ต่อยอดจาก Top Performers

---

*อัปเดตล่าสุด: 2026-05-06 | AI Content Engine V1*
