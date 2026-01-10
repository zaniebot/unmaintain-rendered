```yaml
number: 15040
title: "uv-build: default glob for `license-files`"
type: issue
state: open
author: DimitriPapadopoulos
labels:
  - enhancement
  - build-backend
assignees: []
created_at: 2025-08-03T09:04:14Z
updated_at: 2025-08-04T10:57:20Z
url: https://github.com/astral-sh/uv/issues/15040
synced_at: 2026-01-10T03:32:46Z
```

# uv-build: default glob for `license-files`

---

_Issue opened by @DimitriPapadopoulos on 2025-08-03 09:04_

### Summary

How about providing a default glob when `license-files` is omitted from `pyproject.toml`, like other backends?

### Example

* ### setuptools
  **[setuptools/setuptools/dist.py](https://github.com/pypa/setuptools/blob/3544de73b3662a27fac14d8eb9f5c841668d66de/setuptools/dist.py#L594-L597)**
  Lines 594 to 597 in [3544de7](https://github.com/pypa/setuptools/commit/3544de73b3662a27fac14d8eb9f5c841668d66de):
  ```python
              # Default patterns match the ones wheel uses
              # See https://wheel.readthedocs.io/en/stable/user_guide.html
              # -> 'Including license files in the generated wheel file'
              patterns = ['LICEN[CS]E*', 'COPYING*', 'NOTICE*', 'AUTHORS*']
  ```
* ### hatch 
  **[hatch/backend/src/hatchling/metadata/core.py](https://github.com/pypa/hatch/blame/28f233c535508247ffa9a40dab82c1d1cb2b700b/backend/src/hatchling/metadata/core.py#L746)**
  Line 749 in [28f233c](https://github.com/pypa/hatch/commit/28f233c535508247ffa9a40dab82c1d1cb2b700b)
  ```python
                  globs = ['LICEN[CS]E*', 'COPYING*', 'NOTICE*', 'AUTHORS*']
  ```
* ### flit
  **[flit/flit_core/flit_core/config.py](https://github.com/pypa/flit/blob/7d9a070a44f7516eab6095505d80d93f13320388/flit_core/flit_core/config.py#L77)**
  Line 77 in [7d9a070](https://github.com/pypa/flit/commit/7d9a070a44f7516eab6095505d80d93f13320388)
  ```python
  default_license_files_globs = ['COPYING*', 'LICEN[CS]E*']
  ```

---

_Label `enhancement` added by @DimitriPapadopoulos on 2025-08-03 09:04_

---

_Comment by @DimitriPapadopoulos on 2025-08-03 09:08_

See section [Creating a LICENSE](https://packaging.python.org/en/latest/tutorials/packaging-projects/#creating-a-license) of the [Python Packaging User Guide](https://packaging.python.org/en/latest/):
> Most build backends automatically include license files in packages. See your backend’s documentation for more details.

---

_Comment by @hoechenberger on 2025-08-03 12:32_

I actually like the explicit approach one currently needs to take. Instead of using a default include, `uv` could issue a warning about the missing license file 🤔 

---

_Comment by @cbrnr on 2025-08-03 12:36_

Poetry: https://python-poetry.org/docs/libraries/#packaging

---

_Comment by @DimitriPapadopoulos on 2025-08-03 12:36_

This is not about forbidding the explicit approach, but allowing the implicit approach.

This is also not about personal preferences, but consistency across the ecosystem.

---

_Label `build-backend` added by @konstin on 2025-08-04 10:57_

---
