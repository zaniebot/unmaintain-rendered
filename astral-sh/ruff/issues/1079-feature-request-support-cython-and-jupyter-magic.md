---
number: 1079
title: "Feature request: support Cython and Jupyter magic?"
type: issue
state: closed
author: 9r0x
labels:
  - core
assignees: []
created_at: 2022-12-05T21:30:17Z
updated_at: 2024-03-06T16:22:27Z
url: https://github.com/astral-sh/ruff/issues/1079
synced_at: 2026-01-10T01:22:38Z
---

# Feature request: support Cython and Jupyter magic?

---

_Issue opened by @9r0x on 2022-12-05 21:30_

Hi,

I was wondering if we could consider Cython support and/or Jupiter magic because

1. Cython code sometimes exists alongside Python code in a project, and a unified linter would be very useful?
2. Many code and lint inside Jupyter notebook, and % in the code cell could break the lining, would be great if it can be ignored/supported.

Thanks

---

_Label `enhancement` added by @charliermarsh on 2022-12-06 02:28_

---

_Comment by @charliermarsh on 2022-12-06 02:28_

Cython I don't have much experience with so can't commit to anything on that front right now.

Jupyter integration though is very interesting to me and something I'd like to support!

---

_Comment by @MarcoGorelli on 2022-12-06 09:19_

Hey,

I don't know how to use rust, but I've done some relevant work here on the Python side

For Cython, you might be interested in https://github.com/MarcoGorelli/cython-lint (which does everything flake8 used to, plus more) and seeing if that's portable to rust?

For Jupyter, I contributed its support to `black`, there's [this file](https://github.com/psf/black/blob/d4a85643a465f5fae2113d07d22d021d4af4795a/src/black/handle_ipynb_magics.py) that deals with masking/unmasking magic symbols (`%`, `%%`, `!`, `?`) - if possible, I'd strongly suggest something like that over regular expressions

---

_Comment by @charliermarsh on 2022-12-06 14:37_

Amazing, thank you @MarcoGorelli -- that's super helpful. I'll probably just port that file to Rust then.

---

_Comment by @charliermarsh on 2022-12-28 02:19_

Ruff was actually integrated into [nbQA](https://github.com/nbQA-dev/nbQA/pull/773) in the latest release, so you can now run (e.g.):

```
❯ nbqa ruff Untitled.ipynb
Untitled.ipynb:cell_1:2:5: F841 Local variable `x` is assigned to but never used
Untitled.ipynb:cell_2:1:1: E402 Module level import not at top of file
Untitled.ipynb:cell_2:1:8: F401 `os` imported but unused
Found 3 error(s).
1 potentially fixable with the --fix option.
```

Super cool :)

(Would still like to have a first-party integration at some point.)

---

_Referenced in [astral-sh/ruff#1417](../../astral-sh/ruff/pulls/1417.md) on 2022-12-28 02:23_

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:16_

---

_Label `core` added by @charliermarsh on 2022-12-31 18:16_

---

_Comment by @dhruvmanila on 2023-06-27 12:11_

(2) is being worked on (see #5030)

---

_Comment by @vyasr on 2023-07-14 18:47_

@charliermarsh On the Cython side it's worth noting that [isort supports Cython as of isort 5.0.0](https://pycqa.github.io/isort/docs/major_releases/introducing_isort_5.html#cython-support) so you may also be able to port those features over to rust (in the same way that you're hoping to port @MarcoGorelli's excellent cython-lint project). Between cython-lint and isort support I would probably consider that enough to claim Cython support, especially since flake8's support has always been patchy and incidental (prior to the change last year that disabled it entirely, necessitating [flake8-force](https://github.com/kmaehashi/flake8-force)) and I believe the same is true for pylint. Just in case you were curious where to set the goalposts for this issue :smiley: 

This might also be a strong argument for #283, though, and in particular supporting Python plugins alongside Rust plugins. Cython is a prime example of a case where the ruff developers may not have the expertise or interest in maintaining their own solution, but could happily delegate to a plugin author who doesn't have Rust experience but is willing to write one in Python.

Great work on ruff, by the way! Definitely a fan of ruff.

---

_Referenced in [commaai/openpilot#29335](../../commaai/openpilot/issues/29335.md) on 2023-08-11 20:15_

---

_Referenced in [astral-sh/ruff#8094](../../astral-sh/ruff/issues/8094.md) on 2023-10-20 15:31_

---

_Referenced in [rapidsai/cudf#14882](../../rapidsai/cudf/issues/14882.md) on 2024-01-25 19:49_

---

_Comment by @MichaReiser on 2024-02-23 21:58_

@dhruvmanila do we support this now that we support ipython magics?

---

_Comment by @dhruvmanila on 2024-03-05 11:43_

> @dhruvmanila do we support this now that we support ipython magics?

@MichaReiser Yes, we support (2) but not (1). I'm not familiar with Cython so can't say what will be required to support it.

---

_Comment by @MichaReiser on 2024-03-05 11:50_

@9r0x could you help us understand what features you would expect from a linter supporting cpython? Is it Cython specific lint rules or is it something else?

---

_Comment by @charliermarsh on 2024-03-05 13:50_

Cython is a superset of Python. So at minimum it would require a different parser. Lets close this and create a new issue to track Cython, since there are really two independent requests here.

---

_Referenced in [astral-sh/ruff#10250](../../astral-sh/ruff/issues/10250.md) on 2024-03-06 16:22_

---

_Comment by @MichaReiser on 2024-03-06 16:22_

Done, https://github.com/astral-sh/ruff/issues/10250

---

_Closed by @MichaReiser on 2024-03-06 16:22_

---

_Referenced in [rapidsai/build-planning#130](../../rapidsai/build-planning/issues/130.md) on 2024-12-30 20:45_

---
