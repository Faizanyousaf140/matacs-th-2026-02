# AI-assisted proposal triage

## Evaluation policy and sample assumptions

This is a preliminary triage snapshot, not an award decision. Admin configuration
`solar-v1` supplies every field, requirement, scoring anchor, weight and tie-break.
The bidder's own stated licence and confirmed WSIB/$5M insurance are gates. I
also treat the budget ceiling and completion deadline as eligibility limits;
this is an explicit design assumption. A confirmed failure is ineligible;
unknown essential information is flagged, never guessed.

These proposed scoring anchors are assumptions for this exercise, not values
supplied by the employer. Define `clip(x) = max(0, min(1, x))`; each row gives
weighted points. Fixed anchors avoid changing a bidder's score when another
bidder joins or withdraws.

| Criterion | Weighted points |
| --- | --- |
| Experience | `30 * clip(qualifying_projects / 10)` |
| Price | `25 * clip((260000 - price_CAD) / 60000)` |
| Timeline | `15 * clip((16 - weeks) / 6)` |
| Warranty | `15 * (0.5 * clip(panel_years / 25) + 0.5 * clip(workmanship_years / 12))` |
| Team | `15 * (lead + crew + safety + monitoring) / 4`, each item 0 or 1 |

Ten qualifying projects is a sufficient experience target; price reaches full
marks at CAD 200,000, timeline at ten weeks, and warranty at 25/12 years.
Admin approval would be needed before using these assumptions operationally.
Meeting a budget/deadline boundary passes its gate but earns zero advantage
points on that criterion. Sum full-precision contributions, then round half-up
to two decimals. Break equal published totals by experience points, then lower
price; letter order only stabilises otherwise equal display positions, leaving
the substantive tie to the human.

For this sample, accept “since 2021” as the intended five-year cohort; operational
use requires exact project dates. C's broader period does not establish its
qualifying subset. Accept summaries explicitly saying a lead is named. An
unstated optional checklist item earns zero; genuinely unreadable information
remains null. Any unknown scoring input holds the complete score for review.

A ranks first at **77.92**, E second at **68.79**. A's experience, price and
timeline advantage outweighs E's stronger workmanship warranty. B is ineligible
because a subcontractor does not meet the bidder-licence requirement. C is
flagged for experience clarification and acknowledgement of its instruction
attempt; its claimed approval has no authority. D is flagged for its unknown
schedule. Neither receives a fabricated score. C may enter a later ranking
after evidence review; it is not automatically disqualified for suspicious text.

## 1. AI versus rules

The reviewer selects a category and uploads files. Code checks file type/size,
isolates PDF parsing, and retains document/page references; OCR handles scans.
AI turns extracted language into typed facts with source quotes and warnings.
Code validates the schema, units, quote matches and conflicting/missing fields,
then applies configured gates and arithmetic. No LLM chooses ranks or eligibility.
AI adds value in reading varied language; ordinary code is more reliable for
comparisons and calculations. The small prototype starts at extracted text or
derived facts; upload screens, PDF/OCR and authentication are design elements.

## 2. Consistency and explainability

Store document hashes, reviewed facts, configuration hash/version, prompt/model
version and evaluation ID. Reusing identical accepted facts and configuration
reproduces the same scores and ordering. Fresh AI extraction may differ, so
cache accepted extractions; low temperature alone is no guarantee. The results
view shows each gate, weighted contribution, missing field and supporting page
quote. Explanations come from those facts and calculations, not invented prose.
Quote matching confirms location, not semantic accuracy or document authenticity.

## 3. Human oversight and audit

A reviewer checks extracted facts before approving the recommendation and must
resolve holds. For C, acknowledge/ignore the attempted instruction and establish
the experience subset; for D, obtain a confirmed schedule. A correction requires
evidence and a reason, followed by validation and rescoring. A reviewer can
record a different award decision with justification but cannot silently erase
a failed requirement. Log actor, timestamp, before/after facts, source references,
review acknowledgements, criteria/prompt/model hashes, scores and final decision
in an append-only audit store. Prototype JSON exports demonstrate provenance,
not a production audit service or a real person's approval.

## 4. Changing criteria

An edit creates a new immutable category version. Validate weight totals and
rubrics, mark prior recommendations stale, and reevaluate every proposal against
the same version. Reuse verified fields only where their definitions still
match; newly required or changed fields need extraction/review. Preserve old
results and show what changed. Never compare scores from different versions or
silently alter an earlier award record.

## 5. Risks and safeguards

- **Extraction errors/hallucinations:** typed nullable fields, source evidence,
  unit checks, conflict flags and human confirmation; unknown is not zero.
- **Manipulation:** document content has no instruction authority; no model
  tools, schema-limited outputs, warning review, and deterministic scoring.
  Keyword detection is supplementary, not a guarantee against all attacks.
- **Bias:** identical disclosed rubrics; capped experience credit, evidence-only
  checks and reasoned human overrides. Names/marketing claims earn no points.
- **Cost/failures:** input/token caps, caching and explicit timeout/refusal/error
  handling; reject oversized prototype inputs instead of silently truncating.
- **Privacy/security:** only fictional inputs here; restrict operational access,
  isolate parsers, protect credentials, and use documented retention/deletion.
