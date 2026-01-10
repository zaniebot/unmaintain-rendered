---
number: 12210
title: "S113 - Rule Doesn't Take `httpx` Default Timeout into Account"
type: issue
state: closed
author: fullerzz
labels:
  - rule
assignees: []
created_at: 2024-07-05T20:31:23Z
updated_at: 2024-07-07T04:47:45Z
url: https://github.com/astral-sh/ruff/issues/12210
synced_at: 2026-01-10T01:22:52Z
---

# S113 - Rule Doesn't Take `httpx` Default Timeout into Account

---

_Issue opened by @fullerzz on 2024-07-05 20:31_

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

The S113 rule has the following description:

> Checks for uses of the Python requests or httpx module that omit the timeout parameter.

This rule fails to take into account that `httpx` enforces a default timeout of 5.0 seconds globally. 

Omitting the timeout parameter in httpx's top-level API calls still results in a 5 second timeout. Likewise, the same applies to both `httpx.Client` and `httpx.AsyncClient`.

### httpx Docs Used for Reference
https://www.python-httpx.org/advanced/timeouts/

> HTTPX is careful to enforce timeouts everywhere by default.
The default behavior is to raise a TimeoutException after 5 seconds of network inactivity.

```python
client = httpx.Client()  # 5s timeout by default
httpx.get('http://example.com/api/v1/example')  # 5s timeout by default
```

Timeouts can be explicitely disabled as so:
```python
# Using the top-level API:
httpx.get('http://example.com/api/v1/example', timeout=None)

# Using a client instance:
with httpx.Client() as client:
    client.get("http://example.com/api/v1/example", timeout=None)
```

### Suggestion
Maybe S113 can check for `timeout=None` in the case of `httpx`. Or I can just disable S113 if this rule is working as intended.

---

_Label `rule` added by @charliermarsh on 2024-07-05 20:31_

---

_Comment by @charliermarsh on 2024-07-05 20:32_

I think it makes sense to respect the default and only flag explicit `timeout=None`, at least based on the above.

---

_Comment by @evanrittenhouse on 2024-07-06 01:33_

Feel free to assign this to me, I have a local PR (mostly) working

---

_Assigned to @evanrittenhouse by @charliermarsh on 2024-07-06 01:58_

---

_Comment by @charliermarsh on 2024-07-06 01:58_

Thanks!

---

_Comment by @trim21 on 2024-07-06 04:32_

https://github.com/astral-sh/ruff/pull/12213

---

_Comment by @trim21 on 2024-07-07 03:43_

https://github.com/astral-sh/ruff/pull/12213 is merged, maybe we can close this?

---

_Closed by @charliermarsh on 2024-07-07 04:47_

---
