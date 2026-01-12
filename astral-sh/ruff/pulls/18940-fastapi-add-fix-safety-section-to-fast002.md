```yaml
number: 18940
title: "[`FastAPI`] Add fix safety section to `FAST002`"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-06-25T18:26:11Z
updated_at: 2025-07-02T19:59:21Z
url: https://github.com/astral-sh/ruff/pull/18940
synced_at: 2026-01-12T15:56:28Z
```

# [`FastAPI`] Add fix safety section to `FAST002`

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

Part of #15584

This PR adds a fix safety section to [fast-api-non-annotated-dependency (FAST002)](https://docs.astral.sh/ruff/rules/fast-api-non-annotated-dependency/#fast-api-non-annotated-dependency-fast002). It also re-words the availability section since I found it confusing.

The lint/fix was added in #11579 as always unsafe.
No reasoning is given in the original PR/code as to why this was chosen.
Example of why the fix is unsafe:
https://play.ruff.rs/3bd0566e-1ef6-4cec-ae34-3b07cd308155
```py
from fastapi import Depends, FastAPI, Query

app = FastAPI()

# Fix will remove the parameter default value
@app.get("/items/")
async def read_items(commons: dict = Depends(common_parameters)):
    return commons

# Fix will delete comment and change default parameter value
@app.get("/items/")
async def read_items_1(q: str = Query(  # This comment will be deleted
    default="rick")):
    return q
```
After fixing both instances of `FAST002`:
```py
from fastapi import Depends, FastAPI, Query
from typing import Annotated

app = FastAPI()

# Fix will remove the parameter default value
@app.get("/items/")
async def read_items(commons: Annotated[dict, Depends(common_parameters)]):
    return commons

# Fix will delete comment and change default parameter value
@app.get("/items/")
async def read_items_1(q: Annotated[str, Query()] = "rick"):
    return q
```

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-06-25 18:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @AlexWaygood on 2025-06-25 21:28_

---

_Review requested from @dylwil3 by @MichaReiser on 2025-06-26 06:58_

---

_@dylwil3 approved on 2025-06-26 17:37_

Thank you!

---

_Merged by @dylwil3 on 2025-06-26 17:38_

---

_Closed by @dylwil3 on 2025-06-26 17:38_

---

_Branch deleted on 2025-07-02 19:59_

---
