```yaml
number: 3744
title: nbqa + isort produces false positive
type: issue
state: closed
author: patrick-kidger
labels:
  - bug
assignees: []
created_at: 2023-03-26T17:00:39Z
updated_at: 2023-09-14T20:02:53Z
url: https://github.com/astral-sh/ruff/issues/3744
synced_at: 2026-01-12T15:54:44Z
```

# nbqa + isort produces false positive

---

_@patrick-kidger_

Given a `ipynb` notebook cell containing only imports:
```
{
  "cells": [
    {
      ...
      "source": [
        "import foo"
      ]
    },
    ...
  ]
}
```
then
```
nbqa ruff --select=I001 --diff my_notebook.ipynb
```
produces
```
@@ -1,6 +1,7 @@
 # %%NBQA-CELL-SEP1d84dd
 import foo
 
+
```
i.e. it attempts to add superfluous padding at the end.

If e.g. the config file has `lines-after-imports = 2` then two superfluous lines of padding are added.

---

_Comment by @charliermarsh on 2023-03-26 17:15_

Ahh interesting. I'm assuming this is because `nbqa` concatenates the cells internally, and so can't draw a distinction between a cell consisting of _just_ imports (with non-import content in the subsequent) and a single cell containing both imports and further content.

---

_Comment by @charliermarsh on 2023-03-26 17:16_

I'll have to look into how the `nbqa` integration works (since that's implemented in `nbqa`), though we're also in the process of shipping first-class Jupyter support within Ruff so we'd be much more likely to be able to fix this once that's live.

---

_Label `bug` added by @charliermarsh on 2023-03-26 17:16_

---

_Comment by @MarcoGorelli on 2023-04-01 15:38_

hey - just FYI, I'm hoping to have a fix for this on the nbqa side some time this weekend

looking forward to first-class jupyter support directly in ruff anyway!

---

_Comment by @MarcoGorelli on 2023-04-03 07:31_

Hi @patrick-kidger - could you try with nbqa version 1.7.0 please? I'd like to think this is now fixed

---

_Comment by @patrick-kidger on 2023-04-03 19:40_

Hmm, I'm trying it with
```
repos:
  - repo: https://github.com/nbQA-dev/nbQA
    rev: 1.7.0
    hooks:
    - id: nbqa-ruff 
```
and am still getting
```
nbqa-ruff................................................................Failed
- hook id: nbqa-ruff
- exit code: 1

my_notebook.ipynb:cell_1:1:1: I001 [*] Import block is un-sorted or un-formatted
```

My use-case is linting this repo: https://github.com/patrick-kidger/equinox/tree/3744-reproducer. If you want a reproducer than clone the branch I've linked there and run `pre-commit run --all-files`.

---

_Comment by @MarcoGorelli on 2023-04-03 20:18_

> If you want a reproducer than clone the branch I've linked there and run pre-commit run --all-files.

thanks, this is useful! will look into it

---

_Comment by @MarcoGorelli on 2023-04-04 18:51_

OK got it - it's because you have `lines-after-imports = 2`

I'm just having a look to see if there's an easy way to turn that off for notebooks which I can document and suggest

---

_Comment by @charliermarsh on 2023-09-14 20:02_

I just confirmed that running Ruff directly on the notebook has the expected behavior (e.g., `ruff check Untitled.ipynb --select I --fix`).

Going to close for now since any further refinements in `nbqa` can probably be tracked over there!

---

_Closed by @charliermarsh on 2023-09-14 20:02_

---
