---
number: 14609
title: "[Bug] [DOC501 & DOC502] Numpy still docstrings with `:exc:` directive for exceptions are falsely flagged"
type: issue
state: closed
author: AbstractUmbra
labels: []
assignees: []
created_at: 2024-11-26T13:26:01Z
updated_at: 2024-11-26T14:30:03Z
url: https://github.com/astral-sh/ruff/issues/14609
synced_at: 2026-01-10T01:22:55Z
---

# [Bug] [DOC501 & DOC502] Numpy still docstrings with `:exc:` directive for exceptions are falsely flagged

---

_Issue opened by @AbstractUmbra on 2024-11-26 13:26_

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
### Intro

Searched keywords: DOC501, DOC502, numpy exception
Command invoked: `ruff check project/utils.py`
Ruff config:-
<details>
<summary>pyproject.toml excerpt</summary>

```toml
[tool.ruff]
line-length = 125
target-version = "py311"

[tool.ruff.lint]
select = [
    "A",
    "ANN",
    "ASYNC",
    "B",
    "BLE",
    "C4",
    "COM",
    "DOC",
    "DTZ",
    "E",
    "EM",
    "ERA",
    "F",
    "FA",
    "FBT",
    "FURB",
    "G",
    "I",
    "INP",
    "ISC",
    "NPY",
    "PD",
    "PERF",
    "PGH",
    "PIE",
    "PLC",
    "PLE",
    "PLW",
    "PTH",
    "PYI",
    "Q",
    "Q003",
    "RET",
    "RSE",
    "RUF",
    "S",
    "SIM",
    "SLOT",
    "T20",
    "TC",
    "TID",
    "TRY",
    "UP",
    "YTT",
]
ignore = [
    "ANN401",
    "F401",
    "F402",
    "F403",
    "F405",
    "PD011",   # this is not a numpy codebase
    "PERF203",
    "PLC0414", # pyright ruling for `as` imports needed
    "Q000",
    "RUF001",
    "RUF009",
    "SIM105",
    "TRY003",  # over-eager rule
    "TRY301",  # unrealistic rule
    "UP034",
    "UP038",
]
unfixable = [
    "E501", # line length handled in other ways by ruff format
    "ERA",  # Don't delete commented out code
]
[tool.ruff.lint.per-file-ignores]
"tests/*" = [
    "S101",   # we use `assert` in tests
    "EM101",  # testing exceptions are okay I feel
    "INP001", # we don't import tests generally
]

[tool.ruff.format]
# Like Black, use double quotes for strings.
quote-style = "double"
# Like Black, indent with spaces, rather than tabs.
indent-style = "space"
# Like Black, respect magic trailing commas.
skip-magic-trailing-comma = false
# Like Black, automatically detect the appropriate line ending.
line-ending = "auto"

[tool.ruff.lint.isort]
split-on-trailing-comma = true
combine-as-imports = true

[tool.ruff.lint.pydocstyle]
convention = "numpy"

[tool.ruff.lint.flake8-annotations]
allow-star-arg-any = true

[tool.ruff.lint.flake8-pytest-style]
fixture-parentheses = false
mark-parentheses = false
parametrize-names-type = "csv"

[tool.ruff.lint.flake8-quotes]
inline-quotes = "double"

[tool.ruff.lint.flake8-tidy-imports.banned-api]
# https://discuss.python.org/t/problems-with-typeis/55410/6
# https://discuss.python.org/t/problems-with-typeis/55410/46
# Until what can go into a TypeIs/TypeGuard changes, these are just dangerous.
"typing.TypeIs".msg = "TypeIs is fundamentally unsafe, even when using it as described to be safe"
"typing.TypeGuard".msg = "TypeGuard is fundamentally unsafe"
"typing_extensions.TypeIs".msg = "TypeIs is fundamentally unsafe, even when using it as described to be safe"
"typing_extensions.TypeGuard".msg = "TypeGuard is fundamentally unsafe"
```

</details>

Ruff version:-
```sh
❯ ruff --version             
ruff 0.8.0
```

### Problem

When using the `numpy` style of docstrings, I tend to use the `:exc:` role for my documentation generation using Sphinx, this however is falsely flagged under the above two rules. I was wondering if this could be accounted for in the rule checking?

### Reproducible code

<details>
<summary>Code</summary>

```py
def foo() -> None:
    """Testing method.

    Raises
    -------
    :exc:`RuntimeError`
    """
    raise RuntimeError
```

</details>

<details>
<summary>Output</summary>

```sh
❯ ruff check project/test.py --preview
project/test.py:2:5: DOC501 Raised exception `RuntimeError` missing from docstring
  |
1 |   def foo() -> None:
2 |       """Testing method.
  |  _____^
3 | | 
4 | |     Raises
5 | |     -------
6 | |     :exc:`RuntimeError`
7 | |     """
  | |_______^ DOC501
8 |       raise RuntimeError
  |
  = help: Add `RuntimeError` to the docstring

project/test.py:2:5: DOC502 Raised exception is not explicitly raised: `:exc:`RuntimeError``
  |
1 |   def foo() -> None:
2 |       """Testing method.
  |  _____^
3 | | 
4 | |     Raises
5 | |     -------
6 | |     :exc:`RuntimeError`
7 | |     """
  | |_______^ DOC502
8 |       raise RuntimeError
  |
  = help: Remove `:exc:`RuntimeError`` from the docstring

Found 2 errors.
```

---

_Comment by @MichaReiser on 2024-11-26 13:34_

I believe this is the same as https://github.com/astral-sh/ruff/issues/12520

---

_Comment by @AbstractUmbra on 2024-11-26 13:53_

> I believe this is the same as #12520

I did see this issue and I believe since this relates to *Sphinx* style docstrings, not *Numpy* still that they are different.
As this issue also links to the larger issue to track the implementation of `pydoclint`, in which it has a checkbox next to the `numpy` implementation.

---

_Comment by @MichaReiser on 2024-11-26 13:56_

Ah sorry, I misread your description. I assumed you use the sphinx style instead of you using sphinx to generate the documentation

Do you have a link documenting that `:exc:` is a valid role? I couldn't find it in the numpy docstring documentation 

---

_Comment by @AbstractUmbra on 2024-11-26 14:05_

It is not very well published, but it is found [here](https://www.sphinx-doc.org/en/master/usage/domains/python.html#role-py-exc).

---

_Comment by @MichaReiser on 2024-11-26 14:08_

Hmm okay, but that's sphinx after all? It doesn't seem to be part of the official numpy guide?

---

_Comment by @AbstractUmbra on 2024-11-26 14:18_

Oh I guess that would be correct then.
It's numpy *style* strings but using Sphinx directives and roles, my mistake. It is a duplicate of the earlier issue!

---

_Comment by @MichaReiser on 2024-11-26 14:30_

I'll merge this issue into the existing issue. 

---

_Closed by @MichaReiser on 2024-11-26 14:30_

---
