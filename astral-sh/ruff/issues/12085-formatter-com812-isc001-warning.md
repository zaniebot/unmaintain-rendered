```yaml
number: 12085
title: "formatter: `COM812`, `ISC001` warning"
type: issue
state: closed
author: Mogost
labels:
  - question
assignees: []
created_at: 2024-06-28T05:59:45Z
updated_at: 2024-06-28T07:13:12Z
url: https://github.com/astral-sh/ruff/issues/12085
synced_at: 2026-01-10T11:09:54Z
```

# formatter: `COM812`, `ISC001` warning

---

_Issue opened by @Mogost on 2024-06-28 05:59_

The following configuration triggers a warning message but shouldn't.
All linter rules are enabled but warning contains rules that are in ignore section.

```
[tool.ruff]
line-length = 120
indent-width = 4

[tool.ruff.format]
quote-style = "double"
indent-style = "space"
skip-magic-trailing-comma = false
line-ending = "auto"
preview = true

[tool.ruff.lint]
select = ["ALL"]
ignore = [
...
    "COM812",  # https://docs.astral.sh/ruff/rules/missing-trailing-comma/
    "ISC001",  # https://docs.astral.sh/ruff/rules/single-line-implicit-string-concatenation/
]
```


```
❯ ruff format .
warning: The following rules may cause conflicts when used with the formatter: `COM812`, `ISC001`. To avoid unexpected behavior, we recommend disabling these rules, either by removing them from the `select` or `extend-select` configuration, or adding them to the `ignore` configuration.
2051 files left unchanged
```

---

_Comment by @MichaReiser on 2024-06-28 07:02_

I tried to reproduce the issue with 

```toml
[tool.ruff]
preview=true

[tool.ruff.format]
preview = true
quote-style = "double"
indent-style = "space"
skip-magic-trailing-comma = false
line-ending = "auto"

[tool.ruff.lint]
select = ["ALL"]
ignore = [
    "COM812",
    "ISC001",
]
```

And ran

```
$ uv tool run ruff format
warning: `uv tool run` is experimental and may change without warning.
Resolved 1 package in 3ms
Installed 1 package in 4ms
 + ruff==0.5.0
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
13 files left unchanged
```

I do get warnings about incompatible linter rules but not about `COM812` or `ISC001` being incompatible with the formatter. Do you have an other `pyproject.tom` in a sub folder?

---

_Label `question` added by @MichaReiser on 2024-06-28 07:02_

---

_Comment by @Mogost on 2024-06-28 07:13_

Oops. There is indeed a subdirectory with its own pyproject.toml. As soon as I removed it, the problem went away. Thanks and sorry for the inconvenience.
And thanks for the awesome job!

---

_Closed by @Mogost on 2024-06-28 07:13_

---
