```yaml
number: 15801
title: pyproject.toml fix-only=True cannot be toggled per directory by using multiple pyproject.toml files and the extend setting.
type: issue
state: open
author: jogo-openai
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2025-01-29T00:55:33Z
updated_at: 2025-02-04T20:57:58Z
url: https://github.com/astral-sh/ruff/issues/15801
synced_at: 2026-01-12T15:54:55Z
```

# pyproject.toml fix-only=True cannot be toggled per directory by using multiple pyproject.toml files and the extend setting.

---

_@jogo-openai_

### Description

I am trying to enable `fix-only` for some directories and not for others, while running `ruff check .` in the repo root directory.

Ruff `fix-only` config option doesn't seem to use the 'closest' config file for each individual file.

https://docs.astral.sh/ruff/configuration/#config-file-discovery

```
$ ruff --version
ruff 0.9.3
```


```.
├── foo
│   ├── bar.py
│   └── pyproject.toml
└── pyproject.toml
```

`pyproject.toml`:
```
[tool.ruff]
lint.select = ["F"]
```

`foo/pyproject.toml`:
```
[tool.ruff]
extend = "../pyproject.toml"
fix-only = true
```

`foo/bar.py`:
```python
import os

from __future__ import annotations
```

When running `ruff check from the root directory here, I expect it to use `fix-only` from `foo/pyproject.toml` since it's the closest config file to `foo/bar.py` but it doesn't.

```
$ ruff check .
foo/bar.py:1:8: F401 [*] `os` imported but unused
  |
1 | import os
  |        ^^ F401
2 |
3 | from __future__ import annotations
  |
  = help: Remove unused import: `os`

foo/bar.py:3:1: F404 `from __future__` imports must occur at the beginning of the file
  |
1 | import os
2 |
3 | from __future__ import annotations
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ F404
  |

Found 2 errors.
[*] 1 fixable with the `--fix` option.
```

When running from the directory foo, the same command respects `fix-only` from `foo/pyproject.toml`.

```
$ ruff check .
Fixed 1 error.
```

---

_Label `bug` added by @dylwil3 on 2025-01-29 18:15_

---

_Label `configuration` added by @dylwil3 on 2025-01-29 18:15_

---

_Label `bug` removed by @dylwil3 on 2025-01-29 18:23_

---

_Comment by @dylwil3 on 2025-01-29 18:29_

Thank you! I'm able to reproduce this behavior. 

Trying to decide if this should be a bug, feature request, or expected behavior that is not documented. For comparison, the same sort of thing happens if you have conflicting settings for `output-format` or `show-fixes`, etc. It feels like it should be expected behavior for "display" settings but a bug/feature request for settings like `fix` and `fix-only`. But this probably needs some input from others.

---

_Label `needs-decision` added by @dylwil3 on 2025-01-29 18:29_

---

_Comment by @MichaReiser on 2025-02-02 16:21_

This is the expected behavior as it is a global setting that affects the entire CLI, similar to other settings like `output-format`, `show-fixes`, etc. 

Can you try using `unsafe-fixes = [ALL]` in the directories where you want to disable fixes? However, that only works as long as you don't want to fix unsafe fixes with `--fix`

---

_Comment by @jogo-openai on 2025-02-03 19:06_

> Can you try using `unsafe-fixes = [ALL]` in the directories where you want to disable fixes? However, that only works as long as you don't want to fix unsafe fixes with `--fix`

We are hoping to do the opposite, have fixes enabled everywhere but only fail some projects on checks without fixes.

We are hoping to do something like this:

Have a single CI job that runs across multiple projects in a single repo that calls `ruff check --fix`, where  all projects have the fix items enabled and a subset of projects can opt into failing on fixes that don't have a fix for them.

---

_Comment by @dylwil3 on 2025-02-04 02:29_

I wonder if a version of https://github.com/astral-sh/ruff/issues/1256 could achieve this- where the warning level was hierarchically configurable?

---

_Comment by @MichaReiser on 2025-02-04 07:48_

> I wonder if a version of [#1256](https://github.com/astral-sh/ruff/issues/1256) could achieve this- where the warning level was hierarchically configurable?

You mean by setting all rules without a fix to severity warning? It sounds somewhat cumbersome or even impossible because it would require setting all rules without a fix to severity error but some rules only sometimes have a fix.

I do think that Red Knot's new rule configuration should help where the configuration allows to override `fixability` for a subset of files. But that still sounds rather annoying. 

I think my recommendation here is to run ruff multiple times. Either per-project other at least twice: once over the projects that want to apply fixes and a second time over projects that don't want to apply fixes. 

It does sound to me that this is a mono repository setup. 

---

_Comment by @jogo-openai on 2025-02-04 16:51_

> I think my recommendation here is to run ruff multiple times. Either per-project other at least twice: once over the projects that want to apply fixes and a second time over projects that don't want to apply fixes.
> 
> It does sound to me that this is a mono repository setup.

This is a monorepo setup, where _all projects apply automatic fixes_ but some want to additionally fail for ruff check rules that cannot be automatically fixed.

Today we already run `ruff check --fix-only --exit-non-zero-on-fix` over everything, but to run `ruff check` on the subset of projects that want to additionally fail on that command, we would either:

* Generate some sort of centralized list of projects that opt-in. Project CI definition for everything else is located per project, so we would need something clunky here
* Run a separate ruff CI step for each project that opts in, in which case the setup overhead for these jobs would be much greater than the time it takes for ruff to run in each case.

We can make this approach work it just seems less efficient and/or more awkward.

---

_Comment by @MichaReiser on 2025-02-04 17:56_

Yeah I understand. Thanks for the extra context. Ruff doesn't have a solution for this today and it's not obvious to me how we'd change Ruff to make this work on a project level. It requires some more design thinking, and it probably is a while before we have time to dig into this more.

---

_Comment by @jogo-openai on 2025-02-04 20:57_

Thanks @MichaReiser we will go with one of the workaround for now, much appreciated.

---
