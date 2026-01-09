---
number: 16376
title: Ruff unexpectedly moving third party packages to first party section
type: issue
state: closed
author: tboddyspargo
labels:
  - question
  - isort
assignees: []
created_at: 2025-02-25T15:26:28Z
updated_at: 2025-02-26T08:59:36Z
url: https://github.com/astral-sh/ruff/issues/16376
synced_at: 2026-01-07T13:12:16-06:00
---

# Ruff unexpectedly moving third party packages to first party section

---

_Issue opened by @tboddyspargo on 2025-02-25 15:26_

# Summary

The presence of a directory (e.g. `snowflake`) at the "config root" of the `ruff` invocation causes any import starting with the same name (e.g. `snowflake.sqlalchemy`) to get categorized as "first party" instead of the expected "third party."


Background: We have a 3rd party dependency in some of our internal packages named `snowflake-sqlalchemy` (import path: `snowflake.sqlalchemy`). We also share a global `ruff.toml` file at the root of our monorepo which we use to achieve consistent formatting and linting behaviors across multiple packages. Finally, there happens to be a folder named `snowflake` at the root of our monorepo (sibling to `ruff.toml`) - which is not a python package and doesn't contain any python files at all. This triggers the behavior I'm seeing.

<details><summary>Minimal Repro</summary>
<p>

```
├── pkg_b
│   ├── __init__.py
│   └── utils.py
├── ruff.toml
└── snowflake
    └── README.md
```

___init__.py_
```
import os

from pkg_b.utils import my_var
from snowflake.sqlalchemy import snowdialect

assert my_var and snowdialect and os
```

_utils.py_
```
my_var = 1
```

_ruff.toml_
```toml
line-length = 120

[lint]
extend-select = ["I"]
```

</p>
</details> 


# Expected behavior

I expected that the categorization for an import path starting with `snowflake` to _not_ be influenced by the presence of directories at the config root sharing that same name - in particular this is unexpected when the directory isn't related to python code in any way.

Is it reasonable to expect that an import can only have a "first party" match if the src path is actually a valid and importable python module?

# Actual behavior

`snowflake.sqlalchemy` is (IMO incorrectly) categorized as a first party import.

<details><summary>Logs</summary>
<p>

```
[2025-02-25][12:58:50][ruff::resolve][DEBUG] Using user-specified configuration file at: /Users/me/dev/ruff-repro/ruff.toml
[2025-02-25][12:58:50][ruff::commands::check][DEBUG] Identified files to lint in: 3.140834ms
[2025-02-25][12:58:50][ruff::diagnostics][DEBUG] Checking: /Users/me/dev/ruff-repro/pkg_b/__init__.py
[2025-02-25][12:58:50][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'os' as Known(StandardLibrary) (KnownStandardLibrary)
[2025-02-25][12:58:50][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'pkg_b.utils' as Known(FirstParty) (SamePackage)
[2025-02-25][12:58:50][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'snowflake.sqlalchemy' as Known(FirstParty) (SourceMatch("/Users/me/dev/ruff-repro"))
[2025-02-25][12:58:50][ruff::commands::check][DEBUG] Checked 1 files in: 614.042µs
```

</p>
</details> 



---

_Label `question` added by @MichaReiser on 2025-02-25 16:51_

---

_Comment by @MichaReiser on 2025-02-25 16:53_

Thanks. Let me have a look at it. It might be difficult for us to help without a reproduction. 

I do suspect that the problem comes from where the `ruff.toml` is located. Can you share the entire project structure including the location of the `ruff.toml` and where the file with the incorrectly sorted imports is?


I moved this issue to the ruff repository because I didn't get the impression that it is specific to the VS code extension

---

_Label `isort` added by @MichaReiser on 2025-02-25 16:53_

---

_Comment by @MichaReiser on 2025-02-25 16:55_

And could you maybe share your `ruff.toml` configuration as well? 

I do suspect that part of the issue is that the `ruff.toml` is shared between different packages unless you manually specified the `src` option?

---

_Comment by @tboddyspargo on 2025-02-25 18:26_

> I moved this issue to the ruff repository because I didn't get the impression that it is specific to the VS code extension

Thank you! Yes - my mistake!


> It might be difficult for us to help without a reproduction....
> Can you share the entire project structure including the location of the ruff.toml and where the file with the incorrectly sorted imports is?

