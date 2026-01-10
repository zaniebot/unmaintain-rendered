```yaml
number: 8669
title: Notebook related rule updates
type: issue
state: closed
author: dhruvmanila
labels:
  - rule
assignees: []
created_at: 2023-11-14T04:36:41Z
updated_at: 2024-04-26T04:08:01Z
url: https://github.com/astral-sh/ruff/issues/8669
synced_at: 2026-01-10T11:09:50Z
```

# Notebook related rule updates

---

_Issue opened by @dhruvmanila on 2023-11-14 04:36_

This is an open discussion around linting Jupyter Notebooks using certain rules that doesn't map well with the notebook model.

For some context, Notebooks are made up of cells, each cell can be assumed to act as a single Python file but that's not exactly true as anything defined in a cell is available throughout the notebook via the global scope.

There are certain lint rules which doesn't map well with notebooks and this issue is to keep track of them in a central place. We can discuss on possible solutions here.

- [x] [`E402: module-import-not-at-top-of-file`](https://docs.astral.sh/ruff/rules/module-import-not-at-top-of-file/) - Usually imports definition in a notebook is scattered across multiple cells. The current workaround is to ignore this rule but one could argue that this rule should work at _cell_ level i.e., "Module level import not at top of cell".
- [x] [`B018: useless-expression`](https://docs.astral.sh/ruff/rules/useless-expression/), [`B015: useless-comparison`](https://docs.astral.sh/ruff/rules/useless-comparison/) - Expressions in a cell are not useless if they're at the end because the result of that expression is the output of the cell. So, these rules should be updated to avoid raising any violation if it's the last expression in the cell.
- [x] [`E703: useless-semicolon`](https://docs.astral.sh/ruff/rules/useless-semicolon/) - On the other hand, semicolons are used to avoid the output mentioned in the previous point. If there's a trailing semicolon for the last expression in a cell, it's probably best to avoid raising a violation for it. The formatter does a similar thing: https://github.com/astral-sh/ruff/pull/8590
- [x] [`D100: undocumented-public-module`](https://docs.astral.sh/ruff/rules/undocumented-public-module/) - This is irrelevant for Notebooks.

---

_Label `rule` added by @dhruvmanila on 2023-11-14 04:36_

---

_Comment by @alkatar21 on 2023-11-16 08:01_

I think another rule that makes less sense in notebooks is `D100`. Module docstrings are there most likely in markdown.
And I'm also not shure about `T201`, because you need this from time to time if something within the cell should also be output and not just the last line?

---

_Comment by @dhruvmanila on 2023-11-20 19:58_

Thanks for chiming in on the issue!

Can you help me understand when does `D100` would give a false positive in Notebook? Do you mean that the content of the module docstring will be as a separate markdown cell? If so, then I think yeah we should avoid triggering this rule. Also, I think `D100` in general isn't relevant for Notebooks as they can't be imported.

For `flake8-print` rules, I would actually recommend to disable them for Notebooks as some users might actually not want them to be present.

---

_Comment by @alkatar21 on 2023-11-20 21:55_

> Do you mean that the content of the module docstring will be as a separate markdown cell?

Yes, I would put everything I write in a module in the module docstring in a notebook in a markdown cell, which is why it makes no sense to have a module docstring.

>  If so, then I think yeah we should avoid triggering this rule. Also, I think D100 in general isn't relevant for Notebooks as they can't be imported.

That's what I basically wanted to say. That it makes no sense to me in notebooks that the rule triggers.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-11-21 18:37_

---

_Comment by @dhruvmanila on 2023-11-28 16:31_

I'm a bit torn on how to go for `E402`. There are a few solutions for this:

1. We recommend disabling them for notebooks
2. By default we disable them for notebooks
3. For notebooks, we'd check the rule for each cell so "Module level import not at top of cell"

I'm thinking of going with (3) in preview.

---

_Closed by @dhruvmanila on 2023-11-29 00:32_

---

_Comment by @redeboer on 2024-04-25 15:56_

Thanks for maintaining this great project!

> Another potential solution is to introduce `E403` rule that is specifically for notebooks that works at cell level while `E402` will be disabled for notebooks.
> https://github.com/astral-sh/ruff/pull/8872#issue-2015007451

In some of our projects, we work with notebooks where we prefer to put all imports in one cell at the top of the notebook. If I understand correctly, there is no rule anymore that can enforce this. Might it be an idea to define a new rule `E403` specifically for notebooks, like mentioned by @dhruvmanila, but that works on the whole notebook?

If this idea is worth pursuing, I can post it as a separate issue.

---

_Comment by @dhruvmanila on 2024-04-26 04:08_

@redeboer Yeah, I think we can discuss this in a separate issue.

---
