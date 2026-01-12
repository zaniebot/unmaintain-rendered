```yaml
number: 21658
title: "Ruff format ignores quote-style = \"single\" configuration"
type: issue
state: closed
author: w0r1dhe110
labels:
  - question
assignees: []
created_at: 2025-11-27T10:10:06Z
updated_at: 2025-11-28T03:41:55Z
url: https://github.com/astral-sh/ruff/issues/21658
synced_at: 2026-01-12T15:54:57Z
```

# Ruff format ignores quote-style = "single" configuration

---

_@w0r1dhe110_

### Question


I've configured Ruff to use single quotes by setting:
```pyproject.toml
[tool.ruff.format]
quote-style = "single"
```
```.pre-commit-config.yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v6.0.0
    hooks:
      - id: end-of-file-fixer
      - id: trailing-whitespace
      - id: check-merge-conflict
      - id: check-yaml
      - id: check-json

  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.14.6
    hooks:
      - id: ruff
        args: [--fix]
      - id: ruff-format
```

However, when running `uv run pre-commit run -a`, it continues to convert single quotes to double quotes instead of preserving single quotes as specified.

**Expected behavior:**
- Single quotes should be maintained where used
- No conversion to double quotes should occur

**Actual behavior:**
- Single quotes are consistently being changed to double quotes
- The `quote-style = "single"` setting appears to be ignored

**Environment:**
- ruff version:  v0.14.6
- ruff-pre-commit version:  v0.14.6
- OS: Windows 10


### Version

_No response_

---

_Label `question` added by @w0r1dhe110 on 2025-11-27 10:10_

---

_Comment by @MichaReiser on 2025-11-27 10:32_

Can you try running `ruff check --show-settings <problematic_file> -v`. The `-v` enables verbose mode, logging more information about the configuration discovery `--show-settings` logs the resolved settings for that file. For example, in my case, this is

```toml
# Formatter Settings
formatter.exclude = []
formatter.unresolved_target_version = 3.10
formatter.per_file_target_version = {}
formatter.preview = disabled
formatter.line_width = 100
formatter.line_ending = auto
formatter.indent_style = space
formatter.indent_width = 4
formatter.quote_style = double
formatter.magic_trailing_comma = respect
formatter.docstring_code_format = disabled
formatter.docstring_code_line_width = dynamic
```



---

_Comment by @w0r1dhe110 on 2025-11-28 02:35_


When running `uv run ruff check --show-settings pyproject.toml -v`, the output configuration is as follows:

```
# Formatter Settings
formatter.exclude = [
        "*.pyi",
]
formatter.unresolved_target_version = 3.14
formatter.per_file_target_version = {}
formatter.preview = disabled
formatter.line_width = 88
formatter.line_ending = auto
formatter.indent_style = space
formatter.indent_width = 4
formatter.quote_style = single
formatter.magic_trailing_comma = respect
formatter.docstring_code_format = disabled
formatter.docstring_code_line_width = dynamic
```

When executing `uv run ruff format`, the configuration takes effect. However, when running `uv run pre-commit run -a`, the configuration does not take effect—single quotes are still being replaced with double quotes.


---

_Comment by @w0r1dhe110 on 2025-11-28 03:41_

After upgrading pre-commit, the issue was resolved.

---

_Closed by @w0r1dhe110 on 2025-11-28 03:41_

---
