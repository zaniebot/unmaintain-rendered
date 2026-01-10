```yaml
number: 12757
title: "Allow specifying `files` in persistent configuration"
type: issue
state: open
author: daniil-berg
labels:
  - wish
  - needs-design
assignees: []
created_at: 2024-08-08T15:54:31Z
updated_at: 2024-08-08T16:42:17Z
url: https://github.com/astral-sh/ruff/issues/12757
synced_at: 2026-01-10T11:09:54Z
```

# Allow specifying `files` in persistent configuration

---

_Issue opened by @daniil-berg on 2024-08-08 15:54_

It would be nice, if I could specify the files to check, format, etc. via the ruff configuration file (in my case `pyproject.toml`). I thought of something analogous to the [`files` directive of `mypy`](https://mypy.readthedocs.io/en/stable/config_file.html#confval-files).

Maybe I missed something, but I found no mention of this ability to specify files in the `ruff` documentation and found no issues about it, when I searched for the terms `files config` here.

Just trying out something like this gave me an error for commands like `ruff check` or `ruff format`:

```toml
# pyproject.toml
[tool.ruff]
files = ["src", "tests"]
# ...
```

```
ruff failed
  Cause: Failed to parse /path/to/pyproject.toml
  ...
unknown field `files`
```

I assume this is expected and not a bug. Therefore please consider this a humble feature request.

---

_Label `question` added by @MichaReiser on 2024-08-08 16:16_

---

_Comment by @MichaReiser on 2024-08-08 16:17_

You can use [`include`](https://docs.astral.sh/ruff/settings/#include) (and `extend-include)` and [`exclude`](https://docs.astral.sh/ruff/settings/#exclude) (and `extend-exclude`) to specify which files should be included when running check or format. 


---

_Comment by @charliermarsh on 2024-08-08 16:18_

I think this is a bit different @MichaReiser. In mypy, specifying `files` lets you run `mypy` without passing it any files (and it uses that list as the default). 

---

_Comment by @charliermarsh on 2024-08-08 16:19_

So the ask here is that you can set `files` in the configuration, and `ruff check` would default to checking those files (instead of `.`).

---

_Comment by @MichaReiser on 2024-08-08 16:26_

But I think that's roughly the same, isn't it?

> A comma-separated list of paths which should be checked by mypy if none are given on the command line. Supports recursive file globbing using [glob](https://docs.python.org/3/library/glob.html#module-glob), where * (e.g. *.py) matches files in the current directory and **/ (e.g. **/*.py) matches files in any directories below the current one. User home directory and environment variables will be expanded.

You can set `exclude` and `include` and that specifies the paths that Ruff checks by default. You can explicitly pass files to `ruff check` that aren't covered by the set defined by `exclude`/`include` and ruff will check those files (unless `force-exclude` is specified)

---

_Comment by @daniil-berg on 2024-08-08 16:31_

@MichaReiser Maybe I am misunderstanding the logic of those directives. Would you be so kind and provide the correct configuration to accomplish the following:
- `ruff check` (without any following positional arguments) should operate on the directories `src` and `tests` only.
- The default inclusion and exclusion patterns of ruff are applied unchanged.

---

_Comment by @MichaReiser on 2024-08-08 16:39_

Yeah you might be right. It might be possible but you would at least need to override `include` but then there's https://github.com/astral-sh/ruff/issues/9019

---

_Label `question` removed by @MichaReiser on 2024-08-08 16:42_

---

_Label `wish` added by @MichaReiser on 2024-08-08 16:42_

---

_Label `needs-design` added by @MichaReiser on 2024-08-08 16:42_

---
