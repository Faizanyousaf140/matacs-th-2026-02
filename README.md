# MATACS-TH-2026-02 - AI Proposal Triage Design

**Candidate:** Muhammad Faizan Yousaf  
**Reference:** MATACS-TH-2026-02  
**Time spent:** Approximately 3.5 hours, estimated from the breakdown below.

| Activity | Estimated time spent |
| --- | --- |
| Understanding the brief | 30-40 minutes |
| Planning the process and evaluation rules | 40-60 minutes |
| Coding and revisions | 60-80 minutes |
| Testing and debugging | 20-40 minutes |
| Final review | 10-20 minutes |
| GitHub setup and push | 10-20 minutes |
| Total estimated range | 170-260 minutes |

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

I used ChatGPT for drafting and coding support, then reviewed and tested the
output. I corrected a generated test to use UTF-8 because it failed with
Windows' default encoding, and reran the tests successfully.

## Data and assumptions

- Demo inputs are derived only from the five fictional proposals supplied in
  the assessment. No real client data is used.
- The original brief and proposal text are excluded from the repository.
- API credentials are read from environment variables. `.env.example` contains
  empty settings; no API keys are included.
- The write-up records the scoring anchors, budget/deadline eligibility policy,
  experience-window interpretation and missing-information assumptions.

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

The offline demo does not call an AI service. The optional live extraction
client is covered by mocked API tests.
