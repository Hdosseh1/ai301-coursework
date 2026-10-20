# Procedure: how this tool grades a PR package

Four stages, in order. Do not grade anything until the read order is
finished and the notes it asks for are written down; the checks run
against those notes, not against a first impression of the pull
request. In live mode, read `scope.md` first as SKILL.md directs, then
start here.

## Read order

Read the package in this order, and write the note named at each step
before moving on.

1. **Repo facts.** Copy the "pull requests" line and list every ask
   in it as its own numbered item (each template section, each
   checklist item, the issue-link form, a changelog or whatsnew entry,
   a title convention, tests). Then copy the "contribution policy"
   line: add its PR asks to the same list (tests, named check
   commands such as a format, lint, or test command), and note which
   of these its AI policy is: no AI policy stated; permissive with no
   disclosure ask; asks only that contributors understand or review
   their work; asks that text be written by humans in their own
   words; requires disclosure in pull requests or "in any form" (and
   whether it asks for the tool and the extent). Mark asks the
   package cannot show (a CLA, a vouch flow) as "not visible".
2. **Issue.** One line for the reported behavior and one for the
   expected behavior, in the issue's own terms.
3. **Thread highlights.** For each line, note the role. List every
   comment by an OWNER, MEMBER, COLLABORATOR, or CONTRIBUTOR that
   gives direction on the cause, the fix, or the design, quoting it.
   If there are none, write "no maintainer direction".
4. **Plan context.** Write five lists:
   - *Repro*: the exact failing input or condition from the repro
     evidence (command, file, request, key sequence) and its recorded
     failing result.
   - *Scope*: what the plan says it will change, and where.
   - *Excluded*: everything in its not-in-scope line.
   - *Deliverables*: every file in its files list and every named
     deliverable (each code change, a doc update, a warning, each
     named test), numbered.
   - *Test plan*: each expected observation, numbered, marked "bug
     repro" if it re-runs a failure of the reported bug (one per
     failure mode the plan names), "control" or "secondary"
     otherwise; plus each named suite or check command.
   Also copy any deferral, known-limit, or deviation note word for
   word.
