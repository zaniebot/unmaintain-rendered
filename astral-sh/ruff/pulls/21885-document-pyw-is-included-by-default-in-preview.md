```yaml
number: 21885
title: "Document `*.pyw` is included by default in preview"
type: pull_request
state: merged
author: Avasam
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-12-10T04:07:11Z
updated_at: 2025-12-10T16:52:45Z
url: https://github.com/astral-sh/ruff/pull/21885
synced_at: 2026-01-10T16:42:11Z
```

# Document `*.pyw` is included by default in preview

---

_Pull request opened by @Avasam on 2025-12-10 04:07_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Document `*.pyw` is included by default.
Originally requested in https://github.com/astral-sh/ruff/issues/13246 and added in https://github.com/astral-sh/ruff/pull/20458

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_@Avasam reviewed on 2025-12-10 05:45_

---

_Review comment by @Avasam on `docs/configuration.md`:354 on 2025-12-10 05:45_

Oh hang on, it's only in preview.

What's your policy then? I though this was forgotten, but maybe it's not documented on purpose.

```suggestion
By default, Ruff will discover files matching `*.py`, `*.pyi`, `*.ipynb`, or `pyproject.toml`.
In [preview](preview.md) mode, Ruff will also discover `*.pyw` by default.
```

---

_Renamed from "Document `*.pyw` is included by default" to "Document `*.pyw` is included by default in preview" by @Avasam on 2025-12-10 05:54_

---

_Review requested from @amyreese by @MichaReiser on 2025-12-10 07:07_

---

_Label `documentation` added by @MichaReiser on 2025-12-10 07:07_

---

_@amyreese approved on 2025-12-10 16:41_

---

_Comment by @amyreese on 2025-12-10 16:42_

Thank you! I did indeed miss adding that to the documentation, and I think it makes sense to mention that it's currently restricted to preview mode.

---

_Merged by @amyreese on 2025-12-10 16:43_

---

_Closed by @amyreese on 2025-12-10 16:43_

---

_Branch deleted on 2025-12-10 16:52_

---
