---
number: 2140
title: PTH123 false positive
type: issue
state: closed
author: JonathanPlasse
labels:
  - bug
assignees: []
created_at: 2023-01-24T19:20:30Z
updated_at: 2023-01-25T01:40:39Z
url: https://github.com/astral-sh/ruff/issues/2140
synced_at: 2026-01-07T13:12:14-06:00
---

# PTH123 false positive

---

_Issue opened by @JonathanPlasse on 2023-01-24 19:20_

Taken directly from this repository in the `script` folder.

```python
def main(*, name: str, code: str, linter: str) -> None:
    """Generate boilerplate for a new rule."""
    # Create a test fixture.
    with (ROOT_DIR / "resources/test/fixtures" / dir_name(linter) / f"{code}.py").open(
        "a",
    ):
        pass
```

Here is the minimal code

```python
from pathlib import Path

(Path("") / "").open()
```

Generate the error `PTH123` `open("foo")` should be replaced by `Path("foo").open()`

Ruff version: `0.0.231`

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->


---

_Comment by @charliermarsh on 2023-01-24 19:33_

Definitely an error. Does the originating plugin raise this? (Regardless we should fix.)

---

_Label `bug` added by @charliermarsh on 2023-01-24 19:33_

---

_Comment by @JonathanPlasse on 2023-01-24 19:38_

No, it does not.

---

_Comment by @JonathanPlasse on 2023-01-24 20:26_

The problem comes from `collect_call_path` that returns `["open"]` which is confused for the builtin `open`.
Maybe `collect_call_path` should return `["", "open"]` to separate the two.

---

_Comment by @charliermarsh on 2023-01-24 20:31_

`collect_call_path` returns `["", "open"]` when `open` is bound to the builtin.

---

_Comment by @charliermarsh on 2023-01-24 20:32_

I'm guessing the problem here is that we're treating `x.open()` as if it's equivalent to `open` within the rule.

---

_Comment by @charliermarsh on 2023-01-25 00:45_

I can fix tonight.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-25 01:30_

---

_Referenced in [astral-sh/ruff#2144](../../astral-sh/ruff/pulls/2144.md) on 2023-01-25 01:40_

---

_Closed by @charliermarsh on 2023-01-25 01:40_

---
