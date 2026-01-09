---
number: 22146
title: Rules documentation flake8 pyflakes
type: issue
state: open
author: fulminemizzega
labels:
  - question
assignees: []
created_at: 2025-12-22T18:21:37Z
updated_at: 2025-12-22T23:41:06Z
url: https://github.com/astral-sh/ruff/issues/22146
synced_at: 2026-01-07T13:12:16-06:00
---

# Rules documentation flake8 pyflakes

---

_Issue opened by @fulminemizzega on 2025-12-22 18:21_

Hello, I believe there is a typo in the tutorial documentation and rules (or I do not understand them correctly).
In some places the `F` rule set is said to be a part of Flake8:
https://github.com/astral-sh/ruff/blob/ec034fc359208e725bba7ba14f9d3dd1c3273c02/docs/tutorial.md?plain=1#L204
https://github.com/astral-sh/ruff/blob/ec034fc359208e725bba7ba14f9d3dd1c3273c02/README.md?plain=1#L316
https://github.com/astral-sh/ruff/blob/ec034fc359208e725bba7ba14f9d3dd1c3273c02/README.md?plain=1#L316

in other places is specified to be from Pyflakes
https://github.com/astral-sh/ruff/blob/4a937543b932a18e419c87b1f2717107b4dd2164/docs/linter.md?plain=1#L31
or also here https://docs.astral.sh/ruff/rules/#pyflakes-f

I'm not familiar with them (which is why I'm reading the documentation), but to me it looks like `F` is from Pyflakes.

---

_Comment by @charliermarsh on 2025-12-22 18:35_

Both are, in some sense, correct. The underlying rules come from Pyflakes. Flake8 wraps Pyflakes, pycodestyle, and other tools to generate diagnostics. So the rule categorization does come from Flake8, but the rule logic itself is in Pyflakes.


---

_Label `question` added by @dylwil3 on 2025-12-22 20:50_

---

_Comment by @fulminemizzega on 2025-12-22 23:41_

When I started reading the tutorial and got to the line where it says that Flake8's rule F is enabled by default, then I wanted to see what this rule set is, so I went to rules and started looking for Flake8 (F). I found (FIX), (FA) but no (F) and this threw me off. Read this as a feedback from a new user, I understand that to someone used to the various python tools this is minor nitpicking.

---
