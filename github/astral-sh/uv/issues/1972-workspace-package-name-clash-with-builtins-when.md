---
number: 1972
title: Workspace package name clash with builtins - when uv is enabled
type: issue
state: closed
author: mihalikv
labels:
  - bug
assignees: []
created_at: 2024-02-25T21:21:13Z
updated_at: 2024-02-28T15:43:14Z
url: https://github.com/astral-sh/uv/issues/1972
synced_at: 2026-01-07T13:12:16-06:00
---

# Workspace package name clash with builtins - when uv is enabled

---

_Issue opened by @mihalikv on 2024-02-25 21:21_

### Steps to Reproduce

1. clone repository https://github.com/mihalikv/improved-umbrella
2. run `rye sync`

### Expected Result

**With uv**: successful installation


### Actual Result

When **uv** is disabled: it just works.


When **uv** is enabled:
```
vilo@Vilo:~/test_project$ rye sync
Reusing already existing virtualenv
Generating production lockfile: /home/vilo/test_project/requirements.lock
error: Failed to build editables
  Caused by: Failed to build editable: file:///home/vilo/test_project/nested_lib
  Caused by: Build backend failed to build wheel through `build_editable()`:
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 6, in <module>
  File "/home/vilo/.cache/uv/.tmp30t7GI/.venv/lib/python3.11/site-packages/hatchling/build.py", line 82, in build_editable
    return os.path.basename(next(builder.build(directory=wheel_directory, versions=['editable'])))
                            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/vilo/.cache/uv/.tmp30t7GI/.venv/lib/python3.11/site-packages/hatchling/builders/plugin/interface.py", line 90, in build
    self.metadata.validate_fields()
  File "/home/vilo/.cache/uv/.tmp30t7GI/.venv/lib/python3.11/site-packages/hatchling/metadata/core.py", line 244, in validate_fields
    self.core.validate_fields()
  File "/home/vilo/.cache/uv/.tmp30t7GI/.venv/lib/python3.11/site-packages/hatchling/metadata/core.py", line 1334, in validate_fields
    getattr(self, attribute)
  File "/home/vilo/.cache/uv/.tmp30t7GI/.venv/lib/python3.11/site-packages/hatchling/metadata/core.py", line 1207, in dependencies
    self._dependencies = list(self.dependencies_complex)
                              ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/vilo/.cache/uv/.tmp30t7GI/.venv/lib/python3.11/site-packages/hatchling/metadata/core.py", line 1158, in dependencies_complex
    from packaging.requirements import InvalidRequirement, Requirement
  File "/home/vilo/.cache/uv/.tmp30t7GI/.venv/lib/python3.11/site-packages/packaging/requirements.py", line 7, in <module>
    from ._parser import parse_requirement as _parse_requirement
  File "/home/vilo/.cache/uv/.tmp30t7GI/.venv/lib/python3.11/site-packages/packaging/_parser.py", line 10, in <module>
    from ._tokenizer import DEFAULT_RULES, Tokenizer
  File "/home/vilo/.cache/uv/.tmp30t7GI/.venv/lib/python3.11/site-packages/packaging/_tokenizer.py", line 6, in <module>    from .specifiers import Specifier
  File "/home/vilo/.cache/uv/.tmp30t7GI/.venv/lib/python3.11/site-packages/packaging/specifiers.py", line 26, in <module>
    from .utils import canonicalize_version
  File "/home/vilo/.cache/uv/.tmp30t7GI/.venv/lib/python3.11/site-packages/packaging/utils.py", line 8, in <module>
    from .tags import Tag, parse_tag
  File "/home/vilo/.cache/uv/.tmp30t7GI/.venv/lib/python3.11/site-packages/packaging/tags.py", line 27, in <module>
    logger = logging.getLogger(__name__)
             ^^^^^^^^^^^^^^^^^
AttributeError: module 'logging' has no attribute 'getLogger'
---
error: could not write production lockfile for workspace

Caused by:
    failed to generate lockfile
```

### Version Info

```
rye 0.26.0
commit: 0.26.0 (d245f625e 2024-02-23)
platform: linux (x86_64)
self-python: cpython@3.12
symlink support: true
uv enabled: false
```

### Stacktrace

_No response_

---

_Renamed from "Workspace dependencies clash with builtins" to "Workspace dependencies clash with builtins - when uv is enabled" by @mihalikv on 2024-02-25 21:21_

---

_Renamed from "Workspace dependencies clash with builtins - when uv is enabled" to "Workspace package name clash with builtins - when uv is enabled" by @mihalikv on 2024-02-25 21:22_

---

_Comment by @mitsuhiko on 2024-02-25 21:40_

This would be a bug in uv. Can be reproduced from that repo:

