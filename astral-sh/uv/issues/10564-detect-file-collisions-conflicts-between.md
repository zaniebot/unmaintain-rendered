```yaml
number: 10564
title: Detect file collisions / conflicts between different packages while installing
type: issue
state: open
author: mgorny
labels:
  - needs-decision
assignees: []
created_at: 2025-01-13T14:37:31Z
updated_at: 2025-04-06T06:23:51Z
url: https://github.com/astral-sh/uv/issues/10564
synced_at: 2026-01-12T16:00:16Z
```

# Detect file collisions / conflicts between different packages while installing

---

_@mgorny_

It is possible for two PyPI projects to provide files using the same names. When using `uv pip install`, installing both would result in `uv` silently overwriting files from the package installed earlier, and leaving over metadata for both.

For example, consider the [htmlmin](https://pypi.org/project/htmlmin/) packages and its [htmlmin2](https://pypi.org/project/htmlmin2/) fork. Both install a `htmlmin` Python package. If I install two packages: one needing `htmlmin` and another one forked to use `htmlmin2`, I'm going to end up with `htmlmin` from either of these packages (depending on installation order), and `.dist-info` for both of them — therefore both packages will assume that their dependencies are satisfied, although I'm going to effectively end up with files from one of them (or a mix of both).

I think it would be better if uv could detect when a newly installed package is attempting to overwrite files that were installed by another package before, and ideally print an error, indicating that both packages cannot be installed simultaneously.

It could also be worthwhile to consider a slightly different scenario when two packages e.g. install `multipart.py` and `multipart` directory (it was previously the case with [multipart](https://pypi.org/project/multipart/) and [python-multipart](https://pypi.org/project/python-multipart/) packages, though the latter finally renamed itself).

---

_Comment by @charliermarsh on 2025-01-13 15:26_

I don't know if we can do this, since some packages and ecosystems do this _intentionally_. It's common with Jupyter, for example, to install contents into overlapping directories.

---

_Comment by @mgorny on 2025-01-13 16:18_

Perhaps we could at least add a warning. I've never seen it done intentionally, however I've seen multiple times when people published packages using the same Python name either accidentally, or, well, deliberately but never realizing that packages could be installed in another order.

---

_Label `needs-decision` added by @zanieb on 2025-01-13 18:24_

---

_Comment by @eli-schwartz on 2025-04-06 06:23_

> I don't know if we can do this, since some packages and ecosystems do this _intentionally_. It's common with Jupyter, for example, to install contents into overlapping directories.

Installing contents into overlapping directories, as in effectively namespace packages? That is entirely fine and reasonable and within the specification for how packages work. Arguably this is even okay if one package secretly injects itself into another, non-namespace, package.

They should never install different file contents to the same *files* in overlapping directories. That's what is being discussed here.

(As a special exception, multiple packages can install the same `__init__.py` with morally identical contents, i.e. the only thing the file does is `pkgutil.extend_path()`. Those should be allowed despite file collisions.)

---
