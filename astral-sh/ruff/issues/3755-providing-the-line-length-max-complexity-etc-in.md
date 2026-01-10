```yaml
number: 3755
title: Providing the line length, max-complexity, etc in --statistics
type: issue
state: open
author: cclauss
labels:
  - wish
assignees: []
created_at: 2023-03-26T23:30:12Z
updated_at: 2023-05-02T21:59:38Z
url: https://github.com/astral-sh/ruff/issues/3755
synced_at: 2026-01-10T11:09:46Z
```

# Providing the line length, max-complexity, etc in --statistics

---

_Issue opened by @cclauss on 2023-03-26 23:30_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
@JonathanPlasse

Similar to #3748 but for providing the value for `line-length`, `max-complexity`, `max-args`… when running `ruff --select=ALL --statistics .`

When linting a new repo, I might need to set all seven of the following numbers.
```toml
[tool.ruff]
line-length = 100  # Recommended: 88

[tool.ruff.mccabe]
max-complexity = 15   # Recommended: 10

[tool.ruff.pylint]
max-args = 8   # Recommended: 5
max-branches = 17   # Recommended: 12
max-returns = 12   # Recommended: 6
max-statements = 52   # Recommended: 50
```
It takes a bit of trial and error to find the length of longest line or the most complex function.  It would save a lot of time and aggravation if `--statistics` did not just show a random `E501` error but instead showed me the error message from the longest line.  I could the copy that `line-length` value and paste it into `pyproject.toml`


---

_Label `wishlist` added by @charliermarsh on 2023-03-26 23:31_

---

_Comment by @arya-k on 2023-05-02 21:59_

I'd like to try and tackle this!

---
