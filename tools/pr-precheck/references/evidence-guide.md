# Evidence guide: where evidence lives in a PR package

In an eval bundle a package has these parts, in order: the header
(`source`, `captured`, `calibration`), **Repo facts** (repo, latest
release, a "pull requests" line with the template sections and PR
asks, and a "contribution policy" line with the contributing rules and
any AI policy), **Issue** (title, opener and role, body),
**Thread highlights** (one line per comment: date, author, role in
parentheses, summary), **Plan context** (the repro evidence the plan
built on, then the accepted plan: its diagnosis, its scope statement,
its not-in-scope line, its files list, its test plan, and any
deferral or deviation note), and **Candidate PR** (Title, Description,
Commits, Diff, Test evidence). The bundle is the whole world; never
fetch.

In live mode the same parts come from these places. Repo facts: the
working copy's `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`
(Path Review keeps it there), and any `AI_POLICY.md`, plus the
course's PR criteria for Path Review. Issue and thread: the issue page
and its comments on GitHub, with each commenter's author-association
badge. Plan context: the student's `plan.md` (diagnosis, change,
not-in-scope list, test plan, and the `## Deviations` section) and
their posted repro comment. Candidate PR: the draft title and
description the student provides, `git log --oneline main..HEAD` for
commits, `git diff main...HEAD` for the diff, and the before/after
output and check results the draft carries.

## Plan fidelity (harness category: silent-drift)

- **Where it lives:** the plan's boundary is spread over four places
  in the plan context; read all of them. The scope statement ("Scope,
  one bounded change:", "Plan:"), the not-in-scope line ("Not in
  scope:"), the files list ("Files:"), and every deliverable the plan
  names (a doc update, a warning, a test, a changelog entry). A
  deferral or deviation note lives in the plan ("Deliberately
  deferred", "Known limit", "Honest limit", a `## Deviations`
  section) and is often restated in the description. What actually
  changed lives in the diff: each `--- a/<path>` header names a file,
  and each `@@` hunk is one change to read. The description's
  fidelity claims live in its sentences that start "Implements",
  "Per the plan", "exactly", "in full", "no other changes", "no
  functional changes outside", and in any list of what was "added" or
  "included".
- **What good looks like:** every changed file is in the plan's files
  list or is a test, fixture, or repo-asked changelog entry for the
  fix, and every hunk does what the plan's scope statement says and
  nothing else. Every deliverable the plan names is in the diff, or
  the plan or description says plainly that it was deferred and why.
  An honest deviation re-ties a mismatch: "the plan's deferral note
  covers the second edge case; this PR does not handle it, and here is
  why" is a ready PR's sentence.
- **What silent drift looks like:** more than the plan, with no note.
  A hunk in a file the plan never names, a refactor of a helper the
  fix does not need ("simplify helpers"), a new config field or flag,
  a rename pass, reworded help text, or extra commits whose titles
  name such work. The commit list is a quick tell: a commit titled
  "feat: add X option" or "refactor: tidy naming" on a bug-fix PR
  points at the hunk to find. Less than the plan, with no note: a
  promised doc update or test absent from the diff. And a description
  that claims fidelity the diff contradicts ("implements the plan
  exactly" over an extra hunk, "the docs now cover this" with no docs
  file in the diff) is drift
  too, not a comms problem: read the claim, then find the hunk that
  proves or disproves it.

## Test evidence (harness category: not-tested)

- **Where it lives:** the PR's "Test evidence" section and any
  Before/After or "Validation Steps" blocks in the description. Read
  it next to the plan's "Test plan" (what will be re-run and what is
  expected after) and the plan's "Repro evidence" (the exact failing
  input: the command, the file, the request, the key sequence). The
  repo's own checks are named in the repo-facts block ("pass the test
  suite", "all code checks passed (pre-commit)", a list of format,
  lint, and test commands) and in the plan's test plan (a named test
  command, "the suite on the touched package"). Live, the
  checks are the five CI jobs in `docs/CONTRIBUTING.md`.
- **What decisive looks like:** the plan's failing input is re-run on
  the branch and the after-result is shown or stated as an
  observation: the printed value, "exit 0", the expected output line,
  a table with the missing row now present, the error text that now
  appears. The before is
  shown next to it or cited from the repro evidence. When the plan
  names two failure modes ("repro 1" and "repro 2"), both are re-run.
  Each named suite or check is reported with an outcome ("passes (614
  tests)", "lint and typecheck clean").
- **What not-tested looks like:** "tested locally, works now";
  "verified working on my machine for a week"; "all tests pass" with
  no command; "no further testing seemed necessary". Evidence on the
  wrong path: the repro needs two files but the evidence runs one;
  the repro sends one kind of request but the evidence sends another
  and checks an unrelated property; the evidence runs the repro's
  control case, the one that never broke. A new unit test whose
  assertions cover the unchanged path does not prove the fix either.
  Compare the input in the evidence against the input in the repro,
  word by word.

## Diff quality (harness category: unreviewable)

- **Where it lives:** the unified diff (every `-` and `+` line, hunk
  by hunk) and the commit list. Live: `git diff main...HEAD` and
  `git log --oneline main..HEAD`.
- **What reviewable looks like:** a reviewer sees the fix and only
  the fix. Every added line is the change, its test, its explaining
  comment, or a repo-asked entry. Lines that are removed and re-added
  unchanged next to a real edit of the same statement (a multi-line
  call where one argument changes, a `match` that gains one arm) are
  how diff tools show an edit; they are not churn.
- **The debris tells:** an added `print("DEBUG ...")`, a print of a
  variable, or a debug log; a commented-out line of code (`# x = ...`,
  `// println!(...)`); a kept "first attempt" in comments; a function
  nobody calls (an `_unused` helper, a probe under a "dead code"
  allowance); a TODO note on a line the fix does not touch; whole
  blocks or hunks re-printed byte-for-byte; an import
  list restructured, or a struct literal joined onto one line, with no
  change in meaning. Commit messages like "wip", "fix", "fmt +
  cleanup", or "misc cleanups while debugging" say debris is likely;
  confirm it in the diff.

## Standards and comms (harness category: standards-wall)

- **Where it lives:** the repo-facts "pull requests" line names the
  template's sections and checklist items and any PR asks (issue-link
  form, changelog or whatsnew entry, title convention, before/after
  for UI changes). The "contribution policy" line names the
  contributing guide's asks and the AI policy text. The PR honors them
  in its title, its description (section labels or their content),
  and its diff (a changelog or whatsnew file). Maintainer direction
  lives in the thread highlights: the role in parentheses says who is
  speaking. Live: `.github/PULL_REQUEST_TEMPLATE.md`,
  `docs/CONTRIBUTING.md`, any `AI_POLICY.md`, the course's requirement
  of an AI-use disclosure in every Path Review PR, and the issue's
  comments with author-association badges.
- **What compliant looks like:** each template section is present
  with content about this PR, in any order and under any label that
  carries it ("Type of changes: Bug fix", "Problem:" / "Solution:",
  "fixes #123"). Each checklist item is answered, "n/a" with a reason
  included. An asked-for changelog or whatsnew entry is in the diff.
  Where the policy requires AI disclosure (in pull requests, or "in
  any form", sometimes naming the tool and the extent), the
  description says which tool was used and how much; where the repo
  states no AI policy, no disclosure is needed.
- **What a wall looks like:** a required checklist absent, no
  "closes #N" where the template asks for one, a required whatsnew or
  changelog entry missing from the diff, or a strict AI policy met
  with no disclosure. These hold a PR however good the fix is.
  (Whether the description's claims match the diff is plan fidelity,
  above.)
