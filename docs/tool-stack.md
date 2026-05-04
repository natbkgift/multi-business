# Tool Stack | ชุดเครื่องมือและค่าใช้จ่าย

## เครื่องมือหลัก

| Category | Tool | Cost/month | ใช้กับระบบ | ลิงก์ |
|----------|------|-----------|-----------|-------|
| **Automation** | n8n Cloud | $20 (~700 ฿) | ทุกระบบ | [n8n.io](https://n8n.io) |
| **Automation** | Make.com Starter | $9 (~315 ฿) | Real Estate, Freight | [make.com](https://make.com) |
| **CRM** | HubSpot Free | ฟรี | Real Estate | [hubspot.com](https://hubspot.com) |
| **Messaging** | LINE OA Free | ฟรี (500 msg/เดือน) | Sauna, Freight | [manager.line.biz](https://manager.line.biz) |
| **Messaging** | LINE OA Light | 1,200 ฿ | เมื่อ traffic เพิ่ม | - |
| **Messaging** | Facebook Messenger | ฟรี | Real Estate | - |
| **Database** | Google Sheets | ฟรี (Workspace) | ทุกระบบ | - |
| **AI** | OpenAI GPT-4o | $20-50 (~700-1,750 ฿) | Content, Quote Draft | [openai.com](https://openai.com) |
| **AI** | Claude API (Anthropic) | $20-50 (~700-1,750 ฿) | SOP, Email Draft | [anthropic.com](https://anthropic.com) |
| **Document** | Google Docs/Slides | ฟรี (Workspace) | Quote PDF | - |
| **Form** | Google Forms | ฟรี | Lead Capture, Freight | - |
| **Dashboard** | Google Looker Studio | ฟรี | KPI Dashboard | [lookerstudio.google.com](https://lookerstudio.google.com) |
| **SOP/Wiki** | Notion | ฟรี (personal) / $8/user | Intern Training | [notion.so](https://notion.so) |
| **Video** | CapCut | ฟรี | Short-form Content | - |
| **Design** | Canva Pro | $13 (~455 ฿) | Thumbnails, Quotation | [canva.com](https://canva.com) |
| **Hosting** | VPS (n8n self-hosted) | ~300 ฿ | ทางเลือกแทน n8n Cloud | DigitalOcean/Vultr |

## สรุปค่าใช้จ่าย

| สถานการณ์ | ค่าใช้จ่าย/เดือน |
|-----------|----------------|
| **Minimum** (n8n self-hosted, LINE free, AI minimal) | ~1,500 ฿ |
| **Recommended** (n8n cloud + Make.com + LINE OA Light) | ~2,500-3,500 ฿ |
| **Full Stack** (ทุกอย่าง + AI Heavy) | ~4,000-6,000 ฿ |

## ลำดับการ Setup (Priority Order)

```
Phase 1 — เริ่มทำ Freight (สัปดาห์ที่ 1-2):
  1. Google Workspace (ฟรี)
  2. n8n Cloud หรือ Make.com ($9-20)
  3. LINE OA (ฟรี)

Phase 2 — Real Estate + Sauna (สัปดาห์ที่ 5-8):
  4. HubSpot CRM (ฟรี)
  5. Facebook Business Manager (ฟรี)

Phase 3 — Content + Full Automation (สัปดาห์ที่ 9-12):
  6. OpenAI API ($20+)
  7. Canva Pro ($13)
  8. Looker Studio (ฟรี)
```

## ทางเลือก Self-Hosted (ประหยัดกว่า)

หากต้องการลดต้นทุน:
- **n8n self-hosted** บน VPS 300฿/เดือน แทนที่ n8n Cloud $20
- **Supabase** (ฟรี tier) แทน Google Sheets สำหรับ data ขนาดใหญ่
- **Chatwoot** (open-source) แทน HubSpot สำหรับ messaging CRM

## API Keys ที่ต้องเตรียม

```
ระบบ Freight:
  - Google Sheets API (Service Account JSON)
  - Google Docs API
  - LINE Messaging API (Channel Access Token)
  - SMTP Email (Gmail App Password)

ระบบ Real Estate:
  - Facebook Graph API (Page Access Token)
  - HubSpot Private App Token
  - Make.com Webhook URL

ระบบ Sauna:
  - LINE Messaging API (Channel Access Token + Channel Secret)
  - LINE OA Webhook URL

ระบบ Content:
  - OpenAI API Key
  - YouTube Data API v3
  - Google Trends (ผ่าน pytrends หรือ unofficial API)
```
