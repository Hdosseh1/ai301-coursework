# Contribution: [Issue Title]

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [2]
**Student:** Hayden Dosseh
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

**Unit 7: Ran my ship-gate and pyrite manifest on the real branch, and pr-precheck made me fix the Testing line**

My manifest lists three checks for pyrite, each with its source: the two affected test files,
`ruff check pyrite/`, and `ruff format --check pyrite/`. The gate runs them and then my
pr-precheck before `gh pr create` can go through. On 2026-11-17 all three checks passed on
`fix/task-list-priority-range`, but pr-precheck rejected the PR because the template's
`pytest tests/ extensions/ -n auto` box was unchecked with no result and no reason
(repo-checks-run and template-asks-met failed). The gate stopped the PR, and I fixed the Testing
line in my description before trying again.

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

**Claim comment:** https://github.com/pyrite-wiki/pyrite/issues/1559#issuecomment-6937090873

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

The `--priority` filter on `pyrite task list` has no range check, even though `task create` and
`task update` only accept priorities from 1 to 10. A typo like `--priority 99` is treated as a
real filter instead of being rejected.

**Expected Behavior**

`task list --priority` should refuse any value outside 1-10 with exit code 2 and a `USAGE_ERROR`
naming `--priority`, the same way `create` and `update` do. Values from 1 to 10 should filter as
they do now.

**Current Behavior**

`task list --priority 99`, `0`, and `-1` all exit 0 and print `"count": 0, "tasks": []`. That
reads as "no tasks have that priority" when the value isn't a valid priority at all.

**Affected Components**

The `--priority` option on `task_list` in `pyrite/cli/task_commands.py` (line 293), which is
missing the `min=1, max=10` that `create` and `update` declare. The tests in
`tests/test_task_cli_priority_range.py` and the `NOT_YET` list in
`tests/test_cli_numeric_options_declare_a_range.py`, which tracks this option as unbounded, also change.

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

- macOS 15.5 (arm64). My system `python3` is 3.9.6, too old for pyrite (3.11+).
- uv, which supplied Python 3.13.5 for the project's `.venv`.
- pyrite cloned from my fork (Hdosseh1/pyrite), on `dev` at `f66b3977` (`pyrite --version`: 0.26.3).
- Set up with the repo's own script, then the git hooks:
  `UV_PYTHON=3.13 scripts/setup-checkout.sh` and `.venv/bin/pre-commit install`.
  The issue's test command, `.venv/bin/pytest tests/test_cli_numeric_options_declare_a_range.py tests/test_task_cli_priority_range.py -q`,
  passed 52/52 on clean `dev`.

Challenges:
- **Python version.** The system Python is 3.9, so a plain venv would not work. Setting
  `UV_PYTHON=3.13` made the setup script build the venv on a uv-managed 3.13 without touching
  the system install.
- **Remotes.** I first cloned my fork as `origin`, but CONTRIBUTING.md ("From a fork") says the
  repo's scripts read `origin/dev`, so `origin` must be upstream. I renamed the remotes so `origin`
  is pyrite-wiki/pyrite and `fork` is my fork.
- **Fork clone race.** `gh repo fork --clone` failed the first time because the fork was not ready
  yet. Cloning again a moment later worked.
- **Not touching my own config.** I ran the repro with `PYRITE_CONFIG_DIR` pointed at a throwaway
  directory, and outside the checkout, whose repo-local `.pyrite/config.yaml` would otherwise
  register the project's own KB.

**Steps to Reproduce**

