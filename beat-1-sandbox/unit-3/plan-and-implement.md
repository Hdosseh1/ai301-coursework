# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

Hdosseh1

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-howard/issues/53#issuecomment-6137884901

````markdown
Here is my plan for this one. It follows from my repro above.

**Cause**

The `phone_us` pattern in `safety/pii_scrubber.py` only allows `-` or `.` between the parts of a number, so the match stops at the space after `)`. My repro didn't show whether the parentheses or the space was the trigger, so I ran two more inputs on `99673c7`:

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

Parentheses with no space already match. Spaces with no parentheses don't. So the space is the trigger.

The same output shows a second problem in that pattern: the match starts at `555`, not at `(`. The pattern opens with `\b`, which can't match in front of `(`. If I only allowed spaces, `(555) 123-4567` would come out as `([REDACTED]`.

**Change**

One change, to the `phone_us` pattern only:

- allow a single space as a separator (`[-.]?` becomes `[-. ]?`)
- replace the leading `\b` with `(?<!\w)`, so a match can start at `(` or `+`

In `tests/unit/test_pii_scrubber.py` I'll remove the `xfail` marker from the four tests the issue names, and add one test that `detect()` returns the full `(555) 123-4567`, parenthesis included.

**What I'm leaving alone**

`test_mixed_pii_and_text` is marked `xfail` for #53, but it fails for a different reason. Its phone number is the dashed `555-123-4567`, which already works. What breaks it is the `street_address` pattern matching part of a sentence, so "Python" gets redacted:

```
$ python -c "import re; from safety.pii_scrubber import PIIScrubber as P; print(re.findall(P.PII_PATTERNS['street_address'], 'I worked at TechCorp for 5 years developing Python applications.', flags=re.I))"
['5 years developing Python appl']
```

My change doesn't touch that pattern, so I'm keeping that test's marker and leaving the address pattern and every other pattern alone.

**How I'll check it**

I'll re-run my repro steps on the branch:

- `scrub('Call me at (555) 123-4567 or 555-123-4567')` should print `Call me at [REDACTED] or [REDACTED]`
- `detect('Call me at (555) 123-4567')` should return one `phone_us` entry with the value `(555) 123-4567`
- the dashed control should come out the same as before
- `pytest tests/unit/test_pii_scrubber.py -q -rx` should report `25 passed, 1 xfailed`

**One thing to flag**

Allowing spaces widens the match. Any 3-3-4 digits separated by single spaces will now count as a phone number (for example `id 123 456 7890`), and `+1 555 123 4567` gets redacted too. I think redacting too much is the safer mistake for a PII scrubber, but I'd like a second opinion on that.

I'm building this on `fix/53-phone-us-space-separator` in my fork.
````

---

## Your branch

**Branch**

`fix/53-phone-us-space-separator`

**Evidence**

These are the three steps from my Unit 2 repro comment, run from the repo root with the venv
activated: first at the branch point (`99673c7`, before any edit), then on
`fix/53-phone-us-space-separator` with the change applied.

**Before**

```
$ python - <<'EOF'
from safety.pii_scrubber import PIIScrubber
s = PIIScrubber()
print(s.scrub('Call me at (555) 123-4567 or 555-123-4567'))
print(s.detect('Call me at (555) 123-4567'))
EOF
Call me at (555) 123-4567 or [REDACTED]
2026-10-12 23:49:24 [info     ] pii_detected                   count=0 types=0
[]

$ python -c "from safety.pii_scrubber import PIIScrubber; print(PIIScrubber().detect('Call me at 555-123-4567'))"
2026-10-12 23:49:24 [info     ] pii_detected                   count=1 types=1
[{'type': 'phone_us', 'value': '555-123-4567', 'start': 11, 'end': 23}]

$ pytest tests/unit/test_pii_scrubber.py -q -rx
..xx.......x.....x....x..                                                [100%]
=========================== short test summary info ============================
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_number_redaction - issue #53: PII scrubber does not redact parenthesized US phone numbers
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_us_phone_formats - issue #53: PII scrubber does not redact parenthesized US phone numbers
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_detect_phone_pii - issue #53: PII scrubber does not redact parenthesized US phone numbers
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_phone_at_start_of_text - issue #53: PII scrubber does not redact parenthesized US phone numbers
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_mixed_pii_and_text - issue #53: PII scrubber does not redact parenthesized US phone numbers
20 passed, 5 xfailed in 0.22s
```

**After**

```
$ python - <<'EOF'
from safety.pii_scrubber import PIIScrubber
s = PIIScrubber()
print(s.scrub('Call me at (555) 123-4567 or 555-123-4567'))
print(s.detect('Call me at (555) 123-4567'))
EOF
Call me at [REDACTED] or [REDACTED]
2026-10-12 23:49:50 [info     ] pii_detected                   count=1 types=1
[{'type': 'phone_us', 'value': '(555) 123-4567', 'start': 11, 'end': 25}]

$ python -c "from safety.pii_scrubber import PIIScrubber; print(PIIScrubber().detect('Call me at 555-123-4567'))"
2026-10-12 23:49:50 [info     ] pii_detected                   count=1 types=1
[{'type': 'phone_us', 'value': '555-123-4567', 'start': 11, 'end': 23}]

$ pytest tests/unit/test_pii_scrubber.py -q -rx
.......................x..                                               [100%]
=========================== short test summary info ============================
XFAIL tests/unit/test_pii_scrubber.py::TestPIIScrubber::test_mixed_pii_and_text - issue #53: PII scrubber does not redact parenthesized US phone numbers
25 passed, 1 xfailed in 0.21s
```

