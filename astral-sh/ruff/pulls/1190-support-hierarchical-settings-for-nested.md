```yaml
number: 1190
title: Support hierarchical settings for nested directories
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/configuration-i
created_at: 2022-12-11T03:41:04Z
updated_at: 2022-12-12T15:12:25Z
url: https://github.com/astral-sh/ruff/pull/1190
synced_at: 2026-01-12T15:55:05Z
```

# Support hierarchical settings for nested directories

---

_@charliermarsh_

The goal of this PR is to introduce hierarchical settings detection for Ruff, similar to the ESLint model, such that when we go to lint a given file, we always use the `pyproject.toml` file "closest" to that file in the filesystem hierarchy.

This has a number of nice side effects -- namely, linting a file becomes independent of the path from which the linter was invoked, and independent of the list of files passed to the `ruff` invocation (neither of which are true today).

(There's one caveat here which is that the `fix` and `format` `pyproject.toml` options, which are semantically part of the _invocation_, aren't going to be inherited -- so we'll use the current working directory to find the `pyproject.toml` to power those two, specific options.)

Resolves #1069.


---

_Converted to draft by @charliermarsh on 2022-12-11 03:41_

---

_Marked ready for review by @charliermarsh on 2022-12-11 03:54_

---

_Comment by @charliermarsh on 2022-12-11 03:56_

There's a small performance hit here from doing two filesystem traversals. It might be possible to avoid it, since we could in theory track the "current" `pyproject.toml` as we iterate over the Python files (we need to know the current settings in order to power the `excludes` behavior).

---

_Merged by @charliermarsh on 2022-12-12 15:12_

---

_Closed by @charliermarsh on 2022-12-12 15:12_

---

_Branch deleted on 2022-12-12 15:12_

---
