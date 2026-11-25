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
**Pull Request:** https://github.com/pyrite-wiki/pyrite/pull/1718

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

**Unit 6: Ran the voice-guard as a Claude Code hook and wrote a repo-memory file for pyrite**

The voice-guard checks every outgoing `gh` post against one voice guide, which my three skills
link to. Its log shows it passed my claim on #1559 and my `gh pr create`, and stopped one repro
post because the body-file path was wrong. Where it fell short: it only checks voice, so on
2026-11-16 it passed my repro report even though Step 12 says the report doesn't go on the issue,
and plan-check couldn't read the linked guide in a headless session. My repo-memory file
(`CLAUDE.local.md`) gave each session pyrite's conventions, the exact lines to change and the
ratchet rule, but it didn't record AGENTS.md's heavy-lock command, and the first full-suite run used
the wrong one; still to change: copy the guide into each skill folder and add the full-suite
command to the memory file.

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

- Tool: Claude Code, in a session on 2026-11-16. I had it read this story, my repo-memory file
  for pyrite (`CLAUDE.local.md`), and the `git diff` of my pyrite checkout, and draft the four
  answers above in one or two plain sentences each, leaving every other section alone.
- What it produced: the four answers, with the `--priority` option at
  `pyrite/cli/task_commands.py:293`, the two test files, and the `NOT_YET` list named as the
  affected components. I committed them as `a9c819f`.
- Difficulties: Affected Components came out at three sentences, not two. The answers were built
  from my uncommitted diff, so the fix was already sitting in my working tree when I wrote them.
  The changelog fragment wasn't in the diff yet, so it isn't listed. The commit also picked up two
  header edits I had made before the session: my name, and Contribution Number changed to `[2]`.
- What changed: I kept the three-sentence answer. Leaving out `pyrite/models/task.py`, which keeps
  hand-written out-of-range priorities, was deliberate, because that file doesn't change; it shows
  up instead as the `qa validate` side effect in my plan and PR.

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

Deviations: two. First, the full suite showed that an existing case in
`tests/test_task_list_priority.py` expected `task list --priority 0` to exit 0, and the plan didn't
name that file. I changed the case to expect exit 2 and `USAGE_ERROR`, in its own commit
(`ddbdcf3d`). So the diff touches five files: `task_commands.py`, the changelog fragment, the two
test files the plan names, and `test_task_list_priority.py`. Second, step 2's conflict handling
didn't hold. While the PR was open, `dev` removed `NOT_YET_PINNED_SIZE` entirely (#1614), so instead
of setting the size to the list's length, I followed the review: deleted my row from `dev`'s list
and kept no pinned size at all.

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

**Full suite.** `scripts/heavy_lock.py -- .venv/bin/pytest tests/ extensions/*/tests/ -n 4 -q`
on the fix commit: 27,110 passed, 310 skipped, 3 xfailed, 3 failed, in 40 minutes. Two failures
were mine: the json and rich runs of a case in `tests/test_task_list_priority.py` that expected
`--priority 0` to exit 0. After I changed that case (`ddbdcf3d`), the file passes (15 passed),
and the three affected test files pass together (73 passed). The third failure,
`test_session_hook_exports_the_url_through_the_env_file`, fails the same way on a clean checkout
of `dev` (`f66b3977`): `scripts/known_cloud_failures.py` raises a `TypeError` under my Mac's
system Python 3.9.6 and exits 0 under 3.13. I haven't rerun the heavy-lock full suite since the
test commit.

**After merging `dev`.** The review asked for `scripts/test-affected --run` after the merge. On the
merge commit (`e644126b`) it passed: 26,489 passed, 130 skipped, 20 xfailed, 0 failed, in 50
minutes. `dev`'s new ratchet check, `scripts/check_ratchets_only_shrink.py --range origin/dev HEAD`,
passed, and ruff stayed clean (1032 files). When I pushed, the pre-push hook ran nearly the whole
suite (`-m "not slow and not e2e and not experimental"`) and passed. With `.venv/bin` first on
`PATH`, that included the cloud-hook test that fails under my system Python 3.9.

**Toolkit**

- Tool: pytest and ruff, run through Claude Code in my pyrite checkout, plus my ship-gate. The
  gate's manifest reruns the two affected test files, `ruff check pyrite/` and
  `ruff format --check pyrite/` before `gh pr create` can run.
