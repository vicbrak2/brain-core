---
name: brain-core
description: Connects to a private backend server that stores the user's own private, internal, or custom business data (such as personal records, account information, internal documents, or knowledge specific to the user's own systems), and can report whether that backend server is currently online. Use this skill only when the user asks about their own private or internal data, or explicitly asks to check if the backend, server, or service is online, working, or healthy. Do not use this skill for general knowledge questions, public real-world facts (weather, news, sports scores, public figures, general trivia), math, coding help, creative writing, or anything the model can already answer without contacting an external server.
metadata:
  require-secret: true
  require-secret-description: Pega tu API key del gateway (GATEWAY_API_KEY o una de GATEWAY_API_KEYS en Railway).
  homepage: https://github.com/google-ai-edge/gallery/tree/main/skills
---

# Brain Core

## Instructions

This skill has two actions. Decide which one to use based on the user's request:

- Use `status` when the user asks whether the backend/server/service is online, working, up, or healthy.
- Use `query` when the user asks about their own private, internal, or custom data that this backend stores (e.g. personal records, account info, internal documents) — not public real-world facts like weather, news, or general trivia.

Call the `run_js` tool with the following exact parameters:

- script name: index.html (optional, this is the default entry point)
- data: A JSON string with the following fields:
  - `action`: String, Required. Must be exactly `"status"` or `"query"`.
  - `question`: String, Required only when `action` is `"query"`. The user's question, in plain text, unmodified.

**Example call for a status check:**

```json
{ "action": "status" }
```

**Example call for a question:**

```json
{ "action": "query", "question": "What were last month's sales numbers?" }
```

## Reading the result

The script always returns a JSON object with an `ok` field.

- When `ok` is `true`, read the `result` field for the answer or status info and report it to the user in plain language.
- When `ok` is `false`, read the `error` field and tell the user in plain language that the backend could not be reached or returned a problem. Do not retry more than once automatically.
