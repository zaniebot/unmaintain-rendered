```yaml
number: 20565
title: "Use `Annotation::tags` instead of hardcoded rule matching in ruff server"
type: pull_request
state: merged
author: danparizher
labels:
  - internal
  - server
assignees: []
merged: true
base: main
head: fix-20532
created_at: 2025-09-25T05:39:02Z
updated_at: 2025-09-26T10:50:08Z
url: https://github.com/astral-sh/ruff/pull/20565
synced_at: 2026-01-10T17:40:28Z
```

# Use `Annotation::tags` instead of hardcoded rule matching in ruff server

---

_Pull request opened by @danparizher on 2025-09-25 05:39_

## Summary

Fixes #20532

## Test Plan

Tested the specific rules by creating a test file, verified the rules are still identified


---

_Comment by @github-actions[bot] on 2025-09-25 05:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser requested changes on 2025-09-25 07:45_

Thank you

The changes in the diagnostic generation code look correct to me but we also need to set the tags when creating the `F401`, `F841`, and `RUF059` diagnostics, similar to how we do it for deprecated in ty 

https://github.com/astral-sh/ruff/blob/edeb45804ef9dbe348d393f3ab77288062a3c77e/crates/ty_python_semantic/src/types/infer/builder.rs#L6243

That also makes me believe that you used the Ruff version bundled with VS code (or local to your project) when testing the change. 

To test the change, set the `ruff.path` setting in VS Code to `<ruff_repository>/target/debug/ruff`. You then need to build ruff with `cargo build --bin ruff` and restart (or reload) VS Code. 

There are two things you can do to verify your test setup is working as expected:

1. Add a `tracing::warn!("Hy from Ruff");` to the `tags` function. You should see this message in Ruff's output panel (if it is using the correct version)
2. F401 etc. shouldn't be highlighted as `Unnecessary` with the version from this PR.

---

_Label `internal` added by @MichaReiser on 2025-09-25 07:45_

---

_Label `server` added by @MichaReiser on 2025-09-25 07:45_

---

_Comment by @danparizher on 2025-09-25 15:03_

Thanks for the tips, I'll try these out today!

---

_Comment by @danparizher on 2025-09-26 02:40_

I was able to test it out with the config you suggested and added the tags in. Let me know if it looks good and if you get the same results!

---

_Review requested from @MichaReiser by @danparizher on 2025-09-26 02:45_

---

_@MichaReiser approved on 2025-09-26 07:06_

---

_Merged by @MichaReiser on 2025-09-26 07:06_

---

_Closed by @MichaReiser on 2025-09-26 07:06_

---

_Comment by @MichaReiser on 2025-09-26 07:06_

Thank you. This is looking great. 

---

_Branch deleted on 2025-09-26 10:50_

---
