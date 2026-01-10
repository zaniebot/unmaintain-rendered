---
number: 10026
title: "Keep single-line signatures when body is an Ellipsis (`...`) in `.py` files"
type: issue
state: closed
author: etienneschalk
labels: []
assignees: []
created_at: 2024-02-18T13:39:16Z
updated_at: 2024-02-18T17:10:18Z
url: https://github.com/astral-sh/ruff/issues/10026
synced_at: 2026-01-10T01:22:49Z
---

# Keep single-line signatures when body is an Ellipsis (`...`) in `.py` files

---

_Issue opened by @etienneschalk on 2024-02-18 13:39_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

## A minimal code snippet that reproduces the bug.

File name: `format_ruff.py`

Before ruff:

```python
class MyClass:
    def __add__(self, other: int) -> int: ...
```

After ruff:

```python
class MyClass:
    def __add__(self, other: int) -> int:
        ...
```

## The command you invoked

```bash
ruff format format_ruff.py 
```

Note: in real-life scenario, one might have installed the Ruff extension on VSCode, opened a file with many stubs like in the minimal code snippet, and see them all formatted, introducing unwanted code change, which can be annoying for version control.

This might also slow down ruff adoption in existing large code bases where the case mentioned above happens a lot.

## The current Ruff settings

No overriden settings. I ran a `pip install ruff` then ran the command above

##  The current Ruff version 

```bash
ruff --version
ruff 0.2.2
```

## What I would like

A way to opt-out of de-inlining the Ellipsis (`...`) in stubs. I have searched in documentation without finding such an option. I browsed the following issues (in Links section).

An option could be:

```toml
[tool.ruff.format]
keep-single-line-ellipsis = true
```

## Links

Related Issues:
- https://github.com/astral-sh/ruff/issues/5822 -> It seems the solution only applied to `.pyi` files, not `.py` ; I would like the option to be activable for all types of files.
- https://github.com/psf/black/issues/1797 (referred to in the issue above)

Related PRs:
- https://github.com/astral-sh/ruff/pull/6592 (exposes the same situation described in this issue! it seems to only apply to `.pyi` files, but maybe it could be generalized to `.py`?)
- https://github.com/astral-sh/ruff/pull/6501


Thanks!

---

_Referenced in [pydata/xarray#8761](../../pydata/xarray/pulls/8761.md) on 2024-02-18 13:40_

---

_Comment by @MichaReiser on 2024-02-18 15:50_

Thanks for the excellent writeup! We've implemented your desired behavior but it's currently only applied when you format your code with `--preview` mode enabled as you can see here https://play.ruff.rs/6cc50dba-b5ae-4dea-9f58-14b179932e65. 

We hope to promote the new formatting to stable in this month, but you can already start using it if you enable `ruff.format.preview = true` or call `ruff format --preview`

Let me know if that works for you

---

_Comment by @etienneschalk on 2024-02-18 16:35_

Hello, thanks for you answer! 

I ran a `pre-commit run --all` after enabling the `preview` flag in `pyproject.toml`:

```toml
[tool.ruff.format]
preview = true
```

It puts back the Ellipses back on their single line :+1: 

However, it also brings new changes, mainly compressing multiple braces in a javascript formatting style.

Excerpt from the xarray repo:

```python
-        self.ds = Dataset(
-            {
-                "a": (("x", "y"), np.ones((300, 400))),
-                "b": (("x", "y"), np.ones((300, 400))),
-            }
-        )
+        self.ds = Dataset({
+            "a": (("x", "y"), np.ones((300, 400))),
+            "b": (("x", "y"), np.ones((300, 400))),
+        })
```

Even though I personally like this new formatting (it looks like javascript formatting, is more concise, and saves  a level of indentation), I would still like to get more granularity inside of the `preview` flag, to choose which features to opt-in (orthogonal configuration), reducing code diff at the same time in a migration context. 

According to the [documentation of Preview](https://docs.astral.sh/ruff/preview/#enabling-preview-mode) we can select rules for the linter with `explicit-preview-rules = true`, however I cannot find an equivalent for the formatter. Is there a "formatting rule" concept, equivalent to the "linting rules", that would enable me to opt-in only the Ellipsis formatting rule?

Thanks!

---

_Comment by @MichaReiser on 2024-02-18 16:55_

That makes sense. There's no such granular opt-in to preview formatting. It's all or nothing, similar to the formatter itself which only provides very few configuration nobs. 

For preview, this is especially important because it ensures that users give feedback to all preview styles and don't just disable the styles they don't like. It prevents situations where all users disabled a controversial style change but we decide releasing it because we never received any negative feedback on the style.

---

_Comment by @etienneschalk on 2024-02-18 17:03_

Okay, I fully understand the point of having a single preview style! 

Thanks a lot for the help on that topic!

---

_Comment by @MichaReiser on 2024-02-18 17:10_

I'll close this issue. You can follow https://github.com/astral-sh/ruff/issues/8678 to get notified when we promote the new preview styles (The style you're calling out won't be promoted)

---

_Closed by @MichaReiser on 2024-02-18 17:10_

---

_Referenced in [astral-sh/ruff#11676](../../astral-sh/ruff/issues/11676.md) on 2024-06-01 08:44_

---
