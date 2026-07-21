# AI-Assisted Internal Request Triage (n8n)

> **Status: learning portfolio prototype.** A public-safe n8n workflow that handles fictional internal requests using transparent validation, human-review controls, and a safe REST/JSON enrichment example.

## Purpose

This project was built as portfolio evidence for a Junior Automation & AI Specialist application. It demonstrates how an internal request can be standardised, assessed against clear business rules, safely routed, and enriched with a reference API response.

It is deliberately a **rule-based prototype**, not an autonomous system:

- it does not approve access, payments, or supplier actions;
- it does not connect to real company systems; and
- it does not use real personal, customer, or employer data.

## Workflow

```text
Webhook
  → Edit Fields (standardise request data)
  → Code in JavaScript (validate required fields and create a review flag)
  → If (human_review_required?)
      ├─ true  → Sensitive / incomplete request
      └─ false → Normal Processing Queue
                   ├─→ Mock Reference API Lookup
                   └─→ Enriched Normal Request (Merge)
```

On the normal route, the **Merge** node combines:

```text
Input 1: original normal-processing request
Input 2: response from the mock REST API
```

The Merge mode is **Combine by Position**. This is appropriate for the single-item synthetic demonstration included here.

## What it demonstrates

- Webhook-based request intake using `POST`
- Field mapping and standardisation in n8n
- A small, explainable JavaScript validation step
- Required-field checks and explicit sensitive-request controls
- Boolean routing with an n8n **If** node
- REST/JSON API consumption using the n8n **HTTP Request** node
- Merging data returned by an API with the original business request
- Clear human-review routing and decision reasons

## Rules in this prototype

A request requires human review when either condition is true:

1. One of these required fields is missing: `requester`, `request_type`, `description`, or `needed_by`.
2. The request type is `access_request`, `payment_request`, or `supplier_request`.

In business language:

```text
Incomplete or sensitive request → human review
Complete low-risk request        → normal processing and mock API lookup
```

The workflow never automatically approves or executes sensitive actions.

## Safe REST/JSON demonstration

The normal route calls this public, credential-free dummy endpoint:

```text
GET https://jsonplaceholder.typicode.com/todos/1
```

It is used only to demonstrate the REST request/JSON response pattern. It is **not** a real business-system integration, and the returned `completed` field has no business meaning for the fictional internal request.

## Tested scenarios

| Synthetic test case | Expected behaviour | Verified result |
|---|---|---|
| Complete `information_request` | Normal processing, mock API lookup, then merged/enriched record | Passed |
| Complete `access_request` | Human-review route; normal processing and API lookup do not run | Passed |

All example names, requests, and dates are fictional.

## Run locally

1. Start n8n locally, for example: `npx n8n`.
2. In n8n, choose **Import from File** and select `workflows/internal-request-triage.json`.
3. Open the workflow. It includes a pinned fictional `information_request` for safe testing.
4. Execute the workflow and inspect **Enriched Normal Request**.
5. To test human-review routing, change the pinned `request_type` to `access_request` or leave a required value blank, then execute again.

## Data and security

- No real company, customer, employee, or production data is included.
- No API keys, credentials, or production webhook URLs are included.
- The exported workflow excludes local instance, workflow, and webhook identifiers.
- This is a local portfolio prototype, not a production deployment.

## Possible next improvements

- Add a small library of synthetic test fixtures and expected outputs.
- Add error-handling for an unavailable API.
- Add controlled LLM classification with structured output and a human-review fallback.
