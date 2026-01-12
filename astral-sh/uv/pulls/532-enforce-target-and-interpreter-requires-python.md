```yaml
number: 532
title: "Enforce target and interpreter `requires-python` versions"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/target-version
created_at: 2023-12-04T03:16:14Z
updated_at: 2023-12-04T10:27:38Z
url: https://github.com/astral-sh/uv/pull/532
synced_at: 2026-01-12T16:04:01Z
```

# Enforce target and interpreter `requires-python` versions

---

_@charliermarsh_

## Summary

This PR modifies the behavior of our `--python-version` override in two ways:

1. First, we always use the "real" interpreter in the source distribution builder. I think this is correct. We don't need to use the fake markers for recursive builds, because all we care about is the top-level resolution, and we already assume that a single source distribution will always return the same metadata regardless of its build environment.
2. Second, we require that source distributions are compatible with _both_ the "real" interpreter version and the marker environment. This ensures that we don't try to build source distributions that are compatible with our interpreter, but incompatible with the target version.

Closes https://github.com/astral-sh/puffin/issues/407.

---

_Renamed from "Enforce target and interpreter requires-python versions" to "Enforce target and interpreter `requires-python` versions" by @charliermarsh on 2023-12-04 03:16_

---

_Review requested from @konstin by @charliermarsh on 2023-12-04 03:16_

---

_Label `enhancement` added by @charliermarsh on 2023-12-04 03:16_

---

_@konstin reviewed on 2023-12-04 10:26_

---

_Review comment by @konstin on `crates/puffin-resolver/src/version_map.rs`:50 on 2023-12-04 10:26_

This is good, but i will make it harder having `python` as a dependency. I think we need separate `python` and `python$build` (or something else that's banned on any index) dependencies. CC @zanieb 

---

_@konstin approved on 2023-12-04 10:27_

---

_@konstin reviewed on 2023-12-04 10:27_

---

_Review comment by @konstin on `crates/puffin-interpreter/src/interpreter.rs`:103 on 2023-12-04 10:27_

Happy to see this gone

---

_Merged by @konstin on 2023-12-04 10:27_

---

_Closed by @konstin on 2023-12-04 10:27_

---

_Branch deleted on 2023-12-04 10:27_

---
