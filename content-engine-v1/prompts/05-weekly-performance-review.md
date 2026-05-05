# Prompt 05 — Weekly Performance Review

> **วัตถุประสงค์:** วิเคราะห์ผลงานคอนเทนต์ประจำสัปดาห์ด้วย AI เพื่อหา Pattern และวางแผนสัปดาห์ถัดไป
>
> **วิธีใช้:** ทุกวันจันทร์ → รวบรวม KPI Data → Copy Prompt → วางใน ChatGPT หรือ Claude → บันทึกผลใน Weekly Review Sheet

---

## Input ที่ต้องกรอกก่อนใช้งาน

| Input | คำอธิบาย | แหล่งข้อมูล |
|-------|---------|-----------|
| **KPI Data** | ข้อมูลจาก KPI Tracker Sheet ของสัปดาห์นั้น | Google Sheets → KPI Tracker |
| **Published Content List** | รายการคอนเทนต์ที่ Publish ในสัปดาห์นั้น | Google Sheets → Publishing Calendar |
| **Platform** | Platform ที่ต้องการวิเคราะห์ | TikTok / Facebook / ฯลฯ |
| **Brand** | แบรนด์ที่ต้องการวิเคราะห์ | Harmony Sauna / ฯลฯ |
| **Week Range** | ช่วงสัปดาห์ | เช่น 6-12 พ.ค. 2026 |

---

## วิธีรวบรวม KPI Data ก่อนใช้ Prompt

### ขั้นตอนที่ 1: Export ข้อมูลจาก KPI Tracker Sheet
Copy ข้อมูลในรูปแบบนี้สำหรับแต่ละคอนเทนต์:

```
Content ID | Platform | Views | 3s Retention | Avg Watch Time | Completion Rate | Likes | Comments | Shares | Saves | Link Clicks | DMs | Leads | Content Score
```

### ขั้นตอนที่ 2: Note สิ่งพิเศษที่เกิดขึ้นในสัปดาห์นั้น
- คอนเทนต์ไหน Viral หรือทำได้ดีผิดคาด?
- คอนเทนต์ไหนทำได้แย่กว่าที่คาด?
- มี External factor ไหมที่อาจส่งผล? (วันหยุด, ข่าวด่วน, ฯลฯ)

---

## Prompt Template (Copy-Paste Ready)

```
คุณเป็น Content Performance Analyst ผู้เชี่ยวชาญด้าน Social Media Analytics

ฉันต้องการวิเคราะห์ผลงานคอนเทนต์ประจำสัปดาห์ดังนี้:

สัปดาห์: [WEEK_RANGE]
Brand: [BRAND_NAME]
Platform: [PLATFORM]

**KPI Data ของสัปดาห์นี้:**
[PASTE_KPI_DATA_TABLE_HERE]

**รายการคอนเทนต์ที่ Publish:**
[LIST_OF_PUBLISHED_CONTENT]

**หมายเหตุพิเศษสัปดาห์นี้:**
[SPECIAL_NOTES_E.G._HOLIDAY_EVENTS]

กรุณาวิเคราะห์และสรุปในรูปแบบ:

---

**1. TOP PERFORMERS (3 อันดับแรก):**
สำหรับแต่ละอันดับ ระบุ:
- Content ID:
- ทำไมทำได้ดี (Hook? Format? Timing? Topic?):
- KPI ที่โดดเด่น:
- สิ่งที่ควรทำซ้ำ:

---

**2. FAILED CONTENT (3 อันดับแย่สุด):**
สำหรับแต่ละอันดับ ระบุ:
- Content ID:
- สาเหตุที่น่าจะล้มเหลว (Hook อ่อน? เวลา? Topic? Format?):
- KPI ที่ต่ำกว่ามาตรฐาน:
- สิ่งที่ควรหลีกเลี่ยง:

---

**3. PATTERNS FOUND (รูปแบบที่พบ):**
- Pattern เชิงบวก (สิ่งที่ทำแล้วได้ผล):
- Pattern เชิงลบ (สิ่งที่ทำแล้วไม่ได้ผล):
- Pattern ด้าน Timing/วันเวลา:
- Pattern ด้าน Format:
- Pattern ด้าน Hook:

---

**4. WHAT TO REPEAT (สิ่งที่ควรทำซ้ำสัปดาห์หน้า):**
- Format:
- Hook Type:
- Topic Area:
- Posting Time:

---

**5. WHAT TO STOP (สิ่งที่ควรหยุดทำ):**
- Format:
- Hook Type:
- Topic Area:
- Posting Time:

---

**6. NEXT WEEK RECOMMENDATIONS:**
- จำนวนคอนเทนต์แนะนำ:
- Mix ที่แนะนำ (เช่น 3 Education, 2 Promotion):
- Topic แนะนำ 3-5 ชิ้น:
- Platform Focus:
- สิ่งที่ต้องทดลอง (A/B Test):

---

**7. CONTENT SCORE SUMMARY:**
- Average Content Score สัปดาห์นี้:
- เปรียบเทียบกับสัปดาห์ที่แล้ว (ถ้ามีข้อมูล):
- Trend (ขึ้น/ลง/เท่าเดิม):
```

