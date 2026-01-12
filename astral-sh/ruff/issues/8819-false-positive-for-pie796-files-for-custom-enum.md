```yaml
number: 8819
title: "False positive for `PIE796` files for custom enum_auto function"
type: issue
state: closed
author: Skylion007
labels:
  - bug
assignees: []
created_at: 2023-11-22T15:33:25Z
updated_at: 2023-12-07T03:03:02Z
url: https://github.com/astral-sh/ruff/issues/8819
synced_at: 2026-01-12T15:54:48Z
```

# False positive for `PIE796` files for custom enum_auto function

---

_@Skylion007_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
The Nvidia cutlass library has it's own function `enum_auto` to populate enums as in `tools/library/scripts/pycutlass/src/pycutlass/library.py`
```
class StrideSupport(enum.Enum):
    Strided = enum_auto()
    Unity = enum_auto()
```
The linter currently complains with `PIE796` which it really should not unless it can be sure the output of enum_auto() is always the same: https://github.com/NVIDIA/cutlass/blob/b5d8a5d9ccb435268e2215144dca162b0fc6a826/python/cutlass_library/library.py#L49

---

_Comment by @zanieb on 2023-11-22 16:13_

Hm... usually a repeated function call _would_ always yield the same results it's more of a special case that it wouldn't using global state. It seems like we'd have some false negatives if we were to allow any repeated function call here? I'm not sure I have a good solution otherwise though.


---

_Comment by @Skylion007 on 2023-11-22 18:27_

Can we check for global variable mutation inside of the function call somehow?

---

_Comment by @Skylion007 on 2023-11-22 18:33_

Yeah, at least enum.auto appears to work properly.

---

_Comment by @charliermarsh on 2023-11-22 18:36_

Ahh I see, so `enum_auto` is a backwards-compatibility thing for Python 3.5. Interesting.

---

_Comment by @zanieb on 2023-11-22 20:08_

Perhaps we could support this specific use-case by detecting cases where `enum.auto` is aliased as another name e.g. `enum_auto` then allow that name even if it's shadowed? Pretty weird though.

---

_Comment by @zanieb on 2023-11-22 20:10_

We also have a weird `auto` method usage at Prefect :D https://github.com/PrefectHQ/prefect/blob/d1894e8089d432cd62c7b63558fa75a8bf7049a7/src/prefect/utilities/collections.py#L41-L72

---

_Comment by @charliermarsh on 2023-12-07 03:02_

I think this is gonna be hard to support as-is without a dedicated setting that requires users to provide an allow-list of functions or methods to treat as unique calls, but at the same time it feels a bit too niche for that. I'm going to close, but if folks have strong objections or other ideas we can reopen.

---

_Closed by @charliermarsh on 2023-12-07 03:02_

---

_Label `bug` added by @charliermarsh on 2023-12-07 03:03_

---
