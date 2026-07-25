# Limitations and human-review policy

## Human-review boundary

The workflow does not approve, execute, or send sensitive actions. It routes incomplete, malformed, sensitive, and unknown request types to `human_review`.

The LLM only generates a short summary after deterministic rules have selected the low-risk normal route. Its response is not used to make routing or approval decisions.

## Known prototype limitations

- The webhook is a local development interface and has no authentication in this learning prototype.
- The JSONPlaceholder call is a mock API, not an employer or business-system integration.
- A failed mock API call currently stops the normal route rather than falling back to a review queue.
- The final Data Table insert can create duplicate rows if a request is retried.
- Local Ollama credentials and Data Tables are intentionally excluded from the public workflow export and must be configured by an importer.

## Data policy

Use only fictitious data for tests, screenshots, and repository files. Do not enter employer, customer, employee, account, payment, access, password, key, token, or production webhook information.
