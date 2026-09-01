# 🤖 n8n Order Automation — End-to-end order processing workflow

A complete n8n workflow that automates order processing from start to finish, with human-in-the-loop email validation and multi-destination storage.

---
## 💼 Business use case

This workflow is designed for businesses that process orders manually and need a reliable approval step before final processing.

It can help automate repetitive tasks such as:

* Reading and processing new orders automatically
* Generating clear AI-powered order summaries
* Requesting human approval before completing an action
* Creating standardized documentation
* Storing validated data across multiple business tools
* Updating order statuses automatically

The workflow can be adapted to different business processes such as order management, lead validation, support requests, internal approvals or document processing.

---

## ✨ What this workflow does

```
┌─────────────┐    ┌──────────────┐    ┌──────────┐    ┌───────────┐    ┌──────────────┐
│  Schedule   │───▶│ Google Sheet │───▶│ Groq AI  │───▶│  Gmail    │───▶│     Wait     │
│  Trigger    │    │  (read rows) │    │ (summary)│    │  (email)  │    │ (human input)│
└─────────────┘    └──────────────┘    └──────────┘    └───────────┘    └──────┬───────┘
                                                                                 │
                                                                          ┌──────▼───────┐
                                                                          │      If      │
                                                                          │  Approved?   │
                                                                          └──┬───────┬───┘
                                                                    Yes      │       │ No
                                                                     ◀───────┘       ▼
                                                                                 Rejected
┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│   Airtable   │◀───│  JavaScript  │◀───│  Google Doc  │◀── (Approved path)
│ Create record│    │ Parse actions│    │  Create doc  │
└──────┬───────┘    └──────────────┘    └──────────────┘
       │
       ▼
┌──────────────┐    ┌──────────────────────┐
│   Sheets     │───▶│        Sheets        │
│  Append row  │    │  Status → processed  │
└──────────────┘    └──────────────────────┘
```

### Step by step

| # | Step | Description |
|---|------|-------------|
| 1 | **Read orders** | Fetches new orders from Google Sheets |
| 2 | **AI summary** | Sends to Groq to generate a structured summary |
| 3 | **Validation email** | Gmail sends an email with **Approve** / **Reject** links |
| 4 | **Wait node** | Workflow pauses and waits for the human decision |
| 5 | **Google Doc** | Creates a document with the validated summary |
| 6 | **JS parsing** | Extracts each action via a custom JavaScript node |
| 7 | **Storage** | Saves to Airtable AND updates Google Sheets |
| 8 | **Status update** | Marks the order as `processed` in the source sheet |

---

## 🛠️ Tech stack

- **Orchestration** — [n8n](https://n8n.io)
- **AI** — [Groq](https://groq.com) (ultra-fast LLM)
- **Storage** — Airtable + Google Sheets
- **Docs** — Google Docs
- **Email** — Gmail
- **Custom logic** — JavaScript node

---

## 📋 Prerequisites

- n8n instance (self-hosted or cloud)
- Google account (Sheets, Docs, Gmail)
- Airtable account
- Groq API key (free)

---

## 🚀 Setup

1. **Clone this repo**
   ```bash
   git clone https://github.com/nass-devAI/n8n-order-automation.git
   ```

2. **Import the workflow**  
   In n8n → *Import from file* → select `workflow.json`

3. **Set up credentials**  
   - Google OAuth2 (Sheets, Docs, Gmail)
   - Airtable API Key
   - Groq API Key

4. **Update the IDs**  
   - Source Google Sheet ID
   - Airtable base ID
   - Validation email address

5. **Activate the workflow** and send a test order!

---

## 📁 Repo structure

```
├── workflow.json          # n8n workflow export
├── README.md
└── docs/
    └── schema.png         # Workflow diagram (optional)
```

---

## 💡 Ideas for improvement

- [ ] Add a Slack webhook for notifications
- [ ] Real-time order tracking dashboard
- [ ] Multi-language support for AI summaries
- [ ] Auto-retry on Groq errors

---

## 📄 License

MIT — free to use, modify, and share.

---

*Built with n8n + a bit of coffee ☕*
