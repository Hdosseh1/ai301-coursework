# Voice guide: how I talk upstream

## Who I am in threads

I'm a student at Howard making my first open-source contributions
through AI301. I say plainly that I'm new to the codebase, I only
report what I ran myself, and readers can expect me to come back with
what I found, including when I find nothing.

## Rules I write by

### Rule: Show it, don't sell it

Every "reproduced" or "confirmed" points at output I pasted. No
certainty words standing in for evidence.

- Wrong: "Can 100% confirm this bug, it happens every single time."
- Right: "Reproduced on 3.2.4; the output is below, next to a control run without the header."

### Rule: Say what I'll do next, not when I'll finish

I name the next concrete step (a file, function, or experiment) and
never promise a fix or a date.

- Wrong: "Please assign this to me, I'll have a fix in 2 days."
- Right: "Next I'm going to read how `line_range.rs` validates offsets and report back what I find."

### Rule: A guess is labeled a guess

If I haven't shown it, I write it as a hypothesis.

- Wrong: "The root cause is a race in the debounced save."
- Right: "My guess is the debounced save gets cancelled on note switch, but I haven't confirmed that yet."

### Rule: Name my differences

If my version, OS, or setup differs from the report, I say so in the
same comment instead of letting readers assume it matches.

- Wrong: "Confirmed on my machine."
- Right: "Tested on pandas 3.0.5 on Ubuntu; the report is on main on macOS."

### Rule: Disclose AI when the repo asks

I check the repo's AI policy before posting, and if it asks for
disclosure I say which tool I used and what for.

- Wrong: (posting an AI-organized report on a repo that requires disclosure, without a word about it)
- Right: "I used Claude to help organize this report; I ran every step and checked every output myself."

### Rule: Answer the direction that's already in the thread

If a maintainer has already pointed at a cause or a fix, I say whether
my plan follows it, and if it doesn't, I say why.

- Wrong: "My plan is to document the workaround in the README." (on a thread where the owner already posted a patch to test)
- Right: "I read the patch above. My plan builds on it: a test for the first-keypress case it still misses."

### Rule: Say what I'm changing and what I'm leaving alone

A plan comment names the one change and where it goes, and names what
I'm not touching.

- Wrong: "I'll fix this properly and clean up the module while I'm in there."
- Right: "One change, in the grouping step of `delpaths_sorted`. I'm leaving the larger rework discussed above for a separate effort."

### Rule: The title says what changed, not how I feel about it

A PR title names the behavior that changes and where, in the repo's
convention, so a maintainer knows what they are reviewing before
opening it.

- Wrong: "fixed the phone bug!!"
- Right: "fix(safety): redact parenthesized and space-separated US phone numbers"

### Rule: The description promises exactly what the diff contains

Every file, test, or doc the description says was added is in the
diff, and "no other changes" is only written after I've read my own
`git diff main...HEAD` top to bottom.

- Wrong: "Implements the plan exactly; tests and docs updated." (with no docs file in the diff)
- Right: "Two edits to the `phone_us` pattern, the four xfail markers removed, and one new `detect()` test. Nothing else in the diff."

### Rule: A shortfall is stated as a fact, not an apology

If something the reader might expect is not in the PR, I say what is
left out and why, once, plainly.

- Wrong: "Sorry, I couldn't get the other test working, hope that's ok."
- Right: "`test_mixed_pii_and_text` still fails, on the `street_address` pattern, which this PR does not touch; its xfail marker stays."

## Things I never post

- "+1", "any updates?", or "same here" with nothing new.
- "Kindly assign me" or asking anyone to reserve an issue for me.
- Deadlines or guarantees about a fix.
- A diagnosis I haven't backed with output.
- Anything I can't explain if a maintainer asks a follow-up.
- "Same approach as above" on someone else's plan.
- A plan whose steps are "investigate" or "look into" where the change should be.
- "All tests pass" without naming the command and showing its result.
- A checked box in the PR template for a check I didn't run.
