```yaml
number: 385
title: "Do not require `# noqa: F821` in try block with ` except NameError` block."
type: issue
state: closed
author: ghuls
labels:
  - bug
assignees: []
created_at: 2022-10-10T09:07:52Z
updated_at: 2022-10-10T14:25:28Z
url: https://github.com/astral-sh/ruff/issues/385
synced_at: 2026-01-10T15:56:05Z
```

# Do not require `# noqa: F821` in try block with ` except NameError` block.

---

_Issue opened by @ghuls on 2022-10-10 09:07_

Do not require `# noqa: F821` in try block with ` except NameError` block.

```python
$ cat test_try.py
def in_ipython_notebook() -> bool:
    try:
        # autoimported by notebooks
        get_ipython()  # type: ignore[name-defined]
    except NameError:
        return False  # not in notebook
    return True
```

```
$ flake8 test_try.py 
test_try.py:1:1: D100 Missing docstring in public module
test_try.py:1:1: D103 Missing docstring in public function

$  ruff test_try.py 
No pyproject.toml found.
Falling back to default configuration...
Found 1 error(s).
test_try.py:4:9: F821 Undefined name `get_ipython`
```


---

_Label `bug` added by @charliermarsh on 2022-10-10 13:08_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-10-10 13:08_

---

_Comment by @charliermarsh on 2022-10-10 13:09_

Will fix this today, hopefully.

---

_Closed by @charliermarsh on 2022-10-10 13:40_

---

_Comment by @ghuls on 2022-10-10 13:49_

Fixes it indeed. Thanks. Polars can now be linted by ruff (uses  more flake8 plugins, but it is a start).

---

_Comment by @charliermarsh on 2022-10-10 14:01_

Awesome. Are there any plugins that are high-priority? We can implement them natively if so.

---

_Comment by @ghuls on 2022-10-10 14:12_

This is the ones that we use for linting:

https://github.com/pola-rs/polars/blob/master/py-polars/requirements-lint.txt

```
black==22.8.0
blackdoc==0.3.7
flake8==5.0.4
flake8-bugbear==22.9.23
flake8-comprehensions==3.10.0
flake8-docstrings==1.6.0
flake8-simplify==0.19.3
flake8-tidy-imports==4.8.0
isort==5.10.1
pyupgrade==2.38.2
```

With this flake8 config:

https://github.com/pola-rs/polars/blob/master/py-polars/.flake8
```
[flake8]
max-line-length = 88
ban-relative-imports = true
docstring-convention = all
extend-ignore =
    # Satisfy black: https://black.readthedocs.io/en/stable/guides/using_black_with_other_tools.html#flake8
    E203,
    # pydocstyle: http://www.pydocstyle.org/en/stable/error_codes.html
    # numpy convention with a few additional lints
    D107, D203, D212, D401, D402, D415, D416,
    # TODO: Remove errors below to further improve docstring linting
    D1,
    # flake8-simplify
    SIM102, SIM117,

extend-exclude =
    # Automatically generated test artifacts
    venv/,
    target/,
```

---

_Comment by @charliermarsh on 2022-10-10 14:25_

Thank you!

For what it's worth:

- `flake8-comprehensions` we (almost) entirely support thanks to @harupy.
- `flake8-docstrings` is next on my list of plugins (I use it too).
- `flake8-bugbear` we should probably do too given its popularity but we haven't started on it.


---
