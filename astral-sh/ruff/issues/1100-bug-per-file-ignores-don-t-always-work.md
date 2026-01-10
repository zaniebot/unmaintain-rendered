```yaml
number: 1100
title: "[bug] per-file-ignores don't always work"
type: issue
state: closed
author: smackesey
labels:
  - bug
assignees: []
created_at: 2022-12-06T03:11:51Z
updated_at: 2022-12-06T04:42:27Z
url: https://github.com/astral-sh/ruff/issues/1100
synced_at: 2026-01-10T12:06:16Z
```

# [bug] per-file-ignores don't always work

---

_Issue opened by @smackesey on 2022-12-06 03:11_

We have this in our config:

```
### pyproject.toml

# ...

[tool.ruff.per-file-ignores]

"examples/docs_snippets/*" = [

  # (local variable assigned but never used): This happens a lot in the docs snippets for didactic
  # purposes.
  "F841",
]
```

If I run from the root:

```
$ ruff --config=pyproject.toml `git ls-files 'examples/docs_snippets/*.py'`
```

ruff does not respect the `F841` ignore in `docs_snippets`.

But if I change it to:

```
$ ruff --config=pyproject.toml `git ls-files 'examples/*.py'`
```

Then ruff _does_ respect the `F841` ignore for `docs_snippets`.

This may have something to do with the fact that there is another `pyproject.toml` at `examples/docs_snippets/pyproject.toml`, but I don't think that should matter given that I explicitly set the config in the CLI.

---

_Comment by @charliermarsh on 2022-12-06 03:26_

Taking a look.

---

_Comment by @charliermarsh on 2022-12-06 03:43_

The problem here is that we have two concepts: the `pyproject.toml`, and the project root. We use the presence of `pyproject.toml` files to infer the project root (along with `.git` and `.hg` directories -- I believe this logic came from Black).

So in the above case, we first find the project root at `examples/doc_snippets` (due to the presence of a `pyproject.toml`). Then we load the `--config=pyproject.toml`, and use _that_ for configuration. But the paths (like `"examples/docs_snippets/*"`) are relative to the project root, so it's really ignoring `"examples/docs_snippets/examples/docs_snippets/*"`.

We should probably set the project root to the directory containing the `pyproject.toml`, if a file is explicitly provided. This might lead to some confusing cases, it's honestly hard to think through which is a testament to the complexity of the current model (which I'm hoping to repo based on some of the other discussions we've had).

Another option is to provide a `--root` command-line argument, similar to `--config`.


---

_Label `bug` added by @charliermarsh on 2022-12-06 03:43_

---

_Comment by @charliermarsh on 2022-12-06 04:00_

https://github.com/charliermarsh/ruff/pull/1101 should fix this for you -- I just tested with the same setup.

---

_Closed by @charliermarsh on 2022-12-06 04:01_

---

_Comment by @charliermarsh on 2022-12-06 04:42_

This is going out in [v0.0.164](https://github.com/charliermarsh/ruff/releases/tag/v0.0.164) (building now).

---