1. Make a throwaway directory and config: `D=$(mktemp -d) && cd "$D" && export PYRITE_CONFIG_DIR="$D/config"`
   (with the checkout's `.venv/bin` on `PATH`).
2. Create a task KB: `pyrite kb create -n zk -t task -p "$D/zk"`.
3. Create two tasks: `pyrite task create "Low priority task" -k zk --priority 3` and
   `pyrite task create "Urgent task" -k zk --priority 9`. Both exit 0.
4. Filter by out-of-range priorities: `pyrite task list -k zk --priority 99`, then `-1`, then `0`.
5. Control: `pyrite task list -k zk --priority 3` (in range), and `pyrite task create "x" -k zk --priority 99`
   (the same value on the sibling option).
6. Observed result: step 4 prints `"count": 0, "tasks": []` and exits 0 for all three values. The in-range
   filter finds its task (`"count": 1`), and `task create --priority 99` is refused with exit 2 and
   `USAGE_ERROR`.

**Reproduction Evidence**

- **Commit showing reproduction:** pending. The next commit, on a `fix/` branch in my fork, will add the
  `task list` cases to `tests/test_task_cli_priority_range.py` and show them failing before line 293 changes.
- **Screenshots/logs:**

  ```
  $ pyrite task list -k zk --priority 99
  {
    "count": 0,
    "tasks": [],
    "outcome": "done"
  }
  exit 0

  $ pyrite task list -k zk --priority 3
  {
    "count": 1,
    "tasks": [
      {
        "id": "low-priority-task",
        "title": "Low priority task",
        "status": "open",
        "assignee": "",
        "priority": 3,
        ...
  exit 0

  $ pyrite task create x -k zk --priority 99
  Error: Invalid value for '--priority': 99 is not in the range 1<=x<=10.
  {"outcome": "error", "error": "Invalid value for '--priority': 99 is not in the range 1<=x<=10.", "error_code": "USAGE_ERROR", "retryable": false, "suggestion": "Try 'pyrite task create --help' for help."}
  exit 2
  ```

  `--priority -1` and `--priority 0` printed the same `"count": 0` body with exit 0.
- **My findings:** The bug is real on today's `dev` (`f66b3977`), which is newer than the `e27d5e48` the
  issue was measured on. It is also in the v0.26.3 release: line 293 of `pyrite/cli/task_commands.py` is
  identical on `origin/main` (`250332d4`), `typer.Option(None, "--priority", help="Filter by priority")`,
  while `create` (line 186) and `update` (line 414) declare `min=1, max=10`. The out-of-range filter
  doesn't fail; it silently returns nothing, which reads as "no tasks have that priority" rather than
  "that is not a priority". repro-check accepted the claim and report together, with every required check
  passing and the control run present.

**Toolkit**

- Tool: repro-check, my Unit 2 skill, now running on the wild `scope.md` and the voice guide in its new
  home. I ran it in a fresh `claude -p` session over `claim_draft.md` and `repro-draft.md` with
  read-only `gh` access, after the voice guard had already passed the claim.
- What it produced: **accept**. env-recorded, steps-rerunnable, behavior-matches, honest-outcome,
  claim-specific and ai-disclosure all passed, and the preferred control-run check passed too. It checked
  my facts against the repo itself (line 293 on `main` and `dev`, `-k` as an alias of `--kb`, the
  CONTRIBUTING line about the trailer). No voice-guide rule was broken.
- Difficulties: it flagged three soft spots. I only described the `-1`/`0` outputs instead of pasting them.
  The steps assume `pyrite` is on `PATH` after `cd "$D"`. And the issue's "maintainer may differ" point
  about hand-written out-of-range priorities belongs in my plan or PR. Separately, a headless session
  can't approve `gh` calls, so it needed `--allowedTools`.
- What changed: I added the `PATH` note to the steps here. The other two go into the plan and the PR
  description.

---

## Solution Approach

**Analysis**

In `pyrite/cli/task_commands.py`, `task_list` declares `--priority` (around line 293) with no min
or max. So 0, -1, 11 and 100 all reach the query, match nothing, and the command prints
`"count": 0` and exits 0. `task create` and `task update` already refuse those values with exit 2
and `USAGE_ERROR`, so the list filter is the only one that doesn't.

**Proposed Solution**

I'll declare `min=1, max=10` on `--priority` and add "(1-10)" to its help text. In the same
commit, as that file requires, I'll remove the `("pyrite task list", "priority")` row from
`NOT_YET` in `tests/test_cli_numeric_options_declare_a_range.py` and lower `NOT_YET_PINNED_SIZE`
from 40 to 39. If another PR changes that line first and it conflicts, I'll merge `dev` into my
branch, keep `dev`'s `NOT_YET` list, remove my row from it, and set `NOT_YET_PINNED_SIZE` to the
list's new length. One side effect goes in the PR description: a priority outside 1-10 written
into a task file by hand can no longer be filtered for, and `qa validate` is where that file gets
reported. I'm not changing any other option, `wiki stubs --limit` (#1560), or how `create` and
`update` validate.

**Implementation Plan**

1. Add the range to `--priority` in `task_list`.
2. In the same commit, remove the `NOT_YET` row and lower `NOT_YET_PINNED_SIZE` to 39. If that
   conflicts with `dev`, keep `dev`'s list, remove my row, and set the size to the list's length.
3. In `tests/test_task_cli_priority_range.py`, add tests that `task list --priority` rejects 0, 11,
   -1 and 100 with exit 2 and `USAGE_ERROR`, plus boundary controls for 1 and 10.
4. Add a `changelog.d` fragment for #1559.
5. Run the manifest checks: the two test files, `ruff check`, and `ruff format --check`.
6. Add one sentence to the PR description saying a hand-written out-of-range priority can no
   longer be filtered for and `qa validate` reports it.

Deviations: none. The branch follows all six steps; the diff touches the four files the plan names
(`task_commands.py`, the two test files, and the changelog fragment).

**Toolkit**

- Tool: plan-check, my Unit 3 skill, run in a fresh headless `claude -p` session in my pyrite checkout over
  `plan.md` and `plan-comment.md`, with read-only `gh` and `git` commands allowed. Claude Code helped me
  draft the comment and add two points to the plan before the run.
- What it produced: **accept** on the first run. All seven required checks passed (cause-grounded,
  fixes-reported-behavior, one-bounded-change, stranger-can-start, observable-test,
  thread-direction-engaged, ai-policy-met), and both preferred checks (unknowns-named,
  comment-carries-plan) passed too. It matched each point of the maintainer's direction to a sentence
  in the comment.
- Difficulties: before the run, the plan left out two things the maintainer asked for: the `qa validate`
  note for the PR and how to handle a conflict on `NOT_YET_PINNED_SIZE`. During the run, the voice check
  was skipped because the session couldn't read `voice-guide.md`, which is a link to a file outside the
  skill folder. It also flagged that step 5 leaves out `scripts/test-affected --run`, that the comment
  never names `pyrite/cli/task_commands.py`, and that `rubric.md` still mentions house rules my scope
  file no longer has. None of those changed the verdict.
- What changed: I added the `qa validate` note and the conflict handling to the plan before grading.
  Still to update: copy the voice guide into the skill folder instead of linking it, add
  `scripts/test-affected --run` to my pre-push checks, and remove the stale house-rules sentence from
  the rubric.

---

## Testing Strategy

**New tests, before and after the fix.** The tests are in their own commit (`399ae7cc`), ahead of
the fix. I ran them with `task_commands.py` as it was in that commit, so without the fix, and the
four new out-of-range cases failed while everything else passed:

```
FAILED tests/test_task_cli_priority_range.py::test_list_rejects_priority_filter_out_of_range[0]
FAILED tests/test_task_cli_priority_range.py::test_list_rejects_priority_filter_out_of_range[11]
FAILED tests/test_task_cli_priority_range.py::test_list_rejects_priority_filter_out_of_range[-1]
FAILED tests/test_task_cli_priority_range.py::test_list_rejects_priority_filter_out_of_range[100]
========================= 4 failed, 16 passed in 2.47s =========================
```

Each failure is `assert 0 == 2` on the exit code. With the fix (`fe9fb006`), both files pass:

```
$ .venv/bin/pytest tests/test_cli_numeric_options_declare_a_range.py tests/test_task_cli_priority_range.py -q
============================== 58 passed in 2.51s ==============================
```

The 1 and 10 boundary cases are marked `@pytest.mark.control`, because filtering at a valid
boundary already worked.

**Lint and format.** `ruff check pyrite/` printed "All checks passed!" and `ruff format --check
pyrite/` printed "223 files already formatted". Those are the manifest's commands. The PR
template's wider version, over `pyrite/ tests/ extensions/`, is also clean (962 files).

**Reproduction rerun.** I reran the steps from my repro on the branch, with a throwaway KB and
tasks at priority 3 and 9. Before the fix, `--priority 99`, `-1` and `0` each printed
`"count": 0, "tasks": []` and exited 0. Now each one is refused:

```
$ pyrite task list -k zk --priority 99
Error: Invalid value for '--priority': 99 is not in the range 1<=x<=10.
{"outcome": "error", "error": "Invalid value for '--priority': 99 is not in the range 1<=x<=10.", "error_code": "USAGE_ERROR", "retryable": false, "suggestion": "Try 'pyrite task list --help' for help."}
exit 2
```

`-1` and `0` give the same error with their own value, also exit 2. The in-range control,
`--priority 3`, still returns the priority-3 task (`"count": 1`) with exit 0.

**Full suite.** `scripts/heavy_lock.py -- .venv/bin/pytest tests/ extensions/*/tests/ -n 4 -q`:
still running, result not in yet.

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

**2026-11-16**

I set up the checkout and reproduced the bug on `dev` (`f66b3977`). I drafted the fix during the
repro: it was one line, the `min=1, max=10` that `create` and `update` already have. I wrote the
plan afterward to match it, so the plan describes a change I had already tried rather than one I
designed first. plan-check accepted it. Then I branched `fix/task-list-priority-range`, committed
the tests first (`399ae7cc`) and the fix second, and recommitted the fix once to reword its
message (`fe9fb006`). I pushed the branch to my fork.

**2026-11-17**

My first `gh pr create` was stopped by my ship-gate. The manifest checks passed, but pr-precheck
rejected the description because the full-suite box was unchecked with no reason. The first
full-suite run, which Claude Code started, used the wrong command: plain
`pytest tests/ extensions/ -n auto`, without the machine-wide heavy lock that pyrite's AGENTS.md
requires. I had it stopped and restarted as
`scripts/heavy_lock.py -- .venv/bin/pytest tests/ extensions/*/tests/ -n 4 -q`. I also cut the
sentence in my PR draft that pointed to my repro comment on the issue, because Step 12 says the PR
description carries the proof.

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

- Typer passes `min`/`max` to Click, which rejects an out-of-range value while it parses the
  arguments, before the command body runs. That's why the error comes back as exit 2 and
  `USAGE_ERROR`, not from pyrite's own code.
- How a ratchet test works: `NOT_YET` plus a pinned size means fixing one option forces the list
  and the count to change in the same commit.
- Showing a test is red without the fix by running it against the file as it was in an earlier
  commit, plus pyrite's `control` marker for tests that are supposed to pass either way.
- pyrite's process: branching from `dev`, `changelog.d` fragments, the commit-msg hook that
  requires a `fix:` commit to touch `tests/`.

**Challenges Overcome**

- My system Python is 3.9 and pyrite needs 3.11+. Setting `UV_PYTHON=3.13` let the setup script
  build the venv on 3.13.
- I cloned my fork as `origin`, but pyrite's scripts read `origin/dev`. I renamed the remotes.
- My own ship-gate blocked my first PR attempt. I fixed the Testing line instead of going around
  the gate.

**What I'd Do Differently Next Time**

- Write the plan before touching code. This time the fix came first and the plan was written to
  match it.
- Read AGENTS.md's test command before starting any heavy run, and run the full suite before
  drafting the PR, not when the gate asks for it.
- Put the full suite, or `scripts/test-affected --run`, in my manifest so the gate checks it up
  front.

---

## Resources Used

- [pyrite-wiki/pyrite#1559](https://github.com/pyrite-wiki/pyrite/issues/1559): the issue named the
  exact line, the test file and fixture to extend, and the ratchet change.
- [CONTRIBUTING.md](https://github.com/pyrite-wiki/pyrite/blob/dev/CONTRIBUTING.md): "From a fork"
  for the remote setup, and "Branches, hooks and pull requests" for branching, changelog fragments
  and the AI policy.
- [AGENTS.md](https://github.com/pyrite-wiki/pyrite/blob/dev/AGENTS.md): the heavy-lock command
  for the full suite.
- Claude Code, which I used to evaluate the issue, plan, write the tests and the fix, run checks,
  and draft text. My own skills (issue-scout, repro-check, plan-check, pr-precheck) graded each
  stage.
