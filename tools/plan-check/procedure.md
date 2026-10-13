# Procedure: how this skill grades a plan package

Four stages, in order. Do not grade anything until the read order is
finished and the notes it asks for are written down; the checks run
against those notes, not against a first impression of the plan.

## Read order

Read the package in this order, and write the note named at each step
before moving on.

1. **Repo facts.** Copy the "contribution policy" line. Note which of
   these it is: no AI policy stated; permissive with no disclosure
   ask; asks only that contributors understand or review their work;
   asks for disclosure in pull requests only; asks that comments be in
   the contributor's own words; requires disclosure in issues,
   comments, or "in any form" (and whether it asks for the tool name).
2. **Issue.** Write one line for the reported behavior and one line
   for the expected behavior, in the issue's own terms.
3. **Thread highlights.** For each line, note the author's role. List
   every comment by an OWNER, MEMBER, COLLABORATOR, or CONTRIBUTOR
   that gives direction on the cause or the fix, quoting it. If there
   are none, write "no maintainer direction".
4. **Repro evidence.** List each numbered step and each control run as
   "what was run -> what was observed". Then write one line: what
   this evidence pins down (which condition flips the result, and
   where the failure is shown to occur).
5. **Candidate plan.** Only now read the plan. Pull out five things:
   the stated cause; every change it proposes (from the scope
   statement, the approach steps, and the files list together); where
   each change goes; the test plan's expected observations; any risk,
   unknown, or deferral.
6. **Candidate plan comment.** Read it last. Note what it says about
   the thread, what it says about AI use, and whether it matches the
   plan.

The evidence is read before the plan on purpose. A plan is written to
be convincing, and a confident diagnosis read first frames everything
after it. With the step-and-control list already written, the plan's
cause has to fit notes it did not get to shape. The same goes for the
thread: a cause the thread states confidently is still only a claim
until it is held against step 4's list.

## Evidence gathering

One gathering move per evidence family in the rubric, using the
locations in `references/evidence-guide.md`.

- **Diagnosis and grounding** (for `cause-grounded`): take the stated
  cause from step 5 and the step-and-control list from step 4. Write
  the cause as one sentence of the form "X at Y makes Z happen". If
  the plan has no sentence that can be put in that form, record "no
  cause stated".
- **Scope** (for `fixes-reported-behavior` and `one-bounded-change`):
  take the change list from step 5 and the expected-behavior line
  from step 2. Number the changes. Next to each, write "needed for
  the reported behavior" or "extra", and for each extra, what kind
  (migration, new option, refactor, other code path, CI, different
  issue). Tests, fixtures, a doc line for the changed behavior, and
  audit-only notes are not numbered as changes.
- **Executability** (for `stranger-can-start`): from step 5, record
  the named site (file, function, or code path) and the chosen
  approach as one line each. Record the verb of every approach step.
  If the site or the approach is given as a question or a choice,
  quote it.
- **Test plan** (for `observable-test`): from step 5, quote each
  expected observation and name the repro step it corresponds to. If
  the test plan names no observation, quote its whole text.
- **Honesty** (for `unknowns-named`): from step 5, quote each risk,
  unknown, or deferral reason, and any date promised in the plan or
  the comment.
- **Comms** (for `thread-direction-engaged`, `ai-policy-met`, and
  `comment-carries-plan`): take the maintainer-direction list from
  step 3, the policy note from step 1, and the comment notes from
  step 6. For each piece of direction, quote the sentence in the
  comment that answers it, or write "not addressed". For the policy,
  quote the comment's AI statement, or write "no AI statement".

In live mode the same moves apply, with the sources swapped as the
evidence guide says: `scope.md` is read first and the issue must be
in the scoped repo; the issue body, comments, and contributing docs
come from GitHub; the repro evidence is the student's posted repro
comment; the plan and comment are the student's drafts. After the
rubric's checks, read `voice-guide.md` and list any rule the draft
comment breaks, quoting the rule.

## Check execution

Run the checks in the rubric's table order, each against the notes
above. Grade every check, even after one has failed: the output
lists them all.

1. `cause-grounded`: go down the step-and-control list one line at a
   time. For each line ask: if the stated cause were true, could
   this result have been recorded? Write "fits", "silent" (the cause
   says nothing about it), or "contradicts". One "contradicts" is a
   fail; quote that line. All "fits" or "silent" is a pass. The
   plan's own sentence that the controls fit is a claim to test, not
   a result. "No cause stated" is a fail.
2. `fixes-reported-behavior`: imagine the change made exactly as
   written, then re-run the repro's failing step with the same
   command. If the expected behavior from step 2 would appear, pass.
   If the user would still have to do something differently, fail.
3. `one-bounded-change`: look at the numbered change list. Any item
   marked "extra" is a fail; quote it. None is a pass.
4. `stranger-can-start`: pass if a site and one chosen approach are
   both recorded. Fail if either is missing, is a question, or is a
   choice; quote it.
5. `observable-test`: pass if at least one quoted observation names
   a specific result. Fail if every line is a feeling, a generality,
   or the existing suite alone.
6. `thread-direction-engaged`: if step 3 says "no maintainer
   direction", pass. Otherwise pass only if each piece of direction
   has an answering sentence in the comment; "not addressed" on
   direction about the cause or fix is a fail.
7. `ai-policy-met`: apply the policy note from step 1. Only a policy
   that requires disclosure in issues, comments, or "in any form"
   can fail this check, and it does when the comment has no AI
   statement (or omits the tool where the policy asks for it).
8. `unknowns-named` and `comment-carries-plan`: grade from the
   honesty and comms notes. These are preferred.

Rules for every check:

- Each grade names the quote or fact that decided it. If no quote
  can be given, the check has not been run yet.
- Grade from the notes. Go back to the package only to copy a quote
  exactly; do not re-read the plan to form a new overall impression
  between checks.
- When the evidence for a check is absent because the package has
  nothing there (no thread comments, no AI policy, no control runs),
  use what the pass condition says about that case; it is not
  `unclear`. Use `unclear` only when the part the check reads is
  missing from the package altogether (for example, no plan comment).
- Length, headings, and tone are never evidence. A check passes or
  fails on its stated condition even when the plan as a whole feels
  stronger or weaker than that; note the tension in the summary.

## Verdict assembly

1. List the seven required checks with their grades.
2. Apply the rubric's verdict rule: if every one is `pass`, the
   verdict is `accept`. If any one is `fail` or `unclear`, the
   verdict is `reject`. `unclear` counts as `fail`. The two preferred
   checks are left out of this step whatever their grades.
3. Before the JSON, give a one-line summary per check. For a reject,
   name the deciding check or checks first and quote the line from
   the package that decided each. In live mode, add the voice-guide
   notes after the checks.
4. Emit the JSON block last, with every check from the rubric
   (required and preferred) in table order, each with its grade and
   one line of evidence, and the verdict from step 2. Nothing follows
   the block.

If this procedure does not say what to do in some case, say so in the
summary and name the missing step, and do not invent one.
