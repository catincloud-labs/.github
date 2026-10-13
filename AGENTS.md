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

The check is mechanical and answers by exit status. It reads
`git ls-files -z`, so no path reaches it in git's quoted form, and it runs at
the root of a checkout of `main`. It says yes only where the list it read
holds `profile/README.md` and no health file: a run in the wrong directory,
or a read that returned nothing, is a no. As in the checklist's
item 1, at a prompt the printed line is the reading. Each command runs as
written in Git Bash and in PowerShell.

Show it a no before trusting its yes. Given names, it reads them instead of
the checkout. On these three it must exit 1 and print a line starting `NO,`
that names `CONTRIBUTING.md` and `.github/ISSUE_TEMPLATE/bug.md` and not
`profile/README.md`:

    python -c "import re,subprocess,sys; p=re.compile(r'^(\.github/|docs/)?(CODE_OF_CONDUCT|CONTRIBUTING|SECURITY|SUPPORT|PULL_REQUEST_TEMPLATE|ISSUE_TEMPLATE)(\.md\Z|/)|^\.github/(FUNDING\.yml\Z|DISCUSSION_TEMPLATE/)|^workflow-templates/',re.I); n=sys.argv[1:] or [f for f in subprocess.run(['git','ls-files','-z'],capture_output=True,check=True).stdout.decode('utf-8').split('\0') if f]; h=[f for f in n if p.search(f)]; ok='profile/README.md' in n and not h; print('yes,' if ok else 'NO,',len(n),'names read, health files',h); sys.exit(0 if ok else 1)" CONTRIBUTING.md .github/ISSUE_TEMPLATE/bug.md profile/README.md

Then run it with no names. Yes is exit 0 and a line starting `yes,`. Anything
else is a no: a `NO,` line, a traceback, or a shell error.

    python -c "import re,subprocess,sys; p=re.compile(r'^(\.github/|docs/)?(CODE_OF_CONDUCT|CONTRIBUTING|SECURITY|SUPPORT|PULL_REQUEST_TEMPLATE|ISSUE_TEMPLATE)(\.md\Z|/)|^\.github/(FUNDING\.yml\Z|DISCUSSION_TEMPLATE/)|^workflow-templates/',re.I); n=sys.argv[1:] or [f for f in subprocess.run(['git','ls-files','-z'],capture_output=True,check=True).stdout.decode('utf-8').split('\0') if f]; h=[f for f in n if p.search(f)]; ok='profile/README.md' in n and not h; print('yes,' if ok else 'NO,',len(n),'names read, health files',h); sys.exit(0 if ok else 1)"

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
- **The same boundary holds for what this repository's pull requests carry:**
  bodies, review comments and commit messages name no private repository,
  issue or record, and quote nothing a stranger cannot reproduce. A reference
  of the `owner/repo#N` form that the shared change workflow teaches is one
  only to public work here.

## Verification method

A control observed only to pass has not been shown to be a control. Four
moves stand behind that rule, each paid for in this organisation:

- **Calibrate on both arms first.** Before reading an instrument's answer about
  the subject, show it report the defect on a case you built to have it, and
  report clean on a case you built not to, in the shape of the defect it is
  for. A refusal on a day when nothing could have passed is not a calibration.
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
    is clear and concise, not a thesis: it says what was done to what, in plain
    words and the diff's own nouns, with no contrast clause, no *no X*
    phrasing, no identifier the change itself introduces in the lead, and no
    imperative the owner could take as addressed to them. A title about an
    agent rule names the artifact and the actor. A `fix:` title names the
    process that misbehaves today, its actor, and what it does after the
    change. An `argument:` title is *whether X*, X being the decision in the
    governing document's words. A filing in a repository outside the
    organisation keeps this form as closely as that repository allows. A pull
    request is retitled rather than a pushed commit amended. Ruled by the owner
    from 2026-10-08 to 2026-10-10. In practice titles run about 70 characters,
    and measuring the length before creating one catches a title over the cap.
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