- What it produced: 4 failed and 16 passed without the fix, 58 passed with it, and ruff clean (223
  files in `pyrite/`, 962 across all three directories). The repro rerun refused 99, -1 and 0
  with exit 2. On 2026-11-17 the gate reported all three manifest checks "ok".
- Difficulties: my manifest only covers the two affected test files, so I had no full-suite result
  until pr-precheck flagged the missing one. The first full-suite run used the wrong command,
  without the heavy lock, and hadn't finished after 10 minutes.
- What changed: the full suite was restarted under `scripts/heavy_lock.py` with `-n 4`. It caught
  two failures in `tests/test_task_list_priority.py` that the manifest's two files missed, so I
  fixed that test case and added the file to the manifest's test row. Still to update: add the full
  suite or `scripts/test-affected --run` to my manifest so the gate checks it before the PR step.

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
description carries the proof. The full suite came back with 3 failures. Two were an existing case
in `tests/test_task_list_priority.py` that expected `--priority 0` to exit 0, so I changed it to
expect the error, in a separate `test:` commit. I ran the third on a clean checkout of `dev` and it
failed the same way there, because of my system Python 3.9, so it isn't from my branch. I added
the changed test file to my manifest.

**2026-11-19**

I installed the Unit 8 PR monitor and ran `monitor.py --state` on #1718. It showed review feedback
in and the branch conflicting with `dev`. The maintainer's automated review (posted 2026-11-17)
said the fix and tests were right, and the one blocking item was a conflict in the ratchet test:
merge `dev` (no rebase), keep my deleted row, and drop `NOT_YET_PINNED_SIZE`, which `dev` had
removed in #1614. It also asked for a `test-affected` result after the merge. A follow-up comment
on 2026-11-19 said that after the merge the PR is ready to land. By the time I merged, `dev` had
moved again and `task_commands.py` conflicted too, which the review didn't cover: `dev` had added
`--dispatchable`, `--parked/--unparked` and `--fields` right after `--priority`. I resolved both
files from `dev`'s version, putting back only my `--priority` line and deleting only my row, and
checked that the net change against `dev` was exactly that. I committed the merge (`e644126b`) and
started `test-affected`.

**2026-11-21**

`test-affected` passed on the merge after 50 minutes. That's too long for the gate's 1,500 s check
limit, so I added it to my manifest's Excluded table with a dated reason instead of making it
must-pass. The first push was stopped partway: because the push brought in 109 `dev` commits, the
pre-push hook ran nearly the whole suite, including the cloud-hook test that fails under my system
Python 3.9. I reran the push with `.venv/bin` first on `PATH`, so the hook ran on 3.13. It passed,
and the merge went up as `ddbdcf3d..e644126b` with no force-push. I updated the PR description
with `gh pr edit`: the merge and how I resolved it, the ratchet check, and the `test-affected`
result. GitHub then showed the PR as mergeable. Before I replied or marked it ready for review, it
came out of draft and markramm merged it at 09:12 UTC that day (merge commit `ccd366ca` on
`dev`), and #1559 closed with it.

**Toolkit**

- Tool: Claude Code in my pyrite checkout, on `fix/task-list-priority-range`. I used it to write
  the tests and the fix, make the commits, push to my fork, and run the checks and the PR command.
- What it produced: three commits: the new tests first (`399ae7cc`), then the fix (`fe9fb006`),
  then the change to an existing test case the fix broke (`ddbdcf3d`). After the review, a merge of
  `dev` (`e644126b`). Against `dev`, the branch touches five files (43 lines added, 5 removed):
  `task_commands.py`, the changelog fragment, and three test files.
- Difficulties: the fix came before the plan, so the order the course asks for got reversed. I
  recommitted the fix once to reword its message. Claude Code started the first full-suite run
  with plain `pytest -n auto` instead of the heavy-lock command in AGENTS.md. It hadn't read
  AGENTS.md yet and ran the command without checking it. The review was checked against an older
  `dev` (`309f934f`), and by my merge `dev` was at `70bb4fd1`, so `task_commands.py` conflicted with
  no instructions for it. And my Mac's system Python 3.9 would likely have failed a 40-minute
  pre-push run, so I stopped the first push after about 11 minutes and reran it with the venv on
  `PATH`.
