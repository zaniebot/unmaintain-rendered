---
number: 13025
title: "Check whether `import`s are at top of notebook"
type: issue
state: open
author: redeboer
labels:
  - rule
  - needs-decision
  - notebook
assignees: []
created_at: 2024-08-21T07:46:06Z
updated_at: 2024-10-09T04:45:36Z
url: https://github.com/astral-sh/ruff/issues/13025
synced_at: 2026-01-10T01:22:53Z
---

# Check whether `import`s are at top of notebook

---

_Issue opened by @redeboer on 2024-08-21 07:46_

Since #8872, [`E402`](https://docs.astral.sh/ruff/rules/module-import-not-at-top-of-file/) works per cell in Jupyter notebooks. However, in some projects, you may want to have the `import`s in one cell on the top of the notebook (e.g. to avoid cluttering code cells further below).

Would it be an idea to either introduce:
1. a new [`lint.pycodestyle`](https://docs.astral.sh/ruff/settings/#lintpycodestyle) setting like `imports-at-top-of-notebook` (defaults to `False`)?
2. a new `E403` rule that is specifically for notebooks that works **at notebook level**? This is a variant of what @dhruvmanila suggested in https://github.com/astral-sh/ruff/pull/8872#issue-2015007451.

I would suggest 1., to avoid potential rule code clashes (e.g. #2194).

---

_Referenced in [ComPWA/compwa.github.io#270](../../ComPWA/compwa.github.io/pulls/270.md) on 2024-08-21 07:46_

---

_Label `rule` added by @AlexWaygood on 2024-08-21 14:55_

---

_Label `needs-decision` added by @AlexWaygood on 2024-08-21 14:55_

---

_Label `notebook` added by @AlexWaygood on 2024-08-21 14:55_

---

_Comment by @dhruvmanila on 2024-08-23 05:21_

Thanks for the feature request. I think this is a useful feature. 

We also discussed about providing a dedicated code action to move all imports in a Jupyter Notebook to a dedicated code cell at the top. Just want to confirm whether this request is to move the imports to a _dedicated_ code cell at the top or just to the first code cell (might contain other Python code) in the notebook.

---

_Comment by @redeboer on 2024-08-23 08:41_

> We also discussed about providing a dedicated code action to move all imports in a Jupyter Notebook to a dedicated code cell at the top.

Awesome!

> Just want to confirm whether this request is to move the imports to a _dedicated_ code cell at the top or just to the first code cell (might contain other Python code) in the notebook.

Ah that's a good point. In practice, we usually have a dedicated cell for this yes, but I'm not sure if everyone works like that. Also, we sometimes put global configurations like `logging` levels or `matplotlib` styles in that top cell (example [here](https://compwa.github.io/report/030.html)).

Perhaps this could be configurable too? Difficult to think of good keywords here though, maybe `imports-at-top-of-notebook = True` and `imports-at-top-of-notebook = "separate"`? Well as I'm writing this, they already sound like a bad idea...

One could even consider implementing a a separate, additional rule that checks whether `import`s are in a dedicated cell.

---

_Comment by @whatnick on 2024-10-09 04:45_

This would be a really useful feature for cleaning up ad-hoc developed notebooks into something more maintainable.

---
