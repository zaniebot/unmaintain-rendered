```yaml
number: 19980
title: Ruff formatter still removes semicolon in Jupyter notebooks
type: issue
state: closed
author: jsh9
labels:
  - formatter
  - needs-info
  - style
assignees: []
created_at: 2025-08-18T23:07:55Z
updated_at: 2025-11-17T07:20:11Z
url: https://github.com/astral-sh/ruff/issues/19980
synced_at: 2026-01-10T11:09:59Z
```

# Ruff formatter still removes semicolon in Jupyter notebooks

---

_Issue opened by @jsh9 on 2025-08-18 23:07_

### Summary

Even with this fix (https://github.com/astral-sh/ruff/issues/8254), Ruff formatter still removes semicolon in Jupyter notebooks.

Steps to reproduce this:

1. Install Ruff 0.12.9
2. Clone this repo: https://github.com/jsh9/pyseismosoil (at this commit: d2be10711e899593a99c33c588138aa0ad06a6f0)
3. Put this in `ruff.toml`:

```
line-length = 79

[format]
quote-style = "single"
docstring-code-format = true
```

4. Run `ruff format .`
5. Check git diff

```bash
>>> git diff examples/Demo_03_Frequency_Spectrum.ipynb
diff --git a/examples/Demo_03_Frequency_Spectrum.ipynb b/examples/Demo_03_Frequency_Spectrum.ipynb
index 2cd833f..1a6d708 100644
--- a/examples/Demo_03_Frequency_Spectrum.ipynb
+++ b/examples/Demo_03_Frequency_Spectrum.ipynb
@@ -588,7 +588,7 @@
     }
    ],
    "source": [
-    "smoothed, fig, ax = fs.get_smoothed(show_fig=True, log_scale=True);"
+    "smoothed, fig, ax = fs.get_smoothed(show_fig=True, log_scale=True)"
    ]
   },
   {
```
This semicolon should not be removed. (Several other Jupyter notebooks in this repo also suffered from this bug.)

### Version

0.9.12

---

_Label `bug` added by @MichaReiser on 2025-08-19 06:22_

---

_Label `formatter` added by @MichaReiser on 2025-08-19 06:22_

---

_Comment by @ntBre on 2025-08-19 14:38_

Thanks, I can reproduce this on 0.12.9 too:

```shell
> ruff format Untitled.ipynb --diff
--- Untitled.ipynb:cell 1
+++ Untitled.ipynb:cell 1
@@ -1 +1 @@
-import math;
+import math

1 file would be reformatted
```

---

_Comment by @MichaReiser on 2025-08-19 15:16_

The PR that added this feature lists a few conditions that must be fulfilled for the formatter to preserve the semicolon:

The conditions required as to when the trailing semicolon should be preserved are:
  1. It should be a top-level statement which is last in the module.
  2. For statement, it can be either assignment, annotated assignment, or augmented assignment.
     Here, the target should only be a single identifier i.e., multiple assignments or tuple     unpacking isn't considered.
  3. For expression, it can be any.


https://github.com/astral-sh/ruff/pull/8590

@dhruvmanila might have more context on the why

---

_Label `bug` removed by @MichaReiser on 2025-08-19 15:16_

---

_Label `style` added by @MichaReiser on 2025-08-19 15:16_

---

_Comment by @dhruvmanila on 2025-08-19 15:23_

Looking into this...

---

_Comment by @dhruvmanila on 2025-08-19 15:35_

Can you specify why would you not want to remove the semicolon in that position?

IIRC, the main advantage of using a semicolon is to hide the output if that's not intended. The main reason that this heuristic was chosen is the [`ast_node_interactivity`](https://ipython.readthedocs.io/en/stable/api/generated/IPython.core.interactiveshell.html#IPython.core.interactiveshell.InteractiveShell.ast_node_interactivity) config option which I played around with when I was working on this but currently I don't remember exactly why is it so.

Cloning the linked repository and running the mentioned notebook suggests that the semicolon is unnecessary because the chart is plotted regardless of whether the semicolon is present or not as seen in the below video:

https://github.com/user-attachments/assets/e6e1a5ad-725f-4b33-9d15-ad7601cba486

Can you provide an example where it's changing the behavior i.e., is removing the semicolon making the output visible when it wasn't when the semicolon was present?

---

_Comment by @dhruvmanila on 2025-08-19 15:42_

I think IPython only chooses to show the output in certain conditions which, IIRC, is what I've tried to replicate here so that the semicolons are still removed in other conditions.

Additionally, I tried to document the conditions in the test notebook (https://github.com/astral-sh/ruff/blob/main/crates/ruff/resources/test/fixtures/trailing_semicolon.ipynb) by running the notebook with various combinations of statements / expressions with or without semicolons. You can see in what conditions the output is shown and not shown.

---

_Label `needs-info` added by @MichaReiser on 2025-10-02 11:24_

---

_Closed by @MichaReiser on 2025-11-17 07:20_

---
