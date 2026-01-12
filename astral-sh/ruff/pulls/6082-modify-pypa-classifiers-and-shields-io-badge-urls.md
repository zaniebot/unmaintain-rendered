```yaml
number: 6082
title: Modify PyPA classifiers and Shields.io badge URLs
type: pull_request
state: merged
author: Eutropios
labels:
  - documentation
assignees: []
merged: true
base: main
head: main
created_at: 2023-07-26T01:16:53Z
updated_at: 2023-07-26T01:25:47Z
url: https://github.com/astral-sh/ruff/pull/6082
synced_at: 2026-01-12T03:30:22Z
```

# Modify PyPA classifiers and Shields.io badge URLs

---

_Pull request opened by @Eutropios on 2023-07-26 01:16_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
<!-- What's the purpose of the change? What does it do, and why? -->
Updated `pyproject.toml` classifiers from `"Development Status :: 4 - Beta"` to `"Development Status :: 5 - Production/Stable"` to reflect the transition from Beta to Full Release.
Updated the `README.md` to use `.com/astral-sh/ruff/...` instead of `.com/charliermarsh/ruff/...` in Shields.io badges to reflect the transition to a company.

## Test Plan
<!-- How was it tested? -->
Utilized the official PyPA classifiers list (located at: https://pypi.org/classifiers/)
Previewed the markdown file in different browsers on Github to ensure all badges and logos still render properly.

---

_Label `documentation` added by @charliermarsh on 2023-07-26 01:21_

---

_Comment by @charliermarsh on 2023-07-26 01:22_

Looks good to me -- thanks!

---

_Merged by @charliermarsh on 2023-07-26 01:25_

---

_Closed by @charliermarsh on 2023-07-26 01:25_

---
