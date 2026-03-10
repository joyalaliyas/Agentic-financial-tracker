# Autonomous Financial Intelligence Pipeline

**An LLM-Orchestrated personal finance engine built on n8n.**

This repository contains a high-performance **n8n workflow** that transforms Telegram into a sophisticated financial terminal. By leveraging **Large Language Models (LLMs)** and **multi-agent orchestration**, the system automates the entire lifecycle of financial tracking—from unstructured data extraction and receipt archiving to real-time behavioral advisory.

---

## 🏗️ System Architecture & Implementation

The engine is engineered using a **Modular Agentic Framework**, ensuring that data ingestion is decoupled from analytical reasoning.

### 1. NLP Ingestion & Entity Extraction

The entry point is a **Telegram Trigger** that accepts both natural language and binary media.

* **NER Processing:** The system utilizes a **Unified Extractor** (powered by DeepSeek) to perform Named Entity Recognition. It isolates variables such as `item`, `amount`, `category`, `platform`, and `payment method` from strings like "Lunch 120 via GPay".
* **Classification Logic:** A **Switch Node** routes transactions into specific pipelines: `Online`, `Daily`, `Income`, or `Expense`.

### 2. AI Category Normalization

To maintain database integrity and prevent categorical drift, the workflow employs an **AI Category Normalizer**.

* **Fuzzy Matching:** Before insertion, the system queries a MongoDB collection of existing categories.
* **Deduplication:** The AI compares the new input against the global list (e.g., mapping "coffee" to "Snacks & Beverages") to ensure a clean, queryable ledger.

### 3. Automated Document Cloud

For image-based inputs (receipts), the system executes a recursive folder-creation logic on **Google Drive**.

* **Dynamic Pathing:** Files are automatically organized into a `Year > Month > Date` directory structure based on the transaction timestamp.
* **Verification:** The workflow triggers a confirmation back to Telegram once the binary transfer is finalized.

### 4. Behavioral Advisor Agent

The "brain" of the system is the **Main Agent**, which functions as a financial logic engine.

* **Contextual Memory:** It retrieves user-defined budget limits and goals from **Airtable**.
* **Data Aggregation:** When a user asks "Can I afford this?", the agent executes complex **MongoDB Aggregation Pipelines** to calculate current-month "burn rates" and "run rates".
* **Deterministic Math:** The system is explicitly instructed to rely on raw database sums rather than LLM "hallucinations" for financial calculations.

---

## 🛠️ Tech Stack

| Component | Technology |
| --- | --- |
| **Automation Engine** | [n8n](https://n8n.io/) |
| **Language Models** | DeepSeek-V3, OpenRouter (various models) |
| **Primary Database** | MongoDB (DSR_RAW_DATA collection) |
| **Memory Bank** | Airtable (User-defined budgets/goals) |
| **Session State** | Redis (Chat history management) |
| **Binary Storage** | Google Drive API |

---

## 📊 Database Schema (MongoDB)

All transactions are normalized into the following schema for high-precision reporting:

```json
{
  "USER_ID": Number,
  "DATE": "YYYY-MM-DD",
  "CATEGORIES": String,
  "ITEM": String,
  "AMOUNT": Double,
  "PAYMENT": String,
  "MODE": "ONLINE" | "OFFLINE",
  "PLATFORM": String,
  "STATUS": "Pending" | "Delivered"
}

```

---

## 🚀 Deployment

1. **Import Workflow:** Load the `final chat bot.json` into your n8n environment.
2. **Environment Variables:** Configure credentials for Telegram, MongoDB, Airtable, Google Drive, and your chosen LLM provider.
3. **Database Initialization:** Create the `DSR_RAW_DATA` collection in MongoDB and a `Memories` table in Airtable with fields for `User`, `Memory`, and `TTL`.
4. **Execution:** Activate the workflow to begin receiving real-time financial telemetry.

---
## ⚖️ License

**MIT License**

Copyright (c) 2026 Joyal

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

## 👨‍💻 Author
**Joyal** 
