```yaml
number: 16694
title: "`pip install --target` (and `sync`) install python if necessary"
type: pull_request
state: merged
author: mikaylathompson
labels:
  - enhancement
assignees: []
merged: true
base: main
head: mikayla/pip-install-target-installs-python
created_at: 2025-11-11T23:26:26Z
updated_at: 2025-11-12T22:42:54Z
url: https://github.com/astral-sh/uv/pull/16694
synced_at: 2026-01-10T05:58:11Z
```

# `pip install --target` (and `sync`) install python if necessary

---

_Pull request opened by @mikaylathompson on 2025-11-11 23:26_

## Summary

As described in https://github.com/astral-sh/uv/issues/12229, `pip install` with `--target` or `--prefix` seem like they should install the necessary python version if it doesn't exist, but they currently don't.

Most minimal reproduction is something like:
```
> uv python uninstall 3.13
...
> uv pip install anyio --target target-dir --python 3.13
error: No interpreter found for Python 3.13 in virtual environments, managed installations, or search path
```

This also fails without `--target`, but a venv is expected in that case, so the with `--target`/`--prefix` is the only version that needs to be fixed. The same mechanism occurs for `uv pip sync` as well.

## Test Plan

Added tests for install and sync that failed before fix and now pass.


---

_@zanieb reviewed on 2025-11-11 23:32_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:13175 on 2025-11-11 23:32_

We should add `#[cfg(feature = "python-managed")]` so this test isn't active for downstream people who don't want coverage for those.

---

_@zanieb reviewed on 2025-11-11 23:33_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:13182 on 2025-11-11 23:33_

This should instead use https://github.com/astral-sh/uv/blob/257243fec5fc1d85817984e7b1c906b6a9ae3e08/crates/uv/tests/it/common/mod.rs#L514

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:13182 on 2025-11-11 23:33_

and should also use `with_python_download_cache`

---

_@zanieb reviewed on 2025-11-11 23:33_

---

_@zanieb reviewed on 2025-11-11 23:50_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1876 on 2025-11-11 23:50_

This isn't quite true — if we can't find _any_ usable interpreter on the system we'll also fetch one. I appreciate the documentation, but I would probably not include this commentary since it's uv's general behavior and we'd be hard-pressed if we had to explain it everywhere, e.g., we don't explain this in `uv venv`.

---

_@zanieb reviewed on 2025-11-11 23:52_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1876 on 2025-11-11 23:52_

(I don't feel strongly so if you prefer to keep it you can, but it'll need to say something like "If a suitable Python interpreter to use for resolution cannot be found, uv will install one...")

---

_Review comment by @mikaylathompson on `crates/uv-cli/src/lib.rs`:1876 on 2025-11-12 00:08_

Ah, got it. I was aiming to add something that would have clarified my "I'm not sure if this is a bug or expected behavior" stance when I was first looking at it, and I think that my confusion, at least, was specific to the pip-specific flows because uv is handling more than the equivalent pip command would.

The original issue seems somewhat unsure for the same reason:
> My expectation was for uv to automatically download the missing Python version as it does with the majority of other uv workflows. Although, I can see this might intentionally not be the case with the "pip compatibility" layer as managing Python versions is not something that pip concerns itself with.

So I think my inclination is to update the wording as you suggested but keep a mention of it in. Fine being overruled on that if you think this is generally an obvious behavior (and only ambiguous if it is missing).

---

_@mikaylathompson reviewed on 2025-11-12 00:08_

---

_Comment by @zanieb on 2025-11-12 13:59_

It seems like in `uv pip compile` we also won't download? https://github.com/astral-sh/uv/blob/5c71b5c124a1ed98fb21502693b63069f97bec6c/crates/uv/src/commands/pip/compile.rs#L295-L301

---

_@zanieb reviewed on 2025-11-12 14:04_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1876 on 2025-11-12 14:04_

I think it's mostly ambiguous if the functionality is missing but I don't really mind either way.

It might be useful to explain why, if we say something... like

> Unlike other install operations, this command does not require discovery of an existing Python
> environment and only searches for a Python interpreter to use for package resolution.
> If a suitable Python interpreter cannot be found, ...

---

_Review requested from @zanieb by @mikaylathompson on 2025-11-12 18:19_

---

_@zanieb reviewed on 2025-11-12 18:51_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:13183 on 2025-11-12 18:51_

The comment is stale

---

_@zanieb reviewed on 2025-11-12 18:51_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_sync.rs`:6205 on 2025-11-12 18:51_

This comment is stale

---

_@zanieb reviewed on 2025-11-12 18:52_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:13175 on 2025-11-12 18:52_

Should we also have a test case where we do `new_with_versions(&[])` and no `--python` request? That should fetch 3.14, I think?

---

_@zanieb approved on 2025-11-12 18:52_

---

_Label `enhancement` added by @zanieb on 2025-11-12 18:52_

---

_@mikaylathompson reviewed on 2025-11-12 21:41_

---

_Review comment by @mikaylathompson on `crates/uv/tests/it/pip_install.rs`:13175 on 2025-11-12 21:41_

Ah, yep, that makes sense

---

_Merged by @mikaylathompson on 2025-11-12 22:42_

---

_Closed by @mikaylathompson on 2025-11-12 22:42_

---

_Branch deleted on 2025-11-12 22:42_

---
