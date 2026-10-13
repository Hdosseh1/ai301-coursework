# Rubric: is this plan ready to post and build from?

Every check reads the plan itself against the issue and its repro
evidence: what the evidence pins down, what the plan would change, what
the thread and the repo ask. None of them reads the write-up's shape. A
three-sentence plan that names the cause, the site, and the observable
result passes; a long, sectioned, confident one that the evidence rules
out fails. Locations named in the Evidence column are mapped in
`references/evidence-guide.md`, and `procedure.md` says when each one
is gathered.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| cause-grounded | The plan's stated cause (its diagnosis line or section), read against every step, artifact, and control run in the repro evidence. | The plan names a specific cause (a mechanism at a named place, not "something changed"), and nothing the repro evidence records contradicts it: the failing run fails the way that cause says, and every control run comes out the way the evidence shows. An inferred cause passes; it does not have to be proven, and a result the cause is merely silent on is not a contradiction. Fails when a recorded result could not happen if the cause were true (the blamed component is shown working in a control run, the damage is shown to exist before the blamed step runs, or an artifact locates the failure in a different layer than the one blamed), when the plan sets a recorded result aside ("red herring", "unrelated", "side effect") without an artifact for doing so, or when the plan names no cause at all. A cause adopted from the thread gets no credit for its source; it is held to the same evidence. | required |
| fixes-reported-behavior | The plan's proposed change, read against the issue's expected behavior and the repro evidence's failing step. | If the change were made exactly as written, the repro's failing step would produce the issue's expected behavior with the user doing nothing differently. A fix limited to the reported case passes when the rest is deferred with a stated reason (a variant raised only in thread comments, a platform the author cannot test, a larger rework left for later). Fails when the plan leaves the behavior in place and routes around it: documenting a workaround, or asking users to change their command or configuration. | required |
| one-bounded-change | The plan's full list of changes and files (scope statement, approach steps, files list), read against the issue's reported behavior. | Every code change in the plan is needed to make the reported behavior stop, and the changes sit in the one code path where the cause lives. These all count as part of that one change: several edits in that path, fixing a sibling site with the identical defect that the issue or repro names, regression tests and fixtures for this fix, a doc line for the changed behavior, and auditing or noting related spots without changing them. Fails when the plan carries any change the issue did not ask for: a dependency migration or upgrade, a new option, prop, or setting, a refactor, restructure, or rewrite of the surrounding module, moving other constructs or code paths onto a new shared mechanism, new CI infrastructure, or a fix for a different issue done "while in the area". A correct core fix does not rescue a plan that bundles these, and offering to split the work into a PR series does not either. | required |
| stranger-can-start | The plan's where and how: the files, functions, or code paths it names, and its approach steps. | A contributor who has never talked to the author could open the code and begin: the plan names where the change goes (a file, function, module, or a named code path such as "the reattach handshake in the server's client-connection handling") and commits to one approach there. An exact line or function left open passes when the plan says so and says how it will be pinned: that is a stated unknown inside a chosen area and approach. Length does not matter. Fails when the steps are investigation instead of change ("profile", "look into", "figure out what changed", "poke around"), when the layer or location is undecided ("somewhere", "not sure which layer"), or when the approach is left as an open choice between alternatives ("upstream or vendored, whichever is easier"). | required |
| observable-test | The plan's test plan, read against the repro evidence's steps and artifacts. | The test plan names at least one concrete observation that would tell a fixed build from a broken one for this issue: a step or command (the repro's own, or an equivalent) and the specific result expected after the change (an output, an exit code, a visible state, a count). Fails when the only stated outcome is a feeling or a generality ("should feel fast", "works afterwards", "nothing else should feel broken"), or when the only test is running the existing suite with nothing observed for the fix itself. | required |
| thread-direction-engaged | The plan comment, read against the thread highlights written by maintainers (author role OWNER, MEMBER, COLLABORATOR, or CONTRIBUTOR). | Where a maintainer in the thread has given direction on this issue's cause or fix (pointed at the culprit code, proposed or settled an approach, posted a patch or test build, opened a fix elsewhere, or ruled an approach out), the comment says how the plan relates to that direction: it follows it, builds on it, or departs from it and says why. Passes automatically when the thread has no comments, or when no maintainer comment gives direction on the cause or fix (a question, a "cannot reproduce", or another user's report is not direction). Fails when such direction exists and the comment presents its plan as if the direction were not there. | required |
| ai-policy-met | The repo-facts contribution policy line (its AI-policy text), read against the plan comment. Treat every package as AI-assisted work: plans in this course are built with AI tools. | If the policy requires disclosing AI use in issues or comments, or "in any form", the plan comment states explicitly that AI was used and to what extent, and names the tool if the policy asks for the tool. Passes automatically when the repo states no AI policy, when the policy is permissive with no disclosure ask, when it only asks contributors to understand, review, or take responsibility for their work, or when disclosure is asked only in pull requests. A rule that comments be written by humans in their own words is met by a specific comment in a person's own voice; do not guess authorship from tone. | required |
| unknowns-named | The plan's risk, unknown, and open-question statements, and anything the plan comment flags for review. | The plan names at least one thing it has not verified (a risk, an unmeasured cost, an untested platform, a site not yet pinned), or says there is none and why, and it promises no date. Strengthens a plan; a terse plan with nothing left open is never held for lacking a risk section. | preferred |
| comment-carries-plan | The plan comment, read against the candidate plan. | A maintainer reading only the comment learns what will change and where, in words that could only belong to this issue, and the comment agrees with the plan (same cause, same scope). Reported for the author; never holds a package. | preferred |

## Verdict rule

Accept if every required check passes. Reject if any required check
grades fail or unclear: unclear counts as fail, because a plan that
cannot be verified from the package is not ready to build from.
Preferred checks are reported but never change the verdict, in either
direction.

`unclear` is only for a missing part: the package does not contain the
thing the check reads (no plan comment, no repro evidence quoted). When
the part is there, the grade is pass or fail. An empty thread or a
repo with no AI policy is not a missing part; the pass conditions
above say those pass.

In live mode the same rule applies to the student's `plan.md` and
draft comment. Under the Path Review house rules in `scope.md`, a
classmate's plan comment on the same issue is not maintainer
direction and never fails `thread-direction-engaged`. After a
recorded deviation, the updated plan is graded whole, by the same
checks: the deviation note is part of the plan.