5. **Diff.** Read the diff before the description. For each `--- a/`
   file header, write the path. For each `@@` hunk, write one line:
   what it changes in meaning ("adds a capacity check before the
   copy", "renames three members", "re-prints the same lines", "adds a
   new flag"), and whether it contains any debris tell from the
   evidence guide's diff-quality family, quoting the line.
6. **Commits.** List each commit message.
7. **Test evidence.** For each run shown or stated, write "input ->
   observed result", and whether it is before or after. Then list
   each suite or check command reported run with its stated outcome.
8. **Title and description.** Read them last. Quote every claim about
   what the PR changes, includes, or leaves out (anything "added",
   "included", "now documents", "tests cover"), every fidelity claim
   ("exactly", "in full", "no other changes"), any disclosed
   limitation or deferral, any AI statement, and each section label or
   answered checklist item.

The diff is read before the description on purpose. A description is
written to be convincing, and a confident "implements the plan
exactly, no changes beyond it" read first frames everything after it.
With each hunk already noted from the diff alone, every claim in the
description has to match notes it did not get to shape. The plan is
read before the diff for the same reason in the other direction: the
boundary is written down before any hunk can argue it was needed.

## Evidence gathering

One gathering move per evidence family, using the locations in
`references/evidence-guide.md`.

- **Plan fidelity** (for `plan-fidelity-diff`): pair the diff with the
  plan. For each hunk note from step 5, write the item in the step-4
  Scope or Deliverables list it belongs to, or "test", "explaining
  comment", "repo-asked entry", "formatting only", or "extra" with
  what kind (refactor, new option or flag or config field, rename,
  unrelated message or help text, file or path the plan excludes or
  never names). Then for each numbered deliverable from step 4, write
  "in diff" with the hunk, "recorded deferral" with the note quoted,
  or "missing".
- **Description claims** (for `description-matches-diff`): for each
  claim quoted in step 8, write the hunk that makes it true, or "not
  in diff", or, for a fidelity claim, the "extra" hunks from the
  plan-fidelity pairing that contradict it.
- **Repro re-run** (for `repro-rerun-decisive`): for each test-plan
  item marked "bug repro" in step 4, find the step-7 run whose input
  matches the repro's failing input, word by word. Record "re-run:
  <quoted observed result>", "different input: <quoted input>", or
  "not re-run". For each control or secondary item, record whether it
  is stated as checked or pinned by a test in the diff.
- **Repo checks** (for `repo-checks-run`): list every suite or check
  command from step 1 and step 4, and next to each, the step-7 outcome
  or "not reported".
- **Debris** (for `diff-clean`): collect every debris line from step 5,
  quoted, and every hunk noted as "formatting only" or "re-prints the
  same lines". For a re-printed line, check whether it belongs to a
  statement the fix edits (the same call, match, or block whose
  content changes); if so, strike it from the list. Quote any commit
  message from step 6 that names debugging or cleanup.
- **Standards and comms** (for `template-asks-met`,
  `ai-disclosure-met`, and `thread-direction-engaged`): for each
  numbered ask from step 1, write where the PR meets it (the section
  label or content, the checklist answer, the hunk adding the entry,
  the issue reference) or "ignored"; leave "not visible" asks as they
  are. Write the description's AI statement, or "no AI statement".
  For each piece of maintainer direction from step 3, quote the
  description's sentence that answers it, or "not addressed".

In live mode the same moves apply to the live sources the evidence
guide names: `plan.md` and its `## Deviations` section for step 4,
`git diff main...HEAD` for step 5, `git log --oneline main..HEAD` for
step 6, the draft's evidence for step 7, the draft title and
description for step 8, and the repo's PR template and
`docs/CONTRIBUTING.md` for step 1. After the checks are graded, read
`voice-guide.md` and list every rule the draft title or description
breaks, quoting the rule and the sentence.

## Check execution

Run the checks in the rubric's table order, each against the notes
above. Grade every check, even after one has failed: the output lists
them all.

1. `plan-fidelity-diff`: fail if any hunk is marked "extra" (quote
   the first one by file and what it does) or any deliverable is
   "missing" (quote it). A "recorded deferral" is not missing.
   Otherwise pass.
2. `description-matches-diff`: fail if any claim is "not in diff" or
   any fidelity claim is contradicted by an "extra" hunk; quote the
   claim. A disclosed limitation is never a fail. Otherwise pass.
3. `repro-rerun-decisive`: fail if any "bug repro" item is "not
   re-run" or "different input"; quote the evidence line, or write
   "no run of <repro input>". Fail if the only evidence is a claim
   with no observation. Otherwise pass, provided the observed result
   matches the expected-after.
4. `repo-checks-run`: fail if any named command is "not reported";
   name it. Pass if every one has an outcome, or none was named.
5. `diff-clean`: fail if the debris list is non-empty after the
   strike-out; quote the first debris line. Otherwise pass.
6. `template-asks-met`: fail if any ask is "ignored"; name it.
   Otherwise pass.
7. `ai-disclosure-met`: apply the policy note from step 1. Only a
   policy that requires disclosure in pull requests or "in any form"
   can fail this check, and it does when the gathering recorded "no
   AI statement" (or the statement omits a tool or extent the policy
   asks for). In live mode, the rubric's verdict rule sets Path
   Review's policy to "requires disclosure in pull requests".
8. `thread-direction-engaged` and `commits-describe-change`: grade
   from the comms notes and the step-6 commit list. These are
   preferred.

Rules for every check:

- Each grade names the quote or fact that decided it in its evidence
  line. If no quote or fact can be given, the check has not been run
  yet.
- Grade from the notes. Go back to the package only to copy a quote
  exactly; do not re-read the whole package between checks to form a
  new overall impression.
- When the package has nothing in a place a check reads (no thread
  comments, no AI policy, no template, no named check commands), use
  what the pass condition says about that case; that is not
  `unclear`. Use `unclear` only when the part a check reads is absent
  from the package altogether (no plan context, no diff).
- Length, headings, tone, and confidence are never evidence. A check
  passes or fails on its stated condition even when the pull request
  as a whole feels stronger or weaker; note the tension in the
  summary.

## Verdict assembly

1. List the seven required checks with their grades.
2. Apply the rubric's verdict rule: if every required check is
   `pass`, the verdict is `accept`. If any required check is `fail`
   or `unclear`, the verdict is `reject`; `unclear` counts as `fail`.
   The two preferred checks are left out of this step whatever their
   grades.
3. Name the deciding check: for a reject, the first failing required
   check in rubric table order. Quote the package line that decided
   it. If more than one required check failed, name the others after
   it, in table order, each with its quote.
4. Write the summary before the JSON: the verdict and the deciding
   check first, then one line per check in table order. In live mode,
   add the voice-guide notes, then any procedure gaps.
5. Emit the JSON block last, with every check from the rubric
   (required and preferred) in table order, each with its grade and
   one line of evidence, and the verdict from step 2. Nothing follows
   the block.

If this procedure does not say what to do in some case, say so in the
summary under "Procedure gaps", name the missing step, and do not
invent one.
