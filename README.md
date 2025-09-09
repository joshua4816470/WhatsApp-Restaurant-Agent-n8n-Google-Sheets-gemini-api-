# WhatsApp Restaurant Agent (n8n + Google Sheets)

Conversational WhatsApp assistant for a restaurant, built on **n8n** with an **AI Agent (Gemini)**, **Simple Memory**, and **Google Sheets** as the lightweight database.  
Customers can ask FAQs, check the menu/stock, and place orders. Confirmed orders are appended to an **Orders** sheet.



##  Features
- WhatsApp conversation (Meta WhatsApp Business Cloud API)
- AI Agent with tool use (Gemini via n8n “Google Gemini Chat Model”)
- Short-term memory per user (Simple Memory)
- Google Sheets as storage:
  - **Inventory** (menu, quantities, availability)
  - **FAQ** (question/answer list)
  - **Orders** (appended when an order is confirmed)
- Natural order flow: collect **name → item → quantity → check stock → confirm → write order**
- Robust sending: always returns a WhatsApp reply even when tool outputs vary

- 
---

## 📊 Google Sheets Schema

Create one spreadsheet with three tabs:

**Inventory**
| Food Item     | Quantity | Status       |
|---------------|--------:|--------------|
| Mutton Thali  |      12 | Available    |
| Extra Rice    |       0 | Out of Stock |

**FAQ**
| Question                         | Answer                                              |
|----------------------------------|-----------------------------------------------------|
| What are your opening hours?     | We are open daily from 11:00 AM to 10:00 PM.       |

**Orders**
| Customer Name | Food Item     | Quantity Ordered | Order Date  | Status    |
|---------------|---------------|-----------------:|-------------|-----------|
| Sam           | Mutton Thali  |                2 | 2025-08-19  | Confirmed |

> Column names must match exactly.

---

## Getting Started

### 1) WhatsApp Business Cloud
- In **Meta Business Manager** create a **System User** and generate a long-lived token with:
  - `whatsapp_business_management`
  - `whatsapp_business_messaging`
- Note your **Phone Number ID** and **WABA ID**.

### 2) n8n
- Use **n8n Cloud** or self-host (Docker).
- Create **Credentials → WhatsApp account** and paste the system-user token.

### 3) Google Sheets
- Create OAuth credentials (or use Service Account) and add them in **Credentials**.
- If OAuth: add the n8n **Redirect URI** shown in the credential form.

### 4) Import the workflow
- Import your JSON (e.g., `My workflow copy.json`) into n8n.
- Wire the credentials on the three Sheets nodes (**INFO**, **GET FAQ**, **update**) and the **Send message** node.

---

## ⚙ Node Configuration

### AI Agent → **Settings**
- **Tool choice:** `Auto` (or `Always use tools/Required`)
- **Max tool calls per turn:** `2`

### AI Agent → **System Message** (copy-paste)