I'll work on providing more detail here. As a start, I added the location of `ruff.toml` to the main issue description. That structure roughly mimics what I have in real life. The submodule with a name that collides with a third-party package exists in `package_a`, while the file getting analyzed as having unsorted imports exists in `package_b`.


> And could you maybe share your `ruff.toml` configuration as well?

<details><summary>ruff.toml</summary>
<p>

```toml
line-length = 120

extend-exclude = [
    "*venv*",
    "app_data",
    "__pycache__",
    ".pytest_cache",
    "build",
    "dist",
    "snap_*.py",     # auto-generated snapshot files from pytest-snapshot.
]

[lint]
extend-select = ["D", "I"] # enable pydocstyle and isort

# - We ignore a default set of rules that Ruff indicates are problematic when used
#   in conjunction with their formatting rules. https://docs.astral.sh/ruff/formatter/#format-suppression
# - We also disable some rules that we feel would unnecessarily annoy or distract developers (e.g. consistent string quoting)
# - See https://docs.astral.sh/ruff/rules/ for reference.
ignore = [
    ##### The following rules are recommended by Ruff because they might conflict with their formatter. #####
    ##### See https://docs.astral.sh/ruff/formatter/#conflicting-lint-rules                             #####
    "W191",   # pycodestyle - tab-indentation
    "E111",   # pycodestyle - indentation-with-invalid-multiple
    "E114",   # pycodestyle - indentation-with-invalid-multiple-comment
    "E117",   # pycodestyle - over-indented
    "D206",   # pydocstyle - indent-with-spaces
    "D300",   # pydocstyle - triple-single-quotes
    "Q000",   # flake8 - bad-quotes-inline-string
    "Q001",   # flake8 - bad-quotes-multiline-string
    "Q002",   # flake8 - bad-quotes-docstring
    "Q003",   # flake8 - avoidable-escaped-quote
    "COM812", # flake8 - missing-trailing-comma
    "COM819", # flake8 - prohibited-trailing-comma
    "ISC001", # flake8 - single-line-implicit-string-concatenation
    "ISC002", # flake8 - multi-line-implicit-string-concatenation

    ##### The following rules are disabled because they are overly distracting at the moment. #####
    "D100", # undocumented-public-module
    "D105", # pydocstyle - undocumented-magic-method (e.g. __str__)
    "D107", # pydocstyle - undocumented-public-init
    "D212", # pydocstyle - multi-line-summary-first-line
]

[lint.pydocstyle]
convention = "google"

[lint.isort]
# https://docs.astral.sh/ruff/faq/#how-does-ruffs-import-sorting-compare-to-isort

```

</p>
</details> 

---

_Comment by @tboddyspargo on 2025-02-25 19:03_

Okay - I found a detail that had escaped me before and also have a minimal repro! **EDIT:** I've updated the main issue description with the minimal repro.

The detail I had missed is the presence of a directory named `snowflake` as a sibling of the `ruff.toml` file.  The repro doesn't depend on any name collision from internal packages or submodules, it's merely the presence of a directory of the same name at the "config root" that causes `ruff` to categorize a third party package import as "first party" - even when the directory doesn't have any python files in it.

```
├── pkg_b
│   ├── __init__.py
│   └── utils.py
├── ruff.toml
└── snowflake
    └── README.md
```

___init__.py_
```
import os

from pkg_b.utils import my_var
from snowflake.sqlalchemy import snowdialect

assert my_var and snowdialect and os
```

_utils.py_
```
my_var = 1
```

_ruff.toml_
```toml
line-length = 120

[lint]
extend-select = ["I"]
```

---

_Comment by @tboddyspargo on 2025-02-25 22:01_

This issue is also reproducible with a structure like this and without using a single `ruff.toml` file across multiple packages.

```
├── __init__.py
├── snowflake
│   └── README.md
└── utils.py
```

This is also a bit contrived, but I think it might be reasonable to support directories that are never "importable" (docs, binaries, data files, etc.) existing at the `ruff` config root.

---

_Comment by @MichaReiser on 2025-02-26 08:59_

Thanks for producing a repro. This now sounds to be the same as https://github.com/astral-sh/ruff/issues/10519

Requiring an `__init__.py` does seem reasonable, but it would be a breaking change

---

_Closed by @MichaReiser on 2025-02-26 08:59_

---
