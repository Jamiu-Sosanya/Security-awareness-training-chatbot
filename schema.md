# Airtable Schema — CyberSense Chatbot

## Base name: CyberSense Training

---

## Table 1: Sessions

| Field name        | Field type        | Notes                                      |
|------------------|-------------------|--------------------------------------------|
| session_id        | Single line text  | Primary key — e.g. sess_1713600000000      |
| topic             | Single select     | phishing, passwords, social-engineering, data-handling |
| score             | Number (integer)  | Running correct answer count               |
| total_questions   | Number (integer)  | Questions answered so far                  |
| difficulty_level  | Single select     | easy, medium, hard                         |
| started_at        | Date/time         | ISO 8601                                   |
| completed_at      | Date/time         | ISO 8601 — null until session_complete     |
| grade             | Single line text  | Novice, Practitioner, or Expert            |
| weak_areas        | Long text         | JSON array of weak area strings            |

---

## Table 2: Responses

| Field name     | Field type       | Notes                                         |
|---------------|------------------|-----------------------------------------------|
| response_id    | Auto number      | Auto-generated primary key                    |
| session_id     | Single line text | Foreign key — links to Sessions.session_id    |
| question_text  | Long text        | The question that was asked                   |
| user_answer    | Single line text | Full answer text the user selected            |
| correct        | Checkbox         | True if user answered correctly               |
| difficulty     | Single select    | Difficulty level when question was asked      |
| topic          | Single select    | Topic category                                |
| timestamp      | Date/time        | When the answer was submitted                 |

---

## Airtable API setup

1. Go to airtable.com/create/tokens
2. Create a Personal Access Token
3. Scopes needed: data.records:read, data.records:write
4. Add your Base to the token's access list
5. Copy the Base ID from your base URL: airtable.com/YOUR_BASE_ID/...
6. Add these to n8n credentials:
   - Airtable API Key: your personal access token
   - Base ID: your base ID (starts with app...)
   - Sessions table name: Sessions
   - Responses table name: Responses
