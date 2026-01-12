```yaml
number: 11524
title: "Remove empty strings when converting to f-string (`UP032`)"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - rule
assignees: []
merged: true
base: main
head: dhruv/remove-empty-string
created_at: 2024-05-24T04:31:45Z
updated_at: 2024-05-27T05:15:01Z
url: https://github.com/astral-sh/ruff/pull/11524
synced_at: 2026-01-12T15:55:38Z
```

# Remove empty strings when converting to f-string (`UP032`)

---

_@dhruvmanila_

## Summary

This PR brings back the functionality to remove empty strings when converting to an f-string in `UP032`.

For context, https://github.com/astral-sh/ruff/pull/8712 added this functionality to remove _trailing_ empty strings but it got removed in https://github.com/astral-sh/ruff/pull/8697 possibly unexpectedly so.

There's one difference which is that this PR will remove _any_ empty strings and not just trailing ones. For example,

```diff
--- /Users/dhruv/playground/ruff/src/UP032.py
+++ /Users/dhruv/playground/ruff/src/UP032.py
@@ -1,7 +1,5 @@
 (
-    "{a}"
-    ""
-    "{b}"
-    ""
-).format(a=1, b=1)
+    f"{1}"
+    f"{1}"
+)
```

## Test Plan

Run `cargo insta test` and update the snapshots.


---

_@dhruvmanila reviewed on 2024-05-24 04:32_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pyupgrade/rules/f_strings.rs`:478 on 2024-05-24 04:32_

The main reason for this PR is to remove this little lexer usage which uses the owned data from `String` token.

---

_Review requested from @charliermarsh by @dhruvmanila on 2024-05-24 04:32_

---

_Label `rule` added by @dhruvmanila on 2024-05-24 04:32_

---

_Comment by @github-actions[bot] on 2024-05-24 04:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @snowsignal on `crates/ruff_linter/src/rules/pyupgrade/rules/f_strings.rs`:222 on 2024-05-24 15:59_

Nit: It may be useful to clarify the new condition of this variant.
```suggestion
    /// The format string only contains literal parts and is non-empty.
    Literal,
```

---

_@snowsignal approved on 2024-05-24 16:07_

---

_Renamed from "Remove empty strings when converting to f-string" to "Remove empty strings when converting to f-string (`UP032`)" by @dhruvmanila on 2024-05-27 05:00_

---

_Merged by @dhruvmanila on 2024-05-27 05:05_

---

_Closed by @dhruvmanila on 2024-05-27 05:05_

---

_Branch deleted on 2024-05-27 05:05_

---
