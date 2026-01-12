```yaml
number: 16043
title: Root exclusions in the server to project root
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: dhruv/exclude-root
created_at: 2025-02-08T17:59:44Z
updated_at: 2025-02-10T05:00:40Z
url: https://github.com/astral-sh/ruff/pull/16043
synced_at: 2026-01-12T15:55:53Z
```

# Root exclusions in the server to project root

---

_@dhruvmanila_

## Summary

fixes: #16041 

## Test Plan

Using the [project](https://github.com/bwcc-clan/polebot) in the linked issue:

Notice how the project "polebot" is in the "play" directory which is included in the `exclude` setting as:

```toml
exclude = ["play"]
```

**Before this fix**

```
DEBUG ruff:worker:0 ruff_server::resolve: Ignored path via `exclude`: /private/tmp/ruff-test/play/polebot/src/utils/log_tools.py
```

**After this fix**

```
DEBUG ruff:worker:2 ruff_server::resolve: Included path via `include`: /private/tmp/ruff-test/play/polebot/src/utils/log_tools.py
```

I also updated the same project to remove the "play" directory from the `exclude` setting and made sure that anything under the `polebot/play` directory is included:

```
DEBUG  ruff:worker:4 ruff_server::resolve: Included path via `include`: /private/tmp/ruff-test/play/polebot/play/test.py
```

And, excluded when I add the directory back:

```
DEBUG  ruff:worker:2 ruff_server::resolve: Ignored path via `exclude`: /private/tmp/ruff-test/play/polebot/play/test.py
```

---

_Label `bug` added by @dhruvmanila on 2025-02-08 17:59_

---

_Label `server` added by @dhruvmanila on 2025-02-08 17:59_

---

_Renamed from "Directly include Settings struct for the server" to "Root exclusions in the server to project root" by @dhruvmanila on 2025-02-08 17:59_

---

_Marked ready for review by @dhruvmanila on 2025-02-08 18:05_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-02-08 18:05_

---

_Comment by @github-actions[bot] on 2025-02-08 18:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-02-09 14:04_

---

_@MichaReiser reviewed on 2025-02-09 14:04_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/resolve.rs`:64 on 2025-02-09 14:04_

Nit: Might be worth to just pass &resolver_settings`

---

_Merged by @dhruvmanila on 2025-02-10 04:57_

---

_Closed by @dhruvmanila on 2025-02-10 04:57_

---

_Branch deleted on 2025-02-10 04:57_

---
