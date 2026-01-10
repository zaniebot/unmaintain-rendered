```yaml
number: 14455
title: "B024: ruff in pre-commit fails to recognise B024 with the presence of a class variable"
type: issue
state: closed
author: cmp0xff
labels:
  - question
assignees: []
created_at: 2024-11-19T13:55:04Z
updated_at: 2024-11-21T15:08:04Z
url: https://github.com/astral-sh/ruff/issues/14455
synced_at: 2026-01-10T11:09:56Z
```

# B024: ruff in pre-commit fails to recognise B024 with the presence of a class variable

---

_Issue opened by @cmp0xff on 2024-11-19 13:55_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

# List of keywords I searched for before creating this issue
- `B024`

# A minimal code snippet that reproduces the bug
```py
from abc import ABC


class Foo(ABC):
    a: int
    def method(self) -> None:
        print("nothing")
```
# The command you invoked
```ps
poetry run pre-commit run -a
```

# The current Ruff settings
## Relevant snippet of `.pre-commit-config.yaml`
```yml
  - repo: https://github.com/astral-sh/ruff/
    rev: 0.7.4
    hooks:
      # Run the linter.
      - id: ruff
        args:
          - --fix
          - --exit-non-zero-on-fix
```
## Relevant snippet of `pyproject.py`
```
[tool.ruff]
line-length = 88
force-exclude = true # Recommended for pre-commit (https://github.com/charliermarsh/ruff#force-exclude)
# Exclude a variety of commonly ignored directories.
exclude = [
    ".bzr",
    ".direnv",
    ".eggs",
    ".git",
    ".git-rewrite",
    ".hg",
    ".mypy_cache",
    ".nox",
    ".pants.d",
    ".pytype",
    ".ruff_cache",
    ".svn",
    ".tox",
    ".venv",
    "__pypackages__",
    "_build",
    "buck-out",
    "build",
    "dist",
    "node_modules",
    "venv",
]
target-version = "py310"
# unsafe-fixes = true

[tool.ruff.lint.per-file-ignores]
"*ipynb*" = [
  "B018",  # https://docs.astral.sh/ruff/rules/useless-expression/
]

[tool.ruff.lint]
# Enable Pyflakes (`F`) and a subset of the pycodestyle (`E`)  codes by default.
select = ["ALL"]
ignore = [
  "ANN001",  # https://docs.astral.sh/ruff/rules/missing-type-function-argument/
  "ANN201",  # https://docs.astral.sh/ruff/rules/missing-return-type-undocumented-public-function/
  "ANN202",  # https://docs.astral.sh/ruff/rules/missing-return-type-private-function/
  "ANN401",  # https://docs.astral.sh/ruff/rules/any-type/
  "ARG",     # https://docs.astral.sh/ruff/rules/#flake8-unused-arguments-arg
  "B007",    # https://docs.astral.sh/ruff/rules/unused-loop-control-variable/
  "B023",    # https://docs.astral.sh/ruff/rules/function-uses-loop-variable/
  "B905",    # https://docs.astral.sh/ruff/rules/zip-without-explicit-strict/
  "C408",    # https://docs.astral.sh/ruff/rules/unnecessary-collection-call/
  "C901",    # https://docs.astral.sh/ruff/rules/complex-structure/
  "D",       # https://docs.astral.sh/ruff/rules/#pydocstyle-d
  "DTZ001",  # https://docs.astral.sh/ruff/rules/call-datetime-without-tzinfo/
  "E501",    # https://docs.astral.sh/ruff/rules/line-too-long/ (handled by black)
  "E722",    # https://docs.astral.sh/ruff/rules/bare-except/
  "E731",    # https://docs.astral.sh/ruff/rules/lambda-assignment/
  "F841",    # https://docs.astral.sh/ruff/rules/unused-variable/
  "G",       # https://docs.astral.sh/ruff/rules/#flake8-logging-format-g
  "S101",    # https://docs.astral.sh/ruff/rules/assert/
  "S301",    # https://docs.astral.sh/ruff/rules/suspicious-pickle-usage/
  "S608",    # https://docs.astral.sh/ruff/rules/hardcoded-sql-expression/
  "SIM114",  # https://docs.astral.sh/ruff/rules/if-with-same-arms/
  "BLE", "COM", "EM", "ERA", "FIX", "FBT", "FLY", "INP",
  "N",
  "PD",
  "PERF", "PGH",
  "PLC", "PLR", "PLW",
  "PT",
  "PTH",
  "PYI", "RET",
  "SLF",
  "T201",
  "TCH", "TD", "TID", "TRY", "UP", "W"
]
```
# The current Ruff version
`0.7.4`


---

_Comment by @dylwil3 on 2024-11-20 05:28_

Thank you for submitting this issue!

This behavior is intentional - it is assumed that an unassigned, annotated class variable is intended to be abstract.

But that has certainly been a point of discussion/refinement. See [this flake8-bugbear issue](https://github.com/PyCQA/flake8-bugbear/issues/293) and the issues linked therein. Let us know if you think it should be further refined!

---

_Label `question` added by @MichaReiser on 2024-11-20 07:27_

---

_Comment by @cmp0xff on 2024-11-20 08:21_

Hi, thank you for the reply.

In such a case, the documentation needs to be updated. For example, in [abstract-base-class-without-abstract-method](https://github.com/astral-sh/docs/blob/main/site/ruff/rules/abstract-base-class-without-abstract-method/index.html), instead of
> Checks for abstract classes without abstract methods.

we can write
> Checks for abstract classes without abstract methods or class variables.

Please let me know if I can continue completing and implementing this idea.

---

_Comment by @dylwil3 on 2024-11-20 16:15_

That'd be great! I think the check only skips if the class variable is annotated and not assigned, though. So you may want to adjust the wording a bit, maybe:

> Checks for abstract classes without abstract methods. Class variables which are annotated but not assigned are regarded as abstract.

---

_Assigned to @cmp0xff by @dylwil3 on 2024-11-20 16:16_

---

_Comment by @cmp0xff on 2024-11-20 19:41_

I am working on the documentation. Meanwhile, upon checking the [Python documentation](https://docs.python.org/3/library/typing.html#typing.ClassVar), it turns out that only when subscribed by `typing.ClassVar` is an annotate variable considered a class variable. Copied from the above-mentioned documentation:

```py
class Starship:
    stats: ClassVar[dict[str, int]] = {} # class variable
    damage: int = 10                     # instance variable
```

What I had as an example in the original post is therefore an instance variable, not a class variable. This contradicts the discussion in https://github.com/PyCQA/flake8-bugbear/issues/293, which only has to do with `typing.ClassVar`.

---

_Comment by @dylwil3 on 2024-11-20 20:21_

Hey @cmp0xff  - you'll want to open a PR for `ruff` not `docs`. The relevant code is here:

https://github.com/astral-sh/ruff/blob/3c52d2d1bd8042d98415f70ecc8d6402795756ef/crates/ruff_linter/src/rules/flake8_bugbear/rules/abstract_base_class.rs#L13-L19


> Meanwhile, upon checking the [Python documentation](https://docs.python.org/3/library/typing.html#typing.ClassVar), it turns out that only when subscribed by typing.ClassVar is an annotate variable considered a class variable.

That's an interesting point... can you open a separate issue for this? For the present issue let's just change the documentation.

Thank you!


---

_Closed by @dylwil3 on 2024-11-21 15:08_

---
