# 🤖 AI-Powered Customer Service & Stock Management System

> An intelligent multi-agent system built with n8n that fully automates customer service and stock management via Telegram — with real-time sentiment analysis, admin monitoring, and human escalation when needed.

---

## 📌 The Problem

Businesses running on Telegram face these daily challenges:

- Customer service rep spending hours answering repetitive questions
- Stock manager manually updating inventory after every order
- Slow response times leading to lost sales
- No visibility into customer satisfaction levels
- Human errors in order processing and stock tracking
- No system to handle multiple customers simultaneously

---

## ✅ The Solution

A multi-agent AI system connected directly to Telegram that handles everything automatically — answering questions, processing orders, managing stock, analyzing customer sentiment, and alerting the team when needed.

---

## 🗺️ System Architecture

![System Architecture](./architecture.png)

```
Customer Message (Telegram)
           ↓
      Router Agent
      (Groq Model)
      /     |     \
     ↓      ↓      ↓
 Question  Order  Unclear
    ↓        ↓      ↓
Questions  Orders  Asks for
  Agent    Agent  Clarification
    ↓        ↓
Answer    Confirm
from RAG  + Update
           Sheets
           ↓
    End of Conversation
           ↓
    Sentiment Analysis
     /             \
    ↓               ↓
Positive         Negative
    ↓               ↓
Log as          Alert Admin
Happy           + Escalate
Customer        if needed
```

---

## ⚙️ System Components

---

### Part 1 — Knowledge Base Setup

![Knowledge Base](./screenshots/01_knowledge_source.png)

**What it does:**
- Connects to Google Drive and pulls all business files and PDFs automatically
- Splits documents into smart chunks using Recursive Character Text Splitter
- Converts text into vector embeddings using Gemini Embeddings
- Stores all vectors in Supabase Vector Database
- AI agents answer from actual business files — not from general knowledge

> 💡 Result: AI answers are always accurate and based on real business data.

---

### Part 2 — Stock Management

![Stock Management](./screenshots/02_stock.png)

**What it does:**
- Connected directly to Telegram for instant stock updates
- Any inventory change is recorded immediately in Google Sheets
- System confirms back to admin that data has been updated
- Zero manual spreadsheet work required

> 💡 Result: Stock is always 100% accurate in real time.

---

### Part 3 — Router Agent

![Router Agent](./screenshots/03_router_agent.png)

**What it does:**
- Receives all customer messages from Telegram
- Groq AI model analyzes message intent instantly
- Routes to the correct specialized agent automatically
- If message is unclear → asks customer for clarification politely
- Maintains conversation memory for context

> 💡 Result: Every customer gets routed to the right agent within seconds.

---

### Part 4 — Orders Agent

![Orders Agent](./screenshots/04_orders_agent.png)

**What it does:**
- Creates new orders with full customer details in Google Sheets
- Cancels existing orders when requested
- Retrieves order status and history instantly
- Sends order confirmation to customer automatically:

```
✅ Order Confirmed!
Order ID: #1234
Product: [Product Name]
Status: Processing
Expected delivery: 2-3 days
```

---

### Part 5 — Questions Agent

![Questions Agent](./screenshots/05_questions_agent.png)

**What it does:**
- Searches Supabase Vector Database using RAG
- Answers all product and service questions accurately
- Responds based on actual business files — zero hallucination
- Replies in the same language the customer used

---

### Part 6 — Sentiment Analysis (Post-Conversation)

![Sentiment Analysis](./screenshots/06_sentiment.png)

**When does it run?**
After the conversation ends — not during it — to analyze the full picture.

**What it analyzes:**
- All customer messages in the conversation
- Overall tone and language used
- Specific keywords indicating satisfaction or frustration

**Results logged in Google Sheets:**

| Customer | Sentiment | Score | Issue | Date |
|----------|-----------|-------|-------|------|
| Ahmed | Negative | 0.2 | Late delivery | ... |
| Sara | Positive | 0.9 | None | ... |
| Mohamed | Neutral | 0.5 | None | ... |

