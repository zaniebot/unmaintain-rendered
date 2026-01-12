```yaml
number: 1232
title: Apply CLI options even when no pyproject.toml is found
type: pull_request
state: merged
author: cdbrendel
labels: []
assignees: []
merged: true
base: main
head: overrides-no-toml
created_at: 2022-12-14T02:48:59Z
updated_at: 2022-12-14T15:13:33Z
url: https://github.com/astral-sh/ruff/pull/1232
synced_at: 2026-01-12T15:55:06Z
```

# Apply CLI options even when no pyproject.toml is found

---

_@cdbrendel_

Thanks for all your fantastic work on this remarkable tool! 

I've been having an issue that when there is no `pyproject.toml` found, command line options are ignored in lieu of the hard-coded defaults. For example, `foo.py` produces two F401 errors by default (good!), but when I try to specify `ruff` options via the CLI (`--select`, `--ignore`, etc.) they're not respected (bad!).

```python
# foo.py
import os
import sys
```

```console
$ ruff --ignore=F401 foo.py
Found 2 error(s).
foo.py:1:8: F401 `os` imported but unused
foo.py:2:8: F401 `sys` imported but unused
2 potentially fixable with the --fix option.
```

I can make these command line options work if I put a `pyproject.toml` anywhere on the path, regardless of its content. I imagine the intended behavior is that options on the CLI should override defaults regardless of whether or not a `pyproject.toml` is found. 

From what I can tell, when a `pyproject.toml` is found, `ruff::resolve()` calls `Resolver::resolve_settings()`, which ultimately calls `Configuration::apply()`, which is responsible for joining the command line options: 

https://github.com/charliermarsh/ruff/blob/765d21c7b06e780fb91cbcc79909836883faf0dd/src/resolver.rs#L131-L133

However, `Configuration::apply()` is not called unless a `pyproject.toml` is found. It looks like 9853b0728ba9289753cb3be984de75677a4713c6 removed the following lines which had been responsible for merging command line options with the default configuration:

https://github.com/charliermarsh/ruff/blob/4bb6b4851a639d1855c7fb5d02ea53a074c9a178/src/main.rs#L93-L98

This PR adds back this functionality, although my solution is a bit inelegant given the recent removal of this sort of logic from `main.rs`. Maybe a better fix would be to break the call to `Configuration::apply()` above out into a separate method that is called in all cases? Happy to work more on it if so.

Either way, figured I'd offer the PR instead of just opening an issue. :) (Sorry for any silly issues; I'm a baby rustacean.)

---

_Marked ready for review by @cdbrendel on 2022-12-14 02:50_

---

_Comment by @charliermarsh on 2022-12-14 03:52_

D’oh, this was just a total oversight from a large refactor that went out recently, to change how settings are resolved. You’re totally right.

---

_Merged by @charliermarsh on 2022-12-14 03:55_

---

_Closed by @charliermarsh on 2022-12-14 03:55_

---

_Comment by @charliermarsh on 2022-12-14 03:55_

Thank you!

---

_Comment by @charliermarsh on 2022-12-14 03:55_

(I really appreciate the clarity and thoroughness of your PR summary too.)

---

_Comment by @cdbrendel on 2022-12-14 15:13_

Thank you! Very kind of you to say. :) 

---

_Branch deleted on 2022-12-14 15:13_

---
