```yaml
number: 4577
title: "Unused lints don't trigger for tuples."
type: issue
state: closed
author: vloddot
labels: []
assignees: []
created_at: 2023-05-22T13:54:20Z
updated_at: 2025-03-25T14:35:52Z
url: https://github.com/astral-sh/ruff/issues/4577
synced_at: 2026-01-12T15:54:44Z
```

# Unused lints don't trigger for tuples.

---

_@vloddot_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

### The bug
Tuples don't produce unused lints.

For example, this lints as unused:

```python
a = input()
```

However, this doesn't:

```python
a, b = input().split(' ')
```

### Command
```sh
ruff <filename> --isolated
```

### Ruff settings
Default.

### Ruff version
ruff 0.0.269

---

_Comment by @charliermarsh on 2023-05-22 14:07_

This is consistent with Flake8 and Pyflakes behavior, and I believe is intentional:

- https://github.com/PyCQA/pyflakes/issues/143
- https://github.com/PyCQA/pyflakes/issues/341

---

_Comment by @charliermarsh on 2023-05-22 14:07_

Note that Ruff and Pyflakes _will_ flag if the RHS is a tuple:

```python
def f():
    c, d = (2, 3)

```

---

_Comment by @charliermarsh on 2023-05-22 14:12_

We could perhaps try to flag `a` and `b` if _all_ tuple members are unused, but it adds enough complexity that I'm going to mark this as "expected" for now.

---

_Closed by @charliermarsh on 2023-05-22 14:12_

---

_Comment by @rodya-mirov on 2025-03-25 13:50_

Is there another lint we can add that _will_ trigger on this? I see this is consistent with pyflakes, but it's undesirable (to me) and I know there are quite a few other lint sets available. Google is only showing this one issue, so not sure where else to ask.

---

_Comment by @MichaReiser on 2025-03-25 14:35_

See https://docs.astral.sh/ruff/rules/unused-unpacked-variable/

---
