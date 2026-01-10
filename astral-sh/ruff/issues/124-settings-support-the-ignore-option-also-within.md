---
number: 124
title: "Settings: Support the `ignore` option also within `pyproject.toml`"
type: issue
state: closed
author: amotl
labels: []
assignees: []
created_at: 2022-09-07T21:43:38Z
updated_at: 2022-09-08T08:08:09Z
url: https://github.com/astral-sh/ruff/issues/124
synced_at: 2026-01-10T01:22:37Z
---

# Settings: Support the `ignore` option also within `pyproject.toml`

---

_Issue opened by @amotl on 2022-09-07 21:43_

Hi again,

after running into #119 as well, we tried to ignore `F821` by adding it to the `pyproject.toml` file within the `[tool.ruff]` section, like:
```
[tool.ruff]
ignore = ["F821"]
```

However, `ruff` complained with:
```
Failed to load pyproject.toml: unknown field `ignore`, expected one of `line-length`, `exclude`, `select` for key `tool.ruff` at line 199 column 1
```

It looks like the option, although being implemented within `settings.rs`, is currently only available as a command line option, to be used like `ruff --ignore=F821 .`. I think it would be sweet to be en par with `flake8` in this case, because `ignore` will probably be a popular option.

With kind regards,
Andreas.


---

_Renamed from "Support the `ignore` field also within settings" to "Settings: Support the `ignore` field also within `pyproject.toml`" by @amotl on 2022-09-07 21:44_

---

_Renamed from "Settings: Support the `ignore` field also within `pyproject.toml`" to "Settings: Support the `ignore` option also within `pyproject.toml`" by @amotl on 2022-09-07 21:44_

---

_Comment by @amotl on 2022-09-08 00:10_

Other than that, I don't know if the program should fall back to a default configuration without further ado.
```
Falling back to default configuration...
```
At the first sight, the problem from the misconfiguration outlined above was not easy to spot, because the subsequent very large admonition output made the original error message scroll above the fold [^1].

Would making `ruff` fail hard be an option here?

[^1]: We are using `line-length = 120`, so the output was pretty huge.


---

_Comment by @charliermarsh on 2022-09-08 02:12_

Good call, can definitely support this.

---

_Referenced in [astral-sh/ruff#126](../../astral-sh/ruff/pulls/126.md) on 2022-09-08 02:22_

---

_Closed by @charliermarsh on 2022-09-08 02:34_

---

_Comment by @amotl on 2022-09-08 08:08_

Thank you very much!

---

_Referenced in [astral-sh/ruff#262](../../astral-sh/ruff/issues/262.md) on 2022-09-23 21:46_

---
