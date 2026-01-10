```yaml
number: 13351
title: "Bug: False format for list slicing with self.x as index"
type: issue
state: closed
author: CareF
labels: []
assignees: []
created_at: 2024-09-14T09:20:18Z
updated_at: 2024-09-14T09:26:40Z
url: https://github.com/astral-sh/ruff/issues/13351
synced_at: 2026-01-10T11:09:55Z
```

# Bug: False format for list slicing with self.x as index

---

_Issue opened by @CareF on 2024-09-14 09:20_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

```python
# file foo.py
a = list(range(5))
b = 2
print(a[1:b])


class Foo:
    def __init__(self):
        self.a = list(range(5))
        self.b = 2

    def bar(self):
        # The next line will be formatted to self.a[1 : self.b]
        print(self.a[1:self.b])
```

Running `ruff format tmp.py` will change the `self.a[1:self.b]` to `self.a[1 : self.b]` which is a conflict with `flake8: E203`. However the `a[1:b]` is not touched. No extra configs are needed.

```
$ ruff --version
ruff 0.6.5
```


---

_Comment by @trag1c on 2024-09-14 09:25_

I believe this is intended behavior. It doesn't conflict with Ruff's `E203`, and is in line with Black's style: https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html#slices
> PEP 8 [recommends](https://www.python.org/dev/peps/pep-0008/#whitespace-in-expressions-and-statements) to treat `:` in slices as a binary operator with the lowest priority, and to leave an equal amount of space on either side, except if a parameter is omitted (e.g. `ham[1 + 1 :]`). It recommends no spaces around `:` operators for “simple expressions” (`ham[lower:upper]`), and extra space for “complex expressions” (`ham[lower : upper + offset]`). Black treats anything more than variable names as “complex” (`ham[lower : upper + 1]`).

---

_Comment by @CareF on 2024-09-14 09:26_

Thank you!

---

_Closed by @CareF on 2024-09-14 09:26_

---
