# 🤖 Agentic Financial Tracker

An **AI-powered personal finance tracker** built using **n8n, Telegram, DeepSeek AI, and MongoDB**.
This project allows users to log expenses, income, and transactions simply by sending **natural language messages** to a Telegram bot.

The system automatically **extracts, categorizes, and stores financial data** using AI agents and workflow automation. 

---

# 🚀 Features

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

### 🧠 AI Powered Data Extraction

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

Example structure:

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

Users can generate a report directly from Telegram.

The system will:

1. Retrieve user data from MongoDB
2. Convert it into structured format
3. Generate an **Excel report**
4. Send the report through Telegram

---

### 📁 File Upload Storage

Users can upload receipts or files.

These files are automatically stored in **Google Drive** with a folder structure:

```
Year
  └── Month
       └── Date
             └── Uploaded Files
```

---

# 🏗️ System Architecture

```
User (Telegram)
      │
      ▼
Telegram Bot Trigger
      │
      ▼
AI Information Extractor
      │
      ▼
Transaction Type Classifier
      │
      ▼
Category Normalization Agent
      │
      ▼
MongoDB Storage
      │
      ▼
Confirmation Message
```

Additional services used:

* Google Drive (file storage)
* Airtable (memory storage)
* Excel export system

---

# 🛠 Tech Stack

| Technology       | Purpose              |
| ---------------- | -------------------- |
| n8n              | Workflow automation  |
| Telegram Bot API | User interface       |
| DeepSeek LLM     | NLP and extraction   |
| MongoDB          | Transaction database |
| Google Drive     | File storage         |
| Airtable         | AI memory storage    |
| JavaScript nodes | Custom data logic    |

---

# 📦 Installation

### 1 Install n8n

```
npm install -g n8n
```

or run with Docker.

---

### 2 Import Workflow

1. Open **n8n**
2. Click **Import Workflow**
3. Upload the provided workflow JSON file

---

### 3 Configure Credentials

Connect the following services:

* Telegram Bot API
* MongoDB
* DeepSeek API
* Google Drive
* Airtable

---

### 4 Activate the Workflow

Start the workflow and send messages to your Telegram bot.

---

# 💡 Example Interaction

User message:

```
Swiggy burger 220
```

Bot response:

```
Successfully added

Date: 2026-03-12
Item: Burger
Category: Dining out / restaurants
Amount: 220
Payment Method: GPay
Platform: Swiggy
Status: Pending
```

---

# 🎯 Use Cases

* Personal finance tracking
* AI expense logging
* Budget monitoring
* Automated spending reports
* AI financial assistants
---

# 📜 License

MIT License

```
MIT License

Copyright (c) 2026 Joyal Aliyas

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction.
```
---
