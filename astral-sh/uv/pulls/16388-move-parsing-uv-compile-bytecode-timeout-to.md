```yaml
number: 16388
title: "Move parsing `UV_COMPILE_BYTECODE_TIMEOUT` to EnvironmentOptions"
type: pull_request
state: open
author: andrey-berenda
labels: []
assignees: []
base: main
head: move-parsing-compile-bytecode-timeout-to-EnvironmentOptions
created_at: 2025-10-21T14:56:53Z
updated_at: 2025-10-27T15:21:57Z
url: https://github.com/astral-sh/uv/pull/16388
synced_at: 2026-01-12T16:12:14Z
```

# Move parsing `UV_COMPILE_BYTECODE_TIMEOUT` to EnvironmentOptions

---

_@andrey-berenda_

## Summary

Move parsing `UV_COMPILE_BYTECODE_TIMEOUT` to EnvironmentOptions
Relates https://github.com/astral-sh/uv/issues/14720

## Test Plan

- Tests with existing tests


---

_Marked ready for review by @andrey-berenda on 2025-10-22 20:20_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_install.rs`:3965 on 2025-10-22 21:01_

Hm, I don't think we should be persisting the default value here. If it wasn't explicitly configured, then it shouldn't be persisted for subsequent operations.

---

_@zanieb reviewed on 2025-10-22 21:01_

---

_Review comment by @andrey-berenda on `crates/uv/tests/it/tool_install.rs`:3965 on 2025-10-22 21:22_

I removed using default in parsing
And used it only in function where we use it

But I think it is inconsistent a little - sometimes we use default - for http-timeout for example
and sometimes no

---

_@andrey-berenda reviewed on 2025-10-22 21:22_

---

_Review requested from @zanieb by @andrey-berenda on 2025-10-22 21:36_

---

_Review comment by @andrey-berenda on `crates/uv/tests/it/tool_install.rs`:3965 on 2025-10-27 15:21_

What do you think about it? 
I can remove default values from all places if you think it is better

---

_@andrey-berenda reviewed on 2025-10-27 15:21_

---
