# System Design AI-Assisted Proposal Triage

## 1. Proposed Process

The system follows a structured evaluation workflow:

1. An administrator configures an evaluation category, including mandatory requirements, weighted criteria, scoring rules, and thresholds.
2. The reviewer uploads proposal documents and selects the relevant category.
3. An AI extraction component reads each proposal and returns structured information, including supporting page references and short evidence quotations.
4. The application validates the extraction output against a predefined schema. Missing, ambiguous, or conflicting information is flagged.
5. Deterministic application logic checks mandatory requirements before calculating weighted scores.
6. Eligible proposals are ranked, with criterion-level scores and explanations available to the reviewer.
7. The reviewer investigates flagged information, compares proposals, and makes the final award decision.

The process separates information extraction from evaluation so that an AI-generated interpretation cannot directly determine eligibility or the winning proposal.

## 2. AI Usage and Scoring

AI is used to extract proposal information, not to make procurement decisions.

The extraction prompt requests explicit evidence for relevant fields, including contractor licence details, WSIB clearance, insurance, experience, price, timeline, warranties, and team information. Each extracted field includes a status and supporting evidence.

The application performs the following tasks deterministically:

- Validates the extraction structure and required fields.
- Checks mandatory requirements.
- Calculates criterion scores using configured formulas.
- Applies criterion weights and produces provisional rankings.
- Identifies proposals that require clarification.

The proposed scoring configuration uses the following weights:

| Criterion | Weight |
|---|---:|
| Relevant experience | 30% |
| Price | 25% |
| Timeline | 15% |
| Warranty | 15% |
| Team and approach | 15% |

The example scoring configuration uses a target of 10 qualifying projects for experience, a CAD $260,000 budget ceiling for price, a 16-week maximum timeline, and reference warranty periods of 25 years for panels and 10 years for workmanship. These are explicit configuration assumptions and should be adjustable by an administrator.

## 3. Edge Cases and Risks

**Missing or ambiguous information:** The system does not invent missing values. It flags incomplete evidence for human clarification and withholds a final score when a weighted criterion cannot be evaluated.

**Mandatory requirement failure:** A proposal that fails a mandatory requirement is marked ineligible. A strong price or experience score cannot compensate for this failure.

**Conflicting information:** If different sections of a proposal contain conflicting values, the system preserves the conflicting evidence and requests reviewer attention rather than selecting one value without justification.

**Prompt injection:** Proposal documents are treated as untrusted input. Instructions embedded in a document that attempt to change system behaviour, influence rankings, or override evaluation rules are ignored. The extraction component is instructed to return facts only.

**Incorrect AI extraction:** Extracted values are linked to page references and evidence quotations. Reviewers can inspect the original document and correct inaccurate extraction before evaluation proceeds.

**Security and privacy:** Uploaded documents and extracted information should be accessible only to authorized users. A production implementation should apply access controls, secure storage, and appropriate retention policies.

## 4. Human Oversight and Auditability

The system provides decision support rather than autonomous procurement.

Reviewers can inspect mandatory checks, extracted evidence, criterion scores, weighted contributions, and clarification flags. They remain responsible for resolving uncertainty and selecting the final awardee.

An audit record should retain:

- Original proposal reference and extracted information.
- Supporting evidence and extraction status.
- Criteria configuration and version used.
- Mandatory check results and scoring breakdown.
- Reviewer corrections, overrides, and reasons.
- Reviewer identity, timestamps, and final award decision.

Any override should require a reason. This makes it possible to understand how an evaluation was produced and why a final decision differed from the provisional ranking.

## 5. Changes to Evaluation Criteria

Evaluation criteria should be stored as versioned configuration rather than embedded directly in application code.

When an administrator changes a weight, threshold, or mandatory requirement, the system creates a new configuration version and validates it before use. Weighted criteria must total 100%.

Existing evaluations retain the configuration version under which they were originally assessed. If a reviewer needs to evaluate proposals again using updated criteria, the system should create an explicit new evaluation rather than silently changing historical results.

## Conclusion

The proposed design combines AI-assisted document extraction with deterministic evaluation and human oversight. Its main priorities are traceability, consistent scoring, transparent explanations, and safe handling of incomplete or untrusted proposal information. The final procurement decision always remains with the human reviewer.