---

## ตัวอย่าง Output — Harmony Sauna สัปดาห์ที่ 19

**Input Data (ตัวอย่าง):**

| Content ID | Platform | Views | 3s Ret | Avg Watch | Completion | Likes | Comments | Shares | Score |
|-----------|---------|-------|--------|-----------|-----------|-------|---------|-------|-------|
| HS-001 | TikTok | 15,400 | 72% | 38s | 48% | 520 | 67 | 35 | 85 |
| HS-002 | TikTok | 2,100 | 31% | 12s | 18% | 45 | 8 | 3 | 22 |
| HS-003 | Facebook | 8,200 | - | - | - | 280 | 92 | 58 | 74 |
| HS-004 | TikTok | 6,800 | 58% | 29s | 38% | 190 | 33 | 14 | 63 |
| HS-005 | Instagram | 3,900 | - | - | - | 180 | 22 | 9 | 48 |

---

**ตัวอย่าง Output จาก AI:**

**1. TOP PERFORMERS:**

อันดับ 1 — HS-001 (Score: 85)
- ทำไมทำได้ดี: Hook "Personal Story" + 3s Retention 72% สูงมาก, Format 60s เหมาะกับ TikTok
- KPI โดดเด่น: Views 15,400, Completion 48%, Comments 67
- ควรทำซ้ำ: Hook แบบ Personal Story + Tip List format

อันดับ 2 — HS-003 (Score: 74)
- ทำไมทำได้ดี: Facebook Post แบบ Educational ได้ Shares สูง (58) = Organic reach ดี
- KPI โดดเด่น: Comments 92, Shares 58
- ควรทำซ้ำ: Educational format บน Facebook

อันดับ 3 — HS-004 (Score: 63)
- ทำไมทำได้ดี: How-to content ทำได้ปานกลาง-ดี Completion 38% โอเค
- ควรทำซ้ำ: How-to format แต่ปรับ Hook ให้แข็งแกร่งขึ้น

**2. FAILED CONTENT:**

อันดับแย่สุด — HS-002 (Score: 22)
- สาเหตุ: 3s Retention เพียง 31% — Hook อ่อนมาก ไม่ดึงดูดในวินาทีแรก
- ควรหลีกเลี่ยง: เปิดด้วยการแนะนำตัว/แบรนด์ก่อน

**3. PATTERNS:**
- ✅ Personal Story Hook > Informational Hook (2x ผลลัพธ์)
- ✅ TikTok 60s > TikTok 30s สัปดาห์นี้
- ❌ Instagram Reels ได้ผลต่ำกว่าทุก Platform
- 📅 โพสต์วันพฤหัส 18:00 ทำได้ดีกว่าวันจันทร์

**4. WHAT TO REPEAT:**
- Hook แบบ Personal Story หรือ "คุณรู้ไหม..."
- TikTok 60s format
- Facebook Educational Post
- โพสต์เวลา 17:00-19:00

**5. WHAT TO STOP:**
- Hook แบบ Brand Introduction ในวินาทีแรก
- Instagram Reels (พักก่อน 2 สัปดาห์)

**6. NEXT WEEK RECOMMENDATIONS:**
- จำนวน: 6 คอนเทนต์
- Mix: 3 TikTok (Personal Story + How-to + Trend), 2 Facebook Educational, 1 Promotion
- Topics แนะนำ: Sauna กับการนอนหลับ, Behind the Scene, Before/After Experience
- A/B Test: เปรียบเทียบ Hook 2 แบบสำหรับ TikTok

---

## Benchmark KPI แนะนำต่อ Platform

| Platform | Views ดี | 3s Retention ดี | Completion ดี | Content Score ดี |
|---------|---------|----------------|-------------|----------------|
| TikTok | >5,000 | >60% | >40% | >60 |
| Facebook | >3,000 reach | N/A | N/A | >55 |
| Instagram | >2,000 | >50% | >35% | >55 |
| YouTube Shorts | >1,000 | >65% | >45% | >60 |

> หมายเหตุ: Benchmark นี้เป็นแนวทางเริ่มต้นสำหรับแบรนด์ใหม่ ควรปรับตามผลจริงของแต่ละแบรนด์

---

## Checklist วัน Weekly Review (ทุกวันจันทร์)

- [ ] Export KPI ครบทุกคอนเทนต์ที่ Publish ในสัปดาห์ที่ผ่านมา
- [ ] Note External Factors พิเศษ
- [ ] Run Prompt 05 สำหรับทุกแบรนด์
- [ ] กรอกผลลัพธ์ใน Weekly Review Sheet
- [ ] อัปเดต Repurpose Decision ใน KPI Tracker
- [ ] ส่ง Summary ให้ Owner ภายใน 12:00 วันจันทร์

---

*อัปเดตล่าสุด: 2026-05-06 | AI Content Engine V1*
