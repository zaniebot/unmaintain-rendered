```yaml
number: 8607
title: "Support `all-extras` configuration for `uv sync` command"
type: issue
state: open
author: zmeir
labels:
  - help wanted
  - configuration
assignees: []
created_at: 2024-10-27T11:39:46Z
updated_at: 2025-09-02T17:37:01Z
url: https://github.com/astral-sh/uv/issues/8607
synced_at: 2026-01-12T15:59:30Z
```

# Support `all-extras` configuration for `uv sync` command

---

_@zmeir_

The `tool.uv.pip` configuration section allows setting `all-extras = true` which affects all subcommands in the `uv pip` namespace. Is it possible to add the same configuration for the `[tool.uv]` table? This will allow, for example, to use `uv sync` to sync all extras automatically (instead of having to type `uv sync --all-extras`).

The current section in the documentation, for reference:  
![image](https://github.com/user-attachments/assets/41975282-49b9-447d-9791-f08ad52e3e98)

```
$ uv --version
uv 0.4.27 (36b729e92 2024-10-25)
```

---

_Label `configuration` added by @zanieb on 2024-10-27 13:44_

---

_Comment by @zanieb on 2024-10-27 13:45_

Seems fine to me, though it might be kind of hard to get the interaction with `--extra` right. We might want to add a `default-extras` option like `default-groups` instead? #8471 

---

_Comment by @zmeir on 2024-10-27 13:48_

> We might want to add a `default-extras` option like `default-groups` instead? #8471

That sounds great!

---

_Comment by @superlopuh on 2024-12-12 08:35_

I would love this also, we have a setup with multiple optional dependency groups, all of which make sense to install in a developer environment to make sure that the tests still pass.

---

_Label `help wanted` added by @zanieb on 2024-12-12 14:05_

---

_Comment by @vivienm on 2024-12-12 22:42_

There is a neat workaround used in Litestar:

* A `full` extra includes all other extras (https://github.com/litestar-org/litestar/blob/f31ef97d6cb725bf9898f55abbb5150b36823f27/pyproject.toml#L85-L87)
* `litestar[full]` is part of the `dev` dependency group (https://github.com/litestar-org/litestar/blob/f31ef97d6cb725bf9898f55abbb5150b36823f27/pyproject.toml#L119-L121)

This way, the default developer environment comes with all dependencies.

---

_Comment by @pygarap on 2025-09-02 14:53_

Adding `include-extra` or `include-optional` to the dependency-groups, would be perfect:
```
[project.optional-dependencies]
plot = [
    "matplotlib>=3.6.3"
]

[dependency-groups]
dev = [
    { include-group = "lint" },
    { include-extra = "plot" }
]
lint = [
    "ruff>=0.11.13",
    "ty>=0.0.1a16",
]
```

---

_Comment by @zmeir on 2025-09-02 16:46_

@tallerasaf keep in mind that dependency-groups aren’t just a uv thing - they are a part of the Python packaging standards as described in [PEP 735](https://peps.python.org/pep-0735/). So implementing what you ask means either breaking the standard or submitting (and accepting) a new PEP.

I don’t suppose breaking the standard is desirable, and adding this capability to the standard seems to contradict the PEP, so I doubt it will be possible.

But don’t get me wrong, I was also hoping for something like that :)

---

_Comment by @pygarap on 2025-09-02 17:37_

@zmeir We can do it in `[tool.uv.dependency-groups]` instead of `[dependency-groups]`:
```
[project.optional-dependencies]
plot = [
    "matplotlib>=3.6.3"
]
[dependency-groups]
dev = [
    { include-group = "lint" }
]
lint = [
    "ruff>=0.11.13",
    "ty>=0.0.1a16",
]
[tool.uv.dependency-groups]
dev = [
    { include-extra = "plot" }
]
```
<img width="964" height="650" alt="Image" src="https://github.com/user-attachments/assets/5b4f4818-33be-4724-a0df-cb1d30d3e8bb" />

---
