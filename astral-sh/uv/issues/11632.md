```yaml
number: 11632
title: "Allow `sources` override for `uv sync` / `uv run` by any means [command line, environment variable or config file]"
type: issue
state: open
author: jpedrick
labels:
  - enhancement
assignees: []
created_at: 2025-02-19T18:41:19Z
updated_at: 2025-12-03T08:19:10Z
url: https://github.com/astral-sh/uv/issues/11632
synced_at: 2026-01-10T03:23:53Z
```

# Allow `sources` override for `uv sync` / `uv run` by any means [command line, environment variable or config file]

---

_Issue opened by @jpedrick on 2025-02-19 18:41_

### Summary

Without over-specifying a solution, and related to #7945, I would like to be able to functionally place a `uv.toml` in my project directory to override sources/default groups/etc. I would exclude the `uv.toml` file from my git repository, so it would only be used for local development.

Currently, uv doesn't seem to allow merging of a `uv.toml` with a [tool.uv] section and it doesn't allow overriding project level settings:

```
warning: Found both a `uv.toml` file and a `[tool.uv]` section in an adjacent `pyproject.toml`. The `[tool.uv]` section will be ignored in favor of the `uv.toml` file.
error: Failed to parse: `uv.toml`. The `sources` field is not allowed in a `uv.toml` file. `sources` is only applicable in the context of a project, and should be placed in a `pyproject.toml` file instead.
```


Also, secondarily, it appears the `[tool.uv]` section contains both project level settings and global/user level settings, but you can only define it in one place.

This would be very useful for me in the context of working without a mono-repo in the context of several highly coupled projects. I want to temporarily override some packages as "editable," but I don't want to modify my `pyproject.toml` to do so. Specifying `[tool.uv.sources]` is also not ideal, as different developers may have their sources installed at different locations.

I also use direnv and Makefiles, so any suggestions using environment variables or command line arguements would be suitable.

An example `uv.toml` I would want:


```
default-groups = ["dev", "test"] # in pyproject.toml no default groups

[sources]
depa = { path = '${PROJECT_ROOT}/my-external-dep1/depa', editable=true }
depb = { path = '${PROJECT_ROOT}/my-external-dep2/depb', editable=true }
```


```
[project]
name = 'myproject'
dependencies = [
  'depa',
  'depb',
]

[dependency-groups]
dev = [
    'mypy',
    'ruff',
    'pre-commit'
]
test = [
    'pytest',
   'moto[server]',
]
...
```



### Example

_No response_

---

_Label `enhancement` added by @jpedrick on 2025-02-19 18:41_

---

_Comment by @nat-n on 2025-10-31 15:03_

I agree it would be useful to be able to  override paths in `tool.uv.sources` (e.g. via env var, CLI option, or separate config file) when calling `uv pip install`. My use case is when performing the operation inside a docker container, it can be complicated to replicate the relative paths of sources when mounting all the relevant projects into the container, and I don't want to have to modify the pyproject.toml, hence being able to override those paths would be a nice solution.

---

_Comment by @ondrados on 2025-11-16 10:51_

I’m running into the same issue, and an override mechanism would be extremely useful.

My scenario: one of my dependencies normally comes from a private index defined in `tool.uv.sources` and `tool.uv.index`. That works for normal installs, but when I’m iterating on that dependency I want to point it to a local checkout instead of publishing a new version every time.

What I need is a way to temporarily override the source — via environment variables, CLI flags, or a local config file — so I can switch between “use the private index” and “use my local path” without touching tracked project settings.

---

_Comment by @oelhammouchi on 2025-12-03 08:19_

I have the exact same problem, a fix would be very useful!

---
