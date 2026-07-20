# Internal Request Triage (n8n)

> **Status: learning prototype / work in progress.** A public-safe n8n workflow that triages fictional internal requests using clear validation and human-review controls.

## Why this exists

This project demonstrates how a business request can be handled consistently before it reaches a team queue. It was built as a portfolio learning project for a Junior Automation & AI Specialist application.

The current version is **rule-based**, not AI-powered: it uses a small JavaScript validation step and n8n visual routing. An LLM classification stage and a safe dummy REST integration are planned extensions, not implemented features.

## Workflow

```text
Webhook Intake
  → Prepare Request Data
  → Validate Request
  → Requires Human Review?
      ├─ Yes → Human Review Queue
      └─ No  → Normal Processing Queue
```

## What it demonstrates

- Webhook-based request intake (`POST`)
- Field mapping and standardisation in n8n
- Small JavaScript validation for required fields
- Basic business controls: sensitive request types and incomplete requests require human review
- Boolean conditional routing using an n8n If node
- Clear routing labels and decision reasons

## Rules in this prototype

A request requires human review when:

- a required field is missing: `requester`, `request_type`, `description`, or `needed_by`; or
- its type is `access_request`, `payment_request`, or `supplier_request`.

The workflow **does not approve or execute sensitive actions automatically**.

## Test evidence

| Synthetic test case | Expected route | Observed result |
|---|---|---|
| Complete `information_request` | `normal_processing` | Passed |
| Complete `access_request` | `human_review` | Passed |

All example names, requests, and dates are fictional.

## Run locally

1. Start n8n locally (for example, `npx n8n`).
2. In n8n, choose **Import from File** and select `workflows/internal-request-triage.json`.
3. Open the workflow. It includes one pinned synthetic `information_request` for safe testing.
4. Execute the workflow and inspect the output of the two queue nodes.

To test the human-review route, change the pinned `request_type` to `access_request`, or leave one required value blank.

## Data and security

- No real company, customer, employee, or production data is included.
- No API keys, credentials, or production webhook URLs are included.
- The exported workflow has been sanitised to remove local instance, workflow, and webhook identifiers.
- This is a local portfolio prototype, not a production deployment.

## Planned next improvements

- Add a safe dummy REST/JSON lookup using n8n's HTTP Request node.
- Add controlled LLM classification with structured output and a human-review fallback.
- Add more synthetic test cases and safe execution screenshots.