- What changed: the full suite was rerun with the heavy-lock command. Next time I'll write and
  check the plan before writing any code, and look up the repo's own test command before starting
  a long run.

---

## Pull Request

**PR Link:** https://github.com/pyrite-wiki/pyrite/pull/1718 ("Declare a 1-10 range on task list
--priority", against `dev` from `Hdosseh1:fix/task-list-priority-range`: `399ae7cc`, `fe9fb006`,
`ddbdcf3d`, and the merge of `dev`, `e644126b`; opened as a draft, merged 2026-11-21 as `ccd366ca`)

**PR Description:** The summary explains that `task list --priority` accepted any integer, so an
out-of-range value printed `"count": 0` and exited 0, and that declaring `min=1, max=10` now
refuses it with exit 2 and `USAGE_ERROR`. It names the ratchet change (the `NOT_YET` row
removed), the changelog fragment, and `Fixes #1559`, and says one existing test changed: the
`--priority 0` case in `tests/test_task_list_priority.py` now expects the error, in its own
commit. Since 2026-11-21 it also says I merged `dev` and resolved the conflict as the review
describes, that `dev`'s ratchet check passes, and that `test-affected` passed after the merge
(26,489 passed, 0 failed). The Testing section shows the new tests failing without the fix (4 failed, 16
passed) and passing with it (58 passed, then 73 across the three affected files), ruff clean, the
reproduction rerun refusing 99, -1 and 0, and the full-suite result: 27,110 passed, 3 failed, two of
them mine and fixed by the test commit, one that fails the same way on clean `dev` because of my
system Python 3.9. The full-suite box stays unchecked with that explanation. The reviewer notes
cover the `qa validate` side effect, what I left alone (#1560, other options), and how I used
Claude Code.

**Maintainer Feedback:**

- 2026-11-17: an automated review from the maintainer's account (markramm), about 15 minutes
  after the PR opened. It found the fix and tests correct and the PR inside the "range-tightening
  CLI fix" class. Blocking: a merge conflict in `tests/test_cli_numeric_options_declare_a_range.py`,
  with exact steps (merge `dev`, keep my deleted row, drop `NOT_YET_PINNED_SIZE`). Non-blocking:
  run `scripts/test-affected --run` after the merge and put the result in the description.
- 2026-11-19: a follow-up comment: merge `dev`, resolve the conflict, address the blocking item,
  and then it is ready to land.
- 2026-11-21: how I addressed it. I merged `dev` with no force-push and resolved the ratchet file
  as the review says, plus a `task_commands.py` conflict the review didn't cover. I pushed the merge
  and put the ratchet check and `test-affected` results in the description.
- 2026-11-21: markramm merged the PR (merge commit `ccd366ca` on `dev`), and #1559 closed with
  it. I haven't posted a reply on the thread.

**Status:** Merged on 2026-11-21 by markramm, into `dev` as `ccd366ca`. It closed #1559.

**Toolkit**

- Tool: pr-precheck, my Unit 4 skill, run by my ship-gate in live mode (one Sonnet call) when
  `gh pr create` was about to run. It graded my branch and `pr_draft.md` against my plan and
  #1559. Claude Code helped me draft the description and ran the command.
- What it produced: on 2026-11-17, **reject**. The manifest checks passed, but two rubric checks
  failed: repo-checks-run (the template's `pytest tests/ extensions/ -n auto` box was unchecked and
  no full-suite or test-affected result was reported) and template-asks-met (that unchecked box
  had no reason). The gate stopped the PR before anything was posted.
- Difficulties: the only thing missing was the full-suite result, and the full suite takes a long
  time on my machine. Separately, my draft linked to the repro comment on the issue, which Step 12
  says the PR description should carry instead.
- What changed: I cut the sentence pointing to the repro comment, reran the full suite under the
  heavy lock, fixed the test case it caught, and rewrote the Testing line with the result. On the
  second try the gate's checks and pr-precheck passed, the voice-guard passed the description, and
  the draft PR opened as #1718. Before that, pyrite's own pre-push hook had blocked my push: my new
  test checked `"--priority"` in raw output instead of through `plain()`, which a repo guard test
  forbids. I fixed the line and folded it into the unpushed test commit. My manifest only runs
  named test files, so it missed that; adding `scripts/test-affected --run` would catch it.

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
