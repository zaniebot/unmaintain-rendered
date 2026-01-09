---
number: 10695
title: "`uvx` and `uv run` not equivalent? uvx fails resolving tool dependencies"
type: issue
state: closed
author: maphew
labels:
  - question
assignees: []
created_at: 2025-01-16T21:59:08Z
updated_at: 2025-01-20T17:23:59Z
url: https://github.com/astral-sh/uv/issues/10695
synced_at: 2026-01-07T13:12:18-06:00
---

# `uvx` and `uv run` not equivalent? uvx fails resolving tool dependencies

---

_Issue opened by @maphew on 2025-01-16 21:59_

On windows in CMD prompt or Powershell with "uv 0.5.20 (1c17662b3 2025-01-15)" I get the below:

foobar.py:
```
# /// script
# requires-python = ">=3.12"
# dependencies = [
#     "click",
#     "rich",
# ]
# ///
print('Hello')
```

Fails:
```
D:\x
λ uvx foobar.py
  x No solution found when resolving tool dependencies:
  `-> Because foobar-py was not found in the package registry and you require foobar-py, we can conclude that your requirements are unsatisfiable.
```

Works:
```
D:\x
λ uv run foobar.py
Hello
```


---

_Comment by @zanieb on 2025-01-16 22:36_

Please remember to consult the documentation before opening issues, as described in #9452.

- https://docs.astral.sh/uv/guides/tools/#running-tools
- https://docs.astral.sh/uv/concepts/tools/#relationship-to-uv-run
- https://docs.astral.sh/uv/reference/cli/#uv-tool-run
- https://docs.astral.sh/uv/reference/cli/#uv-run

---

_Label `question` added by @zanieb on 2025-01-16 22:36_

---

_Comment by @maphew on 2025-01-20 15:28_

Thanks zanieb. I was keying off another Astral page that said the `uvx` was an identical alias to `uv run`. I'll re-open if/when I find the page with the wrong info.

---

_Closed by @maphew on 2025-01-20 15:28_

---

_Comment by @maphew on 2025-01-20 15:49_

...it must not have been an Astral page. Somehow I completely missed the concept of _tool_, and it's quite clear that's a fundamental idea that's been around for awhile. Apologies for the noise!

A small mark in my defense, the error message doesn't give much to go on. For example the OP script  does not _"require foo-py"_ and I do not see anything in the pages referenced above that talks about that (admittedly, I skimmed).

---

_Referenced in [astral-sh/uv#10784](../../astral-sh/uv/issues/10784.md) on 2025-01-20 17:23_

---

_Comment by @zanieb on 2025-01-20 17:23_

We can add a hint to the error here https://github.com/astral-sh/uv/issues/10784

---