```
$ uv pip compile requirements.lock
error: Failed to build editables
  Caused by: Failed to build editable: file:///private/tmp/improved-umbrella/nested_lib
  Caused by: Build backend failed to build wheel through `build_editable()`:
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 6, in <module>
  File "/Users/mitsuhiko/Library/Caches/uv/.tmpAQYEa4/.venv/lib/python3.11/site-packages/hatchling/build.py", line 82, in build_editable
    return os.path.basename(next(builder.build(directory=wheel_directory, versions=['editable'])))
                            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/mitsuhiko/Library/Caches/uv/.tmpAQYEa4/.venv/lib/python3.11/site-packages/hatchling/builders/plugin/interface.py", line 90, in build
    self.metadata.validate_fields()
  File "/Users/mitsuhiko/Library/Caches/uv/.tmpAQYEa4/.venv/lib/python3.11/site-packages/hatchling/metadata/core.py", line 244, in validate_fields
    self.core.validate_fields()
  File "/Users/mitsuhiko/Library/Caches/uv/.tmpAQYEa4/.venv/lib/python3.11/site-packages/hatchling/metadata/core.py", line 1334, in validate_fields
    getattr(self, attribute)
  File "/Users/mitsuhiko/Library/Caches/uv/.tmpAQYEa4/.venv/lib/python3.11/site-packages/hatchling/metadata/core.py", line 1207, in dependencies
    self._dependencies = list(self.dependencies_complex)
                              ^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/mitsuhiko/Library/Caches/uv/.tmpAQYEa4/.venv/lib/python3.11/site-packages/hatchling/metadata/core.py", line 1158, in dependencies_complex
    from packaging.requirements import InvalidRequirement, Requirement
  File "/Users/mitsuhiko/Library/Caches/uv/.tmpAQYEa4/.venv/lib/python3.11/site-packages/packaging/requirements.py", line 7, in <module>
    from ._parser import parse_requirement as _parse_requirement
  File "/Users/mitsuhiko/Library/Caches/uv/.tmpAQYEa4/.venv/lib/python3.11/site-packages/packaging/_parser.py", line 10, in <module>
    from ._tokenizer import DEFAULT_RULES, Tokenizer
  File "/Users/mitsuhiko/Library/Caches/uv/.tmpAQYEa4/.venv/lib/python3.11/site-packages/packaging/_tokenizer.py", line 6, in <module>
    from .specifiers import Specifier
  File "/Users/mitsuhiko/Library/Caches/uv/.tmpAQYEa4/.venv/lib/python3.11/site-packages/packaging/specifiers.py", line 26, in <module>
    from .utils import canonicalize_version
  File "/Users/mitsuhiko/Library/Caches/uv/.tmpAQYEa4/.venv/lib/python3.11/site-packages/packaging/utils.py", line 8, in <module>
    from .tags import Tag, parse_tag
  File "/Users/mitsuhiko/Library/Caches/uv/.tmpAQYEa4/.venv/lib/python3.11/site-packages/packaging/tags.py", line 27, in <module>
    logger = logging.getLogger(__name__)
             ^^^^^^^^^^^^^^^^^
AttributeError: module 'logging' has no attribute 'getLogger'
---
```

Contents of requirements.txt:

```
# generated by rye
# use `rye lock` or `rye sync` to update this lockfile
#
# last locked with the following flags:
#   pre: false
#   features: []
#   all-features: false
#   with-sources: false

-e file:nested_lib
-e file:.
asgiref==3.7.2
    # via django
django==5.0.2
    # via nested-lib
sqlparse==0.4.4
    # via django
```

---

_Label `bug` added by @charliermarsh on 2024-02-25 21:44_

---

_Comment by @charliermarsh on 2024-02-25 21:44_

Thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-25 23:17_

---

_Comment by @charliermarsh on 2024-02-25 23:23_

Not clear to me yet why this works with `pip`. The hook is called with `./improved-umbrella/nested_lib` as the current working directory, and Python adds the current working directory to the path.

---

_Comment by @charliermarsh on 2024-02-25 23:30_

@henryiii -- do you know have an understanding of why this works? `./improved-umbrella/nested_lib` contains a module called `logging`, which the build backend is importing while building.

`python -m build .` fails in that directory, but `python -m build nested_lib` succeeds from the parent.


---

_Comment by @charliermarsh on 2024-02-25 23:34_

I see some code in `pip` to avoid adding the current working directory:

```python
# Remove '' and current working directory from the first entry
# of sys.path, if present to avoid using current directory
# in pip commands check, freeze, install, list and show,
# when invoked as python -m pip <command>
if sys.path[0] in ("", os.getcwd()):
    sys.path.pop(0)
```

I guess that would end up affecting pip's PEP 517 builds, assuming they happen in the same process?

---

_Comment by @henryiii on 2024-02-25 23:38_

The current directory should not be present unless the build backend adds it. `setuptools.build_meta` does not, while `setuptools.build_meta.__legacy__` does.

I believe you can't stop Python from adding the current directory (before/without PYTHONSAFEPATH) in module mode, so you'd probably need a hack like pip's, which build might miss. But I'd have to check the package build backend first (on phone).

---

_Comment by @charliermarsh on 2024-02-25 23:46_

This makes sense. So we need to set the current directory, but avoid adding it to the path. (I’m not totally sure how pip avoids this since that code is in __main__.py, which I thought only ran when invoked via python -m.)

---

_Comment by @henryiii on 2024-02-26 00:07_

If you run “pytest”, then the current directory is not added to the path. But if you run `python -m pytest`, it is there (added by Python). You can try it out, pytest does not try to filter it. Pretty sure we don’t in build either.

---

_Comment by @henryiii on 2024-02-26 00:10_

This is an extremely rare issue for building packages, since most tools would expect to install “logging” as a top level package, which would clash with the std lib. A subpackage logging is fine, but not a top level one (in a wheel).

---

_Referenced in [astral-sh/uv#1975](../../astral-sh/uv/pulls/1975.md) on 2024-02-26 01:02_

---

_Closed by @charliermarsh on 2024-02-26 01:14_

---

_Comment by @henryiii on 2024-02-27 14:37_

FYI, `python -m venv .venv` doesn't work in the directory either, due to Python putting it at the top of the PATH. This means `pipx run` can't be used in this directory either. Also, it seems runpy triggers the import before it loads `__main__`, so not sure it's something we can even work around in build.

---

_Referenced in [pypa/build#741](../../pypa/build/pulls/741.md) on 2024-02-27 14:39_

---

_Comment by @henryiii on 2024-02-28 15:43_

Easiest way to fix this actually is to run `python -Im build`.

---
