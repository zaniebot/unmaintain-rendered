```yaml
number: 4244
title: Ruff should notice the noqa settings at the end of wrapped function definitions
type: issue
state: open
author: retnuh
labels:
  - bug
  - suppression
assignees: []
created_at: 2023-05-05T11:37:13Z
updated_at: 2025-11-10T10:24:48Z
url: https://github.com/astral-sh/ruff/issues/4244
synced_at: 2026-01-10T11:09:47Z
```

# Ruff should notice the noqa settings at the end of wrapped function definitions

---

_Issue opened by @retnuh on 2023-05-05 11:37_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Version:  0.0.261 (and below?)

Hi there, if you have a function that ends up getting wrapped (particularly by black), like this, after adding a new argument:

```
    def _create_dxvif(  
        cls, account, connection, device, iface, name, vgw_or_dxgw, region, dxvif_type, vlan
    ):  # noqa: PLR0913
```

Ruff doesn't notice the `noqa` on the third line, it wants it on the first line.  This is annoying to have to go move the noqa statement, it should be recognized in either place.

This is especially annoying if running in conjunction with pylint - pylint only recognizes the disable statements on the third line, not the first, so you can end up with this ugliness:

```
    def _create_dxvif(   # noqa: PLR0913
        cls, account, connection, device, iface, name, vgw_or_dxgw, region, dxvif_type, vlan
    ):   # pylint: disable=too-many-arguments
``` 
or other annoying variations thereof.

---

_Label `bug` added by @MichaReiser on 2023-05-05 12:12_

---

_Label `noqa` added by @charliermarsh on 2023-05-05 18:12_

---

_Comment by @ssbarnea on 2023-05-15 07:52_

That is indeed a very annoying bug and I am not even aware on how to avoid it. Ruff is removing the them with auto-fix and requiring them when they are missing, basically no way to disable them in a single place. I guess I am forced to add them as global ignore.

---

_Comment by @charliermarsh on 2023-05-16 00:20_

> ...basically no way to disable them in a single place.

Do you mind providing an example of what you mean here? There are errors that you're unable to suppress?

---

_Comment by @jaap3 on 2023-08-31 09:45_

I closed #7006 a duplicate of this one. It's slightly broader as it also applies to wrapped constructor, function and method calls, but it's the same general issue.

I also noticed an inconsistency of where the exact noqa comment should go in wrapped definitions, depending on the rule that is violated.

---

_Comment by @jaap3 on 2023-09-05 07:12_

Just came across this:

```python
def foo():
    (  # noqa: N806
        FooForm,
        BarForm,
    ) = get_form_classes()
```

Which works with `flake8` (`6.1.0` and `pep8-naming: 0.13.3`)

`ruff` (`0.0.287`) on the other hand complains about the casing of the variables.

This does work:

```python
def foo():
    (
        FooForm,  # noqa: N806
        BarForm,  # noqa: N806
    ) = get_form_classes()
```

---

_Comment by @Jason-Francis-github on 2025-11-10 10:24_

This issue is causing a head ache in multiple projects were
 JR devs are ignoring the error in toml etc as they dont understand the root cause. 

Would be great to get this fixed please. 

---
