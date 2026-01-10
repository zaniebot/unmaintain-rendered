```yaml
number: 2082
title: "feat: expose uv_normalize types"
type: pull_request
state: merged
author: tdejager
labels: []
assignees: []
merged: true
base: main
head: feat/expose-uv-normalize-types
created_at: 2024-02-29T14:54:47Z
updated_at: 2024-02-29T15:06:56Z
url: https://github.com/astral-sh/uv/pull/2082
synced_at: 2026-01-10T14:54:43Z
```

# feat: expose uv_normalize types

---

_Pull request opened by @tdejager on 2024-02-29 14:54_

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Expose the uv_normalize types from pep508, so that these can be used with a crates.io version.



---

_@konstin approved on 2024-02-29 14:57_

---

_@konstin reviewed on 2024-02-29 14:58_

---

_Review comment by @konstin on `crates/pep508-rs/src/lib.rs`:47 on 2024-02-29 14:58_

```suggestion
// Parity with the crates.io version of pep508_rs
pub use uv_normalize::{ExtraName, InvalidNameError, PackageName};
```

---

_Merged by @konstin on 2024-02-29 15:06_

---

_Closed by @konstin on 2024-02-29 15:06_

---
