# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-howard/issues/53

**Verdict output**

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

````
**Ranked read-out (accepted):**

1. **#53: PII scrubber misses `(555) 123-4567`.** All six required checks pass and nobody has claimed it: no assignee, no open PR, no comments. It has the clearest spec of the three, with reproduction steps, the observed output, and the four failing tests named.
2. **#75: README and `.env.example` disagree about the API key.** All required checks pass and it's unclaimed. It's a 1–2 hour edit to two files, but it's just making two files agree, so you'd learn less from it.
3. **#68: Keyword search `ZeroDivisionError` on an empty index.** It passes, but only under the Path Review house rule. A classmate (alperensumeroglu) already has an open PR, #73, for it. The house rule says classmates' claims don't block. Read literally, though, the rubric's "no open PR" condition would reject it. Even if you take it, PR #73 may get merged before yours.

**Rejected:** none.

**How the ranking was decided:** all three tie on preferred checks (2 of 4 each). Each has a `good first issue` label and names the files to change. Each fails `responsive-maintainer`, because no maintainer has replied to any student comment in the sampled threads. Each fails `recent-release`, because the repo has never published a release. Your fit profile in `scope.md` is still blank, so it couldn't break the tie. I ordered them by how likely each is to still be open for your PR and how much you'd learn from it. That tie-break is my call, not the rubric's. If you fill in the fit profile, the ranking could change.

