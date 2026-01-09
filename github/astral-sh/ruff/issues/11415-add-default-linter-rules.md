---
number: 11415
title: Add default linter rules
type: issue
state: open
author: AndreuCodina
labels:
  - configuration
  - linter
assignees: []
created_at: 2024-05-13T20:45:14Z
updated_at: 2024-10-11T01:42:48Z
url: https://github.com/astral-sh/ruff/issues/11415
synced_at: 2026-01-07T13:12:15-06:00
---

# Add default linter rules

---

_Issue opened by @AndreuCodina on 2024-05-13 20:45_

In my company we have lots of microservices and nanoservices, and a Python project prototype I maintain to guide how Python projects must be developed and updated.
It's a common complaint not to have default rules in our `pyproject.toml` files, and instead have "magic letters" just because "I chose them".
My job is to ensure data engineers develop software as we do in .NET, i.e. with types.

I want to propose a group of default linter rules, and choose them with groups (letters), instead of choosing a subset.

**pyproject.toml**

```toml
[tool.ruff.lint]
select = ["F", "E", "W", "N", "I", "ANN"]
ignore = ["E501", "ANN101", "ANN102", "ANN401"]
```

Reasoning of `select`:

- F: Unused variables, comparison with `==`, etc.
- E, W, N: PEP 8.
- I: Sort imports.
- ANN: Mandatory return types.

Reasoning of `ignore`:

- E501: Line too long hasn't an automatic fix.
- ANN101, ANN102: Typing `self` and `cls` is strange in Python.
- ANN401: The Python ecosystem is not prepared to not use `Any`.

Besides, I always use this configuration:

```toml
[tool.pyright]
typeCheckingMode = "strict"
```

---

_Comment by @zanieb on 2024-05-13 21:00_

Hi!

We're going to recategorize all of the rules (roughly discussed in https://github.com/astral-sh/ruff/issues/1774) and define new defaults eventually. We're actively working on this, but I'm not sure when we'll be done because it's a big project and we want to get it right.

---

_Comment by @zanieb on 2024-05-13 21:02_

Similarly, this request fits into https://github.com/astral-sh/ruff/issues/1773 which is a part of that effort.

---

_Comment by @dhruvmanila on 2024-05-14 08:00_

@zanieb Do you think this can be merged into either of the linked issue?

---

_Comment by @zanieb on 2024-05-14 14:58_

I'm okay with keeping it open for now unless there's a duplicate issue regarding defaults.

We should add a dedicated tracking issue for the full re-categorization work eventually.

---

_Label `configuration` added by @zanieb on 2024-05-14 14:58_

---

_Label `linter` added by @zanieb on 2024-05-14 14:58_

---

_Comment by @AndreuCodina on 2024-08-18 09:02_

Should we include default rules for some project types? For instance, there're FastAPI rules (https://docs.astral.sh/ruff/rules/#fastapi-fast). It's a library very used, but I don't know if Ruff knows if a package (`fastapi` in this case) is installed and then it doesn't waste resources with those rules.

---

_Referenced in [astral-sh/ruff#13433](../../astral-sh/ruff/issues/13433.md) on 2024-09-23 06:34_

---

_Comment by @AndreuCodina on 2024-10-08 07:00_

Idea: how about declaring the rule "strict" as an alias for the linter rules I posted?

```toml
[tool.ruff.lint]
select = ["strict"]
```

This aligns with Pyright:

```toml
[tool.pyright]
typeCheckingMode = "strict"
```

---

_Comment by @Avasam on 2024-10-11 01:42_

I'll link to https://github.com/astral-sh/ruff/issues/12352 and https://github.com/astral-sh/ruff/discussions/3363#discussioncomment-7266932 as well. With which you could make your own custom defaults for your needs and "levels of strictness"

---

_Referenced in [beeware/briefcase#2292](../../beeware/briefcase/pulls/2292.md) on 2025-05-19 17:08_

---

_Referenced in [astral-sh/ruff#19885](../../astral-sh/ruff/issues/19885.md) on 2025-08-12 21:07_

---
