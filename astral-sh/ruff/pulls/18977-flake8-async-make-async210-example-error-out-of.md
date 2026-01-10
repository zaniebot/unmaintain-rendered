```yaml
number: 18977
title: "[`flake8-async`] Make `ASYNC210` example error out-of-the-box"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-2
created_at: 2025-06-27T07:12:24Z
updated_at: 2025-06-28T15:20:34Z
url: https://github.com/astral-sh/ruff/pull/18977
synced_at: 2026-01-10T18:39:09Z
```

# [`flake8-async`] Make `ASYNC210` example error out-of-the-box

---

_Pull request opened by @MeGaGiGaGon on 2025-06-27 07:12_

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

This PR makes [blocking-http-call-in-async-function (ASYNC210)](https://docs.astral.sh/ruff/rules/blocking-http-call-in-async-function/#blocking-http-call-in-async-function-async210)'s example error out-of-the-box

[Old example](https://play.ruff.rs/20cba4f4-fe2f-428a-a721-311d1a081e64)
```py
async def fetch():
    urllib.request.urlopen("https://example.com/foo/bar").read()
```

[New example](https://play.ruff.rs/5ca2a10d-5294-49ee-baee-0447f7188d9b)
```py
import urllib


async def fetch():
    urllib.request.urlopen("https://example.com/foo/bar").read()
```

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-06-27 07:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @dylwil3 by @MichaReiser on 2025-06-27 17:36_

---

_Label `documentation` added by @ntBre on 2025-06-27 17:38_

---

_@dylwil3 approved on 2025-06-28 15:11_

Thanks!

---

_Merged by @dylwil3 on 2025-06-28 15:11_

---

_Closed by @dylwil3 on 2025-06-28 15:11_

---

_Branch deleted on 2025-06-28 15:20_

---
