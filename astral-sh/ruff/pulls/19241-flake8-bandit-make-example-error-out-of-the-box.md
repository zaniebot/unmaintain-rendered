```yaml
number: 19241
title: "[`flake8-bandit`] Make example error out-of-the-box (`S412`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-07-09T17:43:47Z
updated_at: 2025-07-11T12:26:41Z
url: https://github.com/astral-sh/ruff/pull/19241
synced_at: 2026-01-10T18:33:12Z
```

# [`flake8-bandit`] Make example error out-of-the-box (`S412`)

---

_Pull request opened by @MeGaGiGaGon on 2025-07-09 17:43_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #18972

This PR makes [suspicious-httpoxy-import (S412)](https://docs.astral.sh/ruff/rules/suspicious-httpoxy-import/#suspicious-httpoxy-import-s412)'s example error out-of-the-box. Since the checked imports are classes instead of modules, the example isn't valid. See #19009 for more details
```
PS ~>py -c "import wsgiref.handlers.CGIHandler"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
    import wsgiref.handlers.CGIHandler
ModuleNotFoundError: No module named 'wsgiref.handlers.CGIHandler'; 'wsgiref.handlers' is not a package
PS ~>py -c "from wsgiref.handlers import CGIHandler"
PS ~>
```

[Old example](https://play.ruff.rs/bf48c901-6a46-4795-ba1d-c6af79d5c96e)
```py
import wsgiref.handlers.CGIHandler
```

[New example](https://play.ruff.rs/1f0e1e60-1f0f-484a-9a17-2d0290a68f2a)
```py
from wsgiref.handlers import CGIHandler
```

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-07-09 17:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-07-09 18:23_

Thanks!

How'd you install `wsgiref`? I was trying with `uv` while writing up #19242, but it was complaining about `print` statements without parentheses 😆 

---

_Merged by @ntBre on 2025-07-09 18:25_

---

_Closed by @ntBre on 2025-07-09 18:25_

---

_Branch deleted on 2025-07-09 18:25_

---

_Label `documentation` added by @ntBre on 2025-07-11 12:26_

---
