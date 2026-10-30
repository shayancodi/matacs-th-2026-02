# AI Proposal Triage — System Design

**Assessment Reference:** MATACS-TH-2026-02

## 1. AI vs. Rules

I would use AI for extracting information from uploaded proposal documents because proposals may use different wording, layouts and formats. The extraction service would identify relevant facts such as licence details, insurance coverage, project experience, pricing, timelines, warranties and team information. Each extracted value should include supporting evidence, such as a page number and text excerpt.

I would use conventional application code for file validation, mandatory requirement checks, weighted score calculations, ranking and access control. These operations require predictable and repeatable results. AI should not decide whether a bidder passes mandatory requirements or independently assign final scores.

The extracted information would first pass schema validation and evidence checks. Missing, ambiguous or conflicting information would be sent for human review rather than treated as confirmed.

## 2. Consistency and Explainability

The scoring engine would use a versioned criteria configuration supplied by the administrator. Each configuration would contain mandatory requirements, weights, scoring formulas, thresholds and reference values. The weighted criteria must total 100%.

For repeatability, the same validated evidence and criteria version would always produce the same result. Scores would be calculated by deterministic code, using fixed rounding rules and a documented tie-breaking policy.

Reviewers would see each proposal's extracted evidence, mandatory check results, individual criterion scores, weighted contributions, total score and reason for its position. The system would also identify missing or uncertain information.

Proposed scoring formulas and reference values would be documented as configurable assumptions rather than hard-coded business rules.

## 3. Human in the Loop

A reviewer would confirm extracted information when evidence is missing, ambiguous or conflicting. Mandatory failures and flagged proposals would be clearly identified. Reviewers could request clarification, confirm evidence or correct extraction errors.

The system would provide a ranked shortlist, but a human would make the final award decision. Any manual correction or override would require a reason, the reviewer's identity and a timestamp.

The audit log would record the original uploaded file reference, extraction output, evidence references, criteria version, scoring configuration, calculated results, reviewer changes, overrides and final award decision. Original results would remain available for comparison.

## 4. Changing Criteria

Every administrator update would create a new version of the category's evaluation configuration. Previous evaluations would retain the version under which they were originally calculated.

If criteria or weights change after proposals have been scored, the system would not silently modify existing results. An authorized reviewer could initiate a new evaluation using the updated configuration. The system would preserve both results and show which criteria version produced each ranking.

Before activation, the application would validate that mandatory criteria are defined correctly and weighted criteria total 100%.

## 5. Risks and Safeguards

- **Incorrect extraction:** AI may misread values or miss information. Require structured output, source references, schema validation and human confirmation for uncertain evidence.
- **Hallucinated information:** Never accept an extracted claim without supporting document evidence. Missing facts remain unknown rather than being invented.
- **Prompt injection:** Treat proposal content as untrusted data, not instructions. Ignore embedded requests to change rankings, scores or system behaviour. Use fixed system instructions and validate outputs independently.
- **Bias and inconsistent evaluation:** Apply the same versioned criteria and scoring formulas to every bidder. Keep scoring rules transparent and review unusual outcomes.
- **Missing or conflicting information:** Flag affected criteria and require clarification. Do not silently assign a passing mandatory result or invent a weighted score.
- **Security and privacy:** Restrict access through role-based permissions, validate uploaded files, protect stored documents and avoid sending unnecessary sensitive information to external AI services.
- **Cost and reliability:** Limit document size and processing retries, monitor AI usage and provide clear failure states. A failed AI extraction must not produce an apparently valid ranking.

The system would support procurement decisions, not replace human judgement. No proposal would receive an award solely because of an AI-generated recommendation.