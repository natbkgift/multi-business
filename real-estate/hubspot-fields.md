# HubSpot Custom Fields | Custom Properties สำหรับ Real Estate

## วิธีสร้าง Custom Properties
1. ไปที่ Settings (⚙️) → Properties
2. เลือก **Contact** properties
3. คลิก **Create property**
4. กรอก Field name, Label, Field type ตามตารางด้านล่าง

---

## Contact Properties ที่ต้องสร้าง

| Field Label | Internal Name | Field Type | Options (ถ้ามี) | Required? |
|-------------|--------------|------------|-----------------|----------|
| Lead Source | `lead_source_detail` | Dropdown | Facebook Ad, LINE OA, Website Form, Referral, Other | Yes |
| Property Interest | `property_interest` | Multi-line text | - | Yes |
| Budget Range Min (THB) | `budget_min_thb` | Number | - | Yes |
| Budget Range Max (THB) | `budget_max_thb` | Number | - | Yes |
| Purchase Timeline | `purchase_timeline` | Dropdown | Within 1 Month, 1-3 Months, 3-6 Months, 6-12 Months, Just Looking | Yes |
| Preferred Location | `preferred_location` | Single-line text | - | No |
| Property Type | `property_type` | Checkbox | Condo, House, Townhouse, Commercial, Land | Yes |
| Lead Score | `lead_score_auto` | Number | - | No (calculated) |
| Lead Temperature | `lead_temperature` | Dropdown | Hot 🔥, Warm 🌡️, Cool ❄️, Cold 🧊 | No (calculated) |
| Assigned Agent | `assigned_agent` | Single-line text | - | No |
| Assignment Approved By | `assignment_approved_by` | Single-line text | - | No |
| Assignment Date | `assignment_date` | Date | - | No |
| Last Follow-up Date | `last_followup_date` | Date | - | No |
| Follow-up Count | `followup_count` | Number | - | No |
| Notes for Agent | `notes_for_agent` | Multi-line text | - | No |
| LINE User ID | `line_user_id` | Single-line text | - | No |
| Facebook Lead ID | `facebook_lead_id` | Single-line text | - | No |
| Viewing Scheduled Date | `viewing_scheduled_date` | Date | - | No |
| Offer Amount (THB) | `offer_amount_thb` | Number | - | No |
| Deal Closed Date | `deal_closed_date` | Date | - | No |

---

## Pipeline Stages

ไปที่ Settings → Deals → Pipelines → **สร้าง Pipeline ใหม่** ชื่อ "Real Estate Sales"

| Stage Name | Stage ID | Probability |
|-----------|---------|-------------|
| New Lead | `new_lead` | 10% |
| Contacted | `contacted` | 20% |
| Viewing Scheduled | `viewing_scheduled` | 40% |
| Offer Made | `offer_made` | 60% |
| Negotiating | `negotiating` | 75% |
| Closed Won | `closedwon` | 100% |
| Closed Lost | `closedlost` | 0% |

---

## Workflow Automation ใน HubSpot (Free Tier)

HubSpot Free ไม่มี Workflow automation ที่ซับซ้อน ใช้ Make.com แทน:

- เมื่อ Lead Score ≥ 80: ส่ง LINE Notify ให้ Manager
- เมื่อ Stage = Contacted ≥ 48h: ส่ง follow-up reminder
- เมื่อ Viewing Scheduled Date = วันนี้: ส่ง reminder ให้ agent