**What happens based on result:**

```
😊 Positive → Logged as Happy Customer
😐 Neutral  → Logged, no action needed
😡 Negative → Instant Telegram alert to admin
               + Flagged for human follow-up
               + Escalated if score below 0.3
```

> 💡 Result: Full visibility into customer satisfaction with zero manual tracking.

---

### Part 7 — Error Handling

![Error Handling](./screenshots/07_error_handling.png)

**What it does:**
- If AI doesn't understand → replies politely asking for clarification
- If Google Sheets connection fails → instant alert to admin on Telegram
- If order processing fails → customer is notified and admin is alerted
- All errors are logged automatically for review

```
⚠️ System Alert
Error: Sheets connection failed
Time: 14:32
Action needed: Check Google Sheets API
```

---

### Part 8 — Admin Dashboard (Daily Report)

![Admin Dashboard](./screenshots/08_admin_dashboard.png)

**What it does:**
- Sends an automated daily report to admin via Telegram:

```
📊 Daily Report — [Date]

📦 Orders: 12
💬 Questions answered: 34
😊 Happy customers: 28
😐 Neutral customers: 3
😡 Unhappy customers: 3
⚠️ Errors today: 1
🔥 Most asked: "delivery time"
```

> 💡 Result: Full business visibility without checking anything manually.

---

### Part 9 — Human Escalation

**What it does:**
- If customer sentiment is extremely negative → escalates to human agent
- Sends full conversation summary to admin on Telegram
- Admin can take over the conversation directly

```
🚨 Escalation Alert
Customer: Ahmed
Issue: Very frustrated about delayed order
Sentiment Score: 0.1
Action: Human agent needed immediately
```

---

## 🔄 Full System Flow

```
1. Customer sends message on Telegram
2. Router Agent analyzes intent
3. Routed to correct specialized agent
4. Agent handles request and confirms to customer
5. Stock updated if order was placed
6. After conversation ends → Sentiment Analysis runs
7. Results logged in Google Sheets
8. Admin alerted if negative sentiment detected
9. Daily report sent automatically every evening
```

---

## 📊 Key Results

- ✅ Response time reduced from **hours → seconds**
- ✅ Handles **unlimited simultaneous customers**
- ✅ **Zero manual work** for order processing and stock updates
- ✅ **100% visibility** into customer satisfaction via sentiment tracking
- ✅ AI answers based on **actual business data** — not guesswork
- ✅ Replaced **2 full-time manual roles** with one automated system
- ✅ Admin always informed via **real-time alerts and daily reports**

---

## 🛠️ Tech Stack

| Category | Tools |
|----------|-------|
| **Automation** | n8n (Self-Hosted) |
| **AI Models** | Groq, Ollama, Gemini Embeddings |
| **Vector Database** | Supabase |
| **Communication** | Telegram API |
| **Data Storage** | Google Sheets, Google Drive |
| **Infrastructure** | Docker, VPS, SSL |

---

## 📁 Repository Structure

```
📦 ai-customer-service-agent
 ┣ 📂 screenshots/
 ┃ ┣ 01_knowledge_source.png
 ┃ ┣ 02_stock.png
 ┃ ┣ 03_router_agent.png
 ┃ ┣ 04_orders_agent.png
 ┃ ┣ 05_questions_agent.png
 ┃ ┣ 06_sentiment.png
 ┃ ┣ 07_error_handling.png
 ┃ ┗ 08_admin_dashboard.png
 ┣ 📄 architecture.png
 ┗ 📄 README.md
```

---

## 🔐 Note on Source Code

The full workflow logic, AI prompts, and automation scripts are kept private for confidentiality. This repository documents the system architecture, design decisions, and outcomes only.

---

## 📬 Contact

Interested in a similar system for your business?

- 📧 fares.m.elmetwaly@gmail.com
- 🔗 [LinkedIn](https://www.linkedin.com/in/fares-maaty/)
- 💼 [Upwork Profile](https://upwork.com/your-profile)
