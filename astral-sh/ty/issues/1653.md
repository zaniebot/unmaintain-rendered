```yaml
number: 1653
title: Walking up the tree
type: issue
state: closed
author: alexei
labels:
  - question
assignees: []
created_at: 2025-11-27T09:42:17Z
updated_at: 2025-12-01T10:19:25Z
url: https://github.com/astral-sh/ty/issues/1653
synced_at: 2026-01-10T01:58:59Z
```

# Walking up the tree

---

_Issue opened by @alexei on 2025-11-27 09:42_

### Question

`ty check` walks up the tree, which is problematic to monorepos with nested packages each with its own dependencies e.g:

```
large_package/
    pyproject.toml
    medium_packages/
        a/
            pyproject.toml
        b/
            pyproject.toml
            small_packages/
                b.foo/
                    pyproject.toml
                b.bar/
                    pyproject.toml
                b.baz/
                    pyproject.toml
        c/
            pyproject.toml
            small_packages/
                c.moo/
                    pyproject.toml
                c.goo/
                    pyproject.toml
```

In fact, it's unclear to me why it does it in the first place. If current and parent directories are Python packages, why would `ty` assume it can tread on them equally? It doesn't look like it cares that current directory is a standalone package with its own virtual env etc.

---

The [`ty check --project` documentation](https://docs.astral.sh/ty/reference/cli/#ty-check--project) says that:

> All `pyproject.toml` files will be discovered by walking up the directory tree from the given project directory, as will the project's virtual environment (`.venv`) unless the `venv-path` option is set.

It's not clear what `venv-path` does, as it's not documented anywhere.

---

**So how do I turn off the walking up the tree?** What I found to work so far is to specify paths explicitly in each config i.e. each `pyproject.toml` with:

```toml
[tool.ty.src]
include = [
    "src/",
    "tests/",
]
```

I don't like it that I have to repeat this same config everywhere, though I admin it's sometimes necessary. However I can get past my being annoyed if that's the standard, recommended approach 😄 Not configuring anything seems to give good results, so changing that makes me feel like I'm missing out on something.

This is slightly related to https://github.com/astral-sh/ty/issues/819 in that it's about monorepos. However the author there appears to take a hybrid approach where, while packages don't share dependencies, some of the dev ones run at the top level. Here I'm talking about fully isolated packages.

### Version

ty 0.0.1-alpha.28 (8c342496a 2025-11-25)

---

_Label `question` added by @alexei on 2025-11-27 09:42_

---

_Comment by @MichaReiser on 2025-11-27 10:38_

ty searches for the closest configuration with a `tool.ty` section starting from the current working directory and uses it as your project root. There are some open discussions about whether we should consider any `pyproject.toml` a potential "root", but this leads to a worse experience when you have a workspace with multiple members who share a single virtual env.

For now, the solution is to add a `tool.ty` section to each member you want treated as a dedicated root. But, if you do that, you'll have to repeat your ty configuration for every member because ty doesn't yet support [hierarchical configurations](https://github.com/astral-sh/ty/issues/175).

We're aware that the experience for this use case isn't great yet and your feedback helps us understand the different project layouts users use.


Related discussion https://github.com/astral-sh/ty/issues/819



---

_Comment by @alexei on 2025-11-27 11:56_

Hey @MichaReiser I appreciate your response! I just checked and can confirm (via `uv run ty check -vv`) that **even an empty** `[tool.ty]` section in `pyproject.toml` prevents `ty` from checking out the parent directory. I guess then this is ideal for packages that have a standard "src" layout and the defaults are fine.

---

_Comment by @MichaReiser on 2025-12-01 10:18_

I'll close this as the main discussion for how to better support mono repositories happens in https://github.com/astral-sh/ty/issues/819

---

_Closed by @MichaReiser on 2025-12-01 10:18_

---