The parenthesized number is now redacted and detected, the dashed control is unchanged, and the one
remaining xfail is `test_mixed_pii_and_text`, which fails on the `street_address` pattern and is out of
scope in my plan. On the branch, `ruff check .`, `black --check .`, and `mypy` all pass, and
`make test-unit` reports `380 passed, 49 xfailed`.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. `--include-calibration --only calib-01,calib-02,calib-03,calib-04,pkg-04,pkg-14,pkg-20` (first draft of the rubric, evidence guide, and procedure): `agreement: 3/3 scored items`. The four calibration rows also read `yes` in the table.
2. Full run with the same files, committed as `eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`

Nothing was revised between the two runs.

**Package analysis**

`pkg-15` (laurent22/joplin#16215), category `scope-creep`. My rubric decided **reject**. The gold label is
**reject**
(`"the isolated one-constant fix is bundled with an undici migration, a settings panel, UI rework, and a retry framework; while-in-the-area work the issue never asked for; arguable because the core fix is right"`).

I picked it because it is the scored package where the core of the plan is right. The diagnosis fits the
repro's control matrix, and the first change in the plan is the fix:

> 1. Set the default `autoSelectFamilyAttemptTimeout` to 500 ms at app startup to match current Node (the direct fix).

The grader's evidence in the committed run agrees: `cause-grounded` passed with
"250 ms autoSelectFamilyAttemptTimeout abort; controls 3-5 all fit (500 ms succeeds 10/10, forced 250 fails 8/10)"
and `fixes-reported-behavior` passed with
"Change 1 sets default to 500 ms at startup, so the failing sync succeeds with no user action".
Six of the seven required checks passed. The one that failed was `one-bounded-change`:
"Changes 2-5 are extra: node-fetch->undici migration, new Settings field, silent-success UI fix (#12810), retry-with-backoff wrapper".

My rubric reads it as a reject because `one-bounded-change` ends with "A correct core fix does not rescue a
plan that bundles these, and offering to split the work into a PR series does not either." The plan comment
closes with "Happy to split into a PR series, starting with the undici migration.", which is the exact way
out that sentence shuts. My guess is that without that sentence a grader would weigh the correct fix against
the extras and could go either way.

**Check rationale**

`cause-grounded`, pass condition as it reads now in `tools/plan-check/rubric.md`:

> The plan names a specific cause (a mechanism at a named place, not "something changed"), and nothing the repro evidence records contradicts it: the failing run fails the way that cause says, and every control run comes out the way the evidence shows. An inferred cause passes; it does not have to be proven, and a result the cause is merely silent on is not a contradiction. Fails when a recorded result could not happen if the cause were true (the blamed component is shown working in a control run, the damage is shown to exist before the blamed step runs, or an artifact locates the failure in a different layer than the one blamed), when the plan sets a recorded result aside ("red herring", "unrelated", "side effect") without an artifact for doing so, or when the plan names no cause at all. A cause adopted from the thread gets no credit for its source; it is held to the same evidence.

I did not revise this check between runs; both runs used this wording. It reads this way because of two
versions I rejected before the first run:

- "The cause is proven by the repro evidence." That is too strict. calib-01 is an accept with no control
  run and no code-level evidence, only a cause that fits its four steps, and pkg-14's cause is inferred from
  a regression window. That is where "An inferred cause passes; it does not have to be proven, and a result
  the cause is merely silent on is not a contradiction" comes from. In the full run pkg-14 passed with
  "Cause (stdin wired before OSC responses drained on reattach) fits fresh-clean, reattach-leaks, 0.44.1-clean and cache control; no recorded result contradicts it."
- "The cause agrees with the diagnosis in the thread." calib-03's plan adopts a root cause that a commenter
  states confidently, and the package's own timing matrix rules it out. So the check gives the thread no
  weight: "A cause adopted from the thread gets no credit for its source; it is held to the same evidence."
  In run 1 calib-03 failed with
  "Step 3 shows 25.8 s with --paging=never (no pager) and step 2 shows the same pager seeking in <0.3 s with --color=never, contradicting a pager-binding cause; plan calls highlighting 'unrelated'."

The three fail shapes in the parentheses are the ways a control run can rule a cause out. pkg-11 is the
first one: "Plan blames the collect operator, but the control shows top-level collect yields [null] (length 1) 'exactly as documented'; only select/and/or contexts drop the key, so the blamed component is shown working."

**Trade-offs**

`cause-grounded` gives up wrong causes that the repro has no run to test. Because "a result the cause is
merely silent on is not a contradiction", a plan with a wrong diagnosis passes this check whenever the repro
evidence has no control that touches it. calib-01 has no control run at all and passes on fitting four
steps. I accept that miss. The alternative is demanding proof, and that rejects terse plans that are ready.

Nothing changed between my two runs, so there was no loosened check to canary. The three scored packages
from run 1 were graded the same way in run 2: pkg-04 reject, pkg-14 accept, pkg-20 reject.

What the 20/20 does not show: four of the rejects rest on a single required check. pkg-06 and pkg-15 fail
only `one-bounded-change`, pkg-11 fails only `cause-grounded`, and pkg-20 fails only `ai-policy-met`. One
different grade on any of those flips the package, and I have one full run, so I cannot say how stable
those four are.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
