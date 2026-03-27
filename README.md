# AI-powered-whatsapp-chatbot

n8n workflow for WhatsApp lead qualification and CRM logging.

This repository contains an importable workflow JSON that:

- receives WhatsApp inbound messages
- parses and normalizes contact/message payload
- calls Gemini for conversational qualification and scoring
- classifies lead temperature/status
- writes lead details to Google Sheets
- sends a WhatsApp reply back to the lead

## Workflow File

- `whatsapp chatbot.json`

## Main Node Flow

1. `WhatsApp Trigger`
2. `Parse WhatsApp Data` (Code node)
3. `Lexi - Gemini AI Brain` (Code node with Gemini API call)
4. `Process Gemini Response` (Code node)
5. `Confirmed Lead?` (IF node on intent + score)
6. `CRM Data Format` (Set node)
7. `Save to Elevyx CRM` (Google Sheets append/update)
8. `Send WhatsApp Reply`

## Lead Logic Implemented

- Detects first-message style greetings
- Prompts for business context, timeline, budget, decision-maker status
- Produces structured output fields:
  - `lead_score`
  - `status` (`COLD`, `WARM`, `HOT`, `VALID`, `NOT_VALID`)
  - `intent`
  - `service_interest`
  - `next_action`
- Includes fallback reply path if Gemini call fails

## Required Configuration After Import

In n8n, configure credentials and IDs for:

- WhatsApp trigger/send nodes
- Google Sheets node
- Gemini API key placeholder inside code node
- Spreadsheet/document/sheet IDs for your CRM target

## Import Steps

1. Open n8n
2. Import `whatsapp chatbot.json`
3. Attach credentials for WhatsApp and Google
4. Replace Gemini API key placeholder
5. Test with sample WhatsApp messages
6. Activate workflow

## Notes

- Current JSON includes hardcoded placeholders and IDs that should be replaced per environment.
- Workflow is best suited for SMB lead triage and handoff, not fully autonomous closing.

