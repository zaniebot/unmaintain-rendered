```yaml
number: 1320
title: Python 3.14 availability via --python-version flag
type: issue
state: closed
author: pkelaita
labels: []
assignees: []
created_at: 2025-10-07T20:07:59Z
updated_at: 2025-10-07T20:40:54Z
url: https://github.com/astral-sh/ty/issues/1320
synced_at: 2026-01-12T15:54:25Z
```

# Python 3.14 availability via --python-version flag

---

_@pkelaita_

### Summary

#### Description of Issue

Passing `3.14` into the `--python-version flag` (`uv run ty check some_dir/ --python-version 3.14`) results in the following error:
```
error: invalid value '3.14' for '--python-version <VERSION>'
  [possible values: 3.7, 3.8, 3.9, 3.10, 3.11, 3.12, 3.13]
```

However, ty itself does support python 3.14. If you specify 3.14 in the pyproject.toml (i.e., `requires-python = ">=3.14"`), running `uv run ty check some_dir/` with no version flag will correctly typecheck 3.14 features such as t-strings without throwing an error.

Thus, this seems to be an issue with the `--python-version` flag rather than with ty 3.14 compatibility itself. Importantly, this issue is only relevant in projects which must specify a version lower than 3.14 in the toml but still want to be able to typecheck certain subdirectories on 3.14 (e.g. in my case, the main package must be backwards-compatible to 3.9, but I'd like to use 3.14 features in external scripts directories).

I am using the latest versions of uv and ty:
- `uv 0.8.24 (Homebrew 2025-10-07)`
- `ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)`


#### Temporary Workaround

After some trial and error, I've found that running the following command allows for 3.14-compatible inline usage:
```
uv run ty check some_dir/ --config environment.python-version='"3.14"'
```

That said, it will be much cleaner to run this via the `--python-version` flag. Thanks! Love the astral ecosystem 💪



### Version

ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)

---

_Comment by @AlexWaygood on 2025-10-07 20:40_

Thanks! We actually landed support for this earlier today (in https://github.com/astral-sh/ruff/commit/88c0ce3e389a217dc596374f154e0c1e88b483ad), so it will be included in the next release

---

_Closed by @AlexWaygood on 2025-10-07 20:40_

---
