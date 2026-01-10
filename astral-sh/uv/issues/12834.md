```yaml
number: 12834
title: "uv-pep440 `VersionSpecifiers::contains` incorrect with prereleases"
type: issue
state: closed
author: baszalmstra
labels:
  - bug
assignees: []
created_at: 2025-04-11T12:10:47Z
updated_at: 2025-04-12T20:57:07Z
url: https://github.com/astral-sh/uv/issues/12834
synced_at: 2026-01-10T03:41:47Z
```

# uv-pep440 `VersionSpecifiers::contains` incorrect with prereleases

---

_Issue opened by @baszalmstra on 2025-04-11 12:10_

### Summary

The `uv_pep440` crate yields an incorrect result when matching `0.1a1` against the version specifier `<0.1a2`.

However, solving the following `pyproject.toml` does correctly yield the correct prerelease version (`0.1a1`):

```toml
[project]
name = "uv-prerelease"
version = "0.1.0"
dependencies = [
    "rpi-kms <0.1a2"
]
```

### Simple reproducer test case

The following test program asserts:

```rust
use uv_pep440::{Version, VersionSpecifiers};

fn main() {
    let version: Version = "0.1a1".parse().unwrap();
    let version_specifier: VersionSpecifiers = "<0.1a2".parse().unwrap();
    assert!(version_specifier.contains(&version));
}
```

### Platform

Windows 11 x86_64

### Version

uv 0.6.14

### Python version

Python 3.13

---

_Label `bug` added by @baszalmstra on 2025-04-11 12:10_

---

_Comment by @baszalmstra on 2025-04-11 12:13_

I suspect the issue is this part in the `LessThan` comparison:

https://github.com/astral-sh/uv/blob/50de4644252a101d0f11f7ca8ff1767642b8d38f/crates/uv-pep440/src/version_specifier.rs#L562-L564

The less_than function also already guards against comparing pre-releases against non-prereleases so it seems redundant. But Im uncertain what that part of the code should do.

---

_Assigned to @konstin by @konstin on 2025-04-11 12:19_

---

_Closed by @charliermarsh on 2025-04-12 20:57_

---

_Closed by @charliermarsh on 2025-04-12 20:57_

---
