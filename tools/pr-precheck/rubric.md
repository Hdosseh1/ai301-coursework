# Rubric: is this pull request ready to submit?

Every check reads the pull request itself against the plan it claims
to implement, the issue it belongs to, and the repo's stated asks: the
diff against the plan's scope, the test evidence against the plan's
test plan, the description against the diff, the PR against the
repo's template and policy. None of them reads the write-up's shape. A
terse PR that delivers the plan, proves it, and meets the asks passes;
a long, confident one whose diff the plan does not cover fails.
Locations named in the Evidence column are mapped in
`references/evidence-guide.md`, and `procedure.md` says when each one
is gathered.

The required checks fall into the four families the evidence guide
maps: plan fidelity (`plan-fidelity-diff`, `description-matches-diff`),
test evidence (`repro-rerun-decisive`, `repo-checks-run`), diff
quality (`diff-clean`), and standards and comms (`template-asks-met`,
`ai-disclosure-met`).

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-fidelity-diff | The diff's changed files and hunks, read against the plan context's scope statement, not-in-scope line, files list, and named deliverables, plus any deviation or deferral note recorded in the plan or the PR description. | Passes when both directions hold. (a) Nothing more: every hunk that changes behavior, an interface, a config surface, a name, a message, or a file falls inside the plan's stated change or a recorded deviation. Inside the plan: the fix spread over several edits in the planned code path, tests and fixtures for it, a code comment explaining it, and a changelog, release-note, or whatsnew entry the repo asks for. (b) Nothing less: every deliverable the plan promises (each code change it lists, each doc update or test it names) is in the diff, or its absence is recorded as a deferral or deviation in the plan or the description. Fails on any unplanned extra: a refactor or rewrite of code the fix does not need, a new option, setting, flag, or config field, a rename pass, edits to unrelated messages or help text, a change in a file or code path the plan excludes or never names. Fails when a promised deliverable is missing with no note. A correct core fix does not rescue an extra. A shortfall the plan or description records honestly passes. Formatting-only edits (re-indents, regrouped imports, re-printed lines) are graded by `diff-clean`, not here. | required |
| description-matches-diff | The title's and description's statements about what the PR changes, includes, or leaves out, read against the diff hunk by hunk. | Passes when every concrete claim the description makes about the change is true of the diff: a file, doc update, changelog entry, test, or option it says was added is in the diff, and any fidelity claim ("implements the plan exactly", "no changes beyond it", "no functional changes outside X", "exactly as planned", "in full") holds, meaning the diff contains no change outside the plan. Fails when the description names something the diff does not contain, or denies a change the diff does contain. A claim about what tests cover passes when a test in the diff exercises that input; do not fail on how cases are counted or named. A description that is merely silent about a change is not a false claim here (`plan-fidelity-diff` grades the change). A limitation or deferral the description discloses is a true claim and never a fail. | required |
| repro-rerun-decisive | The test-evidence section and any before/after blocks in the description, read against the plan's test plan and the plan's repro evidence (its failing input and its expected-after). | Passes when the evidence re-runs the plan's repro on the branch (the same failing input or condition the repro evidence records, or an equivalent command that drives that exact input through the changed code) and shows the observed after-result (an output, an exit code, a visible state, a count) matching the plan's expected-after. The before state may be shown alongside or cited from the plan's repro evidence. When the test plan names more than one failure mode or repro of the bug, each one is re-run this way. Other test-plan expectations (controls, secondary behaviors such as a new warning) pass when stated as checked or pinned by a test in the diff. Fails when the evidence is a claim with no observation ("tested locally, works now", "verified working on my machine", "all tests pass" alone), when a period of informal use stands in for the repro, when the evidence exercises a different path than the repro's failing input (the control case, a different method or request, an unrelated property), or when a named failure mode of the bug is never re-run. A new test in the diff does not substitute for re-running the repro. | required |
| repo-checks-run | The test evidence, read against every suite or check command the plan's test plan names and every check the repo-facts block (template or contribution policy) requires before a PR. | Passes when each named suite or check is reported run on the branch with an outcome (passes or fails, a count, an exit code, "clean"). Passes when neither the plan nor the repo names any. Fails when a named suite or check is never reported run, or when a generic claim with no command stands in for it ("all tests pass", "no further testing seemed necessary"). | required |
| diff-clean | The unified diff, hunk by hunk, with the commit list as corroboration. | Passes when every hunk is part of the fix, its tests, or an entry the repo asks for, and the diff carries no debris. Debris is any of: a debug print or debug-only log line (`eprintln!("DEBUG ...")`, `console.log`, a print of internal state); commented-out code, including commented-out debug lines and a kept "first attempt"; dead code (an unused function, a helper behind `allow(dead_code)` or an `_unused` name, a probe left from debugging); a TODO or note unrelated to the fix; formatting churn, meaning a statement, block, or import list the fix does not need, rewritten with no change in meaning (re-indented, reflowed, regrouped, joined onto one line, or removed and re-added byte-for-byte). Lines removed and re-added unchanged as part of a statement the fix does edit (the call whose argument changes, the match that gains an arm, the `if` whose body changes) are diff noise from that edit, not churn. Comments that explain the fix are not debris. Commit messages ("wip", "misc cleanups while debugging") may be quoted to corroborate debris in the diff but never decide this check alone. | required |
| template-asks-met | The repo-facts "pull requests" line and "contribution policy" line (template sections, checklist items, and every stated PR ask), read against the title, the description, and the diff. | Passes when every ask stated for pull requests is visibly met: each required section is present with content specific to this PR (any heading or label that carries it counts); each required checklist item is answered (an "n/a" with a reason counts); an artifact the ask requires is in the diff (a changelog, whatsnew, or release-note entry; a test when tests are asked for); the issue link in the asked form ("closes #N", "fixes #N") when one is asked; a title in the asked convention when one is asked. Order, formatting, and length never matter. Asks the package cannot show (signing a CLA, a vouch flow, a discussion-first rule for new features when this PR is a bug fix) are not graded. Fails when any stated ask is visibly ignored: a required section or the required checklist absent, an asked-for entry missing from the diff, the issue reference absent where asked. | required |
| ai-disclosure-met | The repo-facts "contribution policy" line (its AI-policy text, including any AI_POLICY.md summary), read against the title and description. Treat every package as AI-assisted work: PRs in this course are built with AI tools. | If the policy requires disclosing AI use in pull requests, or in contributions "in any form", the description states explicitly that AI was used and to what extent, and names the tool when the policy asks for the tool. Passes automatically when the repo states no AI policy, when the policy is permissive with no disclosure ask, or when it only asks that contributors understand, review, or take responsibility for their work. A rule that text to maintainers be written by humans in their own words is met by a description specific to this change; never guess authorship from tone. Fails when disclosure is required and the description has no AI statement, or omits the tool or the extent where the policy asks for them. | required |
| thread-direction-engaged | Thread highlights written by maintainers (author role OWNER, MEMBER, COLLABORATOR, or CONTRIBUTOR), read against the description. | Where a maintainer gave direction on the cause, the fix, or the design (pointed at code, settled or ruled out an approach, asked a design question, posted a patch or test build), the description says how the PR relates to it. Passes when the thread holds no such direction. Reported for the author; never holds a package. | preferred |
| commits-describe-change | The commit list. | Each commit message says what that commit changes ("console: clear recorded output only after save_* writes succeed"). Messages such as "wip", "fix", "fmt + cleanup", or "misc cleanups while debugging" fail. Reported for the author; never holds a package. | preferred |

