```yaml
number: 14549
title: No solution found when resolving dependencies for split
type: issue
state: open
author: bra-fsn
labels:
  - bug
assignees: []
created_at: 2025-07-10T20:33:10Z
updated_at: 2025-07-12T16:56:12Z
url: https://github.com/astral-sh/uv/issues/14549
synced_at: 2026-01-10T03:32:45Z
```

# No solution found when resolving dependencies for split

---

_Issue opened by @bra-fsn on 2025-07-10 20:33_

### Summary

I have a strange error, which drives me mad.
`pip install` works, `uv pip install` works locally, it also works "manually" in a GitHub workflow environment (by entering the box and issuing the same command as the workflow step), but it fails when I try to execute it in the workflow.
About the environment:
I'm trying to set up an uv venv in a Github workflow to run pytest for a package.
To add to the mix, I'm using [this](https://github.com/astral-sh/uv/issues/11097) feature, that is: two private registries with our proxy (which returns authenticated redirects). One of the dependencies in the tested package is in these private repos.
I don't think this is the cause of the error, just mentioning it.

The package to be tested is installed this way:
```shell
uv venv
uv --verbose pip install $pip_args -e .[test]
```

`$pip_args` contains the two extra index urls (--extra-index-url https://proxy1 --extra-index-url https://proxy2).

I've saved the two outputs:
https://gist.github.com/bra-fsn/f185e932e7a1ac0259f682be073f9f7d
works.txt is from a python:3.12 container
doesntwork.txt is from the github action's output

`REDACTED_TESTED_PKGNAME` is the package name of the tested package (installed locally, so that's the `.` in `pip install`.
`REDACTED_DEPENDENT_LOCAL_PKG` is a dependency of the tested package (special because it's in a local repo, available only via the proxy)
`REDACTED_REPO_NAME` is the AWS CodeArtifact repo (where the proxy redirects local packages)

I've tried to put the wheel to a HTTP server instead to make sure our proxying is not the cause (but again: it works from a local container and even in the GHA workflow environment if I'm trying to install it manually) and voila, the error message has changed to this:

```
DEBUG Released lock at `/home/runner/_work/package-REDACTED_TESTED_PKGNAME/package-REDACTED_TESTED_PKGNAME/.venv/.lock`
  × No solution found when resolving dependencies:
  ╰─▶ Because the requested Python version (>=3.8) does not satisfy
      Python>=3.9.0 and REDACTED_DEPENDENT_LOCAL_PKG==1.2.6 depends on Python>=3.9.0, we can
      conclude that REDACTED_DEPENDENT_LOCAL_PKG==1.2.6 cannot be used.
      And because only REDACTED_DEPENDENT_LOCAL_PKG==1.2.6 is available, we can conclude that
      all versions of REDACTED_DEPENDENT_LOCAL_PKG cannot be used.
      And because your project depends on REDACTED_DEPENDENT_LOCAL_PKG and your project
      requires REDACTED_TESTED_PKGNAME[test], we can conclude that your project's
      requirements are unsatisfiable.

      hint: The `requires-python` value (>=3.8) includes Python versions that
      are not supported by your dependencies (e.g., REDACTED_DEPENDENT_LOCAL_PKG==1.2.6 only
      supports >=3.9.0). Consider using a more restrictive `requires-python`
      value (like >=3.9.0).
```

So `REDACTED_TESTED_PKGNAME` has `requires-python = ">=3.8"` in its `pyproject.toml` file, while `REDACTED_DEPENDENT_LOCAL_PKG` has `python_requires=">=3.9.0"` in its `setup.py`.

(sorry, I just noticed that `DEPENDENT` should be `DEPENDENCY`, as it's the dependency of the `TESTED_PKGNAME`)

If I change `3.8` to `3.9`, the github version starts to work.
I'm puzzled:

1. why doesn't uv prints this clear error message if I have the package name (from a private repo, so coming from an --extra-index-url)
2. why does it work in two places and doesn't in the automated github workflow?
3. is this a real error on python 3.12? A package to be installed requires >=3.8, one of its dependencies need >=3.9. I would assume this is just fine.

### Platform

Ubuntu 24.04 amd64

### Version

0.7.20

### Python version

3.12.11

---

_Label `bug` added by @bra-fsn on 2025-07-10 20:33_

---

_Comment by @charliermarsh on 2025-07-12 12:10_

Are you running `uv lock`, or `uv sync`, or `uv run` somewhere? That error message suggests we're performing a universal solve, which would be the case for those commands, but not for `uv pip install`.

---

_Comment by @bra-fsn on 2025-07-12 13:35_

> Are you running `uv lock`, or `uv sync`, or `uv run` somewhere? That error message suggests we're performing a universal solve, which would be the case for those commands, but not for `uv pip install`.

No, only `uv venv` and then `uv pip install`.

---

_Comment by @charliermarsh on 2025-07-12 15:08_

Can you share the full verbose logs?

---

_Comment by @bra-fsn on 2025-07-12 16:56_

> Can you share the full verbose logs?

This should be the full one. (https://gist.githubusercontent.com/bra-fsn/f185e932e7a1ac0259f682be073f9f7d/raw/c3aced392cde9c62f39306aac909cbeb8c05b6fc/doesntwork.txt)
Can you see something that is missing? I've downloaded it straight from github.

---
