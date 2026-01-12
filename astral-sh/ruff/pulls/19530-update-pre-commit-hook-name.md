```yaml
number: 19530
title: Update pre-commit hook name
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/update-pre-commit-name
created_at: 2025-07-24T13:23:18Z
updated_at: 2025-07-24T13:44:49Z
url: https://github.com/astral-sh/ruff/pull/19530
synced_at: 2026-01-12T15:56:41Z
```

# Update pre-commit hook name

---

_@ntBre_

## Summary

A couple of months ago now (https://github.com/astral-sh/ruff-pre-commit/pull/124) we changed the hook ID from just `ruff` to `ruff-check` to mirror `ruff-format`. I noticed the `ruff (legacy alias)` when running pre-commit on the release today and realized we should probably update.

## Test Plan

Commit on this PR:

```shell
> git commit -m "Update pre-commit hook name"
check for merge conflicts................................................Passed
Validate pyproject.toml..............................(no files to check)Skipped
mdformat.............................................(no files to check)Skipped
markdownlint-fix.....................................(no files to check)Skipped
blacken-docs.........................................(no files to check)Skipped
typos....................................................................Passed
cargo fmt............................................(no files to check)Skipped
ruff format..........................................(no files to check)Skipped
ruff check...........................................(no files to check)Skipped  <-- 
prettier.................................................................Passed
zizmor...............................................(no files to check)Skipped
Validate GitHub Workflows............................(no files to check)Skipped
shellcheck...........................................(no files to check)Skipped
```

Compared to the release branch:

```shell
> pre-commit run
...
cargo fmt............................................(no files to check)Skipped
ruff format..........................................(no files to check)Skipped
ruff (legacy alias)..................................(no files to check)Skipped
...
```

---

_Label `internal` added by @ntBre on 2025-07-24 13:23_

---

_@AlexWaygood approved on 2025-07-24 13:43_

---

_Merged by @ntBre on 2025-07-24 13:44_

---

_Closed by @ntBre on 2025-07-24 13:44_

---

_Branch deleted on 2025-07-24 13:44_

---
