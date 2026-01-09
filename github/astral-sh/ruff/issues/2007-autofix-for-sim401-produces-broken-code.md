---
number: 2007
title: Autofix for SIM401 produces broken code
type: issue
state: closed
author: henryiii
labels:
  - bug
assignees: []
created_at: 2023-01-20T01:56:24Z
updated_at: 2023-01-20T02:57:20Z
url: https://github.com/astral-sh/ruff/issues/2007
synced_at: 2026-01-07T13:12:14-06:00
---

# Autofix for SIM401 produces broken code

---

_Issue opened by @henryiii on 2023-01-20 01:56_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->

I was trying this on cibuildwheel, and got a broken autofix:

```diff
diff --git a/cibuildwheel/options.py b/cibuildwheel/options.py
index 6b7b335f..d5295083 100644
--- a/cibuildwheel/options.py
+++ b/cibuildwheel/options.py
@@ -558,8 +558,7 @@ def build_options(self, identifier: str | None) -> BuildOptions:
                     if not config_value:
                         # default to manylinux2014
                         image = pinned_images["manylinux2014"]
-                    elif config_value in pinned_images:
-                        image = pinned_images[config_value]
+                    image = pinned_images.get(config_value, config_value)
                     else:
                         image = config_value
```

That's broken. MWE:

```python
config_value = 1
pinned_images = {1}
pinned_images = dict()

if not config_value:
    # default to manylinux2014
    image = pinned_images["manylinux2014"]
elif config_value in pinned_images:
    image = pinned_images[config_value]
else:
    image = config_value
```

```console
> ruff --fix --isolated --select SIM401 tmp.py
```

```python
config_value = 1
pinned_images = {1}
pinned_images = dict()

if not config_value:
    # default to manylinux2014
    image = pinned_images["manylinux2014"]
image = pinned_images.get(config_value, config_value)
else:
    image = config_value
```

Will show the issue. Using 0.0.225 (from brew, isolated example above) & 0.0.226 (in pre-commit, for cibuildwheel).



---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-20 02:02_

---

_Label `bug` added by @charliermarsh on 2023-01-20 02:02_

---

_Comment by @charliermarsh on 2023-01-20 02:03_

Will fix this tonight. I actually thought we _had_ fixed this, but maybe it was just the exact same issue with a different rule. (Correcting myself: yes, it was SIM103 via https://github.com/charliermarsh/ruff/issues/1943.)

---

_Referenced in [astral-sh/ruff#2012](../../astral-sh/ruff/pulls/2012.md) on 2023-01-20 02:48_

---

_Closed by @charliermarsh on 2023-01-20 02:57_

---
