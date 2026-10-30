# MATACS — AI-Assisted Proposal Triage System

**Candidate:** Shayan Akram
**Assessment Reference:** `MATACS-TH-2026-02`
**Time Spent:** 2 Days (Understanding implementing and final push to github)

## Overview

This project presents a design for an AI-assisted proposal triage system for commercial solar installation procurement.

The system helps reviewers evaluate submitted proposals against configurable mandatory requirements and weighted criteria. It extracts relevant information from proposal documents, provides supporting evidence, identifies missing or ambiguous information, and produces a transparent provisional ranking.

**The system supports human decision-making; it does not make the final award decision.**

## How It Works

1. **Configure criteria:** An administrator defines the evaluation category, mandatory requirements, scoring criteria, weights, and thresholds.
2. **Upload proposals:** A reviewer uploads proposals and selects the applicable category.
3. **Extract information:** AI extracts relevant facts into a structured format and provides page references and supporting quotations.
4. **Validate evidence:** The application validates the extracted data and identifies missing, ambiguous, or conflicting information.
5. **Check eligibility:** Deterministic checks evaluate mandatory requirements before weighted scoring.
6. **Calculate scores:** The application applies configured scoring formulas and ranks eligible proposals.
7. **Human review:** The reviewer examines evidence, flags, and score breakdowns before making the final award decision.
8. **Maintain an audit trail:** The system records evidence, criteria versions, scores, reviewer overrides, and the final decision.

## Key Design Decisions

- AI is responsible for information extraction, not eligibility decisions or final scoring.
- Mandatory requirements are evaluated separately from weighted criteria.
- Missing or ambiguous evidence is flagged rather than silently assumed.
- Scoring formulas are deterministic and use versioned criteria configurations.
- Proposal documents are treated as untrusted input. Embedded instructions attempting to influence the system are ignored.
- Human reviewers retain authority over the final award decision.

## Repository Structure

```text
matacs-th-2026-02/
├── README.md
├── WRITEUP.md
├── PROMPTS.md
├── ranking.json
└── diagram/
    └── proposal-triage-flow.png
```

## System Flow

See [the system flow diagram](diagram/proposal-triage-flow.png).

## AI Usage

I used AI as a design and reasoning assistant to help structure the proposal extraction process, explore scoring approaches, identify edge cases, and improve the clarity of the documentation. I reviewed the suggestions rather than accepting them automatically, and kept eligibility checks and weighted scoring as deterministic application logic.

**Example of a correction:** I did not treat weekly progress reports as proof of a performance monitoring plan. These are different things, so I kept the monitoring evidence requirement separate and flagged the proposal for human review rather than awarding unsupported points.

## Scope

This repository contains the proposed system design, evaluation logic, extraction prompt, sample ranking output, and architecture diagram. It is a design exercise and does not claim to be a deployed production application.