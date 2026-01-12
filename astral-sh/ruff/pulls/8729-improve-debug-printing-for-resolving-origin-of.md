```yaml
number: 8729
title: Improve debug printing for resolving origin of config settings
type: pull_request
state: merged
author: chan-vince
labels:
  - cli
assignees: []
merged: true
base: main
head: improve-config-file-debug-prints
created_at: 2023-11-17T00:30:33Z
updated_at: 2023-11-17T01:16:50Z
url: https://github.com/astral-sh/ruff/pull/8729
synced_at: 2026-01-10T23:40:55Z
```

# Improve debug printing for resolving origin of config settings

---

_Pull request opened by @chan-vince on 2023-11-17 00:30_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

When running ruff in verbose mode with `-v`, the first debug logs show where the config settings are taken from. For example:
```
❯ ruff check ./some_file.py -v
[2023-11-17][00:16:25][ruff_cli::resolve][DEBUG] Using pyproject.toml (parent) at /Users/vince/demo/ruff.toml
```

This threw me off for a second because I knew I had no python project there, and therefore no `pyproject.toml` file. Then I realised it was actually reading a `ruff.toml` file (obvious when you read the whole print I suppose) and that the pyproject.toml is a hardcoded string in the debug log.

I think it would be nice to tweak the wording slightly so it is clear that the settings don't neccessarily have to come from a `pyproject.toml` file.

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-11-17 00:42_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2023-11-17 01:04_

Thank you!

---

_Label `cli` added by @charliermarsh on 2023-11-17 01:05_

---

_Merged by @charliermarsh on 2023-11-17 01:10_

---

_Closed by @charliermarsh on 2023-11-17 01:10_

---
