```yaml
number: 6092
title: Traversing wildcard files is sometimes slow
type: issue
state: closed
author: aqeelat
labels:
  - needs-info
assignees: []
created_at: 2023-07-26T11:44:54Z
updated_at: 2023-07-28T04:28:30Z
url: https://github.com/astral-sh/ruff/issues/6092
synced_at: 2026-01-12T15:54:45Z
```

# Traversing wildcard files is sometimes slow

---

_@aqeelat_

I don't know how to explain it, but I noticed that `ruff check */tests/* --select="ARG002"` is very fast but `ruff check */test*/* --select="ARG002"` is very slow.

```bash
start=`date +%s`;ruff check */tests/* --select="ARG002";end=`date +%s`; echo `expr $end - $start`
>>> 0
start=`date +%s`;ruff check */test*/* --select="ARG002";end=`date +%s`; echo `expr $end - $start`
>>> 25
```

I'm using version 280

<details><summary>Details</summary>
<p>

```toml
[tool.ruff]
extend-select = ["B", "Q", "N", "W", "DJ", "PL", "PIE", "ARG", "INT", "TCH", "SIM", "PERF", "I"]
line-length = 120
target-version = "py310"
extend-exclude = ["./scripts_old/"]
fixable = ["I001"]
ignore = [
    # This list includes all of the errors we had at the time of running `ruff check . --statistics`
    # We should slowly remove them 1 by 1 until the ignore list is empty.
#    "E501",
#    "ARG002",
#    "DJ001",
#    "PLR2004",
#    "ARG001",
#    "PLC1901",
#    "PLR0913",
#    "PLR0912",
#    "SIM108",
#    "B904",
#    "PLR0915",
#    "N806",
#    "TCH001",
#    "PLR5501",
#    "E722",
#    "N815",
#    "N818",
#    "SIM102",
#    "B007",
#    "DJ007",
#    "TCH003",
#    "SIM105",
#    "PLR0911",
#    "TCH002",
#    "DJ008",
#    "ARG004",
#    "DJ006",
#    "PERF401",
#    "PERF203",
#    "B905",
]

[tool.ruff.per-file-ignores]
"*/__init__.py" = ["F401"]
"*/test*/*" = ["N802"]

[tool.ruff.flake8-unused-arguments]
ignore-variadic-names = true

[tool.ruff.isort]
combine-as-imports = true
lines-after-imports = 2
force-single-line = true
split-on-trailing-comma = true
```

</p>
</details> 


---

_Comment by @charliermarsh on 2023-07-26 13:58_

I believe that pattern is expanded by your shell, not by Ruff, so two things could be happening: first, that could be time spent by your shell doing the expansion; second, it could be that you're including far more files when using that pattern.

Can check the latter with something like:

```shell
❯ echo */tests*
example/tests example/tests-foo
❯ echo */tests*/*
example/tests-foo/bar example/tests/foo
```

---

_Label `waiting-on-author` added by @charliermarsh on 2023-07-26 13:58_

---

_Comment by @charliermarsh on 2023-07-28 04:28_

Closing for now, can always re-open if new information comes in.

---

_Closed by @charliermarsh on 2023-07-28 04:28_

---
