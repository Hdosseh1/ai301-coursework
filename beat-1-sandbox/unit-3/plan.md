# Plan: issue #53, PII scrubber misses `(555) 123-4567`

- Issue: https://github.com/codepath/pathreview-ai301-fa26-howard/issues/53
- Builds on my repro comment: https://github.com/codepath/pathreview-ai301-fa26-howard/issues/53#issuecomment-6009556947
- Branch: `fix/53-phone-us-space-separator`, on my fork
- Checked against commit `99673c7` (`main`), macOS 15.5, Python 3.11.13

## Diagnosis

The `phone_us` pattern in `safety/pii_scrubber.py` (line 16) cannot match
a space between the parts of a number. Its separators are `[-.]?`, so on
`(555) 123-4567` the match stops at the space after `)`.

My repro showed `(555) 123-4567` passing through while `555-123-4567` was
redacted. That does not say whether the parentheses or the space is the
trigger, so I ran two more inputs on the same commit:

```
$ python - <<'EOF'
import re
from safety.pii_scrubber import PIIScrubber
p = PIIScrubber.PII_PATTERNS["phone_us"]
for t in ["Call me at (555)123-4567", "Call me at 555 123 4567"]:
    print(t, "->", [m.group() for m in re.finditer(p, t)])
EOF
Call me at (555)123-4567 -> ['555)123-4567']
Call me at 555 123 4567 -> []
```

Parentheses with no space already match. Spaces with no parentheses do
not. The space is the trigger.

That output also shows a second, smaller defect in the same pattern: the
match starts at `555`, not at `(`. The pattern opens with `\b`, and there
is no word boundary between a space and `(`, so the optional `\(?` never
gets to match there. If I only allowed spaces, `(555) 123-4567` would be
redacted to `([REDACTED]`, with the opening parenthesis left behind.

## Change

One change, to the `phone_us` entry of `PII_PATTERNS` in
`safety/pii_scrubber.py`. Two edits to that one pattern:

1. Allow a single space as a separator: `[-.]?` becomes `[-. ]?` in all
   three places (after the optional `+1`, after the area code, after the
   exchange).
2. Replace the leading `\b` with `(?<!\w)`, so a match can start at `(`
   or `+` and the whole number is replaced.

The pattern after the change:

```
(?<!\w)(?:\+?1[-. ]?)?\(?([0-9]{3})\)?[-. ]?([0-9]{3})[-. ]?([0-9]{4})\b
```

In `tests/unit/test_pii_scrubber.py`:

- Remove the `@pytest.mark.xfail` marker from the four tests the issue
  names: `test_us_phone_number_redaction`, `test_us_phone_formats`,
  `test_detect_phone_pii`, `test_phone_at_start_of_text`.
  `docs/CONTRIBUTING.md` asks for this when fixing a seeded bug.
- Add one test that `detect("Call me at (555) 123-4567")` returns the
  value `(555) 123-4567`. The four existing tests would still pass with
  the stray `(` left behind, so nothing pins edit 2 without it.

Not in scope:

- `test_mixed_pii_and_text`. It is marked xfail for #53, but it fails for
  a different reason. Its phone number is the dashed `555-123-4567`,
  which already works. The `street_address` pattern matches part of a
  sentence in the test text, so the word "Python" is redacted:

  ```
  $ python -c "import re; from safety.pii_scrubber import PIIScrubber as P; print(re.findall(P.PII_PATTERNS['street_address'], 'I worked at TechCorp for 5 years developing Python applications.', flags=re.I))"
  ['5 years developing Python appl']
  ```

  My change does not touch that pattern, so the test keeps failing and
  its marker stays. I will point this out in the PR and leave the
  address pattern alone.
- `phone_intl` and every other pattern in `PII_PATTERNS`.
- Phone formats the issue does not mention (extensions, letters).
- The `scrub()` and `detect()` methods.

## Test plan

Re-run the steps from my repro comment on the branch:

1. `s.scrub('Call me at (555) 123-4567 or 555-123-4567')` prints
   `Call me at [REDACTED] or [REDACTED]`.
   Before: `Call me at (555) 123-4567 or [REDACTED]`.
2. `s.detect('Call me at (555) 123-4567')` returns
   `[{'type': 'phone_us', 'value': '(555) 123-4567', 'start': 11, 'end': 25}]`.
   Before: `[]`.
3. Control, unchanged: `detect('Call me at 555-123-4567')` still returns
   the value `555-123-4567` at `start` 11, `end` 23.
4. `pytest tests/unit/test_pii_scrubber.py -q -rx` reports
   `25 passed, 1 xfailed`, and the one xfail is `test_mixed_pii_and_text`.
   Before: `20 passed, 5 xfailed`.
5. `make check` and `make test-unit` pass, as `docs/CONTRIBUTING.md`
   requires before a PR.

## Risks and unknowns

- The change widens what counts as a US phone number. Any 3-3-4 run of
  digits separated by single spaces now matches, for example
  `id 123 456 7890`. For a PII scrubber I think redacting too much is the
  safer mistake, but it is a behavior change and I am flagging it for
  review.
- `+1 555 123 4567` gets redacted as a side effect of the same space
  separator. It is one of the formats in `test_us_phone_formats`, so I am
  treating it as part of this fix and not as a separate change.
- Checked so far: I tried the new pattern in a Python session without
  editing the file. The repro inputs gave the results in steps 1 to 3,
  and the rest of `tests/unit` was unaffected (the only tests that
  changed result were the four named above). `safety/pii_scrubber.py` is
  not imported anywhere else in the Python code.
- Not checked yet: `make check` (ruff, black, mypy) on the edited file,
  and the integration tests.

## Deviations

Nothing changed; the plan held. I made the two edits to the `phone_us`
pattern as written, removed the four xfail markers, and added the one
`detect()` test. The results matched the test plan:
`25 passed, 1 xfailed`, with `test_mixed_pii_and_text` as the one xfail
left.

The two things the plan listed as not checked yet are checked now. On
the branch, `ruff check .`, `black --check .`, and `mypy` pass, and
`make test-unit` reports `380 passed, 49 xfailed`. `tests/integration`
has no tests in it, so there was nothing to run there.
