# nistula-technical-assessment

This project is an AI-powered guest messaging automation system developed using FastAPI and PostgreSQL. It processes guest messages received from platforms like Airbnb, WhatsApp, and Booking.com through a webhook API and classifies them into categories such as pricing, availability, complaints, and special requests. Based on the query type, the system generates automated draft replies using Claude AI and applies confidence scoring with escalation logic to decide whether the response should be auto-sent or reviewed by an agent. All guest interactions are stored in the database for tracking and analysis. The complete backend is deployed on Render with live API endpoints and Swagger documentation support.


## LIve URL: https://nistula-technical-assessment-5eg6.onrender.com/docs

## Part 1 — Guest message handler
### Run
1. Create a virtual env (already present in this folder if you kept `venv/`).
2. Install deps:
   ```bash
   pip install -r requirements.txt
   ```
3. Configure environment:
   - Copy `.env.example` to `.env`
   - Set `CLAUDE_API_KEY` and `DATABASE_URL`
4. Start API:
   ```bash
   uvicorn app.main:app --reload --port 8000
   ```
5. Webhook endpoint:
   - `POST /webhook/message`

### Confidence scoring logic (used to set `action`)
Location: `app/services.py` (`calculate_confidence`)

Heuristic score in `[0, 1]`:
- Base: `0.75`
- +0.15 if `query_type` is `pre_sales_availability` or `pre_sales_pricing`
- +0.05 if reply length > 80 characters
- -0.25 if reply contains `sorry` or `unable`

Then `action` thresholds:
- `auto_send` if `confidence > 0.85`
- `agent_review` if `0.60 <= confidence <= 0.85`
- `escalate` if `confidence < 0.60`

### Payload contract
Input payload matches the assignment (source/guest_name/message/timestamp/booking_ref/property_id).
The response returns:
```json
{
  "message_id": "uuid",
  "query_type": "pre_sales_availability",
  "drafted_reply": "...",
  "confidence_score": 0.91,
  "action": "auto_send"
}
```

## Part 2 — Database schema
See `schema.sql`.

## Part 3 — Thinking answers
See `thinking.md`.

