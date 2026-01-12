```yaml
number: 3091
title: uv pip compile --include-self
type: issue
state: closed
author: elbaro
labels: []
assignees: []
created_at: 2024-04-17T12:00:31Z
updated_at: 2024-04-20T01:30:51Z
url: https://github.com/astral-sh/uv/issues/3091
synced_at: 2026-01-12T15:58:41Z
```

# uv pip compile --include-self

---

_@elbaro_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Feature Request: Add `--include-self` option to `uv pip compile` so that the current package is added in the resulting `requirements.txt`.

With the pyproject.toml below, --include-self adds 'my-pkg' to requirements.txt. Then we can ship `requirements.txt` and `uv sync` to install my-app. This can be done outside uv, but --include-self looks cleaner.

```toml
[project]
name = "my-app"
dependencies = [
  "a",
  "b",
]
```

---

_Comment by @charliermarsh on 2024-04-17 13:40_

How would this differ from `uv pip compile pyproject.toml`?

---

_Comment by @elbaro on 2024-04-17 16:33_

`uv pip compile pyproject.toml` prints a and b, but not my-app.

[project]
name = "my-app"
version = "0.1.2"

should add `my_app==0.1.2`.

---

_Comment by @charliermarsh on 2024-04-20 01:30_

I think you can achieve this with, e.g.: `echo "." | uv pip compile -`. It's not as clean as `--include-self` but it avoids adding more options to the CLI that risk confusing new users.

---

_Comment by @charliermarsh on 2024-04-20 01:30_

Going to pass on this for now though I appreciate the suggestion.

---

_Closed by @charliermarsh on 2024-04-20 01:30_

---
