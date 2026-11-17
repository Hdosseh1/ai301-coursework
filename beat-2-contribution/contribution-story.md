# Contribution: [Issue Title]

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** [Student name]
**GitHub Username:** Hdosseh1
**Issue:** https://github.com/pyrite-wiki/pyrite/issues/1559
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built issue-scout's search strategy, ran the first wild scan, patched the rubric and ranking**

I wrote a four-query search strategy (Python and C++ `good first issue`, Verilog, VHDL) and ran
my first real scan on 2026-11-16: 396 issues narrowed to 5, and all 5 were accepted. The scan
exposed several problems. The tie-break pushed issues filed minutes earlier to the top. One
repo took 3 of the 5 slots. The rubric accepted cineforge-agent, a solo repo with 0 stars
created that same day. My hardware queries never reached grading. I added a required
repo-in-use check and a preferred small-codebase check to the rubric, changed the tie-break
to oldest-first, and capped results at 2 per repo, then regraded #1559 and #1560 (both
still accepted).

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I didn't browse for an issue by hand. I used my issue-scout skill, which runs `gh search issues`
with a strategy I wrote from my fit profile: `good first issue` in Python and C++, plus open
Verilog and VHDL issues because I wanted something hardware-related. On 2026-11-16 the first scan
pulled 396 unique issues. It dropped the assigned ones and the ones not updated in 60 days, which
left 346, and graded the top 5 against my rubric. All 5 were accepted, and
pyrite-wiki/pyrite#1559 came out on top. After patching my rubric I regraded it directly. It still
passed all seven required checks and four of the five preferred ones. The one it missed was
small-codebase, because pyrite is about 14 MB.

The issue is a small, well-defined bug. `pyrite task list --priority` accepts any integer, while
`task create` and `task update` already reject anything outside 1-10, so the filter on `list` was
just missed. The issue names the exact line (`pyrite/cli/task_commands.py:293`) and the test file
to extend, and that file already has a fixture for it. That fits what I said I wanted: Python,
tests, and practice fixing bugs. It does go against "avoid giant codebases". I picked it anyway
because the change is one option and one test file, and the project is very active: a release
on 2026-11-16, maintainer replies within a day, and a CONTRIBUTING.md that welcomes AI-assisted
work. I want to learn how a bigger project's workflow runs (branching from `dev`, changelog
fragments, their ratchet test) and how Typer validates options. My honest read of the scout:
its checks held up, but its ranking didn't. #1559 reached the top mostly because my tie-break
favored the newest issues, and it had been filed minutes before the scan. None of my hardware
queries ever reached grading. I changed the tie-break to oldest-first, capped results at 2 per
repo, and added repo-in-use and small-codebase checks to the rubric. The hardware queries still
need fixing, and I haven't rescanned with the new strategy yet.

**Claim comment:** claim pending (2026-11-16)

**Toolkit**

- Tool: issue-scout, a Claude Code skill that drives the `gh` CLI. I wrote
  its search strategy (4 queries: Python and C++ `good first issue`,
  Verilog, VHDL; ranking filters; top 5) and its rubric (7 required checks,
  5 preferred). I ran one full scan, then patched the rubric and regraded
  the two pyrite issues directly.
- What it produced: Scan 1 (2026-11-16) narrowed 396 unique results to 5
  graded candidates and accepted all 5, with pyrite-wiki/pyrite#1559 ranked
  first. On the direct regrade under the patched rubric, #1559 was still
  accepted: every required check passes and 4 of 5 preferred checks pass.
  It fails only small-codebase (repo is 14,499 KB, over the 10 MB line).
- Difficulties: (1) The recency tie-break decided the whole top 5, which
  were issues filed minutes before the scan. (2) There was no per-repo cap,
  so 3 of 5 slots went to one repo. (3) The Verilog/VHDL queries left 166
  survivors but almost none had a newcomer label, so none reached grading.
  (4) The rubric accepted a less-than-1-day-old, 0-star solo repo
  (cineforge-agent). (5) "Avoid giant codebases" was invisible to ranking,
  so 14 MB pyrite ranked first.
- What changed: On 2026-11-16 I added a required repo-in-use check (repo
  age, stars, or an outside contributor's merged PR) and a preferred
  small-codebase check (under 10 MB) to the rubric. In the strategy I
  changed the tie-break to oldest createdAt first and capped results at 2
  per repo. Still to update: the hardware queries need a label filter or
  their own top-N slice. A rescan under the patched strategy has not been
  run yet.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
