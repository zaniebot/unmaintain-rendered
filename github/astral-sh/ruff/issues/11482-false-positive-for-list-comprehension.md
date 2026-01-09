---
number: 11482
title: False positive for list comprehension
type: issue
state: closed
author: jontwo
labels:
  - bug
assignees: []
created_at: 2024-05-21T07:56:37Z
updated_at: 2024-12-20T09:05:32Z
url: https://github.com/astral-sh/ruff/issues/11482
synced_at: 2026-01-07T13:12:15-06:00
---

# False positive for list comprehension

---

_Issue opened by @jontwo on 2024-05-21 07:56_

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

Ruff states that a list comprehension should be used, but the (unsafe) fix changes the syntax. 

I have a script to make a POST request against an API and one of the inputs is a list of parameters to append onto the end of the query.

```
    parser.add_argument(
        "--params",
        nargs=2,
        action="append",
        metavar=("KEY", "VALUE"),
        help="Parameters to add to API call.",
    )
```

These are then urlencoded and appended to the query.

```
    if params:
        url = f"{url}?{urllib_parse.urlencode(params)}"
```

If you just pass the params that come out of `argparse`, then `urllib` doesn't like that it is a list of lists and raises `TypeError: not a valid non-string sequence or mapping object`.

So, I convert to a list of tuples before passing the params in.

```
param_tuples = [(k, v) for k, v in args.params] if args.params else None
```

This is the line that Ruff is complaining about, but the suggested fix (`list(args.params)`) doesn't convert to a list of tuples, it leaves it as a list of lists.

It's subtle, but I do need to convert from lists to tuples! I know it is an unsafe fix, but thought this edge case was worth flagging.


Keywords: "list comprehension", tuple, argparse
Ruff version: 0.4.2
Command: ruff check query_api.py --config pyproject.toml --unsafe-fixes --fix

<details>
<summary>pyproject.toml</summary>
[tool.ruff]
line-length = 100

[tool.ruff.lint]
exclude = ["docs/*", "build/*"]
ignore = [
    "B028", # allow quoting values in strings - see https://peps.python.org/pep-0498/#s-r-and-a-are-redundant
    "COM812", # if we add trailing commas everywhere we also need to rerun black which will reformat them again
    "D105", # missing docstring in magic method
    "D107", # missing docstring in __init__
    "D401", # first line of docstring should be imperative
    "E501", # use Bugbear's B950 instead to allow lines to go slightly over the limit
    "E203", # allow whitespace around : until https://github.com/PyCQA/pycodestyle/issues/373 is fixed
    "RET503", # allow missing return None when other return values present
    "RET504", # allow assignment before return
    "TD002", # missing author in TODO comments
    "TD003", # missing issue reference in TODO comments
    "TD004", # missing expiry version in TODO comments
]
select = [
    "A", # flake8-builtins
    "B", # flake8-bugbear
    "C4", # flake8-comprehensions
    "COM", # flake8-commas
    "D", # pydocstyle
    "E", # error
    "ERA", # eradicate
    "F", # pyflakes
    "G", # flake8-format-logging
    "I", # isort
    "N", # pep8-naming
    "RET", # flake8-return
    "TD", # flake8-todos
    "T20", # flake8-print
    "W", # warning,
]

[tool.ruff.lint.flake8-pytest-style]
fixture-parentheses = false

[tool.ruff.lint.pep8-naming]
ignore-names = [
    "assertGeomsAll2D",
    "assertHistoryEqual",
    "assertPolygonsEqual",
    "assertRastersEqual",
    "assertRasterSrs",
    "banded_strings",
    "doRollover",
    "doRollOver",
    "failureException",
    "GetDescription",
    "GetDriver",
    "GetFileList",
    "GetLayer",
    "GetMetaData",
    "GetMetadata",
    "GetSpatialRef",
    "longMessage",
    "maxDiff",
    "ReadAsArray",
    "SetMetadata",
    "SetMetaData",
    "SetMetadataItem",
    "SetMetaDataItem",
    "setUp",
    "setUpClass",
    "setUpTestData",
    "shortDescription",
    "sum_columns",
    "tearDown",
    "tearDownClass",
    "useZ",
]

[tool.ruff.lint.per-file-ignores]
"setup.py" = ["D10"]
"src/__init__.py" = ["A001", "E402", "F401", "F403"]
"src/**/tests/__init__.py" = ["D104"]
"test_*.py" = ["ARG002", "D10", "D20", "D21", "D30", "D40", "D41", "PT009", "S101", "TID252"]

[tool.ruff.lint.pydocstyle]
convention = "google"

</details>

---

_Label `bug` added by @charliermarsh on 2024-05-21 19:09_

---

_Comment by @charliermarsh on 2024-05-21 19:09_

Thanks! That's tricky.

---

_Comment by @tdulcet on 2024-05-21 20:01_

Maybe try: `list(map(tuple, args.params))`

---

_Comment by @harupy on 2024-12-10 11:59_

@charliermarsh Can I work on this? A similar issue was reported in the flake8-comprehensions repo: https://github.com/adamchainz/flake8-comprehensions/issues/204.

---

_Comment by @charliermarsh on 2024-12-10 13:54_

@harupy please feel free!

---

_Assigned to @harupy by @dylwil3 on 2024-12-10 14:57_

---

_Referenced in [astral-sh/ruff#14909](../../astral-sh/ruff/pulls/14909.md) on 2024-12-11 12:33_

---

_Closed by @dhruvmanila on 2024-12-20 09:05_

---

_Closed by @dhruvmanila on 2024-12-20 09:05_

---
