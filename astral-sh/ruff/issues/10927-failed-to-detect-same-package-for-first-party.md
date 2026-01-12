```yaml
number: 10927
title: "Failed to `detect-same-package` for `first-party`"
type: issue
state: closed
author: serjflint
labels:
  - question
  - isort
assignees: []
created_at: 2024-04-14T15:44:46Z
updated_at: 2024-05-22T08:36:17Z
url: https://github.com/astral-sh/ruff/issues/10927
synced_at: 2026-01-12T15:54:50Z
```

# Failed to `detect-same-package` for `first-party`

---

_@serjflint_

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

Hi!

ruff 0.3.4

I am trying to sort imports like `ruff check library/module.py --fix-only`

```python
from plugins import a

from library import b
```

My config is

```toml
[tool.ruff.lint]
select = ["I"]

[tool.ruff.lint.isort]
detect-same-package = true

section-order = [
    "future",
    "standard-library",
    "third-party",
    "appnexus",
    "first-party",
    "local-folder",
]
[tool.ruff.lint.isort.sections]
appnexus = [
    "library",
    "plugins",
]
```

In the end I receive

```python
from library import b
from plugins import a
```

Instead of the file staying the same.




---

_Comment by @charliermarsh on 2024-04-14 15:53_

Can you explain you expect something other than the result you showed above? You marked them as being part of the same section.

---

_Label `question` added by @charliermarsh on 2024-04-14 15:53_

---

_Label `isort` added by @AlexWaygood on 2024-04-14 16:10_

---

_Comment by @serjflint on 2024-04-14 16:23_

> Can you explain you expect something other than the result you showed above? You marked them as being part of the same section.

I expect ruff do not change anything in the first snippet as `module.py` is a part of the `library`.

---

_Comment by @serjflint on 2024-04-14 16:25_

If `module.py` was outside of `library` (and `plugins`) it would be right to receive the second snippet.

---

_Comment by @charliermarsh on 2024-04-14 16:55_

But you have:
```toml
appnexus = [
    "library",
    "plugins",
]
```

And so imports of `library` and `plugins` are being assigned to that section, correctly.

---

_Comment by @serjflint on 2024-04-15 09:21_

> But you have:
> 
> ```toml
> appnexus = [
>     "library",
>     "plugins",
> ]
> ```
> 
> And so imports of `library` and `plugins` are being assigned to that section, correctly.

But when I write code in `library` `package` and import relative module using absolute import I expect it to be counted as `local-folder` or `first-party`.

---

_Comment by @charliermarsh on 2024-04-17 14:50_

If you have:

```
appnexus = [
    "library",
    "plugins",
]
```

Then any imports of modules named `library` or `plugins` will be classified as `appnexus`.

---

_Comment by @serjflint on 2024-04-17 14:55_

> If you have:
> 
> ```
> appnexus = [
>     "library",
>     "plugins",
> ]
> ```
> 
> Then any imports of modules named `library` or `plugins` will be classified as `appnexus`.

I see. But I think my case is valid for custom sections. Is it possible to modify this behavior?

---

_Comment by @charliermarsh on 2024-05-22 03:11_

I think in that case you'd want to omit `library` from `appnexus`, since you want library imports to be considered first-party?

---

_Comment by @charliermarsh on 2024-05-22 03:12_

Going to close but I can keep answering questions if they come up.

---

_Closed by @charliermarsh on 2024-05-22 03:12_

---

_Comment by @serjflint on 2024-05-22 08:36_

> I think in that case you'd want to omit `library` from `appnexus`, since you want library imports to be considered first-party?

It is `appnexus` for the whole project, but an absolute first-party import when inside `library`.

---
