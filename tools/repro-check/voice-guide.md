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

## Things I never post

- "+1", "any updates?", or "same here" with nothing new.
- "Kindly assign me" or asking anyone to reserve an issue for me.
- Deadlines or guarantees about a fix.
- A diagnosis I haven't backed with output.
- Anything I can't explain if a maintainer asks a follow-up.
