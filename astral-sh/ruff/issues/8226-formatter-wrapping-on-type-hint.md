```yaml
number: 8226
title: "formatter: wrapping on type hint"
type: issue
state: closed
author: henryiii
labels:
  - formatter
assignees: []
created_at: 2023-10-25T20:30:36Z
updated_at: 2023-10-26T00:08:49Z
url: https://github.com/astral-sh/ruff/issues/8226
synced_at: 2026-01-10T11:09:50Z
```

# formatter: wrapping on type hint

---

_Issue opened by @henryiii on 2023-10-25 20:30_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

The following Black'd (23.*) code:

```python
JSONSerializable: TypeAlias = (
    "str | int | float | bool | None | list | tuple | JSONMapping"
)
```

is converted by `ruff format` (0.1.2) into:

```python
JSONSerializable: (
    TypeAlias
) = "str | int | float | bool | None | list | tuple | JSONMapping"
```

Pretty sure that's not intentional? Wrapping on the type hint looks terrible, IMO. :)

PS: While Black produces the code on the top, it will not rewrite the code on the bottom into the top.

---

_Renamed from "formatter: wrapping on type hint (deviation from Black)" to "formatter: wrapping on type hint" by @henryiii on 2023-10-25 20:41_

---

_Comment by @charliermarsh on 2023-10-25 20:54_

I think this is a consequence of https://docs.astral.sh/ruff/formatter/black/#type-annotations-may-be-parenthesized-when-expanded, but it's not great in cases like this.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-25 22:11_

---

_Label `formatter` added by @charliermarsh on 2023-10-25 22:11_

---

_Added to milestone `Formatter: Stable` by @charliermarsh on 2023-10-25 22:11_

---

_Comment by @MichaReiser on 2023-10-26 00:08_

Merging into #8188

---

_Closed by @MichaReiser on 2023-10-26 00:08_

---
