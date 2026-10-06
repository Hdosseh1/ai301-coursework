# Rubric: is this reproduction package ready to post?

Every check reads the thing itself against the issue: what the
artifacts show, what the comments claim, what the repo asks. None of
them reads the write-up's shape. A terse package with the right proof
passes; a long, formatted, confident one with the wrong proof fails.
Locations named in the Evidence column are mapped in
`references/evidence-guide.md`.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| env-recorded | The repro report's environment record (tool version, OS, and any component the issue or thread says decides the failure: driver, shell, browser language, build profile, release vs dev build), read against the version and conditions the issue targets (issue body, reporter's environment, template asks in repo facts). | The report records at least the tool version and platform, AND every component the issue or thread ties to the failure is either recorded and matching, or the difference is stated in the report. Fails when there is no environment record, or when the tested version is older than the one the issue was confirmed on (or a different build type than the one the issue names) and the report does not say so. A different OS or install method on an issue not tied to either is fine. | required |
| steps-rerunnable | The repro report's steps: the commands, input files, configs, and starting state, read as a stranger on the thread who has only the issue and the posted comments. | A stranger could re-run the attempt from a starting state to the trigger using only what is posted: exact commands and inputs are shown, or the report points to something already public in the issue (for example "the issue's script verbatim") and says it used it unchanged. Fails when there are no steps, when any step depends on something not shared (private repo, internal config, an unshown file), or when a step drops a component the issue says is needed to trigger the bug (for example a driver on a driver-specific issue). | required |
| behavior-matches | The artifacts in the repro report (pasted output, logs, exit codes, error text, rendered results, described observations of a control) read against the behavior the issue describes, and the commands that produced them read against the issue's trigger. | The artifacts themselves, not the narration around them, show the issue's behavior (same error, panic, wrong output, or symptom) produced by the issue's trigger: the input contains every element the issue says triggers it. OR the report's stated outcome is cannot-reproduce, and its artifacts show a real attempt at the issue's scenario and what happened instead; a cannot-reproduce passes even when it admits the trigger condition may not have been reached, as long as it says so and names what differed (that admission is the report's content, not a gap). Fails when there are no artifacts, when artifacts show only setup (version banners, a session running), when the input was changed so a different error appears (a validation error, compile error, or syntax error instead of the reported crash or wrong output), or when the artifact contradicts the claim (for example the program still running when a crash is claimed). | required |
| honest-outcome | Every outcome statement in the claim comment and the repro report ("reproduced", "confirmed", "root cause", "guaranteed", "affects X too"), each held against the artifact that would back it. | Each claim is backed by an artifact shown in the package, or is plainly labeled as a guess or hypothesis. A cannot-reproduce that states what was tried and what differed from the report passes. Fails when the package claims more than it shows: a confirmed reproduction over an artifact that shows something else, a "verified" root cause with no shown evidence, certainty words ("100%", "guaranteed", "on two machines") standing in for artifacts, or a generalization to an environment that was not tested or that the thread says does not reproduce. | required |
| claim-specific | The claim comment, read against the issue it sits on. | The comment could only belong to this issue (it names the behavior, component, or result of the student's attempt), and it states a concrete next step the student will actually take. Fails on a bare +1 or me-too, interchangeable boilerplate that would fit any issue, a demand to be assigned or to reserve the issue, self-assignment, or a promise the student cannot back (a guaranteed fix, a fix by a deadline). | required |
| ai-disclosure | The repo-facts contribution policy (AI policy text), read against both comments. Treat every package as AI-assisted work: the student builds it with AI tools in this course. | If the policy requires disclosing AI use in issues or comments (for example "all AI usage in any form must be disclosed"), the claim comment or repro report contains an explicit statement that AI was used and how; it also names the tool if the policy asks for the tool. Passes automatically when the repo states no AI policy, when the policy is permissive without a disclosure ask, or when disclosure is asked only in pull requests. A "comments must be human-written" rule is met by a specific comment in a person's own voice; do not guess at authorship from tone. | required |
| control-run | The repro report's artifacts. | The report shows a contrast run (the same command without the trigger, or the setting the issue says works) next to the failing run. Strengthens a package; its absence never holds one. | preferred |

## Verdict rule

Accept if every required check passes. Reject if any required check
grades fail or unclear: unclear counts as fail, because proof that
cannot be verified is not ready to post. Preferred checks are reported
but never change the verdict.

In live mode on a claim-only draft, grade only `claim-specific`,
`ai-disclosure`, and `honest-outcome` (read against the claim comment
alone: saying a reproduction exists is fine, but certainty or
diagnosis beyond that is not). Report `env-recorded`,
`steps-rerunnable`, `behavior-matches`, and `control-run` as unclear
with evidence "not yet applicable: claim-only draft", and leave them
out of the verdict.
