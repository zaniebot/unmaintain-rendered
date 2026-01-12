```yaml
number: 19017
title: "[`flake8-bandit`] Make `S201` example error out-of-the-box"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-2
created_at: 2025-06-28T18:54:02Z
updated_at: 2025-06-30T18:37:00Z
url: https://github.com/astral-sh/ruff/pull/19017
synced_at: 2026-01-12T15:56:29Z
```

# [`flake8-bandit`] Make `S201` example error out-of-the-box

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

This PR makes [flask-debug-true (S201)](https://docs.astral.sh/ruff/rules/flask-debug-true/#flask-debug-true-s201)'s example error out-of-the-box

[Old example](https://play.ruff.rs/d5e1a013-1107-4223-9094-0e8393ad3c64)
```py
import flask

app = Flask()

app.run(debug=True)
```

[New example](https://play.ruff.rs/c4aebd2c-0448-4471-8bad-3e38ace68367)
```py
from flask import Flask

app = Flask()

app.run(debug=True)
```

Imports were also added to the `Use instead:` section to make it valid code out-of-the-box.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-06-28 19:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @ntBre on 2025-06-30 13:39_

---

_@ntBre approved on 2025-06-30 13:39_

Thanks!

---

_Merged by @ntBre on 2025-06-30 13:39_

---

_Closed by @ntBre on 2025-06-30 13:39_

---

_Branch deleted on 2025-06-30 18:37_

---
