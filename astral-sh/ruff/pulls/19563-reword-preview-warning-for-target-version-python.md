```yaml
number: 19563
title: "Reword preview warning for `target-version` Python 3.14"
type: pull_request
state: merged
author: dylwil3
labels:
  - python314
assignees: []
merged: true
base: main
head: reword-py314-warning
created_at: 2025-07-25T19:55:13Z
updated_at: 2025-07-25T21:09:46Z
url: https://github.com/astral-sh/ruff/pull/19563
synced_at: 2026-01-12T15:56:42Z
```

# Reword preview warning for `target-version` Python 3.14

---

_@dylwil3_

Small rewording to indicate that core development is done but that we may add breaking changes.

Feel free to bikeshed!

Test:

```console
❯ echo "t''" | cargo run -p ruff -- check --no-cache --isolated --target-version py314 -
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/ruff check --no-cache --isolated --target-version py314 -`
warning: Support for Python 3.14 is in preview and may undergo breaking changes. Enable `preview` to remove this warning.
All checks passed!
```


---

_Label `python314` added by @dylwil3 on 2025-07-25 19:55_

---

_Comment by @github-actions[bot] on 2025-07-25 20:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-07-25 20:35_

---

_Merged by @dylwil3 on 2025-07-25 21:09_

---

_Closed by @dylwil3 on 2025-07-25 21:09_

---
