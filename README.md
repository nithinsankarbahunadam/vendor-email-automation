# 📧 Vendor Cold Email Automation with n8n

This repository contains a workflow for automating cold email outreach using [n8n](https://n8n.io/), Google Sheets, and Gmail. It helps streamline the process of sending personalized cold emails to vendors, tracking status, and updating records—all without writing repetitive emails manually.

---

## 🚀 Features

- ✅ Pull vendor records from Google Sheets
- 🧠 Skip already contacted vendors (`Send Status != SENT`)
- 🎯 Randomly select email templates
- ✍️ Personalize each email using `[First Name]`, `[Company]` tags
- 📤 Send emails through Gmail API
- 🗓️ Log `Send Status` and timestamp back into Google Sheets
- ⏳ Built-in wait/delay to prevent Gmail rate limits
- 🔁 Loop through batches until all vendors are processed

---

## 🛠️ Tech Stack

| Tool         | Purpose                             |
|--------------|-------------------------------------|
| n8n          | No-code workflow automation         |
| Google Sheets| Acts as input + output database     |
| Gmail API    | Sends personalized emails           |
| JavaScript   | Dynamic content generation in n8n   |

---

## 📂 Folder Structure

```
📁 vendor-email-automation/
├── workflow.json          # n8n export of the full automation workflow
├── README.md              # You're here
```

---

## 📸 Workflow Overview

### High-Level Steps:
1. **Manual Trigger** – Start the workflow from n8n.
2. **Read Google Sheet** – Get all vendor records from `Sheet1`.
3. **IF Node** – Filter only those not marked as `SENT`.
4. **Batching** – Process one vendor at a time.
5. **Fetch Templates** – From `Sheet2`, select a random email template.
6. **Personalize Content** – Replace placeholders with real data.
7. **Send Email** – Through Gmail node.
8. **Update Status** – Log `Send Status` as `SENT` and time back to Google Sheets.
9. **Wait Node** – Add delay to avoid hitting Gmail limits.
10. **Repeat** – Continue until all eligible rows are processed.

---

## 📝 How to Use

### Prerequisites:
- [n8n](https://docs.n8n.io/getting-started/installation/) installed (local or hosted)
- Connected **Google Sheets** and **Gmail** credentials in n8n
- A Google Sheet with vendor info (Email, First Name, Company, Send Status)
- A Google Sheet (can be second tab) with email templates (Subject, Body)

### Setup Instructions:
1. Clone the repo:
   ```bash
   git clone https://github.com/your-username/vendor-email-automation.git
   cd vendor-email-automation
   ```

2. Import `workflow.json` into n8n.

3. Connect the correct credentials for:
   - Google Sheets
   - Gmail

4. Configure your Google Sheet with:
   - `Sheet1`: Vendor details (Email, First Name, Company, Send Status)
   - `Sheet2`: Email templates (Subject, Body)

5. Run the workflow and watch your outreach go automated!

---

## 🧠 Customization Tips

- Add more personalization tokens (e.g., `[Last Name]`, `[Role]`) using the `Edit Fields` node
- Replace Gmail node with SendGrid/Mailgun for high-volume campaigns
- Integrate a suppression list to skip unsubscribed vendors
- Add a retry mechanism for failed sends

---

## 📜 License

MIT License © [Nithin Sankar]

---

## 🤝 Contributing

Contributions, suggestions, and improvements are welcome! Feel free to fork this project and submit a pull request.

---

## 💬 Questions?

Open an [Issue](https://github.com/your-username/vendor-email-automation/issues) or reach out via [LinkedIn](https://www.linkedin.com/in/nithinsankarbahunadam) if you have any questions or ideas.

---