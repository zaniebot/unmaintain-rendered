```yaml
number: 1027
title: Allow relative paths in requirements.txt
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/relative
created_at: 2024-01-20T19:22:08Z
updated_at: 2024-02-22T20:36:05Z
url: https://github.com/astral-sh/uv/pull/1027
synced_at: 2026-01-12T16:04:22Z
```

# Allow relative paths in requirements.txt

---

_@charliermarsh_

This PR attempts to fix a common footgun in `requirements.txt` files. Previously, to provide a file, you had to use `package_name @ file:///Users/crmarsh/...` -- in other words, an absolute path.

Now, these requirements follow the exact same rules as editables, so you can do:
```
package_name @ ./file.zip
```

And similar.

The way the parsing is setup, this is intentionally _not_ supported when reading metadata -- only when parsing `requirements.txt` directly.

Closes #984.


---

_Review requested from @zanieb by @charliermarsh on 2024-01-20 19:22_

---

_Review requested from @konstin by @charliermarsh on 2024-01-20 19:22_

---

_Label `enhancement` added by @charliermarsh on 2024-01-20 19:22_

---

_Review comment by @konstin on `crates/pep508-rs/src/verbatim_url.rs`:60 on 2024-01-22 08:31_

Could this take a `Option<T> where T: impl AsRef<Path>`? This would merge the branches in the callers

---

_@konstin approved on 2024-01-22 08:33_

---

_@charliermarsh reviewed on 2024-01-22 14:01_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/verbatim_url.rs`:60 on 2024-01-22 14:01_

I tried that but it honestly felt worse because then we had to add `Result` handling in the `from_path` case even when we _know_ we have a working directory. In other words, the error is only possible when there's no working directly, and I can only express that in the type system with separate signatures.

---

_Merged by @charliermarsh on 2024-01-22 14:20_

---

_Closed by @charliermarsh on 2024-01-22 14:20_

---

_Branch deleted on 2024-01-22 14:20_

---

_Comment by @debugger24 on 2024-02-22 19:43_

Can we also add support for same in `pyproject.toml` project -> dependencies ?

---

_Comment by @konstin on 2024-02-22 20:36_

We could, but it is not in the specification ([PEP 621](https://peps.python.org/pep-0621/)). We will likely use `tool.uv.dependencies` or something in the future to support this.

---