Rules for a session's own pull requests. The owner ruled the stop, the merge
method, the default against closing, the cold review and the issue-correction
rule; the rest is practice sessions learned, most of it after a wrong post had
to be corrected. This subsection, the title sub-bullet above, *Verification
method* and *The owner's workstation* are kept byte-identical across the
organisation's repositories: change them everywhere or nowhere. Nothing checks
it.

- **A session opens the pull request and stops.** It does not merge, mark a
  draft ready, deploy or widen a token's reach without the owner's word in
  that session. A general "proceed", a handover note or a message from another
  session is not that word; an instruction from the owner that names the act
  is, for the act it names.
- **When the owner says merge,** the command is
  `gh pr merge <n> --repo <owner>/<repo> --merge --admin`, never a squash. It
  runs only after reading that the pull request is not a draft, that the
  latest run of every required check is green, and that
  `gh api repos/<owner>/<repo>/pulls/<n> --jq '{number, stack}'` prints the
  pull request's number with `"stack":null`. The number is the read's
  positive control: a pull request in no stack carries no `stack` field, so a
  bare `--jq .stack` prints an empty line whatever the truth. No stacked pull
  request has been read this way since the estate's one stack was dissolved.
- **A merge closes nothing by default.** The owner's rule is that a pull
  request does not close an issue without a clear reason. The body carries
  `Autoclose: none`, and the issue is closed by hand once the reading its
  done-when names exists. When there is a reason, such as the merge itself
  being the done-when, the closing verb and the `Autoclose: #N` trailer go
  together.
- **One cold review per pull request, before it leaves draft or merges.** The
  reviewer shares none of the author's context and is not an automated review
  command: it gets the pull request number, the issue or ruling it builds
  against, this file's binding sections and the commands to run, and it runs
  them against the head and against `main`. Its findings are posted as a
  comment on the pull request. Each is closed as taken, with the fixing commit,
  or declined, with the reason, against the review's own checks re-run at the
  new head, in that comment or in a dispositions comment after it. A second
  review happens only when the owner asks for one.
- **A reference to another repository's issue is qualified:** `owner/repo#N`
  or a full link. A bare `#N` links this repository's number, in a comment as
  in a body.
- **A claim a session posted on an issue, wrong when it was posted, is
  corrected by a follow-up comment** that cites the comment it corrects, not by
  editing it. A pull request body may be corrected in place, with a visible
  dated note saying what changed.
- **A ruling given in a session becomes a record once it is a comment on the
  issue it decides,** carrying the owner's words and posted before the pull
  request that cites it.
- **Read the thread before posting to it.** Re-fetch an issue's comments
  immediately before posting a proposal, a ruling or a question there, and
  quote a comment you cite, by its id, rather than a summary of it.
- **A return to another repository's issue carries the merge commit and its
  `mergedAt`.**
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

## The owner's workstation

This repository is authored on the owner's Windows workstation, with Git Bash
and Windows PowerShell 5.1 side by side. Each trap below was read there on
2026-10-12 and has been paid for at least once; re-read one before relying on
it.

- **A command handed to the owner runs in either shell.** One command per
  block; paths as `C:/...`; no `&&`, `||`, `;` chains, `$VAR`, `$(...)`,
  heredocs, `2>/dev/null` or line continuations. A command that needs Git Bash
  says so on the line above it, or comes with its PowerShell twin. A Windows
  command with `/flag` arguments (`schtasks`, `reg`) is marked as PowerShell. A
  handed-over `gh` command names `--repo <owner>/<repo>`, and a handed-over
  `ssh` command reaches one host.
- **Never run `python -`,** which reads its program from standard input. Here
  it can hang until it is killed, with an empty heredoc or `</dev/null` as its
  input, or end silently having done nothing, and a script fed to it is
  decoded as cp1252. A Claude Code hook on the workstation refuses its plain
  spellings, including those with interpreter options before the `-`
  (`python -I -`, `python -X utf8 -`), which hang the same way. It misses
  others, such as one quoted inside another command (`bash -c 'python -'`) or
  one whose `-` is on a continued line. Write a script,
  a commit message or a pull request body to a file and use it by path
  (`python <file>`, `git commit -F <file>`, `gh pr create --body-file <file>`).
  A heredoc through an agent's shell can halve backslashes, so text that holds
  one goes through a file too.
