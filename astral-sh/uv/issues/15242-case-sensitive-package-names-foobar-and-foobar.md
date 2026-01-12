```yaml
number: 15242
title: "case sensitive package names, \"foobar\" and \"FooBar\" are seen as same package"
type: issue
state: closed
author: SamuelMarks
labels:
  - question
assignees: []
created_at: 2025-08-12T16:07:47Z
updated_at: 2025-08-22T15:59:39Z
url: https://github.com/astral-sh/uv/issues/15242
synced_at: 2026-01-12T16:02:06Z
```

# case sensitive package names, "foobar" and "FooBar" are seen as same package

---

_@SamuelMarks_

### Summary

FWIW, this is with:
```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

I have different versions for each (one is a shim for the old upper-camelcase version; that will eventually be replaced by the new all-lowercase version):

```toml
[project]
name = "FooBar"
```

&

```toml
[project]
name = "foobar"
```

But I get:
```
tmp/foobar > uv pip install -e .                 
Using Python 3.12.11 environment at: .venvs/venv-3-12
      Built foobar @ file://tmp/foobar
Uninstalled 2 packages in 4ms
Installed 2 packages in 0.96ms
 ~ foobar==2025.8.8 (from file://tmp/foobar)
(venv-3-12) tmp/foobar > python3 -c 'import foobar; print(foobar.__version__)'
2025.08.08
(venv-3-12) tmp/foobar > python3 -c 'import FooBar; print(FooBar.__version__)'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'FooBar'
(venv-3-12) tmp/foobar > cd shim
(venv-3-12) tmp/foobar/shim > uv pip install -e .
Using Python 3.12.11 environment at: .venvs/venv-3-12
Resolved 1 package in 145ms
      Built foobar @ file://tmp/foobar/shim
Prepared 1 package in 54ms
Uninstalled 1 package in 0.57ms
Installed 1 package in 0.99ms
 - foobar==2025.8.8 (from file://tmp/foobar)
 + foobar==0.15.15 (from file://tmp/foobar/shim)
(venv-3-12) tmp/foobar/shim > python3 -c 'import FooBar; print(FooBar.__version__)'
0.15.15
(venv-3-12) tmp/foobar/shim > python3 -c 'import foobar; print(foobar.__version__)' 
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'foobar'
```

### Platform

Darwin Kernel Version 24.6.0

### Version

uv 0.8.8 (Homebrew 2025-08-09)

### Python version

3.12.11

---

_Label `bug` added by @SamuelMarks on 2025-08-12 16:07_

---

_Renamed from "case sensitive pacakge names, "foobar" and "FooBar" are seen as same package" to "case sensitive package names, "foobar" and "FooBar" are seen as same package" by @SamuelMarks on 2025-08-12 16:09_

---

_Comment by @charliermarsh on 2025-08-12 16:13_

I think this is correct behavior. Package names are normalized according to the [spec](https://packaging.python.org/en/latest/specifications/name-normalization/).

---

_Label `bug` removed by @zanieb on 2025-08-12 16:21_

---

_Label `question` added by @zanieb on 2025-08-12 16:21_

---

_Comment by @SamuelMarks on 2025-08-15 22:43_

@charliermarsh Interesting. I mean it is a question, say there's an old package in bad case `FooBar` and you wanted to switch to a new case `foobar` that is recommended. How would you deprecate the old in favour of the new? - Preferably with an intermediary period were both can be installed concurrently?

The only idea I could think of is to have a `foobar2` package that is maintained until everyone has stopped using `FooBar` and then they are merged into one. fabric / fabric2 did this many years ago.

---

_Comment by @charliermarsh on 2025-08-15 22:51_

Just to clarify, are you referring to the package name (as it appears on PyPI), or the module name (like `import foobar`)?


---

_Comment by @SamuelMarks on 2025-08-15 22:53_

Both. We had an idea at one point to upload FooBar to pypi and foobar to pypi; and eventually throw an error when installing FooBar telling people to install foobar instead (this assumes pypi is case sensitive… which I think it is)

---

_Comment by @charliermarsh on 2025-08-15 22:55_

Unfortunately PyPI won't allow you to upload both `FooBar` and `foobar`, because they conflict (they're "the same" package name per the canonicalization scheme I linked above). PyPI does respect casing when you upload, so e.g. Django is stylized as "Django" rather than "django". But, like, these redirect to the same package:

- https://pypi.org/project/Django/
- https://pypi.org/project/django/

---

_Comment by @charliermarsh on 2025-08-22 15:01_

I think I'm going to close for now since we're limited by the spec here.

---

_Closed by @charliermarsh on 2025-08-22 15:01_

---

_Comment by @SamuelMarks on 2025-08-22 15:59_

Ok yeah 🤷‍♂️ guess there's nothing that can be done to support this use-case.

Thanks for following-up

---
