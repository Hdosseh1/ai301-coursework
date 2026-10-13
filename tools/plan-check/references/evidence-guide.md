# Evidence guide: where evidence lives in a plan package

In an eval bundle a package has seven parts, in order: the header
(`source`, `captured`), **Repo facts**, **Issue** (title, opener and
role, body), **Thread highlights** (one line per comment: date, author,
role in parentheses, summary), **Repro evidence** (environment,
numbered steps, artifacts, control runs, Expected, Actual),
**Candidate plan**, and **Candidate plan comment**. The bundle is the
whole world; never fetch.

In live mode the issue side comes from GitHub: the issue body and its
comments, the repo's `CONTRIBUTING.md` (in Path Review,
`docs/CONTRIBUTING.md`), and any `AI_POLICY.md`. The repro evidence is
the student's own posted repro comment on that issue. The candidate
side is the student's `plan.md` and draft plan comment. Read the
drafts as a maintainer on the thread will: only what the drafts
contain or quote counts.

## Diagnosis and grounding

- **Where it lives:** the plan's cause is its diagnosis line or
  section (often the first sentence: "Cause:", "Diagnosis:", or a
  "Background" paragraph), and it is restated in the plan comment.
  What that cause has to survive lives in the repro evidence: each
  numbered step with its output, each line under "Control" or
  "Control runs", any step that names where an error is raised
  (a `--debug` trace, a stack frame, a timing with one component
  removed), and the closing "Actual" paragraph. In live mode, the
  same parts of the student's posted repro comment.
- **What good looks like:** the cause is a mechanism at a place ("the
  fill length is computed by an unsigned subtraction at this site"),
  and every recorded run comes out the way that mechanism says: the
  failing run fails, and each control behaves as the evidence shows.
  Read each control as a test of the cause. If the plan blames
  component X and a control shows X working in the same build, or
  shows the damage already present before X runs, or shows the same
  cost with X out of the loop, the evidence has ruled X out, however
  confident the plan or the thread is about it. A plan that calls a
  recorded difference a "red herring" or "unrelated" without showing
  why has ignored evidence. An inferred cause that fits everything
  recorded is grounded; it does not need a proof.

## Scope

- **Where it lives:** the plan's scope statement ("In scope",
  "Scope", "Change", "Proposed changes"), its not-in-scope or
  deferral line, its files list, and every numbered approach step.
  Read all of them: extra work often appears only in the step list or
  the files list. The bound to measure against is the issue's
  reported behavior and its "Expected" line.
- **What good looks like:** one bounded change is a set of edits in
  the code path where the cause lives, each one needed for the
  reported behavior to stop, plus the tests that pin it. A stated
  deferral ("not in scope: the larger rework discussed in the
  thread") is a sign of a bounded plan. A drive-by rewrite looks
  like a list where the direct fix is one item among several: a
  library migration, a new option or setting, a module restructure,
  other constructs moved onto a new mechanism, a second bug fixed
  "while touching" the area, new CI jobs. Phrases that mark it:
  "rather than patch just", "fix the class rather than the
  instance", "while in there", "properly". The size of the write-up
  says nothing; count the changes, not the paragraphs.

## Executability

- **Where it lives:** the plan's files or areas list, the function
  and path names inside its approach steps, and the verbs of those
  steps.
- **What good looks like:** a stranger could start without asking
  the author anything, because the plan says where (a file, a
  function, or a named code path) and what will be done there (one
  approach, already chosen). A plan may leave the exact line open if
  it says so and says how it will be found. What cannot be started:
  steps whose verbs are "investigate", "profile", "look into",
  "explore", "figure out"; a location given as "somewhere" or as a
  question ("which layer?"); an approach left as "A or B, whichever
  is easier". Those are plans to make a plan.

## Test plan

- **Where it lives:** the plan's "Test plan" or "Test:" lines, read
  next to the repro evidence's numbered steps, its artifacts (the
  exact output, exit code, or state recorded for the failing run),
  and its "Expected" line.
- **What good looks like:** the test plan points back at a repro
  step or an equivalent command and says what will be seen there
  after the change: the output that should print, the exit code, the
  state of the screen, a count of clean runs. Controls re-run
  unchanged and a regression test added are good extras. A vague
  test plan names no observation: "should feel fast", "works
  afterwards", "nothing else should feel broken", or "run the full
  suite and make sure nothing regresses" with nothing stated about
  the fix itself.

## Honesty

- **Where it lives:** the plan's "Risk" or "Unknown" lines, any
  "I have not yet verified" or "open question" sentence, the
  deferral reasons in its scope statement, and what the plan comment
  flags for review. Certainty claims live in the diagnosis ("the
  root cause is", "clearly", "is simply") and in the comment's
  promises. In live mode, a mid-build deviation is recorded under
  the `## Deviations` heading of `plan.md`, saying what changed
  from the posted plan and why.
- **What good looks like:** what has been checked is stated as
  checked, and what has not is labeled ("I have verified three of
  the six tools and will check the rest before the PR"). A stated
  unknown inside a chosen approach is honest; a decision left
  unmade and presented as a step is not. False confidence is a
  certainty word over something the evidence contradicts or the
  author has not looked at. A deviation that exists only in the diff
  and not in `plan.md` is not recorded.

## Comms

- **Where it lives:** the plan comment, read against two things.
  First, the thread highlights: for each line, the role in
  parentheses says who is speaking. Maintainer direction is a
  comment by an OWNER, MEMBER, COLLABORATOR, or CONTRIBUTOR that
  points at the culprit code, proposes or settles an approach, posts
  a patch or test build, opens a fix elsewhere, or rules an approach
  out. Second, the repo-facts block: the "contribution policy" line
  carries the AI-policy text, and the "bug reports" line carries the
  template asks. In live mode: the issue's comments with their
  author-association badges, and the repo's contributing docs.
- **What good looks like:** a thread-aware comment says how the plan
  relates to the direction already given ("option 2 from the
  discussion here", "along the lines already agreed", "I read the
  patch above and"). If it departs from that direction, it says why.
  Boilerplate reads the same on a thread with an owner's patch in it
  as on an empty one. A thread with no comments, or with only other
  users' reports and maintainer questions, gives nothing to engage.
- **AI disclosure:** read the policy text exactly. Course packages
  are AI-assisted work. If the policy requires disclosing AI use in
  issues or comments, or "in any form", the plan comment must say AI
  was used and to what extent, and name the tool if the policy asks
  for it. No stated policy, a permissive policy, a rule that
  contributors understand and review their work, or a disclosure ask
  that covers only pull requests needs no disclosure line. A rule
  that comments be written by humans is met by a specific comment in
  a person's own voice; never guess authorship from tone.
