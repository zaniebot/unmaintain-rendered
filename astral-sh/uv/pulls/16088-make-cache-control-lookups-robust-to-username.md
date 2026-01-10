```yaml
number: 16088
title: Make cache control lookups robust to username
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/u
created_at: 2025-10-01T16:27:57Z
updated_at: 2025-10-01T20:59:01Z
url: https://github.com/astral-sh/uv/pull/16088
synced_at: 2026-01-10T06:36:15Z
```

# Make cache control lookups robust to username

---

_Pull request opened by @charliermarsh on 2025-10-01 16:27_

## Summary

We serialize the index to the lockfile without the username, so if we compare based on `==` and the user _includes_ the username in their `pyproject.toml`, the check will always fail.

Closes https://github.com/astral-sh/uv/issues/16076.


---

_Review requested from @zanieb by @charliermarsh on 2025-10-01 16:28_

---

_Review request for @zanieb removed by @charliermarsh on 2025-10-01 16:28_

---

_Review requested from @konstin by @charliermarsh on 2025-10-01 16:28_

---

_Review requested from @zanieb by @charliermarsh on 2025-10-01 16:28_

---

_Label `bug` added by @charliermarsh on 2025-10-01 16:28_

---

_Marked ready for review by @charliermarsh on 2025-10-01 16:28_

---

_@zanieb approved on 2025-10-01 20:09_

---

_Comment by @zanieb on 2025-10-01 20:10_

Can we include a test case using the fly proxy index?

---

_Merged by @charliermarsh on 2025-10-01 20:57_

---

_Closed by @charliermarsh on 2025-10-01 20:57_

---

_Branch deleted on 2025-10-01 20:57_

---

_Comment by @charliermarsh on 2025-10-01 20:58_

I've been trying but I can't figure out a good way to test this. It has to be during `sync` (after a `lock` from disk), and it has to involve matching data from the `pyproject.toml` to the URLs persisted to the lock.

---

_Comment by @charliermarsh on 2025-10-01 20:59_

I tried something like this but doesn't quite make sense:

```rust
let context = TestContext::new("3.12");

let pyproject_toml = context.temp_dir.child("pyproject.toml");
pyproject_toml.write_str(
    r#"
    [project]
    name = "foo"
    version = "0.1.0"
    requires-python = ">=3.12"
    dependencies = ["iniconfig"]

    [[tool.uv.index]]
    name = "proxy"
    url = "https://public@pypi-proxy.fly.dev/basic-auth/simple"
    ignore-error-codes = [401]
    "#,
)?;

// Lock with valid credentials.
uv_snapshot!(context.filters(), context.lock(), @r"
success: true
exit_code: 0
----- stdout -----

----- stderr -----
Resolved 2 packages in [TIME]
");

uv_snapshot!(context.filters(), context.sync(), @r"
success: true
exit_code: 0
----- stdout -----

----- stderr -----
Resolved 2 packages in [TIME]
");

Ok(())
```

---
