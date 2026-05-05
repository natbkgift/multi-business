# Lead Scoring Formula | สูตรคำนวณคะแนน Lead

## หลักการ Lead Scoring

Lead Score รวม = Budget Score + Timeline Score + Engagement Score + Source Score

**คะแนนรวม 0-100:**
- 80-100: 🔥 **Hot Lead** — Priority 1, ติดต่อภายใน 1 ชั่วโมง
- 60-79: 🌡️ **Warm Lead** — Priority 2, ติดต่อภายใน 24 ชั่วโมง
- 40-59: ❄️ **Cool Lead** — เข้า nurture email sequence
- 0-39: 🧊 **Cold Lead** — เก็บ database, remarketing ทีหลัง

---

## 1. Budget Score (0-30 คะแนน)

| งบประมาณ (THB) | คะแนน |
|----------------|-------|
| > 10,000,000 | 30 |
| 5,000,001 - 10,000,000 | 25 |
| 3,000,001 - 5,000,000 | 20 |
| 1,500,001 - 3,000,000 | 15 |
| 800,001 - 1,500,000 | 10 |
| 500,001 - 800,000 | 5 |
| < 500,000 | 0 |

## 2. Timeline Score (0-30 คะแนน)

| ระยะเวลาการซื้อ | คะแนน |
|----------------|-------|
| Within 1 Month | 30 |
| 1-3 Months | 25 |
| 3-6 Months | 15 |
| 6-12 Months | 10 |
| Just Looking | 0 |

## 3. Engagement Score (0-25 คะแนน)

| พฤติกรรม | คะแนน |
|---------|-------|
| ตอบ auto-reply ภายใน 5 นาที | +15 |
| ตอบ auto-reply ภายใน 1 ชั่วโมง | +10 |
| ระบุที่ตั้งที่ต้องการ | +5 |
| ระบุประเภทอสังหาฯ | +3 |
| มีประวัติติดต่อมาก่อน | +5 |
| กรอกข้อมูลครบทุก field | +2 |

## 4. Lead Source Score (0-15 คะแนน)

| แหล่งที่มา | คะแนน | เหตุผล |
|-----------|-------|--------|
| Referral | 15 | ลูกค้าแนะนำ = intent สูง |
| LINE OA (Organic) | 12 | ค้นหาเอง = intent สูง |
| Website Form | 10 | Intent ชัดเจน |
| Facebook Lead Ad | 8 | อาจ impulse click |
| Facebook Organic | 5 | Comment/Message |
| Other | 3 | ไม่ทราบแหล่งที่มา |

---

## Google Sheets Formula

ใส่ใน Column `Lead Score` ใน sheet `Leads`:

```excel
=MIN(100,
  IF(E2>10000000,30, IF(E2>5000000,25, IF(E2>3000000,20, IF(E2>1500000,15, IF(E2>800000,10, IF(E2>500000,5,0)))))) +
  IF(F2="Within 1 Month",30, IF(F2="1-3 Months",25, IF(F2="3-6 Months",15, IF(F2="6-12 Months",10,0)))) +
  IF(G2="Referral",15, IF(G2="LINE OA",12, IF(G2="Website Form",10, IF(G2="Facebook Lead Ad",8,3))))
)
```
*(E2=Budget, F2=Timeline, G2=Source)*

---

## Lead Temperature Formula

```excel
=IF(H2>=80,"🔥 Hot",IF(H2>=60,"🌡️ Warm",IF(H2>=40,"❄️ Cool","🧊 Cold")))
```
*(H2=Lead Score)*
