# MATACS-TH-2026-02 - AI Proposal Triage Design

**Candidate:** Muhammad Faizan Yousaf  
**Reference:** MATACS-TH-2026-02  
**Time spent:** In progress; record the actual total after the final review.

This design uses AI to extract proposal facts and fixed rules to check
eligibility and calculate scores. Criteria are configurable, each result has
supporting evidence, and a reviewer makes the final award decision. The optional
Python prototype reproduces the sample ranking.

## Required deliverables

- [Process diagram](diagram/flow.pdf)
- [Design write-up and scoring rubric](WRITEUP.md)
- [Exact AI prompt templates](PROMPTS.md)
- [Expected ranking](ranking.json)
- [This README](README.md)

## Sample outcome

| Proposal | Rank | Status | Score / 100 |
| --- | --- | --- | --- |
| A | 1 | ranked | 77.92 |
| E | 2 | ranked | 68.79 |
| B | - | ineligible | - |
| C | - | flagged | - |
| D | - | flagged | - |

C needs review because its experience period is unclear and its appendix tries
to influence automated scoring. Those instructions are ignored. D needs a
confirmed completion schedule. Both remain unranked until the missing
information is resolved.

## Run and verify

Requires Python 3.10+ and no third-party packages for the demo/tests.

```sh
python prototype/triage.py demo
python -m unittest discover -s prototype/tests -v
```

Use `py` on Windows if needed. The demo runs offline and writes its results,
score breakdowns and audit details to `.local-demo/`. See the
[prototype guide](prototype/README.md) for configuration and optional extraction,
and the [verification record](prototype/VERIFICATION.md) for test coverage.

## AI-use disclosure

I used ChatGPT to help prepare the design, prompts, diagram and prototype.
During testing, I corrected a Windows encoding issue in the generated test code
and reran the demo and test suite to check the fix.

## Optional files

- [Prototype guide](prototype/README.md)
- [Scoring engine and optional AI client](prototype/triage.py)
- [Category configuration](prototype/config.json)
- [Derived demo facts, without proposal text](prototype/demo_facts.json)
- [Behaviour and contract tests](prototype/tests/test_triage.py)
- [Verification record](prototype/VERIFICATION.md)
- [GitHub verification workflow](.github/workflows/verify.yml)
- [Environment-variable template](.env.example)
- [Ignored private/generated files](.gitignore)

The repository contains derived sample facts rather than the original brief or
proposal text. The offline demo does not call an AI service; the optional live
extraction client is covered by mocked API tests.
