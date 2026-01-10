```yaml
number: 1358
title: numpy-array completion evaluation shows different results between macOS and linux
type: issue
state: closed
author: MichaReiser
labels:
  - testing
  - completions
assignees: []
created_at: 2025-10-15T08:45:35Z
updated_at: 2025-10-15T08:53:52Z
url: https://github.com/astral-sh/ty/issues/1358
synced_at: 2026-01-10T02:06:25Z
```

# numpy-array completion evaluation shows different results between macOS and linux

---

_Issue opened by @MichaReiser on 2025-10-15 08:45_

When run on my linux desktop machine (or in CI)

```
cargo r -q -p ty_completion_eval show-one numpy-array --index 1

array (*, 1/19)
array2string
array_equal
array_equiv
array_repr
array_split
array_str
asanyarray
asarray
asarray_chkfinite
ascontiguousarray
asfortranarray
broadcast_arrays
matrix_transpose
ndarray
recarray
__array_api_version__
__array_namespace_info__
_ArrayOrScalarCommon
-----
found 19 completions
```

with python 3.13.7

But there are no completions when run on my mac mini

```
✦ ❯ cargo r -q -p ty_completion_eval show-one numpy-array --index 1

-----
found 0 completions
```

---

_Assigned to @BurntSushi by @MichaReiser on 2025-10-15 08:45_

---

_Label `testing` added by @MichaReiser on 2025-10-15 08:45_

---

_Label `completions` added by @MichaReiser on 2025-10-15 08:45_

---

_Comment by @MichaReiser on 2025-10-15 08:48_

Okay, I think I figured out what the issue was. I had an activated VENV

---

_Closed by @MichaReiser on 2025-10-15 08:53_

---

_Comment by @MichaReiser on 2025-10-15 08:53_

Fixed in https://github.com/astral-sh/ruff/pull/20807

---
