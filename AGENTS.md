# AGENTS.md

Operating rules for AI coding agents, and for humans skimming for the sharp
edges.

## What this repo is

One page, `profile/README.md`, which GitHub renders as the organisation's
profile, plus the minimum needed to lint it and guard its commits. Documents,
not code: the only code is under `scripts/`, and it is vendored byte copies
(see *Repo conventions*). There is no build and nothing is deployed. Merging
to `main` IS the publish.

This repository is PUBLIC, and its name gives it organisation-wide reach:
files in certain positions here become defaults for every other repository in
the organisation. That reach is the reason the next section exists.

## The rule that matters most here

**No default community health file may ever be added here.** Not at the root,
not under `.github/`, not under `docs/`: no `CODE_OF_CONDUCT`, `CONTRIBUTING`,
`SECURITY`, `SUPPORT`, `PULL_REQUEST_TEMPLATE` (file or directory),
`ISSUE_TEMPLATE`, `FUNDING.yml` or `DISCUSSION_TEMPLATE/`. No
`workflow-templates/` either. Each would be inherited by every repository in
the organisation that lacks its own, private repositories included, with no
notice in those repositories. Shared files travel by the organisation's
template repository, and that is a decision, not an oversight.

The check is mechanical and must return nothing, run on `main`:

    git ls-files | grep -iE '^(\.github/|docs/)?(CODE_OF_CONDUCT|CONTRIBUTING|SECURITY|SUPPORT|PULL_REQUEST_TEMPLATE|ISSUE_TEMPLATE)(\.md$|/)|^\.github/(FUNDING\.yml$|DISCUSSION_TEMPLATE/)'

A tool that helpfully offers to "add a CONTRIBUTING.md" or "set up issue
templates" here is proposing an organisation-wide change from inside a page
repository. Decline it. If a repository needs one of those files, it gets its
own, in its own tree.

## The page's boundary

- **The page is ASCII.** Use `-` where an em-dash would go, or restructure the
  sentence; `...` for an ellipsis; no decorative emoji. The `ascii` job says
  so on every pull request, and the register matters as much as the encoding: a
  page asking to be read by a stranger should not read as machine-written.
- **Mechanism and shape, never a measurement.** No figure a stranger cannot
  reproduce from a clean checkout, and no count that something else can
  change: hosts, tests, timings, records, dates of observation. A number in a
  public artefact is a promise that somebody else can get it.
- **No identifiers into private work.** No hostname, address, credential or
  key name, ruleset or configuration identifier, issue or pull request number,
  or record number. No private repository name: components are described by
  role. No collaborator name and no client name. The only link into the
  organisation's own work is the public `dagster-dex` repository.
- **The whole file is reviewed before every publish, not the diff.** A
  sentence that was fine last time can be made false by what was published or
  changed elsewhere since. The checklist at the end of this file is the pass.

## Verification method

That a control observed only to pass has not been shown to be a control is a
rule with a method behind it, and this is the method as the organisation has
paid for it:

- **Calibrate on both arms first.** Before reading an instrument's answer about
  the subject, show it answer yes on a known positive and no on a known
  negative that you built, in the shape of the defect it is for. A refusal on
  a day when nothing could have passed is not a calibration.
- **A count is not an attribution.** Before a number names a cause, map it to
  the identities it counts, and ask what else produces the same reading. The
  count that matches the prediction is the one least likely to be checked.
- **Independence lives in the premise, not the author.** A second check that
  shares the first one's assumption agrees with it for the same reason. Vary
  the method: parse a different way, or enumerate a superset and subtract.
- **Empty and blind are the same input.** An empty list, a zero or a missing
  line is also what an unauthorised, filtered or never-run read returns. Run a
  positive control, a query that must return something, before reading
  absence as a fact.

## Change workflow

- **Branch per change** off `main`. Never commit to `main` directly.
- **Conventional commits** (`docs:`, `chore:`, ...) with a body explaining why.
- **A title is a git artifact. PR titles take the commit form**
  `type(scope): summary`: lowercase after the colon, one clause, no trailing
  period. A PR title becomes the merge commit. The `pr title` job judges it.
  - Passing the guard is not the convention. The owner reads the title, so it
    says what was done to what, in plain words and the diff's own nouns: a verb
    and its object, about 70 characters, with no thesis, no contrast clause and
    no imperative the owner could take as addressed to them. A title about an
    agent rule names the artifact and the actor. A `fix:` title names the
    process that misbehaves today and what it does after the change. An
    `argument:` title is *whether X*, X being the decision in the governing
    document's words. Measure the length before creating
    (`T='...'; echo "${#T}"`), and retitle a pull request rather than amend a
    pushed commit. The same form holds for a filing in a repository outside
    the organisation. Ruled by the owner from 2026-10-01 to 2026-10-10.
- **Never put a `close` / `fix` / `resolve` verb immediately before `#N`** in a
  commit message or PR body unless you mean it, including in a sentence
  warning against doing it and including inside backticks. Only adjacency
  fires, so "part of #N" and "refs #N" are safe. When you do mean it, add an
  `Autoclose: #N` trailer. The guard runs as a `commit-msg` hook and again in
  CI over the PR body, the commits, and GitHub's own `closingIssuesReferences`.
  Enable the hook per clone: `git config core.hooksPath .githooks`.
