```yaml
number: 1911
title: Fix simple launcher test error condition
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/fix-simple-launcher-test
created_at: 2024-02-23T12:35:15Z
updated_at: 2024-02-27T12:15:08Z
url: https://github.com/astral-sh/uv/pull/1911
synced_at: 2026-01-10T14:54:43Z
```

# Fix simple launcher test error condition

---

_Pull request opened by @konstin on 2024-02-23 12:35_

This makes the test path on windows where developer mode is not enabled.

---

_Review requested from @MichaReiser by @konstin on 2024-02-23 12:35_

---

_Review comment by @MichaReiser on `crates/uv/tests/pip_install.rs`:1571 on 2024-02-23 12:37_

We should return an `Err` if the `error.kind()` if it's any other error to fail the test. The condition is only here to not make the test fail if the permissions are missing

```suggestion
    // Os { code: 1314, kind: Uncategorized, message: "A required privilege is not held by the client." }
        // where `Uncategorized` is unstable.
        if error.raw_os_error() == Some(1314) {
					return Ok(());
			 }
			return Err(error);
}

---

_@MichaReiser approved on 2024-02-23 12:37_

Thank you. Sorry that I messed that up.

---

_Merged by @konstin on 2024-02-27 12:15_

---

_Closed by @konstin on 2024-02-27 12:15_

---

_Branch deleted on 2024-02-27 12:15_

---
