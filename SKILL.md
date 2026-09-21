---
name: brain-core
description: Connects to a private backend server that stores the user's own business or personal records, such as users, customers, accounts, orders, inventory, or internal documents. Use this skill when the user asks things like "how many users/customers/orders do I have", "how many records are registered", "look up my account/customer/order data", "search my internal documents", or any question about counts, records, or data in the user's own system or database. Also use this skill when the user asks to check if the backend, server, or service is online, working, up, or healthy. Do not use this skill for public real-world facts (weather, news, sports scores, public figures, general trivia), math, coding help, creative writing, or anything the model can already answer on its own without checking an external database.
metadata:
  require-secret: true
  require-secret-description: Pega tu API key del gateway (GATEWAY_API_KEY o una de GATEWAY_API_KEYS en Railway).
  homepage: https://github.com/google-ai-edge/gallery/tree/main/skills
---

# Brain Core

## Instructions

This skill has two actions. Decide which one to use based on the user's request:

- Use `status` when the user asks whether the backend/server/service is online, working, up, or healthy.
- Use `query` when the user asks about counts, records, or data in their own system — such as users, customers, accounts, orders, inventory, or internal documents. Not for public real-world facts like weather, news, or general trivia.

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
