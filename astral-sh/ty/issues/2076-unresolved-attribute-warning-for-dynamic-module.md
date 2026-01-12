```yaml
number: 2076
title: unresolved-attribute warning for dynamic module attributes
type: issue
state: closed
author: duncanmmacleod
labels:
  - question
assignees: []
created_at: 2025-12-18T15:23:32Z
updated_at: 2025-12-18T16:18:26Z
url: https://github.com/astral-sh/ty/issues/2076
synced_at: 2026-01-12T15:54:26Z
```

# unresolved-attribute warning for dynamic module attributes

---

_@duncanmmacleod_

### Summary

Attempting to access a module-level object (attribute) that is set dynamically (e.g. with `setattr` or `globals()`) results in an `unresolved-attribute` warning, despite the attribute being actually resolvable.

Consider this toy example:

<details open>
<summary>example.py</summary>

```python
globals()["EXAMPLE"] = True
```

</details>

<details open>
<summary>test_example.py</summary>

```python
import example

if example.EXAMPLE:
    print("Example is True")
```

</details>

<details open>
<summary>Python output</summary>

```console
$ python test_example.py
Example is True
```

</details>

<details open>
<summary>ty output</summary>

```console
$ uvx ty check example.py test_example.py
error[unresolved-attribute]: Module `example` has no member `EXAMPLE`
 --> test_example.py:3:4
  |
1 | import example
2 |
3 | if example.EXAMPLE:
  |    ^^^^^^^^^^^^^^^
4 |     print("Example is True")
  |
info: rule `unresolved-attribute` is enabled by default

Found 1 diagnostic
```

</details>

---

A more realistic example (how I actually got here) is when using the [`astropy.constants`](https://docs.astropy.org/en/stable/constants/) module:

<details open>
<summary>test_astropy.py</summary>

```python
from astropy import constants
print(f"Speed of light: {constants.c.value} m/s")
```

</details>

<details open>
<summary>Python output</summary>

```console
$ uv run test_astropy.py
Speed of light: 299792458.0 m/s
```

</details>

<details open>
<summary>ty output</summary>

```console
$ uvx ty check test_astropy.py
error[unresolved-attribute]: Module `astropy.constants` has no member `c`
 --> test_astropy.py:2:26
  |
1 | from astropy import constants
2 | print(f"Speed of light: {constants.c.value} m/s")
  |                          ^^^^^^^^^^^
  |
info: rule `unresolved-attribute` is enabled by default

Found 1 diagnostic
```

</details>

---

Apologies if this is a duplicate of an existing report, I looked but didn't see anything obvious.

### Version

ty 0.0.3

---

_Comment by @AlexWaygood on 2025-12-18 15:32_

Hey, thanks for the report!

Mypy, pyright and pyrefly all emit the same error on `astropy.constants.c` here, so I'm not totally sure that this is a bug in ty. In general, there are lots of dynamic patterns in Python that work at runtime but that can't be supported by static type checkers, unfortunately. I think dynamically assigning globals using the `globals()` dictionary probably falls into that bucket, I'm afraid? And similar for the `utils._set_c()` magic that astropy is doing.

---

_Label `question` added by @AlexWaygood on 2025-12-18 15:32_

---

_Comment by @duncanmmacleod on 2025-12-18 15:53_

Thanks @AlexWaygood for the swift reply! I confirmed that mypy also emits the same (I had a misconfiguration for mypy when testing this early).

I understand that a static typer might not be able to handle myriad dynamic magic, but it would be great if a solution became available.

I searched the docs, but didn't see an answer to this, but is it possible to disable/ignore an error/warning based on a regex of the warning message? In this example it would be great to ignore `unresolved-attribute` in any module when the message refers to `astropy.constants`.

---

_Comment by @AlexWaygood on 2025-12-18 16:00_

No, I don't think we currently support ignoring errors based on a regex, unfortunately. We allow suppressing [specific error codes on a per-module basis](https://docs.astral.sh/ty/reference/configuration/#overrides), e.g. I think you'd do this to ignore all `unresolved-attribute` errors being reported in `foo.py`:

```toml
[[tool.ty.overrides]]
include = ["foo.py"]

[tool.ty.overrides.rules]
unresolved-attribute = "ignore"
```

---

_Comment by @duncanmmacleod on 2025-12-18 16:06_

Thanks @AlexWaygood.

I'll leave it to you to close this or leave it open for discussion as you see fit.

---

_Comment by @AlexWaygood on 2025-12-18 16:15_

Something equivalent to pyrefly's https://pyrefly.org/en/docs/configuration/#replace-imports-with-any option might be helpful for this kind of thing. Thoughts @carljm / @MichaReiser?

---

_Comment by @AlexWaygood on 2025-12-18 16:18_

Okay, I'll close this in favour of #2078 and mention the alternative configuration option there instead. Thanks!

---

_Closed by @AlexWaygood on 2025-12-18 16:18_

---
