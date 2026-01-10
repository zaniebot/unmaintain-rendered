```yaml
number: 2302
title: "Fix `--no-respect-ignore-files` typo in CLI documentation"
type: pull_request
state: closed
author: evroon
labels: []
assignees: []
base: main
head: patch-1
created_at: 2026-01-02T13:15:20Z
updated_at: 2026-01-02T13:48:40Z
url: https://github.com/astral-sh/ty/pull/2302
synced_at: 2026-01-10T02:34:11Z
```

# Fix `--no-respect-ignore-files` typo in CLI documentation

---

_Pull request opened by @evroon on 2026-01-02 13:15_

<!--
Thank you for contributing to ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
`ty check --help` returns this:

```
File selection:
      --respect-ignore-files
          Respect file exclusions via `.gitignore` and other standard ignore files. Use `--no-respect-gitignore` to disable
```

However, the option `--no-respect-gitignore` does not exist, it should be `--no-respect-ignore-files` instead.

## Test Plan

I am not sure testing is needed? I only fixed a typo

---

_Comment by @AlexWaygood on 2026-01-02 13:44_

Thanks! Unfortunately this file is generated from code in the ruff repo.

And it looks like this was fixed a couple of days ago, in https://github.com/astral-sh/ruff/commit/4f2529f353e3ec2f9382a0258952c9c58b08ce89! So this should be fixed automatically when we next run the release script to regenerate this file and cut a new release.

Thank you!

---

_Closed by @AlexWaygood on 2026-01-02 13:44_

---

_Branch deleted on 2026-01-02 13:48_

---
