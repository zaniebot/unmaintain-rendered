---
number: 8331
title: "Formatter: different than black when wrapping parentheses for `assert`"
type: issue
state: open
author: njzjz
labels:
  - formatter
assignees: []
created_at: 2023-10-30T00:55:42Z
updated_at: 2024-02-13T11:39:00Z
url: https://github.com/astral-sh/ruff/issues/8331
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter: different than black when wrapping parentheses for `assert`

---

_Issue opened by @njzjz on 2023-10-30 00:55_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

The ruff version is v0.1.3.

Original code:
```py
class SomeClass:
    def some_method(necessary_key, kwargs):
        assert set(necessary_key).issubset(set(kwargs)), "Must give neccessary key: %s" % ", ".join(necessary_key)
```

Black:
```py
class SomeClass:
    def some_method(necessary_key, kwargs):
        assert set(necessary_key).issubset(
            set(kwargs)
        ), "Must give neccessary key: %s" % ", ".join(necessary_key)
```

ruff:

```py
class SomeClass:
    def some_method(necessary_key, kwargs):
        assert set(necessary_key).issubset(set(kwargs)), (
            "Must give neccessary key: %s" % ", ".join(necessary_key)
        )
```

See https://play.ruff.rs/f16cda56-ec7f-4b88-9dc9-1ba2dd18b39b


---

_Label `formatter` added by @MichaReiser on 2023-10-30 00:58_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-10-30 00:59_

---

_Referenced in [astral-sh/ruff#8388](../../astral-sh/ruff/issues/8388.md) on 2023-10-31 19:41_

---

_Comment by @spaceone on 2023-11-08 13:22_

The ruff style is better and much more readable! E.g. above examples and this here:
```diff
-            assert (
-                not should_fail
-            ), "Creating a school(%s) cli with dc_name=%s was unexpectedly successful" % (
-                school,
-                dc_name,
+            assert not should_fail, (
+                "Creating a school(%s) cli with dc_name=%s was unexpectedly successful"
+                % (
+                    school,
+                    dc_name,
+                )
             )
```
please keep as is.


---

_Removed from milestone `Formatter: Stable` by @MichaReiser on 2024-02-13 11:39_

---

_Referenced in [astral-sh/ruff#13386](../../astral-sh/ruff/issues/13386.md) on 2024-09-18 06:21_

---

_Referenced in [astral-sh/ruff#13663](../../astral-sh/ruff/pulls/13663.md) on 2024-10-23 11:47_

---
