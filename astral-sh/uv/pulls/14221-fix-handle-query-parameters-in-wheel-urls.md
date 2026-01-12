```yaml
number: 14221
title: "Fix: Handle query parameters in wheel URLs"
type: pull_request
state: closed
author: esafak
labels: []
assignees: []
base: main
head: fix/wheel-url-query-params
created_at: 2025-06-23T16:57:09Z
updated_at: 2025-06-23T18:52:20Z
url: https://github.com/astral-sh/uv/pull/14221
synced_at: 2026-01-12T16:11:05Z
```

# Fix: Handle query parameters in wheel URLs

---

_@esafak_

## Summary

The `WheelFilename::try_from<&Url>` implementation was previously passing the full last path segment of a URL (including any query parameters) to `WheelFilename::from_str`. This caused `from_str` to fail when trying to strip the `.whl` suffix because the query parameters were present at the end of the string.

This commit modifies `try_from<&Url>` to correctly strip any query parameters from the extracted filename before attempting to parse it. It achieves this by finding the last `?` in the path segment and taking the substring before it.

Fixes #14217 

## Test Plan

Updated tests to include URLs with query parameters, ensuring the fix works as expected and doesn't introduce regressions for URLs without query parameters.

---

_Review comment by @charliermarsh on `crates/uv-distribution-filename/src/wheel.rs`:328 on 2025-06-23 17:03_

I think we also want to strip fragments (`#` at the end). There's an implementation for this in `URLString`:

```rust
    /// Return the [`UrlString`] with any query parameters and fragments removed.
    pub fn base_str(&self) -> &str {
        self.as_ref()
            .split_once('?')
            .or_else(|| self.as_ref().split_once('#'))
            .map(|(path, _)| path)
            .unwrap_or(self.as_ref())
    }
```

---

_@charliermarsh reviewed on 2025-06-23 17:03_

---

_@charliermarsh reviewed on 2025-06-23 17:25_

---

_Review comment by @charliermarsh on `crates/uv-distribution-filename/src/wheel.rs`:328 on 2025-06-23 17:25_

If you prefer, I can make this patch and merge.

---

_@esafak reviewed on 2025-06-23 17:26_

---

_Review comment by @esafak on `crates/uv-distribution-filename/src/wheel.rs`:328 on 2025-06-23 17:26_

Yes, please! Note that the tests don't cover that case.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-06-23 17:51_

---

_@charliermarsh reviewed on 2025-06-23 18:01_

---

_Review comment by @charliermarsh on `crates/uv-distribution-filename/src/wheel.rs`:328 on 2025-06-23 18:01_

It looks like this code is actually unused (https://github.com/astral-sh/uv/pull/14223) so this isn't quite the right spot. I commented back in the originating issue.

---

_Closed by @charliermarsh on 2025-06-23 18:52_

---
