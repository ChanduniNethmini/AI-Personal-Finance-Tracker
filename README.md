# Personal Finance Tracker — AI Agent

A personal finance assistant that starts with a chat and is powered by an AI agent, has tool calling, a persistent SQLite database, and a Gradio UI. Talk to it like you would a friend—no forms, no buttons, just talking.

![Finance Tracker Screenshot](screenshot.png)

---

## Features

- **Natural language interface**—just say "Paid rent $900" and it will log it for you.
- **Tracking your salary**—set your monthly income and keep an eye on it over time
- **Logging expenses**—log any expense by category with automatic date handling
- **Check your balance**— You can ask how much you have left at any time.
- **Spending breakdown** — see a summary of your spending by category with percentages
- **Typewriter response effect** — responses come in letter by letter, just like ChatGPT
- **Persistent storage** — all data is kept in a local SQLite database

---

## Tech Stack

| Component | Tool |
|-----------|------|
| LLM | OpenAI `gpt-4o` |
| UI | Gradio |
| Database | SQLite3 (built-in Python) |
| Tool Calling | OpenAI function calling |
| HTTP Client | OpenAI Python SDK |
| Config | `python-dotenv` |

---

## Project Structure

```
AI-Personal-Finance-Tracker/
├── notebooks/
│   └── financeTracker.ipynb   # Main application notebook
├── finance.db                  # SQLite database (auto-created on first run)
├── .env                        # API keys — NOT included in repo
├── requirement.txt             # Python dependencies
└── README.md
```

---

## Setup

### 1. Clone / download the project

### 2. Set Up environment

 1. uv init
 2. uv sync
 3. .venv\Scripts\activate - to activate virtual environment
 4. uv add -r requirement.txt - to install dependencies

### Create a `.env` file in the project root and add OpenAI API key:

```
OPENAI_API_KEY=your_openai_api_key_here
```


## Running the App

Open the notebook in Jupyter and run all cells from top to bottom:

```bash
jupyter notebook notebooks/financeTracker.ipynb
```

Or open it directly in VS Code with the Jupyter extension.

Once the last UI cell runs, you will see:

```
Running on local URL: http://127.0.0.1:7860
```

Open that URL in your browser.

---

## Usage Examples

### Set your salary
```
My monthly salary is $3,500
I earn $4,000 a month
Set my income to 2800
```

### Log expenses
```
Paid rent — $900
Bought groceries for $67.50 at Cargills
Paid University fees $1000
Spend on saloon for nails and facials $50
```

### Check your balance
```
How much do I have left?
What's my remaining budget?
How much have I spent so far?
```

### Get a spending breakdown
```
Show me my spending breakdown
Where am I spending the most?
Give me a summary of this month's expenses
```

---

## How It Works

```
User message
     │
     ▼
GPT-4o (with tools list)
     │
     ├── Tool call? YES → run Python function → append result → loop
     │
     └── Tool call? NO  → return text response to user
```

The agent has four tools it can call:

| Tool | When it's called |
|------|-----------------|
| `set_salary` | User mentions income or salary |
| `log_expense` | User mentions spending money |
| `get_balance` | User asks how much is left |
| `get_expense_summary` | User asks for a breakdown or summary |

---

## Database Schema

**`salary` table**

| Column | Type | Description |
|--------|------|-------------|
| id | INTEGER | Auto-increment primary key |
| amount | REAL | Monthly salary amount |
| month | TEXT | Month name (e.g. `April`) |
| year | INTEGER | Year (e.g. `2026`) |
| created_at | TEXT | Timestamp |

**`expenses` table**

| Column | Type | Description |
|--------|------|-------------|
| id | INTEGER | Auto-increment primary key |
| amount | REAL | Expense amount |
| category | TEXT | e.g. `Groceries`, `Rent`, `Dining` |
| description | TEXT | Short description |
| date | TEXT | Date in `YYYY-MM-DD` format |
| created_at | TEXT | Timestamp |

---

## Resetting Test Data

A utility cell at the bottom of the notebook lets you clear the database between test runs:

```python
delete_all_expenses()   # clear expenses only
delete_all_salary()     # clear salary only
show_all()              # preview current records
```

---

## Requirements

```
python-dotenv
openai
ipykernel
ipython
gradio
```

Install with:

```bash
pip install -r requirement.txt
```

---
