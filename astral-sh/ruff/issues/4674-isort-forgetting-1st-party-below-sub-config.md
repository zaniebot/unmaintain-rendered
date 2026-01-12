```yaml
number: 4674
title: "`isort` forgetting 1st party below sub-config"
type: issue
state: closed
author: jamesbraza
labels: []
assignees: []
created_at: 2023-05-26T22:54:34Z
updated_at: 2023-05-29T02:50:37Z
url: https://github.com/astral-sh/ruff/issues/4674
synced_at: 2026-01-12T15:54:44Z
```

# `isort` forgetting 1st party below sub-config

---

_@jamesbraza_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

In today's version of monorepo hurdles, here is a minimal repro repo with `ruff=0.0.267` and `numpy==1.24.3` installed:

```none
.
├── folder_1
│   └── module.py
├── folder_2
│   ├── main.py
│   ├── module.py
│   └── ruff.toml
└── pyproject.toml
```

```toml
# pyroject.toml

[tool.ruff]

select = ["ALL"]
ignore = [
    "D2",
    "INP001",
]
```

```toml
# ruff.toml
```

```python
"""folder_1/module.py."""

SOME_OTHER_MODULE_IMPORT = 0
```

```python
"""folder_2/module.py."""

SOME_SAME_MODULE_IMPORT = 0
```

```python
"""folder_2/main.py."""

import time  # noqa: F401

import numpy as np  # noqa: F401

from folder_1.module import SOME_OTHER_MODULE_IMPORT  # noqa: F401
from folder_2.module import SOME_SAME_MODULE_IMPORT  # noqa: F401
```

All is well, `ruff` reports no issues from the repo root:

```none
repo > ruff .
```

### Issue

Now, changing `ruff.toml` to be:

```toml
# ruff.toml

extend = "../pyproject.toml"
```

Suddenly we get:

```none
repo > ruff .
folder_2/main.py:3:1: I001 [*] Import block is un-sorted or un-formatted
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

So, what's the issue about?
- With sub-config `ruff.toml` without`extend`: `ruff` from root correctly infers `folder_1` as 1st party
- With subconfig `ruff.toml` using `extend`: `ruff` from root now infers `folder_1` as 3rd party

I think sub-config extensions should inherit 1st party understandings from parent config.  What do you think?

---

_Renamed from "`isort`'s forgetting 1st party when extending in sub-configs" to "`isort` forgetting 1st party when extending in sub-configs" by @jamesbraza on 2023-05-26 22:55_

---

_Comment by @charliermarsh on 2023-05-26 23:45_

For the sake of completeness in my explanation and evaluation, should your modules here contain __init__.py files? Or do they intentionally not?

---

_Comment by @jamesbraza on 2023-05-26 23:52_

Yeah one can add them, just for a minimal repro it wasn't necessary.

Having an `__init__.py` in `folder_1` will not change how `ruff`'s `isort` sees `folder_1` as 1st --> 3rd party.

---

_Comment by @jamesbraza on 2023-05-27 00:06_

The issue here comes from `ruff`'s automatic inference of 1st vs 3rd party changing with sub-configs.

If you hardcode `known-first-party` in the top-level config and use `extend` in the sub-config (thus defeating `ruff`'s inference), this issue goes away.

---

_Comment by @charliermarsh on 2023-05-27 00:32_

If you add `select = ["I"]` to your empty `ruff.toml`, I believe you see the same behavior regardless of the `extend`. In the first example provided, the `I` rules aren't actually enabled for `folder_2`.

---

_Comment by @charliermarsh on 2023-05-27 00:33_

(There may actually be an issue here, I'm just trying to pinpoint a faithful reproduction so we can discuss it properly, sorry, not intending to nitpick.)

---

_Comment by @jamesbraza on 2023-05-27 00:44_

Yeah no offense taken!  And you're right about having `select` in the sub-config.

---

### Better Repro

```toml
# pyroject.toml

