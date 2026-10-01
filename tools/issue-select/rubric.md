# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| repo-not-archived | The `archived:` value on the repo line under Repo facts. | Pass if `archived: no`. Fail if `archived: yes`. | required |
| maintainer-alive | The "last 5 default-branch commits" list under Repo facts (dates and author names). | Pass if at least one of those commits is dated within 90 days of the capture date AND is either authored by a human or is a bot commit merging a human's pull request ("Merge pull request #N from <user>/..."). Commits that are only bot dependency bumps, generated content, or leaderboard updates do not count. Otherwise fail. | required |
| single-task | The issue title and body, plus the state of each PR on the "linked PRs:" line under Repo facts. | Fail if ANY of: (a) the issue describes itself as an umbrella, tracking issue, meta issue, or megaissue, or explicitly says its items are to be split into separate issues or PRs. A list of related changes that one PR can deliver (an acceptance-criteria checklist, several files or docs pages to update, several examples or instances of the same bug) is NOT an umbrella and does not fail (a); (b) at least one linked PR is `merged` while the issue is still open (the work is being done piecemeal); (c) the issue is a usage or support question ("how do I...") rather than a request for a code or docs change. Otherwise pass. | required |
| spec-settled | The issue opener and their author_association, the issue labels, the comment thread, and closed (unmerged) PRs on the "linked PRs:" line and in the thread. | Fail if ANY of: (a) the thread shows an open design question (two or more competing approaches, or a maintainer asking what the behavior should be) with no later OWNER/MEMBER/COLLABORATOR comment settling it; (b) the body leaves a key decision undecided ("TBD", "to be decided"); (c) two or more closed, unmerged PRs have attempted the issue; (d) it is a new-feature or enhancement request with no maintainer endorsement, meaning the opener is not OWNER/MEMBER/COLLABORATOR, no maintainer comment agrees with it, and it carries no labels at all. Any label counts as endorsement, including a plain type label such as `type::documentation` or `enhancement`. Clause (d) never applies to bug reports or documentation tasks. Check every clause independently: endorsement (a maintainer opener, a label, a good-first-issue tag) only answers (d) and never overrides a failure on (a), (b), or (c); in particular, count the closed unmerged PRs on the "linked PRs:" line for (c) even when the issue is labeled. Otherwise pass. | required |
| unclaimed | The "this issue: assignees:" and "linked PRs:" values under Repo facts, plus every PR and claim comment ("I'll take this", "working on this", "can I work on this") in the Comments section. | Pass only if ALL of: no assignee; no PR in `open` state, whether formally linked, from a fork, or only mentioned in a comment; and no claim comment dated within 60 days of the capture date. A claim comment older than 60 days with no open PR is stale and does not block. Otherwise fail. | required |
| ai-policy-allows | The "contribution policy" line under Repo facts. | Fail only if the policy bans AI-generated code or documentation outright (e.g. "we do not accept AI-generated code"). Conditions pass: requirements to disclose AI use, understand, test, or review changes, "fully AI-generated contributions are not accepted" with assistive use allowed, or "strongly discouraged". No CONTRIBUTING.md or no statement on AI also passes. | required |
| responsive-maintainer | The "maintainer first-response sample" under Repo facts. | Pass if at least one sampled issue got a first OWNER/MEMBER/COLLABORATOR comment within 7 days. Otherwise fail. | preferred |
| recent-release | The "latest release" line under Repo facts. | Pass if the latest release is dated within 12 months of the capture date. "none published" fails. | preferred |
| newcomer-signal | The issue labels. | Pass if the issue carries a `good first issue`, `help wanted`, or `easy` label (any capitalization). | preferred |
| starting-pointer | The issue body and maintainer comments. | Pass if they name the specific files, directories, components, functions, or docs page to change. | preferred |

## Verdict rule

Accept if and only if every `required` check grades `pass`; any `required`
check that grades `fail` or `unclear` rejects the issue. Checks whose pass
condition defines what absent evidence means (e.g. no stated AI policy
passes, no assignee passes) are graded by that definition, never `unclear`.
`preferred` checks never change the verdict: among accepted issues, rank by
the number of preferred checks passed (more is better), and break ties with
the fit profile in `scope.md` in live mode.
