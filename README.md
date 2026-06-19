# CyberSense — Adaptive Security Awareness Training Chatbot

> An AI-powered chatbot that quizzes employees on cybersecurity best practices, adapts difficulty in real time based on performance, and logs every session to Airtable.

![Demo](docs/demo.gif)

---

## How it works

1. User picks a topic (Phishing, Passwords, Social Engineering, Data Handling)
2. Frontend sends each message to an n8n webhook with full session context
3. AI Agent evaluates the answer, generates feedback, and asks the next question
4. Difficulty adjusts automatically: 2 correct in a row → harder; 2 wrong → easier
5. Every answer is logged to Airtable (Responses table)
6. After 10 questions, the bot delivers a personalised score report with weak areas

![Architecture](docs/architecture.png)

---

## Features

- 4 topic modules: Phishing, Passwords, Social Engineering, Data Handling
- 80 hand-written questions across 3 difficulty tiers (easy, medium, hard)
- Real-time adaptive difficulty — mid-session difficulty changes shown live in header
- Airtable session tracking — every Q&A pair logged with timestamp and difficulty
- Session completion report: score, grade (Novice / Practitioner / Expert), and specific weak areas
- Demo mode (no API needed) — works out of the box for portfolio demos

---

## Tech stack

| Component         | Tool                              |
|------------------|-----------------------------------|
| Automation        | n8n (self-hosted)                 |
| AI model          | OpenRouter (Claude / GPT-4o)      |
| Score tracking    | Airtable (free tier)              |
| Frontend          | Single-file HTML / CSS / JS       |
| Question bank     | JSON files (80 questions)         |

---

## Run locally

```bash
# 1. Clone
git clone https://github.com/YOUR_USERNAME/security-awareness-chatbot.git
cd security-awareness-chatbot

# 2. Import n8n workflow
# Open n8n → Import → select n8n-workflow/chatbot.json
# Add credentials: OpenRouter API, Airtable API

# 3. Set up Airtable
# Create base using airtable-schema/schema.md
# Copy your Base ID and add to n8n Airtable credentials

# 4. Seed question bank (optional)
pip install requests python-dotenv
cp .env.example .env  # add AIRTABLE_API_KEY and AIRTABLE_BASE_ID
python3 scripts/seed-airtable.py

# 5. Update frontend
# Open frontend/index.html
# Set WEBHOOK_URL to your n8n webhook URL
# Set DEMO_MODE = false

# 6. Open in browser
open frontend/index.html
```

---

## Adaptive difficulty logic

| Condition                     | Action                          |
|------------------------------|---------------------------------|
| 2 correct answers in a row   | Difficulty increases one level  |
| 2 wrong answers in a row     | Difficulty decreases one level  |
| Score at hard difficulty     | No further increase             |
| Score at easy difficulty     | No further decrease             |

Difficulty is passed to the AI on every request, which uses it to select question complexity from the question bank.

---

## Airtable session log (sample)

| session_id       | topic    | score | total_q | difficulty | grade        |
|-----------------|----------|-------|---------|------------|--------------|
| sess_1713600000 | phishing | 8     | 10      | hard       | Practitioner |
| sess_1713601000 | passwords| 6     | 10      | medium     | Practitioner |

---

## Ethical use

This tool is built for **internal corporate security awareness training** purposes only. All questions are educational. No user data is collected beyond the session scores stored in your own Airtable base. Do not deploy this tool to collect personal information.

---

## Author

Built by [Your Name] — cybersecurity portfolio project.  
[LinkedIn](#) · [GitHub](#)
