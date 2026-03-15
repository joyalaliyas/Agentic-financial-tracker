# 🤖 Agentic Financial Tracker

An **AI-powered personal finance tracker** built using **n8n, Telegram, DeepSeek AI, OpenRouter, and MongoDB**.
This project allows users to log expenses, income, and transactions simply by sending **natural language messages** to a Telegram bot.

The system automatically **extracts, categorizes, and stores financial data** using a multi-agent AI pipeline and workflow automation.

---

## 🚀 Features

### 💬 Natural Language Expense Logging

Users can log transactions using simple messages:

```
Lunch at Cafe Coffee Day 350
Amazon shoes 1200
Petrol 500
Salary 30000
Paid Rahul 200
```

The system extracts:

* Item
* Category
* Amount
* Platform
* Payment Method
* Transaction Type

---

### 🧠 AI-Powered Data Extraction

The workflow uses an **LLM-based information extractor** to identify:

* Transaction type
  * Online
  * Daily
  * Income
  * Expense
* Spending item
* Amount
* Category
* Platform / Merchant
* Payment method
* Status (delivered, shipped, etc.)
* Notes

---

### 🤖 Multi-Agent AI System

The tracker uses several specialized AI agents:

* **Unified Extractor** — Parses free-form messages into structured transaction data
* **Category Normalization Agent** — Maps new categories to existing ones stored in MongoDB
* **Main Agent (Financial Advisor)** — Answers financial questions, analyzes spending, and checks goals/savings
* **User Chat Model** — Handles conversational queries with date/time awareness and calculation tools
* **Redis-backed Memory** — Retains conversation context across sessions for personalized responses

---

### 📂 Smart Category Management

An AI agent checks new categories against existing categories stored in MongoDB.

Example:

```
Coffee takeaway
↓
Coffee / Tea / Drinks
```

If no similar category exists, the system creates a **normalized new category**.

This ensures the dataset remains **clean and searchable**.

---

### 🗄️ Automated Database Storage

All transactions are stored in **MongoDB**.

Example document structure:

```
USER_ID
DATE
ITEM
CATEGORY
AMOUNT
PAYMENT
PLATFORM
STATUS
NOTE
MODE
```

This structure enables easy analytics and reporting.

---

### 📊 Expense Report Export

Users can generate a report directly from Telegram using the `/xl` command.

The system will:

1. Retrieve the user's transaction data from MongoDB
2. Convert it into a structured format
3. Generate an **Excel report**
4. Send the report file through Telegram

---

### 📁 File Upload Storage

Users can upload receipts or files.

These files are automatically stored in **Google Drive** with a date-based folder structure:

```
Year
  └── Month
       └── Date
             └── Uploaded Files
```

---

## 🤖 Bot Commands

| Command      | Description                                      |
| ------------ | ------------------------------------------------ |
| `/xl`        | Generate and download an Excel expense report    |
| `/savings`   | Check your savings progress and financial goals  |
| `/insert`    | Manually insert a transaction entry              |
| _(any text)_ | Log a transaction using natural language         |
| _(any file)_ | Upload a receipt or document to Google Drive     |

---

## 🏗️ System Architecture

```
User (Telegram)
      │
      ▼
Telegram Bot Trigger
      │
      ├──── File Upload ──────────────► Google Drive Storage
      │
      ▼
Switch (Command Router)
      │
      ├── /xl ──────────────────────► Excel Export → Send via Telegram
      ├── /savings ─────────────────► Goals/Savings Agent
      ├── /insert ──────────────────► Manual Insert → MongoDB
      │
      └── Natural Language ─────────► AI Information Extractor
                                              │
                                              ▼
                                   Transaction Type Classifier
                                   (Daily / Online / Income / Expense)
                                              │
                                              ▼
                                   Category Normalization Agent
                                              │
                                              ▼
                                        MongoDB Storage
                                              │
                                              ▼
                                   Confirmation Message (Telegram)
```

Additional services used:

* **Google Drive** — Receipt and file storage
* **Airtable** — AI agent memory storage
* **Redis** — Conversation history for the chat agent
* **Excel export** — Downloadable transaction reports

---

