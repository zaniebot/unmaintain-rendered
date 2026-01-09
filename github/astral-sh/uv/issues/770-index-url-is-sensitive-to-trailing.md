---
number: 770
title: "Index URL is sensitive to trailing `/`"
type: issue
state: closed
author: zanieb
labels:
  - bug
assignees: []
created_at: 2024-01-04T04:20:23Z
updated_at: 2024-01-04T13:45:52Z
url: https://github.com/astral-sh/uv/issues/770
synced_at: 2026-01-07T13:12:16-06:00
---

# Index URL is sensitive to trailing `/`

---

_Issue opened by @zanieb on 2024-01-04 04:20_

Succeeds without trailing `/`
```
❯ cargo run --bin puffin pip-install requires-transitive-incompatible-with-transitive-8329cfc0-a --index-url https://test.pypi.org/simple
    Finished dev [unoptimized + debuginfo] target(s) in 0.33s
     Running `target/debug/puffin pip-install requires-transitive-incompatible-with-transitive-8329cfc0-a --index-url 'https://test.pypi.org/simple'`
Resolved 2 packages in 181ms
Downloaded 1 package in 13ms
Installed 1 package in 4ms
 + requires-transitive-incompatible-with-transitive-8329cfc0-a==1.0.0
```

Fails with trailing `/`
```
❯ cargo run --bin puffin pip-install requires-transitive-incompatible-with-transitive-8329cfc0-a --index-url https://test.pypi.org/simple/
    Finished dev [unoptimized + debuginfo] target(s) in 0.33s
     Running `target/debug/puffin pip-install requires-transitive-incompatible-with-transitive-8329cfc0-a --index-url 'https://test.pypi.org/simple/'`
error: Package `requires-transitive-incompatible-with-transitive-8329cfc0-a` was not found in the registry.
```

`pip` doesn't care

```
❯ pip install requires-transitive-incompatible-with-transitive-8329cfc0-a --index-url https://test.pypi.org/simple/
Looking in indexes: https://test.pypi.org/simple/
Collecting requires-transitive-incompatible-with-transitive-8329cfc0-a
  Obtaining dependency information for requires-transitive-incompatible-with-transitive-8329cfc0-a from https://test-files.pythonhosted.org/packages/b5/4e/21413d61eedee47925d1d5d3c89302f64dcdb9edb0e656ee5a3d287fa246/requires_transitive_incompatible_with_transitive_8329cfc0_a-1.0.0-py3-none-any.whl.metadata
  Downloading https://test-files.pythonhosted.org/packages/b5/4e/21413d61eedee47925d1d5d3c89302f64dcdb9edb0e656ee5a3d287fa246/requires_transitive_incompatible_with_transitive_8329cfc0_a-1.0.0-py3-none-any.whl.metadata (208 bytes)
Requirement already satisfied: requires-transitive-incompatible-with-transitive-8329cfc0-c==1.0.0 in ./.direnv/python-3.12/lib/python3.12/site-packages (from requires-transitive-incompatible-with-transitive-8329cfc0-a) (1.0.0)
Downloading https://test-files.pythonhosted.org/packages/b5/4e/21413d61eedee47925d1d5d3c89302f64dcdb9edb0e656ee5a3d287fa246/requires_transitive_incompatible_with_transitive_8329cfc0_a-1.0.0-py3-none-any.whl (1.4 kB)
Installing collected packages: requires-transitive-incompatible-with-transitive-8329cfc0-a
Successfully installed requires-transitive-incompatible-with-transitive-8329cfc0-a-1.0.0
```

---

_Label `bug` added by @zanieb on 2024-01-04 04:20_

---

_Comment by @charliermarsh on 2024-01-04 04:32_

👍 We def shouldn't care either.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-04 05:28_

---

_Referenced in [astral-sh/uv#771](../../astral-sh/uv/pulls/771.md) on 2024-01-04 05:29_

---

_Closed by @charliermarsh on 2024-01-04 13:45_

---
