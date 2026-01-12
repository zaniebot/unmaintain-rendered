```yaml
number: 8299
title: "Python Substring: infine loop using ruff v0.1.2"
type: issue
state: closed
author: bigluck
labels:
  - bug
assignees: []
created_at: 2023-10-28T09:00:56Z
updated_at: 2023-10-30T23:38:15Z
url: https://github.com/astral-sh/ruff/issues/8299
synced_at: 2026-01-12T15:54:48Z
```

# Python Substring: infine loop using ruff v0.1.2

---

_@bigluck_

Using `ruff` v0.1.2 and `ruff-pre-commit` v0.1.2, pre-commit fails with:

```bash
ruff.....................................................................Failed
- hook id: ruff
- exit code: 1

error: Failed to converge after 100 iterations.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `my/service/folder/api_v0/router.py`, the rule codes E231, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

my/service/folder/api_v0/router.py:215:106: E231 Missing whitespace after ':'
Found 101 errors (100 fixed, 1 remaining).
```

The code is pretty straightforward:

```python
from sqlalchemy.orm import DeclarativeBase

class Base(DeclarativeBase):
    pass

class MyClass(Base):
    file_uri: Mapped[str] = mapped_column(
        String(1024),
        nullable=False,
    )

...

def my_function(snapshot: MyClass):
    assert snapshot.file_uri is not None, f'Run "{req.run_id}" does not have a snapshot file.'
    snapshot_path = snapshot.file_uri[len(f's3://{self.s3_bucket_name}/'):]
```

Here my `pyproject.toml` settings:

```toml

[tool.mypy]
warn_return_any = true
warn_unused_configs = true

plugins = ["pydantic.mypy"]

# follow_imports = "silent"
warn_redundant_casts = true
warn_unused_ignores = true
disallow_any_generics = true
check_untyped_defs = true
no_implicit_reexport = true

# for strict mypy: (this is the tricky one :-))
disallow_untyped_defs = true

[tool.pydantic-mypy]
init_forbid_extra = true
init_typed = true


[tool.coverage.run]
branch = true
omit = [".devenv/*", "stages/*", "tests/*", ".venv/*"]


[tool.coverage.report]


[tool.coverage.xml]
output = "cov.xml"


[tool.ruff]
# Enable Pyflakes and pycodestyle rules.
select = ["E", "F"]
# select = ["ALL"]
preview = true

# Never enforce `E501` (line length violations).
ignore = ["E501"]

# Always autofix, but never try to fix `F401` (unused imports).
fix = true
unfixable = ["F401"]


[tool.ruff.per-file-ignores]
# "__init__.py" = ["E402"]

```

It works when I set:

```python
    snapshot_path = snapshot.file_uri[len(f's3://{self.s3_bucket_name}/'): len(snapshot.file_uri)]
```

but still, it wants a space between the first and the second length :/

---

_Comment by @charliermarsh on 2023-10-28 10:30_

Thank you for filing!

---

_Label `bug` added by @charliermarsh on 2023-10-28 10:30_

---

_Comment by @FishAlchemist on 2023-10-28 15:28_

This is my simplified pyproject.toml, I don't know if it works for everywhere. Interestingly, I need to run Ruff in the directory where the Python file is located to trigger the issue.
E231 and E202 need to be selected at the same time, otherwise the problem does not occur.
```toml
[tool.ruff]
select = ["E231","E202"]
preview = true
fix = true
```

---

_Comment by @charliermarsh on 2023-10-28 22:37_

(I think flagging those errors in the first place is incorrect -- pycodestyle doesn't.)

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-30 23:21_

---

_Closed by @charliermarsh on 2023-10-30 23:38_

---
