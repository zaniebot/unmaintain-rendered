```yaml
number: 18978
title: "[`flake8-async`] Make `ASYNC220`, `ASYNC221`, and `ASYNC222` examples error out-of-the-box"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-3
created_at: 2025-06-27T07:27:14Z
updated_at: 2025-07-02T19:59:20Z
url: https://github.com/astral-sh/ruff/pull/18978
synced_at: 2026-01-12T15:56:29Z
```

# [`flake8-async`] Make `ASYNC220`, `ASYNC221`, and `ASYNC222` examples error out-of-the-box

---

_@MeGaGiGaGon_

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

All three in one PR since they are in the same file.

This PR makes [create-subprocess-in-async-function (ASYNC220)](https://docs.astral.sh/ruff/rules/create-subprocess-in-async-function/#create-subprocess-in-async-function-async220)'s example error out-of-the-box

[Old example](https://play.ruff.rs/465036af-d75f-4bda-ba24-e50e8618bf16)
```py
async def foo():
    os.popen(cmd)
```

[New example](https://play.ruff.rs/8cf43d50-f9e1-45d6-b711-968c7135f2e0)
```py
import os


async def foo():
    os.popen(cmd)
```

Imports were also added to the `Use instead:` section to make it valid code out-of-the-box.

This PR makes [run-process-in-async-function (ASYNC221)](https://docs.astral.sh/ruff/rules/run-process-in-async-function/#run-process-in-async-function-async221)'s example error out-of-the-box

[Old example](https://play.ruff.rs/0698aaa1-c722-4f04-b56c-61edec06945c)
```py
async def foo():
    subprocess.run(cmd)
```

[New example](https://play.ruff.rs/e05bfcbc-e681-4a28-8f50-2c0c2537d038)
```py
import subprocess


async def foo():
    subprocess.run(cmd)
```

Imports were also added to the `Use instead:` section to make it valid code out-of-the-box.

This PR makes [wait-for-process-in-async-function (ASYNC222)](https://docs.astral.sh/ruff/rules/wait-for-process-in-async-function/#wait-for-process-in-async-function-async222)'s example error out-of-the-box

[Old example](https://play.ruff.rs/4305d477-8995-462d-83ae-435731d71e67)
```py
async def foo():
    os.waitpid(0)
```

[New example](https://play.ruff.rs/ad10c042-3b18-49ca-8f5c-5ab720516da1)
```py
import os


async def foo():
    os.waitpid(0)
```

Imports were also added to the `Use instead:` section to make it valid code out-of-the-box.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-06-27 07:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Assigned to @ntBre by @MichaReiser on 2025-06-27 17:36_

---

_Review requested from @ntBre by @MichaReiser on 2025-06-27 17:36_

---

_Label `documentation` added by @ntBre on 2025-06-30 13:46_

---

_@ntBre approved on 2025-06-30 13:47_

---

_Merged by @ntBre on 2025-06-30 13:47_

---

_Closed by @ntBre on 2025-06-30 13:47_

---

_Branch deleted on 2025-07-02 19:59_

---
