# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-howard/pull/88

**Branch**

`fix/53-phone-us-space-separator`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. `--include-calibration --only calib-01,calib-02,calib-03,calib-04,pkg-02,pkg-11,pkg-13,pkg-18,pkg-20` (first draft of all four files): `agreement: 5/5 scored items`. The four calibration rows also read `yes` in the table.
2. Full run with the same files, committed as `eval-run.txt`: `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with `categories: clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`.

Nothing was revised between the two runs. I picked run 1's packages as the ones most likely to
break a first draft: three clear-accepts that a strict rubric could hold (pkg-02 for its
re-printed lines, pkg-11 for being terse, pkg-13 for its deferred `%VAR%` case), the arguable
unreviewable reject (pkg-18), the AI-disclosure reject (pkg-20), and the four calibration
packages, including the calib-03 trap and the calib-04 borderline.

**Package analysis**

`pkg-18` (nushell/nushell#18850), category `unreviewable`. My rubric decided **reject**. The gold
label is **reject**
(`"the is_terminal gate is correct and evidenced, but the diff drags in an import-restructure hunk, two blocks of identical re-printed lines (pure churn), and a commented-out debug eprintln; arguable because the debris is mechanical churn around a right fix"`).

I picked it because six of the seven required checks pass, and the fix is right. In the
committed run, `plan-fidelity-diff` passed with "Hunks are the is_terminal gating in
src/command.rs plus one test in tests/main.rs, both as planned; import regrouping is
formatting, graded by diff-clean", and `repro-rerun-decisive` passed with "nu --help | cat -v |
grep -c '\^\[' went 214 -> 0, matching the plan's expected-after". The one failing check was
`diff-clean`: "Commented-out debug line '// eprintln!("help colors: tty={}", ...)' plus
regrouped imports and byte-identical re-printed println! lines".

My rubric reads it as a reject because the diff has all three kinds of debris that `diff-clean`
names. The diff has `// eprintln!("help colors: tty={}", std::io::stdout().is_terminal());`,
which is "commented-out code, including commented-out debug lines". The `use` block is split
from one grouped import into five lines, which is "an import list the fix does not need,
rewritten with no change in meaning". And the `println!("{g}General:{r}");` and
`println!("{g}Startup:{r}");` blocks are removed and re-added byte-for-byte. None of those lines
belongs to a statement the fix edits: the fix only changes how `g`, `c`, `d`, `r` are assigned,
so the exception for re-printed lines does not cover them. Plan fidelity passing matters here
too. I made formatting-only edits `diff-clean`'s job ("Formatting-only edits ... are graded by
`diff-clean`, not here"), so pkg-18 fails for exactly one reason, and the reason is its
category.

**Check rationale**

`diff-clean`, pass condition as it reads now in `tools/pr-precheck/rubric.md`:

> Passes when every hunk is part of the fix, its tests, or an entry the repo asks for, and the diff carries no debris. Debris is any of: a debug print or debug-only log line (`eprintln!("DEBUG ...")`, `console.log`, a print of internal state); commented-out code, including commented-out debug lines and a kept "first attempt"; dead code (an unused function, a helper behind `allow(dead_code)` or an `_unused` name, a probe left from debugging); a TODO or note unrelated to the fix; formatting churn, meaning a statement, block, or import list the fix does not need, rewritten with no change in meaning (re-indented, reflowed, regrouped, joined onto one line, or removed and re-added byte-for-byte). Lines removed and re-added unchanged as part of a statement the fix does edit (the call whose argument changes, the match that gains an arm, the `if` whose body changes) are diff noise from that edit, not churn. Comments that explain the fix are not debris. Commit messages ("wip", "misc cleanups while debugging") may be quoted to corroborate debris in the diff but never decide this check alone.

I did not revise this check between runs. It reads this way because of two versions I rejected
before the first run.

- "Any line removed and re-added unchanged is churn," and then a softer version: "a run of three
  or more such lines is churn." Both reject pkg-02, a clear accept. In its `save_svg` hunk,
  `svg = self.export_svg(`, `title=title,` and `theme=theme,` are removed and re-added unchanged,
  three lines in a row, because the next line changes `clear=clear` to `clear=False`. That is how
  a diff tool shows an edit to one argument of a multi-line call, not churn. pkg-18's re-printed
  `println!` lines are separate statements that nothing edits. So the line I drew is whether the
  re-printed lines belong to "a statement the fix does edit". In the committed run pkg-02 passed
  with "re-printed export_html(/export_svg( lines belong to statements the fix edits".
- "Commit messages like 'wip' or 'fmt' fail the check." The commit list is the quickest tell, but
  a bad commit message on a clean diff is not an unreviewable diff. So messages "may be quoted to
  corroborate debris in the diff but never decide this check alone". They are graded separately
  by the preferred check `commits-describe-change`, which never holds a package.

**Trade-offs**

The exception for "a statement the fix does edit" gives up churn that hides inside an edited
statement. If a PR changes one argument of a call and also reflows or re-indents the rest of that
call, `diff-clean` passes it. I accept that miss: the alternative counted pkg-02's three re-printed
lines as churn and rejected a clear accept.

I checked that the exception did not loosen the check where it matters. In the committed run,
pkg-04's re-printed match arms passed `diff-clean` the same way ("re-printed match arms belong to
the edited match statement"; pkg-04 is still a reject, on `repro-rerun-decisive`). The three
unreviewable packages all still failed `diff-clean`: pkg-12, pkg-15, and pkg-18, which also ran
in run 1 and was graded the same way both times.

What the 20/20 does not show: three of the rejects rest on a single required check. pkg-04 fails
only `repro-rerun-decisive`, pkg-18 fails only `diff-clean`, and pkg-20 fails only
`ai-disclosure-met`. One different grade on any of those flips the package. pkg-20 is the one
that matters most, because the `standards-wall` category has two packages and the other one,
pkg-01, is held by two checks (`template-asks-met` and `repo-checks-run`). I have one full run,
so I cannot say how stable those three are.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
