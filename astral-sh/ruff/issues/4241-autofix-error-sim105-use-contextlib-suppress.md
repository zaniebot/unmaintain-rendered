```yaml
number: 4241
title: "[Autofix error] SIM105 Use `contextlib.suppress(KeyError)` instead of `try`-`except`-`pass`"
type: issue
state: closed
author: bittner
labels: []
assignees: []
created_at: 2023-05-05T10:19:55Z
updated_at: 2023-05-05T13:02:48Z
url: https://github.com/astral-sh/ruff/issues/4241
synced_at: 2026-01-12T15:54:44Z
```

# [Autofix error] SIM105 Use `contextlib.suppress(KeyError)` instead of `try`-`except`-`pass`

---

_@bittner_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
**Code Snippet:**

```python
class EnvironContext(patch.dict):
    ...

    def __enter__(self):

        super(EnvironContext, self).__enter__()

        for key in self.clear_variables:
            try:
                self.in_dict.pop(key)
            except KeyError:
                pass
```

**Executed command:**

```console
ruff . --fix
```

**Error tried to fix:**

```python
cli_test_helpers/decorators.py:58:13: SIM105 Use `contextlib.suppress(KeyError)` instead of `try`-`except`-`pass`
```

**Relevant settings:**

```toml
[tool.ruff]
extend-exclude = ["examples"]
extend-select = ["B", "BLE", "C4", "C90", "COM", "DJ", "DTZ", "EM", "G", "I", "N", "PIE", "PL", "PT", "PTH", "R", "S", "SIM", "T10", "TID", "W", "YTT"]

[tool.ruff.per-file-ignores]
"cli_test_helpers/commands.py" = ["S602"]
"setup.py" = ["PTH"]
"tests/*.py" = ["S101"]
```

**Ruff version:** 0.0.264

## Full Source Code and Additional Details

- See https://github.com/painless-software/python-cli-test-helpers/pull/39

---

_Comment by @dhruvmanila on 2023-05-05 10:49_

This is fixed on `main` branch: https://github.com/charliermarsh/ruff/pull/4215

\cc @charliermarsh @MichaReiser A release should be made to for this regression.

---

_Comment by @charliermarsh on 2023-05-05 13:02_

@dhruvmanila 👍, will cut a release later today.

---

_Closed by @charliermarsh on 2023-05-05 13:02_

---