## 🛠 Tech Stack

| Technology       | Purpose                          |
| ---------------- | -------------------------------- |
| n8n              | Workflow automation engine       |
| Telegram Bot API | User interface (chat)            |
| DeepSeek LLM     | NLP extraction and classification|
| OpenRouter       | Financial advisor AI model       |
| MongoDB          | Transaction database             |
| Google Drive     | File and receipt storage         |
| Airtable         | AI agent memory storage          |
| Redis            | Conversation session memory      |
| JavaScript nodes | Custom data transformation logic |

---

## ✅ Prerequisites

Before getting started, ensure you have accounts and API access for the following services:

* [n8n](https://n8n.io/) (self-hosted or cloud)
* [Telegram](https://telegram.org/) account + a bot created via [@BotFather](https://t.me/BotFather)
* [MongoDB](https://www.mongodb.com/atlas) (Atlas free tier or self-hosted)
* [DeepSeek](https://platform.deepseek.com/) API key
* [OpenRouter](https://openrouter.ai/) API key
* [Google Drive](https://drive.google.com/) with OAuth credentials configured in n8n
* [Airtable](https://airtable.com/) account with an API key
* [Redis](https://redis.io/) instance (local or cloud, e.g. [Redis Cloud](https://redis.com/try-free/))

---

## 📦 Installation

### 1. Install n8n

**Option A — npm (global install):**

```bash
npm install -g n8n
n8n start
```

**Option B — Docker:**

```bash
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

Then open [http://localhost:5678](http://localhost:5678) in your browser.

---

### 2. Import Workflow

1. Open your **n8n** instance
2. Click **"New Workflow"** → **"Import from File"**
3. Upload the **`Financial Agent.json`** file from this repository
4. Save the workflow

---

### 3. Configure Credentials

In n8n, go to **Settings → Credentials** and add the following:

| Service            | Credential Type              | Notes                                      |
| ------------------ | ---------------------------- | ------------------------------------------ |
| Telegram Bot API   | Telegram Bot API             | Get token from [@BotFather](https://t.me/BotFather) |
| MongoDB            | MongoDB                      | Connection string (Atlas or local)         |
| DeepSeek           | DeepSeek API                 | API key from [platform.deepseek.com](https://platform.deepseek.com/) |
| OpenRouter         | OpenRouter API               | API key from [openrouter.ai](https://openrouter.ai/) |
| Google Drive       | Google Drive OAuth2          | OAuth credentials from Google Cloud Console|
| Airtable           | Airtable Personal Access Token | From [airtable.com/account](https://airtable.com/account) |
| Redis              | Redis                        | Host, port, and password if required       |

---

### 4. Activate the Workflow

1. Open the imported workflow in n8n
2. Click the **"Active"** toggle in the top-right to enable it
3. Open your Telegram bot and send a message

---

## 💡 Example Interactions

**Logging an expense:**

```
User: Swiggy burger 220
```

```
Bot:  ✅ Successfully added

      Date: 2026-03-12
      Item: Burger
      Category: Dining out / restaurants
      Amount: ₹220
      Payment Method: GPay
      Platform: Swiggy
      Status: Pending
```

**Exporting a report:**

```
User: /xl
```

```
Bot:  📊 [Expense_Report.xlsx]  ← file attachment
```

**Checking savings:**

```
User: /savings
```

```
Bot:  You've saved ₹5,200 this month. You're 65% toward your ₹8,000 goal!
```

---

## 🎯 Use Cases

* Personal finance tracking
* AI-powered expense logging via chat
* Budget monitoring and goal tracking
* Automated spending reports (Excel)
* AI financial assistant with context memory
* Receipt storage and organization

---

## 🤝 Contributing

Contributions are welcome! To contribute:

1. Fork the repository
2. Create a new branch: `git checkout -b feature/your-feature`
3. Make your changes and commit: `git commit -m "Add your feature"`
4. Push to the branch: `git push origin feature/your-feature`
5. Open a Pull Request

Please ensure any workflow changes are exported and committed as updated JSON.

---

## 📜 License

MIT License

```
MIT License

Copyright (c) 2026 Joyal Aliyas

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---
