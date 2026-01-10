```yaml
number: 12200
title: "[New Rule][Django] Verify that `settings` is correctly imported"
type: issue
state: closed
author: bastian-wattro
labels:
  - rule
assignees: []
created_at: 2024-07-05T08:17:57Z
updated_at: 2024-07-23T07:39:09Z
url: https://github.com/astral-sh/ruff/issues/12200
synced_at: 2026-01-10T11:09:54Z
```

# [New Rule][Django] Verify that `settings` is correctly imported

---

_Issue opened by @bastian-wattro on 2024-07-05 08:17_

I'd like to add a rule for Django projects. I'm not quite sure about where to add it (If backwards capability was not an issue, I would add it to DJ)

## What it could do

Verify that `settings` is correctly imported from `django.conf`.

An unsafe fix could exist, as some code might depend on the side effects of importing the module (even though import side-effects are considered poor practice).

## Why is this bad?

[The Django Docs](https://docs.djangoproject.com/en/5.0/topics/settings/#using-settings-in-python-code) state that one *should* import `settings` from `django.conf`.
It is an object (not a module) that is safe to access. Importing settings directly from the project could trigger side-effects.
Also, `django.conf.settings` abstracts the concepts of default settings and site-specific settings; it presents a single interface. It also decouples the code that uses settings from the location of the settings.


## Example
Incorrect:
```python
from myproject import settings
```
Use instead:
```python
from django.conf import settings
```

---

I'm looking forward to your feedback.


---

_Label `rule` added by @MichaReiser on 2024-07-11 06:56_

---

_Comment by @93578237 on 2024-07-23 05:24_

You can already do this with banned-api (no auto fix)

---

_Comment by @bastian-wattro on 2024-07-23 07:39_

Nice.

Add this to your pyproject.toml  and replace `myproject` with the name of your project.   
drop `tool.ruff.` if you're using ruff.toml
```toml
[tool.ruff]
lint.select = [
   …
   "TID251", # tidy imports
]
…
[tool.ruff.lint.flake8-tidy-imports.banned-api]
"myproject.settings".msg = "See https://docs.djangoproject.com/en/dev/topics/settings/#using-settings-in-python-code. Use 'from django.conf import settings' instead."
```

---

_Closed by @bastian-wattro on 2024-07-23 07:39_

---