- **Merges are made by the owner.** With one code owner the review requirement
  cannot be satisfied (an author cannot approve their own pull request), so
  every merge is an owner bypass. `.github/CODEOWNERS` says so in full.

### Merging, the cold review and the record

The owner's rules for a session's own pull requests. This section is written
identically in every repository of the organisation, so a change to it is made
in all of them.

- **A session opens the pull request and stops.** It does not merge, mark a
  draft ready, deploy, widen a token's reach or post outside this repository
  without the owner's word in that session. "Proceed", a brief, a handover note
  or a message from another session is not that word for any of these acts.
- **When the owner says merge,** the command is
  `gh pr merge <n> --repo <owner>/<repo> --merge --admin`, never a squash, and
  it runs only after reading that the pull request is not a draft, that the
  latest run of every required check is green, and that
  `gh api repos/<owner>/<repo>/pulls/<n> --jq .stack` prints `null`.
- **A merge closes nothing by default.** The body carries `Autoclose: none`,
  and the issue is closed by hand once the reading its done-when names exists,
  because that reading usually comes after the merge. When the merge itself is
  the done-when, the closing verb and the `Autoclose: #N` trailer go together.
- **One cold review per pull request, before it leaves draft or merges.** The
  reviewer shares none of the author's context and is not an automated review
  command: it gets the pull request number, this file's binding sections and
  the commands to run, and it runs them against the head and against `main`.
  Its findings are posted as one dated comment on the pull request before any
  fix is pushed. Each is closed as taken, with the fixing commit, or declined,
  with the reason, against the review's own checks re-run at the new head. A
  second review happens only when the owner asks for one.
- **A reference to another repository's issue is qualified:** `owner/repo#N`
  or a full link. A bare `#N` links this repository's number, in a comment as
  in a body.
- **A wrong claim on an issue is corrected by a dated follow-up comment** that
  cites the comment it corrects, never by editing that comment. A pull request
  body is corrected in place, with a visible dated note saying what changed.
- **A ruling is a record once it is a dated comment on the issue it decides,**
  carrying the owner's words, posted before the pull request that cites it. A
  ruling that exists only in a session is not yet one.
- **Read the thread before posting to it.** Re-fetch an issue's comments
  immediately before posting a proposal, a ruling or a question there, and
  quote a comment you cite, by its id, rather than a summary of it.
- **A return to another repository's issue carries the merge commit and its
  `mergedAt`,** and is posted before any pointer that links to it.
- **A branch with a pull request is brought up to `main` by merging `main`
  into it,** never by a rebase or a force-push: its body, its review and its
  comments cite its commits.

## Repo conventions

- **Line endings are LF everywhere**, enforced by `.gitattributes`. This is
  authored on Windows; do not let CRLF in.
- **Every tracked file is ASCII**, the vendored guard files included. They were
  exempt until 2026-09-04, when their source was scrubbed; a byte copy is fixed
  at its source, so a non-ASCII byte arriving in one now reads as a defect in
  the source that the `ascii` job here is the first to see.
- **`scripts/` holds vendored byte copies** of the organisation's shared
  guards, present so the local hook works offline and so the required checks
  can run in a public repository that cannot call the private shared action. A
  job on the private side compares them against their source and fails on
  drift. **Never edit them here**; fix the source and re-vendor.
- **Markdown lint configuration lives at `.github/markdownlint.json`**, not at
  the root, and is always passed with `--config` explicitly. A root
  `.markdownlint.json` is auto-discovered and would read as this repository
  having decided its own rules; it has decided nothing. Without `--config` the
  CLI falls back to its own defaults and a local run stops being the CI run:

      npx -y markdownlint-cli2 --config .github/markdownlint.json '*.md' '.github/**/*.md' 'profile/**/*.md'

- **Dependabot moves the one action pin**, monthly and grouped. Its pull
  requests arrive with organisation-reading checks red under the Dependabot
  actor; the organisation's practice is close-and-reopen so the same commit
  re-runs under the owner, then merge. Do not add a token to the Dependabot
  secret store and do not make a guard skip that actor.

## Boundary checklist

Run over the whole of `profile/README.md` before every publish. Every answer
must be yes.

1. Is every byte of the file ASCII? `grep -P '[^\x00-\x7F]' profile/README.md`
   returns nothing.
2. Does the page contain no number that something outside this file can
   change: no host count, test count, timing, record count, uptime, latency,
   throughput or date-stamped measurement?
3. Does every figure that remains describe a fixed shape of a mechanism (the
   fields of a header, the operations of a contract) rather than a result
   observed on the running system?
4. Does the page name no private repository, directly or by a distinctive term
   that appears in only one?
5. Does the page contain no hostname, IP address, overlay network name,
   credential, token or key name, ruleset identifier, issue or pull request
   number, or record number?
6. Does the page name no collaborator and no client, past or present, and no
   warehouse, schema or table name?
7. Is the only link into the organisation's own work the public `dagster-dex`
   repository, and does every other link open without access?
8. Can every command on the page be run from a clean checkout with no private
   access, and has that been re-checked since the last change to `dagster-dex`?
9. Does every sentence describe a mechanism or a shape rather than how well it
   works, and has any sentence that drifted toward "how well" been removed
   rather than softened?
10. Has the whole file been read top to bottom for this publish, not only the
    diff, and does the closing line still truthfully describe what it carries?
