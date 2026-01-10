---
number: 16547
title: uv build backend lowercases package name in wheel metadata
type: issue
state: closed
author: john-hen
labels:
  - bug
  - build-backend
assignees: []
created_at: 2025-11-01T12:25:28Z
updated_at: 2025-11-07T20:03:11Z
url: https://github.com/astral-sh/uv/issues/16547
synced_at: 2026-01-10T01:26:07Z
---

# uv build backend lowercases package name in wheel metadata

---

_Issue opened by @john-hen on 2025-11-01 12:25_

### Summary

When using the `uv_build` build backend to create a distribution wheel, the package name is lowercased / normalized even though it shouldn't be.

For example, with a project that has just `src/foo/__init__.py` and this `pyproject.toml`
```toml
[project]
name = "Foo"
version = "1.0.0"
description = "Foos the foo."

[build-system]
requires      = ["uv_build == 0.9.7"]
build-backend = "uv_build"
```

running `uv build --wheel` produces `dist/foo-1.0.0-py3-none-any.whl` that when inspected as a `.zip` archive contains this `METADATA` file:
```
Metadata-Version: 2.3
Name: foo
Version: 1.0.0
Summary: Foos the foo.
```

The project name is incorrectly lowercased to "foo" when it was specified as "Foo".

Other build backends don't do that. For example, when using Hatchling with
```toml
[build-system]
requires = ["hatchling == 1.27.0"]
build-backend = "hatchling.build"
```

or Flit with
```toml
[build-system]
requires      = ["flit_core == 3.12.0"]
build-backend = "flit_core.buildapi"

[tool.flit.module]
name = "foo"
```

In those cases, the `METADATA` file inside the wheel contains the **expected**
```
Metadata-Version: 2.4
Name: Foo
Version: 1.0.0
Summary: Foos the foo.
```

[Normalization] "should be done before lookups and comparisons", i.e., packages with the same normalized name are considered to be identical. But here it is applied to metadata upon publication. This is quite surprising when coming from other build backends, especially as it's typically only noticed after pushing to PyPI and thus can only be fixed in a subsequent release.

[Normalization]: https://packaging.python.org/en/latest/specifications/name-normalization


### Platform

Ubuntu 24.04 and Windows 11

### Version

uv 0.9.7

### Python version

_No response_

---

_Label `bug` added by @john-hen on 2025-11-01 12:25_

---

_Comment by @caschb on 2025-11-01 14:26_

This seems to be done on purpose, as the metadata for the name is saved as a PackageName which automatically normalizes the string.

https://github.com/astral-sh/uv/blob/101d0e28925c3f27ca31b5c2557fc32ed9bba29f/crates/uv-build-backend/src/metadata.rs#L582

https://github.com/astral-sh/uv/blob/101d0e28925c3f27ca31b5c2557fc32ed9bba29f/crates/uv-normalize/src/package_name.rs#L11-L16

I agree with you that this is incorrect/unnecessary according to the packaging documentation
Even in their [example here](https://packaging.python.org/en/latest/specifications/core-metadata/#name) they use an unnormalized string for the `Name` field.



---

_Comment by @caschb on 2025-11-01 17:18_

I have an initial solution for this, if the uv folks do consider this a bug and are interested I could open a draft PR and we could work from there, just let me know.

---

_Comment by @charliermarsh on 2025-11-01 19:40_

I think we're unlikely to change our use of normalized names _in general_ (it is intentional) but we could address this case if it's not too invasive.

---

_Referenced in [astral-sh/uv#16548](../../astral-sh/uv/pulls/16548.md) on 2025-11-01 20:37_

---

_Comment by @caschb on 2025-11-01 20:39_

> I think we're unlikely to change our use of normalized names _in general_ (it is intentional) but we could address this case if it's not too invasive.

Ok, I created a PR so you can judge if it's appropriate for the project.
I made sure the change didn't touch anything outside this one case.

---

_Comment by @charliermarsh on 2025-11-01 21:08_

Thank you for the clear PR and test case.

---

_Label `build-backend` added by @konstin on 2025-11-01 21:32_

---

_Comment by @charliermarsh on 2025-11-02 18:27_

Fixed in https://github.com/astral-sh/uv/pull/16548.

---

_Closed by @charliermarsh on 2025-11-02 18:27_

---
