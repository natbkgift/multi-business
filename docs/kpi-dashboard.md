# KPI Dashboard | ตัวชี้วัดและเกณฑ์ความสำเร็จ

## วิธีสร้าง Dashboard

ใช้ **Google Looker Studio** เชื่อมกับ Google Sheets แต่ละ sheet:
1. ไปที่ [lookerstudio.google.com](https://lookerstudio.google.com)
2. สร้าง Data Source → Google Sheets → เลือก sheet ที่ต้องการ
3. สร้าง Chart ตาม metrics ด้านล่าง

---

## KPI ต่อระบบ

### 🚢 Freight Forwarding

| Metric | เป้าหมาย 30 วัน | เป้าหมาย 90 วัน | วิธีวัด |
|--------|----------------|----------------|--------|
| Quote Response Time | < 2 ชั่วโมง | < 30 นาที | (Timestamp Sent - Timestamp Received) |
| Quote-to-Booking Rate | 20% | 35% | Bookings / Quotes × 100 |
| Quotes per Week | +50% vs baseline | +100% | นับ rows ที่ status = Sent |
| Revenue per Route | Track | Optimize top 3 routes | SUM(Revenue) GROUP BY Route |
| Quote Accuracy | 100% | 100% | Complaints / Quotes × 100 |

**Google Sheets Formula ตัวอย่าง:**
```
=COUNTIF(FreightLog!K:K,"Accepted")/COUNTIF(FreightLog!K:K,"<>"&"")*100
```

---

### 🏠 Real Estate

| Metric | เป้าหมาย 30 วัน | เป้าหมาย 90 วัน | วิธีวัด |
|--------|----------------|----------------|--------|
| Lead Response Time | < 5 นาที | < 2 นาที | HubSpot Report: Time to First Contact |
| Lead-to-Viewing Rate | 15% | 25% | Viewings Scheduled / Total Leads × 100 |
| Lead-to-Sale Rate | 5% | 10% | Closed Deals / Total Leads × 100 |
| Cost per Lead | Track | Reduce 30% | Ad Spend / Total Leads |
| Lead Source Mix | - | FB 50%, LINE 30%, Other 20% | COUNTIF by Source |

---

### 🧖 Harmony Sauna

| Metric | เป้าหมาย 30 วัน | เป้าหมาย 90 วัน | วิธีวัด |
|--------|----------------|----------------|--------|
| LINE Conversion Rate | 15% | 25% | Bookings / LINE Inquiries × 100 |
| Repeat Booking Rate | 30% | 50% | Repeat Customers / Total Customers × 100 |
| Booking Confirmation Rate | 90% | 95% | Confirmed / Total Bookings × 100 |
| Coupon Redemption Rate | 40% | 60% | Used Coupons / Issued Coupons × 100 |
| Average Revenue per Visit | Track | Increase 20% | SUM(Revenue) / COUNT(Visits) |

---

### 🎬 AI Content

| Metric | เป้าหมาย 30 วัน | เป้าหมาย 90 วัน | วิธีวัด |
|--------|----------------|----------------|--------|
| Videos Published per Month | 4 | 12 | COUNTIF(ContentCalendar!H:H,"Published") |
| Production Time per Video | -40% vs baseline | -60% | Track manual hours in sheet |
| YouTube Subscribers Growth | +500 | +2,000 | YouTube Studio API |
| Avg View Duration | > 40% | > 50% | YouTube Analytics |
| Content Approval Cycle | < 48h | < 24h | Approval Date - Draft Date |

---

### 📚 SOP / Intern

| Metric | เป้าหมาย 30 วัน | เป้าหมาย 90 วัน | วิธีวัด |
|--------|----------------|----------------|--------|
| Onboarding Completion Rate | 100% | 100% | Completed Modules / Total Modules |
| Time to Productivity | < 7 วัน | < 5 วัน | First Solo Task Date - Start Date |
| Quiz Pass Rate | 80% | 90% | Pass / Attempts × 100 |
| SOP Update Frequency | Monthly | Bi-weekly | Count SOP revision dates |

---

### ⚙️ Operations (ทุกระบบ)

| Metric | เป้าหมาย 30 วัน | เป้าหมาย 90 วัน |
|--------|----------------|----------------|
| Manual Hours Saved/Week | 10 ชั่วโมง | 25 ชั่วโมง |
| Automation Uptime | 95% | 99% |
| Error Rate (workflow fails) | < 5% | < 1% |
| Team Adoption Rate | 80% | 100% |

---

## โครงสร้าง Looker Studio Dashboard

### หน้าหลัก (Overview)
- Scorecard: Manual Hours Saved This Week
- Bar Chart: Quotes vs Bookings (Freight) — monthly
- Pie Chart: Lead Sources (Real Estate)
- Line Chart: Revenue Trend (All businesses) — monthly

### หน้า Freight
- Table: Quote Log with Status filters
- KPI Cards: Response Time, Conversion Rate, Revenue
- Bar Chart: Quotes by Route

### หน้า Real Estate
- Funnel: Lead → Contact → Viewing → Offer → Sale
- KPI Cards: Response Time, Leads This Week
- Pie Chart: Lead Source Distribution

### หน้า Sauna
- Calendar Heat Map: Booking Density
- KPI Cards: Conversion, Repeat Rate, Revenue
- Bar Chart: Package Popularity

### หน้า Content
- Table: Content Calendar with Status
- KPI Cards: Videos Published, Avg Views
- Line Chart: Subscriber Growth
