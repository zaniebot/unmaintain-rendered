```yaml
number: 1353
title: "`uv pip compile` adds setuptools (and pip) to requirements.txt"
type: issue
state: closed
author: ThiefMaster
labels:
  - wontfix
  - compatibility
assignees: []
created_at: 2024-02-15T21:22:00Z
updated_at: 2024-03-04T01:50:24Z
url: https://github.com/astral-sh/uv/issues/1353
synced_at: 2026-01-12T15:58:26Z
```

# `uv pip compile` adds setuptools (and pip) to requirements.txt

---

_@ThiefMaster_

`pip-compile` considers dependencies on `setuptools` (and `pip`) unsafe and mentions this at the end of the generated file:

```py
# The following packages are considered to be unsafe in a requirements file:
# pip
# setuptools
```

or

```py
# The following packages are considered to be unsafe in a requirements file:
# setuptools
```

When using `uv pip compile`, this is not the case and I get `pip` added to the requirements.txt (the `pip` dep is coming from `pip-tools` and `setuptools` from packages like `babel`, `pycountry` and `pip-tools`). But both are packages that you generally never want in a requirements.txt (except maybe in VERY specific circumstances - and in those case you probably want to vendor the versions you require instead of having them as package dependencies).

---

_Renamed from "`uv pip compile` adds setuptools to requirements.txt" to "`uv pip compile` adds setuptools (and pip) to requirements.txt" by @ThiefMaster on 2024-02-15 21:22_

---

_Label `compatibility` added by @charliermarsh on 2024-02-15 21:26_

---

_Comment by @juftin on 2024-02-21 19:27_

FWIW, `--allow-unsafe` is becoming the default in the next major release of `pip-compile`: https://pip-tools.readthedocs.io/en/stable/#deprecations
> In the next major release, the --allow-unsafe behavior will be enabled by default (https://github.com/jazzband/pip-tools/issues/989). Use --no-allow-unsafe to keep the old behavior. It is recommended to pass --allow-unsafe now to adapt to the upcoming change.

---

_Comment by @zanieb on 2024-02-21 20:14_

Thanks for the additional context. If `pip-compile` is moving towards just including these by default perhaps we should continue to do so? It's _more_ safe for us since we don't rely on these packages.

---

_Comment by @charliermarsh on 2024-02-21 20:18_

(We do already "include" these by default, unless I'm misunderstanding your comment.)

---

_Comment by @zanieb on 2024-02-21 20:41_

(edited to clarify)

---

_Comment by @hauntsaninja on 2024-02-21 21:34_

In my opinion, `uv` has the right default behaviour, and soon pip-compile will have the right default behaviour too. If I were uv, I would implement https://github.com/astral-sh/uv/issues/1415 to let people do what they need to, and not change this. (Especially since if you are using uv, I think you have less of a need to mark pip and setuptools as unsafe)

---

_Comment by @charliermarsh on 2024-02-21 21:36_

Yeah, agreed.

---

_Label `wontfix` added by @zanieb on 2024-02-21 21:41_

---

_Comment by @charliermarsh on 2024-03-04 01:50_

Closing as "working as intended".

---

_Closed by @charliermarsh on 2024-03-04 01:50_

---
