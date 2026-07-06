# OpsRadar by Denver

**AI Operations Command Agent for Indonesian Stone Crusher Businesses**
Kaggle "AI Agents: Intensive Vibe Coding" Hackathon Submission — Track: Agents for Business

---

## Problem Statement

Stone crusher business owners in Indonesia struggle to monitor daily operations (production output, machine downtime, fuel stock, visitor/buyer leads) because data is scattered across paper reports and manual checks. OpsRadar lets the owner ask questions in plain Bahasa Indonesia via Telegram, and the system automatically reads the operational database and composes an answer.

## Architecture

```
Telegram → Security Gate → Greeting Shortcut → Memory Cache
→ Agent 1 (Command Parser) → Agent 2 (Data Retrieval)
→ Agent 3 (Analyst) → Telegram
```

A 3-agent pipeline (built with Google Gemini API) interprets the owner's question, retrieves the relevant data from Google Sheets, and writes a clear, human-friendly answer back to Telegram.

A separate **n8n workflow** automates supervisor daily reports: it reads structured reports from a supervisor's Gmail, validates the data, writes it to Google Sheets, and sends a Telegram confirmation with production totals and incident counts.

## Repository Contents

| File | Description |
|---|---|
| `opsradar-by-denver-(public-safe).ipynb` | Main Kaggle Notebook — the 3-agent pipeline (Command Parser → Data Retrieval → Analyst) that powers the Telegram Q&A assistant. |
| `OpsRadar - Gmail Laporan Supervisor (public-safe).json` | n8n workflow — Gmail-to-Sheets automation for supervisor daily reports. |
| `OpsRadar - Stone Crusher Database Public Safe.xlsx` | Sample database (dummy data) showing the structure of the Google Sheets used: Laporan Harian, Lead Buyer, Downtime Log. |

## How to Run

The notebook is designed to run on **Kaggle**, where it reads credentials from Kaggle Secrets rather than from the code itself. To run it:

1. Open the notebook on Kaggle (see link below).
2. Fill in these Kaggle Secrets: `GEMINI_API_KEY`, `TELEGRAM_BOT_TOKEN`, `GOOGLE_SERVICE_ACCOUNT_JSON`, `GOOGLE_SHEET_ID`, `TELEGRAM_OWNER_CHAT_ID`.
3. Run all cells from top to bottom.
4. To activate the live Telegram bot, set `RUN_TELEGRAM_BOT` to `True` in the last cell.

**Live Kaggle Notebook:** *(thttps://www.kaggle.com/code/rommysunarto/opsradar-by-denver-v2)*

The n8n workflow (`.json` file) can be imported directly into any self-hosted or cloud n8n instance via **Import from File**.

## Security

All credentials are stored in Kaggle Secrets or n8n Credentials — nothing is hardcoded in the code. Spreadsheet data is always treated as raw data, never as an instruction (protection against prompt injection). The database file in this repository contains dummy/sample data only.

## Note on Language

The conversational demo (Telegram messages) uses Bahasa Indonesia, since this is the authentic language of the target business owner this project was built for. All technical documentation is in English.
