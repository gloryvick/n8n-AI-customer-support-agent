Glow Skincare- AI Customer Support Agent

An automated customer support workflow built in n8n that reads incoming customer emails, classifies them using Google Gemini, and routes them intelligently: simple inquiries get an instant AI-generated reply, while complaints or sensitive issues are escalated to a human team on Slack. Every interaction is logged to Airtable for tracking and follow-up.

This project was built as part of my automation portfolio to demonstrate how AI can be layered onto a support inbox to reduce response time without losing the human touch where it matters most.

What It Does
Listens for new emails — A Gmail trigger watches the support inbox for incoming customer messages.
Classifies the message — The email content is sent to Google Gemini, which analyzes intent and returns a structured JSON response (e.g. category, sentiment, urgency).
Routes based on classification:
Routine inquiries (order status, product questions, general info) → Gemini drafts a reply, which is sent automatically.
Complaints or sensitive issues → The message is escalated to a Slack channel so a team member can step in personally.
Logs everything — Every email, its classification, and its outcome (auto-replied or escalated) is recorded in Airtable, creating a searchable history of support activity.
Workflow Architecture
Gmail Trigger
     │
     ▼
Google Gemini (Classification)
     │
     ▼
   IF Node
   ┌──────────────┴──────────────┐
   │                              │
Auto-Reply Branch          Escalation Branch
(Gemini drafts reply        (Slack notification
 → Gmail sends it)            to support team)
   │                              │
   └──────────────┬───────────────┘
                   ▼
            Airtable Logging
Tech Stack
Tool	Role
n8n	Workflow orchestration
Gmail	Trigger (incoming mail) + reply delivery
Google Gemini	Email classification + auto-reply drafting
Airtable	Logging and record-keeping
Slack	Escalation notifications for complaints
Key Design Decisions
Structured output parsing: Gemini is prompted to return classification results as JSON rather than free text, which makes the downstream IF/routing logic reliable instead of guessing at keywords.
Escalation over full automation: Complaints are intentionally not auto-replied to. They're routed to a human via Slack, because tone and judgment matter more than speed for unhappy customers.
Single source of truth: Airtable logs both branches of the workflow (auto-replied and escalated), so nothing falls through the cracks and support activity is easy to audit later.
Challenges & Fixes

Building this workflow surfaced a few real-world integration issues worth documenting:

Slack OAuth scope errors — Resolved by creating a custom Slack app with a minimal, purpose-built set of scopes instead of relying on a broad default template.
Gemini model availability errors — An initially selected model was deprecated/unavailable; fixed by switching to a current, supported Gemini model.
JSON body formatting errors in HTTP Request nodes — Resolved by carefully validating the JSON structure being sent, rather than relying on n8n's auto-formatting.
Lessons Learned

This project reinforced the value of structured AI outputs for automation — asking an LLM to return JSON (rather than parsing loose text) turned classification into something a workflow could reliably branch on. It also highlighted where automation should stop: routing complaints to a human rather than letting AI handle every reply.
