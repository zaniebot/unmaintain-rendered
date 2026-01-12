```yaml
number: 1626
title: Add rst backticks rule to pygrep-hooks
type: pull_request
state: closed
author: martinabeleda
labels: []
assignees: []
base: main
head: main
created_at: 2023-01-04T05:16:44Z
updated_at: 2023-01-04T20:45:30Z
url: https://github.com/astral-sh/ruff/pull/1626
synced_at: 2026-01-12T05:36:32Z
```

# Add rst backticks rule to pygrep-hooks

---

_Pull request opened by @martinabeleda on 2023-01-04 05:16_

Implements RST Backticks rule from #980 

---

_Comment by @charliermarsh on 2023-01-04 12:31_

Thanks for this! Is this rule intended to be run on Python code? (Have you used it in practice?) It looks like it's maybe intended to be run on RST directly?

---

_Comment by @charliermarsh on 2023-01-04 12:43_

E.g., I'm not sure that the positive-errors in `PGH005_1.py` are actually picked up by pre-commit, since the pre-commit regex seems to ignore any lines that start with four spaces.

---

_Comment by @martinabeleda on 2023-01-04 19:45_

> Thanks for this! Is this rule intended to be run on Python code? (Have you used it in practice?) It looks like it's maybe intended to be run on RST directly?

Thanks for looking @charliermarsh yeah I was confused about this but raised the PR to get another set of eyes on it. I think you are right that it's not intended to work on indented comments (it looks to be used for linting PEP rst files). 

In that case, does it even make sense to add this rule? I was looking for a good first issue but can close this and find another one to work on

---

_Comment by @charliermarsh on 2023-01-04 20:45_

@martinabeleda - Thanks tons for getting involved! Yeah, I think this rule probably _doesn't_ make sense to include. I'm sorry to mislead. I'm gonna go ahead and edit the pygrep issue to remove any RST-specific rules. I wish I'd caught those before!

My top suggestion right now is to take something from the [`flake8-simplify`](https://github.com/charliermarsh/ruff/issues/998) task. The nice thing is that you can look at the [`flake8-simplify` source](https://github.com/MartinThoma/flake8-simplify/blob/d3cb9556a5255969f83a8329df8f635da41f595d/flake8_simplify/rules/ast_for.py) for any given rule as a hint on how to implement it. Just comment in the issue if you'd like to work on something there, I'd love your help!

---

_Closed by @charliermarsh on 2023-01-04 20:45_

---
