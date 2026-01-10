---
number: 7890
title: Are isort configurations able to be skipped or just applicable in some files?
type: issue
state: closed
author: BT-rmartin
labels: []
assignees: []
created_at: 2023-10-10T09:31:02Z
updated_at: 2023-11-02T22:56:42Z
url: https://github.com/astral-sh/ruff/issues/7890
synced_at: 2026-01-10T01:22:47Z
---

# Are isort configurations able to be skipped or just applicable in some files?

---

_Issue opened by @BT-rmartin on 2023-10-10 09:31_

Hi, I would like to know if there is a way to skip the application of some isort checks for some specific files, or the other way round, to enforce the application of some isort configurations for specific files. 

Here you have an example. 
In general, I would like to have something like this in the files gathering imports
![image](https://github.com/astral-sh/ruff/assets/9987218/570a86f0-ee05-484c-9fb9-cc659272a6e2)

However for the __init__py file, I would prefer the single line imports, but it's changed to gather imports 
![image](https://github.com/astral-sh/ruff/assets/9987218/b868890f-d667-410a-9fcc-2d70ae317667)

If I configure it like this, it keeps the format of __init__.py but splits the other imports of the other files.
[tool.ruff.isort] 
force-single-line = true

So I wonder if there is a way to apply different isort configurations for different files. 

If this option is not available, it would be super great if it could be added in the near future. 
Something similar to ignore some checks in particular files, but for isort configurations. 
[tool.ruff.per-file-ignores]
"__init__.py" = ["F401"]
"__manifest__.py" = ["B018"]
"**/tests/*.py" = ["S101"]

Thanks for the help in advance

---

_Comment by @charliermarsh on 2023-10-10 16:02_

We don't support this kind of file-scoped configuration. The general request has come up a few times (e.g., https://github.com/astral-sh/ruff/issues/7696).

That being said... if you use an `as` alias, Ruff will avoid merging them:

```python
from . import account_account as account_account
```

Ruff will also avoiding marking them as unused in this case. I believe this pattern is quite common -- Pyright treats these redundant aliases as public exports: https://github.com/python/typing/blob/master/docs/source/libraries.rst#library-interface-public-and-private-symbols.

---

_Comment by @BT-rmartin on 2023-10-11 10:49_

Thanks for the quick answer @charliermarsh 
I'm not so interested in using such aliases. 
Any plan to do that file-scope config in the near future or is it discarded?
Any other potential workaround?

---

_Comment by @charliermarsh on 2023-10-11 13:36_

Okay, got it. I do consider it a valid solution as it's the recommended mechanism in the typing community for marking imported symbols as public rather than private (and so if you're re-importing things in an `__init__.py` file for the purpose of re-exporting them, and you want re-exports to be formatted differently than other imports as above, it seems like an appropriate pattern to use). But hopefully we will support file-scoped configuration at some point. I expect that we _will_ support it, though I doubt it will be within the next month or two.

I'm going to merge into https://github.com/astral-sh/ruff/issues/7696 and make that issue more general to track the file-scoped configuration request.


---

_Closed by @charliermarsh on 2023-10-11 13:36_

---

_Comment by @BT-rmartin on 2023-10-26 23:28_

> We don't support this kind of file-scoped configuration. The general request has come up a few times (e.g., #7696).
> 
> That being said... if you use an `as` alias, Ruff will avoid merging them:
> 
> ```python
> from . import account_account as account_account
> ```
> 
> Ruff will also avoiding marking them as unused in this case. I believe this pattern is quite common -- Pyright treats these redundant aliases as public exports: https://github.com/python/typing/blob/master/docs/source/libraries.rst#library-interface-public-and-private-symbols.

@charliermarsh I was by the way trying this approach and it continues being replaced
For instance, from 
from . import test_crm_activity_report as test_crm_activity_report
from . import test_crm_lead as test_crm_lead
from . import test_disable_records as test_disable_records

into 
from . import test_crm_activity_report, test_crm_lead, test_disable_records

when running ruff check . --fix

Any other solution?

---

_Comment by @dhruvmanila on 2023-10-27 08:41_

Hey, can you provide us with the config being used for this? I'm unable to reproduce this locally with `v0.1.3`. If I use aliases, then it keeps the imports separated and it'll only merge when I remove the aliases.

---

_Comment by @BT-rmartin on 2023-11-02 22:56_

@dhruvmanila 

Here it is. Maybe it was a version issue. I had ruff 0.0.278. Now with 0.1.3 it's respected but the imports are reordered

My configuration was like this

[tool.ruff]
select = ["E", "F", "I", "PL", "B", "Q", "D", "N", "S", "G", "RUF", "W"]
ignore = ["D100", "D101", "D104", "D213", "D203", "RUF012"]
exclude = ["**/ext/"]

line-length = 115

[tool.ruff.per-file-ignores]
"__init__.py" = ["F401"]
"__manifest__.py" = ["B018"]
"**/tests/*.py" = ["S101"]

[tool.ruff.flake8-quotes]
docstring-quotes = "double"
inline-quotes = "single"
multiline-quotes = "double"
avoid-escape = false


---

_Referenced in [astral-sh/ruff#7696](../../astral-sh/ruff/issues/7696.md) on 2024-01-11 17:05_

---
