```yaml
number: 4810
title: Fill Python requests with platform information during automatic fetches
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/python-fill
created_at: 2024-07-04T15:31:20Z
updated_at: 2024-07-05T21:13:59Z
url: https://github.com/astral-sh/uv/pull/4810
synced_at: 2026-01-12T16:06:28Z
```

# Fill Python requests with platform information during automatic fetches

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/4800

We do this during `install` — it's an important step to ensure the request has the platform information in it.

---

_Label `bug` added by @zanieb on 2024-07-04 15:31_

---

_@zanieb reviewed on 2024-07-04 15:32_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:91 on 2024-07-04 15:32_

Alternatively, perhaps we should be doing this a bit further down in `Self::fetch`?

---

_@zanieb reviewed on 2024-07-04 15:33_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:91 on 2024-07-04 15:33_

Or in `ManagedPythonDownload::from_request` to make sure we never miss it?

---

_Comment by @zanieb on 2024-07-04 15:44_

@konstin could you check this resolves the issue on Linux please?

---

_Marked ready for review by @zanieb on 2024-07-04 15:44_

---

_Comment by @zanieb on 2024-07-04 15:44_

The need for a lightweight testing story for Python toolchain commands grows...

---

_@charliermarsh reviewed on 2024-07-04 17:00_

---

_Review comment by @charliermarsh on `crates/uv-python/src/installation.rs`:91 on 2024-07-04 17:00_

Why don't we have to do it on line 106?

---

_@zanieb reviewed on 2024-07-04 19:22_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:91 on 2024-07-04 19:22_

We should. Great indication that we should enforce this elsewhere haha

---

_@charliermarsh reviewed on 2024-07-04 20:07_

---

_Review comment by @charliermarsh on `crates/uv-python/src/installation.rs`:91 on 2024-07-04 20:07_

Lol

---

_@charliermarsh reviewed on 2024-07-04 20:07_

---

_Review comment by @charliermarsh on `crates/uv-python/src/installation.rs`:91 on 2024-07-04 20:07_

Could they have distinct types?

---

_@charliermarsh reviewed on 2024-07-04 20:07_

---

_Review comment by @charliermarsh on `crates/uv-python/src/installation.rs`:91 on 2024-07-04 20:07_

Like `fill` returns a different type that you can only get by calling `fill`. That would have the benefit of making it impossible to miss while also giving us the flexibility to `fill` when we want (vs. coupling with the constructor).

---

_@zanieb reviewed on 2024-07-04 20:20_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:91 on 2024-07-04 20:20_

Yes that sounds reasonable but there are a weird amount of requests types to cast between already and I'd want to think a little more holistically if we add another.

---

_Comment by @konstin on 2024-07-05 08:00_

Still failing for me:

```
$ rm -rf ~/.local/share/uv/ ~/.cache/uv/ && cargo run -q -- tool install --preview -p 3.12 --force black --python-preference only-managed
error: Failed to query Python interpreter at `/home/konsti/.local/share/uv/python/cpython-3.12.3-macos-aarch64-none/install/bin/python3`
  Caused by: Exec format error (os error 8)
```

---

_Comment by @zanieb on 2024-07-05 15:39_

@konstin that's because I didn't change it in the other place Charlie pointed out

---

_Comment by @charliermarsh on 2024-07-05 21:04_

I'm gonna add and merge for now -- we can revisit if we wanna do something better.

---

_Merged by @charliermarsh on 2024-07-05 21:13_

---

_Closed by @charliermarsh on 2024-07-05 21:13_

---

_Branch deleted on 2024-07-05 21:13_

---
