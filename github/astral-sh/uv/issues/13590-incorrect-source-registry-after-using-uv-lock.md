---
number: 13590
title: Incorrect source registry after using uv lock with custom indexes
type: issue
state: closed
author: praisethedeviI
labels:
  - question
assignees: []
created_at: 2025-05-22T08:01:37Z
updated_at: 2025-05-24T13:31:28Z
url: https://github.com/astral-sh/uv/issues/13590
synced_at: 2026-01-07T13:12:18-06:00
---

# Incorrect source registry after using uv lock with custom indexes

---

_Issue opened by @praisethedeviI on 2025-05-22 08:01_

### Summary

When using a custom index configuration in pyproject.toml, the `uv lock` command incorrectly sets the source.registry for ALL packages to the custom index URL, even for packages that should come from PyPI

-----
### My toml configuration files

1. I have pyproject.toml
```toml
[project]
name = "my-project"
version = "1.0.0"
requires-python = "==3.11.*"
dependencies = [
    "assistant-core>=7.0.7,<8",
]


[tool.uv]
package = false

[[tool.uv.index]]
name = "assist-core"
# It's actually different domain with gitlab on it
url = "https://gitlab.com/api/v4/groups/949/-/packages/pypi/simple"
explicit = true

[tool.uv.sources]
assistant-core = { index = "assist-core" }
```

2. I use uv.toml or UV_INDEX with authentication:
```toml
[[index]]
name = "assist-core"
# different domain
url = "https://__token__:<TOKEN>@gitlab.com/api/v4/groups/949/-/packages/pypi/simple"
```

--------
### Expected behavior

* Only packages specified in `[tool.uv.sources]` should have the custom registry as their source
* All other packages should maintain `source = { registry = "https://pypi.org/simple" }`
* Manually correcting the lockfile and re-running `uv lock` should not revert the changes

-------
### Actual behavior

* All packages in the lockfile get `source = { registry = "https://gitlab.com/..." }`
* Manually correcting the registry to PyPI and re-running `uv lock` doesn't update the lockfile (suggesting uv doesn't detect this as a change that needs resolution)

-------
### Additional Context
* The issue occurs regardless of whether authentication is provided via uv.toml or UV_INDEX environment variable
* The workaround of manually editing the lockfile is not sustainable as it needs to be redone after every dependency update

-------
### Also:

After changing the index URL in uv.toml (but index URL in the pyproject.toml hasn't been changed) to `url = "https://__token__:<TOKEN>@gitlab.com/api/v4/packages/pypi/simple"`, the lockfile was fixed. However:
* `uv sync --locked` and `uv lock --check` report that the lockfile needs updating
* Meanwhile, `uv lock`, `uv lock --dry-run` and `uv sync --check` report no updates needed and don't modify the file



### Platform

Darwin 24.4.0 arm64

### Version

uv 0.7.3 (3c413f74b 2025-05-07)

### Python version

Python 3.11.11

---

_Label `bug` added by @praisethedeviI on 2025-05-22 08:01_

---

_Comment by @charliermarsh on 2025-05-22 11:37_

Why are you repeating the index definition across those two files? I don't think they'll get "merged" like that -- my guess is the above is as-if you _only_ defined:

```toml
[[index]]
name = "assist-core"
# different domain
url = "https://__token__:<TOKEN>@gitlab.com/api/v4/groups/949/-/packages/pypi/simple"
```

In which case, the index _isn't_ explicit.

---

_Label `bug` removed by @charliermarsh on 2025-05-22 11:37_

---

_Label `question` added by @charliermarsh on 2025-05-22 11:37_

---

_Comment by @praisethedeviI on 2025-05-22 14:41_

Writing `explicit = true` in _uv.toml_ helped me

I repeated because I can't leave my credentials in _pyproject.toml_, so I also tried to create gitignored _uv.toml_

Is there any way to get rid of the warning "Found both a uv.toml file and a [tool.uv] section in an adjacent pyproject.toml. The [tool.uv] section will be ignored in favor of the uv.toml file"?

If I leave the index only in uv.toml, I get an error: 
```
Caused by: Failed to parse entry: assistant-core
  Caused by: Package assistant-core references an undeclared index: `assist-core`
```

----------

Also, instead of using _uv.toml_ with a repeated index defenition, I tried use the variable `UV_INDEX=https://__token__:<TOKEN>@gitlab.com/api/v4/groups/949/-/packages/pypi/simple`. But `uv lock` still writes the wrong registry

I guess that UV_INDEX works as an index in the _toml_ files without explicit configuration


---

_Comment by @charliermarsh on 2025-05-24 12:22_

I would strongly suggest putting the index definition in your `pyproject.toml`, then setting:

```
export UV_INDEX_ASSIST_CORE_USERNAME= __token__
export UV_INDEX_ASSIST_CORE_PASSWORD=<token>
```

This is the recommended setup. (IIRC you're not allowed to reference explicit named indexes defined in a `uv.toml` file.)

---

_Closed by @praisethedeviI on 2025-05-24 13:31_

---
