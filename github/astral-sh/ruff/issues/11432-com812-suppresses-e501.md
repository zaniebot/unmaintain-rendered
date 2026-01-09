---
number: 11432
title: COM812 suppresses E501
type: issue
state: closed
author: savagemindz
labels:
  - question
assignees: []
created_at: 2024-05-15T09:14:55Z
updated_at: 2024-05-15T19:07:23Z
url: https://github.com/astral-sh/ruff/issues/11432
synced_at: 2026-01-07T13:12:15-06:00
---

# COM812 suppresses E501

---

_Issue opened by @savagemindz on 2024-05-15 09:14_

Visual Studio Code
Version: 1.88.1 (Universal)
Commit: e170252f762678dec6ca2cc69aba1570769a5d39
Date: 2024-04-10T17:42:52.765Z (1 mo ago)
Electron: 28.2.8
ElectronBuildId: 27744544
Chromium: 120.0.6099.291
Node.js: 18.18.2
V8: 12.0.267.19-electron.0
OS: Darwin arm64 23.4.0

VsCode Ruff Extension: v2024.20.0
Ruff Version: 0.4.1

Keywords: E501, line-to-long, COM812, "Trailing comma missing"

Start with this (max line length = 100)
![Screenshot 2024-05-15 at 10 07 33](https://github.com/astral-sh/ruff/assets/7400398/bd4930b2-7662-475f-8f8c-008b4f132a3b)

Add a comma to line 3 to solve COM812. There is no warning for E501.
![Screenshot 2024-05-15 at 10 11 58](https://github.com/astral-sh/ruff/assets/7400398/c4cfd4fd-f6e7-4147-8035-b0de0f93f256)

Expected:
I expected E501 to still warn me the line is too long.

My ruff.toml
`# Same as Black.
line-length = 100
indent-width = 4
target-version = "py39"

[lint]
preview = true
select = ["ALL"]
ignore = [
    "CPY001", # Missing copyright notice at top of file
    "D100", # Missing docstring in public module
    "D101", # Missing docstring in public class
    "D102", # Missing docstring in public method
    "D103", # Missing docstring in public function
    "D104", # Missing docstring in public package
    "D105", # Missing docstring in magic method
    "D106", # Missing docstring in public nested class
    "D107", # Missing docstring in __init__
    "D202", # No blank lines allowed after function docstring (found {num_lines})
    "D210", # No whitespaces allowed surrounding docstring text
    "D211", # No blank lines allowed before class docstring
    "D212", # Multi-line docstring summary should start at the first line
    "D407", # Missing dashed underline after section ("{name}")
    "D413", # Missing blank line after last section ("{name}")
    "D401", # First line of docstring should be in imperative mood: "{first_line}"
    "D400", # First line should end with a period
    "D415", # First line should end with a period, question mark, or exclamation point
    "ERA001", # Found commented out code
    "G004", # Logging statement uses f-string
    "PGH003", # Use specific rule codes when ignoring type issues
    "RUF012", # Mutable class attributes should be annotated with typing.ClassVar
    "T201", # print found
    "T203", # pprint found
    "TRY301", # Abstract raise to an inner function
    "TRY400", # Use logging.exception instead of logging.error
    "UP007", # Use X | Y for type annotations
]

# Allow fix for all enabled rules (when `--fix`) is provided.
fixable = ["ALL"]
unfixable = []

# Allow unused variables when underscore-prefixed.
dummy-variable-rgx = "^(_+|(_+[a-zA-Z0-9_]*[a-zA-Z0-9]+?))$"

[format]
# Like Black, use double quotes for strings.
quote-style = "double"

# Like Black, indent with spaces, rather than tabs.
indent-style = "space"

# Like Black, respect magic trailing commas.
skip-magic-trailing-comma = false

# Like Black, automatically detect the appropriate line ending.
line-ending = "auto"

# Enable auto-formatting of code examples in docstrings. Markdown,
# reStructuredText code/literal blocks and doctests are all supported.
#
# This is currently disabled by default, but it is planned for this
# to be opt-out in the future.
docstring-code-format = false

# Set the line length limit used when formatting code snippets in
# docstrings.
#
# This only has an effect when the `docstring-code-format` setting is
# enabled.
docstring-code-line-length = "dynamic"`

Let me know if there is anything else you need.

Thanks


---

_Comment by @charliermarsh on 2024-05-15 14:36_

I think the root cause here is a bit different -- check out the exceptions listed in the [rule documentation](https://docs.astral.sh/ruff/rules/line-too-long/). If a line consists of a single word (i.e., no splittable whitespace), then we don't mark it as overlong.

---

_Label `question` added by @charliermarsh on 2024-05-15 14:36_

---

_Comment by @savagemindz on 2024-05-15 16:40_

Ahh yes, ok, I missed that rule. Is it possible to make that rule configurable? The only reason I ask is this came up when flake8 scanned the line as too long but ruff sees it as ok.

Being forced to add an # noqa: E501 to the line made it work in both linters.

---

_Comment by @charliermarsh on 2024-05-15 19:07_

Unfortunately we don't expose settings for this. There's an issue (#8383) with some discussion around tweaking the logic if you want to chime in there!

---

_Closed by @charliermarsh on 2024-05-15 19:07_

---