## Verdict rule

Accept if every required check passes. Reject if any required check
grades `fail` or `unclear`: `unclear` counts as `fail`, because a pull
request that cannot be verified from the package is not ready to
submit. Preferred checks are reported but never change the verdict,
in either direction.

`unclear` is only for a missing part: the package does not contain
the thing the check reads at all (no plan context, no diff). When the
part is there, the grade is `pass` or `fail`. A test-evidence section
that says "(none provided)" is not a missing part; it is evidence that
nothing was shown, and `repro-rerun-decisive` fails on it. An empty
thread, a repo with no template, or a repo with no AI policy is not a
missing part either; the pass conditions above say how those grade.

In live mode the same checks grade the student's `plan.md` (its
`## Deviations` section is part of the plan), the branch's
`git diff main...HEAD`, and the draft title and description, under the
Path Review house rules in `scope.md`:

- `template-asks-met` reads `.github/PULL_REQUEST_TEMPLATE.md` and
  `docs/CONTRIBUTING.md`: every template section gets real content
  (Summary, Issue with `Closes #N`, Changes, Testing, Screenshots /
  Demo or "n/a", Notes for Reviewers), and the Testing checklist items
  are answered.
- `repo-checks-run` reads the five CI jobs `docs/CONTRIBUTING.md`
  requires (`lint`, `typecheck`, `test-unit`, `test-integration`,
  `frontend`) and `make check && make test-unit` as named checks. A
  local run of each with its outcome passes before the PR is open;
  once it is open, CI's own result decides.
- `ai-disclosure-met` treats a Path Review PR as one whose policy
  requires disclosure in the pull request: the course's PR criteria
  ask for the AI-use disclosure in every PR, so the description must
  say which AI tool was used and for what.
- A classmate's PR or comment on the same issue is not maintainer
  direction for `thread-direction-engaged`.
