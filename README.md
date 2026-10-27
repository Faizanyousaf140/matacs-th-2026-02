# MATACS-TH-2026-02 - AI Proposal Triage Design

**Candidate:** Muhammad Faizan Yousaf  
**Reference:** MATACS-TH-2026-02  
**Time actually spent:** 23.1 minutes of AI-assisted implementation and verification before packaging (27 Oct 2026, 22:13 to 22:36 Pakistan time). Earlier brief discussion and any additional candidate review are excluded.

This submission describes an AI-assisted review process with configurable
criteria, evidence-backed extraction, deterministic scoring and human award
authority. No deployed application is required. Start with the diagram and
write-up; the optional command-line prototype reproduces the sample results.

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

C's instruction attempt has no scoring authority. Its experience period needs
clarification and its security warning needs reviewer acknowledgement before
ranking. D has no confirmed schedule. These are preliminary results: a human
must review evidence and make the final award decision.

## Run and verify

Requires Python 3.10+ and no third-party packages for the demo/tests.

```sh
python prototype/triage.py demo
python -m unittest discover -s prototype/tests -v
```

Use `py` on Windows if needed. The demo writes results and provenance into
ignored `.local-demo/`; it makes no network calls and exactly reproduces
`ranking.json`. The [prototype guide](prototype/README.md) explains optional
live extraction, configuration changes and deliberate scope limits.
The [verification record](prototype/VERIFICATION.md) distinguishes checks run
locally from live API and hosted checks that remain unperformed.

## AI-use disclosure

ChatGPT assisted with interpreting the brief, drafting the design and prompts,
creating the diagram, implementing the prototype, and running verification.
Tests found that the draft AI-generated client assumed API JSON always had the
expected object shape; this was corrected with explicit shape checks and
regression tests that reject malformed responses safely.
No live model extraction was performed: the public demo uses derived structured
facts rather than a claimed AI extraction log.

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

The original brief and proposal text are intentionally absent. Submit this
public repository URL through the assessment Google Form; the ZIP is a handoff
format, not an accepted assessment submission.
