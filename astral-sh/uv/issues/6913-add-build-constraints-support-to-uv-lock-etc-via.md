---
number: 6913
title: "Add `build-constraints` support to `uv lock` etc. via `pyproject.toml`"
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
  - good first issue
  - help wanted
assignees: []
created_at: 2024-09-01T16:57:10Z
updated_at: 2025-06-05T04:16:41Z
url: https://github.com/astral-sh/uv/issues/6913
synced_at: 2026-01-10T01:24:07Z
---

# Add `build-constraints` support to `uv lock` etc. via `pyproject.toml`

---

_Issue opened by @charliermarsh on 2024-09-01 16:57_

Can follow the way we manage `constraint-dependencies.`

See:  https://github.com/astral-sh/uv/issues/5561#issuecomment-2323330613.

---

_Label `enhancement` added by @charliermarsh on 2024-09-01 16:57_

---

_Label `good first issue` added by @charliermarsh on 2024-09-01 16:57_

---

_Label `help wanted` added by @charliermarsh on 2024-09-01 16:57_

---

_Referenced in [astral-sh/uv#5561](../../astral-sh/uv/issues/5561.md) on 2024-09-01 16:57_

---

_Comment by @menkotoglou on 2024-09-02 10:44_

I could have a look to this @charliermarsh. 

---

_Comment by @charliermarsh on 2024-09-02 18:01_

Go for it!

---

_Comment by @charliermarsh on 2024-09-08 18:32_

@menkotoglou -- Just let me know if you're not able or don't plan to get to it (or need some pointers!), so we can prioritize accordingly.

---

_Comment by @menkotoglou on 2024-09-08 20:02_

Hey @charliermarsh, sorry for not taking it on this week, some stuff came up and didn't have time to take it on. Was actually planning to start working on it this week, and of course revert back to you if any help needed.

Wdyt?

---

_Comment by @inoa-jboliveira on 2024-09-09 19:42_

I am trying to understand if this is the feature I need. My problem:

1. I have my dependencies and dev dependencies in `pyproject.toml`
2. I have a constraint file `constraints.txt`
3. I build `pip compile pyproject.toml -c constraints.txt -o requirements.txt`
4. I build `pip compile pyproject.toml --extra dev -c contraints.txt -c requirements.txt -o requirements-dev.txt`

This is the most sane way I could find so both requirements.txt and requirements-dev.txt contain the same libs except for the dev ones and they respect the constraints file which are sub dependencies I need to limit but do not depend myself.

I want to move to `uv lock` with a `uv export` fallback for generating the same files as before.

From the [docs](https://docs.astral.sh/uv/concepts/resolution/#dependency-constraints):


> uv supports constraints files (--constraint constraints.txt), like pip, which narrow the set of acceptable versions for the given packages. 

It seems `uv lock` does not have the constraint mechanism described above.

I would like to perform 2 things:

1. Create a first `uv.lock` constrained by current `requirements-dev.txt` which will force all current versions on it
2. Have a workflow to run
    ```
    uv lock --constraint constrains.txt  # Preferably this being configured in pyproject.toml so it is guaranteed to be true
    uv export > requirements.txt
    uv export --extra dev > requirements-dev.txt
    ```
Eventually I will remove the exports, but for validating the process, I will need to make sure the output versions are identical to the original versions
 
Is this the correct issue?

---

_Comment by @inoa-jboliveira on 2024-09-09 19:55_

Ok, I found a workaround. If I set 

```
[project.optional-dependencies]
constraints = [
    "foo>=2.0"
]
```

UV will enforce the optional dependencies when resolving the lock.

~It still does not solve my whole migration problem, but I think I can hack something together~

Edit: 
It does work. I temporarily added my whole requirementes-dev.txt to another "optional-dependencies" called "reqs", ran uv lock, removed the block, ran uv lock again, and the exports now match perfectly the original pip compiled requirements files.

I leave this here in case anyone else wants to migrate from pip compile to uv lock

---

_Referenced in [astral-sh/uv#11585](../../astral-sh/uv/pulls/11585.md) on 2025-02-17 23:07_

---

_Closed by @charliermarsh on 2025-02-18 01:58_

---

_Comment by @dimaqq on 2025-06-05 04:16_

🦄 MAGIC 🦄 

I just came here to complain about smth like pip's constraints.txt and guess what? The feature is already there, just not very discoverable.

---
