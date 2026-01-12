```yaml
number: 5118
title: "Suggestion: per-file-includes"
type: issue
state: closed
author: kobymeir
labels: []
assignees: []
created_at: 2023-06-15T10:43:32Z
updated_at: 2025-11-06T22:04:01Z
url: https://github.com/astral-sh/ruff/issues/5118
synced_at: 2026-01-12T15:54:45Z
```

# Suggestion: per-file-includes

---

_@kobymeir_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
As shown in the configuration there is an option to exclude rules:
https://beta.ruff.rs/docs/configuration/#using-pyprojecttoml

I want an option to include rules on specific files patterns
for example, I want to force specific rules on `_test.py` files in my project.

```
# enforce `E402` (import violations) in all `*_test.py` files.
[tool.ruff.per-file-ignores]
"*_test.py" = ["E402"]
```


---

_Renamed from "Suggestion: per-file-ignores" to "Suggestion: per-file-includes" by @kobymeir on 2023-06-15 10:43_

---

_Comment by @charliermarsh on 2023-06-15 13:41_

👍 I do want to support something like this, though I believe this is a duplicate of #3172.

---

_Closed by @charliermarsh on 2023-06-15 13:41_

---

_Comment by @kobymeir on 2023-06-15 14:45_

My use case is a bit different, as I'm trying to use the banned-api https://beta.ruff.rs/docs/settings/#flake8-tidy-imports-banned-api within `*_test.py` files.

Maybe this example of what i'm try to accomplish is better:
```ini
[tool.ruff.flake8-tidy-imports]
[tool.ruff.flake8-tidy-imports.banned-api]
"random".msg = "The random module is forbidden within test files, as it might cause them not to be deterministic"
"random".pattern = "**/*_test.py"
```




---
