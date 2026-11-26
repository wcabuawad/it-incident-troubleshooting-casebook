# IT Incident Troubleshooting Casebook

A portfolio project demonstrating structured IT incident analysis, safe troubleshooting, and evaluation of AI-generated technical support.

> **Portfolio/demo disclaimer:** All incidents, organizations, users, hostnames, IP addresses, and outputs in this repository are fictional examples created for demonstration. They do not describe a real employer incident or contain confidential information.

## Purpose

This repository demonstrates how an IT support professional can:

- turn an ambiguous user complaint into a structured incident;
- form and test diagnostic hypotheses;
- choose low-risk, evidence-led troubleshooting steps;
- distinguish symptoms from root causes;
- explain why a proposed fix is appropriate;
- evaluate AI-generated support advice for accuracy and operational safety.

## Case studies

| ID | Scenario | Skills demonstrated |
|---|---|---|
| 001 | Domain user cannot sign in; other users are affected intermittently | Windows domain concepts, DNS, network diagnostics, incident triage |

## Evaluation framework

Each proposed support answer is assessed on:

1. Technical accuracy
2. Diagnostic sequence and evidence
3. Safety and change control
4. Clarity and communication
5. Completeness and verification

See [`rubric.md`](rubric.md) and [`evaluations/001-response-review.md`](evaluations/001-response-review.md).

## How to use this portfolio

1. Read the incident statement in `cases/`.
2. Review the diagnostic plan and expected evidence.
3. Compare a proposed AI response against the rubric.
4. Explain which advice is useful, incomplete, or unsafe.

## Scope and honesty

This is a demonstration of troubleshooting reasoning and technical documentation. It is not a claim of expertise in Linux, Microsoft 365, Azure, MDM, or advanced PowerShell development. Commands are included as illustrative diagnostic examples and should be validated against the actual environment before use.

## Suggested future extensions

- Add a Remote Desktop Services connectivity case.
- Add a SQL application connectivity case.
- Add a Windows DNS/DHCP case.
- Add automated checks for case-file formatting or rubric scoring.