The shared facts behind every grade: the repo isn't archived; the last commit was 2026-09-16 by Aburke225, a human, 15 days ago; and docs/CONTRIBUTING.md says nothing about AI.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-howard/issues/53",
    "checks": [
      {"name": "repo-not-archived", "grade": "pass", "evidence": "GitHub API: archived: false"},
      {"name": "maintainer-alive", "grade": "pass", "evidence": "2026-09-16 by Aburke225: chore: track five more manifest entries against the tracker (15 days ago, human author)"},
      {"name": "single-task", "grade": "pass", "evidence": "one regex fix in pii_scrubber.py for the parenthesized phone format; no umbrella language, no linked PRs"},
      {"name": "spec-settled", "grade": "pass", "evidence": "bug report with repro and observed output: scrub('Call me at (555) 123-4567 ...') leaves it unredacted; opened by Aburke225 (COLLABORATOR); no debate, no closed PRs"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; no linked or mentioned PRs among open PRs #73-#78; 0 comments"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no statement on AI; no AGENTS.md or AI_POLICY.md"},
      {"name": "responsive-maintainer", "grade": "fail", "evidence": "sampled issues #72, #65, #61, #54: student comments from 2026-09-18 onward have no OWNER/MEMBER/COLLABORATOR reply"},
      {"name": "recent-release", "grade": "fail", "evidence": "releases: none published"},
      {"name": "newcomer-signal", "grade": "pass", "evidence": "labels: bug, good first issue, safety, tier-1"},
      {"name": "starting-pointer", "grade": "pass", "evidence": "names pii_scrubber.py and tests/unit/test_pii_scrubber.py with four failing test names"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-howard/issues/75",
    "checks": [
      {"name": "repo-not-archived", "grade": "pass", "evidence": "GitHub API: archived: false"},
      {"name": "maintainer-alive", "grade": "pass", "evidence": "2026-09-16 by Aburke225: chore: track five more manifest entries against the tracker (15 days ago, human author)"},
      {"name": "single-task", "grade": "pass", "evidence": "'Make the two files agree': README.md and .env.example only; no linked PRs"},
      {"name": "spec-settled", "grade": "pass", "evidence": "bug states the observed mismatch (OPENROUTER_API_KEY missing from .env.example); opened by Aburke225 (COLLABORATOR); no comments, no closed PRs"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; no linked or mentioned PRs among open PRs #73-#78; 0 comments"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no statement on AI; no AGENTS.md or AI_POLICY.md"},
      {"name": "responsive-maintainer", "grade": "fail", "evidence": "sampled issues #72, #65, #61, #54: student comments from 2026-09-18 onward have no OWNER/MEMBER/COLLABORATOR reply"},
      {"name": "recent-release", "grade": "fail", "evidence": "releases: none published"},
      {"name": "newcomer-signal", "grade": "pass", "evidence": "labels: bug, good first issue, docs, tier-1"},
      {"name": "starting-pointer", "grade": "pass", "evidence": "Relevant files: README.md, .env.example (core/config.py defines both keys)"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-howard/issues/68",
    "checks": [
      {"name": "repo-not-archived", "grade": "pass", "evidence": "GitHub API: archived: false"},
      {"name": "maintainer-alive", "grade": "pass", "evidence": "2026-09-16 by Aburke225: chore: track five more manifest entries against the tracker (15 days ago, human author)"},
      {"name": "single-task", "grade": "pass", "evidence": "index([]) should not raise; one function in rag/retriever/keyword_search.py plus removing one xfail marker"},
      {"name": "spec-settled", "grade": "pass", "evidence": "bug states observed ZeroDivisionError from BM25Okapi on index([]); opened by Aburke225 (COLLABORATOR); no closed PRs"},
      {"name": "unclaimed", "grade": "pass", "evidence": "open PR #73 by alperensumeroglu (NONE, classmate) cross-referenced 2026-09-15; Path Review house rule: classmates' claims do not block"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no statement on AI; no AGENTS.md or AI_POLICY.md"},
      {"name": "responsive-maintainer", "grade": "fail", "evidence": "sampled issues #72, #65, #61, #54: student comments from 2026-09-18 onward have no OWNER/MEMBER/COLLABORATOR reply"},
      {"name": "recent-release", "grade": "fail", "evidence": "releases: none published"},
      {"name": "newcomer-signal", "grade": "pass", "evidence": "labels: bug, good first issue, rag, tier-1"},
      {"name": "starting-pointer", "grade": "pass", "evidence": "Relevant files: rag/retriever/keyword_search.py, tests/unit/test_keyword_search.py"}
    ],
    "verdict": "accept"
  }
]
```
````

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Full run (original rubric): `agreement: 18/20 scored items  (bar: 18/20: PASS)`
2. `--only issue-01,issue-04,issue-05,issue-10,issue-20` (after rewording `single-task` (a) and `spec-settled` (d)): `agreement: 5/5 scored items`
3. Full run: `agreement: 19/20 scored items  (bar: 18/20: PASS)`
4. `--only issue-15,issue-09,issue-01,issue-04` (after adding the "Check every clause independently" sentence to `spec-settled`): `agreement: 4/4 scored items`
5. Full run, same rubric as the final run: `agreement: 20/20 scored items  (bar: 18/20: PASS)`
6. Full run, committed as `eval-run.txt` (same rubric as run 5): `agreement: 18/20 scored items  (bar: 18/20: PASS)`

**Issue analysis**

`issue-15` (zulip/zulip#19589). My rubric's decision in the final run: **reject**. Gold label: **reject**
(`"years of design debate and two abandoned PRs behind a friendly label"`).

The bundle's repo facts say `this issue: assignees: none; linked PRs: zulip/zulip#20840 (closed); zulip/zulip#23123 (closed)`,
and the issue line is `opened by esamson (NONE) on 2021-08-18, state open, labels: help wanted, area: integrations, good first issue`.
It passes liveness, claim and policy, so the reject comes from `spec-settled` clause (c): "two or more closed,
unmerged PRs have attempted the issue". In run 5 the grader's evidence for that check was
`spec-settled fail: linked PRs: zulip/zulip#20840 (closed); zulip/zulip#23123 (closed)`.

This issue didn't always come out right. In run 3 it was graded **accept**, and the grader's `spec-settled` evidence was
`labels: help wanted, area: integrations, good first issue`. It saw the labels as maintainer endorsement and
stopped there, without counting the two abandoned PRs. I fixed this by adding the sentence "endorsement (a maintainer
opener, a label, a good-first-issue tag) only answers (d) and never overrides a failure on (a), (b), or (c)",
and it has been rejected in every run since (runs 4, 5 and 6).

**Check rationale**

`spec-settled`, as currently written in `rubric.md`:

> Fail if ANY of: (a) the thread shows an open design question (two or more competing approaches, or a maintainer asking what the behavior should be) with no later OWNER/MEMBER/COLLABORATOR comment settling it; (b) the body leaves a key decision undecided ("TBD", "to be decided"); (c) two or more closed, unmerged PRs have attempted the issue; (d) it is a new-feature or enhancement request with no maintainer endorsement, meaning the opener is not OWNER/MEMBER/COLLABORATOR, no maintainer comment agrees with it, and it carries no labels at all. Any label counts as endorsement, including a plain type label such as `type::documentation` or `enhancement`. Clause (d) never applies to bug reports or documentation tasks. Check every clause independently: endorsement (a maintainer opener, a label, a good-first-issue tag) only answers (d) and never overrides a failure on (a), (b), or (c); in particular, count the closed unmerged PRs on the "linked PRs:" line for (c) even when the issue is labeled. Otherwise pass.

Each clause is aimed at a scope trap that hides behind a friendly label:
- (a) catches long design debates such as issue-15 and calib-04.
- (b) and (d) catch issue-20. It was `opened by cursor[bot] (NONE)` with `labels: none`, and its body says `Logo asset TBD.` The feature request was never endorsed and still has an open product decision.
- (c) is set at **two** closed PRs, not one, because issue-09 is a gold accept with one closed PR (`linked PRs: conda/conda#11627 (closed)`). issue-15 has two.

The last three sentences were added during iteration:
- "Any label counts as endorsement" and the exemption for bugs and docs came after run 1. There, issue-01 (gold accept) failed with the evidence `opened by dashagurova (CONTRIBUTOR) with no maintainer comments and only type::documentation label`.
- "Check every clause independently" came after run 3, where issue-15 passed on its labels alone (see Issue analysis).

**Trade-offs**

Setting clause (c) at two closed PRs gives up a case I accept it will miss: an issue whose single earlier
attempt was abandoned because the problem is harder than it looks will still pass this check.

The stricter "check every clause independently" wording also has a cost I can see. After adding it, I re-ran the canaries
`--only issue-15,issue-09,issue-01,issue-04` and got `agreement: 4/4 scored items`, and run 5 scored 20/20.
In the committed run 6, though, issue-09 flipped:
`issue-09  accept  reject   NO     failed: spec-settled, responsive-maintainer (preferred), starting-pointer (preferred)`.
issue-09 is an issue from 2018 with a long history, and with every clause checked independently the grader is more willing to read an old
thread as an unsettled design question. I kept the wording because it fixed a consistent miss on
issue-15. The issue-09 miss happened in one run of three on the same rubric, and the final run still passes the bar
(`agreement: 18/20 scored items  (bar: 18/20: PASS)`).

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

[TODO, write this yourself; it's graded on being your own words:

1. The issue's fit to your interests and to the time available.
2. What the verdict identified correctly, and what you weighed that the rubric could
   not.
3. The anticipated difficulty in claiming it.]

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
