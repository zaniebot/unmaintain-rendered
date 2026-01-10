```yaml
number: 3134
title: "`suppress-none-returning = true` won't work"
type: issue
state: closed
author: nrbnlulu
labels:
  - question
assignees: []
created_at: 2023-02-22T16:47:07Z
updated_at: 2023-02-22T20:10:27Z
url: https://github.com/astral-sh/ruff/issues/3134
synced_at: 2026-01-10T11:09:46Z
```

# `suppress-none-returning = true` won't work

---

_Issue opened by @nrbnlulu on 2023-02-22 16:47_

For these configurations `suppress-none-returning = true` would be ignored.

<details>
    <summary>pyproject.toml</summary>  

```toml
[tool.ruff]
line-length = 100
select = ["ALL"]
target-version = "py38"
ignore = [
    "TID252",
    # we use asserts in tests and to hint mypy
    "E501", # line too long, handled by black.
    "S101",
    "S102",
    "S104",
    "S324",
    "EXE002",
    # maybe we can enable this in future
    # we'd want to have consistent docstrings in future
    "D",
    "ANN101", # missing annotation for self?
    # definitely enable these, maybe not in tests
    "ANN001",
    "ANN002",
    "ANN003",
    "ANN102",
    "ANN201",
    "ANN202",
    "ANN204",
    "ANN205",
    "ANN206",
    "ANN401",
    "PGH003",
    "PGH004",
    "RET504",
    "RET505",
    "RET506",
    "RET507",
    "BLE001",
    "B008",
    "N811",
    "N804",
    "N818",
    # Variable `T` in function should be lowercase
    # this seems a potential bug or opportunity for improvement in ruff
    "N806",

    # first argument should named self (found in tests)
    "N805",

    "N815",

    # shadowing builtins
    "A001",
    "A002",
    "A003",

    "ARG001",
    "ARG002",
    "ARG003",
    "ARG004",
    "ARG005",
    "FBT001",
    "FBT002",
    "FBT003",

    "PT001",
    "PT023",

    # enable these, we have some in tests
    "B006",
    "PT004",
    "PT007",
    "PT011",
    "PT012",
    "PT015",
    "PT017",
    "C414",
    "N802",

    "SIM117",
    "SIM102",

    "F841",
    "B027",
    "B905",
    "ISC001",

    # same?
    "S105",
    "S106",

    "DTZ003",
    "DTZ005",

    "RSE102",
    "SLF001",

    # in tests
    "DTZ001",

    "EM101",
    "EM102",
    "EM103",

    "B904",
    "B019",

    "N801",
    "N807",

    # pandas
    "PD",

    # code complexity
    "C",
    "C901",

    # trailing commas
    "COM812",

    "PLR",
    "INP",
    "TRY",
    "SIM300",
    "SIM114",
    "DJ008",
]
fix = true
src = ["<source>", "tests"]

[tool.ruff.flake8-annotations]
suppress-none-returning = true
```
</details>

---

_Label `question` added by @charliermarsh on 2023-02-22 18:16_

---

_Comment by @charliermarsh on 2023-02-22 18:17_

Do you mind including a code snippet + command on which this reproduces? Thanks!

---

_Comment by @nrbnlulu on 2023-02-22 18:53_

Hey @charliermarsh Thanks for you great work here. sorry for not including enough information.

run `pre-commit run -a`
and 
```python
def return_implicit_none() -> Optional[int]:
    if random.randint(1, 2) == 2:
        return 2
```
would become
```python
def return_implicit_none() -> Optional[int]:
    if random.randint(1, 2) == 2:
        return 2
    return None
```
repo: https://github.com/nrbnlulu/reproduce-return-none-ruff/tree/main

---

_Comment by @charliermarsh on 2023-02-22 19:57_

No worries! Thanks for the added info. I think you _may_ be mixing up responsibilities. `suppress-none-returning` only applies to the flake8-annotation rules (those that stat with `ANN`), and is used to suppress annotation requirements for functions that don't return anything.

What you're seeing is the expected behavior for that rule (`RET503`). I'd suggest adding `RET503` to your ignore list if it's not something you want to enforce!

---

_Comment by @nrbnlulu on 2023-02-22 20:10_

Thanks!

---

_Closed by @nrbnlulu on 2023-02-22 20:10_

---
