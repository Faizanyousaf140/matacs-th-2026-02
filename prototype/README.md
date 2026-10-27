# Optional prototype

Python 3.10+; standard library only. Run commands from the repository root.
On Windows, use `py` instead of `python` if that is your Python launcher.

## Offline demonstration

```sh
python prototype/triage.py demo
python -m unittest discover -s prototype/tests -v
```

Expected summary:

```text
A  rank= 1  ranked      score=77.92
E  rank= 2  ranked      score=68.79
B  rank= -  ineligible  score=-
C  rank= -  flagged     score=-
D  rank= -  flagged     score=-
```

The demo makes **no network call**. `demo_facts.json` is a manually structured,
AI-assisted transcription of derived sample facts, not a live model response.
It contains letters, numbers, booleans, source-location labels and warnings;
no original proposal text is redistributed. Evidence quote values are withheld
as null in this public fixture. Therefore offline replay does not demonstrate
source-to-fact extraction accuracy or completed human approval.

The three generated files in ignored `.local-demo/` are `ranking.json`,
`details.json` (contributions, holds, failures), and `audit.json` (versions,
hashes, timestamp and a reproducible evaluation ID). Rerunning the demo replaces
these local exports; an append-only operational audit store is only designed,
not implemented. The evaluation ID excludes the run timestamp.

## Configuration changes

Copy `config.json` to an ignored local directory, edit its version/rubric,
and run:

```sh
python prototype/triage.py --config .private/config-v2.json demo --out .local-v2
```

All requirements, fields, weights, scoring anchors and tie-break references
come from configuration. Invalid weights, types, references or anchors are
rejected. The generic engine supports target, inverse-linear, composite and
checklist rubrics; a new rubric *kind* would require an engine extension.
The admin UI and immutable-version database are not built.

## Optional AI extraction

Only use the supplied fictional samples. Keep the assessment brief and any
local sample text in ignored `.private/`. Do not commit the original PDFs,
proposal text, rendered requests or live extraction evidence.

1. Put one supplied summary in `.private/A.txt`, retaining page labels if known.
2. Set `OPENAI_MODEL` to an available model supporting Responses Structured
   Outputs. Set `OPENAI_API_KEY` in your terminal environment for a live call.
   `.env.example` documents the variables; the script does not load `.env`.
3. Preview the exact request without sending it:

```sh
python prototype/triage.py request --proposal .private/A.txt --id A --model YOUR_MODEL
python prototype/triage.py schema
```

4. Optionally send the prompt (this incurs API usage):

```sh
python prototype/triage.py extract --proposal .private/A.txt --id A --output .private/A-extracted.json
```

The command validates completion, refusals, exact output properties, fact types,
proposal ID and source quote matches. It saves provenance in a sibling
`A-extracted.audit.json`, without API credentials. It makes one request with a
30-second timeout; failures produce no accepted extraction. Requests have a
16,000-character input limit and 4,000 output-token cap. A refusal or truncated
response must be reviewed rather than treated as an empty eligible proposal.

5. A human reviews the facts and warnings. For local experimentation, combine
   reviewed response objects into a JSON list and evaluate:

```sh
python prototype/triage.py rank --facts .private/reviewed-facts.json --out .local-reviewed
```

`rank` validates structured inputs but cannot authenticate a reviewer's identity
or prove they reviewed them. Persistent approval/override workflows are outside
this small prototype. Warnings require explicit resolution in a new local input;
they are never automatically cleared by ranking code. Tests that mutate C's
facts demonstrate a counterfactual resolution, not a claim about C.

The live client contract is tested with mocked HTTP responses. **No live model
call was made during preparation**, because no API credential was available.
The tests do not prove a model will resist every prompt injection.