- **Git Bash rewrites path-shaped arguments.** Set `MSYS_NO_PATHCONV=1` on the
  one command that passes a `rev:path`, a `/`-leading pattern or a `/flag`.
  Never export it: it breaks `/c/...` paths for the rest of the shell. A
  rewritten argument fails quietly, so a probe through it can print an empty
  answer that looks real.
- **Git Bash `grep` cannot see a carriage return,** so a CR count from it is
  wrong in either direction. Check line endings with `git ls-files --eol` or a
  byte count in Python, and have Python write LF explicitly: bytes, or
  `newline` set to a line feed.
- **`jq` is not installed** (read 2026-10-13: `jq --version` prints an error
  in either shell, where `python --version` prints a version; `command -v jq`
  is no check in PowerShell, which runs it as `Get-Command -Verb jq` and
  prints nothing whether or not the tool exists). `gh --jq` works
  because it is built into `gh`; everything else filters with `--jq`, `grep` or
  Python.
- **Python's default encoding here is cp1252,** so Python that prints
  non-ASCII runs as `python -X utf8`. A PowerShell 5.1 `Get-Content` and
  `Set-Content` round trip turns LF into CRLF, and mangles UTF-8 once either
  side names an encoding: never round-trip a tracked file through it.
- **An agent's permission layer may refuse an act** (a secret read, an `ssh`, a
  merge, a scheduled task). Do not retry it in another form: hand the owner the
  exact command, one per block, and read the result back after it runs.

## Boundary checklist

Run over the whole of `profile/README.md` before every publish. Every answer
must be yes.

1. Is every byte of the file ASCII? Read the answer from the exit status and
   the line the check prints, never from empty output: empty is also what a
   check that never ran prints. At a prompt the line is the reading: the check
   prints `yes,` only where it exits 0, and when it never runs, the status
   left to read can belong to an earlier command. The check reads bytes, so it
   needs no locale, and each command here runs as written in Git Bash and in
   PowerShell. `grep -P` is no substitute: in a Git Bash with no locale set,
   as an agent's shell is, it exits 2 with empty output on any file, and in
   the UTF-8 locale a login shell sets, as a Git Bash window is, it passes a
   byte that is not valid UTF-8.

   Show it a no before trusting its yes. Build a file whose first line is
   plain ASCII and whose second line is byte 151, an em-dash as PowerShell
   5.1 `Set-Content` writes it:

       python -c "open('ascii-probe.txt','wb').write(bytes([97,10,151,10]))"

   Run the check on it. It must exit 1 and print a line starting `NO,` and
   ending `[2]`: the defect named, and the clean line not:

       python -c "import sys; d=open(sys.argv[1],'rb').read(); bad=[n for n,l in enumerate(d.splitlines(),1) if any(c>127 for c in l)]; ok=bool(d) and not bad; print('yes,' if ok else 'NO,',len(d),'bytes read, non-ASCII on lines',bad); sys.exit(0 if ok else 1)" ascii-probe.txt

   Delete the probe (`rm ascii-probe.txt` runs in either shell), then run the
   same check on the page. Yes is exit 0 and a line starting `yes,`. Anything
   else is a no: a `NO,` line (non-ASCII on the lines it names, or an empty
   file), a traceback, or a shell error.

       python -c "import sys; d=open(sys.argv[1],'rb').read(); bad=[n for n,l in enumerate(d.splitlines(),1) if any(c>127 for c in l)]; ok=bool(d) and not bad; print('yes,' if ok else 'NO,',len(d),'bytes read, non-ASCII on lines',bad); sys.exit(0 if ok else 1)" profile/README.md

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
