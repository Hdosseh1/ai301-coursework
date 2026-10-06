# Evidence guide: where proof lives in a reproduction package

In an eval bundle a package has five parts, in order: the header
(`source`, `captured`), **Repo facts**, **Issue** (title, body, the
reporter's environment), **Thread highlights**, and the two candidate
parts: **Candidate claim comment** and **Candidate repro report**. The
bundle is the whole world; never fetch.

In live mode the issue side comes from GitHub (the issue body, its
comments, the repo's `CONTRIBUTING.md`, any `AI_POLICY.md`, and the
issue template under `.github/ISSUE_TEMPLATE/`), and the candidate side
is the student's draft files. Read the drafts as a stranger on the
thread will: only what the drafts contain or quote counts.

## Environment

- **Where it lives:** the environment line or table in the repro
  report (usually first). The target to compare against lives in the
  issue body (the reporter's version, OS, build), in thread comments
  that confirm or narrow it ("confirmed on main", "only Release
  builds", "also on macOS"), and in the repo facts' latest release and
  template asks (`--version` output, `conda info`, driver, shell).
- **What good looks like:** the tool version and platform are named.
  Every component the issue or thread ties to the failure is recorded
  (a Windows-only issue names Windows and the driver, a build-profile
  issue names the build, a language-dependent issue names the browser
  language). If the tested version differs from the issue's, the report
  says so in words ("filed against 13.0.0; still present on 15.2.0").
  Testing an *older* version than the issue was confirmed on, without
  saying so, is a silent deviation: whatever it shows is that version's
  behavior, not evidence about the reported bug.

## Steps

- **Where it lives:** the repro report's steps or command transcript:
  every `$`-prefixed command, file written with `printf`/`cat`, config
  shown, and the starting state (empty directory, fresh install, a
  layout created from the issue).
- **What good looks like:** someone with only the issue and the posted
  comments could type it again and reach the trigger. Inputs are shown
  or are byte-for-byte the issue's own (said explicitly). Nothing
  depends on private code, internal config, or unshown files. A
  required condition from the issue (a driver, a flag such as
  `--replace`, a language setting) is not dropped. Count does not
  matter: one exact command is complete; ten vague steps are not.

## Behavior shown

- **Where it lives:** the artifacts in the repro report: pasted
  output, logs, error text, exit codes, rendered output, parser
  results, and control runs. Read each next to the issue's own "actual"
  output or symptom description, and read the command that produced it
  next to the issue's trigger.
- **What good looks like:** the artifact itself shows the issue's
  behavior: same panic or error message, same wrong output, same
  symptom, from an input that contains every element the issue names
  as the trigger. Check the input character by character against the
  issue (`:-N` vs `N:`, `{}` vs `.`, `=` vs `:`); a changed input that
  produces a *different* error (argument validation, compile error,
  syntax error) is an adjacent behavior even when the narration calls
  it "the crash". An artifact that shows only setup (version banner,
  session list, "tabs visible") shows nothing. An artifact that
  contradicts the narration (window still open, prompt returned, when a
  crash is claimed) shows the narration is wrong. A control run (the
  same command without the trigger behaving correctly) is the strongest
  form of this proof.
- **Cannot-reproduce:** the artifacts show a real attempt at the
  issue's scenario (commands plus their output) and what happened
  instead. That is real evidence and passes this family, even when the
  report admits it may not have hit the exact trigger condition, as
  long as it names what differed. Only a report that *claims* to have
  reproduced the bug is held to "the artifact shows the issue's
  behavior".

## Honesty

- **Where it lives:** every outcome sentence in the claim comment and
  the report ("reproduced", "confirmed", "root cause is", "verified",
  "guaranteed", "also affects the Store release"), and the matching
  artifact, or its absence, in Behavior shown.
- **What good looks like:** each claim points at something shown.
  Hypotheses are labeled ("looks like", "may require", "suggests"). A
  cannot-reproduce says so first, names what was tried, and names what
  differed from the report (OS, shell, limits, data shape). Red flags:
  certainty words or repetition counts ("100%", "ten times", "two
  machines") in place of an artifact; a root cause asserted with no
  transcript, log, or code excerpt; extending the bug to an environment
  the report did not test, or that a maintainer in the thread said does
  not reproduce.

## Comms

- **Where it lives:** the claim comment, against the issue; both
  comments, against the repo facts' contribution policy (the AI-policy
  text) and template asks.
- **What good looks like:** the claim comment could only sit on this
  issue: it names the behavior or what the student found, and it
  states one concrete next step (a file, function, or experiment).
  Boilerplate looks the same on any issue: "kindly assign", "keep it
  reserved", "fix in 2 days guaranteed", "+1 any updates?",
  "assigning myself".
- **AI disclosure:** read the repo facts' policy text exactly. Course
  packages are AI-assisted work. If the policy requires disclosing AI
  use in issues or comments ("all AI usage in any form must be
  disclosed"), one of the comments must say AI was used and to what
  extent (and name the tool if the policy asks). No stated policy, a
  permissive policy, or a disclosure ask that covers only pull requests
  needs no disclosure line. A rule that comments be written by humans
  is met by a specific comment in a person's own voice; never guess
  authorship from tone.
