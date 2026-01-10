---
number: 13876
title: uv pip install -r requirements.txt should always reinstall packages with a specified path
type: issue
state: closed
author: danra
labels:
  - question
assignees: []
created_at: 2025-06-06T00:01:14Z
updated_at: 2025-06-23T16:42:37Z
url: https://github.com/astral-sh/uv/issues/13876
synced_at: 2026-01-10T01:25:39Z
---

# uv pip install -r requirements.txt should always reinstall packages with a specified path

---

_Issue opened by @danra on 2025-06-06 00:01_

### Summary

`uv pip install -r requirements.txt` should always reinstall packages which have a specified path. Currently it apparently doesn't, which is different from the behavior for packages with a path specified on the command-line, which do always reinstall (following #12038). That's unexpected.

Verbosely:

```
uv pip install ./package
```
always re-installs the package.

In contrast, given requirements.txt with the content:
```
 ./package
```

running
```
uv pip install -r requirements.txt
```

does not always reinstall the package.


### Platform

Darwin 24.4.0 arm64

### Version

uv 0.7.11 (90a4416ab 2025-06-04)

### Python version

Python 3.13.1

---

_Label `bug` added by @danra on 2025-06-06 00:01_

---

_Comment by @charliermarsh on 2025-06-06 00:03_

Candidly this is pretty unlikely to change and has been talked about in a few other issues -- you can read about the caching behavior here: https://docs.astral.sh/uv/concepts/cache/#dependency-caching

---

_Comment by @danra on 2025-06-06 00:26_

@charliermarsh Indeed, that doc describes a path *on the command-line* as an explicit special case:

> As a special case, uv will always rebuild and reinstall any local directory dependencies passed explicitly on the command-line (e.g., uv pip install .).

But why? The same rationale of #12038 holds regardless of whether the dependency is specified on the command-line or requirements file:

> When dealin with local packages, uv is also caching the installation parameter making it very dificult when I work on package to run my tox or nox session with a uv backend as everytime I make a modification my local package in the venv are not updated. I fail to see a configuration where people would install a local package without wanting to update it as usually when the package is locally installed it means that you are currently working on it.

---

_Comment by @konstin on 2025-06-06 13:56_

For packages you change locally, you can use editable installs or `cache-keys` for compiled packages.

---

_Label `bug` removed by @charliermarsh on 2025-06-22 02:13_

---

_Label `question` added by @charliermarsh on 2025-06-22 02:13_

---

_Comment by @charliermarsh on 2025-06-22 02:22_

Yeah, I'd suggest editables in this case (or `cache-keys` to mark sources as invalidating the package).

---

_Closed by @charliermarsh on 2025-06-22 02:22_

---

_Comment by @danra on 2025-06-23 03:13_

It's likely others would stumble into this unexpected inconsistency as well, regardless of alternatives. That's why I suggest it might be a bug, though that depends on the trade-offs.

@charliermarsh you'd mentioned those trade-offs were discussed in other issues in addition to https://github.com/astral-sh/uv/issues/12038. Would you mind attaching the references to those?

---

_Comment by @danra on 2025-06-23 16:42_

I think I actually see the main problem myself - one wouldn't want `uv run ...` to unconditionally re-build local paths specified in the project.

---
