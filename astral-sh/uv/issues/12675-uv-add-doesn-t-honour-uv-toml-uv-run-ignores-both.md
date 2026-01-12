```yaml
number: 12675
title: "`uv add` doesn't honour `uv.toml`; `uv run` ignores both `uv.toml` and `--extra-index-url` flags"
type: issue
state: closed
author: knguyen1
labels:
  - bug
assignees: []
created_at: 2025-04-04T15:48:56Z
updated_at: 2025-04-04T16:18:12Z
url: https://github.com/astral-sh/uv/issues/12675
synced_at: 2026-01-12T16:01:10Z
```

# `uv add` doesn't honour `uv.toml`; `uv run` ignores both `uv.toml` and `--extra-index-url` flags

---

_@knguyen1_

### Summary

## Adding a package

Without `--extra-index-url`, it fails:

```
(my-project) vscode ➜ /workspaces/my-project (master) $ uv add requests
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of graphrag==1.0.1.dev23 and your project depends on graphrag==1.0.1.dev23, we can conclude that your
      project's requirements are unsatisfiable.
```

With `--extra-index-url` it succeeds:

```
(my-project) vscode ➜ /workspaces/my-project (master) $ uv add requests --extra-index-url 'https://my-artifactory1/simple'
warning: Indexes specified via `--extra-index-url` will not be persisted to the `pyproject.toml` file; use `--index` instead.
Resolved 157 packages in 1.56s
      Built my-project @ file:///workspaces/my-project
```

The contents of my `uv.toml`

```
(my-project) vscode ➜ /workspaces/my-project (master) $ cat ~/.config/uv/uv.toml 
[pip]
index-url = "https://my-artifactory2/simple"
extra-index-url = ["https://my-artifactory1/simple"]
```

~~## Bonus bug~~

~~`uv run` ignores both `uv.toml` and `--extra-index-url` flags:~~

Works with: 

`uv run --extra-index-url 'https://my-artifactory1/simple'  python --version`

### Platform

Linux 6.1.84-99.169.amzn2023.x86_64 x86_64 GNU/Linux

### Version

uv 0.6.12

### Python version

Python 3.12.9

---

_Label `bug` added by @knguyen1 on 2025-04-04 15:48_

---

_Comment by @charliermarsh on 2025-04-04 15:51_

`uv run python --version --extra-index-url 'https://my-artifactory1/simple'` is passing the `--extra-index-url` argument to `python`. You need to put any uv arguments before the command name (in this case, `python`).


---

_Comment by @knguyen1 on 2025-04-04 15:53_

> `uv run python --version --extra-index-url 'https://my-artifactory1/simple'` is passing the `--extra-index-url` argument to `python`. You need to put any uv arguments before the command name (in this case, `python`).

Got it.  Will edit original issue.

---

_Comment by @charliermarsh on 2025-04-04 15:54_

Separately, remove the `[pip]` from your `uv.toml`. Settings under `[pip]` only apply to the `uv pip` commands.

---

_Comment by @knguyen1 on 2025-04-04 15:58_

> Separately, remove the `[pip]` from your `uv.toml`. Settings under `[pip]` only apply to the `uv pip` commands.

That did the trick.  Can close this issue.

---

_Closed by @knguyen1 on 2025-04-04 16:18_

---
