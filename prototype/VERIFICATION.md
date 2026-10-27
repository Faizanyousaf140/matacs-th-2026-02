# Verification record

Prepared on 27 October 2026. Local runtime: CPython 3.12 on Linux.

## Executed checks

| Check | Result |
| --- | --- |
| Behaviour, validation, API-contract and submission tests | 52 passed |
| Recompute published `ranking.json` | Exact JSON equality |
| Independent arithmetic for A/E | 77.92 / 68.79 |
| Repeated runs and shuffled input | Same scores, ranking and reasons |
| Budget/deadline boundaries | Boundary passes; excess fails |
| Bidder licence / WSIB / insurance | Confirmed failures cannot be rescued by price |
| Missing required or weighted facts | Flagged; no invented score |
| C's unresolved warning/experience | Both must be resolved before ranking |
| Counterfactual C review resolution | Can re-enter ranking; never receives automatic full marks |
| Changed category priorities | E can outrank A under a new configuration |
| Unknown fields, model-supplied scores, bad types | Rejected |
| JSON duplicate properties / non-finite numbers | Rejected |
| Mocked API completion, refusal, timeout/network/HTTP failure | Controlled acceptance or rejection |
| Malformed API object/content shapes | Rejected safely after a regression fix |
| Prompt rendering / source-quote checks | Escaping and evidence checks passed |
| README links and complete file index | Checked by the suite |
| Write-up length | 852 whitespace-delimited words; below 1,000 |
| Diagram | One PDF page, rendered and visually inspected with embedded fonts |

Run the checks yourself from the repository root:

```sh
python -m unittest discover -s prototype/tests -v
python prototype/triage.py demo
```

## Verification boundaries

- The offline demonstration replays derived facts; it does not extract an actual
  PDF or measure AI extraction accuracy.
- The optional live Responses API client was tested with mocked HTTP responses,
  not a paid/live model request. No API credential was available.
- GitHub was not connected for publication. Hosted Actions and public visibility
  remain to be verified after the candidate pushes the repository.
- The workflow is configured for Windows/Linux and Python 3.10/3.13. Those four
  hosted combinations were not run in this local preparation environment.
- Upload/OCR, identity verification, immutable storage, reviewer approvals and
  award decisions are specified in the design, not deployed production features.

Tests that alter proposal values or weights are functional scenarios, not new
claims about any sample contractor. No original proposal text or live evidence
quotes are included in the public fixture.
