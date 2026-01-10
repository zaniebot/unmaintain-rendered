```yaml
number: 12359
title: Misleading error hint about unknown configuration file keys
type: issue
state: open
author: pradyunsg
labels:
  - bug
  - configuration
assignees: []
created_at: 2024-07-17T10:55:57Z
updated_at: 2025-10-03T11:58:38Z
url: https://github.com/astral-sh/ruff/issues/12359
synced_at: 2026-01-10T11:09:54Z
```

# Misleading error hint about unknown configuration file keys

---

_Issue opened by @pradyunsg on 2024-07-17 10:55_

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

Version: 0.5.2

#### Reproduction instructions

1. Create a `ruff.toml` file with the contents:

    ```toml
    extend-select = [
        "W", # pycodestyle warnings
        "I", # isort
        "N", # pep8-naming
        "D", # pydocstyle
        "B", # flake8-bugbear
        "PYI" # flake8-pyi
    ]

    # Group errors by file.
    output-format = "grouped"

    # Show source context of error.
    show-source = true

    # Show summary of fixes.
    show-fixes = true
    ```

2. Create a `src/foo.py` file with no contents.
3. Run `ruff check` and see the problematic error message.

#### Output

```sh-session
❯ ruff check
ruff failed
  Cause: Failed to parse /Users/[...]/ruff.toml
  Cause: TOML parse error at line 1, column 1
  |
1 | extend-select = [
  | ^^^^^^^^^^^^^^^^^
unknown field `show-source`
```

The error message points to the line about `extend-select` when complaining about `show-source` as the key that is unknown. It would be better for the error to point at line 14 entirely OR, ideally, point only at `show-source` part of that line since that's the key in the key-value pair in the configuration file that is unknown.


---

_Renamed from "Misleading error hint about invalid configuration file keys" to "Misleading error hint about unknown configuration file keys" by @pradyunsg on 2024-07-17 10:56_

---

_Comment by @MichaReiser on 2024-07-17 11:24_

That makes sense. Although I don't think there's much that we can do about it other than writing our own toml parser. The error message comes directly from the `toml` crate 

https://github.com/astral-sh/ruff/blob/117203f71356718b35579ebc6e9805c291dc50cf/crates/ruff_workspace/src/pyproject.rs#L52-L55

---

_Comment by @MichaReiser on 2024-07-17 11:26_

Could be related to https://github.com/toml-rs/toml/issues/589

---

_Label `bug` added by @charliermarsh on 2024-07-17 13:56_

---

_Label `configuration` added by @charliermarsh on 2024-07-17 13:56_

---

_Comment by @charliermarsh on 2024-07-17 13:56_

Yeah that's a shame. Thanks @pradyunsg.

---

_Comment by @chirizxc on 2025-10-03 11:58_

Here's the original problem https://github.com/serde-rs/serde/issues/1183

---
