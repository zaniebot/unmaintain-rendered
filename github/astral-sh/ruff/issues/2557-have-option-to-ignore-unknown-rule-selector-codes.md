---
number: 2557
title: Have option to ignore unknown rule selector codes
type: issue
state: closed
author: mhvk
labels:
  - question
assignees: []
created_at: 2023-02-03T21:57:30Z
updated_at: 2024-12-25T14:42:08Z
url: https://github.com/astral-sh/ruff/issues/2557
synced_at: 2026-01-07T13:12:14-06:00
---

# Have option to ignore unknown rule selector codes

---

_Issue opened by @mhvk on 2023-02-03 21:57_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

In `astropy`, we're very happily converting our style checking to `ruff` but the pace of `ruff` development causes some issues for me at least: I've set up my (emacs with flycheck) to auto-run `ruff`, but now any time the version of `ruff` installed on the system does not exactly match the one on which the exclude/include selection codes in `pyproject.toml` are based, such that a newly implemented (or removed) selector is not known, one just gets a failure and `ruff` does not run. (Specifically, in `astropy` we had listed `PLC2201` as something not to run, but this has been deprecated and now any time I visit a branch that does not yet have that one changed to `SIM300`, I get errors.) 

Such problems could be alleviated if there were an option for ignoring unknown selector codes and just run with the ones that are recognized. Would it be possible to implement this? I don't think it would even need a new code; it seems logical under "-q" (well, logical to the extent that I thought it was when I read "Print lint violations, but nothing else").

---

_Comment by @charliermarsh on 2023-02-03 22:10_

Let me think on the broader request, but first: I'm sorry about the `PLC2201` thing. In hindsight, I should've changed that to a redirect (by pointing it to `SIM300`) rather than removing it entirely (which had the potential to cause breakages, as you experienced). It's not something that I anticipate happening often, and I'll try to be more careful about it going forward.

---

_Label `question` added by @charliermarsh on 2023-02-03 22:10_

---

_Comment by @aberres on 2023-03-07 09:12_

I would welcome this as well. This way, one could add additional selects/ignores while not updating the version used in CI yet.

---

_Comment by @charliermarsh on 2023-05-18 16:47_

I think my preference is to err on the side of keeping configuration strict, and to try and avoid backwards-incompatible changes as much as possible. I'm also hopeful that slowing the pace of new rule rollouts will make this less challenging for users.

---

_Closed by @charliermarsh on 2023-05-18 16:47_

---
