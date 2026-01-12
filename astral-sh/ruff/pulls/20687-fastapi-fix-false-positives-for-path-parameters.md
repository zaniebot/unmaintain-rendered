```yaml
number: 20687
title: "[`fastapi`] Fix false positives for path parameters that FastAPI doesn't recognize (`FAST003`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-20680
created_at: 2025-10-02T22:46:38Z
updated_at: 2025-10-09T20:58:11Z
url: https://github.com/astral-sh/ruff/pull/20687
synced_at: 2026-01-12T15:57:07Z
```

# [`fastapi`] Fix false positives for path parameters that FastAPI doesn't recognize (`FAST003`)

---

_@danparizher_

## Summary

Fixes #20680


---

_Comment by @github-actions[bot] on 2025-10-02 22:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dscorbett on 2025-10-03 00:55_

`is_fastapi_valid_path_parameter` doesn’t exactly match [the regex FastAPI uses](https://github.com/Kludex/starlette/blob/0.48.0/starlette/routing.py#L121).

---

_Comment by @ntBre on 2025-10-03 13:38_

It makes sense to me just to use the same regex. It looks like they just iterate over the regex matches to extract parameters too, which might help us simplify `PathParamIterator` as well.

---

_Label `bug` added by @ntBre on 2025-10-03 13:38_

---

_Label `rule` added by @ntBre on 2025-10-03 13:38_

---

_@ntBre approved on 2025-10-09 19:58_

Thanks! I pushed one commit pushing the refactor a bit further. Now that we're using a regex, we can just use `Regex::captures_iter`. I also added a link to the upstream regex for easier reference in the future.

---

_Merged by @ntBre on 2025-10-09 20:10_

---

_Closed by @ntBre on 2025-10-09 20:10_

---

_Branch deleted on 2025-10-09 20:58_

---
