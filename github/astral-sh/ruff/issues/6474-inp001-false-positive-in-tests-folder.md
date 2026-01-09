---
number: 6474
title: INP001 false positive in tests folder
type: issue
state: open
author: flying-sheep
labels:
  - help wanted
assignees: []
created_at: 2023-08-10T07:49:18Z
updated_at: 2025-05-26T09:27:38Z
url: https://github.com/astral-sh/ruff/issues/6474
synced_at: 2026-01-07T13:12:15-06:00
---

# INP001 false positive in tests folder

---

_Issue opened by @flying-sheep on 2023-08-10 07:49_

The recommended Python + Pytest layout is

- `src/`
  - `mypkg/`
    - `__init__.py`
    - `subpkg/`
      - `__init__.py`
- `tests/`
  - `test_thing.py`
  - `test_more/`
    - `test_other_thing.py`

Notably, `tests` and all files under it *aren’t packages*. The [recommended import mode](https://docs.pytest.org/en/7.1.x/explanation/goodpractices.html#tests-outside-application-code) enforces that you can’t import from them.

Therefore `INP001` shouldn’t trigger when `tests` is on top level of the project.

In fact, it would be great to have both a rule that enforces the opposite and to ensure that `tests/**/test_*.py` files don’t import things from each other.

---

_Comment by @tjkuson on 2023-08-10 08:27_

I think this could be related to #6114.

---

_Comment by @flying-sheep on 2023-08-10 08:56_

Pytest collects and imports individual files. That’s independent from Python’s namespace packages.

It’s possible that that other issue incidentally fixes this, but this is an independent issue.

---

_Comment by @charliermarsh on 2023-08-10 13:03_

Interesting. To what degree is this specific to pytest? On the surface it feels like per-file-ignores is the right solution here…

---

_Label `needs-decision` added by @charliermarsh on 2023-08-10 13:38_

---

_Comment by @flying-sheep on 2023-08-10 14:43_

It’s very specific to Pytest. I’m coming from the perspective that when people have e.g.

```toml
[tool.ruff]
select = ['INP', 'PT']
```

```
project_dir
+- src/...
+- tests/
   +- test_foo.py
   +- test_bar/
      +- __init__.py
      +- test_bar_basic.py
```

then they should optimally see something like this:

```
tests/test_bar/__init__.py:1:1: PT030 File `tests/test_bar/__init__.py` found. Pytest test directories shouldn’t contain packages.
```

Currently they will see an instruction to do the opposite of the recommended route:

```
tests/test_foo.py:1:1: INP001 File `tests/test_foo.py` is part of an implicit namespace package. Add an `__init__.py`.
```


---

_Referenced in [UCL-ARC/python-tooling#219](../../UCL-ARC/python-tooling/pulls/219.md) on 2023-11-06 12:13_

---

_Comment by @sanmai-NL on 2023-11-15 09:38_

See also #8690 

---

_Referenced in [astral-sh/ruff#8145](../../astral-sh/ruff/issues/8145.md) on 2023-11-18 23:10_

---

_Referenced in [astral-sh/ruff#8794](../../astral-sh/ruff/issues/8794.md) on 2023-11-20 19:27_

---

_Comment by @Artalus on 2025-05-22 11:12_

For what it's worth, it seems that even _with_ the `__init__.py` file present, `ruff` still complains about the `INP001` (unlike the original flake linter):
```
[artalus@idpad foo]$ tree --gitignore
.
├── pyproject.toml
├── README.md
├── src
│   └── foo
│       └── __init__.py
├── tests
│   └── test_foo.py
└── uv.lock

[artalus@idpad foo]$ uv pip install flake8-no-pep420 ruff
Using Python 3.13.2 environment at: /tmp/420/.venv
Resolved 6 packages in 519ms
Installed 3 packages in 52ms
 + flake8==7.2.0
 + flake8-no-pep420==2.8.0
 + ruff==0.11.10

[artalus@idpad foo]$ flake8
./tests/test_foo.py:1:1: INP001 File is part of an implicit namespace package. Add an __init__.py?

[artalus@idpad foo]$ ruff check
tests/test_foo.py:1:1: INP001 File `tests/test_foo.py` is part of an implicit namespace package. Add an `__init__.py`.
Found 1 error.

[artalus@idpad foo]$ touch tests/__init__.py

[artalus@idpad foo]$ flake8

[artalus@idpad foo]$ ruff check
tests/test_foo.py:1:1: INP001 File `tests/test_foo.py` is part of an implicit namespace package. Add an `__init__.py`.
Found 1 error.
```

We ended up disabling `INP001` for the tests dir:
```toml
# /pyproject.toml
[tool.ruff.lint.per-file-ignores]
"tests/*" = [  # /* ensures it works for all subdirectories
    "INP001",  # "implicit namespace package" is invalid for pytest
]

# OR,

# /tests/ruff.toml
# inherit all settings from the main file
extend = "../pyproject.toml"
[lint]
ignore = [
    "INP001",
]
```

---

_Comment by @calumy on 2025-05-23 06:37_

I believe the `__init__.py` file needs to be present in the test directory to fix the `INP001` error

```
.
├── pyproject.toml
├── README.md
├── src
│   └── foo
│       └── __init__.py
├── tests
│   ├── test_foo.py
│   └── __init__.py
└── uv.lock
```

(I haven't actually tested this, but I believe it is correct as we have `__init__.py` files in our test directories and don't see these errors.)

---

_Comment by @flying-sheep on 2025-05-26 09:05_

> I believe the `__init__.py` file needs to be present in the test directory to fix the INP001 error

The `tests` directory is not a Python package, so putting an `__init__.py` anywhere in the `tests` directory is wrong, don’t do it. Therefore Ruff asking you to do that is an issue. The issue right here that you’re commenting under.

Saying “put `__init__.py` into the tests directory” in here is like if I went to the vet with my sick dog and you came in saying “did you try killing the dog?” Yeah, sure, that would remove the issue, but that’s not the point now, is it?

---

_Label `needs-decision` removed by @MichaReiser on 2025-05-26 09:26_

---

_Label `help wanted` added by @MichaReiser on 2025-05-26 09:26_

---

_Comment by @MichaReiser on 2025-05-26 09:27_

Special casing `tests` in INP001` if it's at the root of the project seems like a reasonable default to me.

---

_Referenced in [astral-sh/ruff#20269](../../astral-sh/ruff/pulls/20269.md) on 2025-09-05 19:22_

---
