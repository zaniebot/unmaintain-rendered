```yaml
number: 15404
title: Remove pygments pin
type: pull_request
state: merged
author: calumy
labels:
  - documentation
assignees: []
merged: true
base: main
head: remove-pygments-pin
created_at: 2025-01-10T16:25:05Z
updated_at: 2025-01-11T10:15:34Z
url: https://github.com/astral-sh/ruff/pull/15404
synced_at: 2026-01-12T15:55:51Z
```

# Remove pygments pin

---

_@calumy_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The recent release of Pygments ([2.19.1](https://github.com/pygments/pygments/releases/tag/2.19.1)) allows the pinned version to be removed as the PYI alias for Python syntax highlighting has been removed.

## Test Plan

- Follow the steps outlined in https://github.com/astral-sh/ruff/blob/main/CONTRIBUTING.md#mkdocs to get the documentation site running locally.
- Spot test rules pages that have PYI code blocks to ensure that syntax highlighting remains e.g. [http://127.0.0.1:8000/ruff/rules/complex-if-statement-in-stub/](http://127.0.0.1:8000/ruff/rules/complex-if-statement-in-stub/).

**Note:** I am unable to test the insiders build but would assume that it functions locally as I do not have access to MkDocs Insiders, but I would like to assume that it functions in the same way as the non-insiders build.


---

_@charliermarsh approved on 2025-01-10 16:56_

---

_Merged by @charliermarsh on 2025-01-10 17:15_

---

_Closed by @charliermarsh on 2025-01-10 17:15_

---

_Label `documentation` added by @AlexWaygood on 2025-01-11 10:15_

---
