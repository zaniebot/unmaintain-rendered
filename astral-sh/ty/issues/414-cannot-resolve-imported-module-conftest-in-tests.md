```yaml
number: 414
title: "Cannot resolve imported module `conftest` in `tests`"
type: issue
state: closed
author: MatthewMckee4
labels:
  - help wanted
  - imports
assignees: []
created_at: 2025-05-15T22:06:20Z
updated_at: 2025-05-26T08:08:58Z
url: https://github.com/astral-sh/ty/issues/414
synced_at: 2026-01-10T02:34:09Z
```

# Cannot resolve imported module `conftest` in `tests`

---

_Issue opened by @MatthewMckee4 on 2025-05-15 22:06_

### Summary

I'm maybe not sure how to best class this error, but when using pytest its obviously quite common to import from conftest.py in a test file, in the root or any subdirectory directory of the _tests_ directory.

in a test file like `tests/test_foo.py`

```py
from conftest import ... # error[unresolved-import]: Cannot resolve imported module `conftest`
```

we see this error.

I am aware that this code (and practice) is not ideal. But i imagine many users may have this issue when type checking tests directory.

Also, perhaps this is not a bug, but I think something useful to think about

### Version

0.0.0-alpha.8

---

_Renamed from "Cannot resolve imported module `conftest`" to "Cannot resolve imported module `conftest` in `tests`" by @MatthewMckee4 on 2025-05-15 22:06_

---

_Comment by @danielhollas on 2025-05-15 22:59_

Does the error go away if you add `__init__.py` file to your `tests/` directory?

---

_Label `imports` added by @AlexWaygood on 2025-05-16 00:58_

---

_Comment by @carljm on 2025-05-16 02:17_

Where is the `conftest.py` file that you are trying to import from? Can you show a full example with all the relevant files to reproduce the issue?

---

_Label `needs-mre` added by @AlexWaygood on 2025-05-16 02:24_

---

_Comment by @MatthewMckee4 on 2025-05-16 11:54_

I am aware that this isn't the best practice, and also maybe not very common

```bash
mkdir /tmp/repro
cd /tmp/repro 
mkdir tests
echo "import pytest

class Helper:
    def __call__(self) -> int:
        return 1

@pytest.fixture
def helper() -> Helper:
    return Helper()
" > tests/conftest.py
echo "from conftest import Helper

def test_ex(helper: Helper):
    assert helper() > 0
" > tests/test_ex.py
uv venv
uv pip install pytest ty
pytest
uv run ty check tests
```
Adding this makes ty and pytest both fail
```bash
touch tests/__init__.py
```

---

_Comment by @kuyugama on 2025-05-16 12:07_

Pytest adds directory with tests to sys.path. So all contents of the "tests"(in your case) can be imported without mentioning it. I don't know if `ty` aware of such case.

---

_Comment by @carljm on 2025-05-16 12:10_

I suspect that if you check your tests separately, from the `tests/` directory (or using `--project tests/`), this will work fine. (Well, except that then ty won't be able to find your non-test code!)

I'm not sure if we want to add a special case for pytest, but if we implement #179 I think it would just be a matter of adding `tests/` to `src.root` in your config.

---

_Comment by @carljm on 2025-05-16 12:12_

Oh, `--extra-search-path tests/` is probably another option that can work here right now.

---

_Comment by @AlexWaygood on 2025-05-16 13:38_

Possibly we could consider having the `src.root` setting default to `[".", "src/", "tests/"]` instead of just `[".", "src/"]`? The "src/" directory is "just" a convention, after all; it's not something mandated by the language that all Python source code needs to be in an `src/` directory. (It's pretty different to e.g. Rust in that way.) It seems plausible that there are nearly as many people using pytest and putting all their tests in `tests/` as there are people using the `src/` layout?

---

_Label `needs-mre` removed by @AlexWaygood on 2025-05-18 02:36_

---

_Label `help wanted` added by @AlexWaygood on 2025-05-19 16:12_

---

_Comment by @carljm on 2025-05-22 03:21_

I think we could add `tests/` to the default roots if it exists and if `tests/__init__.py` doesn't exist.

---

_Closed by @MichaReiser on 2025-05-26 08:08_

---
