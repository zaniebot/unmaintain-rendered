```yaml
number: 10360
title: Update ruff pre-commit example
type: pull_request
state: closed
author: Skylion007
labels:
  - do-not-merge
assignees: []
base: main
head: patch-1
created_at: 2024-03-12T14:38:49Z
updated_at: 2024-03-13T07:45:01Z
url: https://github.com/astral-sh/ruff/pull/10360
synced_at: 2026-01-10T22:47:01Z
```

# Update ruff pre-commit example

---

_Pull request opened by @Skylion007 on 2024-03-12 14:38_

Updates ruff pre-commit example to not use deprecated ruff command

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_@charliermarsh reviewed on 2024-03-12 14:39_

---

_Review comment by @charliermarsh on `README.md`:160 on 2024-03-12 14:39_

Is this part necessary given that `--force-exclude` is in the entry? https://github.com/astral-sh/ruff-pre-commit/blob/522df5be28c6c515e78ef3c2790b9f233680f04d/.pre-commit-hooks.yaml#L4

---

_@Skylion007 reviewed on 2024-03-13 01:14_

---

_Review comment by @Skylion007 on `README.md`:160 on 2024-03-13 01:14_

I was just copying the pre-commit config. Your right it probably isn't. I'll remove the arg.

---

_Comment by @github-actions[bot] on 2024-03-13 01:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @zanieb on 2024-03-13 03:34_

This looks incorrect. 

https://github.com/astral-sh/ruff-pre-commit/blob/main/.pre-commit-hooks.yaml#L4

The `ruff` id corresponds to a `ruff check` entry already.

I don't think this needs to be changed.

---

_Label `do-not-merge` added by @zanieb on 2024-03-13 03:34_

---

_Comment by @dhruvmanila on 2024-03-13 07:45_

Yeah, this change isn't required. What you're seeing in the README is the user side of the config while the hooks are defined using the `check` subcommand.

@Skylion007 Do you have any pre-commit output where it's showing a deprecation warning?

---

_Closed by @dhruvmanila on 2024-03-13 07:45_

---
