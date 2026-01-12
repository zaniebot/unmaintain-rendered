```yaml
number: 3981
title: Unexpected change in string contents
type: issue
state: closed
author: sbdchd
labels: []
assignees: []
created_at: 2023-04-16T00:26:35Z
updated_at: 2023-04-16T23:27:10Z
url: https://github.com/astral-sh/ruff/issues/3981
synced_at: 2026-01-12T15:54:44Z
```

# Unexpected change in string contents

---

_@sbdchd_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

not entirely sure which rule is changing it, but for some reason the help text string for a Django ORM column description is being rewritten from using a fancy forward quote: `’` to use a tilde `

```diff
 customer_email = models.CharField(
-    max_length=255, help_text="The customer’s email address."
+    max_length=255, help_text="The customer`s email address."
 )
```

---

_Comment by @dhruvmanila on 2023-04-16 08:00_

Duplicate of #3977

---

_Closed by @sbdchd on 2023-04-16 23:27_

---
