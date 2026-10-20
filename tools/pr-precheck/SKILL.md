---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer one question about one PR package: **is this pull request
ready to submit?** A PR package is a candidate pull request (its
title, description, commit list, unified diff, and test evidence),
read against two things: the accepted plan it claims to implement
(that plan's scope, its not-in-scope line, its files list, its test
plan, and any deviation or deferral note) and the issue that plan
belongs to (the issue body, the thread, and the repo's stated asks for
pull requests and its contribution policy).

Grade exactly one package per run. Do not answer any other question:
not whether the fix is clever, not whether the issue was worth fixing,
not whether you would have planned it differently. The plan was
already accepted; you grade whether this PR delivers that plan,
proves it, is reviewable, and meets the repo's stated asks. Never
answer from gut feel: execute the components in this directory.

## Inputs and modes

There are two modes. Decide which one you are in before reading
anything else: if you were handed an eval package bundle (a markdown
file headed `# Eval package:`), you are in eval mode; otherwise you
are in live mode.

**Live mode** grades the student's own pull request before it is
opened. Gather these inputs, each from the named source:

- **The plan**: the student's `plan.md`, including its
  `## Deviations` section (the deviation notes recorded after the
  build). A house-chain student reads the house plan the instructor
  routed them to and the house repro pack instead; the same checks
  grade the same things there.
- **The repro evidence the plan built on**: the student's posted
  repro comment on the issue, or the house repro pack.
- **The diff**: everything the branch changes relative to the repo's
  default branch. Produce it by running `git diff main...HEAD` (three
  dots) from the student's working copy with the fix branch checked
  out, and take the commit list from `git log --oneline main..HEAD`.
  Do not grade uncommitted work: if `git status` shows uncommitted
  changes, report them and grade only what is committed.
- **The draft PR title and description**: the draft text the student
  gives you (pasted, or a file such as `pr-draft.md` they name). If
  there is no draft, stop and ask for it; do not write one and then
  grade it.
- **The test evidence**: the before/after output and check results in
  the draft description or in captured output the student points to.
  Evidence that is not in the draft does not exist for a reviewer, so
  it does not exist for you either.
- **The issue side**: the issue body and its comments, the repo's PR
  template (`.github/PULL_REQUEST_TEMPLATE.md`), its contributing docs
  (in Path Review, `docs/CONTRIBUTING.md`), and any AI policy file.
  Read them from the working copy where they exist and from GitHub
  (the `gh` CLI, the GitHub API, or the web) for the issue and its
  comments.

**Eval mode** grades a frozen package bundle. The bundle is the whole
world: every fact comes from the bundle text. Fetch nothing, run
nothing, read no other file. The live sources above map onto the
bundle's sections: repo facts, issue, thread highlights, plan context,
and the candidate PR's title, description, commits, diff, and test
evidence. Eval mode always grades a complete package: every check in
the rubric, the full verdict rule, no shortcuts.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else, before any input
is gathered.

1. Find its `Repo:` line. If it still carries a bracketed placeholder
   (for example `<ORG>/<PATH-REVIEW-REPO>`), stop without grading and
   tell the student: "Fill the `Repo:` line in `scope.md` with your
   section's Path Review repo (it is in the install step of this
   unit's activity page), then re-run." Never guess a scope.
2. Check that the pull request targets that repo: the working copy's
   upstream remote and the issue URL must both belong to it. If either
   points anywhere else, refuse to grade and say which input is out of
   scope.
3. Apply the house rules `scope.md` lists as facts about the
   environment: the PR comes from the student's fork on a
   `fix/<issue-number>-<slug>` branch, the template is always used,
   one PR per issue, and a classmate's PR on the same issue is not
   maintainer direction and does not block this one. The rubric's
   verdict rule says how its checks read those rules.

In eval mode, ignore `scope.md` entirely; do not open it.

## The voice seam (live mode only)

In live mode, after the rubric's checks are graded, read
`voice-guide.md`: the student's own rules for how they write upstream.
Hold the outgoing PR text, the draft title and the draft description,
against each rule. For every rule the draft breaks, report it in the
summary under a "Voice guide" heading: quote the rule's name, quote
the sentence in the draft that breaks it, and say what would fix it.

The voice guide never changes the verdict on its own, because no
check in `rubric.md` reads it: a draft can break a voice rule and
still be ready, and the student decides whether to revise. In eval
mode, ignore `voice-guide.md` entirely: voice is personal and carries
no gold labels; the universal communication checks live in the
rubric.

## Component reads

Three files decide every grade, and you read all three before
grading:

- `rubric.md` defines the checks (each row: the check's name, the
  evidence it reads, its pass condition, and its weight) and the
  verdict rule that combines the grades. It decides WHAT passes.
- `references/evidence-guide.md` maps where each evidence family
  lives, in a bundle and live, and what good looks like there. It
  decides WHERE to look.
- `procedure.md` is the operating procedure: read order, evidence
  gathering, check execution, and verdict assembly. Execute it as
  written, step by step. It decides HOW and in what order.

When the procedure is silent on a step you need, do not improvise
around the gap: grade what the written steps let you grade, and name
the missing step in the summary under "Procedure gaps" so the student
can fill it. When a check's pass condition and your impression
disagree, the pass condition wins.

Refusal rule: if `rubric.md` has no checks in its table or no verdict
rule, or if `procedure.md` has no steps under its headings, stop and
say so ("This tool cannot grade: `rubric.md` has no checks" or
"`procedure.md` has no steps"). Do not invent checks or steps to fill
the hole; an empty tool's verdict would look like judgment and be
noise.

## Verdict and output

The verdict space is binary: `accept` means ready to submit (open the
PR); `reject` means hold (fix first). There is no third verdict, no
"accept with reservations", and no score. Reservations go in a check's
evidence line, never in the verdict.

Before the JSON block, give a short readable summary as the procedure's
verdict assembly directs: one line per check, the deciding check
first for a reject, then (live mode only) the voice-guide notes and
any procedure gaps. Then end your reply with this fenced JSON block,
filled in. It must be valid JSON, it must be the last thing in the
reply, and nothing may follow it. In eval mode, `item` is the bundle
id; in live mode it is the PR's URL, or the branch name when the PR is
not opened yet. List every check in the rubric, required and preferred,
in table order.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or
  quote from the package that decided it, in its evidence line. "Looks
  fine" and "seems complete" are not evidence; a file name, a hunk, a
  quoted sentence, or an output line is.
- **Grade the thing, not the polish.** A terse PR that delivers the
  plan and proves it can be ready; a long, confident, beautifully
  formatted one can be hiding drift. Read the diff against the plan,
  the evidence against the test plan, and the description against the
  diff. Length, headings, tone, and enthusiasm are never evidence.
- **The rubric decides, not the run.** If a check passes by its stated
  condition but feels wrong, it passes; note the tension in the
  summary. If it fails by its stated condition but feels harsh, it
  fails. The fix belongs in the rubric, not in this run.
- **The procedure decides how, not the run.** Follow `procedure.md`
  in order and report its gaps instead of silently inventing steps.
- **Unclear defaults to fail.** Treat `unclear` exactly as the
  rubric's verdict rule directs. Where the rule is silent, an
  unverifiable claim is a failing one: a PR you cannot verify from the
  package is not ready to submit.
