---
number: 9412
title: Locking / resolving a version without appropriate distribution wheels available
type: issue
state: closed
author: jonaslb
labels: []
assignees: []
created_at: 2024-11-25T10:05:45Z
updated_at: 2025-02-28T13:37:31Z
url: https://github.com/astral-sh/uv/issues/9412
synced_at: 2026-01-10T01:24:40Z
---

# Locking / resolving a version without appropriate distribution wheels available

---

_Issue opened by @jonaslb on 2024-11-25 10:05_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Currently, the `eccodes` library's latest release, v. 2.39.0, is released without source or manylinux builds. It's an error on their side for sure, and I've created an issue in their repo. However, in spite of this, `uv` still resolves this latest problematic version when locking, and it means that `eccodes` can't be added without version specifier:

```sh
$ uv add eccodes
Resolved 7 packages in 821ms
error: Distribution `eccodes==2.39.0 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform
```

Specifying the old version (which has the right files on PyPI) works fine. However, in my projects I don't depend directly on eccodes, it's somewhere down the dependency tree, so I'd prefer not having to explicitly pin it in my pyproject file.

So, I'm not sure if it should be considered a bug in uv, but it's certainly annoying that a dependency somewhere in the tree can create this kind of disturbance.

Likely related (don't know if considered dupe):
- https://github.com/astral-sh/uv/issues/7816
- https://github.com/astral-sh/uv/issues/8375
- https://github.com/astral-sh/uv/issues/8492

---

_Comment by @charliermarsh on 2024-11-25 14:17_

Ah yeah, this is expected, though it's understandably not a great experience. It's another case of the problem I outlined here: https://github.com/astral-sh/uv/issues/5182#issuecomment-2237858277. I'd prefer to track in that issue. I think right now, you may want to set a constraint on your end (with `constraint-dependencies`).

---

_Closed by @charliermarsh on 2024-11-25 14:17_

---

_Comment by @jonaslb on 2025-02-28 13:34_

Returning to this because I just had the issue again with another ecmwf library, `eckitlib`, which in the latest version only has macos wheels. Since #5182 was closed, I believe the new tracking issue is #9711.

---

_Comment by @konstin on 2025-02-28 13:37_

uv now supports [required environments](https://docs.astral.sh/uv/concepts/resolution/#required-environments) for which wheels must exist for a version to be selected, this should match the original problem well.

---

_Referenced in [astral-sh/uv#11855](../../astral-sh/uv/issues/11855.md) on 2025-02-28 14:15_

---
