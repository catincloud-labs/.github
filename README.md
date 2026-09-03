# .github

This repository carries the organisation profile page for `catincloud-labs`
and the furniture needed to check it. Nothing else.

The page is `profile/README.md`. GitHub renders it on the organisation's
landing page; this file is not shown there.

Deliberately absent: there is no default community health file here (no
`CODE_OF_CONDUCT`, `CONTRIBUTING`, `SECURITY`, `SUPPORT`, funding file, issue
or pull request template) and no `workflow-templates/`. A file in those
positions in a repository with this name is inherited by every repository in
the organisation that lacks its own, silently. That absence is a decision, not
an omission, and `AGENTS.md` names it as the rule that matters most here.

To propose a change: branch, edit, open a pull request. Two checks are
required, named `markdown` and `closing keywords`; three more are advisory.
Enable the local commit-msg hook once per clone so the second required check
fails on your machine rather than in CI:

    git config core.hooksPath .githooks

The page is reviewed in full against the boundary checklist in `AGENTS.md`
before every publish. Everything authored here is ASCII with LF line endings.

Copyright 2026 David Anaya. Apache-2.0.