[tool.ruff]

select = ["I"]
```

Without the sub-config, `ruff` has no complaints.

Then, adding in the sub-config:

```toml
# ruff.toml

select = ["I"]
```

Now, `ruff` changes its opinions on 1st vs 3rd party on `folder_2/main.py`'s import from `folder_1`.

Turns out it wasn't the `extend`, but actually `I` being selected in the sub-config, that caused `ruff` to change its opinions.

---

_Renamed from "`isort` forgetting 1st party when extending in sub-configs" to "`isort` forgetting 1st party below sub-config" by @jamesbraza on 2023-05-27 02:11_

---

_Comment by @charliermarsh on 2023-05-27 02:59_

So, if you omit the `src` field (which is what we use to do first-party eligibility checks), then we use the "project root", which is _generally_ determined by finding the `pyproject.toml` or `ruff.toml` file. This applies regardless of whether you use `extend`.

You can fix it in one of two ways. First, by explicitly setting the `src` in the root directory:

```toml
# pyproject.toml

[tool.ruff]
# Explicitly set the source to the current directory.
# When other configurations `extend` this one, they'll
# use _this_ directory as `src`.
src = ["."]
```

```toml
# ruff.toml

extend = "../pyproject.toml"
```

Second, by explicitly setting the `src` in the subdirectory:

```toml
# ruff.toml

extend = "../pyproject.toml"
src = ["../"]
```

Either of these works as expected. (If you run with `--verbose`, Ruff will also tell you _why_ it's resolving modules as first- or third-party, which can help with debugging.)

I can see why the current behavior might be confusing as seen above, but alternative designs are confusing in other ways. E.g., you could be using `extend` with some shared configuration that doesn't even live in the project hierarchy. In that case, it would be weird to consider that folder the "project root".


---

_Comment by @jamesbraza on 2023-05-27 04:18_

Okay I follow what you're saying, thank you!

> E.g., you could be using extend with some shared configuration that doesn't even live in the project hierarchy. In that case, it would be weird to consider that folder the "project root".

This makes sense.  And I guess `isort` doesn't hit this case because it doesn't support sub-configs (as far as I know).

---

Here are a few possible resolutions:
- Documenting `src` field in `isort`'s settings page for [`known-first-party`](https://beta.ruff.rs/docs/settings/#known-first-party) and [`known-third-party`](https://beta.ruff.rs/docs/settings/#known-third-party)
    - Bonus points: documenting discerning logic for 1st party vs 3rd party vs local
- Creating a new boolean `isort` settings option `inherit-src` (defaulted `false`) to pull `src` from parent config when `extend` is present
- Just close this out 🤷‍♂️ 

Regardless, at least I have a working solution, so thanks!

---

Per `--verbose`, I didn't know that, it's helpful.  One comment is, it only shows up if one includes `--no-cache`:

```none
repo > ruff --no-cache --verbose .
...
[2023-05-26][20:51:46][ruff::rules::isort::categorize][DEBUG] Categorized 'time' as Known(StandardLibrary) (KnownStandardLibrary)
[2023-05-26][20:51:46][ruff::rules::isort::categorize][DEBUG] Categorized 'numpy' as Known(ThirdParty) (NoMatch)
[2023-05-26][20:51:46][ruff::rules::isort::categorize][DEBUG] Categorized 'folder_2.module' as Known(FirstParty) (SamePackage)
[2023-05-26][20:51:46][ruff::rules::isort::categorize][DEBUG] Categorized 'folder_1.module' as Known(ThirdParty) (NoMatch)
...
```


---

_Comment by @charliermarsh on 2023-05-27 19:19_

Totally, I'll try to clarify this a bit in the docs.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-27 19:19_

---

_Comment by @jamesbraza on 2023-05-27 19:50_

Feel free to tag me as a reviewer!

---

_Closed by @charliermarsh on 2023-05-29 02:50_

---
