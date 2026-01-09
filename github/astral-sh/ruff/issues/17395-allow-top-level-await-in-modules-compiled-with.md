---
number: 17395
title: "Allow top-level `await` in modules compiled with `ast.PyCF_ALLOW_TOP_LEVEL_AWAIT`"
type: issue
state: open
author: ntBre
labels:
  - needs-design
assignees: []
created_at: 2025-04-14T17:01:21Z
updated_at: 2025-04-25T16:55:52Z
url: https://github.com/astral-sh/ruff/issues/17395
synced_at: 2026-01-07T13:12:16-06:00
---

# Allow top-level `await` in modules compiled with `ast.PyCF_ALLOW_TOP_LEVEL_AWAIT`

---

_Issue opened by @ntBre on 2025-04-14 17:01_

This is related to the `await` outside async function syntax error, which currently corresponds to [await-outside-async (PLE1142)](https://docs.astral.sh/ruff/rules/await-outside-async/). It's not currently an issue because the syntax error is still converted to a PLE1142 diagnostic in ruff, but we'll want to have a plan for handling this before stabilizing it as a syntax error.

> This one might not belong in syntax-errors and might be better to remain as a disableable rule. Top level await is allowed if the module is compiled with https://docs.python.org/3/library/ast.html#ast.PyCF_ALLOW_TOP_LEVEL_AWAIT. This is useful when working with pyodide, as modules can assume an event loop is running prior to them being imported.

_Originally posted by @mikeshardmind in https://github.com/astral-sh/ruff/issues/17363#issuecomment-2797864786_
            

---

_Assigned to @ntBre by @ntBre on 2025-04-14 17:01_

---

_Label `needs-design` added by @ntBre on 2025-04-14 17:05_

---

_Comment by @dhruvmanila on 2025-04-14 22:46_

Is this even possible to detect statically?

---

_Comment by @ntBre on 2025-04-14 22:49_

Not as far as I understand. I think we'd need some kind of config option if we want to support this.

---

_Comment by @AlexWaygood on 2025-04-15 16:13_

I think we'll want this one to be suppressible on a per-project basis in red-knot as well. Mypy has had similar issues when it comes to checking Jupyter notebooks, which can sometimes use `await` at the top level. (I'm guessing we'll check Jupyter notebooks as isolated projects from red-knot.) See https://github.com/python/mypy/issues/15339, https://github.com/python/mypy/issues/18795, etc.

---

_Comment by @dhruvmanila on 2025-04-15 18:14_

We haven't explored Jupyter Notebook support for red-knot but (a bit hand wavy) I don't think it would be an isolated project because the notebook can import modules from the current project. Anyways, we don't need to discuss that here.

---

_Comment by @MichaReiser on 2025-04-22 09:29_

Interesting. What are the options we have here?

* Keep it a rule
* Introduce parser options (per file?)
* Always allow 


---

_Comment by @AlexWaygood on 2025-04-22 10:21_

> Always allow

I don't think this is a realistic option for non-notebook files: for the vast majority of Python code this is a syntax error; we should definitely flag it by default for non-notebooks.

I'm not totally familiar with the situation for notebooks, honestly, as I've never seriously used notebooks. I can't tell you whether top-level `await`s are allowed by default in notebooks or if it's only if some settings are enabled. I believe they're much more common in notebooks than in regular Python source code, however. For notebooks, we _may_ want it to be an opt-in diagnostic rather than an opt-out diagnostic.

And even for regular Python source code, we'll want this to be at least suppressible on a per-file basis. Although it's pretty rare to use `ast.PyCF_ALLOW_TOP_LEVEL_AWAIT` for non-notebooks, it is _possible_.

I don't have strong opinions on how we make this diagnostic suppressible -- whether it stays as a linter rule or if you add per-file parser options.

---

_Comment by @dhruvmanila on 2025-04-25 16:55_

> I'm not totally familiar with the situation for notebooks, honestly, as I've never seriously used notebooks. I can't tell you whether top-level `await`s are allowed by default in notebooks or if it's only if some settings are enabled. I believe they're much more common in notebooks than in regular Python source code, however. For notebooks, we _may_ want it to be an opt-in diagnostic rather than an opt-out diagnostic or turn it off for notebooks altogether which is what the rule currently does.

For Notebooks, if the backend uses `ipykernel` (which is probably most of the notebooks) there's an option [`autoawait`](https://ipython.readthedocs.io/en/stable/interactive/autoawait.html#using-autoawait-in-a-notebook-ipykernel) which is enabled by default but a user can turn it off which would then make top-level `async` usages a syntax error. So, we should definitely have that as opt-in at least for notebooks.

<img width="806" alt="Image" src="https://github.com/user-attachments/assets/0b178271-93e6-4cf7-bd73-c5311083520c" />


---

_Referenced in [astral-sh/ruff#19122](../../astral-sh/ruff/issues/19122.md) on 2025-07-10 10:36_

---

_Referenced in [astral-sh/ty#1823](../../astral-sh/ty/issues/1823.md) on 2025-12-09 11:58_

---
