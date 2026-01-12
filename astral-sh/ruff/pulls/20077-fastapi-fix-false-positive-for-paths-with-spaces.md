```yaml
number: 20077
title: "[`fastapi`] Fix false positive for paths with spaces around parameters (`FAST003`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-20060
created_at: 2025-08-25T04:55:57Z
updated_at: 2025-08-29T14:30:51Z
url: https://github.com/astral-sh/ruff/pull/20077
synced_at: 2026-01-12T15:56:54Z
```

# [`fastapi`] Fix false positive for paths with spaces around parameters (`FAST003`)

---

_@danparizher_

## Summary

Fixes #20060


---

_Comment by @github-actions[bot] on 2025-08-25 05:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:495 on 2025-08-25 12:13_

I don't think this is correct - trim removes _any_ unicode whitespace, not just ordinary spaces. Why not just remove `.trim` and leave everything else the same?

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:497 on 2025-08-25 12:14_

I think this check already occurs elsewhere https://github.com/astral-sh/ruff/blob/376e3ff39519d3c0563dd8f7a1d0daf9db52e98a/crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs#L167-L172

---

_@dylwil3 requested changes on 2025-08-25 12:17_

Thanks for working on this! I think it needs a few tweaks

---

_Label `bug` added by @ntBre on 2025-08-25 12:46_

---

_Label `rule` added by @ntBre on 2025-08-25 12:46_

---

_Comment by @dscorbett on 2025-08-25 12:58_

I recommend making FAST003 only apply to [parameters as recognized by FastAPI](https://github.com/encode/starlette/blob/0.47.3/starlette/routing.py#L121), instead of just checking for leading and trailing ASCII spaces.

---

_Comment by @dylwil3 on 2025-08-25 13:25_

> I recommend making FAST003 only apply to [parameters as recognized by FastAPI](https://github.com/encode/starlette/blob/0.47.3/starlette/routing.py#L121), instead of just checking for leading and trailing ASCII spaces.

I think if we avoid the trim then this is basically true (modulo reserved keywords) since we check whether we have an identifier (as pointed out above). So I don't think we need to port the regex over.

---

_Comment by @dscorbett on 2025-08-25 14:46_

I don’t think it is true, even ignoring keywords, but I can open another issue later since it’s not directly related to this one.

---

_Review requested from @dylwil3 by @danparizher on 2025-08-25 16:49_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:494 on 2025-08-25 17:35_

Looks like we forgot to remove this part

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_unused_path_parameter.rs`:498 on 2025-08-25 17:35_

Do we need this?

---

_@dylwil3 requested changes on 2025-08-25 17:35_

---

_Review requested from @dylwil3 by @danparizher on 2025-08-26 23:11_

---

_@dylwil3 approved on 2025-08-29 13:37_

Thank you!

---

_Merged by @dylwil3 on 2025-08-29 13:40_

---

_Closed by @dylwil3 on 2025-08-29 13:40_

---

_Branch deleted on 2025-08-29 14:30_

---
