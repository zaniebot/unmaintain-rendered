```yaml
number: 11745
title: "`ruff server`: Formatting a document with syntax problems no longer spams a visible error popup"
type: pull_request
state: merged
author: snowsignal
labels:
  - server
assignees: []
merged: true
base: main
head: jane/server/error-spam
created_at: 2024-06-04T22:41:26Z
updated_at: 2024-06-05T00:18:22Z
url: https://github.com/astral-sh/ruff/pull/11745
synced_at: 2026-01-12T15:55:38Z
```

# `ruff server`: Formatting a document with syntax problems no longer spams a visible error popup

---

_@snowsignal_

## Summary

Fixes https://github.com/astral-sh/ruff-vscode/issues/482.

I've made adjustments to `format` and `format_range` that handle parsing errors before they become server errors. We'll still log this as a problem, but there will no longer be a visible popup.

## Test Plan

Instead of seeing a visible error when formatting a document with syntax issues, you should see this warning in the LSP logs:

<img width="991" alt="Screenshot 2024-06-04 at 3 38 23 PM" src="https://github.com/astral-sh/ruff/assets/19577865/9d68947d-6462-4ca6-ab5a-65e573c91db6">

Similarly, if you try to format a range with syntax issues, you should see this warning in the LSP logs instead of a visible error popup:

<img width="1010" alt="Screenshot 2024-06-04 at 3 39 10 PM" src="https://github.com/astral-sh/ruff/assets/19577865/99fff098-798d-406a-976e-81ead0da0352">




---

_Label `server` added by @snowsignal on 2024-06-04 22:41_

---

_Comment by @codspeed-hq[bot] on 2024-06-04 22:45_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/jane/server/error-spam)

### Merging #11745 will **not alter performance**

<sub>Comparing <code>jane/server/error-spam</code> (9293364) with <code>main</code> (d056d09)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-06-04 22:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@zanieb reviewed on 2024-06-04 23:25_

---

_Review comment by @zanieb on `crates/ruff_server/src/format.rs`:18 on 2024-06-04 23:25_

Nit: reword for clarity?

```suggestion
        // Special case - syntax/parse errors are be handled here instead of
        // being propagated as visible server errors.
```

---

_@charliermarsh reviewed on 2024-06-04 23:30_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/format.rs`:41 on 2024-06-04 23:30_

Should we just return `Ok(None)` here, so that we avoid cloning the source text and applying a full no-op edit?

---

_@snowsignal reviewed on 2024-06-04 23:32_

---

_Review comment by @snowsignal on `crates/ruff_server/src/format.rs`:41 on 2024-06-04 23:32_

Sure, I can re-work the return type to be optional 👍 

---

_@zanieb reviewed on 2024-06-04 23:33_

---

_Review comment by @zanieb on `crates/ruff_server/src/format.rs`:37 on 2024-06-04 23:33_

Just fyi I only commented on one instance of this

---

_Review requested from @zanieb by @snowsignal on 2024-06-04 23:42_

---

_Review requested from @charliermarsh by @snowsignal on 2024-06-04 23:42_

---

_@charliermarsh approved on 2024-06-05 00:16_

---

_Merged by @snowsignal on 2024-06-05 00:18_

---

_Closed by @snowsignal on 2024-06-05 00:18_

---

_Branch deleted on 2024-06-05 00:18_

---
