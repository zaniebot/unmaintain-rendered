```yaml
number: 19297
title: "[`refurb`] Make example error out-of-the-box (`FURB122`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-4
created_at: 2025-07-11T23:16:50Z
updated_at: 2025-07-14T17:55:54Z
url: https://github.com/astral-sh/ruff/pull/19297
synced_at: 2026-01-10T18:33:12Z
```

# [`refurb`] Make example error out-of-the-box (`FURB122`)

---

_Pull request opened by @MeGaGiGaGon on 2025-07-11 23:16_

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

This PR makes [for-loop-writes (FURB122)](https://docs.astral.sh/ruff/rules/for-loop-writes/#for-loop-writes-furb122)'s example error out-of-the-box. I also had to re-name the second case's variables to get both to raise at the same time, I suspect because of limitations in ruff's current semantic model. New names subject to bikeshedding, I just went with the least effort `_b` for binary suffix.

[Old example](https://play.ruff.rs/19e8e47a-8058-4013-aef5-e9b5eab65962)
```py
with Path("file").open("w") as f:
    for line in lines:
        f.write(line)

with Path("file").open("wb") as f:
    for line in lines:
        f.write(line.encode())
```

[New example](https://play.ruff.rs/e96b00e5-3c63-47c3-996d-dace420dd711)
```py
from pathlib import Path

with Path("file").open("w") as f:
    for line in lines:
        f.write(line)

with Path("file").open("wb") as f_b:
    for line_b in lines_b:
        f_b.write(line_b.encode())
```

The "Use instead" section was also modified similarly.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-07-11 23:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @AlexWaygood on 2025-07-12 13:56_

---

_@dylwil3 approved on 2025-07-14 16:24_

LGTM thank you!

---

_Merged by @dylwil3 on 2025-07-14 16:24_

---

_Closed by @dylwil3 on 2025-07-14 16:24_

---

_Branch deleted on 2025-07-14 17:55_

---
