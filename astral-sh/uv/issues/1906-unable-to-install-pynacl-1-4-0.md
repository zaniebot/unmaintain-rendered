```yaml
number: 1906
title: "Unable to install `pynacl==1.4.0`"
type: issue
state: closed
author: srxavi
labels:
  - compatibility
assignees: []
created_at: 2024-02-23T09:23:17Z
updated_at: 2024-02-23T19:47:59Z
url: https://github.com/astral-sh/uv/issues/1906
synced_at: 2026-01-12T15:58:33Z
```

# Unable to install `pynacl==1.4.0`

---

_@srxavi_

When executing `uv pip install pynacl==1.4.0` i get the following ouptut:

```
Resolved 4 packages in 2.33s
error: Failed to download distributions
  Caused by: Failed to fetch wheel: pynacl==1.4.0
  Caused by: Failed to build: pynacl==1.4.0
  Caused by: Invalid pyproject.toml
  Caused by: TOML parse error at line 3, column 12
  |
3 | requires = [
  |            ^^^^^^^^^^^^
Expected a valid marker name, found 'python_implementation'
cffi>=1.4.1; python_implementation != 'PyPy'
```

**Platform**: MacOS Sonoma 14.3.1 on M2 Pro
**Version**: 0.1.9

---

_Comment by @konstin on 2024-02-23 14:23_

The pyproject.toml in is not spec compliant, it's `platform_python_implementation` not `python_implementation`. This is a legacy value that is supported by some python tooling outside the spec, we could consider supporting it too.

```
[build-system]
# Must be kept in sync with `setup_requirements` in `setup.py`
requires = [
    "setuptools>=40.8.0",
    "wheel",
    "cffi>=1.4.1; python_implementation != 'PyPy'",
]
build-backend = "setuptools.build_meta"
```

The error message is unfortunately misplaced, but that seems like a serde or toml bug.

---

_Label `compatibility` added by @konstin on 2024-02-23 14:23_

---

_Comment by @charliermarsh on 2024-02-23 15:13_

@konstin - Do you have a reference that we can include for `python_implementation`?

---

_Comment by @konstin on 2024-02-23 18:19_

There's two compatibility layers: The more popular one, https://peps.python.org/pep-0345/#environment-markers

This one comes from https://github.com/pypa/packaging/issues/72 / https://github.com/pypa/packaging/pull/73/files though, which is now just https://github.com/pypa/packaging/blob/7bcd6d8ec33f0a614f9d017d31be4b50ece1549a/src/packaging/_parser.py#L326-L330

As usual, i advocate for emitting a warning when we install such as a package, so that this can be fixed upstream.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-23 19:14_

---

_Comment by @alex on 2024-02-23 19:29_

Huh. TIL. I'll send PRs to our packages to use `platform_python_implementation`.

---

_Closed by @charliermarsh on 2024-02-23 19:47_

---
