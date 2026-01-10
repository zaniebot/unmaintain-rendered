---
number: 5569
title: "Copyright rules not selectable with \"CPY\""
type: issue
state: closed
author: Hnasar
labels: []
assignees: []
created_at: 2023-07-06T18:28:10Z
updated_at: 2023-07-10T01:06:05Z
url: https://github.com/astral-sh/ruff/issues/5569
synced_at: 2026-01-10T01:22:44Z
---

# Copyright rules not selectable with "CPY"

---

_Issue opened by @Hnasar on 2023-07-06 18:28_

Unlike with other rules which I can enable with the linter prefix, the CPY rules seems to require the specific error code. 

```
touch foo.py
ruff --select=CPY001 foo.py  # foo.py:1:1: CPY001 Missing copyright notice at top of file
ruff --select=CPY foo.py   # no output
# ruff --version
# ruff 0.0.277
```

Also, the docs still mention the old name of the checker before it was renamed to flake8-copyright in #5236 
https://beta.ruff.rs/docs/rules/#copyright-related-rules-cpy

Thanks for ruff!

---

_Comment by @zanieb on 2023-07-06 22:48_

Hey @Hnasar — this is intentional because this rule is in the "nursery" group which requires explicit opt-in since the rules are still under development. See https://github.com/astral-sh/ruff/pull/4407 for some more background.

The category name was also intentional, see https://github.com/astral-sh/ruff/pull/4701#issuecomment-1599695200 cc @charliermarsh 

---

_Label `question` added by @zanieb on 2023-07-06 22:51_

---

_Comment by @charliermarsh on 2023-07-06 23:05_

I did intend to change the category name, but I think I missed one reference. 

That these rules are in the "nursery" group is supposed to be reflected in the docs which would make this a lot clearer, but we haven't finished that change yet :sob: (https://github.com/astral-sh/ruff/pull/5439)

---

_Comment by @zanieb on 2023-07-07 03:57_

The name should be addressed now by https://github.com/astral-sh/ruff/pull/5577

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:05_

---

_Comment by @charliermarsh on 2023-07-10 01:06_

I'm going to close for now since I think this is mostly blocked on fixing the docs, which is being tracked elsewhere.

---

_Closed by @charliermarsh on 2023-07-10 01:06_

---
