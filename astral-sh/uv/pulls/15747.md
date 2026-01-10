```yaml
number: 15747
title: "Ignore pre-release versions in `uv init --script`"
type: pull_request
state: merged
author: sharkdp
labels:
  - bug
assignees: []
merged: true
base: main
head: david/init-script-no-prerelease
created_at: 2025-09-09T08:50:55Z
updated_at: 2025-09-10T19:35:32Z
url: https://github.com/astral-sh/uv/pull/15747
synced_at: 2026-01-10T06:36:15Z
```

# Ignore pre-release versions in `uv init --script`

---

_Pull request opened by @sharkdp on 2025-09-09 08:50_

## Summary

Ignore managed pre-release versions of Python for purposes of creating a `requires-python` constraint when running `uv init --script`. This makes the behavior consistent with `uv init` for normal projects.

## Test Plan

Added a regression test that makes sure that the constraint is `requires-python = ">=3.13"`, even though a pre-release version of 3.14 is installed.


---

_Label `bug` added by @sharkdp on 2025-09-09 08:51_

---

_@zanieb reviewed on 2025-09-09 11:19_

---

_Review comment by @zanieb on `crates/uv/tests/it/edit.rs`:928 on 2025-09-09 11:19_

We probably shouldn't churn all these

---

_@zanieb reviewed on 2025-09-09 11:19_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:418 on 2025-09-09 11:19_

Why do we assume managed? I think we want to change that?

---

_@zanieb reviewed on 2025-09-09 11:20_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:418 on 2025-09-09 11:20_

This would be the cause of https://github.com/astral-sh/uv/pull/15747/files#r2333143886

---

_@zanieb reviewed on 2025-09-09 11:21_

---

_Review comment by @zanieb on `crates/uv/tests/it/init.rs`:908 on 2025-09-09 11:21_

We'll need to mark this test with the `python-patch` feature to indicate it tests behavior for specific patch version. The distros run the test suite against their Python versions and need to exclude the test. 

I probably would not pin 3.13.2 and 3.11.11 and instead just use 3.13 and 3.11, since it's not necessary for the semantics of the test and we'll just need to remember bump them in the future.



---

_@zanieb reviewed on 2025-09-09 11:22_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:413 on 2025-09-09 11:22_

What motivated this change?

---

_@sharkdp reviewed on 2025-09-09 13:04_

---

_Review comment by @sharkdp on `crates/uv/tests/it/init.rs`:908 on 2025-09-09 13:04_

I added the `python-patch` gating. Not sure if it's still required though. I removed the patch versions from `3.13` and `3.12`, but `3.14.0rc2` still includes `0` as a patch version, I guess?

---

_Review comment by @sharkdp on `crates/uv/tests/it/init.rs`:908 on 2025-09-09 13:14_

Confirmed by Zanie

---

_@sharkdp reviewed on 2025-09-09 13:14_

---

_Marked ready for review by @sharkdp on 2025-09-09 13:17_

---

_@zanieb approved on 2025-09-10 19:35_

---

_Merged by @zanieb on 2025-09-10 19:35_

---

_Closed by @zanieb on 2025-09-10 19:35_

---

_Branch deleted on 2025-09-10 19:35_

---

_Comment by @zanieb on 2025-09-10 19:35_

Thank you!

---
