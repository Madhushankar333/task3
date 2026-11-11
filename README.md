# Customer Query Urgency Classifier

A prompt-engineering project that classifies customer support queries into urgency levels so critical requests are handled first.

## Overview

A customer support team can receive thousands of queries a day. This prompt turns an LLM into a triage system that reads a single customer query and returns exactly one word describing its urgency: **High**, **Medium**, or **Low**.

## Problem Statement


| **Input** | A customer query in natural language |
| **Output** | A single word: `High`, `Medium`, or `Low` |
| **Constraint** | No explanations, punctuation, or extra text |

## Urgency Levels

| Level | Meaning | Typical Examples |

| **High** | Immediate risk or a severe, time-critical problem | Hacked accounts, fraud, service outages blocking work, lost access during an urgent payment |
| **Medium** | A real problem or deadline, but no immediate danger | Delayed refunds, order changes with an approaching deadline, billing discrepancies |
| **Low** | Informational or routine, with no deadline | Plan inquiries, how-to questions, profile or settings changes |

## Prompt Design

The prompt uses these techniques:

- **Role assignment:** The model acts as a customer support triage system.
- **Explicit category definitions:** Each level has concrete criteria, which reduces ambiguity at the Medium boundary.
- **Decision guidelines:** Urgency is judged by impact and time sensitivity, not by tone or words like "urgent".
- **Higher-level tie-breaker:** When a query fits two levels, the higher one is chosen, since under-prioritizing a critical issue costs more than over-prioritizing a minor one.
- **Few-shot examples:** Seven labeled examples anchor the expected behavior.
- **Strict output rules:** The response is limited to one word, which makes it easy to parse automatically.
- **Delimiters:** Triple quotes separate the customer query from the instructions.

## Usage

1. Copy the prompt template into your LLM application or API call.
2. Replace `{{QUERY}}` with the customer's message.
3. Send the request and read the single-word response.

## Tips for Production Use

- Set the model temperature to `0` for consistent classifications.
- Validate the output against the allowed set (`High`, `Medium`, `Low`) and retry or flag anything else.
- Review misclassified queries regularly and add them as few-shot examples.
- Adapt the category definitions to your own business, such as adding industry-specific High triggers.

## Limitations

- Classifications depend only on the text of the query, with no customer history or account context.
- Ambiguous or very short queries may default to a higher urgency because of the tie-breaker rule.
- Queries in other languages or with heavy slang may need extra examples.
