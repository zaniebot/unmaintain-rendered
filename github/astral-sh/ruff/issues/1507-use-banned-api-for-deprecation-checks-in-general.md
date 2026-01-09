---
number: 1507
title: "Use `banned-api` for deprecation checks in general"
type: issue
state: open
author: not-my-profile
labels:
  - rule
assignees: []
created_at: 2022-12-31T09:22:39Z
updated_at: 2023-06-23T10:56:03Z
url: https://github.com/astral-sh/ruff/issues/1507
synced_at: 2026-01-07T13:12:14-06:00
---

# Use `banned-api` for deprecation checks in general

---

_Issue opened by @not-my-profile on 2022-12-31 09:22_

The checks for uses of deprecated standard library features are currently split up across different rules, for example:

* UP019: `typing.Text` is deprecated, use `str`
* PGH002: `warn` is deprecated in favor of `warning`

Since there are many more deprecated things in the standard library (e.g. the [24 superseded modules](https://docs.python.org/3/library/superseded.html)), I think it would make more sense to define these checks via the recently introduced `banned-api` mechanism rather than introducing separate checks for every single thing that is deprecated in the standard library. And I think we could reuse the flexible configuration  mechanism we already have for rules.

In particular I suggest the following to be a default config:

```toml
[tool.ruff.banned-api.select]
"typing.Text" = {msg = "deprecated", autofix-to = "str"}
"logging.warn" = {msg = "deprecated", autofix-to = "logging.warning"}
"optparse" = {msg = "deprecated, use `argparse` instead"}
# etc ... other 23 deprecated stdlib modules
````

And allow users to either:

* disable these checks completely via `tool.ruff.banned-api.select = {}`
* ignore specific errors via e.g. `tool.ruff.banned-api.ignore = ["optparse"]`
* ban additional API via `tool.ruff.banned-api.extend-select`

Since this would make `banned-api` more of a core feature I think it would make sense to move the setting out of the `flake8-tidy-imports` namespace (and change the check code from TID251 to some RUF code).

The steps I have in mind are:

1. convert TID251 to RUF???, moving the config section and enabling `banned-api` to be configured via select, ignore, extend-select & extend-ignore
2. ban the 24 deprecated stdlib modules by default (along with `typing.Text` and `logging.warn` as shown above)
3. make UP019 and PGH002 transform `banned-api`, e.g. `ruff.extend-ignore = [ "UP019" ]` would be internally converted to `ruff.banned-api.extend-ignore = ["typing.Text"]`
4. #835
5. Implement `autofix-to` for `banned-api`.
6. Introduce a `params` setting for `banned-api`, so that e.g. UP021 could be defined as
    `banned-api.select."subprocess.run" = {params = {universal_newlines = {autofix-to="text"}}`

What do you think of this?

---

_Renamed from "Allow `banned-api` to be configured via the same mechanism we use for rules" to "Use `banned-api` for deprecation checks in general" by @not-my-profile on 2022-12-31 09:25_

---

_Label `rule` added by @charliermarsh on 2022-12-31 18:11_

---

_Referenced in [astral-sh/ruff#1996](../../astral-sh/ruff/pulls/1996.md) on 2023-01-19 15:57_

---

_Comment by @flying-sheep on 2023-06-23 10:56_

This also intersects with [UP035 / deprecated-import](https://beta.ruff.rs/docs/rules/deprecated-import/)

---

_Referenced in [astral-sh/ruff#10836](../../astral-sh/ruff/issues/10836.md) on 2024-04-08 13:35_

---
