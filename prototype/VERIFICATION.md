# Verification

Run from the repository root:

```sh
python -m unittest discover -s prototype/tests -v
python prototype/triage.py demo
```

## Results

- The ranking matches `ranking.json`: A scores 77.92 and E scores 68.79.
- Repeated runs and shuffled inputs produce the same scores and ordering.
- B remains ineligible despite its lower price. C and D remain flagged while
  their review issues are unresolved.
- Tests cover budget/deadline boundaries, missing information, invalid types,
  changed criteria, tie-breaking, source evidence and API error handling.
- The write-up is 852 words. README links resolve, and the diagram is a
  single-page PDF that has been rendered and checked.

The original 52-test suite passed on Windows with Python 3.14 after fixing the
write-up's UTF-8 read. This package includes one additional regression test that
simulates a Windows cp1252 default, bringing the total to 53. All 53 tests pass
on Linux with Python 3.12 and `ResourceWarning` treated as an error. The HTTP
error test also checks that its response body is closed.

The GitHub workflow covers Windows/Linux and Python 3.10/3.13/3.14. Check its
results after pushing the repository.

## Scope

The demo evaluates structured facts from the supplied fictional examples.
The API client is tested with mocked responses; live extraction accuracy has
not been measured. Tests that change facts are behaviour checks, not additional
claims about the contractors. The design describes PDF/OCR, authenticated
review and persistent audit storage; those services are outside this prototype.
