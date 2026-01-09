---
number: 12424
title: Bug in pre-release compatibility
type: issue
state: closed
author: schlamar
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-03-24T10:31:31Z
updated_at: 2025-06-22T02:24:35Z
url: https://github.com/astral-sh/uv/issues/12424
synced_at: 2026-01-07T13:12:18-06:00
---

# Bug in pre-release compatibility

---

_Issue opened by @schlamar on 2025-03-24 10:31_

### Summary

We get an error (these are internal packages)

> No solution found when resolving dependencies
> Because only lib<1.8.0b1 is available and X>=10.0.0 depends on lib>=1.8.0b1, we can conclude that X>=10.0.0 cannot be used 
> hint: `lib` was requested with a pre-release-marker (...), but pre-releases weren't enabled

According to the [docs](https://docs.astral.sh/uv/pip/compatibility/#pre-release-compatibility), pre-releases should be implicitly enabled if it is a direct dependency:

> If the package is a direct dependency, and its version markers include a pre-release specifier

lib is a direct dependency of X in pyproject.toml:

```toml
dependencies = [
    "lib>=1.8.0b1,<2",
]
```

So I would expect that `uv pip install` works without `--prerelease allow`

My guess is that uv does not correctly detect that this is a direct dependency with pre-release specifier as we are using a range specifier (`<2`).

### Platform

Windows

### Version

0.6.9

### Python version

_No response_

---

_Label `bug` added by @schlamar on 2025-03-24 10:31_

---

_Comment by @charliermarsh on 2025-03-24 13:09_

Is there any way to create a minimal reproduction here?

---

_Label `needs-mre` added by @charliermarsh on 2025-03-24 14:33_

---

_Comment by @schlamar on 2025-03-26 08:53_

My guess was wrong - this has nothing to do with range specifier. There is a general bug here. 

Attached 4 wheels to reproduce this issue. These were produced with a minimal pyproject.toml and no Python code with `python -m build --wheel`:

```toml
[build-system]
requires = ["setuptools>=61.0"]
build-backend = "setuptools.build_meta"

[project]
name = "lib"
requires-python = ">=3.7"
version = "1.7.0"
dependencies = [
    # "lib>=1.8.0b1",
]
```

Y has no range specifier as dependecy (`"lib>=1.8.0b1"`). X has range specifier (`"lib>=1.8.0b1,<2"`). Both fail:

```
> uv venv && uv pip install --offline --no-cache -f dist "X>=10.0dev0"
Using CPython 3.13.1
Creating virtual environment at: .venv
Activate with: .venv\Scripts\activate
  × No solution found when resolving dependencies:
  ╰─▶ Because only lib<1.8.0b1 is available and x==10.0.0 depends on lib>=1.8.0b1, we can conclude that x==10.0.0 cannot be used.
      And because only x==10.0.0 is available and you require x==10.0.0, we can conclude that your requirements are unsatisfiable.

      hint: `lib` was requested with a pre-release marker (e.g., lib>=1.8.0b1), but pre-releases weren't enabled (try: `--prerelease=allow`)

> uv venv && uv pip install --offline --no-cache -f dist "Y>=10.0dev0" 
Using CPython 3.13.1
Creating virtual environment at: .venv
Activate with: .venv\Scripts\activate
  × No solution found when resolving dependencies:
  ╰─▶ Because only lib<1.8.0b1 is available and y==10.0.0 depends on lib>=1.8.0b1, we can conclude that y==10.0.0 cannot be used.
      And because only y==10.0.0 is available and you require y==10.0.0, we can conclude that your requirements are unsatisfiable.

      hint: `lib` was requested with a pre-release marker (e.g., lib>=1.8.0b1), but pre-releases weren't enabled (try: `--prerelease=allow`)
```

The issue is not reproducible, if lib-1.7.0 wheel is missing.

[dist.zip](https://github.com/user-attachments/files/19463390/dist.zip)

---

_Comment by @charliermarsh on 2025-03-26 13:07_

How are you telling uv where to find X, Y, and lib? Are you using `--find-links`? Are they on your registry?

---

_Comment by @charliermarsh on 2025-03-26 13:10_

Why would installing `X` work? It depends on `Requires-Dist: lib<2,>=1.8.0b1`, but you didn't enable pre-releases, and the only wheel in that range is `1.8.0b1`.

---

_Comment by @schlamar on 2025-03-26 15:21_

Your documentation says that pre-releases are implicitly enabled if a pre-release is a direct dependency of a requested package. In this case lib is a direct dependency of X and the version markers has a pre-release specifier

> If the package is a direct dependency, and its version markers include a pre-release specifier

https://docs.astral.sh/uv/pip/compatibility/#pre-release-compatibility

---

_Comment by @schlamar on 2025-03-26 15:22_

> How are you telling uv where to find X, Y, and lib? Are you using `--find-links`? Are they on your registry?

Yes, see my example. I use `-f` == `--find-links` to the dist directory (which is dist.zip extracted).

---

_Comment by @charliermarsh on 2025-03-26 15:32_

It looks like `lib` is a direct dependency of `X`, but it's a transitive dependency of "your project". It's like if `requests` was published to PyPI with a dependency specifier that contained a pre-release marker. If you ran `uv pip install requests`, we wouldn't consider that dependency of `requests` to be a "direct" dependency.

---

_Comment by @schlamar on 2025-03-27 07:32_

So if I understand this correctly, "direct dependency" in this context means the package requested by `uv pip install <PACKAGE>` and not dependencies of "package"?

---

_Comment by @charliermarsh on 2025-03-27 13:00_

Yeah, with some nuance for local source trees. If you do `uv pip install .` or `uv pip install ./path/to/project` or similar, then the dependencies of that source tree (at `.` or `./path/to/project`) will be considered direct dependencies.

---

_Comment by @schlamar on 2025-03-27 17:47_

Ok thanks for clarification. Maybe you want to add this to the docs?

---

_Closed by @charliermarsh on 2025-06-22 02:24_

---
