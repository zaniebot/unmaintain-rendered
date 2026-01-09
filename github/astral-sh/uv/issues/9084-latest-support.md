---
number: 9084
title: "@latest support?"
type: issue
state: open
author: drcongo
labels:
  - question
assignees: []
created_at: 2024-11-13T13:32:07Z
updated_at: 2025-05-03T18:20:38Z
url: https://github.com/astral-sh/uv/issues/9084
synced_at: 2026-01-07T13:12:18-06:00
---

# @latest support?

---

_Issue opened by @drcongo on 2024-11-13 13:32_

I'm coming from a poetry background here and one thing I regularly need to do is upgrade multiple packages to the latest compatible version, while leaving others untouched. With poetry, this is `poetry show -o` to list packages where updates are available, and then I can `poetry add {package}@latest` to upgrade the ones I want to upgrade - after each upgrade, I then run the test suite to make sure nothing has broken (so I definitely don't want to upgrade everything all at once).

I've been trying to do this on a project I've converted to `uv` but there doesn't seem to be a simple way to do this. These are what I've found in the docs...

* `uv add 'httpx>0.1.0' --upgrade-package httpx` is incredibly verbose and requires me to look up what bounds I want to give it - ie: `uv add httpx --upgrade-package httpx` does nothing. Doing this for a lot of packages takes _considerably_ longer than the poetry way.
* `uv lock --upgrade-package requests` only works with loose constraints in the pyproject.toml so I'm having to go in and hand edit the pyproject.toml in order to test the project with the latest version

Have I missed something in the docs? Is support for `uv add {package}@latest` or similar on the roadmap at all?

---

_Comment by @charliermarsh on 2024-11-13 16:01_

Can you explain why `uv lock --upgrade-package requests` isn't working here? Do you have an upper-bound on your package or something?

---

_Label `question` added by @charliermarsh on 2024-11-13 16:01_

---

_Comment by @drcongo on 2024-11-19 11:05_

Hey @charliermarsh - yes, exactly that. The pyproject.toml had, if I remember correctly,`~=` some version. I'm bound to be doing the above flows while switching another project over to `uv` soon so I'll post a more concrete example soon.

PS. Thank you so much for `ruff` and `uv`, they're the most exciting things to happen to Python in years.

---

_Comment by @MicaelJarniac on 2025-05-03 17:56_

Possibly related to https://github.com/astral-sh/uv/issues/6794?

---

_Comment by @zanieb on 2025-05-03 18:20_

@konstin and I have been talking about `uv add` changing bounds, related to #6794 and #12946 

---
